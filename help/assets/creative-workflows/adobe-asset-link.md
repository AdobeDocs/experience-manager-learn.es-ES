---
title: Uso de Adobe Asset Link Extension con AEM Assets
description: 'Los diseñadores y los usuarios creativos ahora pueden utilizar los recursos de Adobe Experience Manager en sus aplicaciones de escritorio favoritas de Adobe Creative Cloud. La extensión Vínculo de recursos de Adobe para Adobe Creative Cloud Enterprise amplía la capacidad de buscar y examinar, ordenar, previsualización, cargar recursos, extraer, modificar, registrar y vista metadatos de recursos de AEM en herramientas de Creative Cloud como Adobe Photoshop, InDesign y Illustrator. '
feature: adobe-asset-link
topics: authoring, collaboration, operations, sharing, metadata, images
audience: all
doc-type: feature video
activity: use
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '1092'
ht-degree: 1%

---


# Uso de Adobe Asset Link Extension con AEM Assets{#using-adobe-asset-link-extension-with-aem-assets}

Los diseñadores y los usuarios creativos ahora pueden utilizar los recursos de Adobe Experience Manager en sus aplicaciones de escritorio favoritas de Adobe Creative Cloud. La extensión Vínculo de recursos de Adobe para Adobe Creative Cloud Enterprise amplía la capacidad de buscar y examinar, ordenar, previsualización, cargar recursos, extraer, modificar, registrar y vista metadatos de recursos de AEM en herramientas de Creative Cloud como Adobe Photoshop, InDesign y Illustrator.


## Vínculo de recurso de Adobe 1.1

Adobe Asset Link v1.1 ahora ofrece compatibilidad con la vinculación directa de InDesigns entre Adobe Asset Link y AEM Assets. Con la compatibilidad con la vinculación directa de InDesign, ahora puede colocar (Colocar elementos vinculados o Colocar copia) o arrastrar y soltar recursos digitales en InDesign desde AEM Assets mediante el panel Vínculo de recursos de Adobe. Además, introduce la representación *Solo para ubicación* (FPO).

>[!VIDEO](https://video.tv.adobe.com/v/28988/?quality=12&learn=on)

>[!NOTE]
>
>Utilice únicamente a su Enterprise ID o Federated ID de Adobe Creative Cloud. Asegúrese de [configurar AEM para el vínculo de recursos de Adobe](https://helpx.adobe.com/enterprise/using/configure-aem-for-aal-prerelease.html).


### Capacidades de vínculo de recursos de Adobe

* Adobe Asset Link es una extensión que funciona con PS, AI ID y proporciona acceso directo a los recursos digitales que residen en AEM Assets
* Los elementos creativos iniciarán sesión AEM automáticamente con su Enterprise ID o Federated ID de Adobe IMS
* Los creativos pueden examinar recursos digitales que residen en AEM Assets, además de buscar en AEM Assets y Creative Cloud Assets
* Los elementos creativos pueden acceder a los detalles de los archivos de los recursos que residen en AEM Assets; miniaturas, metadatos básicos y versiones desde el panel
* Los elementos creativos pueden colocar, descargar o arrastrar y soltar recursos en su diseño
* Los elementos creativos pueden modificar los recursos desprotegiéndolos de AEM Assets y trabajando en ellos (WIP) dentro de su cuenta de activos Creative Cloud
* Los creativos pueden volver a registrar un recurso en AEM Assets después de que hayan terminado de modificarlo y la nueva versión se verá reflejada en AEM Assets
* Admite aplicaciones de InDesign, Photoshop y Illustrator de Creative Cloud 2020, 2019 y 2018
* Un usuario puede realizar una búsqueda de recursos desde el panel en la aplicación Vínculo de recursos de Adobe y ordenarlos en función del tamaño, alfabéticamente y por relevancia
* Los usuarios pueden acceder a las colecciones y colecciones inteligentes de AEM Assets y explorarlas directamente desde el panel Vínculo de recursos
* Añadir recursos recién creados a AEM Assets directamente desde el panel
* Un usuario puede arrastrar y soltar recursos directamente en marcos de InDesign

### Colocación de AEM Assets en InDesign

Puede colocar un recurso en el diseño de InDesign mediante una de las siguientes opciones:

* **Colocar copia** : al incrustar un recurso (mediante la opción Colocar copia) se coloca una copia del recurso original en el diseño de InDesign después de descargar los binarios en el sistema local. Adobe Asset Link no mantiene ningún vínculo entre la copia incrustada y el recurso original. Si el recurso original se modifica en AEM Assets, debe eliminarlo del archivo InDesign y volver a incrustarlo de AEM Assets.

* **Colocar vinculados** : al trabajar con documentos de InDesign, ahora tiene la opción de hacer referencia a los recursos de AEM Assets además de incrustar directamente los recursos (mediante la opción Colocar copia en el menú contextual). Al hacer referencia a recursos, puede colaborar con otros usuarios e incorporar cualquier actualización realizada al recurso original en AEM Assets. Para hacer referencia a un recurso de AEM Assets, utilice la opción Colocar vinculado en el menú contextual.

### Resolución de sólo ubicación (FPO)

Cuando los archivos de recursos grandes se colocan en Documentos de InDesign desde AEM Assets mediante el vínculo de recursos de Adobe, los usuarios de elementos creativos deben esperar unos segundos después de iniciar la operación de colocación. Esto afecta a la experiencia general del usuario. Con Adobe Asset Link ahora puede colocar temporalmente una imagen de baja resolución del recurso original desde AEM Assets, reduciendo así el tiempo necesario para colocar una imagen. Al mismo tiempo, aumenta la productividad y la experiencia del usuario en general. La imagen de menor resolución se coloca temporalmente y cuando se necesita el resultado final para imprimir o publicar, es necesario reemplazar las representaciones de FPO por los originales. Si desea reemplazar varias imágenes de FPO con sus respectivas imágenes originales, navegue al panel **_Ventanas > Vínculos_** y descargue los recursos originales. Una vez descargadas las imágenes originales, elija Reemplazar todas las FPO con los originales.

>[!NOTE]
>
> *Para la* representación solo de colocación (FPO) solo funciona con la opción Colocar vinculado. También debe habilitar la compatibilidad con la representación de FPO en el flujo de trabajo de AEM Assets *Dam Update Asset*.

Las representaciones de FPO son sustitutos ligeros de los recursos originales. Tienen la misma proporción de aspecto, pero son de menor tamaño en comparación con las imágenes originales. Actualmente, InDesign solo admite la importación de representaciones FPO para los siguientes tipos de imágenes:

* JPEG
* GIF
* PNG
* TIFF
* PSD
* BMP

Si una representación FPO no está disponible para un recurso específico en AEM Assets, se hace referencia al recurso original de alta resolución. Para las imágenes de FPO, el estado de FPO se muestra en el panel Vínculos de InDesign.



## Información sobre la autenticación de Adobe Asset Link con AEM Assets{#understanding-adobe-asset-link-authentication-with-aem-assets}

Funcionamiento de la autenticación de Adobe Asset Link en el contexto de Adobe Identity Management Services (IMS) y Adobe Experience Manager Author.

![Arquitectura de vínculos de recursos de Adobe](assets/adobe-asset-link-article-understand.png)

Descargar [Arquitectura de vínculos de recursos de Adobe](assets/adobe-asset-link-article-understand-1.png)

1. La extensión Vínculo de recursos de Adobe realiza una solicitud de autorización, a través de la aplicación Adobe Creative Cloud Desktop, al servicio de administración de identidades de Adobe (IMS) y, una vez finalizado el proceso, recibe un token de portador.
2. La extensión Vínculo de recursos de Adobe se conecta a AEM Author a través de HTTP(S), incluido el token del portador obtenido en **Paso 1**, mediante el esquema (HTTP/HTTPS), el host y el puerto proporcionados en la configuración JSON de la extensión.
3. El controlador de autenticación del portador de AEM extrae el distintivo del portador de la solicitud y lo valida con Adobe IMS.
4. Una vez que Adobe IMS valida el token de portador, se crea un usuario en AEM (si no existe) y se sincronizan los datos de perfil y grupo/pertenencia desde Adobe IMS. El usuario AEM recibe un token de inicio de sesión AEM estándar, que se devuelve a la extensión Vínculo de recursos de Adobe como una cookie en la respuesta HTTP(S).
5. Interacciones posteriores (por ejemplo: exploración, búsqueda, desprotección de recursos, etc.) con la extensión Vínculo de recurso de Adobe, se generan solicitudes HTTP a AEM Author que se validan con el autentificador de inicio de sesión de AEM, mediante el controlador de autentificación de autentificador AEM estándar.

>[!NOTE]
>
>Al caducar el token de inicio de sesión, **Los pasos 1-5** se invocarán automáticamente, autenticando la extensión de vínculo de recursos de Adobe mediante el token del portador y vuelven a emitir un nuevo token de inicio de sesión válido.

## Recursos adicionales{#additional-resources}

* [Sitio web de Vínculo de recursos de Adobe](https://www.adobe.com/creativecloud/business/enterprise/adobe-asset-link.html)