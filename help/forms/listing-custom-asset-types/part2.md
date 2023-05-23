---
title: Listado de tipos de recursos personalizados en AEM Forms
description: Parte 2 de Lista de tipos de recursos personalizados en AEM Forms
uuid: 6467ec34-e452-4c21-9bb5-504f9630466a
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 4b940465-0bd7-45a2-8d01-e4d640c9aedf
topic: Development
role: Developer
level: Experienced
exl-id: f221d8ee-0452-4690-a936-74bab506d7ca
last-substantial-update: 2019-07-10T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 0%

---

# Listado de tipos de recursos personalizados en AEM Forms {#listing-custom-asset-types-in-aem-forms}

## Crear una plantilla personalizada {#creating-custom-template}

Para el propósito de este artículo, estamos creando una plantilla personalizada para mostrar los tipos de recursos personalizados y los tipos de recursos OOTB en la misma página. Para crear una plantilla personalizada, siga las siguientes instrucciones

1. Cree una carpeta sling: en /apps. Asígnele el nombre &quot;myportalcomponent&quot;
1. Agregue la propiedad “fpContentType”. Establezca su valor en &quot;**/libs/fd/ fp/formTemplate&quot;.**
1. Agregue una propiedad &quot;title&quot; y establezca su valor en &quot;custom template&quot;. Este es el nombre que verá en la lista desplegable del componente Buscar y listar
1. Cree un &quot;template.html&quot; en esta carpeta. Este archivo contiene el código para aplicar estilo y mostrar los distintos tipos de recursos.

![appsfolder](assets/appsfolder_.png)

El siguiente código enumera los distintos tipos de recursos que utilizan el componente Buscar y listar. Creamos elementos html independientes para cada tipo de recurso, como se muestra por tipo de datos = etiqueta &quot;videos&quot;. Para el tipo de recurso de &quot;vídeos&quot; se utiliza el &lt;video> para reproducir el vídeo en línea. Para el tipo de recurso de &quot;documentos de palabra&quot; utilizamos diferentes marcas html.

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
>Línea 11: cambie la imagen src para que apunte a una imagen de su elección en DAM.
>
>Para enumerar Forms adaptable en esta plantilla, cree un nuevo div y establezca su atributo de tipo de datos en &quot;guía&quot;. Puede copiar y pegar el div cuyo data-type=&quot;printForm&quot; y establecer el tipo de datos del div recién copiado en &quot;guide&quot;

## Configurar El Componente Buscar Y Listar {#configure-search-and-lister-component}

Una vez definida la plantilla personalizada, ahora tenemos que asociarla con el componente &quot;Buscar y listar&quot;. Apunte al explorador [a esta dirección URL ](http://localhost:4502/editor.html/content/AemForms/CustomPortal.html).

Cambie al modo Diseño y configure el sistema de párrafos para incluir el componente Buscar y listar en el grupo de componentes permitidos. El componente Buscar y listar forma parte del grupo Servicios de documentos.

Cambie al modo de edición y añada el componente Buscar y listar a ParSys.

Abra las propiedades de configuración del componente Buscar y listar. Asegúrese de que la pestaña &quot;Carpetas de recursos&quot; esté seleccionada. Seleccione las carpetas desde las que desea enumerar los recursos en el componente Buscar y listar. Para los fines de este artículo, he seleccionado

* /content/dam/VideosAndWordDocuments
* /content/dam/formsanddocuments/assettypes

![assetfolder](assets/selectingassetfolders.png)

Vaya a la pestaña &quot;Mostrar&quot;. Aquí elegirá la plantilla en la que desea mostrar los recursos en el componente de búsqueda y lista.

Seleccione &quot;plantilla personalizada&quot; en la lista desplegable como se muestra a continuación.

![lista de búsqueda](assets/searchandlistercomponent.gif)

Configure los tipos de recursos que desea enumerar en el portal. Para configurar los tipos de pestaña del recurso a la &quot;Lista de recursos&quot; y configurar los tipos de recursos. En este ejemplo, se han configurado los siguientes tipos de recursos

1. Archivos MP4
1. Documentos de Word
1. Documento (es el tipo de recurso OOTB)
1. Plantilla de formulario (es un tipo de recurso OOTB)

La siguiente captura de pantalla muestra los tipos de recursos configurados para la lista

![assettypes](assets/assettypes.png)

Ahora que ha configurado el componente de portal Buscar y listar, es hora de ver el listado en acción. Apunte al explorador [a esta dirección URL ](http://localhost:4502/content/AemForms/CustomPortal.html?wcmmode=disabled). Los resultados deben ser similares a la imagen que se muestra a continuación.

>[!NOTE]
>
>Si el portal enumera tipos de recursos personalizados en un servidor de publicación, asegúrese de conceder permiso de &quot;lectura&quot; al usuario de &quot;fd-service&quot; en el nodo **/apps/fd/fp/extensions/querybuilder**

![assettypes](assets/assettypeslistings.png)
[Descargue e instale este paquete mediante el administrador de paquetes.](assets/customassettypekt1.zip) Contiene documentos mp4 y word de ejemplo y archivos xdp que se utilizan como tipos de recursos para enumerarlos mediante el componente buscar y listar
