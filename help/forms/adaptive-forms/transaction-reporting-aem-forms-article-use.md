---
title: Uso de informes de transacciones en AEM Forms
description: Los informes de transacciones de AEM Forms le permiten mantener un recuento de todas las transacciones realizadas desde una fecha especificada en su implementación de AEM Forms.
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 36c38cb6-6f6a-4328-abf5-7a30059b66ce
last-substantial-update: 2019-03-20T00:00:00Z
duration: 68
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 23%

---

# Uso de informes de transacciones en AEM Forms{#using-transaction-reporting-in-aem-forms}

La creación de informes de transacciones para capturar el número de envíos de formularios, la representación de documentos mediante servicios de documentos y la representación de comunicaciones interactivas (canales web y de impresión) se ha introducido con AEM Forms 6.4.1. Esta capacidad se ha introducido con AEM Forms 6.4.1 para la pila OSGi de AEM Forms y 6.5.20 para la pila JEE de AEM Forms.

## Habilitar informes de transacciones {#enabling-transaction-reporting}

De forma predeterminada, el registro de transacciones está desactivado. Para habilitar el registro de transacciones, siga los pasos que se mencionan a continuación:

* [Abrir configMgr](http://localhost:4502/system/console/configMgr)
* Buscar &quot;Informes de transacciones de Forms&quot;
* Seleccione la casilla de verificación &quot;Registrar transacciones&quot;
* Guarde los cambios

Una vez habilitados los informes de transacciones, puede enviar Forms adaptable, generar documentos mediante servicios de documentos o procesar documentos de comunicación interactiva para ver los informes de transacciones en acción.

## Visualización del informe de transacciones {#viewing-transaction-report}

Para ver el informe de transacciones, inicie sesión en AEM Forms como administrador. Solo los miembros del grupo fd-Administrator pueden ver el informe de transacciones.

Seleccionar herramientas | Forms | Ver informe de transacciones

o vea el informe de transacciones haciendo clic [aquí](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)

![Informes de transacciones](assets/transactionreporting.gif)

En la captura de pantalla anterior Documento procesado es el número de documentos generados mediante servicios de documentos. Documentos representados es el número de documentos de comunicación interactiva (web e impresos) representados. Forms enviado es el número de envíos de formularios adaptables.

Una transacción permanece en el búfer durante un período especificado (tiempo de búfer de vaciado + tiempo de replicación inversa). De forma predeterminada, el recuento de transacciones tarda aproximadamente 90 segundos en reflejarse en el informe de transacciones.

Las acciones como enviar un formulario PDF, utilizar la interfaz de usuario del agente para obtener una vista previa de una comunicación interactiva o usar los métodos de envío de formulario no estándar no se contabilizan como transacciones. AEM Forms proporciona un API para registrar esas transacciones. Llame al API desde sus implementaciones personalizadas y registrar una transacción.

Si está viendo el informe de transacciones en la instancia de autor, asegúrese de que la replicación inversa esté configurada en todas las instancias de publicación.

Para obtener más información acerca de los informes de transacciones [haga clic aquí](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)
