---
title: Configuración del envío del documento de canal web
seo-title: Configuración del envío del documento de canal web
description: Esta es la última parte de un tutorial de varios pasos para crear su primer documento interactivo de comunicaciones. En esta parte, vemos el envío del documento del canal web por correo electrónico.
seo-description: Esta es la última parte de un tutorial de varios pasos para crear su primer documento interactivo de comunicaciones. En esta parte, vemos el envío del documento del canal web por correo electrónico.
uuid: c1066600-1abd-4401-b04f-b93c28603cc7
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 1a7cf095-c5d8-4d92-a018-883cda76fe70
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '389'
ht-degree: 1%

---


# Configuración del envío del documento de canal web {#setting-up-the-delivery-of-web-channel-document}


En esta parte, vemos el envío del documento del canal web por correo electrónico.

Una vez definido y probado el documento de comunicación interactivo de canal web, necesita un mecanismo de envío para entregar el documento de canal web al destinatario.

Para poder utilizar el correo electrónico como un mecanismo de envío para nuestro documento de canal web, necesitamos realizar un cambio menor en el modelo de datos de formulario.

[Para obtener más información sobre el envío de canal web por correo electrónico](/help/forms/interactive-communications/delivery-of-web-channel-document-tutorial-use.md)

Inicie sesión en AEM Forms.

* Vaya a Forms ->Integraciones de datos

* Abra el modelo de datos RetirementAccountStatement en modo de edición.

* Seleccione el objeto balances y haga clic en el botón de edición.

* Seleccione el icono &quot;lápiz&quot; para abrir el argumento de identificación en modo de edición.

* Cambie el enlace a &quot;RequestAttribute&quot;.

* Establezca el número de cuenta en el valor de enlace como se muestra a continuación.

* De esta manera se pasa el número de cuenta a través del atributo de solicitud al modelo de datos del formulario

* Asegúrese de guardar los cambios.
   ![fdm](assets/requestattribute.gif)

## Probar Envío de correo electrónico del Documento de Canal web {#test-email-delivery-of-web-channel-document}

* [Instalación de los recursos de muestra mediante el administrador de paquetes](assets/webchanneldelivery.zip)
* [Iniciar sesión en crx](http://localhost:4502/crx/de/index.jsp#)

* Vaya a /home/users

* Busque un usuario administrador en el nodo del usuario.

* Seleccione el nodo de perfil del usuario administrador.

* Cree una propiedad llamada &quot;accountnumber&quot;. Asegúrese de que el tipo de propiedad es una cadena.

* Establezca el valor de esta propiedad accountnumber en &quot;3059827&quot;. Puede establecer este valor en cualquier número aleatorio como desee.

* [Abra getad.html](http://localhost:4502/content/getad.html)

* El código asociado con esta URL obtendrá el número de cuenta del usuario que ha iniciado sesión. A continuación, este número de cuenta se pasa como atributo de solicitud a FDM. El FDM recuperará los datos asociados con este número de cuenta y rellenará el documento de canal web.

>[!NOTE]
>
>Consulte el archivo **/apps/AEMForms/fetchad/GET.jsp** en crx. Asegúrese de que la variable de cadena webChannelDocument apunte a una ruta de documento de comunicación válida.
