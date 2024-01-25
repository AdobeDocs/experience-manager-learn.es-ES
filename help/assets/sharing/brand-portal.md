---
title: Uso de Brand Portal
description: AEM Vídeos introductorios sobre la integración de Autor y AEM Assets Brand Portal de.
feature: Brand Portal
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-06-15T00:00:00Z
doc-type: Feature Video
exl-id: 42f13a19-52bf-413d-a141-63f1f0910dce
duration: 2546
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1703'
ht-degree: 0%

---

# Uso de Brand Portal con AEM Assets{#using-brand-portal-with-aem-assets}

Guías de vídeo sobre la integración de Adobe Experience Manager AEM () con Assets Brand Portal.

## Funciones y mejoras de Brand Portal de septiembre de 2019

En septiembre de 2019, Brand Portal presenta de forma más destacada el abastecimiento de recursos, que aumenta la velocidad del contenido y permite un intercambio fácil y rápido de recursos entre los autores Experience Manager y los creativos y colaboradores de terceros.

### Abastecimiento de recursos Brand Portal{#asset-sourcing}

El abastecimiento de recursos de Brand Portal se utiliza para recopilar recursos de agencias y equipos de terceros, sincronizándolos sin problemas con el autor del Experience Manager para su revisión y uso.

>[!VIDEO](https://video.tv.adobe.com/v/29365?quality=12&learn=on)

*Se requiere Experience Manager Author 6.5 SP2 (6.5.2) o superior para utilizar el Abastecimiento de recursos*

Revisar [Habilitar el autor del Experience Manager para el abastecimiento de recursos](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/brand-portal-asset-sourcing.html?lang=es) para obtener instrucciones sobre cómo configurar el abastecimiento de recursos en Experience Manager Author.

## Funciones y mejoras de Brand Portal de febrero de 2019{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354?quality=12&learn=on)

La versión de febrero de 2019 de Brand Portal se centra en las mejoras en la búsqueda de texto y en las solicitudes de los clientes principales.

### Mejoras de búsqueda

Brand Portal mejora la búsqueda con la búsqueda de texto parcial en el predicado de propiedad en el panel de filtrado. Para permitir la búsqueda de texto parcial, debe habilitar la búsqueda parcial en el predicado de propiedad en el formulario de búsqueda.

Siga leyendo para obtener más información sobre la búsqueda de texto parcial y la búsqueda con comodines.

#### Búsqueda parcial de frases

Ahora puede buscar recursos especificando solo una parte (es decir, una palabra o dos) de la frase buscada en el panel de filtrado.

**Caso de uso** : La búsqueda parcial de frases resulta útil cuando no está seguro de la combinación exacta de palabras que se producen en la frase buscada.

Por ejemplo, si el formulario de búsqueda en Brand Portal utiliza el predicado de propiedad para la búsqueda parcial del título de los recursos, al especificar el término camp, se devuelven todos los recursos con la palabra camp en su frase de título.

#### Búsqueda con comodines

El Brand Portal permite usar el asterisco (*) en las consultas de búsqueda junto con una parte de la palabra en la frase buscada.

**Caso de uso** : Si no está seguro de las palabras exactas que se producen en la frase buscada, puede utilizar una búsqueda con comodines para rellenar los huecos en la consulta de búsqueda.

Por ejemplo, si especifica climb*, se devolverán todos los recursos que tengan palabras que comiencen por los caracteres climb en la frase de título si el formulario de búsqueda en Brand Portal utiliza el predicado de propiedades para la búsqueda parcial en el título de los recursos.

Del mismo modo, se especifica:

* \*climb devuelve todos los recursos que tienen palabras que terminan con caracteres climb en la frase de título.
* \*climb\* devuelve todos los recursos que tienen palabras que comprenden los caracteres escalar en la frase de título.

#### Habilitar la jerarquía de carpetas

Los administradores ahora pueden configurar cómo se muestran las carpetas a los usuarios no administradores (editores, visualizadores y usuarios invitados) al iniciar sesión.
[Habilitar la jerarquía de carpetas](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) La configuración de se agrega en Configuración general, en el panel Herramientas de administración. Si la configuración es:

* Habilitado, el árbol de carpetas que comienza desde la carpeta raíz es visible para los usuarios que no son administradores. De este modo, se les concede una experiencia de navegación similar a la de los administradores.
* Desactivado, solo se muestran las carpetas compartidas en la página de aterrizaje.

[Habilitar la jerarquía de carpetas](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) (cuando está activada) le ayuda a diferenciar las carpetas con los mismos nombres compartidos desde diferentes jerarquías. Al iniciar sesión, los usuarios no administradores ahora ven las carpetas principales virtuales (y antecesoras) de las carpetas compartidas.

Las carpetas compartidas están organizadas dentro de los respectivos directorios en carpetas virtuales. Puede reconocer estas carpetas virtuales con un icono de candado.

Tenga en cuenta que la miniatura predeterminada de las carpetas virtuales es la imagen en miniatura de la primera carpeta compartida.

### Compatibilidad con representaciones de vídeo de Dynamic Media

AEM Los usuarios cuya instancia de autor de la esté en el modo híbrido de Dynamic Media pueden obtener una vista previa de las representaciones de medios dinámicos y descargarlas, además de los archivos de vídeo originales.

Para permitir la previsualización y la descarga de representaciones de medios dinámicos en cuentas de inquilino específicas, los administradores deben especificar la configuración de Dynamic Media (URL del servicio de vídeo (URL de puerta de enlace DM) y el ID de registro para recuperar el vídeo dinámico) en la configuración de vídeo del panel de herramientas de administración.

Puede obtener una vista previa de los vídeos de Dynamic Media en:

* Página de detalles del recurso
* Vista de tarjeta del recurso
* Página de vista previa de vínculos compartidos

Las codificaciones de vídeo de Dynamic Media se pueden descargar desde:

* Brand Portal
* Vínculo compartido

### Publicación programada en Brand Portal

Flujo de trabajo de publicación de recursos (y carpetas) desde [AEM (6.4.2.0)](https://helpx.adobe.com/experience-manager/6-5/release-notes/sp-release-notes.html#main-pars_header_9658011) Las instancias de autor en Brand Portal se pueden programar para una fecha y hora posteriores.

Del mismo modo, los recursos publicados se pueden eliminar del portal en una fecha (hora) posterior mediante la programación del flujo de trabajo Cancelar la publicación de Brand Portal.

### Alias de inquilino configurable en la URL

Las organizaciones pueden personalizar la dirección URL de su portal si tienen un prefijo alternativo en la dirección URL. Para obtener un alias para el nombre del inquilino en la URL de su portal existente, las organizaciones deben ponerse en contacto con el servicio de asistencia del Adobe.

Tenga en cuenta que solo se puede personalizar el prefijo de la dirección URL de Brand Portal y no la dirección URL completa.
Por ejemplo, una organización con un dominio existente `wknd.brand-portal.adobe.com` puede obtener `wkndinc.brand-portal.adobe.com` creado a petición.

AEM Sin embargo, la instancia de autor de puede ser [configurado](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html) solo con la URL del id de inquilino y no con la URL del alias de inquilino (alternativa).

**Caso de uso** : las organizaciones pueden satisfacer sus necesidades de promoción de la marca personalizando la dirección URL del portal, en lugar de atenerse a la dirección URL proporcionada por el Adobe.

## Funciones y mejoras de Brand Portal de diciembre de 2018{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707?quality=12&learn=on)

### Acceso de invitado

AEM El portal de marca permite a los invitados acceder al portal. Un usuario invitado no necesita credenciales para entrar al portal y puede acceder y descargar todas las carpetas y colecciones públicas. Los usuarios invitados pueden agregar recursos a su light-box (colección privada) y descargarlos. También pueden ver la búsqueda de etiquetas inteligentes y los predicados de búsqueda establecidos por los administradores. La sesión de invitado no permite a los usuarios crear colecciones y búsquedas guardadas, ni compartirlas más, acceder a la configuración de carpetas y colecciones y compartir recursos como vínculos.

### Descarga acelerada

Los usuarios de Brand Portal pueden aprovechar las descargas rápidas basadas en Aspera para obtener velocidades hasta 25 veces más rápidas y disfrutar de una experiencia de descarga perfecta, independientemente de su ubicación en todo el mundo. Para descargar los recursos más rápido desde Brand Portal o un vínculo compartido, los usuarios deben seleccionar la opción Habilitar la aceleración de descarga en el cuadro de diálogo de descarga, siempre que la aceleración de descarga esté habilitada en su organización.

* [Guía para acelerar las descargas desde Brand Portal](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Servidor de prueba de Aspera Connect](https://test-connect.asperasoft.com/)

### Informe de inicio de sesión del usuario

Se ha introducido un nuevo informe, para rastrear los inicios de sesión de los usuarios. El informe Inicios de sesión de usuarios puede ser fundamental para permitir a las organizaciones auditar y comprobar a los administradores delegados y a otros usuarios de Brand Portal.

Los registros de informes muestran los nombres, los ID de correo electrónico, las personas (administrador, visor, editor, invitado), los grupos, el último inicio de sesión, el estado de la actividad y el recuento de inicios de sesión de cada usuario.

### Acceso a las representaciones originales

Los administradores pueden restringir el acceso del usuario a los archivos de imagen originales (jpeg, tiff, png, bmp, gif, pjpeg, x-portable-anymap, x-portable-bitmap, x-portable-graymap, x-portable-pixmap, x-rgb, x-xbitmap, x-xpixmap, x-icon, image/photoshop, image/x-photoshop, psd, image/vnd.adobe.photoshop) y dar acceso a las representaciones de baja resolución que descargan desde Brand Portal o un vínculo compartido. Este acceso se puede controlar en el nivel de grupo de usuarios desde la pestaña Grupos de la página Funciones de usuario en el panel Herramientas de administración.

### Nuevas configuraciones

Se agregan seis nuevas configuraciones para que los administradores habiliten o deshabiliten las siguientes funcionalidades en inquilinos específicos:

* Permitir el acceso de invitados
* Permitir que los usuarios soliciten acceso a Brand Portal
* Permitir que los administradores eliminen recursos de Brand Portal
* Permitir la creación de colecciones públicas
* Permitir la creación de colecciones públicas inteligentes
* Permitir la aceleración de descarga

### Otras mejoras

* *Ruta de jerarquía de carpetas en vistas de tarjetas y listas* — permite a los usuarios conocer la ubicación de las carpetas almacenadas en una instancia de Brand Portal. Ayuda a los usuarios a diferenciar las carpetas que tienen el mismo nombre en una jerarquía de carpetas diferente.
* *Opción Información general* — proporciona a los usuarios no administradores metadatos sobre el recurso o la carpeta seleccionando el recurso o la carpeta y, a continuación, la opción descripción general en la barra de herramientas. Actualmente, muestra el título, la fecha de creación y la ruta

### Adobe I/O aloja la interfaz de usuario para configurar las integraciones de autenticación

Brand Portal utiliza el Adobe I/O [https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/) para crear la aplicación JWT, que permite configurar integraciones de autenticación para permitir la integración de AEM Assets con Brand Portal. Anteriormente, la interfaz de usuario para configurar integraciones de OAuth estaba alojada en `https://marketing.adobe.com/developer/`. Para obtener más información sobre la integración de AEM Assets con Brand Portal para publicar recursos y colecciones en Brand Portal, consulte [Configuración de la integración de AEM Assets con Brand Portal](https://helpx.adobe.com/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html).

## Funciones y mejoras de Brand Portal de febrero de 2018{#brand-portal-features-and-enhancements-632}

Las nuevas funciones mejoraron la funcionalidad orientada a alinear Brand Portal AEM con los recursos de la.

>[!VIDEO](https://video.tv.adobe.com/v/26354?quality=12&learn=on)

### Mejoras de navegación

* AEM Interfaz de usuario actualizada que se alinea con la interfaz de usuario de Coral3 y que utiliza la interfaz de usuario de Coral3.
* Acceso rápido y sencillo a las herramientas administrativas a través del nuevo logotipo de Adobe.
* Navegación de productos mediante una superposición
* Navegación rápida a carpetas principales desde una carpeta secundaria.
* Opción Omnisearch para desplazarse a las herramientas administrativas y al contenido.
* La vista de tarjeta, la vista de lista y la vista de columna le ayudan a examinar fácilmente las carpetas anidadas.
* La ordenación de recursos en la vista de lista ya no se restringe al número de recursos que se muestran en la pantalla. Se ordenan todos los recursos de una carpeta.

### Mejoras de búsqueda

* La funcionalidad Omnisearch permite realizar una búsqueda rápida de recursos y archivos en Brand Portal.
* Omnisearch también proporciona una opción para buscar recursos dentro de una carpeta o ubicación específica
* Sugerencias automáticas de palabras clave para facilitar la búsqueda
* Mejore su Omnisearch con filtros adicionales. Opción para guardar el resultado de búsqueda en una colección inteligente para que vuelva a visitar la búsqueda más adelante.
* Admite búsqueda de recursos con etiquetas inteligentes
* AEM AEM Los recursos etiquetados inteligentes se pueden compartir desde la interfaz de usuario de Brand Portal y usar etiquetas inteligentes para la búsqueda de recursos en Brand Portal.

### Mejoras de uso compartido de archivos

* El usuario puede compartir un recurso mediante la opción de uso compartido de vínculos.
* Al compartir recursos, el usuario puede establecer una fecha de caducidad para cada recurso. Proporciona a los usuarios más control sobre los recursos compartidos.
* Un usuario externo con un vínculo de recurso compartido puede descargar la imagen y ver sus propiedades.
* La jerarquía de carpetas anidada original se conserva para las carpetas de recursos descargadas.

### Capacidades administrativas y de creación de informes

* Ahora, el esquema de metadatos de AEM Assets AEM se puede publicar desde la publicación de metadatos a Brand Portal, desde la publicación en el sitio web de.
* Los administradores pueden crear y administrar tres tipos de informes: recursos descargados, caducados y publicados
* Capacidad para configurar la columna que debe incluirse en el informe.
* Cree ajustes preestablecidos de imagen para los recursos de Brand Portal.
* Capacidad para modificar el formulario de carril de búsqueda de administración o buscar Forms para incluir opciones de filtrado adicionales.
* Actualice y previsualice el fondo de pantalla personalizado para su marca
* Informe de uso para conocer el número de usuarios, el espacio de almacenamiento utilizado y el total de recursos.

## Recursos adicionales{#additional-resources}

* [Novedades de en Brand Portal](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/introduction/whats-new.html?lang=es#introduction)
* [AEM Agentes de replicación de autor de](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [Guía de descarga acelerada](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Documentos de Adobe de AEM Assets Brand Portal](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html)
* [Documentos de Adobe de AEM Assets Dynamic Media](https://experienceleague.adobe.com/docs/)
* [Descargar Aspera Connect](https://downloads.asperasoft.com/connect2/)
* [Servidor de prueba de Aspera Connect](https://test-connect.asperasoft.com/)
