---
title: Vínculo de inicio de sesión personalizado de SAML 2.0
description: Aprenda a desarrollar un enlace de inicio de sesión personalizado de SAML 2.0 para AEM.
version: Experience Manager as a Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
last-substantial-update: 2025-03-11T00:00:00Z
duration: 520
source-git-commit: 34f098de6bd15875e5534250b28c08bdb62e74fa
workflow-type: tm+mt
source-wordcount: '977'
ht-degree: 0%

---


# Gancho de inicio de sesión de SAML 2.0

Aprenda a desarrollar un enlace de inicio de sesión personalizado de SAML 2.0 para AEM. Este tutorial proporciona instrucciones paso a paso para crear un enlace de inicio de sesión personalizado que se integra con un proveedor de identidad de SAML 2.0, lo que permite a los usuarios autenticarse mediante sus credenciales de SAML.

Si el IDP no puede enviar los datos del perfil de usuario y la pertenencia al grupo de usuarios en la aserción SAML, o si los datos deben transformarse antes de la sincronización a AEM, se pueden implementar enlaces SAML personalizados para ampliar el proceso de autenticación SAML. Los enlaces de SAML permiten la personalización de la asignación de la pertenencia a grupos, la modificación de los atributos del perfil de usuario y la adición de lógica empresarial personalizada durante el flujo de autenticación.

>[!NOTE]
>Los vínculos SAML personalizados son compatibles con **AEM as a Cloud Service** y **AEM LTS**. Esta función no está disponible en versiones anteriores de AEM.

## Casos de uso comunes

Los ganchos personalizados de SAML son útiles cuando es necesario:

+ Asignar dinámicamente miembros de grupo en función de la lógica empresarial personalizada más allá de lo que se proporciona en las afirmaciones de SAML
+ Transform or enrich user profile data before it&#39;s synchronized to AEM
+ Asignación de estructuras de atributos SAML complejas a propiedades de usuario de AEM
+ Implementar reglas de autorización personalizadas o asignaciones de grupos condicionales
+ Añadir el registro o la auditoría personalizados durante la autenticación SAML
+ Integración con sistemas externos durante el proceso de autenticación

## `SamlHook` interfaz de servicio OSGi

La interfaz `com.adobe.granite.auth.saml.spi.SamlHook` proporciona dos métodos de vínculo que se invocan en diferentes etapas del proceso de autenticación SAML:

### método `postSamlValidationProcess()`

Este método se denomina **después de** que la respuesta de SAML se haya validado, pero **antes de** que se inicie el proceso de sincronización del usuario. Este es el lugar ideal para modificar los datos de aserción de SAML, como agregar o transformar atributos.

```java
public void postSamlValidationProcess(
    HttpServletRequest request, 
    Assertion assertion, 
    Message samlResponse)
```

#### Casos de uso

+ Agregar pertenencias de grupo adicionales a la afirmación
+ Transformar valores de atributos antes de sincronizarlos
+ Enriquecer la aserción con datos de fuentes externas
+ Validar reglas de negocio personalizadas

### método `postSyncUserProcess()`

Este método se denomina **después de** que se haya completado el proceso de sincronización de usuarios. Este enlace se puede utilizar para realizar operaciones adicionales una vez creado o actualizado el usuario AEM.

```java
public void postSyncUserProcess(
    HttpServletRequest request, 
    HttpServletResponse response, 
    Assertion assertion,
    AuthenticationInfo authenticationInfo, 
    String samlResponse)
```

#### Casos de uso

+ Actualizar propiedades adicionales del perfil de usuario no incluidas en la sincronización estándar
+ Crear o actualizar recursos personalizados relacionados con el usuario en AEM
+ Déclencheur de flujos de trabajo o notificaciones después de la autenticación del usuario
+ Registrar eventos de autenticación personalizados

**Important:** To modify user properties in the repository, the hook implementation requires:

+ Referencia de `SlingRepository` insertada mediante `@Reference`
+ Un [usuario de servicio](https://experienceleague.adobe.com/es/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) configurado con los permisos apropiados (configurado en &quot;Modificación del servicio de asignador de usuarios del servicio de Apache Sling&quot;)
+ Administración de sesiones adecuada con bloques try-catch-finally

## Implementación de un vínculo SAML personalizado

Los siguientes pasos describen cómo crear e implementar un vínculo SAML personalizado.

### Creación de la implementación del vínculo SAML

Cree una nueva clase Java en el proyecto AEM que implemente la interfaz `com.adobe.granite.auth.saml.spi.SamlHook`:

```java
package com.mycompany.aem.saml;

import com.adobe.granite.auth.saml.spi.Assertion;
import com.adobe.granite.auth.saml.spi.Attribute;
import com.adobe.granite.auth.saml.spi.Message;
import com.adobe.granite.auth.saml.spi.SamlHook;
import org.apache.jackrabbit.api.JackrabbitSession;
import org.apache.jackrabbit.api.security.user.Authorizable;
import org.apache.jackrabbit.api.security.user.UserManager;
import org.apache.sling.auth.core.spi.AuthenticationInfo;
import org.apache.sling.jcr.api.SlingRepository;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.osgi.service.component.annotations.ReferenceCardinality;
import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.Designate;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.annotation.Nonnull;
import javax.jcr.RepositoryException;
import javax.jcr.Session;
import javax.jcr.ValueFactory;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@Component
@Designate(ocd = SampleImpl.Configuration.class, factory = true)
public class SampleImpl implements SamlHook {
    @ObjectClassDefinition(name = "Saml Sample Authentication Handler Hook Configuration")
    @interface Configuration {
        @AttributeDefinition(
                name = "idpIdentifier",
                description = "Identifier of SAML Idp. Match the idpIdentifier property's value configured in the SAML Authentication Handler OSGi factory configuration (com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>) this SAML hook will hook into"
        )
        String idpIdentifier();

    }

    private static final String SAMPLE_SERVICE_NAME = "sample-saml-service";
    private static final String CUSTOM_LOGIN_COUNT = "customLoginCount";

    private final Logger log = LoggerFactory.getLogger(getClass());

    private SlingRepository repository;

    @SuppressWarnings("UnusedDeclaration")
    @Reference(name = "repository", cardinality = ReferenceCardinality.MANDATORY)
    public void bindRepository(SlingRepository repository) {
        this.repository = repository;
    }

    /**
     * This method is called after the user sync process is completed.
     * At this point, the user has already been synchronized in OAK (created or updated).
     * Example: Track login count by adding custom attributes to the user in the repository
     *
     * @param request
     * @param response
     * @param assertion
     * @param authenticationInfo
     * @param samlResponse
     */
    @Override
    public void postSyncUserProcess(HttpServletRequest request, HttpServletResponse response, Assertion assertion,
                                    AuthenticationInfo authenticationInfo, String samlResponse) {
        log.info("Custom Audit Log: user {} successfully logged in", authenticationInfo.getUser());

        // This code executes AFTER the user has been synchronized in OAK
        // The user object already exists in the repository at this point
        Session serviceSession = null;
        try {
            // Get a service session - requires "sample-saml-service" to be configured as system user
            // Configure in: "Apache Sling Service User Mapper Service Amendment"
            serviceSession = repository.loginService(SAMPLE_SERVICE_NAME, null);

            // Get the UserManager to work with users and groups
            UserManager userManager = ((JackrabbitSession) serviceSession).getUserManager();

            // Get the authorizable (user) that just logged in
            Authorizable user = userManager.getAuthorizable(authenticationInfo.getUser());

            if (user != null && !user.isGroup()) {
                ValueFactory valueFactory = serviceSession.getValueFactory();

                // Increment login count
                long loginCount = 1;
                if (user.hasProperty(CUSTOM_LOGIN_COUNT)) {
                    loginCount = user.getProperty(CUSTOM_LOGIN_COUNT)[0].getLong() + 1;
                }
                user.setProperty(CUSTOM_LOGIN_COUNT, valueFactory.createValue(loginCount));
                log.debug("Set {} property to {} for user {}", CUSTOM_LOGIN_COUNT, loginCount, user.getID());

                // Save all changes to the repository
                if (serviceSession.hasPendingChanges()) {
                    serviceSession.save();
                    log.debug("Successfully saved custom attributes for user {}", user.getID());
                }
            } else {
                log.warn("User {} not found or is a group", authenticationInfo.getUser());
            }

        } catch (RepositoryException e) {
            log.error("Error adding custom attributes to user repository for user: {}",
                     authenticationInfo.getUser(), e);
        } finally {
            if (serviceSession != null) {
                serviceSession.logout();
            }
        }
    }

    /**
     * This method is called after the SAML response is validated but before the user sync process starts.
     * We can modify the assertion here to add custom attributes.
     *
     * @param request
     * @param assertion
     * @param samlResponse
     */
    @Override
    public void postSamlValidationProcess(@Nonnull HttpServletRequest request, @Nonnull Assertion assertion, @Nonnull Message samlResponse) {
        // Add the attribute "memberOf" with value "sample-group" to the assertion
        // In this example "memberOf" is a multi-valued attribute that contains the groups from the Saml Idp
        log.debug("Inside postSamlValidationProcess");
        Attribute groupsAttr = assertion.getAttributes().get("groups");
        if (groupsAttr != null) {
            groupsAttr.addAttributeValue("sample-group-from-hook");
        } else {
            groupsAttr = new Attribute();
            groupsAttr.setName("groups");
            groupsAttr.addAttributeValue("sample-group-from-hook");
            assertion.getAttributes().put("groups", groupsAttr);
        }
    }

}
```

### Configuración del vínculo de SAML

El vínculo SAML utiliza la configuración OSGi para especificar a qué IDP debe aplicarse. Cree un archivo de configuración OSGi en el proyecto en:

`/ui.config/src/main/content/jcr_root/wknd-examples/osgiconfig/config.publish/com.mycompany.aem.saml.CustomSamlHook~okta.cfg.json`

```json
{
  "idpIdentifier": "$[env:SAML_IDP_ID;default=http://www.okta.com/exk4z55r44Jz9C6am5d7]",
  "service.ranking": 100
}
```

El `idpIdentifier` debe coincidir con el valor `idpIdentifier` configurado en la configuración de fábrica correspondiente de OSGi del controlador de autenticación SAML (PID: `com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>.cfg.json`). Esta coincidencia es crítica: el vínculo SAML solo se invocará para la instancia del controlador de autenticación SAML que tenga el mismo valor `idpIdentifier`. El controlador de autenticación SAML es una configuración de fábrica, lo que significa que puede tener varias instancias (por ejemplo, `com.adobe.granite.auth.saml.SamlAuthenticationHandler~okta.cfg.json`, `com.adobe.granite.auth.saml.SamlAuthenticationHandler~azure.cfg.json`) y cada vínculo está vinculado a un controlador específico a través de `idpIdentifier`. La propiedad `service.ranking` controla el orden de ejecución cuando se configuran varios enlaces (primero se ejecutan los valores más altos).

### Añadir dependencias de Maven

Agregue la dependencia SAML SPI necesaria al `pom.xml` del proyecto principal de AEM Maven.

**Para proyectos de AEM as a Cloud Service**, use la dependencia de la API de AEM SDK que incluye las interfaces de SAML:

```xml
<dependency>
    <groupId>com.adobe.aem</groupId>
    <artifactId>aem-sdk-api</artifactId>
    <version>${aem.sdk.api}</version>
    <scope>provided</scope>
</dependency>
```

El artefacto `aem-sdk-api` contiene todas las interfaces SAML de Adobe Granite necesarias, incluida `com.adobe.granite.auth.saml.spi.SamlHook`.

### Configuración del usuario del servicio (opcional)

Si el vínculo SAML necesita modificar el contenido en el repositorio JCR de AEM, como las propiedades del usuario (como se muestra en el ejemplo `postSyncUserProcess`), debe configurarse un [usuario de servicio](https://experienceleague.adobe.com/es/docs/experience-manager-learn/cloud-service/developing/advanced/service-users):

1. Cree una asignación de usuario de servicio en el proyecto en `/ui.config/src/main/content/jcr_root/apps/myproject/osgiconfig/config/org.apache.sling.serviceusermapping.impl.ServiceUserMapperImpl.amended~saml.cfg.json`:

```json
{
  "user.mapping": [
    "com.mycompany.aem.core:sample-saml-service=saml-hook-service"
  ]
}
```

1. Cree un script de repoinit para definir el usuario del servicio y los permisos en `/ui.config/src/main/content/jcr_root/apps/myproject/osgiconfig/config/org.apache.sling.jcr.repoinit.RepositoryInitializer~saml.cfg.json`:

```
create service user saml-hook-service with path system/saml

set ACL for saml-hook-service
    allow jcr:read,rep:write,rep:userManagement on /home/users
end
```

Esto otorga al servicio permisos de usuario para leer y modificar las propiedades de usuario en el repositorio.

### Implementación en AEM

Implementar el enlace de SAML personalizado en AEM como Cloud Service:

1. Generar el proyecto AEM
1. Transfiera el código al repositorio de Git de Cloud Manager
1. Implementar mediante una canalización de implementación de pila completa
1. El vínculo de SAML se activará automáticamente cuando un usuario se autentique mediante SAML


### Consideraciones importantes

+ **Identificador de IDP coincidente**: el `idpIdentifier` configurado en el enlace de SAML debe coincidir exactamente con el `idpIdentifier` de la configuración de fábrica del Controlador de autenticación de SAML (`com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>`)
+ **Nombres de atributos**: asegúrese de que los nombres de atributos a los que se hace referencia en el enlace (p. ej., `groupMembership`) coincidan con los atributos configurados en el Controlador de autenticación de SAML
+ **Rendimiento**: mantén las implementaciones de enlaces ligeras mientras se ejecutan durante cada autenticación de SAML
+ **Control de errores**: las implementaciones del enlace de SAML deben producir `com.adobe.granite.auth.saml.spi.SamlHookException` cuando se produzcan errores críticos que no superen la autenticación. El controlador de autenticación de SAML detectará estas excepciones y devolverá `AuthenticationInfo.FAIL_AUTH`. Para las operaciones del repositorio, detecte siempre `RepositoryException` y registre los errores de forma adecuada. Utilice bloques try-catch-finally para garantizar una limpieza adecuada de los recursos
+ **Pruebas**: Pruebe exhaustivamente los vínculos personalizados en entornos inferiores antes de implementarlos en producción
+ **Varios enlaces**: Se pueden configurar varias implementaciones de enlaces de SAML; se ejecutarán todos los enlaces coincidentes. Utilice la propiedad `service.ranking` en el componente OSGi para controlar el orden de ejecución (los valores de mayor clasificación se ejecutan primero). Para reutilizar un vínculo de SAML en varias configuraciones de fábrica del controlador de autenticación SAML (`com.adobe.granite.auth.saml.SamlAuthenticationHandler~<unique-id>`), cree varias configuraciones de vínculo (configuraciones de fábrica OSGi), cada una con un `idpIdentifier` diferente que coincida con el controlador de autenticación SAML correspondiente
+ **Security**: Validate and sanitize all data from SAML assertions before using them in business logic
+ **Repository access**: When modifying user properties in `postSyncUserProcess`, always use a [service user](https://experienceleague.adobe.com/es/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) with appropriate permissions rather than administrative sessions
+ **Permisos de usuario de servicio**: Conceda los permisos mínimos necesarios al [usuario de servicio](https://experienceleague.adobe.com/es/docs/experience-manager-learn/cloud-service/developing/advanced/service-users) (por ejemplo, solo `jcr:read` y `rep:write` en `/home/users`, no derechos de administrador completos)
+ **Administración de sesiones**: Use siempre bloques try-catch-finally para asegurarse de que las sesiones del repositorio se cierren correctamente, incluso si se producen excepciones
+ **Sincronización del usuario**: el vínculo `postSyncUserProcess` se ejecuta después de sincronizar el usuario con OAK, por lo que se garantiza que el objeto de usuario existe en el repositorio en ese momento
