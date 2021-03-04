---
title: Uso de la extensión de Adobe Asset Link con AEM Assets
description: 'Los diseñadores y los usuarios creativos ahora pueden utilizar los recursos de Adobe Experience Manager en sus aplicaciones de escritorio favoritas de Adobe Creative Cloud. La extensión Adobe Asset Link para Adobe Creative Cloud Enterprise amplía la capacidad para buscar, examinar, ordenar, previsualizar, cargar recursos, retirar, modificar, registrar y ver metadatos de recursos de AEM en herramientas de Creative Cloud como Adobe Photoshop, InDesign e Illustrator. '
feature: Adobe Asset Link
version: 6.4, 6.5
topic: Administración de contenido
role: Profesional empresarial
level: Principiante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1102'
ht-degree: 1%

---


# Uso de la extensión de Adobe Asset Link con AEM Assets{#using-adobe-asset-link-extension-with-aem-assets}

Los diseñadores y los usuarios creativos ahora pueden utilizar los recursos de Adobe Experience Manager en sus aplicaciones de escritorio favoritas de Adobe Creative Cloud. La extensión Adobe Asset Link para Adobe Creative Cloud Enterprise amplía la capacidad para buscar, examinar, ordenar, previsualizar, cargar recursos, retirar, modificar, registrar y ver metadatos de recursos de AEM en herramientas de Creative Cloud como Adobe Photoshop, InDesign e Illustrator.


## Adobe Asset Link 1.1

Adobe Asset Link v1.1 ahora proporciona compatibilidad de vinculación directa de InDesign entre Adobe Asset Link y AEM Assets. Con la compatibilidad con la vinculación directa de InDesign, ahora puede colocar (colocar elementos vinculados o colocar una copia) o arrastrar y soltar recursos digitales en InDesign desde AEM Assets a través del panel Adobe Asset Link. Además, introduce la representación *Solo para ubicación* (FPO).

>[!VIDEO](https://video.tv.adobe.com/v/28988/?quality=12&learn=on)

>[!NOTE]
>
>Use su Adobe Creative Cloud Enterprise ID o Federated ID únicamente. Asegúrese de [configurar AEM para Adobe Asset Link](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html).


### Funciones de Adobe Asset Link

* Adobe Asset Link es una extensión que funciona con PS, AI e ID y que proporciona acceso directo a recursos digitales que residen en AEM Assets
* Los creativos iniciarán sesión automáticamente en AEM con su Adobe IMS Enterprise ID o Federated ID
* Los creativos pueden examinar recursos digitales que residen en AEM Assets, además de buscar en AEM Assets y Creative Cloud Assets
* Los creativos pueden acceder a los detalles de los archivos para los recursos que residen en AEM Assets; miniaturas, metadatos básicos y versiones desde dentro del panel
* Los creativos pueden colocar, descargar o arrastrar y soltar recursos en su diseño
* Los creativos pueden modificar los recursos desprotegiéndolos de AEM Assets y trabajando en ellos (WIP) en su cuenta de Creative Cloud Assets
* Los creativos pueden volver a registrar un recurso en AEM Assets una vez que hayan terminado de modificarlo, y la nueva versión se reflejará en AEM Assets
* Admite las aplicaciones de escritorio de Creative Cloud 2020, 2019 y 2018 de InDesign, Photoshop e Illustrator
* Un usuario puede realizar una búsqueda de recursos desde el panel en la aplicación de Adobe Asset Link y ordenarlos en función del tamaño, alfabéticamente y por relevancia
* Los usuarios pueden acceder a las colecciones y colecciones inteligentes de AEM Assets y explorarlas directamente desde el panel de Asset Link
* Agregue los recursos recién creados a AEM Assets directamente desde el panel
* Un usuario puede arrastrar y soltar recursos directamente en marcos de InDesign

### Colocación de AEM Assets en InDesign

Puede colocar un recurso en el diseño de InDesign mediante una de las siguientes opciones:

* **Colocar copia** : al incrustar un recurso (mediante la opción Colocar copia), se coloca una copia del recurso original en el diseño de InDesign después de descargar los binarios en el sistema local. Adobe Asset Link no mantiene ningún vínculo entre la copia incrustada y el recurso original. Si el recurso original se modifica en AEM Assets, debe eliminar el recurso incrustado del archivo de InDesign y volver a incrustarlo desde AEM Assets.

* **Colocar elementos vinculados** : al trabajar con documentos de InDesign, ahora tiene la opción de hacer referencia a los recursos de AEM Assets, además de incrustar directamente los recursos (mediante la opción Colocar copia del menú contextual). Al hacer referencia a los recursos, puede colaborar con otros usuarios e incorporar cualquier actualización realizada al recurso original en AEM Assets. Para hacer referencia a un recurso desde AEM Assets, utilice la opción Colocar vinculado en el menú contextual.

### Resolución Solo para ubicación (FPO)

Cuando se colocan archivos de recursos grandes en documentos de InDesign de AEM Assets mediante Adobe Asset Link, los usuarios creativos deben esperar unos segundos después de iniciar la operación de colocación. Esto afecta a la experiencia general del usuario. Con Adobe Asset Link, ahora puede colocar temporalmente una imagen de baja resolución del recurso original desde AEM Assets, lo que reduce el tiempo necesario para colocar una imagen. Al mismo tiempo, aumenta la experiencia y la productividad del usuario en general. La imagen de menor resolución se coloca temporalmente y cuando se requiere la salida final para imprimir o publicar, se deben reemplazar las representaciones de FPO por las originales. Si desea reemplazar varias imágenes de FPO con sus respectivas imágenes originales, vaya al panel **_Windows > Links_** y, a continuación, descargue los recursos originales. Una vez descargadas las imágenes originales, elija Reemplazar todas las FPO con originales.

>[!NOTE]
>
> *La representación Solo para ubicación (FPO)*  solo funciona para la opción Colocar vinculado. También debe habilitar la compatibilidad con la representación de FPO dentro del flujo de trabajo de AEM Assets *Dam Update Asset*.

Las representaciones de FPO son sustitutos ligeros de los recursos originales. Tienen la misma relación de aspecto, pero tienen un tamaño menor que las imágenes originales. Actualmente, InDesign solo admite la importación de representaciones de FPO para los siguientes tipos de imagen:

* JPEG
* GIF
* PNG
* TIFF
* PSD
* BMP

Si una representación de FPO no está disponible para un recurso específico en AEM Assets, se hace referencia al recurso de alta resolución original en su lugar. Para las imágenes de FPO, el estado de FPO se muestra en el panel Vínculos de InDesign.

## Explicación de la autenticación de Adobe Asset Link con AEM Assets{#understanding-adobe-asset-link-authentication-with-aem-assets}

Cómo funciona la autenticación de Adobe Asset Link en el contexto de Adobe Identity Management Services (IMS) y Adobe Experience Manager Author.

![Arquitectura de Adobe Asset Link](assets/adobe-asset-link-article-understand.png)

Descargar [Arquitectura de Adobe Asset Link](assets/adobe-asset-link-article-understand-1.png)

1. La extensión Adobe Asset Link realiza una solicitud de autorización, a través de la aplicación de escritorio de Adobe Creative Cloud, al servicio de gestión de identidades (IMS) de Adobe y, cuando se realiza correctamente, recibe un token de portador.
2. La extensión de Adobe Asset Link se conecta a AEM Author a través de HTTP(S), incluido el token del portador obtenido en **Paso 1**, utilizando el esquema (HTTP/HTTPS), el host y el puerto proporcionados en la configuración JSON de la extensión.
3. El gestor de autenticación del portador de AEM extrae el token del portador de la solicitud y lo valida con Adobe IMS.
4. Una vez que Adobe IMS valida el token del portador, se crea un usuario en AEM (si aún no existe) y se sincronizan los datos de perfil y grupo/pertenencia de Adobe IMS. El usuario de AEM recibe un token de inicio de sesión estándar de AEM, que se devuelve a la extensión Adobe Asset Link como una cookie en la respuesta HTTP(S).
5. Interacciones posteriores (es decir, navegación, búsqueda, desprotección de recursos, etc.) con la extensión Adobe Asset Link, se generan solicitudes HTTP(S) a AEM Author que se validan mediante el token de inicio de sesión de AEM, utilizando el controlador de autenticación de tokens de AEM estándar.

>[!NOTE]
>
>Al expirar el token de inicio de sesión, los **pasos 1-5** se invocarán automáticamente, autenticarán la extensión de Adobe Asset Link con el token del portador y volverán a emitir un token de inicio de sesión nuevo y válido.

## Recursos adicionales{#additional-resources}

* [Sitio web de Adobe Asset Link](https://www.adobe.com/creativecloud/business/enterprise/adobe-asset-link.html)