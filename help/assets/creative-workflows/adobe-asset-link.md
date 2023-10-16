---
title: Adobe AEM Asset Link y el servicio de asistencia
description: Los diseñadores y los usuarios creativos pueden utilizar los recursos de Adobe Experience Manager en sus aplicaciones de escritorio de Adobe Creative Cloud favoritas. La extensión Adobe Asset Link para Adobe Creative Cloud for enterprise AEM amplía la capacidad de buscar, examinar, ordenar, previsualizar, cargar recursos, extraer, modificar, registrar y ver metadatos de recursos en herramientas de Creative Cloud como Adobe XD, Photoshop, InDesign y Illustrator.
feature: Adobe Asset Link
version: 6.4, 6.5, Cloud Service
topic: Content Management
role: User
level: Beginner
thumbnail: 28988.jpg
jira: KT-8413, KT-3707
last-substantial-update: 2022-06-25T00:00:00Z
exl-id: 6c49f8c2-f468-4b29-b7b6-029c8ab39ce9
source-git-commit: 8dfc538e93ea5dc114cf0a5d57dd82d924e476ff
workflow-type: tm+mt
source-wordcount: '1047'
ht-degree: 2%

---

# Adobe Asset Link 3.0

Los diseñadores y los usuarios creativos pueden utilizar los recursos de Adobe Experience Manager en sus aplicaciones de escritorio de Adobe Creative Cloud favoritas.

La extensión de Adobe Asset Link para Adobe Creative Cloud for enterprise AEM amplía la capacidad de buscar, examinar, ordenar, previsualizar, cargar recursos, verificar, modificar, registrar y ver metadatos de recursos en aplicaciones de Creative Cloud.

>[!TIP]
>
> Obtenga más información acerca de cómo [Programa de formación de Adobe XD Premium](https://helpx.adobe.com/support/xd.html) puede ayudarle a integrar Asset Link con su flujo de trabajo de Adobe Experience Manager.

## Adobe AEM Asset Link y flujos de trabajo creativos de

El siguiente vídeo ilustra un flujo de trabajo común utilizado por los creativos que trabajan en aplicaciones de Adobe Creative Cloud AEM y que se integran directamente con los usuarios mediante el uso de Adobe Asset Link.

>[!VIDEO](https://video.tv.adobe.com/v/335927?quality=12&learn=on)

## Funciones de Adobe Asset Link

+ Adobe Asset Link se integra con AEM Assets y Assets Essentials.
+ Adobe AEM Asset Link configura automáticamente la conexión con entornos de trabajo basados en la nube (entornos de trabajo basados en la nube, as a Cloud Service y Assets Essentials de AEM Assets).
+ Adobe Asset Link es una extensión que funciona dentro de las aplicaciones de Adobe Creative Cloud:

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ AEM Autenticación automática a los mediante su Enterprise ID o Federated ID de Adobe
+ AEM Busque y examine recursos digitales en el sector de la
+ AEM Acceda a los detalles del archivo de los recursos que residen en el desde el panel:
   + Miniatura   
   + Metadatos básicos
   + Versiones
+ Coloque, descargue o arrastre y suelte recursos en su diseño
+ AEM Modifique los recursos desprotegiéndolos de la cuenta de recursos de Creative Cloud y trabajando en ellos (WIP) en la cuenta de recursos de la cuenta de trabajo en curso (WIP)
+ AEM AEM Vuelva a registrar un recurso en la lista de recursos después de que haya terminado de modificarlo y la nueva versión se reflejará en la nueva versión de la lista de recursos
+ AEM Busque recursos en la aplicación desde el panel en la aplicación Vínculo de recursos de Adobe.
+ Examine colecciones AEM Assets y colecciones inteligentes directamente desde el panel Asset Link
+ AEM Añada los recursos recién creados para que se directamente desde el panel
+ Arrastrar y soltar recursos directamente en marcos de InDesign

## Colocación de recursos en el InDesign

Adobe Asset Link proporciona compatibilidad con vínculos directos de InDesign entre Adobe AEM Asset Link y el servicio de enlace de recursos de la. Con la compatibilidad con la vinculación directa de InDesign, puede colocar (__Colocar elemento vinculado__ o __Colocar copia__) o arrastre y suelte recursos digitales en el InDesign AEM desde el panel Vínculo de recursos de Adobe (el que se encuentra en la barra de herramientas) desde la pantalla de. Además, presenta la representación *Solo para ubicación+ (FPO).

>[!VIDEO](https://video.tv.adobe.com/v/28988?quality=12&learn=on)

>[!NOTE]
>
>Utilice únicamente su Enterprise ID o Federated ID de Adobe Creative Cloud. Asegúrese de que [AEM configurar para vínculo de recursos de Adobe](https://helpx.adobe.com/es/enterprise/using/adobe-asset-link.html).

Puede colocar un recurso en el diseño del InDesign mediante una de las siguientes opciones:

+ **Colocar copia** : La incrustación de un recurso (con la opción Colocar copia ) coloca una copia del recurso original en el diseño de InDesign después de descargar los binarios en el sistema local. Adobe Asset Link no mantiene ningún vínculo entre la copia incrustada y el recurso original. AEM Si el recurso original se modifica en la, debe eliminar el recurso incrustado del archivo de InDesign AEM y volver a incrustar el recurso desde la vista de datos de la.

+ **Colocar elemento vinculado** : al trabajar con documentos de InDesign AEM, tiene la opción de hacer referencia a los recursos desde el, además de incrustar directamente los recursos (mediante la opción Colocar copia del menú contextual). AEM La referencia a recursos le permite colaborar con otros usuarios e incorporar cualquier actualización realizada en el recurso original en la documentación de los recursos de la aplicación de la. AEM Para hacer referencia a un recurso desde el menú contextual, utilice la opción Colocar vinculado.

### Para imágenes solo de ubicación

Cuando los archivos de recursos grandes se colocan en documentos de InDesign AEM Adobe desde el uso de Asset Link, los usuarios creativos deben esperar unos segundos después de iniciar la operación de colocación. Esto afecta a la experiencia general del usuario. Con Adobe AEM Asset Link, puede colocar temporalmente una imagen de baja resolución del recurso original de forma que se reduzca el tiempo necesario para colocar una imagen. Al mismo tiempo, aumenta la experiencia general del usuario y la productividad. La imagen de menor resolución se coloca temporalmente y cuando se requiere la salida final para imprimir o publicar, debe reemplazar las representaciones de FPO por las originales. Si desea reemplazar varias imágenes de FPO con las imágenes originales respectivas, vaya a **_Windows > Vínculos_** y, a continuación, descargue los recursos originales. Una vez descargadas las imágenes originales, elija Reemplazar todos los FPO por originales.

Las representaciones de FPO son sustitutos ligeros de los recursos originales. Tienen la misma proporción de aspecto, pero su tamaño es menor que el de las imágenes originales. Actualmente, InDesign solo admite la importación de representaciones FPO para los siguientes tipos de imagen:

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

AEM Si no hay una representación de FPO disponible para un recurso específico en la lista de recursos, se hace referencia al recurso original de alta resolución en su lugar. Para las imágenes de FPO, el estado de FPO se muestra en el panel Vínculos de InDesign.

## Adobe de la autenticación de Asset Link con AEM Assets

Funcionamiento de la autenticación de Adobe Asset Link en el contexto de Adobe Identity Management Services (IMS) y Adobe Experience Manager Author.

![Adobe Asset Link Architecture](assets/adobe-asset-link-article-understand.png)

1. La extensión de Adobe Asset Link realiza una solicitud de autorización, a través de la aplicación Adobe Creative Cloud Desktop, para almacenar en Adobe el servicio Identity Manager (IMS) y, una vez realizada la acción correctamente, recibe un token de portador.
1. La extensión de vínculo de recursos de Adobe AEM conecta con el autor de la aplicación a través de HTTP (S), incluido el token de portador obtenido en **Paso 1**, utilizando el esquema (HTTP/HTTPS), el host y el puerto proporcionados en la configuración JSON de la extensión.
1. AEM El controlador de autenticación de portador extrae el token de portador de la solicitud y lo valida con Adobe IMS.
1. AEM Una vez que Adobe IMS valida el token de portador, se crea un usuario en (si aún no existe) y se sincronizan los datos de perfil y grupo/pertenencias de Adobe IMS. AEM AEM Al usuario se le emite un token de inicio de sesión de estándar, que se devuelve a la extensión de vínculo de recursos de Adobe como una cookie en la respuesta HTTP(S).
1. Interacciones posteriores (por ejemplo, navegación, búsqueda, registro/salida de recursos, etc.) con la extensión Asset Link de Adobe AEM AEM AEM da como resultado solicitudes HTTP(S) a Autor de la que se validan mediante el token de inicio de sesión de la, utilizando el controlador de autenticación de token estándar.

>[!NOTE]
>
>Al expirar el token de inicio de sesión, **Pasos 1-5** invocará automáticamente, autenticará la extensión de Adobe Asset Link con el token de portador y volverá a emitir un token de inicio de sesión nuevo y válido.

## Recursos adicionales

+ [Sitio web de Adobe Asset Link](https://www.adobe.com/es/creativecloud/business/enterprise/adobe-asset-link.html)
