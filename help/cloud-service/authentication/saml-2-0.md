---
title: AEM SAML 2.0 en as a Cloud Service
description: AEM Obtenga información sobre cómo configurar la autenticación SAML 2.0 en el servicio de publicación as a Cloud Service de.
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
source-git-commit: 11c9173cbb2da75bfccba278e33fc4ca567bbda1
workflow-type: tm+mt
source-wordcount: '3357'
ht-degree: 1%

---

# Autenticación SAML 2.0{#saml-2-0-authentication}

AEM Obtenga información sobre cómo configurar y autenticar usuarios finales (no autores de) en un IDP compatible con SAML 2.0 de su elección.

## AEM ¿Qué SAML es as a Cloud Service para la?

AEM AEM La integración de SAML 2.0 con la publicación (o previsualización) de la, permite a los usuarios finales de una experiencia web basada en la autenticación autenticarse en un IDP (proveedor de identidad) que no es de Adobe AEM y acceder a la misma como un usuario autorizado y con nombre.

|                       | AEM Author | Publicación de AEM |
|-----------------------|:----------:|:-----------:|
| Compatibilidad con SAML 2.0 | ✘ | ✔ |

+++ AEM Comprender el flujo de SAML 2.0 con la ayuda de la aplicación de

AEM El flujo típico de una integración SAML de publicación de es el siguiente:

1. AEM El usuario realiza una solicitud para publicar el mensaje que indica que se requiere autenticación.
   + El usuario solicita un recurso protegido por CUG/ACL.
   + El usuario solicita un recurso que está sujeto a un requisito de autenticación.
   + AEM El usuario sigue un vínculo a un punto final de inicio de sesión de la (por ejemplo, `/system/sling/login`) que solicita explícitamente la acción de inicio de sesión.
1. AEM Hace una petición AuthnRequest al IDP, solicitando al IDP que inicie el proceso de autenticación.
1. El usuario se autentica en IDP.
   + El IDP solicita credenciales al usuario.
   + El usuario ya se ha autenticado con el IDP y no tiene que proporcionar más credenciales.
1. IDP genera una afirmación de SAML que contiene los datos del usuario y la firma utilizando el certificado privado del IDP.
1. IDP envía la afirmación de SAML a través del POST AEM HTTP, a través del explorador web del usuario, a Publicación de.
1. AEM Publicación recibe la afirmación de SAML y valida la integridad y autenticidad de la afirmación de SAML usando el certificado público IDP.
1. AEM AEM Publicación de datos administra el registro de usuario de en función de la configuración OSGi de SAML 2.0 y el contenido de la afirmación de SAML.
   + Crea un usuario
   + Sincroniza atributos de usuario
   + AEM Actualizaciones de miembros del grupo de usuarios
1. AEM AEM La publicación de datos establece el `login-token` AEM en la respuesta HTTP, que se utiliza para autenticar solicitudes posteriores a la publicación de la.
1. AEM AEM Publicación de datos redirige al usuario a la dirección URL en la que se publica el documento según lo especificado por el usuario. `saml_request_path` cookie.

+++

## Introducción a la configuración

>[!VIDEO](https://video.tv.adobe.com/v/343040?quality=12&learn=on)

AEM Este vídeo explica cómo configurar la integración de SAML 2.0 con el servicio Publicación as a Cloud Service de y cómo utilizar Okta como IDP.

## Requisitos previos

Se requiere lo siguiente al configurar la autenticación SAML 2.0:

+ Acceso del administrador de implementación a Cloud Manager
+ AEM AEM Acceso de administrador de a un entorno as a Cloud Service
+ Acceso de administrador al IDP
+ Opcionalmente, acceso a un par de claves pública y privada utilizado para cifrar cargas SAML

AEM SAML 2.0 solo se admite para autenticar usuarios para la publicación o la vista previa de la. AEM Para administrar la autenticación de Autor de la mediante y IDP, [integración del IDP con Adobe IMS](https://helpx.adobe.com/es/enterprise/using/set-up-identity.html).


## AEM Instalación del certificado público IDP en el

AEM El certificado público del IDP se añade al Almacén de confianza global y se utiliza para validar que la afirmación de SAML enviada por el IDP es válida.

Flujo de firma de aserción +++SAML

![SAML 2.0: firma de aserción SAML IDP](./assets/saml-2-0/idp-signing-diagram.png)

1. El usuario se autentica en IDP.
1. IDP genera una afirmación de SAML que contiene los datos del usuario.
1. IDP firma la afirmación de SAML usando el certificado privado de IDP.
1. IDP inicia un POST AEM HTTP del lado del cliente para publicar el extremo SAML de (`.../saml_login`) que incluye la afirmación de SAML firmada.
1. AEM Publish recibe el POST HTTP que contiene la afirmación de SAML firmada, puede validar la firma mediante el certificado público IDP.

+++

![Añadir el certificado público IDP al repositorio de confianza global](./assets/saml-2-0/global-trust-store.png)

1. Obtenga la __certificado público__ del IDP. AEM AEM Este certificado le permite a los validar la afirmación de SAML proporcionada a los usuarios desplazados internos.

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
1. Expandir __Añadir certificado del archivo CER__.
1. Seleccionar __Seleccionar archivo de certificado__ y cargue el archivo de certificado proporcionado por el IDP.
1. Salir __Asignar certificado al usuario__ en blanco.
1. Seleccione __Enviar__.
1. El certificado recién agregado aparece encima de __Agregar certificado del archivo CRT__ sección.
1. Tome nota de la __alias__, ya que este valor se utiliza en [Configuración OSGi del controlador de autenticación SAML 2.0](#saml-2-0-authentication-handler-osgi-configuration).
1. Seleccione __Guardar y cerrar__.

AEM AEM AEM El repositorio de confianza global está configurado con el certificado público del IDP de Autor de la publicación, pero dado que SAML solo se utiliza en la publicación de la publicación, el repositorio de confianza global debe replicarse para la publicación, de modo que el certificado público de IDP pueda acceder a él desde allí.

![AEM Replicar el almacén de confianza global para publicar en el servicio de publicación de](./assets/saml-2-0/global-trust-store-replicate.png)

1. Vaya a __Herramientas > Implementación > Paquetes__.
1. Creación de un paquete
   + Nombre del paquete: `Global Trust Store`
   + Versión: `1.0.0`
   + Grupo: `com.your.company`
1. Editar el nuevo __Almacén de confianza global__ paquete.
1. Seleccione el __Filtros__ y agregue un filtro para la ruta raíz `/etc/truststore`.
1. Seleccionar __Listo__ y luego __Guardar__.
1. Seleccione el __Generar__ para el __Almacén de confianza global__ paquete.
1. Una vez creada, seleccione __Más__ > __Replicar__ para activar el nodo Almacén de confianza global (`/etc/truststore`AEM ) para publicar en el sitio web.

## Crear almacén de claves del servicio de autenticación{#authentication-service-keystore}

_Es necesario crear un repositorio de claves para el servicio de autenticación cuando el [Propiedad de configuración OSGi del controlador de autenticación SAML 2.0 `handleLogout` se establece en `true`](#saml-20-authenticationsaml-2-0-authentication) o cuando [Cifrado de firma AuthnRequest/aserción SAML](#install-aem-public-private-key-pair) es obligatorio_

1. AEM AEM Inicie sesión en Author como administrador de para cargar la clave privada.
1. Vaya a __Herramientas > Seguridad > Usuarios__ y seleccione __authentication-service__ usuario y seleccione __Propiedades__ de la barra de acciones superior.
1. Seleccione el __Keystore__ pestaña.
1. Cree o abra el repositorio de claves. Si crea un almacén de claves, mantenga la contraseña a salvo.
   + A [almacén de claves públicas/privadas instalado en este almacén de claves](#install-aem-public-private-key-pair) solo si se requiere cifrado de firma AuthnRequest/aserción SAML.
   + Si esta integración de SAML admite el cierre de sesión, pero no la firma AuthnRequest/aserción SAML, basta con un almacén de claves vacío.
1. Seleccione __Guardar y cerrar__.
1. Cree un paquete que contenga el __authentication-service__ usuario.

   _Utilice la siguiente solución temporal mediante paquetes:_

   1. Vaya a __Herramientas > Implementación > Paquetes__.
   1. Creación de un paquete
      + Nombre del paquete: `Authentication Service`
      + Versión: `1.0.0`
      + Grupo: `com.your.company`
   1. Editar el nuevo __Almacén de claves del servicio de autenticación__ paquete.
   1. Seleccione el __Filtros__ y agregue un filtro para la ruta raíz `/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore`.
      + El `<AUTHENTICATION SERVICE UUID>` se puede encontrar navegando hasta __Herramientas > Seguridad > Usuarios__ y seleccionando __authentication-service__ usuario. El UUID es la última parte de la dirección URL.
   1. Seleccionar __Listo__ y luego __Guardar__.
   1. Seleccione el __Generar__ para el __Almacén de claves del servicio de autenticación__ paquete.
   1. Una vez creada, seleccione __Más__ > __Replicar__ AEM para activar el almacén de claves del servicio de autenticación para publicar la.

## AEM Instalar par de claves pública y privada de la{#install-aem-public-private-key-pair}

_AEM La instalación del par de claves pública y privada de la es opcional_

AEM AEM Se puede configurar la publicación para firmar solicitudes de autenticación (a IDP) y cifrar aserciones de SAML (a). AEM Esto se logra proporcionando una clave privada para la publicación de la publicación de datos, y es una clave pública que coincide con el IDP.

+++ Comprender el flujo de firma de AuthnRequest (opcional)

AEM AEM AuthnRequest (la solicitud al IDP desde Publicación de datos que inicia el proceso de inicio de sesión) se puede firmar mediante Publicación de datos (Publish) de la. AEM Para ello, el servicio de publicación de firma AuthnRequest con la clave privada, de modo que el IDP valide la firma con la clave pública. AEM Esto garantiza al IDP que AuthnRequest se inició y fue solicitado por Publicación de la publicación de la aplicación, y no por un tercero malintencionado.

![SAML 2.0: firma de AuthnRequest de SP](./assets/saml-2-0/sp-authnrequest-signing-diagram.png)

1. AEM El usuario realiza una petición HTTP para publicar el contenido que da lugar a una petición de autenticación SAML para el IDP.
1. AEM Publicación de datos genera la solicitud de SAML que se va a enviar al IDP.
1. AEM AEM La publicación firma la solicitud de SAML con una clave privada de la.
1. AEM La publicación inicia la petición AuthnRequest, un redireccionamiento del lado del cliente HTTP al IDP que contiene la solicitud SAML firmada.
1. AEM AEM IDP recibe la Solicitud AuthnRequest y valida la firma con clave pública, lo que garantiza que la Publicación haya iniciado la Solicitud AuthnRequest.
1. AEM A continuación, Publicación valida la integridad y autenticidad de la afirmación de SAML descifrada utilizando el certificado público IDP.

+++

+++ Comprender el flujo de cifrado de afirmación de SAML (opcional)

AEM Toda la comunicación HTTP entre IDP y Publicación de la debe ser a través de HTTPS y, por lo tanto, segura de forma predeterminada. Sin embargo, según sea necesario, las afirmaciones de SAML pueden cifrarse en el caso de que se requiera confidencialidad adicional además de la proporcionada por HTTPS. AEM Para ello, el IDP cifra los datos de aserción de SAML utilizando la clave privada y, a la vez, Publicación de datos descifra la aserción de SAML utilizando la clave privada.

![SAML 2.0: cifrado de aserción SP SAML](./assets/saml-2-0/sp-samlrequest-encryption-diagram.png)

1. El usuario se autentica en IDP.
1. IDP genera una afirmación de SAML que contiene los datos del usuario y la firma utilizando el certificado privado del IDP.
1. AEM AEM IDP cifra la afirmación de SAML con clave pública, lo que requiere que se descifre la clave privada de la.
1. AEM La afirmación de SAML cifrada se envía mediante el explorador web del usuario a Publicación de la.
1. AEM AEM Publicación recibe la afirmación de SAML y la descifra utilizando una clave privada
1. IDP solicita al usuario que se autentique.

+++

Tanto la firma de AuthnRequest como el cifrado de aserción SAML son opcionales; sin embargo, ambos están habilitados, utilizando [Propiedad de configuración OSGi del controlador de autenticación SAML 2.0 `useEncryption`](#saml-20-authenticationsaml-2-0-authentication), lo que significa que se pueden usar ambos o ninguno.

![AEM Almacén de claves de servicio de autenticación de](./assets/saml-2-0/authentication-service-key-store.png)

1. Obtenga la clave pública, la clave privada (PKCS#8 en formato DER) y el archivo de cadena de certificados (puede ser la clave pública) utilizados para firmar AuthnRequest y cifre la afirmación de SAML. Las claves las suele proporcionar el equipo de seguridad de la organización de TI.

   + Se puede generar un par de claves autofirmado mediante __openssl__:

   ```
   $ openssl req -x509 -sha256 -days 365 -newkey rsa:4096 -keyout aem-private.key -out aem-public.crt
   
   # Provide a password (keep in safe place), and other requested certificate information
   
   # Convert the keys to AEM's required format 
   $ openssl rsa -in aem-private.key -outform der -out aem-private.der
   $ openssl pkcs8 -topk8 -inform der -nocrypt -in aem-private.der -outform der -out aem-private-pkcs8.der
   ```

1. Cargue la clave pública al IDP.
   + Uso del `openssl` método anterior, la clave pública es `aem-public.crt` archivo.
1. AEM AEM Inicie sesión en Author como administrador de para cargar la clave privada.
1. Vaya a __Herramientas > Seguridad > Almacén de confianza__ y seleccione __authentication-service__ usuario y seleccione __Propiedades__ de la barra de acciones superior.
1. Vaya a __Herramientas > Seguridad > Usuarios__ y seleccione __authentication-service__ usuario y seleccione __Propiedades__ de la barra de acciones superior.
1. Seleccione el __Keystore__ pestaña.
1. Cree o abra el repositorio de claves. Si crea un almacén de claves, mantenga la contraseña a salvo.
1. Seleccionar __Añadir clave privada del archivo DER__ AEM y agregue la clave privada y el archivo de cadena a la lista de archivos que se van a:
   + __Alias__: Proporcione un nombre significativo, a menudo el nombre del IDP.
   + __Archivo de clave privada__: cargue el archivo de clave privada (PKCS#8 en formato DER).
      + Uso del `openssl` método anterior, este es el `aem-private-pkcs8.der` archivo
   + __Seleccionar archivo de cadena de certificados__: cargue el archivo de cadena adjunto (puede ser la clave pública).
      + Uso del `openssl` método anterior, este es el `aem-public.crt` archivo
   + Seleccionar __Enviar__
1. El certificado recién agregado aparece encima de __Agregar certificado del archivo CRT__ sección.
   + Tome nota de la __alias__ ya que se utiliza en la [Configuración OSGi del controlador de autenticación SAML 2.0](#saml-20-authentication-handler-osgi-configuration)
1. Seleccione __Guardar y cerrar__.
1. Cree un paquete que contenga el __authentication-service__ usuario.

   _Utilice la siguiente solución temporal mediante paquetes:_

   1. Vaya a __Herramientas > Implementación > Paquetes__.
   1. Creación de un paquete
      + Nombre del paquete: `Authentication Service`
      + Versión: `1.0.0`
      + Grupo: `com.your.company`
   1. Editar el nuevo __Almacén de claves del servicio de autenticación__ paquete.
   1. Seleccione el __Filtros__ y agregue un filtro para la ruta raíz `/home/users/system/cq:services/internal/security/<AUTHENTICATION SERVICE UUID>/keystore`.
      + El `<AUTHENTICATION SERVICE UUID>` se puede encontrar navegando hasta __Herramientas > Seguridad > Usuarios__ y seleccionando __authentication-service__ usuario. El UUID es la última parte de la dirección URL.
   1. Seleccionar __Listo__ y luego __Guardar__.
   1. Seleccione el __Generar__ para el __Almacén de claves del servicio de autenticación__ paquete.
   1. Una vez creada, seleccione __Más__ > __Replicar__ AEM para activar el almacén de claves del servicio de autenticación para publicar la.

## Configurar el controlador de autenticación SAML 2.0{#configure-saml-2-0-authentication-handler}

AEM La configuración de ML de SAID se realiza mediante la variable __Controlador de autenticación de Adobe Granite SAML 2.0__ Configuración de OSGi.
AEM La configuración es una configuración de fábrica de OSGi, lo que significa que un solo servicio de publicación as a Cloud Service AEM de la puede tener varias configuraciones de SAML que cubran árboles de recursos discretos del repositorio; esto resulta útil para implementaciones de varios sitios de la.

+++ Glosario de configuración OSGi del Controlador de autenticación SAML 2.0

### Configuración OSGi del Controlador de autenticación SAML 2.0 de Adobe Granite{#configure-saml-2-0-authentication-handler-osgi-configuration}

|                                   | Propiedad OSGi | Requerido | Formato de valor | Valor predeterminado | Descripción |
|-----------------------------------|-------------------------------|:--------:|:---------------------:|---------------------------|-------------|
| Rutas | `path` | ✔ | Matriz de cadenas | `/` | AEM rutas de acceso de la autenticación para las que se utiliza este controlador. |
| URL de IDP | `idpUrl` | ✔ | Cadena |                           | Dirección URL de IDP a la que se envía la solicitud de autenticación SAML. |
| Alias de certificado IDP | `idpCertAlias` | ✔ | Cadena |                           | AEM Alias del certificado IDP encontrado en el repositorio de confianza global de la organización de seguridad de la red (CLR) de |
| Redirección HTTP de IDP | `idpHttpRedirect` | ✘ | Booleano | `false` | Indica si hay un redireccionamiento HTTP a la URL de IDP en lugar de enviar una AuthnRequest. Configure como. `true` para la autenticación iniciada por IDP. |
| Identificador de IDP | `idpIdentifier` | ✘ | Cadena |                           | AEM ID de IDP único para garantizar la exclusividad de usuario y grupo de la. Si está vacío, la variable `serviceProviderEntityId` se utiliza en su lugar. |
| URL de servicio de consumidor de afirmación | `assertionConsumerServiceURL` | ✘ | Cadena |                           | El `AssertionConsumerServiceURL` Atributo URL en AuthnRequest que especifica dónde `<Response>` AEM el mensaje debe enviarse a la. |
| ID de entidad de SP | `serviceProviderEntityId` | ✔ | Cadena |                           | AEM AEM Identifica de forma exclusiva a los desplazados internos, por lo general el nombre de host de la. |
| Cifrado SP | `useEncryption` | ✘ | Booleano | `true` | Indica si el IDP cifra las afirmaciones de SAML. Requiere `spPrivateKeyAlias` y `keyStorePassword` que se va a establecer. |
| Alias de clave privada SP | `spPrivateKeyAlias` | ✘ | Cadena |                           | El alias de la clave privada en el `authentication-service` almacén de claves del usuario. Obligatorio si `useEncryption` se establece en `true`. |
| Contraseña del almacén de claves SP | `keyStorePassword` | ✘ | Cadena |                           | La contraseña del almacén de claves del usuario del servicio de autenticación. Obligatorio si `useEncryption` se establece en `true`. |
| Redirección predeterminada | `defaultRedirectUrl` | ✘ | Cadena | `/` | La URL de redireccionamiento predeterminada después de la autenticación correcta. AEM Puede ser relativo al host de la (por ejemplo, `/content/wknd/us/en/html`). |
| Atributo de ID de usuario | `userIDAttribute` | ✘ | Cadena | `uid` | AEM Nombre del atributo de aserción de SAML que contiene el ID de usuario del usuario de la. Dejar vacío para utilizar el `Subject:NameId`. |
| AEM Crear usuarios de forma automática | `createUser` | ✘ | Booleano | `true` | AEM Indica si los usuarios de la se crean con una autenticación correcta. |
| AEM Ruta intermedia del usuario | `userIntermediatePath` | ✘ | Cadena |                           | AEM Al crear usuarios de, este valor se utiliza como ruta intermedia (por ejemplo, `/home/users/<userIntermediatePath>/jane@wknd.com`). Requiere `createUser` que se establecerá en `true`. |
| AEM Atributos de usuario de | `synchronizeAttributes` | ✘ | Matriz de cadenas |                           | AEM Lista de asignaciones de atributos SAML que se almacenarán en el usuario de la, con el formato `[ "saml-attribute-name=path/relative/to/user/node" ]` (por ejemplo, `[ "firstName=profile/givenName" ]`). Consulte la [AEM lista completa de atributos nativos de la](#aem-user-attributes). |
| AEM Añadir usuario a grupos de | `addGroupMemberships` | ✘ | Booleano | `true` | AEM AEM Indica si se agrega automáticamente un usuario de a los grupos de usuarios después de una autenticación correcta. |
| AEM atributo de pertenencia a grupo | `groupMembershipAttribute` | ✘ | Cadena | `groupMembership` | AEM Nombre del atributo de aserción SAML que contiene una lista de grupos de usuarios a los que se debe agregar el usuario. Requiere `addGroupMemberships` que se establecerá en `true`. |
| AEM Grupos predeterminados | `defaultGroups` | ✘ | Matriz de cadenas |                           | AEM Siempre se agrega una lista de grupos de usuarios autenticados de la a (por ejemplo, `[ "wknd-user" ]`). Requiere `addGroupMemberships` que se establecerá en `true`. |
| Formato de directiva de IDP de nombre | `nameIdFormat` | ✘ | Cadena | `urn:oasis:names:tc:SAML:2.0:nameid-format:transient` | Valor del parámetro de formato NameIDPolicy que se enviará en el mensaje AuthnRequest. |
| Almacenar respuesta de SAML | `storeSAMLResponse` | ✘ | Booleano | `false` | Indica si la variable `samlResponse` AEM El valor se almacena en la `cq:User` nodo. |
| Controlar cierre de sesión | `handleLogout` | ✘ | Booleano | `false` | Indica si este controlador de autenticación SAML administra la solicitud de cierre de sesión. Requiere `logoutUrl` que se va a establecer. |
| URL de desconexión | `logoutUrl` | ✘ | Cadena |                           | URL de IDP a la que se envía la solicitud de cierre de sesión de SAML. Obligatorio si `handleLogout` se establece en `true`. |
| Tolerancia del reloj | `clockTolerance` | ✘ | Entero | `60` | AEM La tolerancia de sesgo de reloj de IDP y de (SP) al validar aserciones de SAML. |
| Método de resumen | `digestMethod` | ✘ | Cadena | `http://www.w3.org/2001/04/xmlenc#sha256` | Algoritmo de resumen que utiliza el IDP al firmar un mensaje SAML. |
| Método de firma | `signatureMethod` | ✘ | Cadena | `http://www.w3.org/2001/04/xmldsig-more#rsa-sha256` | Algoritmo de firma que utiliza el IDP al firmar un mensaje SAML. |
| Tipo de sincronización de identidad | `identitySyncType` | ✘ | `default` o `idp` | `default` | No cambiar `from` AEM Valor predeterminado para la as a Cloud Service. |
| Clasificación del servicio | `service.ranking` | ✘ | Entero | `5002` | Para lo mismo, se prefieren configuraciones de clasificación más altas `path`. |

### AEM Atributos de usuario de{#aem-user-attributes}

AEM utiliza los siguientes atributos de usuario, que se pueden rellenar mediante el `synchronizeAttributes` en la configuración OSGi del Controlador de autenticación SAML 2.0 de Granite de Adobe.  AEM AEM AEM Cualquier atributo de IDP se puede sincronizar con cualquier propiedad de usuario de, pero la asignación a propiedades de atributo de uso de la aplicación de datos (enumeradas a continuación) permite a los usuarios de la aplicación utilizarlas de forma natural.

| Atributo de usuario | Ruta de propiedad relativa desde `rep:User` nodo |
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

1. Cree un archivo de configuración OSGi en su proyecto en `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` y abra en su IDE.
   + Cambiar `/wknd-examples/` a su `/<project name>/`
   + El identificador después de `~` en el nombre de archivo debe identificar de forma exclusiva esta configuración, por lo que puede ser el nombre del IDP, como `...~okta.cfg.json`. El valor debe ser alfanumérico y contener guiones.
1. Pegue el siguiente JSON en `com.adobe.granite.auth.saml.SamlAuthenticationHandler~...cfg.json` y actualice el archivo `wknd` referencias según sea necesario.

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

1. Actualice los valores según sea necesario en el proyecto. Consulte la __Glosario de configuración OSGi del Controlador de autenticación SAML 2.0__ más arriba para las descripciones de propiedades de configuración
1. Se recomienda, pero no es obligatorio, utilizar variables y secretos de entorno OSGi, cuando los valores pueden cambiar sin estar sincronizados con el ciclo de lanzamiento o cuando los valores difieren entre tipos de entorno/niveles de servicio similares. Los valores predeterminados se pueden configurar con la variable `$[env:..;default=the-default-value]"` sintaxis como se muestra arriba.

Configuraciones de OSGi por entorno (`config.publish.dev`, `config.publish.stage`, y `config.publish.prod`) se puede definir con atributos específicos si la configuración de SAML varía entre entornos.

### Usar cifrado

Cuándo [cifrar la aserción AuthnRequest y SAML](#encrypting-the-authnrequest-and-saml-assertion), se requieren las siguientes propiedades: `useEncryption`, `spPrivateKeyAlias`, y `keyStorePassword`. El `keyStorePassword` contiene una contraseña; por lo tanto, el valor no debe almacenarse en el archivo de configuración OSGi, sino insertarse mediante [valores de configuración secretos](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#secret-configuration-values)

+++Opcionalmente, actualice la configuración de OSGi para utilizar el cifrado

1. Abrir `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.auth.saml.SamlAuthenticationHandler~saml.cfg.json` en su IDE.
1. Añadir las tres propiedades `useEncryption`, `spPrivateKeyAlias`, y `keyStorePassword` como se muestra a continuación.

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

+ `useEncryption` establezca en `true`
+ `spPrivateKeyAlias` contiene el alias de la entrada del almacén de claves para la clave privada utilizada por la integración de SAML.
+ `keyStorePassword` contiene un [Variable de configuración secreta OSGi](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#secret-configuration-values) que contiene el `authentication-service` contraseña del almacén de claves de usuario.

+++

## Configurar el filtro de referente

Durante el proceso de autenticación de SAML, el IDP inicia un POST AEM HTTP del lado del cliente para publicar el de la aplicación de la manera de `.../saml_login` punto final. AEM AEM Si el IDP y la Publicación de la existen en un origen diferente, se puede publicar el de __Filtro de referente__ se configura mediante la configuración de OSGi para permitir HTTP POST desde el origen del IDP.

1. Cree (o edite) un archivo de configuración OSGi en su proyecto en `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/org.apache.sling.security.impl.ReferrerFilter.cfg.json`.
   + Cambiar `/wknd-examples/` a su `/<project name>/`
1. Asegúrese de que `allow.empty` el valor se establece en `true`, el `allow.hosts` (o si lo prefiere, `allow.hosts.regexp`) contiene el origen del IDP, y `filter.methods` incluye `POST`. La configuración de OSGi debe ser similar a:

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

AEM La publicación admite una sola configuración de filtro de referente, por lo que debe combinar los requisitos de configuración de SAML con cualquier configuración existente.

Configuraciones de OSGi por entorno (`config.publish.dev`, `config.publish.stage`, y `config.publish.prod`) se puede definir con atributos específicos si `allow.hosts` (o `allow.hosts.regex`) varían entre entornos.

## Configuración del intercambio de recursos de origen cruzado (CORS)

Durante el proceso de autenticación de SAML, el IDP inicia un POST AEM HTTP del lado del cliente para publicar el de la aplicación de la manera de `.../saml_login` punto final. AEM AEM Si el IDP y la publicación de la existen en hosts o dominios diferentes, haga clic en Publicar en la opción de __CRoss-Origin Resource Sharing (CORS)__ debe configurarse para permitir HTTP POST desde el host/dominio del IDP.

Esta solicitud del POST HTTP `Origin` AEM El encabezado de suele tener un valor diferente al del host de publicación de, por lo que requiere la configuración de CORS.

AEM Al probar la autenticación SAML en el SDK local de la (`localhost:4503`), el IDP puede establecer el `Origin` encabezado a `null`. Si es así, agregue. `"null"` a la `alloworigin` lista.

1. Cree un archivo de configuración OSGi en su proyecto en `/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.adobe.granite.cors.impl.CORSPolicyImpl~saml.cfg.json`
   + Cambiar `/wknd-examples/` al nombre de su proyecto
   + El identificador después de `~` en el nombre de archivo debe identificar de forma exclusiva esta configuración, por lo que puede ser el nombre del IDP, como `...CORSPolicyImpl~okta.cfg.json`. El valor debe ser alfanumérico y contener guiones.
1. Pegue el siguiente JSON en `com.adobe.granite.cors.impl.CORSPolicyImpl~...cfg.json` archivo.

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

Configuraciones de OSGi por entorno (`config.publish.dev`, `config.publish.stage`, y `config.publish.prod`) se puede definir con atributos específicos si `alloworigin` y `allowedpaths` varía según el entorno.

## AEM Configuración de Dispatcher para permitir POST HTTP de SAML

Después de autenticarse correctamente en el IDP, este organizará un POST AEM HTTP para que vuelva a estar registrado en el registro de la dirección de correo electrónico (HTTPs). `/saml_login` punto final (configurado en el IDP). Este POST HTTP a `/saml_login` está bloqueado de forma predeterminada en Dispatcher, por lo que debe permitirse explícitamente el uso de la siguiente regla de Dispatcher:

1. Abrir `dispatcher/src/conf.dispatcher.d/filters/filters.any` en su IDE.
1. Agregue a la parte inferior del archivo una regla de permiso para POST HTTP a direcciones URL que terminen con `/saml_login`.

```
...

# Allow SAML HTTP POST to ../saml_login end points
/0190 { /type "allow" /method "POST" /url "*/saml_login" }
```

Si la reescritura de URL en el servidor web Apache está configurada (`dispatcher/src/conf.d/rewrites/rewrite.rules`), asegúrese de que las solicitudes a `.../saml_login` los puntos finales no se estropean accidentalmente.

## Implementación de la configuración SAML

AEM Las configuraciones de OSGi deben enviarse a Git e implementarse para que se puedan usar de forma as a Cloud Service mediante Cloud Manager.

```
$ git remote -v            
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (fetch)
adobe   https://git.cloudmanager.adobe.com/myOrg/myCloudManagerGit/ (push)
$ git add .
$ git commit -m "SAML 2.0 configurations"
$ git push adobe saml-auth:develop
```

Implemente la rama Git de Cloud Manager de destino (en este ejemplo, `develop`), mediante una canalización de implementación de pila completa.

## Invocación de la autenticación SAML

AEM El flujo de autenticación SAML se puede invocar desde una página web del sitio de, creando vínculos especialmente diseñados o botones. Los parámetros que se describen a continuación se pueden configurar mediante programación según sea necesario, por ejemplo, un botón de inicio de sesión puede configurar el `saml_request_path`AEM , que es donde se lleva al usuario tras la autenticación SAML correcta, a diferentes páginas de la lista de distribución, según el contexto del botón.

### petición de GET

La autenticación SAML se puede invocar creando una solicitud de GET HTTP con el formato:

`HTTP GET /system/sling/login`

y proporcionando parámetros de consulta:

| Nombre del parámetro de consulta | Valor del parámetro de consulta |
|----------------------|-----------------------|
| `resource` | Cualquier ruta JCR, o subruta, que sea la escucha del controlador de autenticación SAML, tal como se define en la variable [Configuración de OSGi del controlador de autenticación SAML 2.0 de Adobe Granite](#configure-saml-2-0-authentication-handler) `path` propiedad. |
| `saml_request_path` | La ruta URL a la que debe dirigirse el usuario después de autenticarse correctamente en SAML. |

Por ejemplo, este vínculo de HTML almacenará en déclencheur el flujo de inicio de sesión de SAML y, cuando se realice correctamente, llevará al usuario a `/content/wknd/us/en/protected/page.html`. Estos parámetros de consulta se pueden configurar mediante programación según sea necesario.

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
| `resource` | Cualquier ruta JCR, o subruta, que sea la escucha del controlador de autenticación SAML, tal como se define en la variable [Configuración de OSGi del controlador de autenticación SAML 2.0 de Adobe Granite](#configure-saml-2-0-authentication-handler) `path` propiedad. |
| `saml_request_path` | La ruta URL a la que debe dirigirse el usuario después de autenticarse correctamente en SAML. |


Por ejemplo, este botón del HTML utilizará un POST HTTP para almacenar en déclencheur el flujo de inicio de sesión de SAML y, una vez realizado correctamente, llevar al usuario a `/content/wknd/us/en/protected/page.html`. Estos parámetros de datos de formulario se pueden definir mediante programación según sea necesario.

```html
<form action="/system/sling/login" method="POST">
    <input type="hidden" name="resource" value="/content/wknd">
    <input type="hidden" name="saml_request_path" value="/content/wknd/us/en/protected/page.html">
    <input type="submit" value="Log in using SAML">
</form>
```

### Configuración de Dispatcher

Tanto los métodos GET de HTTP como los métodos POST AEM requieren el acceso de cliente a los recursos de la `/system/sling/login` AEM puntos finales y, por lo tanto, deben permitirse a través de Dispatcher de.

Permitir los patrones de URL necesarios en función de si se utiliza el GET o el POST

```
# Allow GET-based SAML authentication invocation
/0191 { /type "allow" /method "GET" /url "/system/sling/login" /query="*" }

# Allow POST-based SAML authentication invocation
/0192 { /type "allow" /method "POST" /url "/system/sling/login" }
```
