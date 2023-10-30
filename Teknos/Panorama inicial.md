Evaluación de los assets a proteger, esto es todo lo que sea de valor para la empresa y que se debe proteger. Incluyendo pero no limitandonos a:

- Los datos de Teknos, sus empleados e informacion privada financiera o personal de la organización.
- Los datos de los clientes de Teknos, datos de sus usuarios, sus clientes, datos financieros o personales.
- Datos de acceso a sistemas, credenciales, certificados.
- Información sobre como se estructura la infraestructura de los sistemas.
- Información sensible sobre las vulnerabilidades conocidas dentro de los sistemas.
- El sistema de correo, el sistema de tickets.
- La infraestructura de redes, la infraestructura de virtualización,
- La infraestructura de backups, de contingencia y almacenamiento.
- Los dominios internos, los directorios y usuarios de active directory.
- La operatividad de los clientes de Teknos y sus sistemas empresariales.
- Las personas de importancia para Teknos.
- El hardware y el acceso físico a los sistemas.

Este es el tesoro que tenemos que proteger, parte del tesoro se encuentra dentro del castillo detrás de los firewalls de la empresa donde solo entran aquellos que están autorizados, y que conocen la dirección de acceso por VPN. Estos assets son la infraestructura física, los sistemas de virtualización, el active directory, el correo, los sistemas de teknos en general.

Por otro lado, VLANs más pequeñas se encuentran segmentadas con firewalls internos que no permiten que los que se encuentran ahí dentro se muevan a otras salas del castillo. Estos son los segmentos a los que tienen acceso los clientes y sus usuarios. El problema con estos assets es que se accede por una puerta trasera, donde nadie revisa activamente quienes entran y quienes salen, solo con saber el usuario y la contraseña se puede llegar desde la WAN. 

Los datos de acceso a todos los sistemas ahora mismo se encuentran fuera el castillo, resguardados bajo una ilusión de control en google drive. Aquí se puede controlar el acceso de distintas cuentas de google, pero la cuenta de google de algún empleado fuera comprometida, no tendriamos manera de enterarnos de esto.

Además de esto, muchos assets viven en la mente de los empleados y ex-empleados de teknos, dominio donde es extremadamente dificil de controlar las brechas. Los empleados envían los datos de acceso y las credenciales por mail en texto plano y el uso de claves maestras para el acceso a distintos sistemas es ubicuo.

Por otro lado, nuestra empresa intercambia conocimiento, datos, archivos, y usuarios con otras empresas. Cuando un consultor de Teknos se conecta al VPN de un cliente para trabajar allí, entra el castillo de otra empresa, y puede llevar o traer información sensible o malware de un lado a otro.