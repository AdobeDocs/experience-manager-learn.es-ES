---
title: Desarrollo con usuarios de servicios en AEM Forms
seo-title: Desarrollo con usuarios de servicios en AEM Forms
description: Este artículo lo acompaña durante el proceso de creación de un usuario de servicio en AEM Forms
seo-description: Este artículo lo acompaña durante el proceso de creación de un usuario de servicio en AEM Forms
uuid: 996f30df-3fc5-4232-a104-b92e1bee4713
feature: formularios adaptables
topics: development,administration
audience: implementer,developer
doc-type: article
activity: setup
discoiquuid: 65bd4695-e110-48ba-80ec-2d36bc53ead2
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '447'
ht-degree: 0%

---


# Desarrollo con usuarios de servicios en AEM Forms

Este artículo lo acompaña durante el proceso de creación de un usuario de servicio en AEM Forms

En versiones anteriores de Adobe Experience Manager (AEM), la resolución de recursos administrativos se utilizaba para el procesamiento back-end que requería acceso al repositorio. El uso de la resolución de recursos administrativos está en desuso en AEM 6.3. En su lugar, se utiliza un usuario del sistema con permisos específicos en el repositorio.

Este artículo explica la creación de un usuario del sistema y la configuración de las propiedades del asignador de usuarios.

1. Vaya a [http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp)
1. Inicie sesión como &#39; admin &#39;
1. Haga clic en &quot;Administración de usuarios&quot;
1. Haga clic en &quot;Crear usuario del sistema&quot;
1. Establezca el tipo userid como &#39; data &#39; y haga clic en el icono verde para completar el proceso de creación del usuario del sistema
1. [Abrir configMgr](http://localhost:4502/system/console/configMgr)
1. Busque el servicio Apache Sling Service User Mapper y haga clic en para abrir las propiedades
1. Haga clic en el icono *+* (signo más) para agregar la siguiente asignación de servicios

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. Haga clic en &#39; Guardar &#39;

En la configuración anterior, DevelopingWithServiceUser.core es el nombre simbólico del paquete. getresourceresolver es el subservicio name.data es el usuario del sistema creado en el paso anterior.

También podemos obtener la resolución de recursos en nombre del usuario de fd-service. Este usuario de servicio se utiliza para document services. Por ejemplo, si desea certificar/aplicar derechos de uso, etc., utilizaremos la resolución de recursos del usuario de fd-service para realizar las operaciones

1. [Descargue y descomprima el archivo zip asociado con este artículo.](assets/developingwithserviceuser.zip)
1. Vaya a [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)
1. Carga e inicio del paquete OSGi
1. Asegúrese de que el paquete esté en estado activo
1. Ahora ha creado correctamente un *System User* y también ha implementado el *Service User bundle*.

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
   ResourceResolver serviceResolver = getResolver.getServiceResolver();
   // to get resource resolver specific to fd-service user. This is for Document Services
   ResourceResolver fdserviceResolver = getResolver.getFormsServiceResolver();
   Node resNode = getResolver.getServiceResolver().getResource("/content/dam/data.json").adaptTo(Node.class);
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

