En el servidor Windows CFERTIL-SV020, que funciona como terminal server todos los usuarios forman parte del grupo "administrators". Se entiende que esto es así para permitirle acceso a los usuarios a los módulos de la carpeta `C:\Apps.bit`. 
## Permisos de la carpeta C:\\Apps.bit
Se revisaron los permisos configurados en las carpetas `C:\Apps.bit` y `C:\Apps.bit\produccion`, los cuales indican que cualquier usuario -sea o no administrador- puede leer escribir y ejecutar los archivos de las carpetas. Por lo tanto un usuario raso (no-administrador) debería poder acceder sin problemas.

![[Pasted image 20231121163137.png]]

## Pruebas realizadas

Para realizar pruebas se generó un usuario raso TEST_BIT.

Se verificó que los ejecutables pueden ser corridos con un usuario no-administrador, por ejemplo, el programa `C:\Apps.bit\Produccion\Impuestos\impuesto.exe` puedo ser ejecutado.

![[Pasted image 20231121164452.png]]

Pero, cuando se intentó acceder al mismo ejecutable mediante el acceso directo de la carpeta "Accesos" del escritorio de TEST_BIT, el sistema nos pidió que accedamos como administradores.

![[Pasted image 20231121161441.png]]

![[Pasted image 20231121161532.png]]

Esto es porque el acceso directo tiene la propiedad "ejecutar como administrador", cuando se desactiva se pueden ejecutar los accesos directos sin necesidad de ser administrador. Así están configurados todos o casi todos los accesos directos de la carpeta "Accesos/Producción"  del escritorio.

![[Pasted image 20231121161610.png]]
![[Pasted image 20231121161631.png]]

Se realizó una copia del acceso directo a `Impuesto.exe`, en esta copia se removió la propiedad avanzada "ejecutar como administrador" y el usuario raso pudo a través del acceso directo ejecutar el módulo.

![[Pasted image 20231121161659.png]]
![[Pasted image 20231121161738.png]]

## Cómo aplicar en todos los escritorios

Esta propiedad se puede cambiar en cada uno de los accesos directos de modo tal que afecte a los escritorios de todos los usuarios desde `C:\\Users\\Public\\Desktop\\Accesos\\Produccion.

![[Pasted image 20231121161114.png]]

## Siguientes pruebas

Hace falta verificar si "ejecutar como administrador" es o no necesario para el correcto funcionamiento de los programas de  `C:\Apps.bit\Produccion`.

Para esto, un administrador de BIT tendría que:
- Acceder con un usuario no-administrador como TEST_BIT.
- Ejecutar alguno de los módulos mediante un acceso directo sin la propiedad "ejecutar como administrador".
- Iniciar sesión en el sistema BIT de CFertil y verificar si las aplicaciones funcionan correctamente.