---
title: Uso de recursos de Adobe Stock con AEM Assets
description: AEM permite a los usuarios buscar, previsualizar, guardar y conceder licencias sobre los recursos de Adobe Stock directamente desde AEM. Las organizaciones ahora pueden integrar su plan Adobe Stock Enterprise con AEM Assets para asegurarse de que los recursos con licencia estén ahora disponibles para sus proyectos creativos y de marketing, con las potentes funciones de administración de recursos de AEM.
feature: Adobe Stock
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: a3c3a01e-97a6-494f-b7a9-22057e91f4eb
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '977'
ht-degree: 3%

---

# Uso de Adobe Stock con AEM Assets{#using-adobe-stock-assets-with-aem-assets}

AEM 6.4.2 proporciona a los usuarios la capacidad de buscar, previsualizar, guardar y conceder licencias sobre los recursos de Adobe Stock directamente desde AEM. Las organizaciones ahora pueden integrar su plan Adobe Stock Enterprise con AEM Assets para asegurarse de que los recursos con licencia estén ahora disponibles para sus proyectos creativos y de marketing, con las potentes funciones de administración de recursos de AEM.

>[!VIDEO](https://video.tv.adobe.com/v/24678/?quality=12&learn=on)

>[!NOTE]
>
>La integración requiere un [plan de enterprise Adobe Stock](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) y AEM 6.4 con al menos el Service Pack 2 implementado. Para obtener más información sobre AEM Service Pack 6.4, consulte estos [notas de la versión](https://helpx.adobe.com/es/experience-manager/6-4/release-notes/sp-release-notes.html).

La integración de Adobe Stock y AEM Assets permite que los autores y especialistas en marketing de contenido obtengan fácilmente licencias y utilicen recursos de existencias para fines creativos o de marketing. Puede realizar una búsqueda de recursos de existencias mediante la búsqueda Omni, agregando el filtro de ubicación como Adobe Stock o navegando por la navegación principal de AEM Assets y haciendo clic en el icono Buscar en la interfaz de usuario de Adobe Stock Coral .

## Capacidades

### Buscar y guardar

* Realice una búsqueda de recursos de Adobe Stock sin abandonar AEM espacio de trabajo.
* Guarde los recursos de Adobe Stock para su previsualización, sin necesidad de conceder licencias al recurso.
* Capacidad para obtener licencias y guardar recursos de Adobe Stock en AEM Assets
* Capacidad de buscar recursos similares desde Adobe Stock en la interfaz de usuario de AEM Assets
* Ver un recurso seleccionado de Búsqueda de stock en AEM Assets en el sitio web de Adobe Stock
* Los archivos de recursos con licencia están marcados con un distintivo azul con licencia para facilitar la identificación

### Metadatos de recurso

* Los recursos con licencia se almacenan en AEM Assets. Las propiedades de los recursos contienen metadatos de stock en una pestaña de metadatos de un recurso independiente
* Posibilidad de agregar referencias de licencia a metadatos de recursos

### Perfil de Asset Stock

* Un usuario puede seleccionar un perfil de Adobe Stock en *Usuario > Mis preferencias > Configuración de Stock*
* Se pueden agregar referencias opcionales y obligatorias a la ventana Licencias de recursos.
* Posibilidad de elegir la preferencia de idioma para la ventana de licencias de recursos en función de la región.

### Filtro

* Un usuario puede filtrar los recursos de stock en función del tipo de recurso, la orientación y la vista similares
* El tipo de recurso incluye fotos, ilustraciones, vectores, vídeos, plantillas, 3D, Premium, editoriales
* La orientación incluye Horizontal, Vertical y Cuadrado.
* Ver un filtro similar requiere un número de archivo de Adobe Stock

### Control de acceso

* Los administradores pueden proporcionar permisos a determinados usuarios o grupos para obtener una licencia de los recursos de existencias al configurar la configuración del servicio en la nube de Adobe Stock.
* Si un usuario o grupo específico no tiene permiso para obtener una licencia para los recursos de stock, *Stock Asset Search / Licencias de activos* se deshabilitaría.

## Configuración de Adobe Stock con AEM Assets{#set-up-adobe-stock-with-aem-assets}

AEM 6.4.2 proporciona a los usuarios la capacidad de buscar, previsualizar, guardar y conceder licencias sobre los recursos de Adobe Stock directamente desde AEM. En este vídeo se explica cómo configurar existencias de Adobe con AEM Assets mediante la consola de Adobe I/O.

>[!VIDEO](https://video.tv.adobe.com/v/25043/?quality=12&learn=on)

>[!NOTE]
>
>Para la configuración de Adobe Stock Cloud Service, debe seleccionar el entorno de producción y la ruta de acceso de los recursos con licencia para que apunten a `/content/dam`. El campo Entorno ahora se elimina en AEM.

>[!NOTE]
>
>La integración requiere un [plan de enterprise Adobe Stock](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html) y AEM 6.4 con al menos [Service Pack 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aaem%2F6-4&amp;3_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24) implementado. Para obtener más información sobre AEM Service Pack 6.4, consulte estos [notas de la versión](https://helpx.adobe.com/experience-manager/6-4/release-notes/sp-release-notes.html). También necesita permisos de administrador para [Consola Adobe I/O](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) y Adobe Experience Manager para configurar la integración.

### Instalación {#installations}

* Para AEM 6.4, debe instalar el [AEM Service Pack 2](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?fulltext=AEM*+6*+4*+Service*+Pack*&amp;2_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3Aversion&amp;2_group.propertyvalues.operation=equals&amp;2_group.propertyvalues.0_values=target-version%3Aaem%2F6-4&amp;3_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;3_group.propertyvalues.operation=equals&amp;3_group.propertyvalues.0_values=software-type%3Aservice-and-cumulative-fix&amp;orderby=%40jcr%3Acontent%2Fmetadata%2Fdc%3Atitle&amp;orderby.sort asc&amp;layout=list&amp;p.offset=0&amp;p.limit=24) y luego reinstale el archivo cq-dam-stock-integration-content-1.0.4.zip.
* Asegúrese de que tiene permisos de administrador en [Consola Adobe I/O](https://console.adobe.io/), [Adobe Admin Console](https://adminconsole.adobe.com/) y Adobe Experience Manager para configurar la integración.

#### Configuración de Adobe IMS mediante la consola Adobe I/O {#set-up-adobe-ims-configuration-using-adobe-i-o-console}

1. Cree una configuración de cuenta técnica de Adobe IMS en **Herramientas > Seguridad**
2. Seleccione el *Solución en la nube* como *Adobe Stock* y cree un nuevo certificado o vuelva a utilizar un certificado existente para la configuración.
3. Vaya a la consola Adobe I/O y cree una nueva integración de cuenta de servicio para *Adobe Stock*.
4. Cargue el certificado del paso 2 a su integración de cuenta de servicio de Adobe Stock.
5. Elija la configuración de perfil de Adobe Stock necesaria y complete la integración de servicio.
6. Utilice los detalles de integración para completar la configuración de la cuenta técnica de Adobe IMS
7. Asegúrese de que puede recibir el token de acceso mediante la cuenta técnica de Adobe IMS.

![Cuenta técnica de IMS de Adobe](assets/screen_shot_2018-10-22at12219pm.png)

#### Configuración de Cloud Services de Adobe Stock {#set-up-adobe-stock-cloud-services}

1. Cree una nueva configuración de Cloud Service para Adobe Stock en **Herramientas > Cloud Services.**
2. Seleccione el *Configuración de Adobe IMS* creado en la sección anterior para su *Adobe Stock Cloud* configuración

3. Asegúrese de seleccionar la variable **ENTORNO** como PROD.
4. **Ruta del recurso con licencia** puede apuntar a cualquier directorio de `/content/dam`.
5. Seleccione la configuración regional y complete la configuración.
6. También puede agregar usuarios/grupos a su servicio de Adobe Stock Cloud para habilitar el acceso para usuarios o grupos específicos.

![Configuración de Stock de Recursos de Adobe](assets/screen_shot_2018-10-22at12425pm.png)

### Recursos adicionales

* [Plan de stock empresarial](https://landing.adobe.com/en/na/products/creative-cloud/ctir-4625-stock-for-enterprise/index.html)
* [Notas de la versión de AEM 6.4 Service Pack 2](https://experienceleague.adobe.com/docs/experience-manager-64/release-notes/sp-release-notes.html?lang=es)
* [Integración de AEM y Adobe Stock](https://experienceleague.adobe.com/docs/experience-manager-65/assets/using/aem-assets-adobe-stock.html)
* [API de integración de la consola de Adobe I/O](https://www.adobe.io/apis/cloudplatform/console/authentication/gettingstarted.html)
* [Documentos de API de Adobe Stock](https://www.adobe.io/apis/creativecloud/stock/docs.html)
