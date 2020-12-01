---
title: Configuración de OKTA con AEM
description: Comprender las distintas opciones de configuración para utilizar el inicio de sesión único mediante okta
feature: administration
topics: development, authentication, security
audience: developer
doc-type: tutorial
activity: setup
version: 6.5
translation-type: tm+mt
source-git-commit: 0b48ae445f4b32deeec08bcb68f805bf19992c9e
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 0%

---


# Autenticar en AEM Author mediante OKTA

El primer paso es configurar la aplicación en el portal OKTA. Una vez que el administrador de OKTA haya aprobado la aplicación, tendrá acceso al certificado de IdP y a la URL de inicio de sesión único. Los siguientes son los ajustes que se utilizan normalmente para registrar una nueva aplicación.

* **Nombre de la aplicación:** es el nombre de la aplicación. Asegúrese de asignar un nombre único a la aplicación.
* **DESTINATARIO SAML:** Después de la autenticación desde OKTA, esta es la dirección URL que se visitará en la instancia de AEM con la respuesta SAML. El controlador de autenticación SAML suele interceptar todas las direcciones URL con / saml_login, pero sería preferible anexarlas después de la raíz de la aplicación.
* **AUDIENCIA** SAML: Es la dirección URL del dominio de la aplicación. No utilice protocol(http o https) en la dirección URL del dominio.
* **ID de nombre de SAML:** seleccione Correo electrónico en la lista desplegable.
* **Entorno**: Elija el entorno adecuado.
* **Atributos**: Estos son los atributos que obtiene del usuario en la respuesta SAML. Especifíquelas según sus necesidades.


![okta-application](assets/okta-app-settings-blurred.PNG)


## Añadir el certificado OKTA (IdP) en el almacén de confianza de AEM

Dado que las afirmaciones de SAML están cifradas, necesitamos agregar el certificado de IdP (OKTA) al almacén de confianza de AEM, para permitir una comunicación segura entre OKTA y AEM.
[Inicialice el almacén](http://localhost:4502/libs/granite/security/content/truststore.html) de confianza, si no se ha inicializado ya.
Recuerde la contraseña del almacén de confianza. Necesitaremos utilizar esta contraseña más adelante en este proceso.

* Vaya a [Almacén de confianza global](http://localhost:4502/libs/granite/security/content/truststore.html).
* Haga clic en &quot;Añadir certificado del archivo CER&quot;. Añada el certificado de IdP proporcionado por OKTA y haga clic en enviar.

   >[!NOTE]
   >
   >No asigne el certificado a ningún usuario

Al agregar el certificado al almacén de confianza, debe obtener el alias de certificado como se muestra en la captura de pantalla siguiente. El nombre del alias puede ser diferente en su caso.

![Alias de certificado](assets/cert-alias.PNG)

**Anote el alias del certificado. Necesitará esto en los pasos posteriores.**

### Configuración del controlador de autenticación SAML

Vaya a [configMgr](http://localhost:4502/system/console/configMgr).
Busque y abra &quot;Adobe Granite SAML 2.0 Authentication Handler&quot;.
Proporcione las siguientes propiedades como se especifica a continuación
A continuación se indican las propiedades clave que deben especificarse:

* **path** : es la ruta en la que se activará el controlador de autenticación
* **Dirección URL** de IdP: esta es su dirección URL de IdP proporcionada por OKTA
* **Alias** de certificado de IDP:Este es el alias que obtuvo al agregar el certificado de IdP a AEM almacén de confianza
* **Id** de entidad de proveedor de servicio:Es el nombre del servidor AEM
* **Contraseña del almacén** de claves:Ésta es la contraseña del almacén de confianza que ha utilizado
* **Redirección** predeterminada:es la dirección URL a la que se redirige cuando la autenticación se realiza correctamente
* **Atributo** UserID:uid
* **Usar codificación**:false
* **Crear automáticamente usuarios** de CRX:true
* **Añadir a grupos**:true
* **Grupos** predeterminados:oktausers(Es el grupo al que se agregarán los usuarios. Puede proporcionar cualquier grupo existente dentro de AEM)
* **NamedIDPolicy**: Especifica restricciones en el identificador de nombre que se usará para representar el asunto solicitado. Copie y pegue la siguiente cadena resaltada **urn:oasis:names:tc:SAML:2.0:nameidformat:emailAddress**
* **Atributos**  sincronizados: son los atributos que se almacenan desde la afirmación SAML en AEM perfil

![saml-authentication-handler](assets/saml-authentication-settings-blurred.PNG)

### Configurar el filtro de Remitente del reenvío Sling de Apache

Vaya a [configMgr](http://localhost:4502/system/console/configMgr).
Busque y abra &quot;Apache Sling Remitente del reenvío Filter&quot;.Establezca las siguientes propiedades como se especifica a continuación:

* **Permitir vacío**: true
* **Permitir hosts**: Nombre de host de IdP (será diferente en su caso)
* **Permitir host** Regexp: Nombre de host de IdP (será diferente en su caso) La captura de pantalla de las propiedades del Remitente del reenvío del filtro de Remitente del reenvío Sling

![remitente del reenvío-filter](assets/sling-referrer-filter.PNG)

#### Configurar el registro DEBUG para la integración OKTA

Al configurar la integración de OKTA en AEM, puede resultar útil revisar los registros DEBUG para AEM controlador de autenticación SAML. Para establecer el nivel de registro en DEBUG, cree una nueva configuración de Sling Logger mediante la consola web OSGi de AEM.

Recuerde eliminar o deshabilitar este registrador en Stage y Production para reducir el ruido de registro.

Al configurar la integración de OKTA en AEM, puede resultar útil revisar los registros DEBUG para AEM controlador de autenticación SAML. Para establecer el nivel de registro en DEBUG, cree una nueva configuración de Sling Logger mediante la consola web OSGi de AEM.
**Recuerde eliminar o deshabilitar este registrador en Stage y Production para reducir el ruido de registro.**
* Vaya a [configMgr](http://localhost:4502/system/console/configMgr)

* Buscar y abrir &quot;Configuración del registrador de registros de Apache Sling&quot;
* Cree un registrador con la siguiente configuración:
   * **Nivel** de registro: Depurar
   * **Archivo** de registro: logs/saml.log
   * **Registrador**: com.adobe.granite.auth.saml
* Haga clic en Guardar para guardar la configuración



#### Probar la configuración de OKTA

Cierre la sesión de la instancia de AEM. Intente acceder al vínculo. Debería ver OKTA SSO en acción.
