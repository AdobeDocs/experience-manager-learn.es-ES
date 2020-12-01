---
title: Uso del Sistema de informes de transacciones en AEM Forms
seo-title: Uso del Sistema de informes de transacciones en AEM Forms
description: Los informes de transacciones de AEM Forms permiten mantener un recuento de todas las transacciones realizadas desde una fecha especificada en la implementación de AEM Forms.
seo-description: Los informes de transacciones de AEM Forms permiten mantener un recuento de todas las transacciones realizadas desde una fecha especificada en la implementación de AEM Forms.
uuid: e6133f7e-c79c-4006-89e7-3bebf7b8229e
feature: adaptive-forms
topics: developing
audience: administrator
doc-type: article
activity: setup
version: 6.4.1,6.5
discoiquuid: 1abdf07a-b9f0-4c58-a1c6-08ae57db2014
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '398'
ht-degree: 0%

---


# Uso del Sistema de informes de transacciones en AEM Forms{#using-transaction-reporting-in-aem-forms}

El sistema de informes de transacciones para capturar el número de envíos de formularios, el procesamiento de documentos mediante servicios de documento y el procesamiento de comunicaciones interactivas (canales Web e impresos) se ha introducido con AEM Forms 6.4.1. Esta capacidad está dirigida principalmente a los clientes que desean obtener una licencia del software en función del número de envíos de formularios y/o documentos procesados. Esta capacidad está disponible actualmente solo en la pila OSGI de AEM Forms.

## Activación del Sistema de informes de transacciones {#enabling-transaction-reporting}

De forma predeterminada, el registro de transacciones está desactivado. Para habilitar el registro de transacciones, siga los pasos que se mencionan a continuación:

* [Abra configMgr](http://localhost:4502/system/console/configMgr)
* Buscar &quot;Sistema de informes de transacción de Forms&quot;
* Seleccione la casilla de verificación &quot;Registrar transacciones&quot;
* Guardar los cambios

Una vez activado el sistema de informes de transacciones, puede enviar Forms adaptable, generar documentos mediante servicios de documento o procesar documentos de comunicación interactiva para ver el sistema de informes de transacciones en acción.

## Visualización del informe de transacciones {#viewing-transaction-report}

Para vista del informe de transacciones, inicie sesión en AEM Forms como administrador. Sólo los miembros del grupo fd-Administrator pueden realizar la vista del informe de transacciones.

Seleccionar herramientas | Forms | Informe Transacciones de Vista

o vista el informe de transacciones haciendo clic [aquí](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)

![TransctionReporting](assets/transactionreporting.gif)

En la captura de pantalla de arriba Documento Procesado es el número de documentos generados mediante servicios de documento. Documentos procesados es el número de documentos de comunicación interactiva (Web e impresión) procesados. Forms Enviado es el número de envíos de formularios adaptables.

Una transacción permanece en el búfer durante un período especificado (tiempo de vaciado del búfer + tiempo de replicación inverso). De forma predeterminada, el recuento de transacciones tarda aproximadamente 90 segundos en reflejarse en el informe de transacciones.

Las acciones como enviar un formulario PDF, utilizar la interfaz de usuario del agente para realizar la previsualización de una comunicación interactiva o utilizar métodos de envío de formularios no estándar no se contabilizan como transacciones. AEM Forms proporciona una API para registrar dichas transacciones. Llame a la API desde sus implementaciones personalizadas para registrar una transacción.

Si está viendo el informe de transacciones en la instancia de creación, asegúrese de que la replicación inversa está configurada en todas las instancias de publicación.

Para obtener más información sobre el sistema de informes de transacciones [haga clic aquí](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)

