---
title: Uso de informes de transacciones en AEM Forms
description: Los informes de transacciones de AEM Forms permiten mantener un recuento de todas las transacciones realizadas desde una fecha especificada en la implementación de AEM Forms.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 36c38cb6-6f6a-4328-abf5-7a30059b66ce
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 1%

---

# Uso de informes de transacciones en AEM Forms{#using-transaction-reporting-in-aem-forms}

Los informes de transacciones para capturar el número de envíos de formularios, el procesamiento de documentos mediante servicios de documentos y la representación de comunicaciones interactivas (canales Web e Imprimir) se han introducido con AEM Forms 6.4.1. Esta capacidad está dirigida principalmente a los clientes que desean obtener una licencia del software en función del número de envíos de formularios y/o documentos procesados. Esta funcionalidad está disponible actualmente solo en la pila OSGI de AEM Forms.

## Habilitación de la creación de informes de transacciones {#enabling-transaction-reporting}

De forma predeterminada, el registro de transacciones está desactivado. Para habilitar el registro de transacciones, siga los pasos que se indican a continuación:

* [Abra configMgr](http://localhost:4502/system/console/configMgr)
* Buscar &quot;Informes de transacciones de Forms&quot;
* Seleccione la casilla de verificación &quot;Registrar transacciones&quot;
* Guarde los cambios

Una vez habilitado el sistema de informes de transacciones, puede enviar el Forms adaptable, generar documentos mediante document services o procesar documentos de Interactive Communication para ver los informes de transacciones en acción.

## Visualización del informe de transacciones {#viewing-transaction-report}

Para ver el informe de transacciones, inicie sesión en AEM Forms como administrador. Solo los miembros del grupo fd-Administrator pueden ver el informe de transacción.

Seleccionar herramientas | Forms | Ver informe de transacciones

o ver el informe de transacciones haciendo clic en [here](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)

![Informes de transacciones](assets/transactionreporting.gif)

En la captura de pantalla de arriba Document Processed es el número de documentos generados usando document services. Los documentos procesados son el número de documentos de comunicación interactiva (Web e Print) procesados. Forms enviado es el número de envíos de formularios adaptables.

Una transacción permanece en el búfer durante un período especificado (tiempo de búfer de vaciado + tiempo de replicación inversa). De forma predeterminada, el recuento de transacciones tarda aproximadamente 90 segundos en reflejarse en el informe de transacciones.

Las acciones como enviar un formulario de PDF, usar la interfaz de usuario del agente para obtener una vista previa de una comunicación interactiva o usar métodos de envío de formularios no estándar no se contabilizan como transacciones. AEM Forms proporciona una API para registrar estas transacciones. Llame a la API desde las implementaciones personalizadas para registrar una transacción.

Si está viendo el informe de transacción en la instancia de autor, asegúrese de que la replicación inversa esté configurada en todas las instancias de publicación.

Para obtener más información sobre los informes de transacciones [haga clic aquí](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)
