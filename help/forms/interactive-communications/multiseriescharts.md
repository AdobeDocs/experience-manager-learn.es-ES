---
title: Gráficos de varias series en AEM Forms
description: Cree el modelo de datos de formulario adecuado para crear gráficos de varias series en documentos de los canales impreso y web.
feature: Interactive Communication
doc-type: article
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f4af7cb9-cc3b-4bec-9428-ab4f1a3cf41a
last-substantial-update: 2019-07-07T00:00:00Z
duration: 446
source-git-commit: 4f818f2ad01d9ecadcf5593aa038c7db15b4d496
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 1%

---

# Gráficos de varias series

AEM Forms 6.5 ha introducido la capacidad de crear y configurar varios gráficos de series. Los gráficos de varias series se suelen utilizar en asociación con el tipo de gráfico de líneas, barras y columnas. El siguiente gráfico es un buen ejemplo de gráfico de varias series. El gráfico muestra el crecimiento de $10,000 USD en 3 diferentes fondos mutuos durante un período de tiempo. Para poder crear y utilizar gráficos de este tipo en AEM Forms, deberá crear el modelo de datos de formulario adecuado.

![Gráfico de varias series](assets/series_charts.png)

Para crear gráficos de varias series en AEM Forms, debe crear un modelo de datos de formulario adecuado con las entidades y asociaciones necesarias entre las entidades. La siguiente captura de pantalla resalta las entidades y las asociaciones entre las 3 entidades. En el nivel superior, tenemos una entidad llamada &quot;Organización&quot;, que tiene una asociación &quot;uno a varios&quot; con la entidad del Fondo. A su vez, la entidad de la Caja tiene una asociación &quot;uno a varios&quot; con la entidad de ejecución.

![Modelo de datos de formulario](assets/form_data_model.png)

## Crear un modelo de datos de formulario para gráficos de varias series

>[!VIDEO](https://video.tv.adobe.com/v/26352?quality=12&learn=on)

### Configurar gráficos de series de líneas

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=12&learn=on)

Para probar esto en su sistema, siga los siguientes pasos

* [AEM Descargue e importe MutualFundFactSheet.zip mediante el Administrador de paquetes de.](assets/mutualfundfactsheet.zip)
* [Descargue SeriesChartSampleData.json en su disco duro.](assets/serieschartsampledata.json) Estos son los datos de ejemplo que se utilizan para rellenar el gráfico.
* [Vaya a Forms y Documentos.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Seleccione suavemente la plantilla de comunicaciones interactivas &quot;MutualFundGrowthFactSheet&quot;.
* Haga clic en Vista previa | Canal de impresión | Cargar datos de ejemplo.
* Busque el archivo de datos de ejemplo que se incluye en este artículo.
* Previsualice el canal de impresión de la comunicación interactiva &quot;MutualFundGrowthFactSheet&quot; con los datos de ejemplo descargados en el paso anterior.
