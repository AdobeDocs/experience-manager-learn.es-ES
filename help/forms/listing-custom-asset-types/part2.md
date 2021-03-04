---
title: Listado de tipos de recursos personalizados en AEM Forms
seo-title: Listado de tipos de recursos personalizados en AEM Forms
description: Parte 2 de la lista de tipos de recursos personalizados en AEM Forms
seo-description: Parte 2 de la lista de tipos de recursos personalizados en AEM Forms
uuid: 6467ec34-e452-4c21-9bb5-504f9630466a
feature: Formularios adaptables
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
discoiquuid: 4b940465-0bd7-45a2-8d01-e4d640c9aedf
topic: Desarrollo
role: Desarrollador
level: Con experiencia
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 0%

---


# Lista de tipos de recursos personalizados en AEM Forms {#listing-custom-asset-types-in-aem-forms}

## Creación de una plantilla personalizada {#creating-custom-template}


A los efectos de este artículo, crearemos una plantilla personalizada para mostrar los tipos de recurso personalizados y los tipos de recurso OOTB en la misma página. Para crear una plantilla personalizada, siga las siguientes instrucciones

1. Crear un sling: en /apps. Denomínela &quot;myportalcomponent&quot;
1. Agregue la propiedad &quot;fpContentType&quot;. Establezca su valor en &quot;**/libs/fd/ fp/formTemplate&quot;.**
1. Agregue una propiedad &quot;title&quot; y establezca su valor en &quot;plantilla personalizada&quot;. Este es el nombre que verá en la lista desplegable del componente de búsqueda y lista
1. Cree un &quot;template.html&quot; en esta carpeta. Este archivo contendrá el código para aplicar estilo y mostrar los distintos tipos de recursos.

![appsfolder](assets/appsfolder_.png)

El siguiente código enumera los distintos tipos de recursos que utilizan el componente de búsqueda y lista. Creamos elementos html independientes para cada tipo de recurso como se muestra en la etiqueta data-type = &quot;videos&quot;. Para el tipo de recurso de &quot;vídeos&quot;, utilizamos el elemento &lt;video> para reproducir el vídeo en línea. Para el tipo de recurso de &quot;documentos de palabras&quot; usamos diferentes marcas html.

```html
<div class="__FP_boxes-container __FP_single-color">
   <div  data-repeatable="true">
     <div class = "__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "videos">
   <video width="400" controls>
       <source src="${path}" type="video/mp4">
    </video>
         <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
     </div>
     <div class="__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "worddocuments">
       <a href="/assetdetails.html${path}" target="_blank">
           <img src ="/content/dam/worddocuments/worddocument.png/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
          </a>
          <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
     </div>
  <div class="__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "xfaForm">
       <a href="/assetdetails.html${path}" target="_blank">
           <img src ="${path}/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
          </a>
          <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
                <a href="{formUrl}"><img src="/content/dam/html5.png"></a><p>

     </div>
  <div class="__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "printForm">
       <a href="/assetdetails.html${path}" target="_blank">
           <img src ="${path}/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
          </a>
          <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
                <a href="{pdfUrl}"><img src="/content/dam/pdf.png"></a><p>
     </div>
   </div>
</div>
```

>[!NOTE]
>
>Línea 11 - Cambie la imagen src para que apunte a una imagen de su elección en DAM.
>
>Para enumerar los formularios adaptables en esta plantilla, cree un nuevo div y establezca su atributo de tipo de datos en &quot;guía&quot;. Puede copiar y pegar el div cuyos datos-type=&quot;printForm&quot; y establecer el tipo de datos del div recién copiado en &quot;guide&quot;

## Configurar el componente Búsqueda y lista {#configure-search-and-lister-component}

Una vez que hemos definido la plantilla personalizada, ahora tenemos que asociar esta plantilla personalizada con el componente &quot;Buscar y listar&quot;. Apunte el navegador [a esta dirección URL ](http://localhost:4502/editor.html/content/AemForms/CustomPortal.html).

Cambie al modo Diseño y configure el sistema de párrafos para incluir el componente Buscar y listar en el grupo de componentes permitidos. El componente Búsqueda y lista forma parte del grupo Servicios de documentos.

Cambie al modo de edición y añada el componente Buscar y listar al ParSys.

Abra las propiedades de configuración del componente &quot;Búsqueda y lista&quot;. Asegúrese de que la pestaña &quot;Carpetas de recursos&quot; esté seleccionada. Seleccione las carpetas de las que desea enumerar los recursos en el componente de búsqueda y lista. A los efectos de este artículo, he seleccionado

* /content/dam/VideosAndWordDocuments
* /content/dam/formsanddocuments/assettypes

![assetfolder](assets/selectingassetfolders.png)

Tabule en la pestaña &quot;Mostrar&quot;. Aquí puede elegir la plantilla que desea que muestre los recursos en el componente de búsqueda y lista.

Seleccione &quot;plantilla personalizada&quot; en la lista desplegable como se muestra a continuación.

![searchandlister](assets/searchandlistercomponent.gif)

Configure los tipos de recursos que desea enumerar en el portal. Para configurar los tipos de pestaña del recurso en la &quot;Lista de recursos&quot; y configurar los tipos de recursos. En este ejemplo se han configurado los siguientes tipos de recursos

1. Archivos MP4
1. Documentos de Word
1. Documento (es el tipo de recurso OOTB)
1. Plantilla de formulario (es el tipo de recurso OOTB)

La siguiente captura de pantalla muestra los tipos de recursos configurados para la lista

![assettypes](assets/assettypes.png)

Ahora que ha configurado el componente del portal de búsqueda y lista, es hora de ver el listado en acción. Apunte el navegador [a esta dirección URL ](http://localhost:4502/content/AemForms/CustomPortal.html?wcmmode=disabled). Los resultados deberían ser como la imagen que se muestra a continuación.

>[!NOTE]
>
>Si su portal enumera tipos de recursos personalizados en un servidor de publicación, asegúrese de dar permiso de &quot;lectura&quot; al usuario &quot;fd-service&quot; al nodo **/apps/fd/fp/extensions/querybuilder**

![](assets/assettypeslistings.png)
[assettypesDescargue e instale este paquete mediante el gestor de paquetes.](assets/customassettypekt1.zip) Contiene ejemplos de documentos mp4 y word y archivos xdp que se utilizarán como tipos de recursos para enumerarlos con el componente de búsqueda y lista
