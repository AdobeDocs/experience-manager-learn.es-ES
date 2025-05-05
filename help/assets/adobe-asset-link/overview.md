---
title: Adobe Asset Link y AEM
description: Los diseñadores y los usuarios creativos pueden utilizar los recursos de Adobe Experience Manager en sus aplicaciones de escritorio de Adobe Creative Cloud favoritas. La extensión Adobe Asset Link para Adobe Creative Cloud para empresas amplía la capacidad de buscar, examinar, ordenar, previsualizar, cargar recursos, verificar, modificar, registrar y ver metadatos de recursos de AEM en herramientas de Creative Cloud como Adobe XD, Photoshop, InDesign y Illustrator.
feature: Adobe Asset Link
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Content Management
role: User
level: Beginner
thumbnail: 28988.jpg
duration: 673
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '983'
ht-degree: 1%

---


# Adobe Asset Link 3.0

Los diseñadores y los usuarios creativos pueden utilizar los recursos de Adobe Experience Manager en sus aplicaciones de escritorio de Adobe Creative Cloud favoritas.

La extensión Adobe Asset Link para Adobe Creative Cloud para empresas amplía la capacidad de buscar, examinar, ordenar, previsualizar, cargar recursos, verificar, modificar, registrar y ver metadatos de recursos de AEM en aplicaciones de Creative Cloud.

## Funciones de Adobe Asset Link

+ Adobe Asset Link se integra con AEM Assets y Assets Essentials.
+ Adobe Asset Link configura automáticamente la conexión con entornos de AEM basados en la nube (AEM Assets, as a Cloud Service y Assets Essentials)
+ Adobe Asset Link es una extensión que funciona dentro de las aplicaciones de Adobe Creative Cloud:

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ Autenticación automática en AEM con su Adobe Enterprise ID o Federated ID
+ Busque recursos digitales en AEM
+ Acceda a los detalles de archivo de los recursos que residen en AEM desde el panel:
   + Miniatura   
   + Metadatos básicos
   + Versiones
+ Coloque, descargue o arrastre y suelte recursos en su diseño
+ Modifique los recursos desprotegiéndolos de AEM y trabajando en ellos (WIP) en su cuenta de Creative Cloud Assets
+ Vuelva a registrar un recurso en AEM después de que haya terminado de modificarlo y se refleje la nueva versión en AEM
+ Busque recursos en AEM desde el panel en la aplicación de Adobe Asset Link
+ Examine colecciones de AEM Assets y colecciones inteligentes directamente desde el panel Asset Link
+ Añadir recursos recién creados a AEM directamente desde el panel
+ Arrastre y suelte recursos directamente en marcos de InDesign

## Colocación de recursos en InDesign

Adobe Asset Link proporciona compatibilidad con la vinculación directa de InDesign entre Adobe Asset Link y AEM. Con la compatibilidad con la vinculación directa de InDesign, puede colocar (__Colocar elementos vinculados__ o __Colocar copia__) o arrastrar y soltar recursos digitales en InDesign desde AEM a través del panel Adobe Asset Link. Además, presenta la representación *Solo para ubicación+ (FPO).

>[!VIDEO](https://video.tv.adobe.com/v/37233?quality=12&learn=on&captions=spa)

>[!NOTE]
>
>Utilice solo su Adobe Creative Cloud Enterprise ID o Federated ID. Asegúrese de [configurar AEM para Adobe Asset Link](https://helpx.adobe.com/es/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html).

Puede colocar un recurso en el diseño de InDesign mediante una de las siguientes opciones:

+ **Colocar copia**: al incrustar un recurso (con la opción Colocar copia ), se coloca una copia del recurso original en el diseño de InDesign después de descargar los binarios en el sistema local. Adobe Asset Link no mantiene ningún vínculo entre la copia incrustada y el recurso original. Si el recurso original se modifica en AEM, debe eliminar el recurso incrustado del archivo de InDesign y volver a incrustar el recurso de AEM.

+ **Colocar elementos vinculados**: al trabajar con documentos de InDesign, tiene la opción de hacer referencia a los recursos de AEM, además de incrustarlos directamente (mediante la opción Colocar copia del menú contextual). La referencia a recursos le permite colaborar con otros usuarios e incorporar cualquier actualización realizada en el recurso original en AEM. Para hacer referencia a un recurso desde AEM, utilice la opción Colocar vínculo en el menú contextual.

### Para imágenes solo de ubicación

Cuando se colocan archivos de recursos grandes en documentos de InDesign desde AEM mediante Adobe Asset Link, los usuarios creativos deben esperar unos segundos después de iniciar la operación de colocación. Esto afecta a la experiencia general del usuario. Con Adobe Asset Link, puede colocar temporalmente una imagen de baja resolución del recurso original de AEM, lo que reduce el tiempo necesario para colocar una imagen. Al mismo tiempo, aumenta la experiencia general del usuario y la productividad. La imagen de menor resolución se coloca temporalmente y cuando se requiere la salida final para imprimir o publicar, debe reemplazar las representaciones de FPO por las originales. Si desea reemplazar varias imágenes de FPO con las imágenes originales respectivas, vaya al panel **_Windows > Vínculos_** y, a continuación, descargue los recursos originales. Una vez descargadas las imágenes originales, elija Reemplazar todos los FPO por originales.

Las representaciones de FPO son sustitutos ligeros de los recursos originales. Tienen la misma proporción de aspecto, pero su tamaño es menor que el de las imágenes originales. Actualmente, InDesign solo admite la importación de representaciones FPO para los siguientes tipos de imagen:

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

Si no hay una representación de FPO disponible para un recurso específico en AEM, se hace referencia al recurso original de alta resolución en su lugar. Para las imágenes de FPO, el estado de FPO se muestra en el panel Vínculos de InDesign.

## Autenticación de Adobe Asset Link con AEM Assets

Cómo funciona la autenticación de Adobe Asset Link en el contexto de Adobe Identity Management Services (IMS) y Adobe Experience Manager Author.

![Arquitectura de Adobe Asset Link](assets/adobe-asset-link-article-understand.png)

1. La extensión Adobe Asset Link realiza una solicitud de autorización, a través de la aplicación Adobe Creative Cloud Desktop, al servicio Identity Manager de Adobe (IMS) y, una vez realizada la acción correctamente, recibe un token de portador.
1. La extensión Adobe Asset Link se conecta a AEM Author a través de HTTP(S), incluido el token de portador obtenido en **Paso 1**, mediante el esquema (HTTP/HTTPS), el host y el puerto proporcionados en la configuración JSON de la extensión.
1. El controlador de autenticación del portador de AEM extrae el token de portador de la solicitud y lo valida con Adobe IMS.
1. Una vez que Adobe IMS valida el token de portador, se crea un usuario en AEM (si aún no existe) y se sincronizan los datos de perfil y grupo/pertenencias de Adobe IMS. Al usuario de AEM se le emite un token de inicio de sesión de AEM estándar, que se devuelve a la extensión Adobe Asset Link como una cookie en la respuesta HTTP(S).
1. Interacciones posteriores (por ejemplo, explorar, buscar, registrar/retirar recursos, etc.) con la extensión Adobe Asset Link da como resultado solicitudes HTTP(S) al autor de AEM que se validan mediante el token de inicio de sesión de AEM, usando el controlador de autenticación de token de AEM estándar.

>[!NOTE]
>
>Al expirar el token de inicio de sesión, **los pasos del 1 al 5** invocarán automáticamente, autenticarán la extensión de Adobe Asset Link mediante el token de portador y volverán a emitir un token de inicio de sesión nuevo y válido.

## Recursos adicionales

+ [Sitio web de Adobe Asset Link](https://www.adobe.com/es/creativecloud/business/enterprise/adobe-asset-link.html)
