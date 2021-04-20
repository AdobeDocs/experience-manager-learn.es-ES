---
title: Gráficos de varias series en AEM Forms
seo-title: Gráficos de varias series en AEM Forms
description: Cree el Modelo de datos de formulario adecuado para crear gráficos de varias series en documentos impresos y de canales web.
seo-description: Cree el Modelo de datos de formulario adecuado para crear gráficos de varias series en documentos impresos y de canales web.
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 1%

---


# Gráficos de varias series

AEM Forms 6.5 introdujo la capacidad de crear y configurar varios gráficos de series. Los gráficos de series múltiples se suelen utilizar en asociación con el tipo de gráfico Línea, Barra y Columna. El siguiente gráfico es un buen ejemplo de gráfico de varias series. El gráfico muestra el crecimiento de $10,000 USD en 3 fondos mutuos diferentes durante un periodo de tiempo. Para poder crear y utilizar gráficos de este tipo en AEM Forms, debe crear el modelo de datos de formulario adecuado.

![multiserie](assets/seriescharts.jfif)

Para crear gráficos de varias series en AEM Forms, debe crear un modelo de datos de formulario adecuado con las entidades y asociaciones necesarias entre las entidades. La siguiente captura de pantalla resalta las entidades y las asociaciones entre las 3 entidades. En el nivel superior, tenemos una entidad denominada &quot;Organización&quot;, que tiene una asociación de uno a varios con la entidad del Fondo. A su vez, la entidad de la Caja tiene una asociación de uno a varios con la entidad de la ejecución.

![formdatamodel](assets/formdatamodel.jfif)


## Creación de un modelo de datos de formulario para gráficos de varias series

>[!VIDEO](https://video.tv.adobe.com/v/26352/quality=9)


### Configurar gráficos de series de líneas

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=9&learn=on)


Para probar esto en su sistema, siga los siguientes pasos

* [Descargue e importe MutualFundFactSheet.zip mediante el Administrador de paquetes de AEM.](assets/mutualfundfactsheet.zip)
* [Descargue SeriesChartSampleData.json en su disco duro.](assets/serieschartsampledata.json) Estos son los datos de ejemplo que se utilizarán para rellenar el gráfico.
* [Vaya a Formularios y documentos.](https://helpx.adobe.com/aem/forms.html/content/dam/formsanddocuments.html)
* Seleccione suavemente la plantilla de comunicaciones interactivas &quot;MutualFundGrowthFactSheet&quot;.
* Haga clic en Vista previa | Cargar datos de muestra.
* Busque el archivo de datos de ejemplo que se proporciona como parte de este artículo.
* Previsualice el canal de impresión de la comunicación interactiva &quot;MutualFundGrowthFactSheet&quot; con los datos de ejemplo descargados en el paso anterior.
