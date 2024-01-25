---
title: AEM Extraer documento de la lista de documentos de un flujo de trabajo de
description: Componente de flujo de trabajo personalizado para extraer un documento específico de una lista de documentos
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: kt-13918
last-substantial-update: 2023-09-12T00:00:00Z
exl-id: b0baac71-3074-49d5-9686-c9955b096abb
duration: 72
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 0%

---

# Extraer documento de la lista de documentos

AEM Un caso de uso común es enviar los datos del formulario y el archivo adjunto del formulario a un sistema externo mediante el paso para invocar el modelo de datos de formulario en un flujo de trabajo de. Por ejemplo, al crear un caso en ServiceNow, le interesa enviar los detalles del caso con un documento acreditativo. Los archivos adjuntos agregados al formulario adaptable se almacenan en una variable de tipo lista de matrices de documentos y para extraer un documento específico de esta lista de matrices, deberá escribir un código personalizado.

En este artículo se explican los pasos para utilizar el componente de flujo de trabajo personalizado para extraer y almacenar el documento en una variable de documento.

## Crear flujo de trabajo

Es necesario crear un flujo de trabajo para administrar el envío del formulario. El flujo de trabajo debe tener definidas las siguientes variables

* Una variable de tipo ArrayList of Document(Esta variable contendrá los archivos adjuntos del formulario agregados por el usuario)
* Variable de tipo Document.(Esta variable contendrá el documento extraído de ArrayList)

* Añada el componente personalizado al flujo de trabajo y configure sus propiedades
  ![extract-item-workflow](assets/extract-document-array-list.png)

## Configurar formulario adaptable

* Configure la acción de envío del formulario adaptable para almacenar en déclencheur AEM el flujo de trabajo de la
  ![submit-action](assets/store-attachments.png)

## Prueba de la solución

[Implementar el paquete personalizado mediante la consola web OSGi](assets/ExtractItemsFromArray.core-1.0.0-SNAPSHOT.jar)

[Importación del componente de flujo de trabajo mediante el administrador de paquetes](assets/Extract-item-from-documents-list.zip)

[Importar el flujo de trabajo de ejemplo](assets/extract-item-sample-workflow.zip)

[Importar el formulario adaptable](assets/test-attachment-extractions-adaptive-form.zip)

[Previsualización del formulario](http://localhost:4502/content/dam/formsanddocuments/testattachmentsextractions/jcr:content?wcmmode=disabled)

Agregue un archivo adjunto al formulario y envíelo.

>[!NOTE]
>
>El documento extraído se puede utilizar en cualquier otro paso del flujo de trabajo, como Enviar correo electrónico o Invocar paso de FDM
