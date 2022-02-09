---
title: Comprender los distintos tipos de PDF forms y documentos
description: PDF es en realidad una familia de formatos de archivo, y este artículo describe los tipos de PDF que son importantes y relevantes para los desarrolladores de formularios.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: 6.3,6.4, 6.5
feature: PDF Generator
kt: 7071
topic: Development
exl-id: ffa9d243-37e5-420c-91dc-86c73a824083
source-git-commit: 228da29e7ac0d61359c2b94131495b5b433a09dc
workflow-type: tm+mt
source-wordcount: '1277'
ht-degree: 0%

---

# PDF

El formato de documento portátil (PDF) es en realidad una familia de formatos de archivo, y este artículo detalla los más relevantes para los desarrolladores de formularios. Muchos de los detalles técnicos y estándares de diferentes tipos de PDF están evolucionando y cambiando. Algunos de estos formatos y especificaciones son normas de la Organización Internacional de Normalización (ISO), y algunos son propiedad intelectual específica del Adobe.

Este artículo muestra cómo crear varios tipos de PDF. Le ayuda a comprender cómo y por qué utilizar cada uno de ellos. Todos estos tipos funcionan mejor en la herramienta de cliente más importante para ver y trabajar con PDF: Adobe Acrobat DC.

A continuación se muestra un ejemplo de un archivo PDF/A en Acrobat DC.

![Pdfa](assets/pdfa-file-in-acrobat.png)

Los archivos de muestra pueden [descargado desde aquí](assets/pdf-file-types.zip)

## PDF de arquitectura de Forms XML (PDF XFA)

Adobe utiliza el término formulario de PDF XFA para hacer referencia al Forms dinámico e interactivo que crea con AEM Forms Designer. El Forms y los archivos que crea con Designer se basan en XML Forms Architecture (XFA) de Adobe. En muchos aspectos, el formato de archivo del PDF XFA está más cerca de un archivo de HTML que de un archivo de PDF tradicional. Por ejemplo, el siguiente código muestra el aspecto de un objeto de texto simple en un archivo PDF XFA.

![Campo de texto](assets/text-field.JPG)

XFA Forms se basa en XML. Este formato flexible y bien estructurado permite a un servidor de AEM Forms transformar los archivos de Designer en distintos formatos, incluidos el PDF tradicional, el PDF/A y el HTML. Puede ver la estructura XML completa de Forms en Designer seleccionando la ficha Código fuente XML en el Editor de presentaciones. Puede crear XFA Forms estático y dinámico en AEM Forms Designer.

## PDF estático

El diseño estático de los PDF forms XFA nunca cambia en el tiempo de ejecución, pero puede ser interactivo para el usuario. Las siguientes son algunas ventajas de los PDF forms XFA estáticos:

* El diseño estático de los PDF forms XFA nunca cambia en el tiempo de ejecución, pero puede ser interactivo para el usuario.
* Forms estático es compatible con las herramientas de Comentario y marcado de Acrobat.
* Forms estático permite importar y exportar comentarios de Acrobat.
* Forms estático admite la subconfiguración de fuentes, que es una técnica que se puede realizar en un servidor de AEM Forms.
* El Forms estático se puede representar con el visor de PDF integrado que viene con los navegadores modernos.

>[!NOTE]
>
> Puede crear PDF estáticos con AEM Forms Designer guardando el XDP como formulario de PDF estático de Adobe



### Forms dinámico

Los PDF XFA dinámicos pueden cambiar su diseño durante la ejecución, por lo que las funciones de comentarios y marcado no son compatibles. Sin embargo, los PDF XFA dinámicos ofrecen las siguientes ventajas:

* Los formularios dinámicos admiten secuencias de comandos del lado del cliente que cambian la presentación y la paginación del formulario. Por ejemplo, Purchase Order.xdp se expandirá y paginará para dar cabida a una cantidad infinita de datos si los guarda como un formulario dinámico
* Los formularios dinámicos admiten todas las propiedades del formulario durante la ejecución, mientras que los formularios estáticos solo admiten un subconjunto

>[!NOTE]
>
> Puede crear pdfs dinámicos con AEM Forms Designer guardando el XDP como Adobe Formulario XML dinámico

>[!NOTE]
>
> Los formularios dinámicos no se pueden procesar con los visualizadores pdf integrados de los navegadores modernos.

### Archivo PDF (PDF tradicional)

Un documento certificado proporciona a los destinatarios de Forms y al documento PDF garantías adicionales de su autenticidad e integridad.

El formato de PDF más popular y generalizado es el archivo de PDF tradicional. Existen varias formas de crear un archivo PDF tradicional, incluido el uso de Acrobat y muchas herramientas de terceros. Acrobat proporciona todas las formas siguientes de crear archivos PDF tradicionales. Si no tiene Acrobat instalado, es posible que no vea estas opciones en el equipo.

* Al capturar el flujo de impresión de una aplicación de escritorio: Elija el comando Imprimir de una aplicación de creación y seleccione el icono de impresora de Adobe PDF. En lugar de una copia impresa del documento, habrá creado un archivo PDF del documento
* Mediante el uso del complemento Acrobat PDFMaker con aplicaciones de Microsoft Office: Al instalar Acrobat, se agrega un menú de Adobe PDF a las aplicaciones de Microsoft Office y un icono a la cinta de Office. Puede utilizar estas funciones agregadas para crear archivos de PDF directamente en Microsoft Office
* Mediante el uso de Acrobat Distiller para convertir archivos Postscript y Postscript encapsulado (EPS) en PDF: Distiller se utiliza normalmente en la publicación de impresión y en otros flujos de trabajo que requieren una conversión del formato Postscript al formato de PDF
* Bajo el capó, un PDF tradicional es muy diferente a un PDF XFA. No tiene la misma estructura XML y, como se crea capturando el flujo de impresión de un archivo, un PDF tradicional es un archivo estático y de solo lectura.

Un documento certificado proporciona a los destinatarios de documentos PDF y formularios garantías adicionales de su autenticidad e integridad.

### Acróformularios

Las formas son la tecnología de formulario interactivo más antigua de Adobe; datan de la versión 3 de Acrobat. El Adobe proporciona la variable [Referencia de la API de Acrobat Forms](assets/FormsAPIReference.pdf), de mayo de 2003, para facilitar los detalles técnicos de esta tecnología. Las formas son una combinación de los siguientes elementos:

* PDF tradicional que define la presentación estática y los gráficos del formulario.
* Campos de formulario interactivos con tornillos en la parte superior con las herramientas de formulario del programa Adobe Acrobat. Estas herramientas de formulario son un pequeño subconjunto de lo que está disponible en AEM Forms Designer.

### PDF/A (PDF para archivo)

El PDF/A (PDF de Archivos) se basa en las ventajas de almacenamiento de documentos de los PDF tradicionales con muchos detalles específicos que mejoran el archiving a largo plazo. El formato de archivo PDF tradicional ofrece muchas ventajas para el almacenamiento de documentos a largo plazo. La naturaleza compacta del PDF facilita la transferencia fácil y conserva el espacio, y su naturaleza bien estructurada permite potentes funciones de indexación y búsqueda. El PDF tradicional cuenta con un amplio soporte para los metadatos y el PDF tiene un largo historial de soporte para diferentes entornos informáticos.

Al igual que el PDF, el PDF/A es una especificación estándar ISO. Fue desarrollado por un grupo de trabajo que incluía AIIM (Asociación para la Administración de la Información y la Imagen), NPES (Asociación Nacional de Equipos de Impresión) y la Oficina Administrativa de los Tribunales de Estados Unidos. Dado que el objetivo de la especificación de PDF/A es proporcionar un formato de archivo a largo plazo, se omiten muchas funciones de PDF para que los archivos puedan ser independientes. A continuación se indican algunos puntos clave sobre la especificación que mejoran la reproducibilidad a largo plazo del archivo PDF/A:

* Todo el contenido debe estar contenido en el archivo y no puede haber dependencias de fuentes externas como hipervínculos, fuentes o programas de software.
* Todas las fuentes deben estar incrustadas, y deben ser fuentes que tengan una licencia de uso ilimitado para documentos electrónicos.
* JavaScript no está permitido
* No se permite la transparencia
* No se permite la codificación
* No se permite el contenido de audio y vídeo
* Los espacios de color deben definirse de forma independiente del dispositivo
* Todos los metadatos deben seguir ciertos estándares

### Visualización de un archivo PDF/A

Se han creado dos archivos en los archivos de ejemplo desde el mismo archivo de Microsoft Word. Uno se creó como PDF tradicional y el otro como archivo PDF/A. Abra estos dos archivos en Acrobat Professional:

* simpleWordFile.pdf
* simpleWordFilePDFA.pdf

Aunque los documentos tienen el mismo aspecto, el archivo PDF/A se abre con una barra azul en la parte superior, lo que indica que está viendo este documento en modo PDF/A. Esta barra azul es la barra de mensajes del documento de Acrobat, que se ve al abrir ciertos tipos de archivos PDF.

![Pdf-img](assets/pdfa-message.png)

La barra de mensajes del documento incluye instrucciones y posiblemente botones para ayudarle a completar una tarea. Está codificado por colores y verá el color azul cuando abra tipos especiales de PDF (como este archivo de PDF/A), así como PDF certificados y firmados digitalmente. La barra cambia a morada para los PDF forms y a amarilla cuando participa en una revisión para PDF.

>[!NOTE]
>
> Si hace clic en Habilitar edición, quita este documento de conformidad con el PDF/A.
