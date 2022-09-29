---
title: Gráficos de varias series en AEM Forms
description: Cree el Modelo de datos de formulario adecuado para crear gráficos de varias series en documentos impresos y de canales web.
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f4af7cb9-cc3b-4bec-9428-ab4f1a3cf41a
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 0%

---

# Gráficos de varias series

AEM Forms 6.5 ha introducido la capacidad de crear y configurar varios gráficos de series. Los gráficos de series múltiples se suelen utilizar en asociación con el tipo de gráfico Línea, Barra y Columna. El siguiente gráfico es un buen ejemplo de gráfico de varias series. El gráfico muestra el crecimiento de $10,000 USD en 3 fondos mutuos diferentes durante un periodo de tiempo. Para poder crear y utilizar este tipo de gráficos en AEM Forms, deberá crear el Modelo de datos de formulario adecuado.

![Gráfico de varias series](assets/seriescharts.jfif)

Para crear gráficos de varias series en AEM Forms, debe crear un Modelo de datos de formulario adecuado con las entidades y asociaciones necesarias entre las entidades. La siguiente captura de pantalla resalta las entidades y las asociaciones entre las 3 entidades. En el nivel superior, tenemos una entidad denominada &quot;Organización&quot;, que tiene una asociación de uno a varios con la entidad del Fondo. A su vez, la entidad de la Caja tiene una asociación de uno a varios con la entidad de la ejecución.

![Modelo de datos de formulario](assets/formdatamodel.jfif)

## Creación de un modelo de datos de formulario para gráficos de varias series

>[!VIDEO](https://video.tv.adobe.com/v/26352/quality=9)

### Configurar gráficos de series de líneas

>[!VIDEO](https://video.tv.adobe.com/v/26353?quality=9&learn=on)

Para probar esto en su sistema, siga los siguientes pasos

* [Descargue e importe MutualFundFactSheet.zip mediante AEM Administrador de paquetes.](assets/mutualfundfactsheet.zip)
* [Descargue SeriesChartSampleData.json en su disco duro.](assets/serieschartsampledata.json) Estos son los datos de ejemplo que se utilizan para rellenar el gráfico.
* [Vaya a Forms y Documentos.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Seleccione suavemente la plantilla de comunicaciones interactivas &quot;MutualFundGrowthFactSheet&quot;.
* Haga clic en Vista previa | Canal de impresión | Cargar datos de muestra.
* Busque el archivo de datos de ejemplo que se proporciona como parte de este artículo.
* Previsualice el canal de impresión de la comunicación interactiva &quot;MutualFundGrowthFactSheet&quot; con los datos de ejemplo descargados en el paso anterior.
