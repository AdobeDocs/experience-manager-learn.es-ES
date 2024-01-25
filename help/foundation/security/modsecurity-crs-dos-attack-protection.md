---
title: AEM Utilice ModSecurity para proteger su sitio de la de ataques DoS
description: Obtenga información sobre cómo habilitar ModSecurity para proteger el sitio de ataques de denegación de servicio (DoS) mediante el conjunto de reglas principales de ModSecurity de OWASP (CRS).
feature: Security
version: 6.5, Cloud Service
topic: Security, Development
role: Admin, Architect
level: Experienced
jira: KT-10385
thumbnail: KT-10385.png
doc-type: Article
last-substantial-update: 2023-08-18T00:00:00Z
exl-id: 9f689bd9-c846-4c3f-ae88-20454112cf9a
duration: 843
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1172'
ht-degree: 0%

---

# AEM Utilice ModSecurity para proteger el sitio de la de ataques DoS

Obtenga información sobre cómo habilitar ModSecurity para proteger el sitio de ataques de denegación de servicio (DoS) mediante **Conjunto principal de reglas de seguridad de OWASP Mod (CRS)** en Adobe Experience Manager AEM () Publish Dispatcher.


>[!VIDEO](https://video.tv.adobe.com/v/3422976?quality=12&learn=on)

## Información general

El [Abrir proyecto de seguridad de aplicaciones web® (OWASP)](https://owasp.org/) foundation proporciona el [**Principales 10 de OWASP**](https://owasp.org/www-project-top-ten/) que resume las diez preocupaciones de seguridad más críticas para las aplicaciones web.

ModSecurity es una solución de código abierto y multiplataforma que proporciona protección contra una amplia gama de ataques contra aplicaciones web. También permite la monitorización del tráfico HTTP, el registro y el análisis en tiempo real.

OWSAP® también proporciona el [Conjunto de reglas principales de OWASP® ModSecurity (CRS)](https://github.com/coreruleset/coreruleset). El CRS es un conjunto de **detección de ataques** reglas para usar con ModSecurity. Por lo tanto, CRS tiene como objetivo proteger las aplicaciones web de una amplia gama de ataques, incluido el OWASP Top Ten, con un mínimo de falsas alertas.

Este tutorial muestra cómo habilitar y configurar el **PROTECCIÓN DOS** Regla CRS para proteger el sitio de un posible ataque DoS.

>[!TIP]
>
>AEM Es importante tener en cuenta que el de la as a Cloud Service es el de la [CDN administrado](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=es) satisface la mayoría de los requisitos de rendimiento y seguridad del cliente. Sin embargo, ModSecurity proporciona una capa adicional de seguridad y permite reglas y configuraciones específicas del cliente.

## Agregar CRS al módulo del proyecto de Dispatcher

1. Descargue y extraiga [Último conjunto de reglas principales de OWASP ModSecurity](https://github.com/coreruleset/coreruleset/releases).

   ```shell
   # Replace the X.Y.Z with relevent version numbers.
   $ wget https://github.com/coreruleset/coreruleset/archive/refs/tags/vX.Y.Z.tar.gz
   
   # For version v3.3.5 when this tutorial is published
   $ wget https://github.com/coreruleset/coreruleset/archive/refs/tags/v3.3.5.tar.gz
   
   # Extract the downloaded file
   $ tar -xvzf coreruleset-3.3.5.tar.gz
   ```

1. Cree el `modsec/crs` carpetas dentro de `dispatcher/src/conf.d/` AEM en el código de su proyecto de. Por ejemplo, en la copia local de [AEM Proyecto de sitios WKND de WKND](https://github.com/adobe/aem-guides-wknd).

   ![AEM Carpeta CRS dentro del código de proyecto de la: ModSecurity](assets/modsecurity-crs/crs-folder-in-aem-dispatcher-module.png){width="200" zoomable="yes"}

1. Copie el `coreruleset-X.Y.Z/rules` del paquete de versión de CRS descargado en la carpeta `dispatcher/src/conf.d/modsec/crs` carpeta.
1. Copie el `coreruleset-X.Y.Z/crs-setup.conf.example` del paquete de versión de CRS descargado en el `dispatcher/src/conf.d/modsec/crs` y cambie el nombre a `crs-setup.conf`.
1. Deshabilite todas las reglas CRS copiadas del `dispatcher/src/conf.d/modsec/crs/rules` cambiándoles el nombre por `XXXX-XXX-XXX.conf.disabled`. Puede utilizar los siguientes comandos para cambiar el nombre de todos los archivos a la vez.

   ```shell
   # Go inside the newly created rules directory within the dispathcher module
   $ cd dispatcher/src/conf.d/modsec/crs/rules
   
   # Rename all '.conf' extension files to '.conf.disabled'
   $ for i in *.conf; do mv -- "$i" "$i.disabled"; done
   ```

   Consulte cambio de nombre de reglas CRS y archivo de configuración en el código del proyecto WKND.

   ![AEM Reglas CRS desactivadas dentro del código de proyecto de la: ModSecurity ](assets/modsecurity-crs/disabled-crs-rules.png){width="200" zoomable="yes"}

## Habilitar y configurar la regla de protección de denegación de servicio (DoS)

Para habilitar y configurar la regla de protección Denegación de servicio (DoS), siga los siguientes pasos:

1. Habilite la regla de protección DoS cambiando el nombre del `REQUEST-912-DOS-PROTECTION.conf.disabled` hasta `REQUEST-912-DOS-PROTECTION.conf` (o quite el `.disabled` de la extensión rulename) dentro de `dispatcher/src/conf.d/modsec/crs/rules` carpeta.
1. Configure la regla definiendo la variable  **DOS_COUNTER_THRESHOLD, DOS_BURST_TIME_SLICE, DOS_BLOCK_TIMEOUT** variables.
   1. Crear un `crs-setup.custom.conf` dentro del archivo `dispatcher/src/conf.d/modsec/crs` carpeta.
   1. Añada el siguiente fragmento de regla al archivo recién creado.

   ```
   # The Denial of Service (DoS) protection against clients making requests too quickly.
   # When a client is making more than 25 requests (excluding static files) within
   # 60 seconds, this is considered a 'burst'. After two bursts, the client is
   # blocked for 600 seconds.
   SecAction \
       "id:900700,\
       phase:1,\
       nolog,\
       pass,\
       t:none,\
       setvar:'tx.dos_burst_time_slice=60',\
       setvar:'tx.dos_counter_threshold=25',\
       setvar:'tx.dos_block_timeout=600'"    
   ```

En esta configuración de regla de ejemplo, **DOS_COUNTER_THRESHOLD** es 25, **DOS_BURST_TIME_SLICE** es de 60 segundos, y **DOS_BLOCK_TIMEOUT** tiempo de espera: 600 segundos. Esta configuración identifica más de dos incidencias de 25 solicitudes, excluidos los archivos estáticos, en un plazo de 60 segundos que cumplen los requisitos de ataque DoS, lo que provoca que el cliente solicitante se bloquee durante 600 segundos (o 10 minutos).

>[!WARNING]
>
>Para definir los valores adecuados para sus necesidades, colabore con su equipo de seguridad web.

## Inicializar el CRS

Para inicializar CRS, quitar los falsos positivos comunes y agregar excepciones locales para el sitio, siga los siguientes pasos:

1. Para inicializar el CRS, elimine `.disabled` desde el **REQUEST-901-INITIALIZATION** archivo. En otras palabras, cambie el nombre de `REQUEST-901-INITIALIZATION.conf.disabled` archivo a `REQUEST-901-INITIALIZATION.conf`.
1. Para eliminar los falsos positivos comunes como el ping de IP local (127.0.0.1), elimine `.disabled` desde el **REQUEST-905-COMMON-EXCEPTIONS** archivo.
1. AEM Para agregar excepciones locales como la plataforma de la o las rutas específicas del sitio, cambie el nombre de `REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf.example` hasta `REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf`
   1. AEM Añada excepciones de ruta específicas de la plataforma al archivo cuyo nombre se acaba de cambiar.

   ```
   ########################################################
   # AEM as a Cloud Service exclusions                    #
   ########################################################
   # Ignoring AEM-CS Specific internal and reserved paths
   
   SecRule REQUEST_URI "@beginsWith /systemready" \
       "id:1010,\
       phase:1,\
       pass,\
       nolog,\
       ctl:ruleEngine=Off"    
   
   SecRule REQUEST_URI "@beginsWith /system/probes" \
       "id:1011,\
       phase:1,\
       pass,\
       nolog,\
       ctl:ruleEngine=Off"
   
   SecRule REQUEST_URI "@beginsWith /gitinit-status" \
       "id:1012,\
       phase:1,\
       pass,\
       nolog,\
       ctl:ruleEngine=Off"
   
   ########################################################
   # ADD YOUR SITE related exclusions                     #
   ########################################################
   ...
   ```

1. Además, quite el `.disabled` de **REQUEST-910-IP-REPUTATION.conf.disabled** para comprobar el bloque de reputación de IP y `REQUEST-949-BLOCKING-EVALUATION.conf.disabled` para la comprobación de puntuación de anomalías.

>[!TIP]
>
>AEM AEM Al configurar en la versión 6.5 de, asegúrese de reemplazar las rutas anteriores por rutas AMS o locales respectivas que verifiquen el estado de las rutas de acceso (es decir, rutas de latidos).

## Añadir la configuración de Apache ModSecurity

Para habilitar ModSecurity (también conocido como `mod_security` módulo Apache), siga los siguientes pasos:

1. Crear `modsecurity.conf` en `dispatcher/src/conf.d/modsec/modsecurity.conf` con las siguientes configuraciones clave.

   ```
   # Include the baseline crs setup
   Include conf.d/modsec/crs/crs-setup.conf
   
   # Include your customizations to crs setup if exist
   IncludeOptional conf.d/modsec/crs/crs-setup.custom.conf
   
   # Select all available CRS rules:
   #Include conf.d/modsec/crs/rules/*.conf
   
   # Or alternatively list only specific ones you want to enable e.g.
   Include conf.d/modsec/crs/rules/REQUEST-900-EXCLUSION-RULES-BEFORE-CRS.conf
   Include conf.d/modsec/crs/rules/REQUEST-901-INITIALIZATION.conf
   Include conf.d/modsec/crs/rules/REQUEST-905-COMMON-EXCEPTIONS.conf
   Include conf.d/modsec/crs/rules/REQUEST-910-IP-REPUTATION.conf
   Include conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf
   Include conf.d/modsec/crs/rules/REQUEST-949-BLOCKING-EVALUATION.conf
   
   # Start initially with engine off, then switch to detection and observe, and when sure enable engine actions
   #SecRuleEngine Off
   #SecRuleEngine DetectionOnly
   SecRuleEngine On
   
   # Remember to use relative path for logs:
   SecDebugLog logs/httpd_mod_security_debug.log
   
   # Start with low debug level
   SecDebugLogLevel 0
   #SecDebugLogLevel 1
   
   # Start without auditing
   SecAuditEngine Off
   #SecAuditEngine RelevantOnly
   #SecAuditEngine On
   
   # Tune audit accordingly:
   SecAuditLogRelevantStatus "^(?:5|4(?!04))"
   SecAuditLogParts ABIJDEFHZ
   SecAuditLogType Serial
   
   # Remember to use relative path for logs:
   SecAuditLog logs/httpd_mod_security_audit.log
   
   # You might still use /tmp for temporary/work files:
   SecTmpDir /tmp
   SecDataDir /tmp
   ```

1. Seleccione el `.vhost` AEM desde el módulo de Dispatcher del proyecto de la `dispatcher/src/conf.d/available_vhosts`, por ejemplo, `wknd.vhost`, agregue la siguiente entrada fuera de `<VirtualHost>` Bloque.

   ```
   # Enable the ModSecurity and OWASP CRS
   <IfModule mod_security2.c>
       Include conf.d/modsec/modsecurity.conf
   </IfModule>
   
   ...
   
   <VirtualHost *:80>
       ServerName    "publish"
       ...
   </VirtualHost>
   ```

Todo lo anterior _ModSecurity CRS_ y _PROTECCIÓN DOS_ AEM Las configuraciones de están disponibles en el proyecto de sitios WKND de la [tutorial/enable-modsecurity-crs-dos-protection](https://github.com/adobe/aem-guides-wknd/tree/tutorial/enable-modsecurity-crs-dos-protection) para su revisión.

### Validar configuración de Dispatcher

AEM Cuando trabaje con as a Cloud Service, antes de implementar su _Configuración de Dispatcher_ cambia, se recomienda validarlos localmente mediante `validate` secuencia de comandos del [AEM Herramientas de Dispatcher del SDK de la](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html?lang=es).

```
# Go inside Dispatcher SDK 'bin' directory
$ cd <YOUR-AEM-SDK-DIR>/<DISPATCHER-SDK-DIR>/bin

# Validate the updated Dispatcher configurations
$ ./validate.sh <YOUR-AEM-PROJECT-CODE-DIR>/dispatcher/src
```

## Implementación de

Implemente las configuraciones de Dispatcher validadas localmente mediante Cloud Manager [Nivel web](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/configuring-production-pipelines.html?#web-tier-config) o [Pila completa](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/configuring-production-pipelines.html?#full-stack-code) canalización. También puede utilizar la variable [Entorno de desarrollo rápido](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/overview.html) para obtener un tiempo de respuesta más rápido.

## Verificar

Para verificar la protección del DoS, en este ejemplo, enviemos más de 50 solicitudes (25 umbrales de solicitud por dos incidencias) en un lapso de 60 segundos. AEM Sin embargo, estas solicitudes deben pasar por el as a Cloud Service [incorporado](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=es) o cualquiera [otra CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?#point-to-point-CDN) al abrir el sitio web.

Una técnica para lograr el paso a través de CDN es añadir un parámetro de consulta con una variable **nuevo valor aleatorio en cada solicitud de página del sitio**.

Para almacenar en déclencheur un número mayor de solicitudes (50 o más) en un corto periodo (como 60 segundos), la variable Apache [JMeter](https://jmeter.apache.org/) o [Herramienta de referencia o pestaña](https://httpd.apache.org/docs/2.4/programs/ab.html) se puede utilizar.

### Simulación de un ataque DoS mediante un script JMeter

Para simular un ataque DoS utilizando JMeter, siga los siguientes pasos:

1. [Descargar Apache JMeter](https://jmeter.apache.org/download_jmeter.cgi) y [instalar](https://jmeter.apache.org/usermanual/get-started.html#install) it localmente
1. [Ejecutar](https://jmeter.apache.org/usermanual/get-started.html#running) lo utiliza localmente mediante el `jmeter` desde el `<JMETER-INSTALL-DIR>/bin` directorio.
1. Abrir la muestra [WKND-DoS-Attack-Simulation-Test](assets/modsecurity-crs/WKND-DoS-Attack-Simulation-Test.jmx) Script JMX en JMeter utilizando **Abrir** menú de herramientas.

   ![Abra el script de prueba JMX de ataque de WKND DoS de muestra: ModSecurity](assets/modsecurity-crs/open-wknd-dos-attack-jmx-test-script.png)

1. Actualice el **Nombre de servidor o IP** valor de campo en _Página principal_ y _Página de aventura_ AEM Muestreador de solicitudes HTTP que coincide con la dirección URL del entorno de prueba de. Revise otros detalles del script de ejemplo de JMeter.

   ![AEM Solicitud HTTP JMetere del nombre del servidor de - ModSecurity](assets/modsecurity-crs/aem-server-name-http-request.png)

1. Ejecute la secuencia de comandos pulsando la tecla **Inicio** del menú de herramientas. La secuencia de comandos envía 50 solicitudes HTTP (5 usuarios y 10 recuentos de bucle) contra el sitio WKND _Página principal_ y _Página de aventura_. Por lo tanto, un total de 100 solicitudes a archivos no estáticos, califica el ataque DoS por **PROTECCIÓN DOS** Configuración personalizada de regla CRS.

   ![Ejecutar script de JMeter: ModSecurity](assets/modsecurity-crs/execute-jmeter-script.png)

1. El **Ver resultados en la tabla** El oyente JMeter muestra **Error** estado de respuesta para el número de solicitud ~ 53 y posterior.

   ![Respuesta fallida en la vista de resultados en la tabla JMeter - ModSecurity](assets/modsecurity-crs/failed-response-jmeter.png)

1. El **503 Código de respuesta HTTP** se devuelve para las solicitudes fallidas, puede ver los detalles con la variable **Ver árbol de resultados** JMeter Listener.

   ![Respuesta 503 JMeter - ModSecurity](assets/modsecurity-crs/503-response-jmeter.png)

### Revisar registros

La configuración del registrador de ModSecurity registra los detalles del incidente de ataque DoS. Para ver los detalles, siga los siguientes pasos:

1. Descargue y abra `httpderror` archivo de registro de **Publicar Dispatcher**.
1. Buscar palabra `burst` en el archivo de registro, para ver el **error** líneas

   ```
   Tue Aug 15 15:19:40.229262 2023 [security2:error] [pid 308:tid 140200050567992] [cm-p46652-e1167810-aem-publish-85df5d9954-bzvbs] [client 192.150.10.209] ModSecurity: Warning. Operator GE matched 2 at IP:dos_burst_counter. [file "/etc/httpd/conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf"] [line "265"] [id "912170"] [msg "Potential Denial of Service (DoS) Attack from 192.150.10.209 - # of Request Bursts: 2"] [ver "OWASP_CRS/3.3.5"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "paranoia-level/1"] [tag "attack-dos"] [tag "OWASP_CRS"] [tag "capec/1000/210/227/469"] [hostname "publish-p46652-e1167810.adobeaemcloud.com"] [uri "/content/wknd/us/en/adventures.html"] [unique_id "ZNuXi9ft_9sa85dovgTN5gAAANI"]
   
   ...
   
   Tue Aug 15 15:19:40.515237 2023 [security2:error] [pid 309:tid 140200051428152] [cm-p46652-e1167810-aem-publish-85df5d9954-bzvbs] [client 192.150.10.209] ModSecurity: Access denied with connection close (phase 1). Operator EQ matched 0 at IP. [file "/etc/httpd/conf.d/modsec/crs/rules/REQUEST-912-DOS-PROTECTION.conf"] [line "120"] [id "912120"] [msg "Denial of Service (DoS) attack identified from 192.150.10.209 (1 hits since last alert)"] [ver "OWASP_CRS/3.3.5"] [tag "application-multi"] [tag "language-multi"] [tag "platform-multi"] [tag "paranoia-level/1"] [tag "attack-dos"] [tag "OWASP_CRS"] [tag "capec/1000/210/227/469"] [hostname "publish-p46652-e1167810.adobeaemcloud.com"] [uri "/us/en.html"] [unique_id "ZNuXjAN7ZtmIYHGpDEkmmwAAAQw"]
   ```

1. Revise los detalles como _dirección IP del cliente_, acción, mensaje de error y detalles de solicitud.

## Impacto en el rendimiento de ModSecurity

La activación del ModSecurity y las reglas asociadas tiene algunas implicaciones de rendimiento, por lo que tenga en cuenta qué reglas son necesarias, redundantes y omitidas. Asociarse con sus expertos en seguridad web para habilitar y personalizar las reglas de CRS.

### Reglas adicionales

Este tutorial solo habilita y personaliza la variable **PROTECCIÓN DOS** Regla CRS con fines de demostración. Se recomienda asociarse con expertos en seguridad web para comprender, revisar y configurar las reglas adecuadas.
