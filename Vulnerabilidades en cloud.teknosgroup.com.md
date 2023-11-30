<h1 style="color:purple">Confidencial</h1>

### 27-11-2023
## Escaneo de seguridad de Nextcloud
![[Pasted image 20231127151351.png]]
Se utilizó la [herramienta provista por Nextcloud](https://scan.nextcloud.com/) para evaluar la versión del servidor. 

El scan indica "Su versión ha llegado al final de su vida útil y es muy probable que se vea afectada por muchas vulnerabilidades. Lamentablemente, los avisos de seguridad no están disponibles para una versión tan desactualizada, por lo que no podemos crear una lista automatizada de vulnerabilidades. Deberías actualizar lo antes posible."

## Versión de Nextcloud

- Las versiones de Nextcloud que actualmente cuentan con soporte y no tienen vulnerabilidades conocidas son las últimas versiones de **Nextcloud 25 26 y 27**. 
- Cloud.teknosgroup.com corre en  **Nextcloud 14.0.1.1**, cuyo release fue a mediados de 2019.

La página oficial de Nextcloud no provee un advisory de seguridad para esta versión. Por esto, se revisaron manualmente las vulnerabilidades publicadas en [CVEdetails](https://www.cvedetails.com/vendor/15913/Nextcloud.html).

**Se encontró que algunas de las vulnerabilidades presuntamente presentes en nuestro servidor son críticas**.
## Vulnerabilidades notorias

Las siguientes vulnerabilidades llaman especialmente la atención por la facilidad de explotación y el posible impacto, pero hay decenas de vulnerabilidades reportadas para la versión que corremos de nextcloud.
### [CVE-2021-32802](https://www.cvedetails.com/cve/CVE-2021-32802/ "CVE-2021-32802 security vulnerability details")
La vulnerabilidad permite Server Side Request Forgery (SSRF), file disclosure, o potencial ejecución remota de código.
La vulnerabilidad y el método de explotación fueron revelados en un [reporte de Hackerone](https://hackerone.com/reports/1261413) Lo que facilita mucho la explotación.
Un ataque SSRF exitoso fuerza al servidor a realizar las requests que el atacante quiera. Estos ataques suelen permitir una gran enumeración de la red interna, exfiltración de datos y ejecución remota de código. Si bien requieren un atacante más técnico que otros ataques el impacto es de los más altos posibles.
### [CVE-2021-22915](https://www.cvedetails.com/cve/CVE-2021-22915/ "CVE-2021-22915 security vulnerability details") 
La vulnerabilidad permite fuerza bruta contra la autenticación en nextcloud cuando se utiliza ipv6 para realizar el ataque. La explotación también está descripta en un [reporte de Hackerone](https://hackerone.com/reports/1154003). Según este reporte, la protección contra fuerza bruta de Nextcloud es inutil cuando se usa ipv6 para cualquier versión anterior a Nextcloud 19.

Otras vulnerabilidades dicen permitir denegación de servicio, ataques hacia usuarios con el cliente desktop de nextcloud o acceso a recursos no permitidos.
# Tráfico inseguro por HTTP
La comunicación entre nuestro servidor Nextcloud y el portal de acceso web se realiza exclusivamente de manera no encriptada por HTTP. Esto significa que:
- Las contraseñas para acceder viajan a través de la red en texto plano, facilitando mucho un ataque para robar estas credenciales. En general aumenta mucho el riesgo de un ataque de phishing, sniffing de estas credenciales.
- Los archivos con información sensible que sean subidos o descargados pueden ser interceptados o robados en el camino.

Obtener estas credenciales o la información sensible podría llevar a una potente explotación de las vulnerabilidades ya mencionadas.

Especialmente el SSRF, que puede llevar a una enumeración a gran escala de la red interna de Teknos y a una infección del servidor de Nextcloud. 

# Mitigación

- Se debe proceder a actualizar el servidor de nextcloud a una versión más actual.
- Se debe configurar un generador de certificados TLS para poder realizar conexiones seguras entre el cloud y los usuarios.

