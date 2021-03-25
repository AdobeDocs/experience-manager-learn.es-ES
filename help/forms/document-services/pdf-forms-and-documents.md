---
title: Comprender los distintos tipos de PDF forms y documentos
description: PDF es en realidad una familia de formatos de archivo, y este artículo describe los tipos de PDF que son importantes y relevantes para los desarrolladores de formularios.
solution: Experience Manager Forms
product: aem
type: Documentación
role: Desarrollador
level: Principiante,Intermedio
version: 6.3,6.4,6.5
feature: Servicios de documento
kt: 7071
topic: Desarrollo
translation-type: tm+mt
source-git-commit: d9799acb28dfc3c9767374798828754d5a50831f
workflow-type: tm+mt
source-wordcount: '1700'
ht-degree: 0%

---


# PDF

El formato de documento portátil (PDF) es en realidad una familia de formatos de archivo, y este artículo detalla los más relevantes para los desarrolladores de formularios. Muchos de los detalles técnicos y estándares de diferentes tipos de PDF están evolucionando y cambiando. Algunos de estos formatos y especificaciones son normas de la Organización Internacional de Normalización (ISO), y algunos son propiedad intelectual específica del Adobe.

Este artículo muestra cómo crear varios tipos de PDF. Le ayuda a comprender cómo y por qué utilizar cada uno de ellos. Todos estos tipos funcionan mejor en la herramienta de cliente más importante para ver y trabajar con archivos PDF: Adobe Acrobat DC.

A continuación se muestra un ejemplo de archivo PDF/A en Acrobat DC.

![Pdfa](assets/pdfa-file-in-acrobat.png)

Los archivos de muestra se pueden [descargar desde aquí](assets/pdf-file-types.zip)

## XML Forms Architecture PDF

Adobe utiliza el término formulario PDF para referirse al Forms interactivo y dinámico que crea con AEM Forms Designer. El Forms y los archivos que crea con Designer se basan en XML Forms Architecture (XFA) de Adobe. En muchos aspectos, el formato de archivo PDF XFA está más cerca de un archivo HTML que de un archivo PDF tradicional. Por ejemplo, el siguiente código muestra el aspecto de un objeto de texto simple en un archivo PDF XFA.

![Campo de texto](assets/text-field.JPG)

XFA Forms se basa en XML. Este formato flexible y bien estructurado permite que un servidor de AEM Forms transforme los archivos de Designer en distintos formatos, incluidos PDF, PDF/A y HTML tradicionales. Puede ver la estructura XML completa de Forms en Designer seleccionando la ficha Código fuente XML en el Editor de presentaciones. Puede crear XFA Forms estático y dinámico en AEM Forms Designer.

## PDF estático

El diseño estático de los PDF forms XFA nunca cambia en el tiempo de ejecución, pero puede ser interactivo para el usuario. Las siguientes son algunas ventajas de los PDF forms XFA estáticos:

* El diseño estático de los PDF forms XFA nunca cambia en el tiempo de ejecución, pero puede ser interactivo para el usuario.
* Forms estático es compatible con las herramientas de Comentario y marcado de Acrobat.
* Forms estático permite importar y exportar comentarios de Acrobat.
* Forms estático admite la subconfiguración de fuentes, que es una técnica que se puede realizar en un servidor de AEM Forms.
* El Forms estático se puede representar con el visor de PDF integrado que viene con los navegadores modernos.

>[!NOTE]
>
> Puede crear PDF estáticos con AEM Forms Designer guardando el XDP como formulario PDF estático de Adobe

## Formatos PDF

El formato de documento portátil (PDF) es en realidad una familia de formatos de archivo, y este artículo detalla los más relevantes para los desarrolladores de formularios. Muchos de los detalles técnicos y estándares de diferentes tipos de PDF están evolucionando y cambiando. Algunos de estos formatos y especificaciones son normas de la Organización Internacional de Normalización (ISO), y algunos son propiedad intelectual específica del Adobe.

Este artículo muestra cómo crear varios tipos de PDF. Le ayudará a comprender cómo y por qué utilizar cada uno de ellos. Todos estos tipos funcionan mejor en la herramienta de cliente más importante para ver y trabajar con archivos PDF: Adobe Acrobat DC.

Este es un ejemplo de archivo PDF/A en Acrobat DC.

![pdfa](assets/pdfa-file-in-acrobat.png)

Los archivos de muestra se pueden [descargar desde aquí](assets/pdf-file-types.zip)

### PDF XFA

Adobe utiliza el término formulario PDF para referirse a los formularios interactivos y dinámicos que crea con AEM Forms Designer. Es importante tener en cuenta que hay otro tipo de formulario PDF, denominado Acroform, que es diferente de los PDF forms que crea en AEM Forms Designer. Los formularios y archivos que cree con Designer se basan en XML Forms Architecture (XFA) de Adobe. En muchos aspectos, el formato de archivo PDF XFA está más cerca de un archivo HTML que de un archivo PDF tradicional. Por ejemplo, el siguiente código muestra cómo se ve un objeto de texto simple en un archivo PDF XFA.

![text-field](assets/text-field.JPG)

Como puede ver, los formularios XFA están basados en XML. Este formato flexible y bien estructurado permite a un servidor de AEM Forms transformar los archivos de Designer en distintos formatos, incluidos los PDF, PDF/A y HTML tradicionales. Puede ver la estructura XML completa de los formularios en Designer seleccionando la ficha Código fuente XML del Editor de presentaciones. Puede crear formularios XFA estáticos y dinámicos en AEM Forms Designer.

### PDF estático

Los PDF forms XFA estáticos no cambiarán su diseño en tiempo de ejecución, pero pueden ser interactivos para el usuario. Las siguientes son algunas ventajas de los PDF forms XFA estáticos:

* Los PDF forms XFA estáticos no cambiarán su diseño en tiempo de ejecución, pero pueden ser interactivos para el usuario.
* Los formularios estáticos admiten las herramientas de Comentario y Marcado de Acrobat.
* Los formularios estáticos permiten importar y exportar comentarios de Acrobat.
* Los formularios estáticos admiten la subconfiguración de fuente, que es una técnica que se puede realizar en un servidor AEM Forms.
* Los formularios estáticos se pueden procesar con el visor pdf incorporado que viene con los navegadores modernos.

>[!NOTE]
> Puede crear pdfs estáticos con AEM Forms Designer guardando el XDP como formulario PDF estático de Adobe

### Forms dinámico

Los PDF XFA dinámicos pueden cambiar su diseño durante la ejecución, por lo que las funciones de comentarios y marcado no son compatibles. Sin embargo, los PDF XFA dinámicos sí ofrecen las siguientes ventajas:

* Los formularios dinámicos admiten secuencias de comandos del lado del cliente que cambian la presentación y la paginación del formulario. Por ejemplo, Purchase Order.xdp se expandirá y paginará para dar cabida a una cantidad infinita de datos si los guarda como un formulario dinámico
* Los formularios dinámicos admiten todas las propiedades del formulario durante la ejecución, mientras que los formularios estáticos solo admiten un subconjunto

>[!NOTE]
>
> Puede crear pdfs dinámicos con AEM Forms Designer guardando el XDP como Adobe Formulario XML dinámico

>[!NOTE]
>
> Los formularios dinámicos no se pueden procesar con los visualizadores pdf integrados de los navegadores modernos.

### Archivo PDF (PDF tradicional)

Un documento certificado proporciona a los documentos PDF y a los destinatarios de Forms garantías adicionales de su autenticidad e integridad.

El formato PDF más popular y generalizado es el archivo PDF tradicional. Existen muchas formas de crear un archivo PDF tradicional, incluido el uso de Acrobat y muchas herramientas de terceros. Acrobat proporciona todas las formas siguientes de crear archivos PDF tradicionales. Si no tiene Acrobat instalado, es posible que no vea estas opciones en el equipo.

* Al capturar el flujo de impresión de una aplicación de escritorio: Elija el comando Imprimir de una aplicación de creación y seleccione el icono de impresora de Adobe PDF. En lugar de una copia impresa del documento, habrá creado un archivo PDF del documento
* Mediante el uso del complemento Acrobat PDFMaker con aplicaciones de Microsoft Office: Al instalar Acrobat, se agrega un menú Adobe PDF a las aplicaciones de Microsoft Office y un icono a la cinta de Office. Puede utilizar estas funciones agregadas para crear archivos PDF directamente en Microsoft Office
* Mediante Acrobat Distiller para convertir archivos Postscript y Postscript encapsulados (EPS) en PDF: Distiller se utiliza normalmente en la publicación de impresión y en otros flujos de trabajo que requieren una conversión del formato Postscript al formato PDF
* Bajo el capó, un PDF tradicional es muy diferente a un PDF XFA. No tiene la misma estructura XML y, como se crea capturando el flujo de impresión de un archivo, un PDF tradicional es un archivo estático y de solo lectura.

Un documento certificado proporciona a los destinatarios de documentos PDF y formularios garantías adicionales de su autenticidad e integridad.

### Acróformularios

Las formas son la tecnología de formulario interactivo más antigua de Adobe; datan de la versión 3 de Acrobat. Adobe proporciona la [Acrobat Forms API Reference](assets/FormsAPIReference.pdf), de mayo de 2003, para proporcionar los detalles técnicos de esta tecnología. Las formas son una combinación de la variable
los siguientes elementos:

* Un PDF tradicional que define la presentación estática y los gráficos del formulario.
* Campos de formulario interactivos con tornillos en la parte superior con las herramientas de formulario del programa Adobe Acrobat. Estas herramientas de formulario son un pequeño subconjunto de lo que está disponible en AEM Forms Designer.

### PDF/A (PDF para archivo)

PDF/A (PDF para archivos) se basa en las ventajas de almacenamiento de documentos de los PDF tradicionales con muchos detalles específicos que mejoran el archivado a largo plazo. El formato de archivo PDF tradicional ofrece muchas ventajas para el almacenamiento de documentos a largo plazo. La naturaleza compacta del PDF facilita la transferencia fácil y conserva el espacio, y su naturaleza bien estructurada permite potentes capacidades de indexación y búsqueda. El PDF tradicional es muy compatible con los metadatos, y el PDF tiene un largo historial de compatibilidad con distintos entornos informáticos.

Al igual que PDF, PDF/A es una especificación estándar ISO. Fue desarrollado por un grupo de trabajo que incluía AIIM (Asociación para la Administración de la Información y la Imagen), NPES (Asociación Nacional de Equipos de Impresión) y la Oficina Administrativa de los Tribunales de Estados Unidos. Dado que el objetivo de la especificación PDF/A es proporcionar un formato de archivo a largo plazo, muchas funciones PDF se omiten para que los archivos puedan ser independientes. A continuación se indican algunos puntos clave sobre la especificación que mejoran la reproducibilidad a largo plazo del archivo PDF/A:

* Todo el contenido debe estar contenido en el archivo y no puede haber dependencias de fuentes externas como hipervínculos, fuentes o programas de software.
* Todas las fuentes deben estar incrustadas, y deben ser fuentes que tengan una licencia de uso ilimitado para documentos electrónicos.
* JavaScript no está permitido
* No se permite la transparencia
* No se permite la codificación
* No se permite el contenido de audio y vídeo
* Los espacios de color deben definirse de forma independiente del dispositivo
* Todos los metadatos deben seguir ciertos estándares

### Visualización de un archivo PDF/A

Se crearon dos archivos en los archivos de ejemplo a partir del mismo archivo de Microsoft Word. Uno se creó como PDF tradicional y el otro como archivo PDF/A. Abra estos dos archivos en Acrobat Professional:

* simpleWordFile.pdf
* simpleWordFilePDFA.pdf

Aunque los documentos tienen el mismo aspecto, el archivo PDF/A se abre con una barra azul en la parte superior, lo que indica que está viendo este documento en modo PDF/A. Esta barra azul es la barra de mensajes del documento de Acrobat, que se ve al abrir ciertos tipos de archivos PDF.

![Pdf-img](assets/pdfa-message.png)

La barra de mensajes del documento incluye instrucciones y posiblemente botones para ayudarle a completar una tarea. Está codificado por colores y verá el color azul cuando abra tipos especiales de PDF (como este archivo PDF/A), así como PDF certificados y firmados digitalmente. La barra cambia a morada para los PDF forms y a amarilla cuando participa en una revisión de PDF.

>[!NOTE]
>
> Si hace clic en Activar edición, quita este documento del cumplimiento de PDF/A.




