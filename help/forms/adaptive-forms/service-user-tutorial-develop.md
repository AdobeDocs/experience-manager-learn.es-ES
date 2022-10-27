---
title: Desarrollo con usuarios de servicios en AEM Forms
description: Este artículo lo acompaña durante el proceso de creación de un usuario de servicio en AEM Forms
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 5fa3d52a-6a71-45c4-9b1a-0e6686dd29bc
last-substantial-update: 2020-09-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '445'
ht-degree: 1%

---

# Desarrollo con usuarios de servicios en AEM Forms

Este artículo lo acompaña durante el proceso de creación de un usuario de servicio en AEM Forms

En versiones anteriores de Adobe Experience Manager (AEM), la resolución de recursos administrativos se utilizaba para el procesamiento back-end que requería acceso al repositorio. El uso de la resolución de recursos administrativos está obsoleto en AEM 6.3. En su lugar, se utiliza un usuario del sistema con permisos específicos en el repositorio.

Obtenga más información sobre los detalles de [creación y uso de usuarios de servicios en AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/advanced/service-users.html).

Este artículo explica la creación de un usuario del sistema y la configuración de las propiedades del asignador de usuarios.

1. Vaya a [http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp)
1. Inicie sesión como &#39; admin &#39;
1. Haga clic en &quot;Administración de usuarios&quot;
1. Haga clic en &quot;Crear usuario del sistema&quot;
1. Establezca el tipo userid como &#39; data &#39; y haga clic en el icono verde para completar el proceso de creación del usuario del sistema
1. [Abrir configMgr](http://localhost:4502/system/console/configMgr)
1. Buscar _Servicio de asignador de usuarios del servicio Apache Sling_ y haga clic en para abrir las propiedades
1. Haga clic en el *+* icono (más) para añadir la siguiente asignación de servicios

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. Haga clic en &#39; Guardar &#39;

En la configuración anterior, DevelopingWithServiceUser.core es el nombre simbólico del paquete. getresourceresolver es el subservicio name.data es el usuario del sistema creado en el paso anterior.

También podemos obtener la resolución de recursos en nombre del usuario de fd-service. Este usuario de servicio se utiliza para document services. Por ejemplo, si desea certificar/aplicar derechos de uso, etc., utilizaremos la resolución de recursos del usuario de fd-service para realizar las operaciones

1. [Descargue y descomprima el archivo zip asociado con este artículo.](assets/developingwithserviceuser.zip)
1. Vaya a [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)
1. Carga e inicio del paquete OSGi
1. Asegúrese de que el paquete esté en estado activo
1. Ya ha creado correctamente un *Usuario del sistema* y también implementó el *Paquete de usuario de servicio*.

   Para proporcionar acceso a /content, otorgue al usuario del sistema (&#39; data &#39;) permisos de lectura en el nodo de contenido.

   1. Vaya a [http://localhost:4502/useradmin](http://localhost:4502/useradmin)
   1. Buscar los datos del usuario &#39; &#39;. Este es el mismo usuario del sistema que creó en el paso anterior.
   1. Haga doble clic en el usuario y, a continuación, haga clic en la ficha Permisos
   1. Proporcione acceso de lectura a la carpeta &quot;contenido&quot;.
   1. Para utilizar el usuario del servicio para obtener acceso a la carpeta /content, utilice el siguiente código



```java
com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
   
resourceResolver = aemDemoListings.getServiceResolver();
   
// get the resource. This will succeed because we have given ' read ' access to the content node
   
Resource contentResource = resourceResolver.getResource("/content/forms/af/sandbox/abc.pdf");
```

Si desea acceder al archivo /content/dam/data.json del paquete, utilice el siguiente código. Este código supone que ha dado permisos de lectura al usuario &quot;data&quot; en el nodo /content/dam/

```java
@Reference
GetResolver getResolver;
.
.
.
try {
   ResourceResolver serviceResolver = getResolver.getServiceResolver();

   // To get resource resolver specific to fd-service user. This is for Document Services
   ResourceResolver fdserviceResolver = getResolver.getFormsServiceResolver();

   Node resNode = getResolver.getServiceResolver().getResource("/content/dam/data.json").adaptTo(Node.class);
} catch(LoginException ex) {
   // Unable to get the service user, handle this exception as needed
} finally {
  // Always close all ResourceResolvers you open!
  
  if (serviceResolver != null( { serviceResolver.close(); }
  if (fdserviceResolver != null) { fdserviceResolver.close(); }
}
```

A continuación se muestra el código completo de la implementación

```java
package com.mergeandfuse.getserviceuserresolver.impl;
import java.util.HashMap;
import org.apache.sling.api.resource.LoginException;
import org.apache.sling.api.resource.ResourceResolver;
import org.apache.sling.api.resource.ResourceResolverFactory;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import com.mergeandfuse.getserviceuserresolver.GetResolver;
@Component(service = GetResolver.class)
public class GetResolverImpl implements GetResolver {
        @Reference
        ResourceResolverFactory resolverFactory;

        @Override
        public ResourceResolver getServiceResolver() {
                System.out.println("#### Trying to get service resource resolver ....  in my bundle");
                HashMap < String, Object > param = new HashMap < String, Object > ();
                param.put(ResourceResolverFactory.SUBSERVICE, "getresourceresolver");
                ResourceResolver resolver = null;
                try {
                        resolver = resolverFactory.getServiceResourceResolver(param);
                } catch (LoginException e) {

                        System.out.println("Login Exception " + e.getMessage());
                }
                return resolver;

        }

        @Override
        public ResourceResolver getFormsServiceResolver() {

                System.out.println("#### Trying to get Resource Resolver for forms ....  in my bundle");
                HashMap < String, Object > param = new HashMap < String, Object > ();
                param.put(ResourceResolverFactory.SUBSERVICE, "getformsresourceresolver");
                ResourceResolver resolver = null;
                try {
                        resolver = resolverFactory.getServiceResourceResolver(param);
                } catch (LoginException e) {
                        System.out.println("Login Exception ");
                        System.out.println("The error message is " + e.getMessage());
                }
                return resolver;
        }

}
```
