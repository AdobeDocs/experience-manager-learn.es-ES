---
title: Configuración del panel Mezcla de inversiones
description: Esta es la parte 11 del tutorial de varios pasos para crear su primer documento de comunicaciones interactivas. En esta parte, añadiremos gráficos circulares para mostrar la combinación de inversión actual y modelo.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 77de4e76-53ee-467c-a71c-d1d3ea15283b
topic: Development
role: Developer
level: Beginner
exl-id: 774d7a6e-2b8f-4a70-98c5-e7712478ff75
duration: 69
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 0%

---

# Configuración del panel Mezcla de inversiones

En esta parte, agregaremos gráficos circulares para mostrar la combinación de inversión actual y modelo.

* Inicie sesión en AEM Forms y vaya a Adobe Experience Manager > Forms > Forms y documentos.

* Abra la carpeta 401KStatement.

* Abra 401KStatement en modo de edición.

* Agregaremos 2 gráficos circulares para representar la combinación de inversión actual y modelo del titular de la cuenta.

## Combinación de recursos actual {#current-asset-mix}

* Pulse en el panel &quot;CurrentAssetMix&quot; del lado derecho y seleccione el icono &quot;+&quot; e inserte el componente de texto. Cambie el texto predeterminado a &quot;Mezcla de recursos actual&quot;.

* Pulse en el panel &quot;CurrentAssetMix&quot; y seleccione el icono &quot;+&quot; e inserte el componente de gráfico. Pulse en el componente de gráfico recién insertado y haga clic en el icono &quot;llave inglesa&quot; para abrir la hoja de propiedades de configuración del gráfico.

* Establezca las propiedades como se muestra en la siguiente imagen. Asegúrese de que el tipo de gráfico es Gráfico circular.

* Observe el objeto del modelo de datos enlazado a los ejes X e Y. Debe seleccionar el elemento raíz del modelo de datos de formulario y, a continuación, explorar en profundidad para seleccionar el elemento adecuado.

* ![currentassetmix](assets/currentassetmixchart.png)

## Combinación de recursos de modelo {#model-asset-mix}

* Pulse en el panel &quot;Mezcla de recursos recomendada&quot; del lado derecho, seleccione el icono &quot;+&quot; e inserte el componente de texto. Cambie el texto predeterminado a &quot;Mezcla de recursos de modelo&quot;.

* Pulse en el panel &quot;Mezcla de recursos recomendada&quot;, seleccione el icono &quot;+&quot; e inserte el componente de gráfico. Pulse en el componente de gráfico recién insertado y haga clic en el icono &quot;llave inglesa&quot; para abrir la hoja de propiedades de configuración del gráfico.

* Establezca las propiedades como se muestra en la siguiente imagen. Asegúrese de que el tipo de gráfico es Gráfico circular.

* Observe el objeto del modelo de datos enlazado a los ejes X e Y. Debe seleccionar el elemento raíz del modelo de datos de formulario y, a continuación, explorar en profundidad para seleccionar el elemento adecuado.

* ![tipo de recurso](assets/modelassettypechart.png)

## Siguientes pasos

[Preparar el envío del documento de canal web](./parttwelve.md)
