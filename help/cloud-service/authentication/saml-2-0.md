---
title: SAML 2.0 en AEM as a Cloud Service
description: Obtenga información sobre cómo configurar la autenticación SAML 2.0 en el servicio AEM as a Cloud Service Publish.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9351
thumbnail: 343040.jpeg
last-substantial-update: 2024-05-15T00:00:00Z
exl-id: 461dcdda-8797-4a37-a0c7-efa7b3f1e23e
duration: 2200
source-git-commit: 49f8df6e658b35aa3ba6e4f70cd39ff225c46120
workflow-type: tm+mt
source-wordcount: '3919'
ht-degree: 1%

---

# Autenticación SAML 2.0{#saml-2-0-authentication}

AEM Obtenga información sobre cómo configurar y autenticar usuarios finales (no autores de) en un IDP compatible con SAML 2.0 de su elección.

## ¿Qué es SAML para AEM as a Cloud Service?

AEM La integración de SAML 2.0 con Publish AEM (o Vista previa) de la, permite a los usuarios finales de una experiencia web basada en la identidad de los usuarios finales autenticarse en un IDP (proveedor de identidad) que no es de Adobe AEM y acceder a los usuarios autorizados con nombre como los que se les ha asignado el acceso de forma exclusiva.

|                       | AEM Author | Publicación de AEM |
|-----------------------|:----------:|:-----------:|
| Compatibilidad con SAML 2.0 | ✘ | ✔ |

+++ AEM Comprender el flujo de SAML 2.0 con la ayuda de la aplicación de

AEM El flujo típico de una integración SAML de Publish de la siguiente manera es:

1. AEM El usuario realiza una solicitud a Publish para que la autenticación sea necesaria.
   + El usuario solicita un recurso protegido por CUG/ACL.
   + El usuario solicita un recurso que está sujeto a un requisito de autenticación.
   + AEM El usuario sigue un vínculo al punto de conexión de inicio de sesión de la aplicación (es decir, `/system/sling/login`) que solicita explícitamente la acción de inicio de sesión de la aplicación.
1. AEM Hace una petición AuthnRequest al IDP, solicitando al IDP que inicie el proceso de autenticación.
1. El usuario se autentica en IDP.
   + El IDP solicita credenciales al usuario.
   + El usuario ya se ha autenticado con el IDP y no tiene que proporcionar más credenciales.
1. IDP genera una afirmación de SAML que contiene los datos del usuario y la firma utilizando el certificado privado del IDP.
1. IDP envía la afirmación de SAML a través del POST AEM HTTP, a través del explorador web del usuario, a Publish de la.
1. AEM Publish recibe la afirmación de SAML y valida la integridad y autenticidad de la afirmación de SAML utilizando el certificado público IDP.
1. AEM Publish AEM administra el registro de usuario de en función de la configuración OSGi de SAML 2.0 y el contenido de la afirmación de SAML.
   + Crea un usuario
   + Sincroniza atributos de usuario
   + AEM Actualizaciones de miembros del grupo de usuarios
1. AEM Publish AEM AEM establece la cookie `login-token` de la respuesta HTTP, que se utiliza para autenticar las solicitudes posteriores a la respuesta de Publish de la.
1. AEM Publish AEM redirige al usuario a la dirección URL en el Publish de la forma especificada por la cookie `saml_request_path`, en la que se encuentra el usuario.

+++

## Introducción a la configuración

>[!VIDEO](https://video.tv.adobe.com/v/343040?quality=12&learn=on)

Este vídeo explica cómo configurar la integración de SAML 2.0 con el servicio AEM as a Cloud Service Publish y cómo utilizar Okta como IDP.

## Requisitos previos

Se requiere lo siguiente al configurar la autenticación SAML 2.0:

+ Acceso del administrador de implementación a Cloud Manager
+ AEM Acceso de administrador a la configuración de AEM as a Cloud Service
+ Acceso de administrador al IDP
+ Opcionalmente, acceso a un par de claves pública y privada utilizado para cifrar cargas SAML

AEM SAML 2.0 solo se admite para autenticar a los usuarios para que se utilicen para la autenticación de Publish o la vista previa de la. AEM Para administrar la autenticación de Autor de que usa y IDP, [integre el IDP con Adobe IMS](https://helpx.adobe.com/es/enterprise/using/set-up-identity.html).


## AEM Instalación del certificado público IDP en el

AEM El certificado público del IDP se añade al Almacén de confianza global de y se utiliza para validar que la afirmación de SAML enviada por el IDP es válida.

Flujo de firma de aserción +++SAML

![SAML 2.0 - Firma de aserción SAML IDP](./assets/saml-2-0/idp-signing-diagram.png)

1. El usuario se autentica en IDP.
1. IDP genera una afirmación de SAML que contiene los datos del usuario.
1. IDP firma la afirmación de SAML usando el certificado privado de IDP.
1. IDP inicia un POST AEM HTTP del lado del cliente para el extremo SAML (`.../saml_login`) de Publish que incluye la aserción de SAML firmada.
1. AEM Publish recibe el POST HTTP que contiene la afirmación SAML firmada, puede validar la firma utilizando el certificado público IDP.

+++

![Agregar el certificado público IDP al Almacén de confianza global](./assets/saml-2-0/global-trust-store.png)

1. Obtenga el archivo de __certificado público__ del IDP. AEM AEM Este certificado le permite a los validar la afirmación de SAML proporcionada a los usuarios desplazados internos.

   El certificado está en formato PEM y debe tener un aspecto similar al siguiente:

   ```
   -----BEGIN CERTIFICATE-----
   MIIC4jCBAcoCCQC33wnybT5QZDANBgkqhkiG9w0BAQsFADAyMQswCQYDVQQGEwJV
   ...
   m0eo2USlSRTVl7QHRTuiuSThHpLKQQ==
   -----END CERTIFICATE-----
   ```

1. AEM AEM Inicie sesión en el autor de la como administrador de la aplicación.
1. Vaya a __Herramientas > Seguridad > Almacén de confianza__.
1. Cree o abra el Almacén de confianza global. Si crea un almacén de confianza global, guarde la contraseña en un lugar seguro.
1. Expandir __Agregar certificado del archivo CER__.
1. Seleccione __Seleccionar archivo de certificado__ y cargue el archivo de certificado proporcionado por el IDP.
1. Deje __Asignar certificado al usuario__ en blanco.
1. Seleccione __Enviar__.
1. El certificado recién agregado aparece encima de la sección __Agregar certificado del archivo CRT__.
1. Tome nota del __alias__, ya que este valor se usa en la [configuración OSGi del Controlador de autenticación SAML 2.0](#saml-2-0-authentication-handler-osgi-configuration).
1. Seleccione __Guardar y cerrar__.

El repositorio de confianza global está configurado con el certificado público del IDP de Autor de la publicación, pero dado que SAML solo se utiliza en Publish AEM, el repositorio de confianza global debe replicarse en Publish para que el certificado público IDP sea accesible desde allí. El repositorio de confianza global debe replicarse en AEM AEM para que se pueda acceder a él desde el repositorio de confianza global.

AEM ![Replicar el Almacén de confianza global en el Publish de la](./assets/saml-2-0/global-trust-store-replicate.png)

1. Vaya a __Herramientas > Implementación > Paquetes__.
1. Creación de un paquete
   + Nombre del paquete: `Global Trust Store`
   + Versión: `1.0.0`
   + Grupo: `com.your.company`
1. Edite el nuevo paquete de __Almacén de confianza global__.
1. Seleccione la ficha __Filtros__ y agregue un filtro para la ruta raíz `/etc/truststore`.
1. Seleccione __Listo__ y luego __Guardar__.
1. Seleccione el botón __Generar__ para el paquete de __Almacén de confianza global__.
1. AEM Una vez compilado, seleccione __Más__ > __Replicar__ para activar el nodo del Almacén de confianza global (`/etc/truststore`) para que se active en Publish de la forma que desee.

## Crear almacén de claves del servicio de autenticación{#authentication-service-keystore}

_Es necesario crear un almacén de claves para el servicio de autenticación cuando la propiedad de configuración OSGi del controlador de autenticación [SAML 2.0 `handleLogout` está establecida en `true`](#saml-20-authenticationsaml-2-0-authentication) o cuando se requiere [cifrado de firma AuthnRequest/aserción SAML](#install-aem-public-private-key-pair)_

1. AEM AEM Inicie sesión en Author como administrador de para cargar la clave privada.
1. Vaya a __Herramientas > Seguridad > Usuarios__, seleccione el usuario __authentication-service__ y seleccione __Propiedades__ en la barra de acciones superior.
1. Seleccione la pestaña __Keystore__.
1. Cree o abra el repositorio de claves. Si crea un almacén de claves, mantenga la contraseña a salvo.
   + Un almacén de claves [público/privado está instalado en este almacén de claves](#install-aem-public-private-key-pair) solo si se requiere cifrado de firma AuthnRequest/aserción SAML.
   + Si esta integración de SAML admite el cierre de sesión, pero no la firma AuthnRequest/aserción SAML, basta con un almacén de claves vacío.
1. Seleccione __Guardar y cerrar__.
1. Cree un paquete que contenga el usuario __authentication-service__ actualizado.

   _Use la siguiente solución temporal mediante paquetes:_

   1. Vaya a __Herramientas > Implementación > Paquetes__.
   1. Creación de un paquete
      + Nombre del paquete: `Authentication Service`
      + Versión: `1.0.0`
      + Grupo: `com.your.company`
   1. Edite el nuevo paquete __Almacenamiento de claves del servicio de autenticación__.
   1. Seleccione la ficha __Filtros__ y agregue un filtro para la ruta raíz `/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore`.
      + Para encontrar `<AUTHENTICATION SERVICE UUID>`, vaya a __Herramientas > Seguridad > Usuarios__ y seleccione el usuario __authentication-service__. El UUID es la última parte de la dirección URL.
   1. Seleccione __Listo__ y luego __Guardar__.
   1. Seleccione el botón __Generar__ para el paquete de __Almacenamiento de claves del servicio de autenticación__.
   1. AEM Una vez compilada, seleccione __Más__ > __Replicar__ para activar el almacén de claves del servicio de autenticación en Publish de la.

## AEM Instalar par de claves pública y privada de la{#install-aem-public-private-key-pair}

AEM _La instalación del par de claves pública y privada de la es opcional_

AEM Se puede configurar Publish AEM para que firme las solicitudes de autenticación (a IDP) y cifre las afirmaciones de SAML (a). AEM Esto se logra proporcionando una clave privada para el Publish, y está haciendo coincidir la clave pública con el IDP.

+++ Comprender el flujo de firma de AuthnRequest (opcional)

AEM La AuthnRequest (la solicitud al IDP desde Publish AEM que inicia el proceso de inicio de sesión) puede ser firmada por el usuario de Publish de la red de seguridad de la red (). AEM Para ello, Publish firma la petición de autenticación utilizando la clave privada, de modo que el IDP valide la firma utilizando la clave pública. AEM Esto garantiza al IDP que AuthnRequest se inició, y fue solicitado por el usuario de Publish, y no por un tercero malintencionado.

![SAML 2.0 - SP AuthnRequest firma](./assets/saml-2-0/sp-authnrequest-signing-diagram.png)

1. AEM El usuario realiza una solicitud HTTP a Publish que da lugar a una solicitud de autenticación SAML para el IDP.
1. AEM Publish genera la solicitud de SAML para enviar al IDP.
1. AEM Publish AEM firma la solicitud de SAML usando la clave privada de la clave de la cuenta de usuario que se encuentra en el.
1. AEM Publish inicia AuthnRequest, un redireccionamiento del lado del cliente HTTP al IDP que contiene la solicitud SAML firmada.
1. AEM AEM IDP recibe la solicitud de autor y valida la firma utilizando la clave pública de la firma, lo que garantiza que Publish haya iniciado la solicitud de autor ().
1. AEM A continuación, Publish valida la integridad y autenticidad de la afirmación de SAML descifrada utilizando el certificado público IDP.

+++

+++ Comprender el flujo de cifrado de afirmación de SAML (opcional)

AEM Toda la comunicación HTTP entre IDP y Publish debe ser a través de HTTPS, y por lo tanto segura de forma predeterminada. Sin embargo, según sea necesario, las afirmaciones de SAML pueden cifrarse en el caso de que se requiera confidencialidad adicional además de la proporcionada por HTTPS. Para ello, el IDP cifra los datos de la aserción de SAML utilizando la clave privada y, a la vez, Publish descifra la aserción de SAML utilizando la clave privada, lo que resulta en un descifrado por parte de los usuarios de AEM.

![SAML 2.0: cifrado de aserción SP SAML](./assets/saml-2-0/sp-samlrequest-encryption-diagram.png)

1. El usuario se autentica en IDP.
1. IDP genera una afirmación de SAML que contiene los datos del usuario y la firma utilizando el certificado privado del IDP.
1. AEM AEM A continuación, IDP cifra la afirmación de SAML con la clave pública que se encuentra en el proceso de cifrado, lo que requiere que la clave privada se descifre y se descifre con la clave pública que se encuentra en el proceso de descifrado.
1. AEM La afirmación de SAML cifrada se envía a Publish a través del explorador web del usuario.
1. AEM Publish AEM recibe la afirmación de SAML y la descifra utilizando la clave privada de la que dispone el usuario, que la descifra mediante el uso de la clave privada de la que se ha obtenido el.
1. IDP solicita al usuario que se autentique.

+++

Tanto la firma de AuthnRequest como el cifrado de aserción de SAML son opcionales; sin embargo, ambos están habilitados mediante la propiedad de configuración OSGi del controlador de autenticación [SAML 2.0, lo que significa que se pueden usar ambos o ninguno.`useEncryption`](#saml-20-authenticationsaml-2-0-authentication)

AEM ![almacén de claves del servicio de autenticación de la](./assets/saml-2-0/authentication-service-key-store.png)

1. Obtenga la clave pública, la clave privada (PKCS#8 en formato DER) y el archivo de cadena de certificados (puede ser la clave pública) utilizados para firmar AuthnRequest y cifre la afirmación de SAML. Las claves las suele proporcionar el equipo de seguridad de la organización de TI.

   + Se puede generar un par de claves autofirmadas con __openssl__:

   ```
   $ openssl req -x509 -sha256 -days 365 -newkey rsa:4096 -keyout aem-private.key -out aem-public.crt
   
   # Provide a password (keep in safe place), and other requested certificate information
   
   # Convert the keys to AEM's required format 
   $ openssl rsa -in aem-private.key -outform der -out aem-private.der
   $ openssl pkcs8 -topk8 -inform der -nocrypt -in aem-private.der -outform der -out aem-private-pkcs8.der
   ```

1. Cargue la clave pública al IDP.
   + Utilizando el método `openssl` anterior, la clave pública es el archivo `aem-public.crt`.
1. AEM AEM Inicie sesión en Author como administrador de para cargar la clave privada.
1. Vaya a __Herramientas > Seguridad > Almacén de confianza__, seleccione el usuario __servicio de autenticación__ y seleccione __Propiedades__ en la barra de acciones superior.
1. Vaya a __Herramientas > Seguridad > Usuarios__, seleccione el usuario __authentication-service__ y seleccione __Propiedades__ en la barra de acciones superior.
1. Seleccione la pestaña __Keystore__.
1. Cree o abra el repositorio de claves. Si crea un almacén de claves, mantenga la contraseña a salvo.
1. AEM Seleccione __Agregar clave privada desde el archivo DER__ y agregue la clave privada y el archivo de cadena a los siguientes elementos:
   + __Alias__: proporcione un nombre significativo, a menudo el nombre del IDP.
   + __Archivo de clave privada__: Cargue el archivo de clave privada (PKCS#8 en formato DER).
      + Utilizando el método `openssl` anterior, este es el archivo `aem-private-pkcs8.der`
   + __Seleccionar archivo de cadena de certificados__: cargue el archivo de cadena que lo acompaña (puede ser la clave pública).
      + Utilizando el método `openssl` anterior, este es el archivo `aem-public.crt`
   + Seleccionar __Enviar__
1. El certificado recién agregado aparece encima de la sección __Agregar certificado del archivo CRT__.
   + Tome nota del __alias__, ya que se utiliza en la [configuración OSGi del controlador de autenticación SAML 2.0](#saml-20-authentication-handler-osgi-configuration)
1. Seleccione __Guardar y cerrar__.
1. Cree un paquete que contenga el usuario __authentication-service__ actualizado.

   _Use la siguiente solución temporal mediante paquetes:_

   1. Vaya a __Herramientas > Implementación > Paquetes__.
   1. Creación de un paquete
      + Nombre del paquete: `Authentication Service`
      + Versión: `1.0.0`
      + Grupo: `com.your.company`
   1. Edite el nuevo paquete __Almacenamiento de claves del servicio de autenticación__.
   1. Seleccione la ficha __Filtros__ y agregue un filtro para la ruta raíz `/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore`.
      + Para encontrar `<AUTHENTICATION SERVICE UUID>`, vaya a __Herramientas > Seguridad > Usuarios__ y seleccione el usuario __authentication-service__. El UUID es la última parte de la dirección URL.
   1. Seleccione __Listo__ y luego __Guardar__.
   1. Seleccione el botón __Generar__ para el paquete de __Almacenamiento de claves del servicio de autenticación__.
   1. AEM Una vez compilada, seleccione __Más__ > __Replicar__ para activar el almacén de claves del servicio de autenticación en Publish de la.

## Configurar el controlador de autenticación SAML 2.0{#configure-saml-2-0-authentication-handler}

AEM La configuración de SAML de la se realiza a través de la configuración OSGi del Controlador de autenticación __Adobe Granite SAML 2.0__.
La configuración es una configuración de fábrica de OSGi, lo que significa que un solo servicio de AEM as a Cloud Service Publish AEM puede tener varias configuraciones de SAML que cubran árboles de recursos discretos del repositorio; esto resulta útil para implementaciones de varios sitios de.

+++ Glosario de configuración OSGi del Controlador de autenticación SAML 2.0

### Configuración OSGi del Controlador de autenticación SAML 2.0 de Adobe Granite{#configure-saml-2-0-authentication-handler-osgi-configuration}

|                                   | Propiedad OSGi | Requerido | Formato de valor | Valor predeterminado | Descripción |
|-----------------------------------|-------------------------------|:--------:|:---------------------:|---------------------------|-------------|
| Rutas | `path` | ✔ | Matriz de cadenas | `/` | AEM rutas de acceso de la autenticación para las que se utiliza este controlador. |
| URL de IDP | `idpUrl` | ✔ | Cadena |                           | Dirección URL de IDP a la que se envía la solicitud de autenticación SAML. |
| Alias de certificado IDP | `idpCertAlias` | ✔ | Cadena |                           | AEM Alias del certificado IDP encontrado en el almacén de confianza global de la organización de la |
| Redirección HTTP de IDP | `idpHttpRedirect` | ✘ | Booleano | `false` | Indica si hay un redireccionamiento HTTP a la URL de IDP en lugar de enviar una AuthnRequest. Se estableció en `true` para la autenticación iniciada por IDP. |
| Identificador de IDP | `idpIdentifier` | ✘ | Cadena |                           | AEM ID de IDP único para garantizar la exclusividad de usuario y grupo de la. Si está vacío, se utiliza `serviceProviderEntityId` en su lugar. |
| URL de servicio de consumidor de afirmación | `assertionConsumerServiceURL` | ✘ | Cadena |                           | AEM Atributo de dirección URL `AssertionConsumerServiceURL` en AuthnRequest que especifica a dónde debe enviarse el mensaje `<Response>` para su envío a la dirección de correo electrónico de la dirección de correo electrónico de la dirección de correo electrónico |
| ID de entidad de SP | `serviceProviderEntityId` | ✔ | Cadena |                           | AEM AEM Identifica de forma exclusiva a los desplazados internos, por lo general el nombre de host de la. |
| Cifrado SP | `useEncryption` | ✘ | Booleano | `true` | Indica si el IDP cifra las afirmaciones de SAML. Requiere que se establezcan `spPrivateKeyAlias` y `keyStorePassword`. |
| Alias de clave privada SP | `spPrivateKeyAlias` | ✘ | Cadena |                           | Alias de la clave privada en el almacén de claves del usuario `authentication-service`. Obligatorio si `useEncryption` está establecido en `true`. |
| Contraseña del almacén de claves SP | `keyStorePassword` | ✘ | Cadena |                           | La contraseña del almacén de claves del usuario del servicio de autenticación. Obligatorio si `useEncryption` está establecido en `true`. |
| Redirección predeterminada | `defaultRedirectUrl` | ✘ | Cadena | `/` | La URL de redireccionamiento predeterminada después de la autenticación correcta. AEM Puede ser relativo al host de la (por ejemplo, `/content/wknd/us/en/html`). |
| Atributo de ID de usuario | `userIDAttribute` | ✘ | Cadena | `uid` | AEM Nombre del atributo de aserción de SAML que contiene el ID de usuario del usuario de la. Dejar vacío para utilizar `Subject:NameId`. |
| AEM Crear usuarios de forma automática | `createUser` | ✘ | Booleano | `true` | AEM Indica si los usuarios de la se crean con una autenticación correcta. |
| AEM Ruta intermedia del usuario | `userIntermediatePath` | ✘ | Cadena |                           | AEM Al crear usuarios de la, este valor se usa como ruta intermedia (por ejemplo, `/home/users/<userIntermediatePath>/jane@wknd.com`). Requiere que `createUser` se establezca en `true`. |
| AEM Atributos de usuario de | `synchronizeAttributes` | ✘ | Matriz de cadenas |                           | AEM Lista de asignaciones de atributos SAML que se almacenarán en el usuario de la, con el formato `[ "saml-attribute-name=path/relative/to/user/node" ]` (por ejemplo, `[ "firstName=profile/givenName" ]`). AEM Vea la [lista completa de atributos de la aplicación nativa de la](#aem-user-attributes). |
| AEM Añadir usuario a grupos de | `addGroupMemberships` | ✘ | Booleano | `true` | AEM AEM Indica si se agrega automáticamente un usuario de a los grupos de usuarios después de una autenticación correcta. |
| AEM atributo de pertenencia a grupo | `groupMembershipAttribute` | ✘ | Cadena | `groupMembership` | AEM Nombre del atributo de aserción SAML que contiene una lista de grupos de usuarios a los que se debe agregar el usuario. Requiere que `addGroupMemberships` se establezca en `true`. |
| AEM Grupos predeterminados | `defaultGroups` | ✘ | Matriz de cadenas |                           | AEM Siempre se agrega una lista de usuarios autenticados de grupos de usuarios (por ejemplo, `[ "wknd-user" ]`). Requiere que `addGroupMemberships` se establezca en `true`. |
| Formato de directiva de IDP de nombre | `nameIdFormat` | ✘ | Cadena | `urn:oasis:names:tc:SAML:2.0:nameid-format:transient` | Valor del parámetro de formato NameIDPolicy que se enviará en el mensaje AuthnRequest. |
| Almacenar respuesta de SAML | `storeSAMLResponse` | ✘ | Booleano | `false` | AEM Indica si el valor `samlResponse` está almacenado en el nodo `cq:User` de la. |
| Controlar cierre de sesión | `handleLogout` | ✘ | Booleano | `false` | Indica si este controlador de autenticación SAML administra la solicitud de cierre de sesión. Requiere que se establezca `logoutUrl`. |
| URL de desconexión | `logoutUrl` | ✘ | Cadena |                           | URL de IDP a la que se envía la solicitud de cierre de sesión de SAML. Obligatorio si `handleLogout` está establecido en `true`. |
| Tolerancia del reloj | `clockTolerance` | ✘ | Entero | `60` | AEM La tolerancia de sesgo de reloj de IDP y de (SP) al validar aserciones de SAML. |
| Método de resumen | `digestMethod` | ✘ | Cadena | `http://www.w3.org/2001/04/xmlenc#sha256` | Algoritmo de resumen que utiliza el IDP al firmar un mensaje SAML. |
| Método de firma | `signatureMethod` | ✘ | Cadena | `http://www.w3.org/2001/04/xmldsig-more#rsa-sha256` | Algoritmo de firma que utiliza el IDP al firmar un mensaje SAML. |
| Tipo de sincronización de identidad | `identitySyncType` | ✘ | `default` o `idp` | `default` | No cambie el valor predeterminado `from` para AEM as a Cloud Service. |
| Clasificación del servicio | `service.ranking` | ✘ | Entero | `5002` | Se prefieren configuraciones de clasificación más alta para el mismo `path`. |

### AEM Atributos de usuario de{#aem-user-attributes}

AEM utiliza los atributos de usuario siguientes, que se pueden rellenar mediante la propiedad `synchronizeAttributes` en la configuración OSGi del Controlador de autenticación SAML 2.0 de Granite de Adobe.  AEM AEM AEM Cualquier atributo de IDP se puede sincronizar con cualquier propiedad de usuario de, pero la asignación a propiedades de atributo de uso de la aplicación de datos (enumeradas a continuación) permite a los usuarios de la aplicación utilizarlas de forma natural.

| Atributo de usuario | Ruta de propiedad relativa desde el nodo `rep:User` |
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
| Número de teléfono | `profile/phoneNumber` |
| Acerca de mí | `profile/aboutMe` |

+++

1. Cree un archivo de configuración OSGi en su proyecto en `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` y ábralo en su IDE.
   + Cambiar `/wknd-examples/` a `/<project name>/`
   + El identificador después de `~` en el nombre de archivo debe identificar de forma exclusiva esta configuración, por lo que puede ser el nombre del IDP, como `...~okta.cfg.json`. El valor debe ser alfanumérico y contener guiones.
1. Pegue el siguiente JSON en el archivo `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json` y actualice las referencias `wknd` según sea necesario.

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

1. Actualice los valores según sea necesario en el proyecto. Consulte el __glosario de configuración OSGi del Controlador de autenticación SAML 2.0__ anterior para obtener descripciones de las propiedades de configuración
1. Se recomienda, pero no es obligatorio, utilizar variables y secretos de entorno OSGi, cuando los valores pueden cambiar sin estar sincronizados con el ciclo de lanzamiento o cuando los valores difieren entre tipos de entorno/niveles de servicio similares. Los valores predeterminados se pueden establecer con la sintaxis `$[env:..;default=the-default-value]"`, como se muestra arriba.

Las configuraciones de OSGi por entorno (`config.publish.dev`, `config.publish.stage` y `config.publish.prod`) se pueden definir con atributos específicos si la configuración de SAML varía entre entornos.

### Usar cifrado

Al [cifrar AuthnRequest y la aserción SAML](#encrypting-the-authnrequest-and-saml-assertion), se requieren las siguientes propiedades: `useEncryption`, `spPrivateKeyAlias` y `keyStorePassword`. El `keyStorePassword` contiene una contraseña; por lo tanto, el valor no debe almacenarse en el archivo de configuración OSGi, sino insertarse usando [valores de configuración secretos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#secret-configuration-values)

+++Opcionalmente, actualice la configuración de OSGi para utilizar el cifrado

1. Abra `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` en su IDE.
1. Agregue las tres propiedades `useEncryption`, `spPrivateKeyAlias` y `keyStorePassword` como se muestra a continuación.

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

1. Las tres propiedades de configuración OSGi necesarias para el cifrado son:

+ `useEncryption` se estableció en `true`
+ `spPrivateKeyAlias` contiene el alias de entrada del almacén de claves para la clave privada utilizada por la integración de SAML.
+ `keyStorePassword` contiene una [variable de configuración secreta OSGi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#secret-configuration-values) que contiene la contraseña del almacén de claves de usuario `authentication-service`.

+++

## Configurar el filtro de referente

Durante el proceso de autenticación de SAML, el IDP inicia un POST AEM HTTP del lado del cliente para establecer el punto final `.../saml_login` de Publish de manera que se pueda establecer en un punto de conexión de. AEM Si el IDP y el Publish AEM de la existen en un origen diferente, el __Filtro de referente__ de Publish se configura mediante la configuración OSGi para permitir POST HTTP desde el origen del IDP.

1. Cree (o edite) un archivo de configuración OSGi en su proyecto en `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/org.apache.sling.security.impl.ReferrerFilter.cfg.json`.
   + Cambiar `/wknd-examples/` a `/<project name>/`
1. Asegúrese de que el valor `allow.empty` esté establecido en `true`, de que `allow.hosts` (o, si lo prefiere, `allow.hosts.regexp`) contenga el origen del IDP y de que `filter.methods` incluya `POST`. La configuración de OSGi debe ser similar a:

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

AEM Publish admite una sola configuración de filtro de referente, por lo que debe combinar los requisitos de configuración de SAML con cualquier configuración existente.

Las configuraciones de OSGi por entorno (`config.publish.dev`, `config.publish.stage` y `config.publish.prod`) se pueden definir con atributos específicos si `allow.hosts` (o `allow.hosts.regex`) varían entre entornos.

## Configuración del intercambio de recursos de origen cruzado (CORS)

Durante el proceso de autenticación de SAML, el IDP inicia un POST AEM HTTP del lado del cliente para establecer el punto final `.../saml_login` de Publish de manera que se pueda establecer en un punto de conexión de. AEM Si el IDP y el Publish AEM de la existen en hosts o dominios diferentes, se debe configurar el __uso compartido de recursos CRoss-Origin (CORS)__ de Publish para permitir POST HTTP desde el host/dominio del IDP.

El encabezado `Origin` de esta solicitud del POST AEM HTTP generalmente tiene un valor diferente al del host de Publish de la, por lo que requiere la configuración de CORS.

AEM Al probar la autenticación SAML en el SDK local de la (`localhost:4503`), el IDP puede establecer el encabezado `Origin` en `null`. Si es así, agregue `"null"` a la lista `alloworigin`.

1. Cree un archivo de configuración OSGi en su proyecto en `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~saml.cfg.json`
   + Cambiar `/wknd-examples/` a su nombre de proyecto
   + El identificador después de `~` en el nombre de archivo debe identificar de forma exclusiva esta configuración, por lo que puede ser el nombre del IDP, como `...CORSPolicyImpl~okta.cfg.json`. El valor debe ser alfanumérico y contener guiones.
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

## AEM Configuración de Dispatcher para permitir POST HTTP de SAML

Después de autenticarse correctamente en el IDP, el IDP orquestará un POST AEM HTTP para volver a configurar el punto final registrado `/saml_login` de la sesión de trabajo (configurado en el IDP) de la sesión de trabajo de la sesión de trabajo de la sesión de trabajo de la sesión de trabajo de la sesión de trabajo. Este POST HTTP de `/saml_login` está bloqueado de forma predeterminada en Dispatcher, por lo que se debe permitir explícitamente usando la siguiente regla de Dispatcher:

1. Abra `dispatcher/src/conf.dispatcher.d/filters/filters.any` en su IDE.
1. Agregue al final del archivo una regla de permiso para POST HTTP a direcciones URL que terminen con `/saml_login`.

```
...

# Allow SAML HTTP POST to ../saml_login end points
/0190 { /type "allow" /method "POST" /url "*/saml_login" }
```

Si está configurada la reescritura de URL en el servidor web Apache (`dispatcher/src/conf.d/rewrites/rewrite.rules`), asegúrese de que las solicitudes a los puntos finales `.../saml_login` no se manipulen accidentalmente.

### Cómo habilitar la pertenencia a grupos dinámicos para usuarios de SAML en nuevos entornos

Para mejorar significativamente el rendimiento de la evaluación de grupos en nuevos entornos de AEM as a Cloud Service, se recomienda activar la función de pertenencia a grupos dinámicos en nuevos entornos.
También es un paso necesario cuando se activa la sincronización de datos. Más detalles [aquí](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier) .
Para ello, agregue la siguiente propiedad al archivo de configuración OSGI:

`/apps/example/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~example.cfg.json`

Con esta configuración, los usuarios y grupos se crean como [usuarios externos de Oak](https://jackrabbit.apache.org/oak/docs/security/authentication/identitymanagement.html). AEM En la práctica, los usuarios y grupos externos tienen un valor predeterminado `rep:principalName` compuesto por `[user name];[idp]` o `[group name];[idp]`.
Observe que las Listas de control de acceso (ACL) están asociadas al nombre principal de los usuarios o grupos.
Al implementar esta configuración en una implementación existente en la que anteriormente `identitySyncType` no se especificaba ni se establecía en `default`, se crearán nuevos usuarios y grupos y se deberá aplicar ACL a estos nuevos usuarios y grupos. Tenga en cuenta que los grupos externos no pueden contener usuarios locales. [Repoinit](https://sling.apache.org/documentation/bundles/repository-initialization.html) se puede usar para crear ACL para grupos externos SAML, incluso si solo se crearán cuando el usuario inicie sesión.
Para evitar esta refactorización en ACL, se ha implementado una [característica de migración estándar](#automatic-migration-to-dynamic-group-membership-for-existing-environments).

### Almacenamiento de las suscripciones en grupos locales y externos con pertenencia a grupos dinámicos

En los grupos locales, los miembros del grupo se almacenan en el atributo oak: `rep:members`. El atributo contiene la lista de uid de cada miembro del grupo. Se pueden encontrar detalles adicionales [aquí](https://jackrabbit.apache.org/oak/docs/security/user/membership.html#member-representation-in-the-repository).
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

Los grupos externos con pertenencia a grupo dinámico no almacenan ningún miembro en la entrada de grupo.
La pertenencia al grupo se almacena en las entradas de los usuarios. Encontrará documentación adicional [aquí](https://jackrabbit.apache.org/oak/docs/security/authentication/external/dynamic.html). Por ejemplo, este es el nodo de OAK para el grupo:

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

### Migración automática a la pertenencia a grupos dinámicos para entornos existentes

Cuando esta migración está habilitada, se realiza durante la autenticación del usuario y consiste en los siguientes pasos:
1. El usuario local se migra a un usuario externo mientras se mantiene el nombre de usuario original. Esto implica que los usuarios locales migrados, que ahora actúan como usuarios externos, conservan su nombre de usuario original en lugar de seguir la sintaxis de nomenclatura mencionada en la sección anterior. Se agregará una propiedad adicional llamada: `rep:externalId` con el valor de `[user name];[idp]`. No se modificó el usuario `PrincipalName`.
2. Para cada grupo externo recibido en la aserción SAML, se crea un grupo externo. Si existe un grupo local correspondiente, el grupo externo se agrega al grupo local como miembro.
3. El usuario se agrega como miembro del grupo externo.
4. A continuación, el usuario local se elimina de todos los grupos locales de Saml a los que era miembro. La propiedad OAK identifica los grupos locales de muestra: `rep:managedByIdp`. El controlador de autenticación Saml establece esta propiedad cuando el atributo `syncType` no se especifica o no se establece en `default`.

Por ejemplo, si antes de la migración `user1` es un usuario local y un miembro del grupo local `group1`, después de la migración se producirán los siguientes cambios:
`user1` se convierte en un usuario externo. El atributo `rep:externalId` se agrega a este perfil.
`user1` se convierte en miembro del grupo externo: `group1;idp`
`user1` ya no es un miembro directo del grupo local: `group1`
`group1;idp` es miembro del grupo local: `group1`.
`user1` es entonces miembro del grupo local: `group1` a través de la herencia

La pertenencia a grupos para grupos externos se almacena en el perfil de usuario en el atributo `rep:authorizableId`

### Configuración de la migración automática a la pertenencia a grupos dinámicos

1. Habilite la propiedad `"identitySyncType": "idp_dynamic_simplified_id"` en el archivo de configuración OSGI de SAML: `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json` :
2. Configure el nuevo servicio OSGI con PID: `com.adobe.granite.auth.saml.migration.SamlDynamicGroupMembershipMigration~...` con la propiedad:

```
{
  "idpIdentifier": "<vaule of identitySyncType of saml configuration to be migrated>"
}
```

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

AEM El flujo de autenticación SAML se puede invocar desde una página web del sitio de, creando vínculos especialmente diseñados o botones. AEM Los parámetros que se describen a continuación se pueden configurar mediante programación según sea necesario, por ejemplo, un botón de inicio de sesión puede establecer `saml_request_path`, que es donde se lleva al usuario tras la autenticación SAML correcta, en diferentes páginas de la, según el contexto del botón.

### petición de GET

La autenticación SAML se puede invocar creando una solicitud de GET HTTP con el formato:

`HTTP GET /system/sling/login`

y proporcionando parámetros de consulta:

| Nombre del parámetro de consulta | Valor del parámetro de consulta |
|----------------------|-----------------------|
| `resource` | Cualquier ruta JCR, o subruta, que sea la escucha del controlador de autenticación SAML, tal como se define en la propiedad [OSGi del controlador de autenticación SAML 2.0 de Adobe Granite de la configuración ](#configure-saml-2-0-authentication-handler) `path`. |
| `saml_request_path` | La ruta URL a la que debe dirigirse el usuario después de autenticarse correctamente en SAML. |

Por ejemplo, este vínculo de HTML almacenará en déclencheur el flujo de inicio de sesión de SAML y, una vez realizado correctamente, llevará al usuario a `/content/wknd/us/en/protected/page.html`. Estos parámetros de consulta se pueden configurar mediante programación según sea necesario.

```html
<a href="/system/sling/login?resource=/content/wknd&saml_request_path=/content/wknd/us/en/protected/page.html">
    Log in using SAML
</a>
```

## petición del POST

La autenticación SAML se puede invocar creando una solicitud de POST HTTP con el formato:

`HTTP POST /system/sling/login`

y proporcionar los datos del formulario:

| Nombre de datos de formulario | Valor de datos de formulario |
|----------------------|-----------------------|
| `resource` | Cualquier ruta JCR, o subruta, que sea la escucha del controlador de autenticación SAML, tal como se define en la propiedad [OSGi del controlador de autenticación SAML 2.0 de Adobe Granite de la configuración ](#configure-saml-2-0-authentication-handler) `path`. |
| `saml_request_path` | La ruta URL a la que debe dirigirse el usuario después de autenticarse correctamente en SAML. |


Por ejemplo, este botón de HTML utilizará un POST HTTP para almacenar en déclencheur el flujo de inicio de sesión de SAML y, una vez realizado correctamente, llevar al usuario a `/content/wknd/us/en/protected/page.html`. Estos parámetros de datos de formulario se pueden definir mediante programación según sea necesario.

```html
<form action="/system/sling/login" method="POST">
    <input type="hidden" name="resource" value="/content/wknd">
    <input type="hidden" name="saml_request_path" value="/content/wknd/us/en/protected/page.html">
    <input type="submit" value="Log in using SAML">
</form>
```

### Configuración de Dispatcher

Tanto el método GET como el método POST AEM AEM de HTTP requieren acceso de cliente a los extremos de `/system/sling/login` que se van a usar, por lo que se deben permitir a través de la de Dispatcher.

Permitir los patrones de URL necesarios en función de si se utiliza el GET o el POST

```
# Allow GET-based SAML authentication invocation
/0191 { /type "allow" /method "GET" /url "/system/sling/login" /query="*" }

# Allow POST-based SAML authentication invocation
/0192 { /type "allow" /method "POST" /url "/system/sling/login" }
```
