Buenas tardes, estimados.

  

Como en la reunión de hoy mencionaron que los los usuarios finales tenían permisos de administrador para poder acceder a los ejecutables de C:\Apps.bit se realizó en CFERTIL-SV020 una prueba con un usuario raso TEST_BIT que no pertenece al grupo administrators.

  

El usuario no-administrador no podía correr los ejecutables desde el acceso directo de la carpeta "Accesos" de su escritorio, pero sí ejecutandolo directamente desde C:\Apps.bit\Produccion\.

Esto es porque el acceso directo tiene la propiedad "ejecutar como administrador", cuando se desactiva se pueden ejecutar los accesos directos sin necesidad de ser administrador.

  

No detectamos ningún otro programa que no nos dejara ejecutarlo por no tener suficientes permisos.

  

Necesitamos que verifiquen si las aplicaciones funcionan correctamente o no aún cuando no son lanzadas con la propiedad "ejecutar como administrador". Para esto les dejamos los datos de acceso a TEST_BIT:

  

Usuario: TEST_BIT

Contraseña: 95Hf*!)g2.7V

  

Adjunto un archivo pdf que documenta los detalles técnicos de las pruebas de hoy, y cómo replicarlo para desactivar la opción "ejecutar como administrador" en los accesos directos.  

  

  

Saludos cordiales.