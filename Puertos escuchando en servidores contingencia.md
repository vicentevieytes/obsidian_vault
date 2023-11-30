Puertos abiertos escuchando en Geopaxi Contingencia

![[Pasted image 20231130142536.png]]

Puertos abiertos escuchando en Aranami contingencia

![[Pasted image 20231130142717.png]]

Los puertos a la escucha no coinciden en ambos servidores, por lo que es dificil identificar cuales son los utilizados para la sincronización.
Se puede proceder habilitando en el firewall rangos genéricos de puertos de SAP en todos los firewalls de contingencia.
Otra opción es acceder a cada servidor de contingencia, correr un comando para listar los puertos escuchando, decidir cuales de esos servicios deberían ser alcanzables y permitir solo el tráfico hacia esos en el firewall.

Examinando el tráfico entre PRD y Contingencia, se observa que la mayoria de los datos se transfieren usando los puertos por encima del 40000.

Aún así, también se observa tráfico entre PRD y Contingencia usando los puertos estandard de SAP del rango 30000-31000.

Además, se observa que por ejemplo los servidores que funcionan con SAP instancia 0 tienen abiertos puertos en el rango 40000-40099 y los de instancia 3 tienen abiertos puertos en el rango 40300-40399. De la misma manera que ocurre con los puertos estandard de SAP del rango 30000.

Para proceder habría que seleccionar alguna contingencia de prueba. Luego, en el firewall de esta, restringiríamos el tráfico a solo los puertos por encima de 40000 para ver si con esto alcanza para mantener la sincronización. De lo contrario, se habilitarían también los puertos del rango 30000. Luego, restringiríamos a solo los del rango de la instancia particular de la contingencia de prueba.
