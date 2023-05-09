---
title: Configuración del panel de Outlook de jubilación
seo-title: Configuring Retirement Outlook Panel
description: Esta es la parte 10 de un tutorial de varios pasos para crear su primer documento interactivo de comunicaciones. En esta parte, configuraremos el Panel de perspectivas de jubilación añadiendo componentes de texto y gráficos.
seo-description: This is part 10 of a multi-step tutorial for creating your first interactive communications document. In this part, we will configure Retirement Outlook Panel by adding text and chart components.
uuid: 1d5119b5-e797-4bf0-9b10-995b3f051f92
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 2ee2cea2-aefa-4d21-a258-248648f73a68
topic: Development
role: Developer
level: Beginner
exl-id: 0dd8a430-9a4e-4dc7-ad75-6ad2490430f2
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '349'
ht-degree: 1%

---

# Configuración del panel de Outlook de jubilación{#configuring-retirement-outlook-panel}

* Esta es la parte 10 de un tutorial de varios pasos para crear su primer documento interactivo de comunicaciones. En esta parte, configuraremos el Panel de perspectivas de jubilación añadiendo componentes de texto y gráficos.

* Inicie sesión en AEM Forms y vaya a Adobe Experience Manager > Forms > Forms y documentos.

* Abra la carpeta 401KStatement.

* Abra el documento 401KStatement en modo de edición.

**Configurar el área de destino del panel izquierdo**

* Pulse en el área de destino Panel izquierdo del lado derecho y haga clic en el icono &quot;+&quot; para que aparezca el cuadro de diálogo Insertar componente.

* Insertar componente Texto .

* Pulse con cuidado el componente de texto recién agregado para que aparezca la barra de herramientas de componentes

* Seleccione el icono &quot;lápiz&quot; para editar el texto predeterminado.

* Reemplace el texto predeterminado por &quot;**&quot;Perspectivas de ingresos de jubilación&quot;**

**Configuración del área de destino del panel derecho**

* Pulse en el área de destino Panel derecho del lado derecho y haga clic en el icono &quot;+&quot; para que aparezca el cuadro de diálogo Insertar componente.

* Insertar componente Texto .

* Pulse con cuidado el componente de texto recién agregado para que aparezca la barra de herramientas de componentes.

* Seleccione el icono &quot;lápiz&quot; para editar el texto predeterminado.

* Reemplace el texto predeterminado por &quot;**Ingresos mensuales estimados de jubilación&quot;**

## Agregar fragmento de documento de Outlook de ingresos de jubilación {#add-retirement-income-outlook-document-fragment}

* Haga clic en el icono Recursos y aplique el filtro para mostrar recursos de tipo &quot;Fragmentos de documento&quot;. Arrastre y suelte el fragmento de documento RetirementRevenueOutlook en el área de destino del panel izquierdo.

* Puede consultar [a esta página](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/partseven.html) al agregar fragmento de documento a áreas de contenido.

## Adición del gráfico de ingresos mensuales estimado {#adding-estimated-monthly-income-chart}

* Haga clic en el área de destino Panel derecho en el lado derecho. Haga clic en el icono &quot;+&quot; para insertar el componente de gráfico. Utilizaremos un gráfico de columnas para mostrar los ingresos mensuales estimados. Pulse con cuidado el componente de gráfico recién insertado. Seleccione el icono &quot;Llave&quot; para abrir la hoja de propiedades de configuración. Configure el gráfico con las siguientes propiedades como se muestra en la captura de pantalla siguiente.

**AEM Forms 6.4: Configuración del gráfico de columnas de ingresos mensuales estimados**

![form64](assets/estimatedmonthlyincomechart.png)

**AEM Forms 6.5: Configuración del gráfico de columnas de ingresos mensuales estimados**

![forms65](assets/estimatedmonthlyincomechart65.PNG)

## Pasos siguientes

[Configurar gráfico circular](./parteleven.md)
