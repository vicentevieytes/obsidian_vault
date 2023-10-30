# b1

En b1 todos los usuarios necesitan una licensia y en erp podés tener “licencias de test” o “temporales” además del usuario administrador que no necesita licencia.

Usuario support no anadaba despues de reiniciar.

Necesitaban un Add-On y hay que pagar licencias.

El RDS lee una DLL lamada termsvr la dll esta trucada y cada gvez que la reiniciamos la lee

La DLL si no tenes licencia te permite solo dos usuarios activos al mismo tiempo. Cuando pones la licencia la DLL se modifica. Conseguimos una trucada para poder laburar sin licencias.

La otra vuelta es en 2016 y 2019 se corre un servicio de administracion de licencia ese lo que ahce es cada vcez que un usuario se conecta al remotoe ,.e genera un cert temporal de 23 meses. Por mas que reiniciemos el cert sigue pero pasado 3 meses cadcan y el user no ese puede conectar más de una hora. Tenés que poner una licencia para hacer los cert temporales permanentes. Los que no tenian licencia se borraba la base de certificados temporales y se hacia que se generen nuevos. La otra esta en puvote de BA en vez de 3 meses ampliacdo a 99000 días. Entonces no necesitabas reemplazar los cservicios ni niada ni licencia. Cada x tiempo aparece un cartelito que dice te quedean 99700 dias de uso. Ahora estamos empezando a comprar licencias.

| Cliente | VLAN | admsoporte | SO | Licencias RDS |
| --- | --- | --- | --- | --- |
| MARK | 185 | - | Windows 2012 R2 | No tiene |
| GEC | 165 | Seidor | Windows 2012 R2 | No tiene |
| VECHER | 175 | - | Windows 2012 R2 | No tiene |
| GERVASINI | 182 | - | Windows 2016 | Licencia oficial User x50 |
| NRG | 113 | pragmatica | Windows 2016 | Licencia oficial User x50 |
| GEOPATAGONIA | 133 | - | Windows 2016 | Licencia oficial User x50 |
| APAMPA | 170 | Soportebit | Windows 2016 | No tiene |
| ARANAMI | 132 | - | Windows 2016 | No tiene |
| CFERTIL | 142 | - | Windows 2016 | Falta aplicar |
| BREMEN | 172 | maiten | Windows 2019 | Licencia oficial User x25 |
| BREMEN | 172 | maiten | Windows 2019 | Licencia oficial User x25 |
| FORTIN | 193 | pragmatica | Windows 2019 | Licencia oficial User x50 |
| NAFOSA | 202 | admbit / adminvenzis | Windows 2019 | Licencia oficial User x50 |
| CROWE | 105 | - | Windows 2019 | No tiene |
| SIMIENTE | 205 | admBIT | Windows 2019 | Licencia oficial User x50 |
| QUIMECO | 211 | admBIT | Windows 2019 | No tiene |
| FUTURA | 209 | pragmatica | Windows 2019 | Licencia oficial User x50 |
| CIGRA | 213 |  | Windows 2019 | No tiene |