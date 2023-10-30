# Apagar Linux SAP B1

```
cd /etc/init.d/
./sapb1servertools status
./sapb1servertools stop -> parar servicio b1

su - ndbadm
HDB stop  --> Apagar Hana DB

reboot --> Reiniciar servidor

su - ndbadm
HDB start --> Encender Hana DB

cd /etc/init.d/
./sapb1servertools status --> verificar
```