---
title: SAML 2.0 en AEM como Cloud Service
description: Aprenda a configurar la autenticación SAML 2.0 en AEM as a Cloud Service Publish service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
jira: KT-9351
thumbnail: 343040.jpeg
last-substantial-update: 2024-05-15T00:00:00Z
exl-id: 461dcdda-8797-4a37-a0c7-efa7b3f1e23e
duration: 2200
source-git-commit: 2ed303e316577363f6d1c265ef7f9cd6d81491d8
workflow-type: tm+mt
source-wordcount: '4277'
ht-degree: 1%

---

# Autenticación SAML 2.0{#saml-2-0-authentication}

Aprenda a configurar y autenticar usuarios finales (no autores de AEM) en un IDP compatible con SAML 2.0 de su elección.

## ¿Qué es SAML para AEM as a Cloud Service?

La integración de SAML 2.0 con AEM Publish (o Vista previa) permite a los usuarios finales de una experiencia web basada en AEM autenticarse ante un IDP (proveedor de identidad) no Adobe Systems y acceder a AEM como usuario autorizado y designado.

|                       | AEM Author | AEM Publish |
|-----------------------|:----------:|:-----------:|
| Compatibilidad con SAML 2.0 | ✘ | ✔ |

+++ Comprensión del flujo de SAML 2.0 con AEM

El flujo típico de una integración de AEM Publish SAML es el siguiente:

1. El usuario establece un solicitud para AEM Publish el indica que se requiere la autenticación.
   + El usuario solicita un recurso protegido por CUGs/ACL.
   + El usuario solicita un recurso que está sujeto a un requisito Authentication.
   + El usuario sigue un vincular al punto final del inicio de sesión del AEM (es decir, `/system/sling/login`) que solicita explícitamente la acción inicio de sesión.
1. AEM realiza una AuthnRequest al IDP, solicitando al IDP que inicio proceso de autenticación.
1. El usuario se autentica en IDP.
   + El IDP solicita credenciales al usuario.
   + El usuario ya se ha autenticado con el IDP y no tiene que proporcionar más credenciales.
1. IDP genera una afirmación de SAML que contiene los datos del usuario y la firma utilizando el certificado privado del IDP.
1. IDP envía la afirmación de SAML a través de HTTP POST, a través del explorador web del usuario (RESPECTIVE_PROTECTED_PATH/saml_login), a AEM Publish.
1. AEM Publish recibe la afirmación de SAML y valida la integridad y autenticidad de la afirmación de SAML utilizando el certificado público IDP.
1. AEM Publish administra el registro de usuario de AEM en función de la configuración OSGi de SAML 2.0 y el contenido de la aserción SAML.
   + Crea usuario
   + Sincroniza usuario atributos
   + Actualizaciones AEM grupo de usuarios abono
1. AEM Publish establece el cookie de AEM `login-token` en la respuesta HTTP, que se utiliza para autenticar solicitudes posteriores a AEM Publish.
1. AEM Publish redirige al usuario a la dirección URL en AEM Publish según se especifica en la cookie `saml_request_path`.

+++

## Introducción a la configuración

>[!VIDEO](https://video.tv.adobe.com/v/343040?quality=12&learn=on)

Este vídeo explica cómo configurar la integración de SAML 2.0 con el servicio de publicación de AEM as a Cloud Service y cómo utilizar Okta como IDP.

## Requisitos previos

Se requiere lo siguiente al configurar la autenticación SAML 2.0:

+ Acceso de Deployment Manager a Cloud Manager
+ AEM acceso de administrador a AEM como entorno de Cloud Service
+ Acceso de administrador a IDP
+ Opcionalmente, el acceso a un par de claves pública/privada utilizado para cifrar cargas útiles SAML
+ AEM Sites páginas (o árboles Página), publicadas en AEM Publish y [protegidas por grupos cerrados de usuarios (CUG)](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/sites/authoring/sites-console/page-properties#permissions)

SAML 2.0 solo se admite para autenticar usos de AEM Publish o Vista previa. Para administrar la autenticación de AEM Autor utilizando IDP, [integre el IDP con Adobe Systems IMS](https://helpx.adobe.com/es/enterprise/using/set-up-identity.html).


## Instalación del certificado público de IDP en AEM

El certificado público del IDP se añade al almacén de confianza global de AEM y se utiliza para validar que la afirmación SAML enviada por el IDP sea válida.

+++Flujo de firma de aserción SAML

![SAML 2.0: firma de afirmación de SAML de IDP](./assets/saml-2-0/idp-signing-diagram.png)

1. El usuario se autentica en IDP.
1. IDP genera una aserción SAML que contiene los datos del usuario.
1. IDP firma la afirmación SAML utilizando el certificado privado del IDP.
1. IDP inicia una POST HTTP lado del cliente para AEM el extremo SAML (`.../saml_login`) del Publish que incluye la aserción SAML firmada.
1. AEM Publish recibe el POST HTTP que contiene la aserción SAML firmada, puede validar la firma mediante el certificado público de IDP.

+++

![añadir el certificado público de IDP al almacén de confianza global](./assets/saml-2-0/global-trust-store.png)

1. Obtenga el archivo de __certificado público__ del IDP. Este certificado permite a AEM validar la afirmación de SAML proporcionada a AEM por el IDP.

   El certificado está en formato PEM y debe tener un aspecto similar al siguiente:

   ```
   -----BEGIN CERTIFICATE-----
   MIIC4jCBAcoCCQC33wnybT5QZDANBgkqhkiG9w0BAQsFADAyMQswCQYDVQQGEwJV
   ...
   m0eo2USlSRTVl7QHRTuiuSThHpLKQQ==
   -----END CERTIFICATE-----
   ```

1. Inicie sesión en AEM Author como administrador de AEM.
1. Desplácese hasta __Herramientas almacén de confianza__ > > Security.
1. Crear o abra Global Trust Store. Si crea un almacén de confianza global, tienda el contraseña en un lugar seguro.
1. Expanda __añadir certificado desde el archivo__ CER.
1. Seleccione __Seleccionar archivo de certificado__ y cargue el archivo de certificado proporcionado por el IDP.
1. Deje __Asignar certificado al usuario__ en blanco.
1. Seleccione __Enviar__.
1. El certificado recién agregado aparece encima de la sección __Agregar certificado del archivo CRT__.
1. Anote el __alias__, ya que este valor se utiliza en la [configuración](#saml-2-0-authentication-handler-osgi-configuration) de SAML 2.0 Authentication Handler OSGi.
1. Seleccione __Guardar y cerrar__.

El almacén de confianza global se configura con el certificado público del IDP en AEM Autor, pero como SAML solo se usa en AEM Publish, el almacén de confianza global debe replicarse en AEM Publish para que el certificado público de IDP sea accesible allí.

![Replicar el almacén de confianza global para AEM Publish](./assets/saml-2-0/global-trust-store-replicate.png)

1. Vaya a __Herramientas > Implementación > Paquetes__.
1. Creación de un paquete
   + Nombre del paquete: `Global Trust Store`
   + Versión: `1.0.0`
   + Grupo: `com.your.company`
1. Editar el nuevo __paquete Global Trust Store__ .
1. Seleccione el __pestaña Filtros__ y agregue un filtro para la ruta `/etc/truststore`raíz.
1. Seleccione __Listo__ y luego __Guardar__.
1. Seleccione el __botón de compilación__ para el paquete Global __Trust Store__ .
1. Una vez generado, seleccione Más > Replicar __para activar el almacén de confianza global nodo (__) para AEM Publish. ____ `/etc/truststore`

## Crear almacén de claves del servicio de autenticación{#authentication-service-keystore}

_Es necesario crear un almacén de claves para el servicio de autenticación cuando el Propiedad [&#x200B; de configuración OSGi del `handleLogout`gestor de autenticación SAML 2.0 está establecido en `true`](#saml-20-authenticationsaml-2-0-authentication) o cuando [se requiere el cifrado de firma AuthnRequest/aserción SAML](#install-aem-public-private-key-pair)_

1. Inicie sesión en AEM Author como administrador de AEM para cargar la clave privada.
1. Vaya a __Herramientas > Seguridad > Usuarios__, seleccione el usuario __authentication-service__ y seleccione __Propiedades__ en la barra de acciones superior.
1. Seleccione la pestaña __Keystore__.
1. Cree o abra el repositorio de claves. Si crea un almacén de claves, mantenga la contraseña a salvo.
   + Un almacén de claves [público/privado está instalado en este almacén de claves](#install-aem-public-private-key-pair) solo si se requiere cifrado de firma AuthnRequest/aserción SAML.
   + Si esta integración de SAML admite el cierre de sesión, pero no la firma AuthnRequest/aserción SAML, basta con un almacén de claves vacío.
1. Seleccione __Guardar y cerrar__.
1. Crear un paquete que contiene el usuario actualizado __del servicio__ de autenticación.

   _Use la siguiente solución temporal utilizando paquetes :_

   1. Desplácese hasta Herramientas __> paquetes de > implementación__.
   1. Crear un paquete
      + Nombre del paquete: `Authentication Service`
      + Versión: `1.0.0`
      + Grupo: `com.your.company`
   1. Editar el nuevo __paquete Authentication Service Key Store__ .
   1. Seleccione el __pestaña Filtros__ y agregue un filtro para la ruta `/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore`raíz.
      + Se `<AUTHENTICATION SERVICE UUID>` puede encontrar navegando a __Herramientas > Security > Users y seleccionando__ usuario __de servicio__ de autenticación. El UUID es la última parte del URL.
   1. Seleccione __Listo__ y luego __Guardar__.
   1. Seleccione el botón __Generar__ para el paquete de __Almacenamiento de claves del servicio de autenticación__.
   1. Una vez compilado, seleccione __Más__ > __Replicar__ para activar el almacén de claves del servicio de autenticación en AEM Publish.

## Instalación del par de claves pública y privada de AEM{#install-aem-public-private-key-pair}

_La instalación del par de claves pública y privada de AEM es opcional_

AEM Publish se puede configurar para firmar AuthnRequests (a IDP) y cifrar aserciones SAML (para AEM). Esto se logra proporcionando una clave privada a AEM Publish y haciendo coincidir la clave pública con el IDP.

+++ Comprender el flujo de firma de AuthnRequest (opcional)

La AuthnRequest (la solicitud al IDP desde AEM Publish que inicia el proceso de inicio de sesión) puede ser firmada por AEM Publish. Para ello, AEM Publish firma la AuthnRequest utilizando la clave privada, que el IDP valida la firma utilizando la clave pública. Esto garantiza al IDP que AuthnRequest fue iniciado y solicitado por AEM Publish, y no por un tercero malintencionado.

![SAML 2.0: firma de SP AuthnRequest](./assets/saml-2-0/sp-authnrequest-signing-diagram.png)

1. El usuario establece un petición HTTP para AEM Publish que da como resultado una solicitud de autenticación SAML para el IDP.
1. AEM Publish genera el solicitud SAML para enviar al IDP.
1. AEM Publish firma el solicitud SAML con la clave privada de AEM.
1. AEM Publish inicia AuthnRequest, un redirección lado del cliente HTTP al IDP que contiene el solicitud SAML firmado.
1. IDP recibe la AuthnRequest y valida la firma utilizando la clave pública de AEM, garantizando AEM Publish inició la AuthnRequest.
1. A continuación, AEM Publish valida la integridad y autenticidad de la afirmación de SAML descifrada mediante el certificado público IDP.

+++

+++ Comprender el flujo de cifrado de afirmación de SAML (opcional)

Toda la comunicación HTTP entre IDP y AEM Publish debe realizarse a través de HTTPS y, por lo tanto, ser segura de forma predeterminada. Sin embargo, según sea necesario, las afirmaciones de SAML pueden cifrarse en el caso de que se requiera confidencialidad adicional además de la proporcionada por HTTPS. Para ello, el IDP cifra los datos de aserción SAML utilizando la clave privada y AEM Publish descifra la aserción SAML utilizando la clave privada.

![SAML 2.0 - SP Cifrado de aserción SAML](./assets/saml-2-0/sp-samlrequest-encryption-diagram.png)

1. El usuario se autentica en IDP.
1. IDP genera una aserción SAML que contiene los datos del usuario y la firma con el certificado privado del IDP.
1. A continuación, IDP cifra la afirmación de SAML con la clave pública de AEM, que requiere la clave privada de AEM para descifrar.
1. La afirmación de SAML cifrada se envía a AEM Publish a través del explorador web del usuario.
1. AEM Publish recibe la afirmación de SAML y la descifra utilizando la clave privada de AEM.
1. IDP solicita al usuario que se autentique.

+++

Tanto la firma AuthnRequest como el cifrado de aserción SAML son opcionales, sin embargo, ambos están habilitados mediante el Propiedad [de configuración OSGi del controlador de `useEncryption`](#saml-20-authenticationsaml-2-0-authentication)autenticación SAML 2.0, lo que significa que se pueden usar ambos o ninguno.

![AEM clave de servicio de autenticación tienda](./assets/saml-2-0/authentication-service-key-store.png)

1. Obtenga la clave pública, la clave privada (PKCS#8 en DER formato) y el archivo de cadena de certificados (puede ser la clave pública) utilizados para firmar AuthnRequest y cifre la afirmación SAML. Las claves suelen ser proporcionadas por el equipo de seguridad de la organización de TI.

   + Se puede generar un par de claves autofirmadas con __openssl__:

   ```
   $ openssl req -x509 -sha256 -days 365 -newkey rsa:4096 -keyout aem-private.key -out aem-public.crt
   
   # Provide a password (keep in safe place), and other requested certificate information
   
   # Convert the keys to AEM's required format 
   $ openssl rsa -in aem-private.key -outform der -out aem-private.der
   $ openssl pkcs8 -topk8 -inform der -nocrypt -in aem-private.der -outform der -out aem-private-pkcs8.der
   ```

1. Cargue la clave pública al IDP.
   + Usando el `openssl` método anterior, la clave pública es el `aem-public.crt` archivo.
1. Inicie sesión en AEM Autor como administrador de AEM para cargar la clave privada.
1. Desplácese hasta __Herramientas almacén de confianza__ > seguridad >, seleccione __usuario de servicio__ de autenticación y seleccione __Propiedades__ en la barra de acciones superior.
1. Desplácese hasta __Herramientas > Usuarios > seguridad, seleccione__ usuario __de servicio__ de autenticación y seleccione __Propiedades__ en la barra de acciones superior.
1. Seleccione el pestaña de __Keystore__ .
1. Crear o abra el almacén de claves. Si crea un almacén de claves, mantenga la contraseña a salvo.
1. Seleccione __añadir clave privada del archivo__ DER y añada la clave privada y el archivo de cadena a AEM:
   + __Alias__: proporcione un nombre descriptivo, a menudo el nombre del IDP.
   + __Archivo__ de clave privada: Cargue el archivo de clave privada (PKCS#8 en DER formato).
      + Usando el `openssl` método anterior, este es el `aem-private-pkcs8.der` archivo
   + __Seleccionar archivo de cadena de certificados__: cargue el archivo de cadena que lo acompaña (puede ser la clave pública).
      + Utilizando el método `openssl` anterior, este es el archivo `aem-public.crt`
   + Seleccionar __Enviar__
1. El certificado recién agregado aparece encima de la sección __Agregar certificado del archivo CRT__.
   + Tome nota del __alias__, ya que se utiliza en la [configuración OSGi del controlador de autenticación SAML 2.0](#saml-20-authentication-handler-osgi-configuration)
1. Seleccione __Guardar y cerrar__.
1. Crear un paquete que contiene el usuario actualizado __del servicio__ de autenticación.

   _Use la siguiente solución temporal utilizando paquetes :_

   1. Desplácese hasta Herramientas __> paquetes de > implementación__.
   1. Crear un paquete
      + Nombre del paquete: `Authentication Service`
      + Versión: `1.0.0`
      + Grupo: `com.your.company`
   1. Edite el nuevo paquete __Almacenamiento de claves del servicio de autenticación__.
   1. Seleccione la ficha __Filtros__ y agregue un filtro para la ruta raíz `/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore`.
      + Se `<AUTHENTICATION SERVICE UUID>` puede encontrar navegando a __Herramientas > Security > Users y seleccionando__ usuario __de servicio__ de autenticación. El UUID es la última parte del URL.
   1. Seleccione Listo ____ y luego __Guardar__.
   1. Seleccione el botón __Generar__ para el paquete de __Almacenamiento de claves del servicio de autenticación__.
   1. Una vez compilado, seleccione __Más__ > __Replicar__ para activar el almacén de claves del servicio de autenticación en AEM Publish.

## Configurar el controlador de autenticación SAML 2.0{#configure-saml-2-0-authentication-handler}

La configuración de SAML de AEM se realiza a través de la configuración OSGi __Controlador de autenticación Adobe Granite SAML 2.0__.
La configuración es una configuración de fábrica de OSGi, lo que significa que un solo servicio de publicación de AEM as a Cloud Service puede tener varias configuraciones de SAML que cubran árboles de recursos discretos del repositorio; esto resulta útil para implementaciones de AEM de varios sitios.

+++ Glosario de configuración OSGi del Controlador de autenticación SAML 2.0

### Adobe Systems Configuración OSGi de Granite SAML 2.0 Authentication Handler{#configure-saml-2-0-authentication-handler-osgi-configuration}

|                                   | OSGi Propiedad | Necesario | Valor formato | Valor predeterminado | Descripción |
|-----------------------------------|-------------------------------|:--------:|:---------------------:|---------------------------|-------------|
| Rutas | `path` | ✔ | Matriz de cadenas | `/` | AEM rutas para las que se utiliza este controlador de autenticación. |
| URL de IDP | `idpUrl` | ✔ | Cadena |                           | Dirección URL de IDP a la que se envía la solicitud de autenticación SAML. |
| Alias de certificado IDP | `idpCertAlias` | ✔ | Cadena |                           | El alias del certificado IDP encontrado en el almacén de confianza global de la AEM |
| redirección HTTP de IDP | `idpHttpRedirect` | ✘ | Booleano | `false` | Indica si hay un redireccionamiento HTTP a la URL de IDP en lugar de enviar una AuthnRequest. Se estableció en `true` para la autenticación iniciada por IDP. |
| Identificador de IDP | `idpIdentifier` | ✘ | Cadena |                           | ID de IDP único para garantizar la exclusividad de usuarios y grupos de AEM. Si está vacío, se utiliza `serviceProviderEntityId` en su lugar. |
| URL de servicio de consumidor de afirmación | `assertionConsumerServiceURL` | ✘ | Cadena |                           | Atributo de URL `AssertionConsumerServiceURL` en AuthnRequest que especifica dónde se debe enviar el mensaje `<Response>` a AEM. |
| ID de entidad de SP | `serviceProviderEntityId` | ✔ | Cadena |                           | Identifica de forma exclusiva AEM al IDP; Por lo general, el AEM host nombre. |
| Encriptación SP | `useEncryption` | ✘ | Booleano | `true` | Indica si el IdP cifra las afirmaciones SAML. Requiere `spPrivateKeyAlias` y `keyStorePassword` debe configurarse. |
| Alias de clave privada de SP | `spPrivateKeyAlias` | ✘ | Cadena |                           | El alias de la clave privada en la `authentication-service` clave del usuario tienda. Obligatorio si `useEncryption` está establecido en `true`. |
| Clave SP tienda contraseña | `keyStorePassword` | ✘ | Cadena |                           | El contraseña de las claves del usuario &quot;servicio de autenticación&quot; tienda. Obligatorio si `useEncryption` está establecido en `true`. |
| redirección predeterminado | `defaultRedirectUrl` | ✘ | Cadena | `/` | La URL de redireccionamiento predeterminada después de la autenticación correcta. Puede ser relativo al host de AEM (por ejemplo, `/content/wknd/us/en/html`). |
| Atributo de ID de usuario | `userIDAttribute` | ✘ | Cadena | `uid` | Nombre del atributo de aserción SAML que contiene el ID de usuario del usuario de AEM. Dejar vacío para utilizar el `Subject:NameId`archivo . |
| Automático crear usuarios AEM | `createUser` | ✘ | Booleano | `true` | Indica si los usuarios de AEM se crean con una autenticación correcta. |
| Ruta intermedia del usuario de AEM | `userIntermediatePath` | ✘ | Cadena |                           | Al crear usuarios de AEM, este valor se usa como ruta intermedia (por ejemplo, `/home/users/<userIntermediatePath>/jane@wknd.com`). Requiere que `createUser` se establezca en `true`. |
| AEM atributos usuario | `synchronizeAttributes` | ✘ | Matriz de cadenas |                           | Lista de asignaciones de atributos SAML para tienda en el usuario AEM, en la formato `[ "saml-attribute-name=path/relative/to/user/node" ]` (por ejemplo, `[ "firstName=profile/givenName" ]`). Ver la [lista completa de atributos nativos de AEM](#aem-user-attributes). |
| Añadir usuario a grupos de AEM | `addGroupMemberships` | ✘ | Booleano | `true` | Indica si se agrega automáticamente un usuario de AEM a los grupos de usuarios de AEM después de la autenticación correcta. |
| atributo de pertenencia a grupo de AEM | `groupMembershipAttribute` | ✘ | Cadena | `groupMembership` | Nombre del atributo de aserción SAML que contiene una lista de AEM grupos de usuario a los que debe añadirse el usuario. Requiere `addGroupMemberships` que esté configurado en `true`. |
| Grupos de AEM predeterminados | `defaultGroups` | ✘ | Matriz de cadenas |                           | Un lista de AEM usuario grupos a los que siempre se agregan usuarios autenticados (por ejemplo, `[ "wknd-user" ]`). Requiere `addGroupMemberships` que esté configurado en `true`. |
| Formato NameIDPolicy | `nameIdFormat` | ✘ | Cadena | `urn:oasis:names:tc:SAML:2.0:nameid-format:transient` | El valor del parámetro NameIDPolicy formato que se va a enviar en el mensaje AuthnRequest. |
| Almacenar respuesta de SAML | `storeSAMLResponse` | ✘ | Booleano | `false` | Indica si el `samlResponse` valor está almacenado en el AEM `cq:User` nodo. |
| Controlar cierre de sesión | `handleLogout` | ✘ | Booleano | `false` | Indica si este controlador de autenticación SAML administra la solicitud de cierre de sesión. Requiere `logoutUrl` configurarse. |
| Cerrar sesión URL | `logoutUrl` | ✘ | Cadena |                           | IdP URL a dónde se envía el solicitud de cierre de sesión de SAML. Obligatorio si `handleLogout` está establecido en `true`. |
| Tolerancia del reloj | `clockTolerance` | ✘ | Entero | `60` | El reloj IDP y AEM (SP) sesga la tolerancia al validar las afirmaciones SAML. |
| Método de resumen | `digestMethod` | ✘ | Cadena | `http://www.w3.org/2001/04/xmlenc#sha256` | El algoritmo de resumen que utiliza el IDP al firmar un mensaje SAML. |
| Método de firma | `signatureMethod` | ✘ | Cadena | `http://www.w3.org/2001/04/xmldsig-more#rsa-sha256` | Algoritmo de firma que utiliza el IDP al firmar un mensaje SAML. |
| Tipo de sincronización de identidad | `identitySyncType` | ✘ | `default` o `idp` | `default` | No cambie `from` el valor predeterminado de AEM como un Cloud Service. |
| clasificación de servicio | `service.ranking` | ✘ | Entero | `5002` | Se prefieren configuraciones de clasificación superior para el mismo `path`. |

### AEM atributos usuario{#aem-user-attributes}

AEM utiliza los siguientes atributos de usuario, que se pueden completar mediante el `synchronizeAttributes` Propiedad en la configuración OSGi de Adobe Systems SAML 2.0 Authentication Handler.  Cualquier atributo de IDP se puede sincronizar con cualquier AEM Propiedad de usuario, sin embargo, la asignación a las propiedades de atributo de uso AEM (enumeradas a continuación) AEM permite utilizarlos de forma natural.

| Atributo de usuario | Ruta de Propiedad relativa desde `rep:User` nodo |
|--------------------------------|--------------------------|
| Título (por ejemplo, `Mrs`) | `profile/title` |
| Nombre dado (es decir, nombre) | `profile/givenName` |
| Apellido (es decir, apellido) | `profile/familyName` |
| Título de trabajo | `profile/jobTitle` |
| Dirección de correo electrónico | `profile/email` |
| Dirección | `profile/street` |
| Ciudad | `profile/city` |
| Código postal | `profile/postalCode` |
| País | `profile/country` |
| Número telefónico | `profile/phoneNumber` |
| Acerca de mí | `profile/aboutMe` |

+++

1. Cree un archivo de configuración OSGi en su proyecto en `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` y ábralo en su IDE.
   + Cambiar `/wknd-examples/` a `/<project name>/`
   + El identificador después de `~` en el nombre de archivo debe identificar de forma exclusiva esta configuración, por lo que puede ser el nombre del IDP, como `...~okta.cfg.json`. El valor debe ser alfanumérico y contener guiones.
1. Pegar el siguiente JSON en el `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json` archivo y actualice las `wknd` referencias según sea necesario.

   ```json
   {
       "path": [ "/content/wknd", "/content/dam/wknd" ], 
       "idpCertAlias": "$[env:SAML_IDP_CERT_ALIAS;default=certalias___1652125559800]",
       "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/exk4z55r44Jz9C6am5d7]",
       "idpUrl": "$[env:SAML_IDP_URL;default=https://dev-5511372.okta.com/app/dev-5511372_aemasacloudservice_1/exk4z55r44Jz9C6am5d7/sso/saml]",
       "serviceProviderEntityId": "$[env:SAML_AEM_ID;default=https://publish-p123-e456.adobeaemcloud.com]",
       "useEncryption": false,
       "createUser": true,
       "userIntermediatePath": "wknd/idp",
       "synchronizeAttributes":[
           "firstName=profile/givenName"
       ],
       "addGroupMemberships": true,
       "defaultGroups": [ 
           "wknd-users"
       ]
   }
   ```

1. Actualice los valores según lo requiera el proyecto. Consulte el glosario __de__ configuración de SAML 2.0 Authentication Handler OSGi anterior para obtener descripciones Propiedad de configuración. El `path` debe contener los árboles de contenido que están protegidos por grupos de usuarios cerrados (CUG) y requieren autenticación y este controlador de autenticación debe ser responsable de proteger.
1. Se recomienda, pero no es obligatorio, utilizar variables y secretos de entorno OSGi, cuando los valores pueden cambiar sin estar sincronizados con el ciclo de lanzamiento o cuando los valores difieren entre tipos de entorno/niveles de servicio similares. Los valores predeterminados se pueden establecer con la sintaxis `$[env:..;default=the-default-value]"`, como se muestra arriba.

Las configuraciones de OSGi por entorno (`config.publish.dev`, `config.publish.stage` y `config.publish.prod`) se pueden definir con atributos específicos si la configuración de SAML varía entre entornos.

### Usar cifrado

Al [cifrar AuthnRequest y la aserción SAML](#encrypting-the-authnrequest-and-saml-assertion), se requieren las siguientes propiedades: `useEncryption`, `spPrivateKeyAlias` y `keyStorePassword`. El `keyStorePassword` contiene una contraseña; por lo tanto, el valor no debe almacenarse en el archivo de configuración OSGi, sino insertarse usando [valores de configuración secretos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=es#secret-configuration-values)

+++Opcionalmente, actualice la configuración de OSGi para usar cifrado

1. Abra `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` en su IDE.
1. añadir las tres propiedades `useEncryption`, `spPrivateKeyAlias`, y `keyStorePassword` como se muestra a continuación.

   ```json
   {
   "path": [ "/content/wknd", "/content/dam/wknd" ], 
   "idpCertAlias": "$[env:SAML_IDP_CERT_ALIAS;default=certalias___1234567890]",
   "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/abcdef1235678]",
   "idpUrl": "$[env:SAML_IDP_URL;default=https://dev-5511372.okta.com/app/dev-123567890_aemasacloudservice_1/abcdef1235678/sso/saml]",
   "serviceProviderEntityId": "$[env:SAML_AEM_ID;default=https://publish-p123-e456.adobeaemcloud.com]",
   "useEncryption": true,
   "spPrivateKeyAlias": "$[env:SAML_AEM_KEYSTORE_ALIAS;default=aem-saml-encryption]",
   "keyStorePassword": "$[secret:SAML_AEM_KEYSTORE_PASSWORD]",
   "createUser": true,
   "userIntermediatePath": "wknd/idp"
   "synchronizeAttributes":[
       "firstName=profile/givenName"
   ],
   "addGroupMemberships": true,
   "defaultGroups": [ 
       "wknd-users"
   ]
   }
   ```

1. Las tres propiedades de configuración de OSGi requeridas para el cifrado son:

+ `useEncryption` configurado como `true`
+ `spPrivateKeyAlias` contiene el alias de entrada del almacén de claves para la clave privada utilizada por la integración de SAML.
+ `keyStorePassword` contiene un [variable](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=es#secret-configuration-values) de configuración secreta OSGi que contiene el `authentication-service` contraseña del almacén de claves usuario.

+++

## Configuración del filtro de referente

Durante el proceso de autenticación SAML, el Idp inicia una lado del cliente POST HTTP para AEM punto final del `.../saml_login` Publish. Si el IDP y el AEM Publish existen en diferentes origen, el Filtrar __de referencia de__ AEM Publish se configura a través de la configuración de OSGi para permitir HTTP POST desde el origen del IDP.

1. Crear (o edite) un archivo de configuración de OSGi en su proyecto en `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/org.apache.sling.security.impl.ReferrerFilter.cfg.json`.
   + Cambie `/wknd-examples/` a su `/<project name>/`
1. Asegúrese de que el `allow.empty` valor está establecido en `true`, el `allow.hosts` (o, si lo prefiere, `allow.hosts.regexp`) contiene el origen del IDP e `filter.methods` incluye `POST`. La configuración de OSGi debe ser similar a:

   ```json
   {
       "allow.empty": true,
       "allow.hosts.regexp": [ ],
       "allow.hosts": [ 
           "$[env:SAML_IDP_REFERRER;default=dev-123567890.okta.com]"
       ],
       "filter.methods": [
           "POST",
       ],
       "exclude.agents.regexp": [ ]
   }
   ```

AEM Publish admite una única configuración de filtro de referente, por lo que debe combinar los requisitos de configuración de SAML con cualquier configuración existente.

Las configuraciones de OSGi por entorno (`config.publish.dev`, `config.publish.stage` y `config.publish.prod`) se pueden definir con atributos específicos si `allow.hosts` (o `allow.hosts.regex`) varían entre entornos.

## Configuración del intercambio de recursos de origen cruzado (CORS)

Durante el proceso de autenticación SAML, el IDP inicia un POST HTTP del lado del cliente en el punto final `.../saml_login` de AEM Publish. Si el IDP y la publicación de AEM existen en hosts/dominios diferentes, el __intercambio de recursos de origen de CRoss (CORS)__ de AEM Publish debe configurarse para permitir HTTP POST desde el host/dominio del IDP.

El encabezado `Origin` de esta solicitud HTTP POST suele tener un valor diferente al host de publicación de AEM, por lo que requiere la configuración CORS.

Al probar la autenticación SAML en el SDK de AEM local (`localhost:4503`), el IDP puede establecer el `Origin` encabezado en `null`. Si es así, agregue `"null"` a la lista `alloworigin`.

1. Crear un archivo de configuración de OSGi en su proyecto en `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~saml.cfg.json`
   + Cambio `/wknd-examples/` del nombre del proyecto
   + El identificador después de en `~` el nombre de archivo debe identificar de forma exclusiva esta configuración, por lo que puede ser el nombre del IDP, como `...CORSPolicyImpl~okta.cfg.json`. El valor debe ser alfanumérico con guiones.
1. Pegue el siguiente JSON en el archivo `com.adobe.granite.cors.impl.CORSPolicyImpl~...cfg.json`.

```json
{
    "alloworigin": [ 
        "$[env:SAML_IDP_ORIGIN;default=https://dev-1234567890.okta.com]", 
        "null"
    ],
    "allowedpaths": [ 
        ".*/saml_login"
    ],
    "supportedmethods": [ 
        "POST"
    ]
}
```

Las configuraciones de OSGi por entorno (`config.publish.dev`, `config.publish.stage` y `config.publish.prod`) se pueden definir con atributos específicos si `alloworigin` y `allowedpaths` varían entre entornos.

## Configuración de AEM Dispatcher para permitir HTTP POST de SAML

Después de autenticarse correctamente en el IDP, el IDP orquestará un POST HTTP de vuelta al punto final `/saml_login` registrado de AEM (configurado en el IDP). Este POST HTTP para `/saml_login` está bloqueado de forma predeterminada en Dispatcher, por lo que debe permitirse explícitamente usando la siguiente regla de Dispatcher:

1. Abra `dispatcher/src/conf.dispatcher.d/filters/filters.any` en su IDE.
1. añadir en la parte inferior del archivo, se muestra una regla de permitir HTTP POST a URL que terminan con `/saml_login`.

```
...

# Allow SAML HTTP POST to ../saml_login end points
/0190 { /type "allow" /method "POST" /url "*/saml_login" }
```

>[!NOTE]
>Al implementar varias configuraciones de SAML en AEM para varias rutas protegidas y puntos finales de IDP distintos, asegúrese de que el IDP publique en el punto final de RESPECTIVE_PROTECTED_PATH/saml_inicio de sesión para seleccionar la configuración de SAML adecuada en el lado AEM. Si hay duplicado configuraciones de SAML para la misma ruta protegida, la selección de la configuración de SAML se producirá de forma aleatoria.

Si URL reescritura en el servidor web Apache está configurada (`dispatcher/src/conf.d/rewrites/rewrite.rules`), asegúrese de que las solicitudes a los `.../saml_login` puntos finales no se vean afectadas accidentalmente.

## Pertenencia a grupos dinámicos

La pertenencia a grupos dinámicos es una característica [de Apache Jackrabbit Oak](https://jackrabbit.apache.org/oak/docs/security/authentication/external/dynamic.html) que aumenta el rendimiento de grupo evaluación y aprovisionamiento. En esta sección se describe cómo se almacenan los usuarios y los grupos cuando esta función está habilitada y cómo modificar la configuración del controlador de Authentication SAML para habilitarlo para entornos nuevos o existentes.

### Cómo habilitar la pertenencia dinámica a grupos para usuarios de SAML en entornos nuevos

Para mejorar significativamente el rendimiento de la evaluación grupo en entornos nuevos AEM como Cloud Service, se recomienda la activación de la función de pertenencia dinámica a grupos en entornos nuevos.
Este también es un paso necesario cuando se activa la sincronización de datos. Más detalles [aquí](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier) .
Para ello, añada la siguiente Propiedad al archivo de configuración de OSGI:

`/apps/example/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~example.cfg.json`

Con esta configuración, los usuarios y grupos se crean como [usuarios externos de Oak](https://jackrabbit.apache.org/oak/docs/security/authentication/identitymanagement.html). En AEM, los usuarios y grupos externos tienen un(a) `rep:principalName` predeterminado compuesto(a) por `[user name];[idp]` o `[group name];[idp]`.
Observe que las Listas de control de acceso (ACL) están asociadas al nombre principal de los usuarios o grupos.
Al implementar esta configuración en una implementación existente en la que anteriormente `identitySyncType` no se especificaba ni se establecía en `default`, se crearán nuevos usuarios y grupos y se deberá aplicar ACL a estos nuevos usuarios y grupos. Tenga en cuenta que los grupos externos no pueden contener usuarios locales. [Repoinit](https://sling.apache.org/documentation/bundles/repository-initialization.html) se puede usar para crear ACL para grupos externos SAML, incluso si solo se crearán cuando el usuario inicie sesión.
Para evitar esta refactorización en ACL, se ha implementado una [característica de migración estándar](#automatic-migration-to-dynamic-group-membership-for-existing-environments).

### Cómo se almacenan las membresías en grupos locales y externos con miembros del grupo dinámica

En los grupos locales, los miembros grupo se almacenan en el atributo oak: `rep:members`. El atributo contiene el lista de uid de cada miembro del grupo. Puede encontrar [detalles adicionales aquí](https://jackrabbit.apache.org/oak/docs/security/user/membership.html#member-representation-in-the-repository).
Ejemplo:

```
{
  "jcr:primaryType": "rep:Group",
  "rep:principalName": "operators",
  "rep:managedByIdp": "SAML",
  "rep:members": [
    "635afa1c-beeb-3262-83c4-38ea31e5549e",
    "5e496093-feb6-37e9-a2a1-7c87b1cec4b0",
    ...
  ],
   ...
}
```

Los grupos externos con miembros del grupo dinámicas no tienda ningún miembro en la entrada grupo.
En cambio, el miembros del grupo se almacena en las entradas de los usuarios. Encontrará documentación adicional [aquí](https://jackrabbit.apache.org/oak/docs/security/authentication/external/dynamic.html). Por ejemplo, ésta es la nodo OAK para la grupo:

```
{
  "jcr:primaryType": "rep:Group",
  "jcr:mixinTypes": [
    "rep:AccessControllable"
  ],
  "jcr:createdBy": "",
  "jcr:created": "Tue Jul 16 2024 08:58:47 GMT+0000",
  "rep:principalName": "GROUP_1;aem-saml-idp-1",
  "rep:lastSynced": "Tue Jul 16 2024 08:58:47 GMT+0000",
  "jcr:uuid": "d9c6af8a-35c0-3064-899a-59af55455cd0",
  "rep:externalId": "GROUP_1;aem-saml-idp-1",
  "rep:authorizableId": "GROUP_1;aem-saml-idp-1"
}
```

Este es el nodo de un usuario miembro de ese grupo:

```
{
  "jcr:primaryType": "rep:User",
  "jcr:mixinTypes": [
    "rep:AccessControllable"
  ],
  "surname": "Test",
  "rep:principalName": "testUser",
  "rep:externalId": "test;aem-saml-idp-1",
  "rep:authorizableId": "test",
  "rep:externalPrincipalNames": [
    "projects-users;aem-saml-idp-1",
    "GROUP_2;aem-saml-idp-1",
    "GROUP_1;aem-saml-idp-1",
    "operators;aem-saml-idp-1"
  ],
  ...
}
```

### Habilitación de la pertenencia a grupos dinámicos para usuarios de SAML en entornos existentes

Como se explica en la sección anterior, el formato de los usuarios y grupos externos es ligeramente diferente del utilizado para los usuarios y grupos locales. Es posible definir una nueva ACL para grupos externos y aprovisionar nuevos usuarios externos, o utilizar la herramienta de migración como se describe a continuación.

#### Habilitar la pertenencia a grupos dinámicos para entornos existentes con usuarios externos

El controlador Authentication SAML crea usuarios externos cuando se especifica la siguiente Propiedad: `"identitySyncType": "idp"`. En este caso, se pueden activar las miembros del grupo dinámicas modificando este Propiedad para: `"identitySyncType": "idp_dynamic"`. No es necesario realizar ninguna migración.

#### Migración automática a la pertenencia a grupos dinámicos para entornos existentes con usuarios locales

El controlador de autenticación SAML crea usuarios locales cuando se especifica la siguiente propiedad: `"identitySyncType": "default"`. También es el valor predeterminado cuando no se especifica la propiedad. En esta sección describimos los pasos que debe seguir el procedimiento de migración automática.

Cuando esta migración está habilitada, se realiza durante la autenticación del usuario y consiste en los siguientes pasos:
1. El usuario local se migra a un usuario externo manteniendo el nombre de usuario original. Esto implica que los usuarios locales migrados, que ahora actúan como usuarios externos, conservan su nombre de usuario original en lugar de seguir la sintaxis de nomenclatura mencionada en la sección anterior. Se agregará un Propiedad adicional llamado: `rep:externalId` con el valor de `[user name];[idp]`. El usuario `PrincipalName` no se modifica.
2. Por cada grupo externo recibido en la aserción SAML, se crea un grupo externo. Si existe un grupo local correspondiente, el grupo externo se agrega al grupo local como miembro.
3. El usuario se agrega como miembro del grupo externo.
4. El usuario local es eliminado de todos los grupos locales de Saml de los que era miembro. Los grupos locales de SAML se identifican mediante el Propiedad OAK: `rep:managedByIdp`. El controlador de Authentication Saml establece este Propiedad cuando el atributo `syncType` no está especificado o no está configurado en `default`.

Por instancia, si antes de la migración `user1` es un usuario local y un miembro de grupo `group1`local, después de la migración se producirán los siguientes cambios:
`user1` se convierte en un usuario externo. El atributo `rep:externalId` se agrega a su perfil.
`user1`se convierte en miembro del grupo externo: `group1;idp`ya no es miembro directo del grupo local: `user1` `group1` es miembro del grupo local: `group1;idp`.`group1`

`user1` es entonces un miembro de la herencia local grupo: `group1` aunque

El miembros del grupo para grupos externos se almacena en el usuario perfil en el Propiedad `rep:externalPrincipalNames`

### Configuración de la migración automática a Dynamic miembros del grupo

1. Habilite la Propiedad `"identitySyncType": "idp_dynamic_simplified_id"` en el archivo de configuración SAML OSGi: `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json`
2. Configure el nuevo servicio OSGi con Factory PID comenzando con: `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration~`. Por ejemplo, un PID puede ser: `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration~myIdP`. Establezca la siguiente Propiedad:

```
{
  "idpIdentifier": "<value of IDP Identifier (idpIdentifier)" property from the "com.adobe.granite.auth.saml.SamlAuthenticationHandler" configuration to be migrated>"
}
```

Para migrar varias configuraciones de SAML, se deben crear varias configuraciones de fábrica de OSGi para `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration`, cada una de las cuales especifique un `idpIdentifier` para migrar.

## Implementación de la configuración SAML

Las configuraciones de OSGi deben enviarse a Git e implementarse en AEM as a Cloud Service mediante Cloud Manager.

```
$ git remote -v            
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (fetch)
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (push)
$ git add .
$ git commit -m "SAML 2.0 configurations"
$ git push adobe saml-auth:develop
```

Implemente la rama de Git de Cloud Manager de destino (en este ejemplo, `develop`) mediante una canalización de implementación de pila completa.

## Invocación de la autenticación SAML

El flujo de autenticación SAML se puede invocar desde una página web del sitio de AEM, creando vínculos especialmente diseñados o botones. Los parámetros que se describen a continuación se pueden configurar mediante programación según sea necesario, por lo que, por instancia, un botón de inicio de sesión puede configurar el `saml_request_path`, que es donde se toma el usuario tras la autenticación SAML correcta, en diferentes páginas de AEM, según el contexto del botón.

## Almacenamiento en caché seguro mientras se utiliza SAML

En la AEM publicar instancia, la mayoría de las páginas suelen almacenarse en caché. Sin embargo, para las rutas protegidas por SAML, almacenamiento en caché deben deshabilitarse o protegerse almacenamiento en caché habilitarse mediante la configuración auth_checker. Para obtener más información, consulte los detalles proporcionados [aquí](https://experienceleague.adobe.com/es/docs/experience-manager-dispatcher/using/configuring/permissions-cache)

Tenga en cuenta que si almacena en caché rutas protegidas sin habilitar el auth_checker, puede experiencia comportamiento impredecible.

### petición GET

La autenticación SAML se puede invocar creando una petición HTTP GET con el formato:

`HTTP GET /system/sling/login`

y proporcionando parámetros de consulta:

| Nombre del parámetro de consulta | Valor del parámetro de consulta |
|----------------------|-----------------------|
| `resource` | Cualquier ruta JCR, o subruta, que sea la que escuche el controlador de autenticación SAML, tal como se define en la configuración OSGi[&#x200B; &#x200B;](#configure-saml-2-0-authentication-handler) de `path`Adobe Systems Granite SAML 2.0 Propiedad Authentication Handler OSGi. |
| `saml_request_path` | La ruta URL a la que debe llevarse el usuario después de una autenticación SAML correcta. |

Por ejemplo, esta vincular HTML activará el flujo de inicio de sesión SAML y, si se realiza correctamente, llevará la usuario a `/content/wknd/us/en/protected/page.html`. Estos parámetros de consulta se pueden configurar mediante programación según sea necesario.

```html
<a href="/system/sling/login?resource=/content/wknd&saml_request_path=/content/wknd/us/en/protected/page.html">
    Log in using SAML
</a>
```

## petición POST

La autenticación SAML se puede invocar creando un petición HTTP POST en la formato:

`HTTP POST /system/sling/login`

y proporcionar los datos del formulario:

| Nombre de datos del formulario | Valor de datos de formulario |
|----------------------|-----------------------|
| `resource` | Cualquier ruta JCR, o subruta, que sea la que escuche el controlador de autenticación SAML, tal como se define en la configuración OSGi[&#x200B; &#x200B;](#configure-saml-2-0-authentication-handler) de `path`Adobe Systems Granite SAML 2.0 Propiedad Authentication Handler OSGi. |
| `saml_request_path` | La ruta URL a la que debe llevarse el usuario después de una autenticación SAML correcta. |


Por ejemplo, este botón de HTML utilizará un POST HTTP para almacenar en déclencheur el flujo de inicio de sesión de SAML y, una vez realizado correctamente, llevar al usuario a `/content/wknd/us/en/protected/page.html`. Estos parámetros de datos de formulario se pueden definir mediante programación según sea necesario.

```html
<form action="/system/sling/login" method="POST">
    <input type="hidden" name="resource" value="/content/wknd">
    <input type="hidden" name="saml_request_path" value="/content/wknd/us/en/protected/page.html">
    <input type="submit" value="Log in using SAML">
</form>
```

### Configuración de Dispatcher

Tanto el método HTTP GET como el método POST requieren acceso de cliente a los extremos `/system/sling/login` de AEM y, por lo tanto, se deben permitir a través de AEM Dispatcher.

Permita los patrones de URL necesarios en función de si se usa GET o POST

```
# Allow GET-based SAML authentication invocation
/0191 { /type "allow" /method "GET" /url "/system/sling/login" /query "*" }

# Allow POST-based SAML authentication invocation
/0192 { /type "allow" /method "POST" /url "/system/sling/login" }
```
