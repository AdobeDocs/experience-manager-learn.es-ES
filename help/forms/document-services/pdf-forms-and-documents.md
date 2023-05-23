---
title: Comprender los distintos tipos de PDF forms y documentos
description: PDF es en realidad una familia de formatos de archivo y en este artículo se describen los tipos de PDF que son importantes y relevantes para los desarrolladores de formularios.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: 6.4, 6.5
feature: PDF Generator
kt: 7071
topic: Development
exl-id: ffa9d243-37e5-420c-91dc-86c73a824083
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '1277'
ht-degree: 0%

---

# PDF

Portable Document Format (PDF) es en realidad una familia de formatos de archivo, y este artículo detalla los más relevantes para los desarrolladores de formularios. Muchos de los detalles técnicos y estándares de diferentes tipos de PDF están evolucionando y cambiando. Algunos de estos formatos y especificaciones son estándares de la Organización Internacional de Normalización (ISO), y algunos son propiedad intelectual específica propiedad del Adobe.

Este artículo muestra cómo crear varios tipos de PDF. Le ayuda a comprender cómo y por qué utilizar cada una de ellas. Todos estos tipos funcionan mejor en la herramienta de cliente principal para ver y trabajar con PDF: Adobe Acrobat DC.

A continuación se muestra un ejemplo de un archivo PDF/A en Acrobat DC.

![Pdfa](assets/pdfa-file-in-acrobat.png)

Los archivos de muestra pueden ser [descargado desde aquí](assets/pdf-file-types.zip)

## PDF de arquitectura de Forms XML (PDF XFA)

Adobe utiliza el término formulario de PDF XFA para hacer referencia al Forms interactivo y dinámico que crea con AEM Forms Designer. El Forms y los archivos que crea con Designer se basan en la arquitectura XML Forms de Adobe (XFA). En muchos aspectos, el formato de archivo XFA PDF está más cerca de un archivo HTML que de un archivo PDF tradicional. Por ejemplo, el siguiente código muestra el aspecto de un objeto de texto simple en un archivo de PDF XFA.

![Campo de texto](assets/text-field.JPG)

XFA Forms se basan en XML. Este formato bien estructurado y flexible permite a un servidor de AEM Forms transformar los archivos de Designer en distintos formatos, incluidos el PDF tradicional, el PDF/A y el HTML. Puede ver la estructura XML completa de Forms en Designer seleccionando la pestaña Fuente XML del Editor de diseño. Puede crear Forms XFA estático y dinámico en AEM Forms Designer.

## PDF estático

El diseño de los PDF forms XFA estáticos nunca cambia durante la ejecución, pero pueden ser interactivos para el usuario. Las siguientes son algunas ventajas de los PDF forms XFA estáticos:

* El diseño de los PDF forms XFA estáticos nunca cambia durante la ejecución, pero pueden ser interactivos para el usuario.
* El Forms estático admite las herramientas Comentario y Marcado de Acrobat.
* La Forms estática permite importar y exportar comentarios de Acrobat.
* La Forms estática admite la configuración secundaria de fuentes, que es una técnica que se puede realizar en un servidor de AEM Forms.
* El Forms estático se puede representar mediante el visualizador de PDF integrado que se incluye con los navegadores modernos.

>[!NOTE]
>
> Puede crear PDF estáticos con AEM Forms Designer guardando el XDP como Formulario de PDF estático de Adobe



### Dynamic Forms

Los PDF XFA dinámicos pueden cambiar su diseño durante la ejecución, por lo que las características de comentarios y marcado no son compatibles. Sin embargo, los PDF XFA dinámicos sí ofrecen las siguientes ventajas:

* Los formularios dinámicos admiten scripts del lado del cliente que cambian el diseño y la paginación del formulario. Por ejemplo, Orden de compra.xdp se expandirá y paginará para dar cabida a una cantidad interminable de datos si lo guarda como un formulario dinámico
* Los formularios dinámicos admiten todas las propiedades del formulario en tiempo de ejecución, mientras que los formularios estáticos solo admiten un subconjunto

>[!NOTE]
>
> Puede crear archivos PDF dinámicos utilizando AEM Forms Designer guardando el XDP como Formulario XML dinámico de Adobe

>[!NOTE]
>
> Los formularios dinámicos no se pueden procesar con los visualizadores de PDF integrados en los exploradores modernos.

### Archivo de PDF (PDF tradicional)

Un documento certificado proporciona a los destinatarios de Forms y a los documentos PDF garantías adicionales de su autenticidad e integridad.

El formato de PDF más popular y generalizado es el archivo de PDF tradicional. Existen muchas formas de crear un archivo de PDF tradicional, incluido el uso de Acrobat y muchas herramientas de terceros. Acrobat proporciona todas las formas siguientes de crear archivos de PDF tradicionales. Si no tiene Acrobat instalado, es posible que no vea estas opciones en el equipo.

* Capturando el flujo de impresión de una aplicación de escritorio: elija el comando Print de una aplicación de creación y seleccione el icono Adobe PDF printer. En lugar de una copia impresa del documento, habrá creado un archivo de PDF del documento
* Mediante el complemento Acrobat PDFMaker con aplicaciones de Microsoft Office: al instalar Acrobat, agrega un menú Adobe PDF a las aplicaciones de Microsoft Office y un icono a la cinta de opciones de Office. Puede utilizar estas funciones agregadas para crear archivos de PDF directamente en Microsoft Office
* Mediante Acrobat Distiller para convertir archivos Postscript y Postscript encapsulado (EPS) en PDF: Distiller se utiliza generalmente en la publicación de impresión y otros flujos de trabajo que requieren una conversión del formato Postscript al formato PDF
* Bajo el capó, un PDF tradicional es muy diferente a un PDF XFA. No tiene la misma estructura XML y, como se crea capturando el flujo de impresión de un archivo, un PDF tradicional es un archivo estático y de sólo lectura.

Un documento certificado proporciona a los destinatarios de formularios y documentos del PDF garantías adicionales de su autenticidad e integridad.

### Acroforms

Acroforms es la tecnología de formulario interactivo más antigua de Adobe; se remonta a la versión 3 de Acrobat. El Adobe proporciona el [Referencia de la API de Acrobat Forms](assets/FormsAPIReference.pdf), de mayo de 2003, para proporcionar los detalles técnicos de esta tecnología. Acroforms es una combinación de los siguientes elementos:

* PDF tradicional que define el diseño estático y los gráficos del formulario.
* Campos de formulario interactivos que se atornillan en la parte superior con las herramientas de formulario del programa Adobe Acrobat. Estas herramientas de formulario son un pequeño subconjunto de lo que está disponible en AEM Forms Designer.

### PDF/A (PDF para archivo)

PDF/A (PDF de archivos) se basa en las ventajas del almacenamiento de documentos de los PDF tradicionales con muchos detalles específicos que mejoran el archivado a largo plazo. El formato de archivo PDF tradicional ofrece muchas ventajas para el almacenamiento de documentos a largo plazo. La naturaleza compacta del PDF facilita la transferencia y ahorra espacio, y su naturaleza bien estructurada permite potentes capacidades de indexación y búsqueda. El PDF tradicional ofrece una amplia compatibilidad con los metadatos, y el PDF tiene una larga historia de compatibilidad con diferentes entornos informáticos.

Al igual que el PDF, el PDF/A es una especificación estándar ISO. Fue desarrollado por un grupo de trabajo que incluía AIIM (Asociación para la Gestión de la Información y la Imagen), NPES (Asociación Nacional de Equipos de Impresión) y la Oficina Administrativa de los Tribunales de Estados Unidos. Dado que el objetivo de la especificación PDF/A es proporcionar un formato de archivo a largo plazo, se omiten muchas funciones de PDF para que los archivos puedan ser independientes. A continuación se indican algunos puntos clave de la especificación que mejoran la reproducibilidad a largo plazo del archivo PDF/A:

* Todo el contenido debe estar contenido en el archivo y no puede haber dependencias de fuentes externas como hipervínculos, fuentes o programas de software.
* Todas las fuentes deben estar incrustadas y deben ser fuentes que tengan una licencia de uso ilimitado para documentos electrónicos.
* JavaScript no está permitido
* No se permite la transparencia
* No se permite el cifrado
* No se permite el contenido de audio y vídeo
* Los espacios de color deben definirse de forma independiente del dispositivo
* Todos los metadatos deben seguir ciertos estándares

### Visualización de un archivo de PDF/A

Se crearon dos archivos en los archivos de ejemplo a partir del mismo archivo de Microsoft Word. Uno se creó como PDF tradicional y el otro como archivo PDF/A. Abra estos dos archivos en Acrobat Professional:

* simpleWordFile.pdf
* simpleWordFilePDFA.pdf

Aunque los documentos tienen el mismo aspecto, el archivo PDF/A se abre con una barra azul en la parte superior, que indica que está viendo este documento en modo PDF/A. Esta barra azul es la barra de mensajes del documento de Acrobat, que se ve al abrir ciertos tipos de archivos de PDF.

![Pdf-img](assets/pdfa-message.png)

La barra de mensajes del documento incluye instrucciones y, posiblemente, botones para ayudarle a completar una tarea. Tiene un código de colores y verá el color azul cuando abra tipos especiales de PDF (como este archivo PDF/A), así como PDF certificados y firmados digitalmente. La barra cambia a morada para los PDF forms y a amarilla cuando participa en una revisión de PDF.

>[!NOTE]
>
> Si hace clic en Habilitar edición, elimina este documento de la compatibilidad con PDF/A.
