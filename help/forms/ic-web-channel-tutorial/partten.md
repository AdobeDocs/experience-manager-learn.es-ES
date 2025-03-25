---
title: Configurar el panel de Outlook de retirada
description: Esta es la parte 10 de un tutorial de varios pasos para crear su primer documento de comunicaciones interactivas. En esta parte, configuraremos el Panel de Outlook de retirada agregando componentes de texto y gráficos.
feature: Interactive Communication
doc-type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
discoiquuid: 2ee2cea2-aefa-4d21-a258-248648f73a68
topic: Development
role: Developer
level: Beginner
exl-id: 0dd8a430-9a4e-4dc7-ad75-6ad2490430f2
duration: 71
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '347'
ht-degree: 0%

---

# Configurar el panel de Outlook de retirada{#configuring-retirement-outlook-panel}

* Esta es la parte 10 de un tutorial de varios pasos para crear su primer documento de comunicaciones interactivas. En esta parte, configuraremos el Panel de Outlook de retirada agregando componentes de texto y gráficos.

* Inicie sesión en AEM Forms y vaya a Adobe Experience Manager > Forms > Forms y documentos.

* Abra la carpeta 401KStatement.

* Abra el documento 401KStatement en modo de edición.

**Configurar área de destino de LeftPanel**

* Pulse en el área de destino LeftPanel en el lado derecho y haga clic en el icono &quot;+&quot; para que aparezca el cuadro de diálogo Insertar componente.

* Insertar componente Texto.

* Pulse con cuidado el componente de texto recién agregado para que aparezca la barra de herramientas de componentes

* Seleccione el icono &quot;lápiz&quot; para editar el texto predeterminado.

* Reemplace el texto predeterminado por &quot;**Su previsión de ingresos de jubilación&quot;**

**Configurar área de destino de RightPanel**

* Pulse en el área de destino RightPanel en el lado derecho y haga clic en el icono &quot;+&quot; para que aparezca el cuadro de diálogo Insertar componente.

* Insertar componente Texto.

* Pulse con cuidado el componente de texto recién agregado para que aparezca la barra de herramientas de componentes.

* Seleccione el icono &quot;lápiz&quot; para editar el texto predeterminado.

* Reemplace el texto predeterminado por &quot;**Ingresos mensuales estimados para la jubilación&quot;**

## Añadir Fragmento de Documento de Perspectiva de Ingresos de Jubilación {#add-retirement-income-outlook-document-fragment}

* Haga clic en el icono de Assets y aplique el filtro para mostrar recursos de tipo &quot;Fragmentos de documento&quot;. Arrastre y suelte el fragmento de documento RetirementIncomeOutlook en el área de destino del panel izquierdo.

* Puede hacer referencia a [esta página](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/partseven.html) al agregar fragmento de documento a las áreas de contenido.

## Agregar gráfico de ingresos mensuales estimados {#adding-estimated-monthly-income-chart}

* Haga clic en el área de destino de RightPanel en el lado derecho. Haga clic en el icono &quot;+&quot; para insertar el componente de gráfico. Utilizaremos un gráfico de columnas para mostrar el ingreso mensual estimado. Pulse con cuidado el componente de gráfico recién insertado. Seleccione el icono &quot;Llave inglesa&quot; para abrir la hoja de propiedades de configuración. Configure el gráfico con las siguientes propiedades, como se muestra en la captura de pantalla siguiente.

**AEM Forms 6.4 - Configuración del gráfico de columnas de ingresos mensuales estimados**

![formulario64](assets/estimatedmonthlyincomechart.png)

**AEM Forms 6.5 - Configuración del gráfico de columnas de ingresos mensuales estimados**

![forms65](assets/estimatedmonthlyincomechart65.PNG)

## Siguientes pasos

[Configurar gráfico circular](./parteleven.md)
