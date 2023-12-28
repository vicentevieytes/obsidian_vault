Wazuh is a free and open source security platform that unifies [[XDR]] and [[SIEM]] capabilities. It protects workloads across on-premises, virtualized, containerized, and cloud-based environments. 

###   
indexador wazuh![Instalar el indexador Wazuh](https://wazuh.com/wp-content/themes/wazuh-v3/assets/images/install/wazuh-indexer.png)

El indexador de Wazuh es un motor de análisis y búsqueda de texto completo altamente escalable.  
Este componente central indexa y almacena alertas generadas por el servidor Wazuh.

### servidor wazuh![Instalar el servidor Wazuh](https://wazuh.com/wp-content/themes/wazuh-v3/assets/images/install/wazuh-server.png)

El servidor Wazuh analiza los datos recibidos de los agentes y los procesa utilizando inteligencia sobre amenazas.  
Un único servidor puede analizar datos de miles de agentes y escalar cuando se configura como un clúster. También se utiliza para gestionar los agentes, configurándolos de forma remota cuando sea necesario.

### Panel de control de Wazuh![Instalar el panel de Wazuh](https://wazuh.com/wp-content/themes/wazuh-v3/assets/images/install/wazuh-dashboard.png)

El panel de Wazuh es la interfaz de usuario web para la visualización, el análisis y la gestión de datos.  
Incluye paneles de control para cumplimiento normativo, vulnerabilidades, integridad de archivos, evaluación de configuración, eventos de infraestructura en la nube, entre otros.

Instalado en 192.168.22.104 en cluster BA, con 70gb de disco en un Ubuntu Server.

26/12/2023 22:44:32 INFO: Wazuh API admin credentials not provided, Wazuh API passwords not changed.

26/12/2023 22:44:55 INFO: The password for user admin is YS8.VO+bP9tmUaDH+eMtHDslqyEg4VDm
26/12/2023 22:44:55 INFO: The password for user kibanaserver is UIFmrKe42s68GL0mH8GZ4hqj.cZTklSS
26/12/2023 22:44:55 INFO: The password for user kibanaro is McwDqYBqHIETSLnxp5*dGXwIRUv1*4U?
26/12/2023 22:44:55 INFO: The password for user logstash is 3W0BBdGDCPksmq5+wh.pUUfIrNsM?I2C
26/12/2023 22:44:55 INFO: The password for user readall is uZkP3.e+7mUc463jslIxbBUhvNyk?esZ
26/12/2023 22:44:55 INFO: The password for user snapshotrestore is WMJT?k1IcErCZyH15pjSwyuFzVn.ZYDI
26/12/2023 22:44:55 WARNING: Wazuh indexer passwords changed. Remember to update the password in the Wazuh dashboard and Filebeat nodes if necessary, and restart the services.

pass agentes: echo "TTGS3cur1ty2024" > /var/ossec/etc/authd.pass