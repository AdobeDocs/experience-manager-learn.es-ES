---
title: Uso de Brand Portal
seo-title: Uso de Brand Portal con AEM Assets
description: Tutoriales en vídeo de la integración de AEM Author y AEM Assets Brand Portal.
seo-description: Tutoriales en vídeo de la integración de AEM Author y AEM Assets Brand Portal.
feature: brand-portal
topics: authoring, sharing, collaboration, search, integrations, publishing, metadata, images, renditions, administration
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
team: tm
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '1788'
ht-degree: 1%

---


# Uso de Brand Portal con AEM Assets{#using-brand-portal-with-aem-assets}

Guías de vídeo de la integración de Adobe Experience Manager (AEM) Assets Brand Portal.

## Funciones y mejoras de Brand Portal septiembre de 2019

En septiembre de 2019, Brand Portal presenta principalmente el sistema de fuentes de recursos, que aumenta la velocidad de contenido y permite un intercambio rápido y sencillo de recursos entre autores Experience Manager y creativos y colaboradores de terceros.

### Abastecimiento de recursos de Brand Portal{#asset-sourcing}

La solución Asset Sourcing de Brand Portal se utiliza para recopilar recursos de agencias y equipos de terceros, sincronizándolos sin problemas con el autor Experience Manager para su revisión y uso.

>[!VIDEO](https://video.tv.adobe.com/v/29365/?quality=12&learn=on)

*Se requiere el Experience Manager Author 6.5 SP2 (6.5.2) o bueno para utilizar Asset Sourcing*

Consulte [Habilitar Experience Manager Author para fuentes](https://docs.adobe.com/content/help/en/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/configure-asset-sourcing-in-aem/brand-portal-enable-asset-sourcing.html) de recursos para obtener instrucciones sobre cómo configurar y configurar la fuente de recursos en Experience Manager Author.

## Funciones y mejoras de Brand Portal en febrero de 2019{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

La versión de febrero de 2019 de Brand Portal se centra en las mejoras en la búsqueda de texto y en las principales solicitudes de los clientes.

### Mejoras de búsqueda

Brand Portal mejora la búsqueda con texto parcial en el predicado de propiedades en el panel de filtrado. Para permitir la búsqueda de texto parcial, debe habilitar la búsqueda parcial en Predicado de propiedades en el formulario de búsqueda.

Continúe leyendo para obtener más información sobre la búsqueda de texto parcial y la búsqueda con comodines.

#### Búsqueda de frase parcial

Ahora puede buscar recursos especificando solo una parte (es decir, una palabra o dos) de la frase buscada en el panel de filtrado.

**Caso** de uso: La búsqueda parcial de frases resulta útil cuando no está seguro de la combinación exacta de palabras que se producen en la frase buscada.

Por ejemplo, si el formulario de búsqueda en Brand Portal utiliza Property Predicate para la búsqueda parcial del título de los recursos, al especificar el término camp se devuelven todos los recursos con la palabra camp en su frase de título.

#### Búsqueda de comodines

El portal de marcas permite utilizar el asterisco (*) en la consulta de búsqueda junto con una parte de la palabra de la frase buscada.

**Caso** de uso: si no está seguro de las palabras exactas que se producen en la frase buscada, puede utilizar una búsqueda comodín para llenar los huecos en la consulta de búsqueda.

Por ejemplo, si se especifica escalar*, se devuelven todos los recursos con palabras que comienzan con los caracteres escalados en la frase de título si el formulario de búsqueda en Brand Portal utiliza Predicado de propiedades para la búsqueda parcial del título de los recursos.

Del mismo modo, especificando:

* \*escalar devuelve todos los recursos que tienen palabras que finalizan con caracteres que suben en la frase de título.
* \*escalar\* devuelve todos los recursos que tienen palabras que comprenden los caracteres que suben en la frase de título.

#### Habilitar la jerarquía de carpetas

Los administradores ahora pueden configurar cómo se muestran las carpetas a los usuarios no administradores (editores, visores y usuarios invitados) al iniciar sesión.
[Habilitar la configuración de jerarquía](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) de carpetas se agrega en Configuración general, en el panel Herramientas de administración. Si la configuración es:

* Habilitado, el árbol de carpetas que empieza desde la carpeta raíz está visible para los usuarios no administradores. Por lo tanto, concederles una experiencia de navegación similar a la de los administradores.
* Deshabilitado, solo se muestran las carpetas compartidas en la página de aterrizaje.

[Activar la funcionalidad Jerarquía](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) de carpetas (cuando está activada) ayuda a diferenciar las carpetas con los mismos nombres compartidos de jerarquías diferentes. Al iniciar sesión, los usuarios que no son administradores ahora ven las carpetas principales virtuales (y antecesoras) de las carpetas compartidas.

Las carpetas compartidas están organizadas dentro de los directorios respectivos en carpetas virtuales. Puede reconocer estas carpetas virtuales con un icono de candado.

Tenga en cuenta que la miniatura predeterminada de las carpetas virtuales es la imagen en miniatura de la primera carpeta compartida.

### Compatibilidad con representaciones de vídeo de Dynamic Media

Los usuarios cuya instancia de AEM Author se encuentra en el modo híbrido Dynamic Media pueden realizar previsualizaciones y descargar las representaciones de medios dinámicos, además de los archivos de vídeo originales.

Para permitir la previsualización y descarga de representaciones de medios dinámicos en cuentas de inquilino específicas, los administradores deben especificar la configuración de Dynamic Media (URL del servicio de vídeo (URL de DM-Gateway) y el ID de registro para recuperar el vídeo dinámico) en la configuración de vídeo desde el panel Herramientas de administración.

Los vídeos de Dynamic Media se pueden previsualizar en:

* Página de detalles del recurso
* Vista de tarjetas del recurso
* Página de previsualización de uso compartido de vínculos

Los códigos de vídeo de Dynamic Media se pueden descargar de:

* Brand Portal
* Vínculo compartido

### Publicación programada en Brand Portal

Los recursos (y las carpetas) publican el flujo de trabajo desde [AEM (6.4.2.0)](https://helpx.adobe.com/experience-manager/6-5/release-notes/sp-release-notes.html#main-pars_header_9658011) La instancia de creación en Brand Portal se puede programar para una fecha y hora posteriores.

Del mismo modo, los recursos publicados se pueden eliminar del portal en una fecha (hora) posterior, programando el flujo de trabajo Cancelar publicación desde Brand Portal.

### Alias de inquilino configurable en URL

Las organizaciones pueden personalizar la dirección URL del portal si tienen un prefijo alternativo en la dirección URL. Para obtener un alias para el nombre del inquilino en la dirección URL del portal existente, las organizaciones deben ponerse en contacto con la asistencia de Adobe.

Tenga en cuenta que solo se puede personalizar el prefijo de la dirección URL de Brand Portal y no toda la dirección URL.
Por ejemplo, una organización con un dominio existente `wknd.brand-portal.adobe.com` puede `wkndinc.brand-portal.adobe.com` crearse a petición.

Sin embargo, la instancia de AEM Author solo se puede [configurar](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html) con la dirección URL de identificación del inquilino y no con la URL de alias del inquilino (alternativa).

**Caso** de uso: Las organizaciones pueden satisfacer sus necesidades de marca personalizando la dirección URL del portal, en lugar de atenerse a la dirección URL proporcionada por Adobe.

## Funciones y mejoras de Brand Portal en diciembre de 2018{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707/?quality=9&learn=on)

### Acceso de invitados

AEM portal de marca permite a los invitados acceder al portal. Un usuario invitado no necesita credenciales para entrar en el portal y puede acceder y descargar todas las carpetas y colecciones públicas. Los usuarios invitados pueden añadir recursos a su equipo de edición ligera (colección privada) y descargarlos. También pueden vista los predicados de búsqueda y búsqueda de etiquetas inteligentes establecidos por los administradores. La sesión de invitado no permite a los usuarios crear colecciones y búsquedas guardadas ni compartirlas más, acceder a la configuración de carpetas y colecciones y compartir recursos como vínculos.

### Descarga acelerada

Los usuarios de Brand Portal pueden aprovechar las rápidas descargas basadas en Aspera para obtener velocidades hasta 25 veces más rápidas y disfrutar de una experiencia de descarga sin problemas, independientemente de su ubicación en todo el mundo. Para descargar los recursos más rápidamente desde Brand Portal o un vínculo compartido, los usuarios deben seleccionar la opción Activar aceleración de descarga en el cuadro de diálogo de descarga, siempre que la aceleración de la descarga esté activada en su organización.

* [Guía para acelerar las descargas desde Brand Portal](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Aspera Connect Test Server](https://test-connect.asperasoft.com/)

### Informe de inicio de sesión del usuario

Se ha introducido un nuevo informe para rastrear los inicios de sesión de los usuarios. El informe de inicios de sesión de usuario puede ser fundamental para permitir que las organizaciones auditen y verifiquen a los administradores delegados y a otros usuarios de Brand Portal.

Los registros de informes muestran los nombres, los ID de correo electrónico, las personas (administrador, visor, editor, invitado), los grupos, el último inicio de sesión, el estado de la actividad y el recuento de inicio de sesión de cada usuario.

### Acceso a las representaciones originales

Los administradores pueden restringir el acceso de los usuarios a los archivos de imagen originales (jpeg, tiff, png, bmp, gif, pjpeg, x-portable-anymap, x-portable-bitmap, x-portable-graymap, x-portable-pixmap, x-rgb, x-xbitmap, x-xmap, x-icon, image/photoshop, image/x-photoshop, psd, image/vnd.adobe.photoshop ) y dar acceso a representaciones de baja resolución que descargan desde Brand Portal o vínculo compartido. Este acceso se puede controlar a nivel de grupo de usuarios desde la ficha Grupos de la página Funciones de usuario del panel Herramientas de administración.

### Nuevas configuraciones

Se añaden seis nuevas configuraciones para que los administradores habiliten o deshabiliten las siguientes funcionalidades en inquilinos específicos:

* Permitir el acceso de invitados
* Permitir que los usuarios soliciten acceso a Brand Portal
* Permitir que los administradores eliminen recursos de Brand Portal
* Permitir la creación de colecciones públicas
* Permitir la creación de colecciones inteligentes públicas
* Permitir aceleración de descarga

### Otras mejoras

* *Ruta de jerarquía de carpetas en vistas* de tarjeta y lista — permite a los usuarios conocer la ubicación de las carpetas almacenadas en una instancia de Brand Portal. Ayuda a los usuarios a diferenciar carpetas con el mismo nombre dentro de una jerarquía de carpetas diferente.
* *Opción* Información general — proporciona metadatos sobre el recurso o la carpeta a los usuarios que no son administradores. Para ello, seleccione el recurso o la carpeta y, a continuación, seleccione la opción Información general en la barra de herramientas. Actualmente, muestra el título, la fecha de creación y la ruta

### Interfaz de usuario de los hosts de E/S de Adobe para configurar las integraciones de autenticación

Brand Portal utiliza la interfaz [https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/) de E/S de Adobe para crear una aplicación JWT, que permite configurar integraciones de autenticación para permitir la integración de AEM Assets con Brand Portal. Anteriormente, la interfaz de usuario para configurar integraciones de OAuth estaba alojada en `https://marketing.adobe.com/developer/`. Para obtener más información sobre la integración de AEM Assets con Brand Portal para la publicación de recursos y colecciones en Brand Portal, consulte [Configurar la integración de AEM Assets con Brand Portal](https://helpx.adobe.com/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html).

## Funciones y mejoras de Brand Portal en febrero de 2018{#brand-portal-features-and-enhancements-632}

Las nuevas funciones mejoraron la funcionalidad orientada a alinear Brand Portal con AEM.

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

### Mejoras en la navegación

* Se ha actualizado la interfaz de usuario que se alinea con el AEM y utiliza la interfaz de usuario de Coral3.
* Acceso rápido y sencillo a las herramientas administrativas a través del nuevo logotipo de Adobe.
* Navegación del producto a través de una superposición
* Navegación rápida a carpetas principales desde una carpeta secundaria.
* Opción de Omniture para navegar a las herramientas administrativas y al contenido.
* La Vista de tarjetas, la vista de Listas y la Vista de columnas le ayudan a explorar fácilmente las carpetas anidadas.
* La ordenación de recursos en la Vista de Listas ya no está restringida al número de recursos que se muestran en la pantalla. Se ordenan todos los recursos de una carpeta.

### Mejoras en la búsqueda

* La capacidad de Omniture Search le permite realizar una búsqueda rápida de recursos y archivos dentro de Brand Portal.
* Omniture también proporciona una opción para buscar recursos dentro de una carpeta o ubicación específica
* Sugerencias de palabras clave automáticas para facilitar la búsqueda
* Mejore Omniture con filtros adicionales. Opción de guardar el resultado de la búsqueda en una colección inteligente para que pueda volver a visitar la búsqueda más adelante.
* Admite la búsqueda de recursos con etiquetas inteligentes
* AEM los recursos con etiquetas inteligentes se pueden compartir de AEM a Brand Portal y utilizar etiquetas inteligentes para buscar recursos en Brand Portal.

### Mejoras en el uso compartido de archivos

* El usuario puede compartir un recurso mediante la opción de compartir vínculos.
* Al compartir recursos, el usuario puede establecer una fecha de caducidad para cada recurso. Proporciona a los usuarios más control sobre los recursos compartidos.
* Un usuario externo con un vínculo de uso compartido de recursos puede descargar la imagen y vista sus propiedades.
* La jerarquía de carpetas anidadas original se conserva para las carpetas de recursos descargadas.

### Sistema de informes y capacidades administrativas

* El esquema de metadatos de AEM Assets ahora se puede publicar de AEM a Brand Portal.
* Los administradores pueden crear y administrar tres tipos de informes: recursos descargados, caducados y publicados
* Capacidad para configurar la columna que debe incluirse en el informe.
* Cree ajustes preestablecidos de imagen para los recursos dentro de Brand Portal.
* Posibilidad de modificar el formulario de carril de búsqueda de administración o Buscar en Forms para incluir opciones de filtrado adicionales.
* Actualización y previsualización del fondo de pantalla personalizado para su marca
* Informe de uso para conocer el número de usuarios, el espacio de almacenamiento utilizado y el total de recursos.

## Recursos adicionales{#additional-resources}

* [Novedades de Brand Portal](https://helpx.adobe.com/experience-manager/brand-portal/using/whats-new.html)
* [Agentes de replicación de AEM Author](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [Guía para la descarga acelerada](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Documentos de Adobe de AEM Assets Brand Portal](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html)
* [Documentos de AEM Assets Dynamic Media Adobe](https://docs.adobe.com/docs/es/aem/6-3/author/assets/dynamic-media.html)
* [Descargar Aspera Connect](https://downloads.asperasoft.com/connect2/)
* [Aspera Connect Test Server](https://test-connect.asperasoft.com/)