---
title: Adobe Asset Link y AEM
description: Los diseñadores y los usuarios creativos pueden utilizar los recursos de Adobe Experience Manager en sus aplicaciones de escritorio favoritas de Adobe Creative Cloud. La extensión de vínculo de recursos de Adobe para Adobe Creative Cloud for enterprise amplía la capacidad para buscar, examinar, ordenar, previsualizar, cargar recursos, retirar, modificar, registrar y ver metadatos de AEM recursos en herramientas de Creative Cloud como Adobe XD, Photoshop, InDesign y Illustrator.
feature: Adobe Asset Link
version: 6.4, 6.5, Cloud Service
topic: Content Management
role: User
level: Beginner
thumbnail: 28988.jpg
exl-id: 6c49f8c2-f468-4b29-b7b6-029c8ab39ce9
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1051'
ht-degree: 1%

---

# Adobe Asset Link 3.0

Los diseñadores y los usuarios creativos pueden utilizar los recursos de Adobe Experience Manager en sus aplicaciones de escritorio favoritas de Adobe Creative Cloud.

La extensión de vínculo de recursos de Adobe para Adobe Creative Cloud for enterprise amplía la capacidad para buscar, examinar, ordenar, previsualizar, cargar recursos, extraer, modificar, registrar y ver metadatos de AEM recursos en aplicaciones de Creative Cloud.

>[!TIP]
>
> Obtenga más información sobre cómo [Programa de formación de Adobe XD Premium](https://spark.adobe.com/page/wU7OXv8qKGugO/) puede ayudarle a integrar Asset Link con su flujo de trabajo de Adobe Experience Manager.

## Adobe Asset Link y flujos de trabajo creativos AEM

El siguiente vídeo ilustra un flujo de trabajo común utilizado por los creativos que trabajan en aplicaciones de Adobe Creative Cloud e integran directamente con AEM mediante Adobe Asset Link.

>[!VIDEO](https://video.tv.adobe.com/v/335927/?quality=12&learn=on)

## Funciones de Adobe Asset Link

+ Adobe Asset Link se integra con AEM Assets y Assets Essentials.
+ Adobe Asset Link configura automáticamente la conexión a entornos de AEM basados en la nube (AEM Assets as a Cloud Service y Assets Essentials)
+ Adobe Asset Link es una extensión que funciona dentro de aplicaciones de Adobe Creative Cloud:

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ Autenticación automática para AEM con su Enterprise ID de Adobe o Federated ID
+ Busque recursos digitales en AEM
+ Obtenga acceso a los detalles de los archivos de los recursos que residen en AEM desde con el panel:
   + Miniatura   
   + Metadatos básicos
   + Versiones
+ Colocar, descargar o arrastrar y soltar recursos en su diseño
+ Modifique los activos desprotegiéndolos de AEM y trabajando en ellos (WIP) dentro de su cuenta de Creative Cloud Assets
+ Vuelva a comprobar un recurso en AEM después de que haya terminado de modificarlo y la nueva versión se refleje en AEM
+ Busque recursos en AEM desde el panel en la aplicación de Adobe Asset Link
+ Examinar colecciones inteligentes y de AEM Assets directamente desde el panel Vínculo de recursos
+ Agregue los recursos recién creados a AEM directamente desde el panel
+ Arrastrar y soltar recursos directamente en marcos de InDesign

## Colocación de recursos en InDesign

Adobe Asset Link proporciona compatibilidad de vinculación directa de InDesign entre Adobe Asset Link y AEM. Con la compatibilidad con la vinculación directa de InDesign, puede colocar (__Colocar elemento vinculado__ o __Colocar copia__) o arrastre y suelte recursos digitales en InDesign desde AEM a través del panel Adobe Asset Link . Además, introduce la representación *Solo para ubicación+ (FPO).

>[!VIDEO](https://video.tv.adobe.com/v/28988/?quality=12&learn=on)

>[!NOTE]
>
>Utilice únicamente su Enterprise ID o Federated ID de Adobe Creative Cloud. Asegúrese de que [configurar AEM para Adobe Asset Link](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html).

Puede colocar un recurso en el diseño de InDesign mediante una de las siguientes opciones:

+ **Colocar copia** - Al incrustar un recurso (mediante la opción Colocar copia ), se coloca una copia del recurso original en el diseño de InDesign después de descargar los binarios en el sistema local. Adobe Asset Link no mantiene ningún vínculo entre la copia incrustada y el recurso original. Si el recurso original se modifica en AEM, debe eliminarlo del archivo de InDesign y volver a incrustarlo desde AEM.

+ **Colocar elemento vinculado** : al trabajar con documentos de InDesign, tiene la opción de hacer referencia a los recursos de AEM además de incrustar directamente los recursos (mediante la opción Colocar copia en el menú contextual). Hacer referencia a recursos le permite colaborar con otros usuarios e incorporar cualquier actualización realizada en el recurso original en AEM. Para hacer referencia a un recurso desde AEM, utilice la opción Colocar vinculado en el menú contextual.

### Para imágenes solo de colocación

Cuando se colocan archivos de recursos grandes en documentos de InDesign desde AEM utilizando Adobe Asset Link, los usuarios creativos deben esperar unos segundos después de iniciar la operación de colocación. Esto afecta a la experiencia general del usuario. Con Adobe Asset Link puede colocar temporalmente una imagen de baja resolución del recurso original desde AEM, lo que reduce el tiempo necesario para colocar una imagen. Al mismo tiempo, aumenta la experiencia y la productividad del usuario en general. La imagen de menor resolución se coloca temporalmente y cuando se requiere la salida final para imprimir o publicar, se deben reemplazar las representaciones de FPO por las originales. Si desea reemplazar varias imágenes de FPO con las imágenes originales correspondientes, vaya a **_Windows > Vínculos_** y, a continuación, descargue los recursos originales. Una vez descargadas las imágenes originales, elija Reemplazar todas las FPO con originales.

Las representaciones de FPO son sustitutos ligeros de los recursos originales. Tienen la misma relación de aspecto, pero tienen un tamaño menor que las imágenes originales. Actualmente, InDesign solo admite la importación de representaciones de FPO para los siguientes tipos de imagen:

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

Si una representación de FPO no está disponible para un recurso específico en AEM, se hace referencia al recurso original de alta resolución en su lugar. Para las imágenes de FPO, el estado de FPO se muestra en el panel Vínculos de InDesign.

## Autenticación de Asset Link de Adobe con AEM Assets

Cómo funciona la autenticación de Adobe Asset Link en el contexto de Adobe Identity Management Services (IMS) y Adobe Experience Manager Author.

![Arquitectura de Asset Link de Adobe](assets/adobe-asset-link-article-understand.png)

1. La extensión de Adobe Asset Link realiza una solicitud de autorización, a través de Adobe Creative Cloud Desktop App, para almacenar en Adobe el servicio de ID Manage (IMS) y, cuando se realiza correctamente, recibe un token de portador.
1. La extensión de Adobe Asset Link se conecta a AEM Author a través de HTTP(S), incluido el token del portador obtenido en **Paso 1**, utilizando el esquema (HTTP/HTTPS), el host y el puerto proporcionados en la configuración JSON de la extensión.
1. AEM gestor de autenticación del portador extrae el token del portador de la solicitud y lo valida con Adobe IMS.
1. Una vez que Adobe IMS valida el token del portador, se crea un usuario en AEM (si aún no existe) y se sincronizan los datos de perfil y grupo/pertenencia de Adobe IMS. El usuario AEM recibe un token de inicio de sesión AEM estándar, que se devuelve a la extensión de vínculo de recursos de Adobe como una cookie en la respuesta HTTP(S).
1. Interacciones posteriores (es decir, navegación, búsqueda, desprotección de recursos, etc.) con la extensión Adobe Asset Link resulta en solicitudes HTTP(S) a AEM Author que se validan con el token de inicio de sesión AEM, utilizando el controlador de autenticación de token estándar AEM.

>[!NOTE]
>
>Al expirar el token de inicio de sesión, **Pasos 1 a 5** invocará automáticamente, autenticará la extensión de Adobe Asset Link con el token del portador y volverá a emitir un token de inicio de sesión nuevo y válido.

## Recursos adicionales

+ [Sitio web de Adobe Asset Link](https://www.adobe.com/es/creativecloud/business/enterprise/adobe-asset-link.html)
