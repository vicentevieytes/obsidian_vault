Este documento describe la configuración básica de AppLocker según la
## Servicio "Application Identity"

Applocker depende del servicio, así que hay que activarlo. Accediendo a services.msc.

![[Pasted image 20231020122505.png]]

Para automatizar el inicio del servicio, acceder al cmd como administrador y correr
`sc.exe start appidsvc`
`sc.exe config appidsvc start= auto`
Verificar que el servicio esté corriendo y cumpla Startup Type =  "Automatic".

## Encender AppLocker
Acceder a secpol.msc y navegar a Application Control Policies\\AppLocker
![[Pasted image 20231020123707.png]]
Acceder a "Configure rule enforcement", activar el enforcement de todas las reglas y aplicar.
![[Pasted image 20231020123853.png]]

## Configuración predeterminada
Desplegar la pestaña AppLocker, inicialmente no abrá reglas definidas ningún tipo de ejecutable.
![[Pasted image 20231020124130.png]]

En cada de las pestañás de reglas, hacer click derecho y seleccionar "Create default rules"
![[Pasted image 20231020124300.png]]
![[Pasted image 20231020124331.png]]
![[Pasted image 20231020124344.png]]

## Agregar excepciones
Executable rules
	Click derecho en la regla para permitir todo lo de la carpeta windows
	Añadir excepciones
	Seleccionar
	C:/Windows/regedit.exe
	C:/Windows/System32/cmd.exe
	C:/Windows/System32/WindowsPowershell/powershell.exe
	C:/Windows/System32/WindowsPowershell/powershell_ISE.exe