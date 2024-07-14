---
title: Uso de recursos de Adobe Stock con AEM Assets
description: AEM La función de búsqueda de recursos de Adobe Stock AEM permite a los usuarios buscar, previsualizar, guardar y obtener licencias directamente desde los recursos de la aplicación de la. Las organizaciones ahora pueden integrar su plan empresarial de Adobe Stock con AEM Assets AEM para asegurarse de que los recursos con licencia ahora están disponibles en gran medida para sus proyectos creativos y de marketing, con las potentes capacidades de administración de recursos de los que dispone el usuario.
feature: Adobe Stock
version: 6.5
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-06-26T00:00:00Z
doc-type: Feature Video
exl-id: a3c3a01e-97a6-494f-b7a9-22057e91f4eb
duration: 1079
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '861'
ht-degree: 1%

---

# Uso de Adobe Stock con AEM Assets{#using-adobe-stock-assets-with-aem-assets}

AEM 6.4.2 permite a los usuarios buscar, previsualizar, guardar y obtener licencias de recursos de Adobe Stock AEM directamente desde los recursos de los que dispone de una licencia de usuario de. Las organizaciones ahora pueden integrar su plan empresarial de Adobe Stock con AEM Assets AEM para asegurarse de que los recursos con licencia ahora están disponibles en gran medida para sus proyectos creativos y de marketing, con las potentes capacidades de administración de recursos de los que dispone el usuario.

>[!VIDEO](https://video.tv.adobe.com/v/24678?quality=12&learn=on)

>[!NOTE]
>
>La integración requiere un [plan Adobe Stock AEM empresarial](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) y un plan de trabajo 6.4 con al menos Service Pack 2 implementado. AEM Para obtener detalles del paquete de servicio 6.4 de la versión, consulte estas [notas de la versión](https://helpx.adobe.com/es/experience-manager/6-4/release-notes/sp-release-notes.html).

La integración de Adobe Stock y AEM Assets permite a los autores y especialistas en marketing de contenido obtener licencias fácilmente y utilizar recursos de stock para fines creativos o de marketing. Puede realizar una búsqueda de recursos de Stock mediante Omni Search, añadiendo el filtro de ubicación como Adobe Stock o navegando por la navegación principal de AEM Assets y haciendo clic en el icono Buscar en la IU de Adobe Stock Coral.

## Capacidades

### Buscar y guardar

* Realice una búsqueda de recursos de Adobe Stock AEM sin salir de espacio de trabajo de la.
* Guarde los recursos de Adobe Stock para previsualizarlos sin licenciar el recurso.
* Capacidad para obtener licencias y guardar recursos de Adobe Stock en AEM Assets
* Capacidad para buscar recursos similares de Adobe Stock en la interfaz de usuario de AEM Assets
* Ver un recurso seleccionado en Búsqueda de stock en AEM Assets en el sitio web de Adobe Stock
* Los archivos de recursos con licencia están marcados con un distintivo azul para facilitar la identificación

### Metadatos de recurso

* El recurso con licencia se almacena en AEM Assets. Las propiedades del recurso contienen metadatos de Stock en una pestaña de metadatos de recurso independiente
* Capacidad para añadir referencias de licencia a metadatos de recursos

### Perfil de stock de recursos

* Un usuario puede seleccionar el perfil de Adobe Stock en *Usuario > Mis preferencias > Configuración de Stock*
* Se pueden agregar referencias obligatorias y opcionales a la ventana Licencias de recursos.
* Capacidad para elegir las preferencias de idioma para la ventana Licencias de recursos en función de la región.

### Filter

* Un usuario puede filtrar los recursos de stock en función del tipo de recurso, la orientación y la vista similares
* El tipo de recurso incluye fotos, ilustraciones, vectores, vídeos, plantillas, 3D, Premium, editorial
* La orientación incluye Horizontal, Vertical y Cuadrado.
* Ver Un filtro similar requiere Adobe Stock Número de archivo

### Control de acceso

* Los administradores pueden proporcionar permisos a determinados usuarios/grupos para obtener licencias de recursos de stock al configurar el servicio en la nube de Adobe Stock.
* Si un usuario o grupo específico no tiene permiso para obtener licencia de recursos de existencias, se deshabilitaría la capacidad *Búsqueda de recursos de existencias / Licencias de recursos*.

## Configuración de Adobe Stock con AEM Assets{#set-up-adobe-stock-with-aem-assets}

AEM 6.4.2 permite a los usuarios buscar, previsualizar, guardar y obtener licencias de recursos de Adobe Stock AEM directamente desde los recursos de los que dispone de una licencia de usuario de. Este vídeo explica cómo configurar las existencias de Adobe con AEM Assets mediante la consola de Adobe I/O.

>[!VIDEO](https://video.tv.adobe.com/v/25043?quality=12&learn=on)

>[!NOTE]
>
>Para la configuración del servicio en la nube de Adobe Stock, debe seleccionar el entorno de producción y la ruta del recurso con licencia apuntar a `/content/dam`. AEM El campo Entorno ahora se ha eliminado en la.

>[!NOTE]
>
>La integración requiere un [plan Adobe Stock AEM empresarial](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) y un plan de trabajo 6.4 con al menos [Service Pack 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aaem%2F6-4&amp;3_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-aggregate-fix&amp;orderby=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort=asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24) implementado. AEM Para obtener detalles del paquete de servicio 6.4 de la versión, consulte estas [notas de la versión](https://helpx.adobe.com/es/experience-manager/6-4/release-notes/sp-release-notes.html). También necesita permisos de administrador en [Consola de Adobe I/O](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) y Adobe Experience Manager para configurar la integración.

### Instalación {#installations}

* AEM AEM Para la versión 6.4, debe instalar el paquete de servicio 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aaem%2F6-4&amp;3_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-aggregate-fix&amp;orderby=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort=asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24) de [y, a continuación, volver a instalar el archivo cq-dam-stock-integration-content-1.0.4.zip.
* Asegúrese de tener permisos de administrador en [Consola de Adobe I/O](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) y Adobe Experience Manager para configurar la integración.

#### Configuración de Adobe IMS mediante la consola de Adobe I/O {#set-up-adobe-ims-configuration-using-adobe-i-o-console}

1. Cree una configuración de cuenta técnica de Adobe IMS en **Herramientas > Seguridad**
2. Seleccione la *solución de nube* como *Adobe Stock* y cree un nuevo certificado o reutilice un certificado existente para la configuración.
3. Vaya a la consola de Adobe I/O y cree una nueva integración de cuenta de servicio para *Adobe Stock*.
4. Cargue el certificado del paso 2 a la integración de cuenta de servicio de Adobe Stock.
5. Elija la configuración del perfil de Adobe Stock necesaria y complete la integración del servicio.
6. Utilice los detalles de la integración para completar la configuración de la cuenta técnica de Adobe IMS
7. Asegúrese de que puede recibir el token de acceso mediante la cuenta técnica de IMS de Adobe.

![Cuenta técnica de Adobe IMS](assets/screen_shot_2018-10-22at12219pm.png)

#### Configuración de Cloud Service de Adobe Stock {#set-up-adobe-stock-cloud-services}

1. Cree una nueva configuración de servicio en la nube para Adobe Stock en **Herramientas > Cloud Service.**
2. Seleccione la *configuración de Adobe IMS* creada en la sección anterior para su configuración de *Adobe Stock Cloud*

3. Asegúrese de seleccionar **ENTORNO** como PROD.
4. **La ruta del recurso con licencia** puede apuntar a cualquier directorio de `/content/dam`.
5. Seleccione la configuración regional y complete la configuración.
6. También puede agregar usuarios/grupos a su servicio en la nube de Adobe Stock para habilitar el acceso a usuarios o grupos específicos.

![Configuración de Assets Stock de Adobe](assets/screen_shot_2018-10-22at12425pm.png)

### Recursos adicionales

* [Plan de Stock Empresarial](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* AEM [Notas de la versión del paquete de servicio 2 de.4](https://experienceleague.adobe.com/docs/experience-manager-65/release-notes/release-notes.html?lang=es)
* AEM [Integrar el código de tiempo de la y Adobe Stock](https://experienceleague.adobe.com/docs/experience-manager-65/assets/using/aem-assets-adobe-stock.html)
* [API de integración de consola de Adobe I/O](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Documentos de API de Adobe Stock](https://www.adobe.io/apis/creativecloud/stock/docs.html)
