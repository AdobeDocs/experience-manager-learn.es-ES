---
title: Configuración del panel Mezcla de Inversiones
seo-title: Configuración del panel Mezcla de Inversiones
description: Esta es la parte 11 de un tutorial de varios pasos para crear su primer documento de comunicaciones interactivo.En esta parte, agregaremos gráficos circulares para mostrar la combinación de inversión actual y modelo.
seo-description: Esta es la parte 11 de un tutorial de varios pasos para crear su primer documento de comunicaciones interactivo.En esta parte, agregaremos gráficos circulares para mostrar la combinación de inversión actual y modelo.
uuid: b0132912-cb6e-4dec-8309-5125d29ad291
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 77de4e76-53ee-467c-a71c-d1d3ea15283b
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '358'
ht-degree: 0%

---


# Configuración del panel Mezcla de Inversiones

En esta parte, agregaremos gráficos circulares para mostrar la combinación de inversión actual y modelo.

* Inicie sesión en AEM Forms y vaya a Adobe Experience Manager > Forms > Forms y Documentos.

* Abra la carpeta 401KStatement.

* Abra la instrucción 401KStatement en modo de edición.

* Agregaremos 2 gráficos circulares para representar la combinación de inversión actual y modelo del titular de la cuenta.

## Mezcla de recursos actual {#current-asset-mix}

* Toque en el panel &quot;CurrentAssetMix&quot; del lado derecho y seleccione el icono &quot;+&quot; e inserte el componente de texto. Cambie el texto predeterminado a &quot;Mezcla de recursos actual&quot;.

* Toque en el panel &quot;CurrentAssetMix&quot; y seleccione el icono &quot;+&quot; e inserte el componente de gráfico. Toque el componente de gráfico recién insertado y haga clic en el icono &quot;llave inglesa&quot; para abrir la hoja de propiedades de configuración del gráfico.

* Defina las propiedades como se muestra en la imagen siguiente. Asegúrese de que el tipo de gráfico sea Gráfico circular.

* Observe el objeto del modelo de datos que está enlazado a los ejes X e Y. Debe seleccionar el elemento raíz del modelo de datos de formulario y, a continuación, desplazarse hacia abajo para seleccionar el elemento adecuado.

* ![currentassetmix](assets/currentassetmixchart.png)

## Mezcla de recursos de modelo {#model-asset-mix}

* Puntee en el panel &quot;Mezcla de recursos recomendada&quot; en el lado derecho y seleccione el icono &quot;+&quot; e inserte el componente de texto. Cambie el texto predeterminado a &quot;Mezcla de recursos de modelo&quot;.

* Puntee en el panel &quot;Mezcla de recursos recomendada&quot; y seleccione el icono &quot;+&quot; e inserte el componente de gráfico. Toque el componente de gráfico recién insertado y haga clic en el icono &quot;llave inglesa&quot; para abrir la hoja de propiedades de configuración del gráfico.

* Defina las propiedades como se muestra en la imagen siguiente. Asegúrese de que el tipo de gráfico sea Gráfico circular.

* Observe el objeto del modelo de datos que está enlazado a los ejes X e Y. Debe seleccionar el elemento raíz del modelo de datos de formulario y, a continuación, desplazarse hacia abajo para seleccionar el elemento adecuado.

* ![assettype](assets/modelassettypechart.png)

