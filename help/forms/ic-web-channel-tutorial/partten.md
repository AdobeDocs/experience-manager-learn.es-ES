---
title: Configuración del panel de perspectivas de jubilación
seo-title: Configuración del panel de perspectivas de jubilación
description: Esta es la parte 10 de un tutorial de varios pasos para crear su primer documento interactivo de comunicaciones. En esta parte, configuraremos el panel de perspectivas de jubilación agregando componentes de texto y gráficos.
seo-description: Esta es la parte 10 de un tutorial de varios pasos para crear su primer documento interactivo de comunicaciones. En esta parte, configuraremos el panel de perspectivas de jubilación agregando componentes de texto y gráficos.
uuid: 1d5119b5-e797-4bf0-9b10-995b3f051f92
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 2ee2cea2-aefa-4d21-a258-248648f73a68
translation-type: tm+mt
source-git-commit: f6a9c0522219614b87c483504c9c64875f2e4286
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 0%

---


# Configuración del panel de perspectivas de jubilación{#configuring-retirement-outlook-panel}

* Esta es la parte 10 de un tutorial de varios pasos para crear su primer documento interactivo de comunicaciones. En esta parte, configuraremos el panel de perspectivas de jubilación agregando componentes de texto y gráficos.

* Inicie sesión en AEM Forms y vaya a Adobe Experience Manager > Forms > Forms y Documentos.

* Abra la carpeta 401KStatement.

* Abra el documento 401KStatement en modo de edición.

**Configurar el área de destinatario del panel izquierdo**

* Toque en el área de destinatario del panel izquierdo del lado derecho y haga clic en el icono &quot;+&quot; para que aparezca el cuadro de diálogo Insertar componente.

* Insertar componente Texto.

* Toque suavemente el componente de texto recientemente agregado para que aparezca la barra de herramientas del componente

* Seleccione el icono &quot;lápiz&quot; para editar el texto predeterminado.

* Reemplace el texto predeterminado con &quot;**Your Retirement Revenue Outlook&quot; (Perspectivas de ingresos de jubilación).**

**Configuración del área de destinatario del panel derecho**

* Puntee en el área de destinatario del panel derecho en el lado derecho y haga clic en el icono &quot;+&quot; para que aparezca el cuadro de diálogo Insertar componente.

* Insertar componente Texto.

* Toque suavemente el componente de texto recientemente agregado para que aparezca la barra de herramientas de componentes.

* Seleccione el icono &quot;lápiz&quot; para editar el texto predeterminado.

* Sustitúyase el texto predeterminado por &quot;Ingresos de jubilación mensuales **estimados&quot;.**

## Añadir fragmento de Documento de perspectivas de ingresos de jubilación {#add-retirement-income-outlook-document-fragment}

* Haga clic en el icono Recursos y aplique el filtro para mostrar los recursos de tipo &quot;Fragmentos de Documento&quot;. Arrastre y suelte el fragmento de documento RetirementRevenueOutlook en el área de destinatario del panel izquierdo.

* Puede hacer referencia [a esta página](https://helpx.adobe.com/experience-manager/kt/forms/using/interactive-communication-web-channel-aem-forms/9.html) al añadir fragmento de documento a las áreas de contenido.

## Añadir el gráfico de ingresos mensuales estimados {#adding-estimated-monthly-income-chart}

* Haga clic en el área de destinatario del panel derecho en el lado derecho. Haga clic en el icono &quot;+&quot; para insertar el componente de gráfico. Utilizaremos un gráfico de columnas para mostrar los ingresos mensuales estimados. Toque suavemente el componente de gráfico recién insertado. Seleccione el icono de &quot;llave inglesa&quot; para abrir la hoja de propiedades de configuración.Configure el gráfico con las siguientes propiedades como se muestra en la captura de pantalla siguiente.

**AEM Forms 6.4 - Configuración del gráfico de columnas de ingresos mensuales estimados**

![form64](assets/estimatedmonthlyincomechart.png)

**AEM Forms 6.5 - Configuración del gráfico de columnas de ingresos mensuales estimados**

![forms65](assets/estimatedmonthlyincomechart65.PNG)




