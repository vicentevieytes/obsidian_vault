El cliente Farmanet tiene un servidor redhat donde corre servicios SAP, además de esto, tiene un servidor de contingencia que mantiene una copia de los sistemas corriendo listos para hacer un cambio en caso de necesidad. El servidor de contingencia estuvo crasheando.

El mensaje de error dice:

```bash
Core dump to |/usr/lib/systemd/systemd-coredump 10506 0 0 6 1694617218 0 fnet.loc osqueryd pipe failed.

```

```bash
PID     USER        PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
25892 hepadm    20   0  140.3g 126.2g   6.5g S 556.8  16.0  14:18.49 hdbindexserver
27414 hepadm    20   0 3170280 342884 243028 R 101.2   0.0   0:04.06 hdbnsutil

```

**El proceso 25892, ejecutado por el usuario hepadm, es el que consume la mayor cantidad de recursos. Este proceso es el servidor de indexación de SAP HANA, que se utiliza para optimizar la búsqueda y recuperación de datos en la base de datos**

```bash
                    total        used        free      shared  buff/cache   available
Mem:          787Gi       777Gi       3.0Gi       6.0Gi       7.1Gi       3.8Gi
Swap:         4.0Gi       2.6Gi       1.4Gi
```

**El server usa casi toda la memoria que tiene asignada.**

```bash
[root@fsaphep01 ~]# df -h
Filesystem                         Size  Used Avail Use% Mounted on
devtmpfs                           394G     0  394G   0% /dev
tmpfs                              394G  120K  394G   1% /dev/shm
tmpfs                              394G  1.4M  394G   1% /run
tmpfs                              394G     0  394G   0% /sys/fs/cgroup
/dev/mapper/rhel-root               70G  9.1G   61G  13% /
/dev/sda1                         1014M  306M  709M  31% /boot
/dev/mapper/rhel-home              377G   32G  345G   9% /home
/dev/mapper/vg--data-lv--data      1.5T  831G  705G  55% /hana/data
/dev/mapper/vg--log-lv--log        500G   18G  483G   4% /hana/log
/dev/mapper/vg--shared-lv--shared  1.0T   28G  996G   3% /hana/shared
tmpfs                               79G   12K   79G   1% /run/user/42
tmpfs                               79G     0   79G   0% /run/user/974
tmpfs                               79G  4.0K   79G   1% /run/user/1001
tmpfs                               79G     0   79G   0% /run/user/0
```

El server tiene bastante almacenamiento libre en todos los storages

Los “dispositivos” relacionados con el sistema SAP son

```bash
/dev/mapper/vg--data-lv--data      1.5T  831G  705G  55% /hana/data
/dev/mapper/vg--log-lv--log        500G   18G  483G   4% /hana/log
/dev/mapper/vg--shared-lv--shared  1.0T   28G  996G   3% /hana/shared
```

que estan montados en /hana/{data, log, shared} respectivamente

```bash
[root@fsaphep01 ~]# journalctl -xe
Sep 13 13:37:59 fsaphep01.fnet.loc kernel: [ 31245] 0 31245 1831 19 53248 0 0 sleep
Sep 13 13:37:59 fsaphep01.fnet.loc kernel: oom-kill:constraint=CONSTRAINT_NONE,nodemask=(null),cpuset=/,mems_allowed=0,global_oom,ta>
Sep 13 13:37:59 fsaphep01.fnet.loc kernel: Out of memory: Killed process 28890 (hdbindexserver) total-vm:335970848kB, anon-rss:29296>
Sep 13 13:37:59 fsaphep01.fnet.loc kernel: Core dump to |/usr/lib/systemd/systemd-coredump 29180 973 973 6 1694623079 0 fsaphep01.fn>
Sep 13 13:38:07 fsaphep01.fnet.loc kernel: oom_reaper: reaped process 28890 (hdbindexserver), now anon-rss:0kB, file-rss:0kB, shmem->

```

## Diagnóstico

En la timestamp **Sep 13 13:37:59,** la kernel de linux generó una log entry indicando un out of memory event (oom-kill) se puede ver en la linea 2 de los logs.

En la linea 3 indica que el programa con pid 28890, identificado como "hdbindexserver” fue forzosamente terminado por este evento oom.

En la linea 4 indica a donde se escribió el coredump.

En la linea 5 indica que el proceso reaper limpió la memoria que estaba ocupada por el proceso que fue terminado.

Pareciera que en determinados momentos se llena la memoria que tiene asignada el proceso y en ese momento el sistema tiene que killearlos.