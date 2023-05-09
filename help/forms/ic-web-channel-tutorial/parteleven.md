---
title: Configuración del panel Mezcla de Inversiones
seo-title: Configuring Investment Mix Panel
description: Esta es la parte 11 del tutorial de varios pasos para crear su primer documento interactivo de comunicaciones. En esta parte, agregaremos gráficos circulares para mostrar la combinación de inversión actual y la del modelo.
seo-description: This is part 11 of multistep tutorial for creating your first interactive communications document.In this part, we will add pie charts to display the current and model investment mix.
uuid: b0132912-cb6e-4dec-8309-5125d29ad291
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 77de4e76-53ee-467c-a71c-d1d3ea15283b
topic: Development
role: Developer
level: Beginner
exl-id: 774d7a6e-2b8f-4a70-98c5-e7712478ff75
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 0%

---

# Configuración del panel Mezcla de Inversiones

En esta parte, agregaremos gráficos circulares para mostrar la combinación de inversión actual y modelo.

* Inicie sesión en AEM Forms y vaya a Adobe Experience Manager > Forms > Forms y documentos.

* Abra la carpeta 401KStatement.

* Abra la instrucción 401KS en modo de edición.

* Añadiremos 2 gráficos circulares para representar la combinación de inversión actual y modelo del titular de la cuenta.

## Combinación de activos actuales {#current-asset-mix}

* Pulse en el panel &quot;Combinación de recursos actual&quot; del lado derecho, seleccione el icono &quot;+&quot; e inserte el componente de texto. Cambie el texto predeterminado a &quot;Combinación de recursos actual&quot;.

* Pulse en el panel &quot;Combinación de recursos actual&quot; y seleccione el icono &quot;+&quot; e inserte el componente de gráfico. Pulse en el componente de gráfico recién insertado y haga clic en el icono &quot;llave inglesa&quot; para abrir la hoja de propiedades de configuración del gráfico.

* Establezca las propiedades como se muestra en la imagen siguiente. Asegúrese de que el tipo de gráfico sea circular.

* Tenga en cuenta el objeto del modelo de datos que está enlazado a los ejes X e Y. Debe seleccionar el elemento raíz del modelo de datos de formulario y, a continuación, desplazarse hacia abajo para seleccionar el elemento adecuado.

* ![currentassetmix](assets/currentassetmixchart.png)

## Mezcla de recursos de modelo {#model-asset-mix}

* Pulse en el panel &quot;Mezcla de recursos recomendados&quot; del lado derecho, seleccione el icono &quot;+&quot; e inserte el componente de texto. Cambie el texto predeterminado a &quot;Mezcla de recursos de modelo&quot;.

* Pulse en el panel &quot;Mezcla de recursos recomendados&quot; y seleccione el icono &quot;+&quot; e inserte el componente de gráfico. Pulse en el componente de gráfico recién insertado y haga clic en el icono &quot;llave inglesa&quot; para abrir la hoja de propiedades de configuración del gráfico.

* Establezca las propiedades como se muestra en la imagen siguiente. Asegúrese de que el tipo de gráfico sea circular.

* Tenga en cuenta el objeto del modelo de datos que está enlazado a los ejes X e Y. Debe seleccionar el elemento raíz del modelo de datos de formulario y, a continuación, desplazarse hacia abajo para seleccionar el elemento adecuado.

* ![assettype](assets/modelassettypechart.png)

## Pasos siguientes

[Preparación para entregar un documento de canal web](./parttwelve.md)
