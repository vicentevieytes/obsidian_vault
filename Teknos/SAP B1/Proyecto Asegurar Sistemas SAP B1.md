# Proyecto Asegurar Sistemas SAP B1 con Terminal Servers RDP.

18 de Octubre de 2023                                                               

## Objetivo

El objetivo de este proyecto es documentar y ejercer una política de ciberseguridad robusta para todos los sistemas SAP que utilizan un Windows Terminal Server para la conexión remota por RDP de los usuarios. Este documento recopila los primeros avances y enuncia las primeras políticas de seguridad ya definidas.

## Introducción

SAP Business One es un sistema de gestión empresarial (ERP) diseñado especialmente para PYMEs. SAP B1 integra diversas funciones comerciales en una sola plataforma. Esto incluye la gestión de finanzas, ventas, inventario, compras y relaciones con los clientes, entre otros aspectos.

SAP B1 es altamente personalizable, lo que significa que las empresas pueden adaptar el sistema para satisfacer sus necesidades específicas y requisitos empresariales.

Los clientes SAP B1 que funcionan con terminal servers en nuestro hosting al día de la fecha son: Mark, GEC, Vecher, Gervasini, NRG, Geopatagonia, Apampa, Aranami, Cfertil, Bremen, Fortin, Nafosa, Crowe, Simiente, Quimeco, Futura y próximamente Cigra.

| Cliente | VLAN | Contacto cliente | Contacto Soporte |
| --- | --- | --- | --- |
| MARK | 185 | gonzalo@frigorificomark.com | rjerez@pipobiconsulting.com |
| GEC | 165 | vtonelli@gec-sa.net / pmariani@gec-sa.net | Seidor (revisar) |
| VECHER | 175 | adrian@vecher.com.ar | rjerez@pipobiconsulting.com |
| GERVASINI | 182 | - | benjamin.moll@pragmaticaconsultores.com |
| NRG | 113 | damaris.albornoz@nrgproppants.com | benjamin.moll@pragmaticaconsultores.com |
| GEOPATAGONIA | 133 | - | benjamin.moll@pragmaticaconsultores.com |
| APAMPA | 170 | - | mgitter@bit.com.ar |
| ARANAMI | 132 | cecilia.abadia@aranamindustrial.com | mgitter@bit.com.ar |
| CFERTIL | 142 | valentinapace@campofertilsrl.com | mgitter@bit.com.ar |
| BREMEN | 172 | spalomeque@e-bremen.com | jcasares@maiten.com |
| BREMEN | 172 | spalomeque@e-bremen.com | jcasares@maiten.com |
| FORTIN | 193 | - | rrossano@teknosgroup.com |
| NAFOSA | 202 | oscar.prieto@nafosa.es | msavino@invenzis.com |
| CROWE | 105 | - | soportehosting@teknosgroup.com |
| SIMIENTE | 205 |  | benjamin.moll@pragmaticaconsultores.com |
| FUTURA | 209 | fatimastuven@futura.com.ar | benjamin.moll@pragmaticaconsultores.com |
| QUIMECO | 211 |  |  |
| CIGRA | 213 |  |  |

Los sistemas SAP B1 en Teknos están compuestos por:

- Un servidor de SAP B1 con la base de datos en el mismo servidor. (Casi siempre OpenSuse)
- Un servidor Windows que funciona como terminal server usando Remote Desktop Services
- Un servidor Windows opcional, para correr SAP Integration Framework.

Estos servidores comparten una VLan y forman parte de una red definida por un firewall virtual. 

![Untitled](Teknos/SAP%20B1/Proyecto%20Asegurar%20Sistemas%20SAP%20B1%20con%20Terminal%20Ser%2063c3ca458d0847509e10d6f4951439e5/Untitled.png)

Puede haber variaciones (SAP instalado en Windows, más de un TS o más de un IF, etc.). Por lo tanto, cada sistema tiene que analizarse por separado, ya que tendrán variaciones en el uso, los requerimientos y la arquitectura.

Además, hay distintos tipos de clientes:

- Clientes donde administramos todo el sistema.
- Clientes donde solo administramos el servidor SAP B1, pero no los usuarios del Terminal Server ni el software del cliente SAP B1.
- Clientes que administran todo ellos, solo proveemos hosting.

Las políticas de seguridad van a ser algo distintas para cada uno de estos, ya que el control de cada dominio y la responsabilidad de asegurar el sistema varían.

Teknos no está en control de los usuarios ni sus workstations. Por lo que la protección empieza en el Terminal server y en los firewalls.

## Políticas de seguridad para los Windows Terminal Server

Las siguientes son las primeras políticas que se consideraron específicamente para los WTS.

En cuanto estas políticas iniciales hayan sido efectivamente ejercidas y en cuanto se sigan identificando riesgos la política de seguridad se expandirá de manera acorde. 

### Restricción de capacidades de usuarios.

Los usuarios finales deben estar lo más restringidos posibles de manera tal que puedan continuar realizando su trabajo. AppLocker debe estar configurado en todos los servidores donde sea posible y como mínimo se deben aplicar las reglas predeterminadas para que los usuarios no puedan correr software que ellos mismos introducen o descargan, además se debe prohibir el acceso al símbolo de sistema, a powershell, y el editor de registro. [[SV020 B1 Configuración AppLocker]]

![Untitled](Teknos/SAP%20B1/Proyecto%20Asegurar%20Sistemas%20SAP%20B1%20con%20Terminal%20Ser%2063c3ca458d0847509e10d6f4951439e5/Untitled%201.png)

### ******************************Estado actual:******************************

Aplicado a modo de prueba en Aranami, NRG, Futura y Quimeco. Si no se reportan problemas se debe proceder con todos los demás donde sea posible.

Para sistemas que usen agrobit o otro software que puede que no estén instalados en las carpetas predeterminadas puede que haya que configurar reglas más complejas para la limitación de usuarios.

Una vez estén en funcionamiento estas reglas básicas se procederá a restringir caso por caso las aplicaciones que no sean necesarias.

---

### Manejo de versiones y parches de seguridad

Se debe tener inventario de la última aplicación de parches de seguridad del sistema operativo para cada uno de los servidores. En un principio con una tolerancia de un mes desde la última aplicación de los parches de seguridad. Además, todos los servidores deberían ser windows 2016 o superior.

### ******************************Estado actual:******************************

Ahora mismo se tienen:

- 3 servidores windows 2012
- 6 servidores windows 2016
- 8 servidores windows 2019

Mark, GEC, y Vecher hay que actualizar de windows 2012 a uno más nuevo, ya se notificó a los clientes. Están en proyecto de actualización aunque no tenemos fecha definida.

Hasta que no se avance con la actualización a un Windows más nuevo no se pueden aplicar parches de seguridad ni actualizaciones de mucho del software.

| SERVERs | SO | Última actualización del SO |
| --- | --- | --- |
| MARK-SV002 | Windows 2012 R2 | Falta reiniciar |
| GEC-SV002 | Windows 2012 R2 | 20/09/2023 |
| VECHER-SV002 | Windows 2012 R2 | 19/09/2023 |
| GERVASINI-SV020 | Windows 2016 | Falta reiniciar |
| NGR-SV020 | Windows 2016 | 19/09/2023 |
| GPATAG-SV020 | Windows 2016 | 19/09/2023 |
| APAMPA-SV020 | Windows 2016 | 19/09/2023 |
| ARANAMI-SV020 | Windows 2016 | 19/09/2023 |
| CFERTIL-SV020 | Windows 2016 | 19/09/2023 |
| BREMEN-SV020 | Windows 2019 | 15/10/2023 |
| BREMEN-SV030 | Windows 2019 | 15/10/2023 |
| FORTIN-SV020 | Windows 2019 | 19/09/2023 |
| NAFOSA-SV020 | Windows 2019 | 20/09/2023 |
| CROWE-SV020 | Windows 2019 | 19/09/2023 |
| SIMIEN-SV020 | Windows 2019 | 19/09/2023 |
| QUIMECO-SV020 | Windows 2019 | 19/09/2023 |
| FUTURA-SV020 | Windows 2019 | 19/09/2023 |

---

### Manejo de vulnerabilidades

La presencia de vulnerabilidades en las aplicaciones permitidas pueden dar lugar a ataques como el escalado de privilegios o la ejecución de código arbitraria. Es por esto que es importante mantener un inventario del software utilizado en cada terminal server.

Se debe mantener inventario de las vulnerabilidades detectadas en el software instalado y periodicamente se debe actualizar. Kaspersky ofrece informes de vulnerabilidades presentes en todos los endpoints donde está instalado, de ahí se pueden filtrar las de los servidores B1. De no ser posible aplicar una actualización esto debe quedar justificado.

### ****************************Estado actual:****************************

Inicialmente estas son las aplicaciones que necesitan ser actualizadas según kaspersky en cada uno de los servidores. 

| TERMINAL SERVER | SO | Apps vulnerables que necesitan ser actualizadas (segun kaspersky) |
| --- | --- | --- |
| MARK-SV002 | Windows 2012 R2 | Java JRE 1.8.x |
| GEC-SV002 | Windows 2012 R2 | Java JRE 1.8.x |
| VECHER-SV002 | Windows 2012 R2 |  |
| GERVASINI-SV020 | Windows 2016 |  |
| NGR-SV020 | Windows 2016 |  |
| GPATAG-SV020 | Windows 2016 | ASP.NET Web Frameworks, Oracle Java JRE 1.8.x |
| APAMPA-SV020 | Windows 2016 |  |
| ARANAMI-SV020 | Windows 2016 | WinRAR, Firefox,  Java JRE 1.8.x, |
| CFERTIL-SV020 | Windows 2016 | WinRAR,  |
| BREMEN-SV020 | Windows 2019 | Java JRE 1.8.x, 7-zip |
| BREMEN-SV030 | Windows 2019 | Java JRE 1.8.x, 7-zip |
| FORTIN-SV020 | Windows 2019 |  |
| NAFOSA-SV020 | Windows 2019 |  |
| CROWE-SV020 | Windows 2019 |  |
| SIMIEN-SV020 | Windows 2019 |  |
| QUIMECO-SV020 | Windows 2019 |  |
| FUTURA-SV020 | Windows 2019 |  |

---

### Antivirus/Protección ante amenazas

La protección ante amenazas de Kaspersky debe estar activada en todos los terminal servers. Y en caso de no estar activada esto tiene que poder justificarse. 

Se debe mantener un inventario del estado de la protección en los servidores, para esto se puede utilizar el informe generado por Kaspersky y filtrado para los sistemas B1.

### Estado actual:

Ahora mismo todos los servidores windows tienen activada la protección. Pero últimamente ocurrió un problema donde Kaspersky detecta intentos de log-in de distintos usuarios como un ataque de fuerza bruta y niega todas las conexiones, llevando a que tengamos que apagar la protección.

La solución a esto vendrá de aplicar mejores políticas de seguridad de red y analizando una configuración distinta del firewall, mientras tanto, se debe tomar nota de cuando se apaga la protección y se debe volver a encender cuanto antes.

---

### Políticas de contraseñas

Se deben ejercer políticas respecto a la complejidad de las contraseñas y cada cuanto estas deben ser cambiadas. Por default, Windows server configura un conjunto de reglas iniciales, por lo que ya tenemos una configuración inicial. Eventualmente se debe evaluar si estas reglas definen una política lo suficientemente robusta.

![Untitled](Teknos/SAP%20B1/Proyecto%20Asegurar%20Sistemas%20SAP%20B1%20con%20Terminal%20Ser%2063c3ca458d0847509e10d6f4951439e5/Untitled%202.png)

---

## A definir próximamente:

- Políticas de red y configuración de firewalls virtuales.
    - Documentar configuraciones actuales.
    - Analizar visibilidad de la red local al sistema B1 y de la red local de Teknos
    desde los terminal server.
    - Prohibir la salida de conexiones RDP/SSH desde el terminal server hacia otros servidores de la LAN.
- Políticas puertos abiertos/ servicios ofrecidos.
    - Documentar los puertos abiertos de cada servidor y analizar cuáles son necesarios.
    - Cerrar los puertos que no sean necesarios
- Politicas de seguridad para los servidores SAP B1 Linux.
    - Documentar uso actual de los servidores, qué usuarios realizan los procesos y sus permisos.
    - Politicas de protección de endpoints, considerar aplicar antivirus.
- Politicas de seguridad para los servidores SAP B1 Windows.
    - Documentar caso de Nafosa, el servidor SAP corre en Windows con SQL Server.
    - Documentar usuarios y privilegios necesarios.
- Políticas de backup.
    - Documentar el proceso de backup actual.
    - Analizar posible vulnerabilidad en la conectividad actual con servidores de backup.
- Monitoreo y Logging.
    - Politicas de logging para los firewalls virtuales.
    - Analizar posibilidad de centralizar los logs en un SIEM.
- Manejo del acceso privilegiado.
    - Políticas credenciales de root/ usuarios administradores.
    - Realizar pruebas de sistemas de manejo del acceso privilegiado/ manejo de secretos.