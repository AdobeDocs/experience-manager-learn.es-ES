---
title: Uso de Brand Portal
description: Tutoriales en vídeo de la integración de AEM Author y AEM Assets Brand Portal.
feature: Brand Portal
version: 6.3, 6.4, 6.5
topic: Administración de contenido
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '1774'
ht-degree: 2%

---


# Uso de Brand Portal con AEM Assets{#using-brand-portal-with-aem-assets}

Guías de vídeo de la integración de Adobe Experience Manager (AEM) Assets Brand Portal.

## Mejoras y funciones de septiembre de 2019 de Brand Portal

Brand Portal, que se  en septiembre de 2019, presenta Asset Sourcing, que aumenta la velocidad de contenido y permite un intercambio rápido y sencillo de recursos entre autores Experience Manager y creativos y colaboradores de terceros.

### Abastecimiento de recursos de Brand Portal{#asset-sourcing}

La fuente de recursos de Brand Portal se utiliza para recopilar recursos de agencias y equipos de terceros, sincronizándolos sin problemas con el Autor Experience Manager para su revisión y uso.

>[!VIDEO](https://video.tv.adobe.com/v/29365/?quality=12&learn=on)

*Se requiere el Experience Manager Autor 6.5 SP2 (6.5.2) o bueno para utilizar Asset Sourcing*

Consulte [Habilitar Autor de Experience Manager para el abastecimiento de recursos](https://docs.adobe.com/content/help/en/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/configure-asset-sourcing-in-aem/brand-portal-enable-asset-sourcing.html) para obtener instrucciones sobre cómo configurar el abastecimiento de recursos en Autor de Experience Manager.

## Mejoras y funciones de Brand Portal de febrero de 2019{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

La versión de febrero de 2019 de Brand Portal se centra en las mejoras en la búsqueda de texto y en las principales solicitudes de los clientes.

### Mejoras de búsqueda

Brand Portal mejora la búsqueda con búsqueda de texto parcial en el predicado de propiedades del panel de filtrado. Para permitir la búsqueda de texto parcial, debe habilitar la búsqueda parcial en el predicado de propiedades en el formulario de búsqueda.

Continúe leyendo para obtener más información sobre la búsqueda de texto parcial y la búsqueda de comodines.

#### Búsqueda parcial de frases

Ahora puede buscar recursos especificando solo una parte (es decir, una palabra o dos) de la frase buscada en el panel de filtrado.

**Caso**  de uso: La búsqueda parcial de frases resulta útil cuando no está seguro de la combinación exacta de palabras que se producen en la frase buscada.

Por ejemplo, si el formulario de búsqueda en Brand Portal utiliza Predicado de propiedades para la búsqueda parcial del título de los recursos, al especificar el término &quot;campo&quot; se devuelven todos los activos con la palabra &quot;campo&quot; en la frase del título.

#### Búsqueda comodín

Brand Portal permite utilizar el asterisco (*) en la consulta de búsqueda junto con una parte de la palabra de la frase buscada.

**Caso**  de uso: si no está seguro de las palabras exactas que se producen en la frase buscada, puede utilizar una búsqueda comodín para rellenar los huecos en la consulta de búsqueda.

Por ejemplo, si especifica escalar*, se devuelven todos los recursos que tengan palabras que empiecen por los caracteres escalar en la frase de título si el formulario de búsqueda en Brand Portal utiliza Predicado de propiedades para la búsqueda parcial del título de los recursos.

Del mismo modo, especificando:

* \*escalar devuelve todos los recursos que tienen palabras que terminan con caracteres en su frase de título.
* \*escalar\* devuelve todos los recursos que tienen palabras que componen los caracteres que suben en la frase de título.

#### Habilitar la jerarquía de carpetas

Los administradores ahora pueden configurar cómo se muestran las carpetas a los usuarios no administradores (editores, visualizadores y usuarios invitados) al iniciar sesión.
[La opción Habilitar ](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) jerarquía de carpetas se agrega en Configuración general, en el panel Herramientas de administración. Si la configuración es:

* Habilitado, el árbol de carpetas que comienza desde la carpeta raíz es visible para los usuarios que no son administradores. Por lo tanto, concederles una experiencia de navegación similar a la de los administradores.
* Deshabilitado, solo las carpetas compartidas se muestran en la página de aterrizaje.

[Habilitar la funcionalidad ](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) Jerarquía de carpetas (cuando está habilitada) ayuda a diferenciar las carpetas con los mismos nombres compartidos de jerarquías diferentes. Al iniciar sesión, los usuarios no administradores ven ahora las carpetas principales virtuales (y antecesoras) de las carpetas compartidas.

Las carpetas compartidas están organizadas dentro de los directorios respectivos en carpetas virtuales. Puede reconocer estas carpetas virtuales con un icono de bloqueo.

Tenga en cuenta que la miniatura predeterminada de las carpetas virtuales es la imagen en miniatura de la primera carpeta compartida.

### Compatibilidad con representaciones de vídeo de Dynamic Media

Los usuarios cuya instancia de AEM Author esté en modo híbrido Dynamic Media pueden obtener una vista previa y descargar las representaciones de Dynamic Media, además de los archivos de vídeo originales.

Para permitir la previsualización y descarga de representaciones de Dynamic Media en cuentas de inquilino específicas, los administradores deben especificar la configuración de Dynamic Media (URL del servicio de vídeo (URL de DM-Gateway) y el ID de registro para recuperar el vídeo dinámico) en la configuración de vídeo desde el panel de herramientas de administración.

Los vídeos de Dynamic Media se pueden previsualizar en:

* Página de detalles del recurso
* Vista de tarjeta del recurso
* Vincular página de vista previa de uso compartido

Las codificaciones de vídeo de Dynamic Media se pueden descargar desde:

* Brand Portal
* Vínculo compartido

### Publicación programada en Brand Portal

El flujo de trabajo de publicación de recursos (y carpetas) de [AEM (6.4.2.0)](https://helpx.adobe.com/experience-manager/6-5/release-notes/sp-release-notes.html#main-pars_header_9658011) La instancia de autor en Brand Portal se puede programar para una fecha y hora posteriores.

Del mismo modo, los recursos publicados se pueden eliminar del portal en una fecha (hora) posterior, programando el flujo de trabajo Cancelar publicación desde Brand Portal .

### Alias de inquilino configurable en la dirección URL

Las organizaciones pueden personalizar la dirección URL del portal si tienen un prefijo alternativo en la dirección URL. Para obtener un alias para el nombre de inquilino en la URL de su portal existente, las organizaciones deben ponerse en contacto con el servicio de asistencia técnica de Adobe.

Tenga en cuenta que solo se puede personalizar el prefijo de la URL de Brand Portal y no la URL completa.
Por ejemplo, una organización con un dominio existente `wknd.brand-portal.adobe.com` puede obtener `wkndinc.brand-portal.adobe.com` creado si se solicita.

Sin embargo, la instancia de Autor de AEM solo puede [configurarse](https://helpx.adobe.com/es/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html) con la dirección URL de identificación del inquilino y no con la dirección URL de alias del inquilino (alternativa).

**Caso**  de uso: Las organizaciones pueden satisfacer sus necesidades de promoción de la marca personalizando la URL del portal, en lugar de atenerse a la URL proporcionada por Adobe.

## Mejoras y funciones de diciembre de 2018 de Brand Portal{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707/?quality=9&learn=on)

### Acceso de invitado

AEM Brand Portal permite a los invitados acceder al portal. Un usuario invitado no necesita credenciales para entrar al portal y puede acceder y descargar todas las carpetas y colecciones públicas. Los usuarios invitados pueden agregar recursos a su buzón de luz (colección privada) y descargar los mismos. También pueden ver los predicados de búsqueda y búsqueda de etiquetas inteligentes establecidos por los administradores. La sesión de invitado no permite a los usuarios crear colecciones y guardar búsquedas ni compartirlas más, acceder a la configuración de carpetas y colecciones y compartir recursos como vínculos.

### Descarga acelerada

Los usuarios de Brand Portal pueden aprovechar las rápidas descargas basadas en Aspera para obtener velocidades hasta 25 veces más rápidas y disfrutar de una experiencia de descarga sin problemas independientemente de su ubicación en todo el mundo. Para descargar los recursos más rápido desde Brand Portal o un vínculo compartido, los usuarios deben seleccionar la opción Habilitar aceleración de descarga en el cuadro de diálogo de descarga, siempre que la aceleración de la descarga esté habilitada en su organización.

* [Guía para acelerar las descargas desde Brand Portal](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Servidor de prueba de Aspera Connect](https://test-connect.asperasoft.com/)

### Informe de inicio de sesión del usuario

Se ha introducido un nuevo informe para realizar un seguimiento de los inicios de sesión de los usuarios. El informe de inicios de sesión de usuario puede ser fundamental para que las organizaciones puedan auditar y verificar los administradores delegados y otros usuarios de Brand Portal.

Los registros de informes muestran los nombres, los ID de correo electrónico, las personas (administrador, visor, editor, invitado), los grupos, el último inicio de sesión, el estado de la actividad y el recuento de inicio de sesión de cada usuario.

### Acceso a representaciones originales

Los administradores pueden restringir el acceso de los usuarios a los archivos de imagen originales (jpeg, tiff, png, bmp, gif, pjpeg, x-portable-anymap, x-portable-bitmap, x-portable-graymap, x-portable-pixmap, x-rgb, x-xbitmap, x-xmap, x-icon, image/photoshop, image/x-photoshop, psd, image/vnd.adobe.photoshop ) y proporcionan acceso a representaciones de baja resolución que descargan de Brand Portal o de un vínculo compartido. Este acceso se puede controlar a nivel de grupo de usuarios desde la ficha Grupos de la página Funciones de usuario del panel Herramientas de administración.

### Nuevas configuraciones

Se añaden seis nuevas configuraciones para que los administradores habiliten o deshabiliten las siguientes funcionalidades en inquilinos específicos:

* Permitir el acceso de invitados
* Permitir que los usuarios soliciten acceso a Brand Portal
* Permitir que los administradores eliminen recursos de Brand Portal
* Permitir la creación de colecciones públicas
* Permitir la creación de colecciones inteligentes públicas
* Permitir aceleración de descargas

### Otras mejoras

* *Ruta de jerarquía de carpetas en las vistas*  de tarjeta y lista: permite a los usuarios saber la ubicación de las carpetas almacenadas dentro de una instancia de Brand Portal. Ayuda a los usuarios a diferenciar carpetas con el mismo nombre dentro de una jerarquía de carpetas diferente.
* *Opción Información general* : proporciona metadatos sobre el recurso o la carpeta a los usuarios que no son administradores. Para ello, seleccione el recurso o la carpeta y, a continuación, seleccione la opción Información general en la barra de herramientas. Actualmente, muestra el título, la fecha de creación y la ruta

### La interfaz de usuario de los hosts de Adobe I/O para configurar las integraciones de oAuth

Brand Portal utiliza la interfaz [https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/) de Adobe I/O para crear la aplicación JWT, que permite configurar integraciones oAuth para permitir la integración de AEM Assets con Brand Portal. Anteriormente, la interfaz de usuario para configurar integraciones de OAuth estaba alojada en `https://marketing.adobe.com/developer/`. Para obtener más información sobre la integración de AEM Assets con Brand Portal para la publicación de recursos y colecciones en Brand Portal, consulte [Configuración de la integración de AEM Assets con Brand Portal](https://helpx.adobe.com/es/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html).

## Mejoras y funciones de Brand Portal de febrero de 2018{#brand-portal-features-and-enhancements-632}

Las nuevas funciones mejoraron la funcionalidad orientada a alinear Brand Portal con AEM.

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

### Mejoras en la navegación

* Interfaz de usuario actualizada que se alinea con el AEM y utiliza la IU de Coral3.
* Acceso rápido y fácil a las herramientas administrativas a través del nuevo logotipo de Adobe.
* Navegación del producto a través de una superposición
* Navegación rápida a carpetas principales desde una carpeta secundaria.
* Opción Omnisearch para navegar a las herramientas administrativas y al contenido.
* La vista de tarjeta, la vista de lista y la vista de columna le ayudan a navegar fácilmente por las carpetas anidadas.
* La ordenación de recursos en la Vista de lista ya no está restringida al número de recursos que se muestran en la pantalla. Todos los recursos de una carpeta se ordenan.

### Mejoras de búsqueda

* La funcionalidad Omnisearch permite realizar una búsqueda rápida de recursos y archivos dentro de Brand Portal.
* Omnisearch también proporciona una opción para buscar recursos dentro de una carpeta o ubicación específica
* Sugerencias de palabras clave automáticas para facilitar la búsqueda
* Mejore su Omnisearch con filtros adicionales. Opción para guardar el resultado de la búsqueda en una colección inteligente para que vuelva a visitar la búsqueda más adelante.
* Admite la búsqueda inteligente de recursos etiquetados
* AEM los recursos con etiquetas inteligentes se pueden compartir de AEM a Brand Portal y usar etiquetas inteligentes para la búsqueda de recursos en Brand Portal.

### Mejoras en el uso compartido de archivos

* El usuario puede compartir un recurso mediante la opción de compartir vínculos.
* Al compartir recursos, el usuario puede establecer una fecha de caducidad para cada recurso. Proporciona a los usuarios más control sobre los recursos compartidos.
* Un usuario externo con un vínculo de uso compartido de recursos puede descargar la imagen y ver sus propiedades.
* La jerarquía de carpetas anidadas original se conserva para las carpetas de recursos descargadas.

### Capacidades administrativas y de informes

* El esquema de metadatos de AEM Assets ahora se puede publicar de AEM a Brand Portal.
* Los administradores pueden crear y administrar tres tipos de informes: recursos descargados, caducados y publicados
* Capacidad para configurar la columna que debe incluirse en el informe.
* Crear ajustes preestablecidos de imagen para los recursos dentro de Brand Portal.
* Posibilidad de modificar Admin Search Rail Form o Search Forms para incluir opciones de filtrado adicionales.
* Actualizar y previsualizar el fondo de pantalla personalizado para su marca
* Informe de uso para obtener información sobre la cantidad de usuarios, el espacio de almacenamiento utilizado y el total de recursos.

## Recursos adicionales{#additional-resources}

* [Novedades de Brand Portal](https://helpx.adobe.com/experience-manager/brand-portal/using/whats-new.html)
* [Agentes de replicación de AEM Author](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [Guía para la descarga acelerada](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Documentos de Adobe de AEM Assets Brand Portal](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal.html)
* [Documentos de Adobe de AEM Assets Dynamic Media](https://docs.adobe.com/docs/es/aem/6-3/author/assets/dynamic-media.html)
* [Descargar Aspera Connect](https://downloads.asperasoft.com/connect2/)
* [Servidor de prueba de Aspera Connect](https://test-connect.asperasoft.com/)