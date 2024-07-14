---
title: AEM Configuración de OKTA con la
description: Comprender los distintos ajustes de configuración para utilizar el inicio de sesión único mediante okta
feature: Adaptive Forms
version: 6.5
topic: Administration
role: Admin
level: Experienced
exl-id: 85c9b51e-92bb-4376-8684-57c9c3204b2f
last-substantial-update: 2021-06-09T00:00:00Z
duration: 153
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '733'
ht-degree: 0%

---

# AEM Autenticar con OKTA para crear un

El primer paso es configurar la aplicación en el portal OKTA. Una vez que el administrador de OKTA apruebe la aplicación, tendrá acceso al certificado IdP y a la URL de inicio de sesión único. A continuación se indican las opciones que se suelen utilizar para registrar una nueva aplicación.

* **Nombre de aplicación:** Este es su nombre de aplicación. Asegúrese de asignar un nombre único a la aplicación.
* AEM **Destinatario de SAML:** Después de la autenticación de OKTA, esta es la dirección URL que se visita en su instancia con la respuesta de SAML. El controlador de autenticación SAML normalmente intercepta todas las direcciones URL con / saml_login, pero sería preferible anexarlo después de la raíz de la aplicación.
* **Audiencia SAML**: Esta es la dirección URL de dominio de su aplicación. No utilice protocol(http o https) en la dirección URL del dominio.
* **ID de nombre de SAML:** Seleccione Correo electrónico de la lista desplegable.
* **Entorno**: elija el entorno apropiado.
* **Atributos**: estos son los atributos que obtiene sobre el usuario en la respuesta de SAML. Especifíquelas según sus necesidades.


![okta-application](assets/okta-app-settings-blurred.PNG)


## AEM Añadir el certificado OKTA (IdP) al almacén de confianza de la

AEM AEM Dado que las afirmaciones de SAML están cifradas, es necesario añadir el certificado IdP (OKTA) al almacén de confianza de, para permitir la comunicación segura entre OKTA y los.
[Inicializar el almacén de confianza](http://localhost:4502/libs/granite/security/content/truststore.html), si no se ha inicializado ya.
Recuerde la contraseña del almacén de confianza. Necesitaremos usar esta contraseña más adelante en este proceso.

* Vaya a [Almacén de confianza global](http://localhost:4502/libs/granite/security/content/truststore.html).
* Haga clic en &quot;Agregar certificado del archivo CER&quot;. Añada el certificado IdP proporcionado por OKTA y haga clic en enviar.

  >[!NOTE]
  >
  >No asigne el certificado a ningún usuario

Al agregar el certificado al almacén de confianza, debe obtener el alias de certificado como se muestra en la captura de pantalla siguiente. El nombre del alias podría ser diferente en su caso.

![Alias de certificado](assets/cert-alias.PNG)

**Tome nota del alias del certificado. Lo necesita en los pasos posteriores.**

### Configurar el controlador de autenticación SAML

Vaya a [configMgr](http://localhost:4502/system/console/configMgr).
Busque y abra &quot;Controlador de autenticación de Adobe Granite SAML 2.0&quot;.
Proporcione las siguientes propiedades como se especifica a continuación
Las siguientes son las propiedades clave que deben especificarse:

* **ruta**: es la ruta en la que se activa el controlador de autenticación
* **Dirección URL de IdP**:Dirección URL de IdP proporcionada por OKTA
* AEM **Alias de certificado IDP**: Este es el alias que obtuvo cuando agregó el certificado IdP al almacén de confianza de la
* AEM **Id. de entidad de proveedor de servicios**:Este es el nombre de su servidor de la red de servicios de la red de servicio de la red
* **Contraseña del almacén de claves**: Esta es la contraseña del almacén de confianza que utilizó
* **Redirección predeterminada**: Esta es la dirección URL a la que se redirigirá si la autenticación se realiza correctamente
* **Atributo UserID**:uid
* **Usar cifrado**:false
* **Crear automáticamente usuarios de CRX**:true
* **Agregar a grupos**:true
* **Grupos predeterminados**:oktausers(Este es el grupo al que se agregan los usuarios. AEM Puede proporcionar cualquier grupo existente dentro de la lista de grupos de trabajo
* **NamedIDPolicy**: especifica restricciones en el identificador de nombre que se utilizará para representar al asunto solicitado. Copie y pegue la siguiente cadena resaltada **urn:oasis:names:tc:SAML:2.0:nameidformat:emailAddress**
* AEM **Atributos sincronizados**: estos son los atributos que se están almacenando desde la aserción SAML en el perfil de la

![controlador de autenticación saml](assets/saml-authentication-settings-blurred.PNG)

### Configurar el filtro de referente de Apache Sling

Vaya a [configMgr](http://localhost:4502/system/console/configMgr).
Busque y abra &quot;Filtro de referente de Apache Sling&quot;. Establezca las siguientes propiedades como se especifica a continuación:

* **Permitir vacío**: false
* **Permitir hosts**: nombre de host de IdP (esto es diferente en su caso)
* **Permitir regexp host**: nombre de host de IdP (esto es diferente en su caso)
Captura de pantalla de las propiedades del referente del filtro de referente de Sling

![filtro de referente](assets/okta-referrer.png)

#### Configurar el registro de depuración para la integración de OKTA

AEM AEM Al configurar la integración de OKTA en el, puede resultar útil revisar los registros de DEPURACIÓN para el controlador de autenticación SAML de la. AEM Para establecer el nivel de registro en DEPURACIÓN, cree una nueva configuración de Sling Logger a través de la consola web de OSGi de OSGi.

Recuerde quitar o deshabilitar este registrador en Fase y Producción para reducir el ruido de registro.

AEM AEM Al configurar la integración de OKTA en el, puede resultar útil revisar los registros de DEPURACIÓN para el controlador de autenticación SAML de la autenticación de la. AEM Para establecer el nivel de registro en DEPURACIÓN, cree una nueva configuración de Sling Logger a través de la consola web de OSGi de OSGi.
**Recuerde quitar o deshabilitar este registrador en Fase y Producción para reducir el ruido de registro.**
* Vaya a [configMgr](http://localhost:4502/system/console/configMgr)

* Busque y abra &quot;Configuración del registrador de Apache Sling&quot;
* Cree un registrador con la siguiente configuración:
   * **Nivel de registro**: Depurar
   * **Archivo de registro**: logs/saml.log
   * **Registrador**: com.adobe.granite.auth.saml
* Haga clic en Guardar para guardar la configuración.

#### Pruebe la configuración de OKTA

AEM Cierre la sesión de la instancia de. Intente acceder al vínculo. Debería ver el SSO de OKTA en acción.
