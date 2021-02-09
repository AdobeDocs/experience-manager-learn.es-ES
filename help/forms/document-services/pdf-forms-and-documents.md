---
title: Comprender los diferentes tipos de PDF forms y documentos
description: PDF es en realidad una familia de formatos de archivo, y este artículo describe los tipos de archivos PDF que son importantes y relevantes para los desarrolladores de formularios.
solution: Experience Manager Forms
product: aem
type: Documentation
role: Developer
level: Beginner,Intermediate
version: 6.3,6.4,6.5
feature: Document Services
topic: development
kt: 7071
translation-type: tm+mt
source-git-commit: 1e945afddda3d7735005029952a9d7ec46828bc6
workflow-type: tm+mt
source-wordcount: '1294'
ht-degree: 0%

---


# Formatos PDF

El formato de Documento portátil (PDF) es en realidad una familia de formatos de archivo y este artículo detalla los más relevantes para los desarrolladores de formularios. Muchos de los detalles técnicos y estándares de los diferentes tipos de PDF están evolucionando y cambiando. Algunos de estos formatos y especificaciones son normas de la Organización Internacional de Normalización (ISO), y algunos son propiedad intelectual específica de Adobe.

Este artículo muestra cómo crear varios tipos de archivos PDF. Te ayudará a entender cómo y por qué usar cada uno. Todos estos tipos funcionan mejor en la herramienta de cliente principal para ver y trabajar con archivos PDF: Adobe Acrobat DC.

Este es un ejemplo de archivo PDF/A en Acrobat DC.
![pdfa](assets/pdfa-file-in-acrobat.png)

Los archivos de ejemplo se pueden [descargar desde aquí](assets/pdf-file-types.zip)

## PDF XFA

Adobe utiliza el término formulario PDF para referirse a los formularios interactivos y dinámicos que se crean con AEM Forms Designer. Es importante tener en cuenta que hay otro tipo de formulario PDF, denominado Acroform, que es diferente de los PDF forms que se crean en AEM Forms Designer. Los formularios y archivos que se crean con Designer se basan en XML Forms Architecture (XFA) de Adobe. En muchos sentidos, el formato de archivo PDF XFA está más cerca de un archivo HTML que de un archivo PDF tradicional. Por ejemplo, el siguiente código muestra el aspecto de un objeto de texto simple en un archivo PDF XFA.

![text-field](assets/text-field.JPG)

Como puede ver, los formularios XFA están basados en XML. Este formato flexible y bien estructurado permite a un servidor de AEM Forms transformar los archivos de Designer en diferentes formatos, incluidos los PDF, PDF/A y HTML tradicionales. Puede ver la estructura XML completa de los formularios en Designer seleccionando la ficha Código fuente XML del Editor de presentaciones. Puede crear formularios XFA estáticos y dinámicos en AEM Forms Designer.

## PDF estático

Los PDF forms XFA estáticos no cambiarán su diseño en tiempo de ejecución, pero pueden ser interactivos para el usuario. Las siguientes son algunas de las ventajas de los PDF forms XFA estáticos:

* Los PDF forms XFA estáticos no cambiarán su diseño en tiempo de ejecución, pero pueden ser interactivos para el usuario.
* Los formularios estáticos admiten las herramientas Comentario y Marcado de Acrobat.
* Los formularios estáticos permiten importar y exportar comentarios de Acrobat.
* Los formularios estáticos admiten la subconfiguración de fuente, que es una técnica que se puede realizar en un servidor AEM Forms.
* Los formularios estáticos se pueden procesar con el visor de PDF integrado que viene con los navegadores modernos.

>[!NOTE]
> Puede crear archivos PDF estáticos con AEM Forms Designer guardando el XDP como formulario PDF estático de Adobe

## Forms dinámico

Los archivos PDF XFA dinámicos pueden cambiar su diseño en tiempo de ejecución, por lo que no se admiten las funciones de comentario y marca. Sin embargo, los archivos PDF XFA dinámicos tienen las siguientes ventajas:

* Los formularios dinámicos admiten secuencias de comandos de lado del cliente que cambian la presentación y la paginación del formulario. Por ejemplo, Purchase Order.xdp se expandirá y paginará para dar cabida a una cantidad infinita de datos si los guarda como un formulario dinámico
* Los formularios dinámicos admiten todas las propiedades del formulario en tiempo de ejecución, mientras que los formularios estáticos solo admiten un subconjunto


>[!NOTE]
> Puede crear archivos PDF dinámicos con AEM Forms Designer guardando el XDP como formulario XML dinámico de Adobe

>[!NOTE]
> Los formularios dinámicos no se pueden procesar con los visores de PDF integrados de los navegadores modernos.


## Archivo PDF (PDF tradicional)

El formato PDF más popular y generalizado es el archivo PDF tradicional. Existen muchas formas de crear un archivo PDF tradicional, incluido el uso de Acrobat y muchas herramientas de terceros. Acrobat ofrece todas las formas siguientes de crear archivos PDF tradicionales. Si no tiene Acrobat instalado, es posible que no vea estas opciones en el equipo.

* Capturando el flujo de impresión de una aplicación de escritorio: Elija el comando Imprimir de una aplicación de creación y seleccione el icono de la impresora Adobe PDF. En lugar de una copia impresa del documento, habrá creado un archivo PDF del documento
* Mediante el uso del complemento Acrobat PDFMaker con aplicaciones de Microsoft Office: Al instalar Acrobat, agrega un menú Adobe PDF a las aplicaciones de Microsoft Office y un icono a la cinta de Office. Puede utilizar estas funciones agregadas para crear archivos PDF directamente en Microsoft Office
* Mediante el uso de Acrobat Distiller para convertir archivos Postscript y Postscript encapsulado (EPS) en archivos PDF: Distiller suele utilizarse en la publicación impresa y otros flujos de trabajo que requieren una conversión del formato Postscript al formato PDF
* Bajo el capó, un PDF tradicional es muy diferente a un PDF XFA. No tiene la misma estructura XML y, como se crea capturando el flujo de impresión de un archivo, un PDF tradicional es un archivo estático y de solo lectura.

Un Documento certificado ofrece a los destinatarios de formularios y documentos PDF garantías adicionales de su autenticidad e integridad.

## Acróformularios

Las formas son la antigua tecnología de formularios interactivos de Adobe; datan de la versión 3 de Acrobat. Adobe proporciona la [Acrobat Forms API Reference](assets/FormsAPIReference.pdf), de mayo de 2003, para proporcionar los detalles técnicos de esta tecnología. Las formas son una combinación de
siguientes elementos:

* Un PDF tradicional que define la presentación estática y los gráficos del formulario.
* Campos de formulario interactivos que aparecen en negrita encima de las herramientas de formulario de Adobe Acrobat programa. Tenga en cuenta que estas herramientas de formulario son un pequeño subconjunto de lo que está disponible en AEM Forms Designer.

## PDF/A (PDF para archivo)

PDF/A (PDF para archivos) se basa en las ventajas del almacenamiento de documento de los archivos PDF tradicionales con muchos detalles específicos que mejoran el archivado a largo plazo. El formato de archivo PDF tradicional oferta muchas ventajas para el almacenamiento de documentos a largo plazo. La naturaleza compacta del PDF facilita la transferencia y conserva el espacio, y su naturaleza bien estructurada permite potentes funciones de indexación y búsqueda. Los archivos PDF tradicionales son muy compatibles con los metadatos y los archivos PDF tienen un largo historial de compatibilidad con distintos entornos informáticos.

Al igual que PDF, PDF/A es una especificación estándar ISO. Fue desarrollado por una fuerza de tarea que incluyó a AIIM (Asociación para la Gestión de la Información y la Imagen), NPES (National Printing Equipment Association) y la Oficina Administrativa de los Tribunales de los Estados Unidos. Dado que el objetivo de la especificación PDF/A es proporcionar un formato de archivo a largo plazo, muchas funciones de PDF se omiten para que los archivos puedan ser independientes. A continuación se indican algunos puntos clave sobre la especificación que mejoran la reproducibilidad a largo plazo del archivo PDF/A:

* Todo el contenido debe estar contenido en el archivo y no puede haber dependencia de fuentes externas como hipervínculos, fuentes o programas de software.
* Todas las fuentes deben estar incrustadas y deben ser fuentes que tengan una licencia de uso ilimitado para documentos electrónicos.
* JavaScript no está permitido
* No se permite la transparencia
* No se permite la codificación
* No se permite el contenido de audio y vídeo
* Los espacios de color deben definirse de forma independiente del dispositivo
* Todos los metadatos deben seguir ciertos estándares

## Visualización de un archivo PDF/A

Se crearon dos archivos en los archivos de ejemplo a partir del mismo archivo de Microsoft Word. Uno se creó como PDF tradicional y el otro como archivo PDF/A. Abra estos dos archivos en Acrobat Professional:

simpleWordFile.pdf
simpleWordFilePDFA.pdf

Aunque los documentos tienen el mismo aspecto, el archivo PDF/A se abre con una barra azul en la parte superior, lo que indica que está viendo este documento en modo PDF/A. Esta barra azul es la barra de mensajes de documento de Acrobat, que verá al abrir determinados tipos de archivos PDF.

![pdf-img](assets/pdfa-message.png)

La barra de mensajes de documento incluye instrucciones, y posiblemente botones, para ayudarle a completar una tarea. Tiene un código de color y verá el color azul al abrir tipos especiales de archivos PDF (como este archivo PDF/A), así como archivos PDF certificados y firmados digitalmente. La barra cambia a púrpura para PDF forms y a amarillo cuando participa en una revisión de PDF.

>[!NOTE]
> Si hace clic en Activar edición, eliminará este documento de la compatibilidad con PDF/A.




