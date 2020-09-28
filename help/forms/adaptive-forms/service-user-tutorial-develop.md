---
title: Desarrollo con usuarios de servicios en AEM Forms
seo-title: Desarrollo con usuarios de servicios en AEM Forms
description: Este artículo lo acompaña durante el proceso de creación de un usuario de servicio en AEM Forms
seo-description: Este artículo lo acompaña durante el proceso de creación de un usuario de servicio en AEM Forms
uuid: 996f30df-3fc5-4232-a104-b92e1bee4713
feature: adaptive-forms
topics: development,administration
audience: implementer,developer
doc-type: article
activity: setup
discoiquuid: 65bd4695-e110-48ba-80ec-2d36bc53ead2
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---


# Desarrollo con usuarios de servicios en AEM Forms

Este artículo lo acompaña durante el proceso de creación de un usuario de servicio en AEM Forms

En versiones anteriores de Adobe Experience Manager (AEM), la resolución de recursos administrativos se utilizaba para el procesamiento back-end, que requería acceso al repositorio. El uso de la resolución de recursos administrativos está obsoleto en AEM 6.3. En su lugar, se utiliza un usuario del sistema con permisos específicos en el repositorio.

En este artículo se explica la creación de un usuario del sistema y la configuración de las propiedades del asignador de usuarios.

1. Vaya a [http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp)
1. Iniciar sesión como &#39; admin &#39;
1. Haga clic en &#39; Administración de usuarios &#39;
1. Haga clic en &#39; Crear usuario del sistema &#39;
1. Configure el tipo userid como &#39; data &#39; y haga clic en el icono verde para completar el proceso de creación del usuario del sistema
1. [Abrir configMgr](http://localhost:4502/system/console/configMgr)
1. Busque &#39; Servicio de asignación de usuarios del servicio Apache Sling &#39; y haga clic para abrir las propiedades
1. Haga clic en el icono *+* (más) para agregar la siguiente asignación de servicios

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. Haga clic en &#39; Guardar &#39;

En la configuración anterior, DevelopingWithServiceUser.core es el nombre simbólico del paquete. getresources.esolver es el nombre del subservicio. data es el usuario del sistema creado en el paso anterior.

También podemos obtener la resolución de recursos en nombre del usuario de fd-service. Este usuario de servicio se utiliza para servicios de documento. Por ejemplo, si desea certificar/aplicar derechos de uso, etc., usaremos la resolución de recursos del usuario de fd-service para realizar las operaciones

1. [Descargue y descomprima el archivo zip asociado con este artículo.](assets/developingwithserviceuser.zip)
1. Vaya a [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)
1. Carga y inicio del paquete OSGi
1. Asegúrese de que el paquete está en estado activo
1. Ahora ha creado correctamente un usuario ** del sistema y también ha implementado el paquete *de usuario del servicio*.

   Para proporcionar acceso a /content, otorgue al usuario del sistema (&#39; datos &#39;) permisos de lectura en el nodo de contenido.

   1. Vaya a [http://localhost:4502/useradmin](http://localhost:4502/useradmin)
   1. Buscar los datos del usuario &#39;. Es el mismo usuario del sistema que creó en el paso anterior.
   1. Haga clic con el botón doble en el usuario y, a continuación, haga clic en la ficha &#39; Permisos &#39;
   1. Proporcione acceso de lectura a la carpeta &#39;content&#39;.
   1. Para utilizar el usuario del servicio para obtener acceso a la carpeta /content, utilice el siguiente código

   ```java
   com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
   
   resourceResolver = aemDemoListings.getServiceResolver();
   
   // get the resource. This will succeed because we have given ' read ' access to the content node
   
   Resource contentResource = resourceResolver.getResource("/content/forms/af/sandbox/abc.pdf");
   ```

   Si desea acceder al archivo /content/dam/data.json del paquete, debe utilizar el código siguiente. Este código supone que ha concedido permisos de lectura al usuario &quot;data&quot; en el nodo /content/dam/

   ```java
   @Reference
   GetResolver getResolver;
   .
   .
   .
   ResourceResolver serviceResolver = getResolver.getServiceResolver();
   // to get resource resolver specific to fd-service user. This is for Document Services
   ResourceResolver fdserviceResolver = getResolver.getFormsServiceResolver();
   Node resNode = getResolver.getServiceResolver().getResource("/content/dam/data.json").adaptTo(Node.class);
   ```

   A continuación se proporciona el código completo de la implementación

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
     HashMap<String, Object> param = new HashMap<String, Object>();
     param.put(ResourceResolverFactory.SUBSERVICE, "getresourceresolver");
     ResourceResolver resolver = null;
     try {
      resolver = resolverFactory.getServiceResourceResolver(param);
     } catch (LoginException e) {
      // TODO Auto-generated catch block
      e.printStackTrace();
     }
     return resolver;
    }
   ```

