
En la reunión del 11 de Octubre de 2023 decidimos que de [la checklist original](https://docs.google.com/document/u/0/d/1X7KZl_CcfeAQ3xTL_I9O_08StXEFEs0PmjlFjuamVlQ/edit), los siguientes proyectos son la prioridad actual para incrementar la seguridad y mitigar los riesgos más críticos de la organización:

### Asegurar servidores RDP para SAP B1. (Marcelo y Vicente)
- Contactar a los clientes y confirmar la restricción de los usuarios RDP a solo las aplicaciones que necesitan.
- Aplicar las políticas de restricción en estos clientes.
- Mantener los parches de seguridad aplicados en todos estos hosts.
- Analizar soluciones para el problema del acceso privilegiado a estos servidores. Algunas posibilidades son:
- Manejo de secretos, con distintas credenciales secretas para cada servidor y manejo de acceso a determinados secretos para cada administrador.
- (Ampliar con más posibilidades)

[[Proyecto Asegurar Sistemas SAP B1]]


### Enumeración externa de las ips públicas y los dominios/ subdominios de la empresa. (Vicente)
- Enumerar subdominios y las ips a las que resuelven.
- Escanear puertos abiertos en cada ip.
- Analizar la exposición en general de la empresa.
- Redactar reporte.
[[Enumeración y escaneos externos]]

### Tests de penetración no autenticados a portales para clientes. (Vicente)
- Definir qué portales son prioridad. Y elegir uno o dos cada dos semanas.
- Primer target desarrollo: [https://devengiemx.alaiaconsulting.com](https://devengiemx.alaiaconsulting.com) [[Pentest no autenticado engiemx]]
- Continuar con producción:
  m.cia.alaiaconsulting.com;
  cia.alaiaconsulting.com;
  m.horas.teknosgroup.com;
  horas.teknosgroup.com;
  rizopartners.rizobacter.com;
  mba.teknosgroup.com;
  m.mba.teknosgroup.com;
  engiemx.alaiaconsulting.com;
  

- Para cada uno enumerar vulnerabilidades y analizar la posibilidad de explotación.
- Redactar reporte.

### Implementar autenticación de dos pasos en el VPN. (Alejandro)


### Mitigación de DDOS/ implementación de WAF con Cloudflare (Facundo y Pablo)


### Implementación de teleport para id y acceso por SSH centralizado a los hosts de K8s. (Facundo y Pablo)
