---
title: Uso de informes de transacciones en AEM Forms
seo-title: Uso de informes de transacciones en AEM Forms
description: Los informes de transacciones en AEM Forms le permiten mantener un recuento de todas las transacciones realizadas desde una fecha especificada en su implementación de AEM Forms.
seo-description: Los informes de transacciones en AEM Forms le permiten mantener un recuento de todas las transacciones realizadas desde una fecha especificada en su implementación de AEM Forms.
uuid: e6133f7e-c79c-4006-89e7-3bebf7b8229e
feature: Adaptive Forms
topics: developing
audience: administrator
doc-type: article
activity: setup
version: 6.4.1,6.5
discoiquuid: 1abdf07a-b9f0-4c58-a1c6-08ae57db2014
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '403'
ht-degree: 0%

---


# Uso de informes de transacciones en AEM Forms{#using-transaction-reporting-in-aem-forms}

Los informes de transacciones para capturar el número de envíos de formularios, el procesamiento de documentos mediante servicios de documentos y la representación de comunicaciones interactivas (canales web e impresos) se han introducido con AEM Forms 6.4.1. Esta capacidad está dirigida principalmente a los clientes que desean obtener una licencia del software en función del número de envíos de formularios y/o documentos procesados. Esta funcionalidad está disponible actualmente solo en la pila OSGI de AEM Forms.

## Habilitación del Sistema de Informes de Transacciones {#enabling-transaction-reporting}

De forma predeterminada, el registro de transacciones está desactivado. Para habilitar el registro de transacciones, siga los pasos que se indican a continuación:

* [Abra configMgr](http://localhost:4502/system/console/configMgr)
* Buscar &quot;Informes de transacciones de formularios&quot;
* Seleccione la casilla de verificación &quot;Registrar transacciones&quot;
* Guarde los cambios

Una vez habilitado el sistema de informes de transacciones, puede enviar formularios adaptables, generar documentos mediante document services o procesar documentos de comunicación interactiva para ver los informes de transacciones en acción.

## Visualización del informe de transacciones {#viewing-transaction-report}

Para ver el informe de transacciones, inicie sesión en AEM Forms como administrador. Solo los miembros del grupo fd-Administrator pueden ver el informe de transacción.

Seleccionar herramientas | Formularios | Ver informe de transacciones

o ver el informe de transacciones haciendo clic [aquí](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)

![Informes de transacciones](assets/transactionreporting.gif)

En la captura de pantalla de arriba Document Processed es el número de documentos generados usando document services. Los documentos procesados son el número de documentos de comunicación interactiva (Web e Print) procesados. Formularios enviados es el número de envíos de formularios adaptables.

Una transacción permanece en el búfer durante un período especificado (tiempo de búfer de vaciado + tiempo de replicación inversa). De forma predeterminada, el recuento de transacciones tarda aproximadamente 90 segundos en reflejarse en el informe de transacciones.

Las acciones como enviar un formulario PDF, utilizar la interfaz de usuario del agente para obtener una vista previa de una comunicación interactiva o utilizar métodos de envío de formularios no estándar no se contabilizan como transacciones. AEM Forms proporciona una API para registrar estas transacciones. Llame a la API desde las implementaciones personalizadas para registrar una transacción.

Si está viendo el informe de transacción en la instancia de autor, asegúrese de que la replicación inversa esté configurada en todas las instancias de publicación.

Para obtener más información sobre los informes de transacciones [haga clic aquí](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)

