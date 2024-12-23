---
title: AEM Contenido protegido en entornos sin encabezado
description: AEM Obtenga información sobre cómo proteger el contenido en sin encabezado
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer, Architect
level: Intermediate
jira: KT-15233
last-substantial-update: 2024-05-01T00:00:00Z
exl-id: c4b093d4-39b8-4f0b-b759-ecfbb6e9e54f
duration: 254
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1151'
ht-degree: 0%

---

# AEM Protección del contenido en entornos sin encabezado

AEM AEM Publish Garantizar la integridad y seguridad de los datos al servir contenido sin encabezado de la es crucial cuando se sirve contenido confidencial. AEM Este procedimiento le guiará por la protección del contenido servido por los extremos de la API de GraphQL sin encabezado de.

Las directrices de este tutorial indican requisitos estrictos para que el contenido esté disponible exclusivamente para usuarios o grupos de usuarios específicos. Es imperativo distinguir entre contenido de marketing personalizado y contenido privado, como PII o datos financieros personales, para evitar confusiones y resultados no deseados. Este tutorial aborda la protección del contenido privado.

Al hablar del contenido de marketing, nos referimos a contenido adaptado a usuarios o grupos individuales, que no está pensado para el consumo general. Sin embargo, es esencial comprender que, aunque este contenido puede estar dirigido a determinados usuarios, su exposición fuera del contexto deseado (por ejemplo, mediante la manipulación de solicitudes HTTP) no supone un riesgo de seguridad, legal o de reputación.

Se enfatiza que todo el contenido abordado en este artículo se asume como privado, y solo puede ser visto por usuarios o grupos designados. El contenido de marketing a menudo no requiere protección, sino que su entrega a usuarios específicos puede ser administrado por la aplicación y almacenado en caché para obtener rendimiento.

Este procedimiento no cubre lo siguiente:

- Proteger los puntos de conexión directamente, pero en su lugar se centra en proteger el contenido que entregan.
- AEM Autenticación para la obtención de tokens de inicio de sesión de Publish o. Los métodos de autenticación y la aprobación de credenciales dependen de los casos de uso y las implementaciones individuales.

## Grupos de usuarios

En primer lugar, debemos definir un [grupo de usuarios](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/accessing/aem-users-groups-and-permissions) que contenga los usuarios que deben tener acceso al contenido protegido.

AEM ![Grupo de usuarios de contenido protegido sin encabezado](./assets/protected-content/user-groups.png){align="center"}

AEM Los grupos de usuarios asignan acceso al contenido sin encabezado de la, incluidos los fragmentos de contenido u otros recursos a los que se hace referencia.

1. AEM Inicie sesión en el autor de la como **administrador de usuarios**.
1. Vaya a **Herramientas** > **Seguridad** > **Grupos**.
1. Seleccione **Crear** en la esquina superior derecha.
1. En la ficha **Detalles**, especifique el **Id. de grupo** y el **Nombre de grupo**.
   - AEM El Id. de grupo y el Nombre de grupo pueden ser cualquier cosa, pero en este ejemplo se usa el nombre **usuarios de API sin encabezado**.
1. Seleccione **Guardar y cerrar**.
1. Seleccione el grupo recién creado y, a continuación, elija **Activar** en la barra de acciones.

Si se requieren varios niveles de acceso, cree varios grupos de usuarios que se puedan asociar con contenido diferente.

### Adición de usuarios a grupos de usuarios

AEM Para conceder acceso a las solicitudes de la API de GraphQL sin encabezado a contenido protegido, puede asociar la solicitud sin encabezado a un usuario que pertenezca a un grupo de usuarios específico. Estos son dos enfoques comunes:

1. **Cuentas técnicas de [AEM as a Cloud Service](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials):**
   - Cree una cuenta técnica en AEM as a Cloud Service Developer Console.
   - AEM Inicie sesión una vez en el Autor de la sesión con la cuenta técnica de.
   - AEM Agregue la cuenta técnica al grupo de usuarios a través de **Herramientas > Seguridad > Grupos > Usuarios de API sin encabezado > Usuarios de la API sin encabezado > Miembros**.
   - **Activar** tanto el usuario técnico de la cuenta como el grupo de usuarios en Publish de la cuenta de usuario de la cuenta de usuario de la cuenta de usuario de la cuenta de usuario de la cuenta de usuario de la cuenta de usuario de la cuenta de usuario de la cuenta de usuario de la cuenta de usuario de AEM.
   - Este método requiere que el cliente sin encabezado no exponga las credenciales de servicio al usuario, ya que son credenciales para un usuario específico y no se deben compartir.

   AEM ![Administración de grupos de cuentas técnicas de](./assets/protected-content/group-membership.png){align="center"}

2. **Usuarios con nombre:**
   - AEM Autentique usuarios con nombre y agréguelos directamente al grupo de usuarios de Publish de la.
   - AEM Este método requiere que el cliente sin encabezado autentique las credenciales de usuario con la autenticación de Publish AEM AEM, obtenga un inicio de sesión o un token de acceso de la y utilice este token para las solicitudes posteriores que se vayan a crear. Los detalles de cómo conseguirlo no se tratan en este procedimiento y dependen de la implementación de.

## Protección de fragmentos de contenido

AEM La protección de los fragmentos de contenido es esencial para proteger el contenido sin encabezado de la y se logra asociando el contenido con un grupo de usuarios cerrado (CUG). AEM Cuando un usuario realiza una solicitud a la API de GraphQL sin encabezado de, el contenido devuelto se filtra en función de los CUG del usuario.

AEM ![CUG sin encabezado](./assets/protected-content/cugs.png){align="center"}

Siga estos pasos para lograr esto a través de [Grupos de usuarios cerrados (CUG)](https://experienceleague.adobe.com/en/docs/experience-manager-learn/assets/advanced/closed-user-groups).

1. AEM Inicie sesión en el autor de la como **usuario de DAM**.
2. Vaya a **Assets > Archivos** y seleccione la **carpeta** que contiene los fragmentos de contenido que desea proteger. Los CUG se aplican jerárquicamente y afectan a las subcarpetas a menos que un CUG diferente los sustituya.
   - Asegúrese de que los usuarios que pertenecen a otros canales que utilizan el contenido de las carpetas están incluidos en este grupo de usuarios. También puede incluir los grupos de usuarios asociados a esos canales en la lista de CUG. Si no es así, esos canales no podrán acceder al contenido.
3. Seleccione la carpeta y elija **Propiedades** en la barra de herramientas.
4. Seleccione la ficha **Permisos**.
5. Escriba el **Nombre de grupo** y seleccione el botón **Agregar** para agregar el nuevo CUG.
6. **Guardar** para aplicar el CUG.
7. **Seleccione** la carpeta de recursos y seleccione **Publish AEM** para enviar la carpeta con los CUG aplicados a Publish, donde se evaluará como un permiso.

Realice estos mismos pasos para todas las carpetas que contienen fragmentos de contenido que deben protegerse, aplicando los CUG correctos a cada carpeta.

AEM Ahora, cuando se realiza una solicitud HTTP al punto de conexión de API de GraphQL sin encabezado de, solo se incluyen en el resultado los fragmentos de contenido accesibles mediante los CUG especificados por el usuario solicitante. Si el usuario no tiene acceso a ningún fragmento de contenido, el resultado estará vacío, aunque devolverá un código de estado HTTP 200.

### Protección del contenido referenciado

AEM Los fragmentos de contenido suelen hacer referencia a otro contenido de la, como imágenes. Para proteger este contenido referenciado, aplique CUG a las carpetas de recursos en las que están almacenados los recursos referenciados. AEM Tenga en cuenta que los recursos a los que se hace referencia suelen solicitarse mediante métodos distintos de los de las API de GraphQL sin encabezado de la. Por lo tanto, la forma en que se pasan los tokens de acceso en las solicitudes a estos recursos a los que se hace referencia puede diferir.

Según la arquitectura de contenido, puede ser necesario aplicar CUG a varias carpetas para garantizar que todo el contenido referenciado esté protegido.

## Impedir el almacenamiento en caché de contenido protegido

AEM as a Cloud Service [almacena en caché las respuestas HTTP de forma predeterminada](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/caching/publish) para mejorar el rendimiento. Sin embargo, esto puede causar problemas al servir contenido protegido. AEM Para evitar el almacenamiento en caché de dicho contenido, [quite los encabezados de caché para extremos específicos](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/caching/publish#how-to-customize-cache-rules-1) en la configuración de Apache de la instancia de Publish de la.

Añada la siguiente regla al archivo de configuración de Apache del proyecto de Dispatcher para eliminar los encabezados de caché de puntos finales específicos:

```xml
# dispatcher/src/conf.d/available_vhosts/example.vhost

<VirtualHost *:80>
    ...
    # Replace `example` with the name of your GraphQL endpoint's configuration name.
    <LocationMatch "^/graphql/execute.json/example/.*$">
        # Remove cache headers for protected endpoints so they are not cached
        Header unset Cache-Control
        Header unset Surrogate-Control
        Header set Age 0
    </LocationMatch>
    ...
</VirtualHost>
```

Tenga en cuenta que esto incurrirá en una penalización de rendimiento, ya que Dispatcher o CDN no almacenarán el contenido en caché. Se trata de un equilibrio entre rendimiento y seguridad.

## AEM Protección de puntos finales de API de GraphQL sin encabezado

AEM Esta guía no trata la protección de los [extremos de API de GraphQL sin encabezado](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/headless/graphql-api/graphql-endpoint), sino que se centra en proteger el contenido que sirven. Todos los usuarios, incluidos los usuarios anónimos, pueden acceder a los extremos que contienen contenido protegido. Solo se devuelve el contenido accesible por los grupos de usuarios cerrados del usuario. AEM Si no se puede acceder a ningún contenido, la respuesta de la API sin encabezado tendrá un código de estado de respuesta HTTP 200, pero los resultados estarán vacíos. Normalmente, la seguridad del contenido es suficiente, ya que los propios extremos no exponen inherentemente datos confidenciales. AEM Si necesita proteger los puntos de conexión, aplíqueles ACL en el Publish de la forma que le resulte más segura a través de [scripts de inicialización del repositorio de Sling (repoinit)](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).
