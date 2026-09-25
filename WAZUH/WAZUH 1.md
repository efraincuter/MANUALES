### Instalar Wazu en Ubuntu 24
```
curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh
sudo bash ./wazuh-install.sh -a
```

### Adicionar un Agente a Ubuntu 24

```
En endpoints -> Deploy New agent -> Seleccionar DEB amd64
Server address: La IP del servidor 
Assign an agent name :  nombre del servidor
Luego seguir los pasos de isntalacion y reinicio de servicio en el servidor TESTAPACHE

en el servidor apache realziar una configuracion en su archivo ossec
root@SRV-TEST:/var/www/html# vim /var/ossec/etc/ossec.conf

En la segunda parte de config esta de esta manera para una pruebas
<ossec_config>
  <localfile>
    <log_format>journald</log_format>
    <location>journald</location>
  </localfile>

  <localfile>
    <log_format>apache</log_format>
    <location>/var/log/apache2/error.log</location>
  </localfile>

  <localfile>
    <log_format>apache</log_format>
    <location>/var/log/apache2/access.log</location>
  </localfile>

  <localfile>
    <log_format>syslog</log_format>
    <location>/var/ossec/logs/active-responses.log</location>
  </localfile>

  <localfile>
    <log_format>syslog</log_format>
    <location>/var/log/dpkg.log</location>
  </localfile>

</ossec_config>

en la opcion de syschek esta de eta forma la priemra linea

 <syscheck>
    <disabled>no</disabled>

    <!-- Frequency that syscheck is executed default every 12 hours -->
    <frequency>300</frequency>

    <scan_on_start>yes</scan_on_start>
    <alert_new_files>yes</alert_new_files>

    <!-- Directories to check  (perform all possible verifications) -->
    <directories>/etc,/usr/bin,/usr/sbin</directories>
    <directories>/bin,/sbin,/boot</directories>
    <directories>/var/www/html</directories>

Reiniciar servicio 
systemctl restart wazu-agent
systemctl restart wazuh-agent


```