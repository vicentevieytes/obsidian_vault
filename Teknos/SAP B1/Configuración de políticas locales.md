# Configuración de políticas locales para restringir usuarios en Windows Terminal Servers.


## Reemplazado por [[SV020 B1 Configuración AppLocker]]

## Objetivo

---

Los terminal servers son servidores windows a donde los usuarios del cliente se conectan via RDP para trabajar. Para evitar que se comprometa la seguridad de estos servidores y mitigar la amenaza que estos usuarios podrían representar al sistema se propone:

- Restringir la capacidad de estos usuarios de ejecutar aplicaciones que no sean indispensables para su rol.
- Restringir la capacidad de estos usuarios de ejecutar comandos, ejecutar el administrador de tareas, y de cambiar su contraseña

## **Procedimiento**

---

## Acceder al editor de object policies.

Para aplicar una política local sobre un grupo, se debe acceder al servidor donde se quiera aplicar la política y seguir el siguiente procedimiento:

1. Acceder a Microsoft Management Console (MMC)
2. Navegar a file > add/remove snap-ins 
    
    ![Untitled](Teknos/SAP%20B1/IMG%20Configuración%20de%20políticas%20locales%20para/Untitled.png)
    

1. Seleccionar el Snap-in *“Group Policy Object Editor*” y clickear en *“add”*. Esto nos permitirá agregar políticas sobre un cierto user o grupo.
    
    ![Untitled](Teknos/SAP%20B1/IMG%20Configuración%20de%20políticas%20locales%20para/Untitled%201.png)
    
2. Seleccionar *“Browse…”* para elegir el user o grupo sobre el que se quieran manejar las políticas

![Untitled](Teknos/SAP%20B1/IMG%20Configuración%20de%20políticas%20locales%20para/Untitled%202.png)

![Untitled](Teknos/SAP%20B1/IMG%20Configuración%20de%20políticas%20locales%20para/Untitled%203.png)

A modo de ejemplo, seleccionamos el grupo de usuarios que no sean administradores, pero es conveniente tener un grupo “SAPClientOnlyUsers” o “RestrictedUsers” dedicado para los usuarios que van a trabajar en el servidor.

Una vez seleccionado el grupo sobre el que queremos editar las politicas, debería verse así. 

![Untitled](Teknos/SAP%20B1/IMG%20Configuración%20de%20políticas%20locales%20para/Untitled%204.png)

Confirmamos.

Una vez agregado el snap-in, podemos configurar políticas para el grupo de usuarios. La view agregada a MMC nos da acceso a un arbol de posibles políticas para configurar.

## Políticas a configurar:

### **Solo permitir ejecución de aplicaciones específicas.**

Navegar en MMC al editor de Policy Objects para el grupo elegido hasta:

 `**Local Computer\{grupo} Policy\User Configuration\Administrative Templates\System\**` 

![Untitled](Teknos/SAP%20B1/IMG%20Configuración%20de%20políticas%20locales%20para/Untitled%205.png)

**Navegar a la política *“Run only specfied windows applicaitons”.***

![Untitled](Teknos/SAP%20B1/IMG%20Configuración%20de%20políticas%20locales%20para/Untitled%206.png)

******************Importante******************: NO CONFUNDIR con “***don’t** run specified Windows Applications”* ya que el efecto de esa política es el opuesto al deseado.

Seleccionar “Enabled”, y hacer click en List of allowed applications *show.*

![Untitled](Teknos/SAP%20B1/IMG%20Configuración%20de%20políticas%20locales%20para/Untitled%207.png)

Esta lista aparecerá vacía en un principio, pero se puede rellenar con los paths a las aplicaciones que queremos permitir ejecución por parte del usuario. En este ejemplo se agregaron los paths para permitir la ejecucion de saplogon.exe, SAP Business One.exe, Acrobat.exe. Otro cliente podría necesitar otros paths.

********************Importante: Esto es solo un ejemplo de posibles aplicaciones para permitir, no se debe replicar exactamente para todos los servidores.********************

![Untitled](Teknos/SAP%20B1/IMG%20Configuración%20de%20políticas%20locales%20para/Untitled%208.png)

Antes de permitir la ejecución de una aplicación, se debe considerar si la aplicación es indispensable para el usuario y si el cliente expresó que la ejecución de estas tiene que estar permitida. La idea es reducir al máximo la capacidad del usuario de afectar el sistema y ejecutar código permitiendo aún así que haga su trabajo.

La siguiente es una lista de posibles paths para añadir a la política de aplicaciones permitidas: 

`**"C:\Program Files\sap\SAP Business One\SAP Business One.exe”**`

`**“C:\Program Files (x86)\SAP\FrontEnd\SAPgui\saplogon.exe”**`

`**“C:\Program Files (x86)\SAP\FrontEnd\SAPgui\SAPgui.exe”**`

`**“C:\Program Files (x86)\SAP\FrontEnd\SAPgui\saplgpad.exe”**`

`**"C:\Program Files\Microsoft Office\root\Office16\WINWORD.EXE”**`

`**"C:\Program Files\Microsoft Office\root\Office16\EXCEL.EXE”**`

`**"C:\Program Files\Adobe\Acrobat DC\Acrobat\Acrobat.exe”**`

**********************Importante: Verificar en cada máquina el path de instalación de la aplicación a permitir, puede que en distintos Windows los mismos programas estén instalados en distintos directorios.********************** 

Aclaración: Esta política no restringe el uso de explorer.exe. Ya que el explorador de windows es la default shell de los usuarios, este se mantiene operativo incluso después de aplicar esta política. En caso de querer restringir de manera extrema al usuario, se podría considerar la posibilidad de cambiar la default shell del usuario a una más restrictiva.

---

### Prohibir el acceso al Panel de Control y a PC settings.

Navegar desde el editor de policy objects a 

 `**\User Configuration\Administrative Templates\Control Panel**`

![Untitled](Teknos/SAP%20B1/IMG%20Configuración%20de%20políticas%20locales%20para/Untitled%209.png)

Habilitar la política *”Prohibit acces to Control Panel and PC settings”*.

![Untitled](Untitled%2010.png)

---

### Prevenir el acceso a la consola de comandos

Navegar desde el editor de policy objects a  `**\User Configuration\AdministrativeTemplates\System\**` y habilitar la política *“Prevent access to the command prompt”*. Esto previene el acceso a CMD y a la ejecución de archivos batch.

Puede llegar a deshabilitar batch logon scripts, así que se debe tener en cuenta antes de habilitar. En las pruebas se configuró un logon script en powershell que no tuvo problema en ejecutarse con esta política activada. 

![Untitled](Untitled%2011.png)

---

### Prevenir el acceso a herramientas de edición del registro

Navegar desde el editor de policy objects a  **`\User Configuration\AdministrativeTemplates\System\`** y habilitar *Prevent access to registry editing tools.*

![Untitled](Untitled%2012.png)

---

### Prevenir acceso al Administrador de Tareas

Navegar desde el editor de policy objects a 

`**\User Configuration\AdministrativeTemplates\System\Ctrl+Alt+Del Options**` y habilitar *Remove Task Manager.* También si se considera necesario se puede habilitar **********************Remove Change Password**********************

![Untitled](Untitled%2013.png)

---

### Opcional: Logon script para iniciar una aplicación en inicio de sesión.

Si se considera necesario que al iniciar la sesión remota se inicie automáticamente la aplicación donde el usuario va a trabajar, se puede configurar la ejecución de un script personalizado en el inicio de sesión del usuario. 

Lo primero es escribir un script en Powershell que ejecute el inicio de la aplicación y almacenarlo con la extensión “.ps1”.

Este es un script de ejemplo que inicia el programa saplogon.exe, se puede cambiar el path por el de cualquier otra aplicación y se pueden agregar más aplicaciones que se quieran ejecutar en el inicio de sesión:

```powershell
**# Start SAP Logon Script
Start-Process -FilePath "C:\Program Files\sap\FrontEnd\SAPGUI\saplogon.exe" -NoNewWindow**
```

********Importante********: ************************************************************************************************************************************************************************El usuario debe tener permisos permisos para leer y ejecutar el script  pero NO para escribir en el archivo. Si esto no se configura correctamente el usuario podría modificar el script a su gusto y ejecutar código arbitrario cada vez que inicie la sesión.************************************************************************************************************************************************************************

************************************************************************************************************************************************************************Almacenar en un directorio a donde no tengan acceso de escritura los usuarios (por ejemplo se puede crear `C:\Scripts` para guardar el script. Siempre revisar los permisos.**

![Untitled](Untitled%2014.png)

Devuelta en MMC, navegar a `**User Configuration\ Windows Settings\ Scripts (Logon/Logoff)**`

Seleccionar *Logon*, ir a *PowerShell Scripts* y agregar el path del powershell script que inicia la aplicación.

![Untitled](Untitled%2015.png)