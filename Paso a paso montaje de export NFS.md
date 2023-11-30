### Lado servidor NFS
Agregar las ips que pueden acceder siguiendo el formato de las ya configuradas.
```
nano /etc/exports
```
### Lado cliente NFS
- Verificar instalación y estado de servicios de cliente NFS
```
yum install nfs-utils
systemctl status nfs-utils.service
systemctl start nfs-utils.service
systemctl enable nfs-utils.service
```
- Verificar que las exports sean accesibles
```
showmount -e [ip_server]
```
- Crear directorio donde montar
```
mkdir /mnt/FileAWS
```
- Editar /etc/fstab y agregar
```
[ip_server]:/BKPHANAPRD/FileAWS /mnt/FileAWS nfs rw 0 0
```
- Montar
```
mount -a
```
