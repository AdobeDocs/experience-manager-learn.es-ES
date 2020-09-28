---
title: Gráficos de serie múltiple en AEM Forms
seo-title: Gráficos de serie múltiple en AEM Forms
description: Cree el modelo de datos de formulario adecuado para crear gráficos de varias series en documentos de impresión y canal web.
seo-description: Cree el modelo de datos de formulario adecuado para crear gráficos de varias series en documentos de impresión y canal web.
feature: interactive-communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---


# Gráficos de varias series

AEM Forms 6.5 ha introducido la capacidad de crear y configurar varios gráficos de series. Los gráficos de series múltiples se utilizan generalmente en asociación con el tipo de gráfico Línea,Barra,Columna. El siguiente gráfico es un buen ejemplo de gráfico de varias series. El gráfico muestra el crecimiento de $10,000 USD en 3 fondos mutuos diferentes durante un período de tiempo. Para poder crear y utilizar gráficos de este tipo en AEM Forms, deberá crear el modelo de datos de formulario correspondiente.

![multiserie](assets/seriescharts.jfif)

Para crear gráficos de varias series en AEM Forms, debe crear un modelo de datos de formulario adecuado con las entidades y asociaciones necesarias entre las entidades. La siguiente captura de pantalla resalta las entidades y asociaciones entre las 3 entidades. En el nivel superior, tenemos una entidad llamada &quot;Organización&quot;, que tiene una asociación de uno a muchos con la entidad del Fondo. La entidad de la Caja, a su vez, tiene una relación de uno a muchos con la entidad de ejecución.

![formdatamodel](assets/formdatamodel.jfif)


## Crear modelo de datos de formulario para gráficos de varias series

>[!VIDEO](https://video.tv.adobe.com/v/26352/quality=9)


### Configurar gráficos de series de líneas

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=9&learn=on)


Para probar esto en su sistema, siga los siguientes pasos

* [Descargue e importe MutualFundFactSheet.zip mediante AEM administrador de paquetes.](assets/mutualfundfactsheet.zip)
* [Descargue SeriesChartSampleData.json en el disco duro.](assets/serieschartsampledata.json) Estos son los datos de ejemplo que se utilizarán para rellenar el gráfico.
* [Vaya a Forms y Documentos.](https://helpx.adobe.com/aem/forms.html/content/dam/formsanddocuments.html)
* Seleccione suavemente la plantilla de comunicaciones interactivas &quot;MutualFundGrowthFactSheet&quot;.
* Haga clic en Previsualización | Cargar datos de muestra.
* Busque el archivo de datos de ejemplo que se proporciona como parte de este artículo.
* Previsualización el canal de impresión de la comunicación interactiva &quot;MutualFundGrowthFactSheet&quot; con los datos de muestra descargados en el paso anterior.
