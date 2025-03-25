---
title: Configuración del envío del documento del canal web
description: Esta es la parte final de un tutorial de varios pasos para crear el primer documento de comunicaciones interactivas. En esta parte, vemos la entrega del documento del canal web por correo electrónico.
feature: Interactive Communication
doc-type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
discoiquuid: 1a7cf095-c5d8-4d92-a018-883cda76fe70
topic: Development
role: Developer
level: Beginner
exl-id: 510d1782-59b9-41a6-a071-a16170f2cd06
duration: 68
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 0%

---

# Configuración del envío del documento del canal web {#setting-up-the-delivery-of-web-channel-document}


En esta parte, vemos la entrega del documento del canal web por correo electrónico.

Una vez definido y probado el documento de comunicación interactiva del canal web, necesita un mecanismo de entrega para enviar el documento del canal web al destinatario.

Para poder utilizar el correo electrónico como mecanismo de envío para nuestro documento de canal web, necesitamos realizar un cambio menor en el modelo de datos de formulario.

[Para obtener más información sobre la entrega del canal web por correo electrónico](/help/forms/interactive-communications/delivery-of-web-channel-document-tutorial-use.md)

Inicie sesión en AEM Forms.

* Vaya a Forms ->Integraciones de datos

* Abra el modelo de datos RetirementAccountStatement en modo de edición.

* Seleccione el objeto balances y pulse en el botón editar.

* Seleccione el icono &quot;lápiz&quot; para abrir el argumento id en modo de edición.

* Cambie el enlace a RequestAttribute.

* Configure el número de cuenta en el valor de enlace como se muestra a continuación.

* De este modo, se pasa el número de cuenta a través del atributo de solicitud al modelo de datos de formulario

* Asegúrese de guardar los cambios.
  ![fdm](assets/requestattribute.gif)

## Probar la entrega por correo electrónico del documento del canal web {#test-email-delivery-of-web-channel-document}

* [Instalación de los recursos de muestra mediante el administrador de paquetes](assets/webchanneldelivery.zip)
* [Iniciar sesión en crx](http://localhost:4502/crx/de/index.jsp#)

* Vaya a /home/users

* Busque el usuario administrador en el nodo del usuario.

* Seleccione el nodo de perfil del usuario administrador.

* Cree una propiedad denominada &quot;accountnumber&quot;. Asegúrese de que el tipo de propiedad sea una cadena.

* Establezca el valor de esta propiedad accountnumber en &quot;3059827&quot;. Puede establecer este valor en cualquier número aleatorio que desee.

* [Abrir getad.html](http://localhost:4502/content/getad.html)

* El código asociado con esta URL obtendrá el número de cuenta del usuario que ha iniciado sesión. Este número de cuenta se pasa como atributo de solicitud al FDM. A continuación, FDM recuperará los datos asociados con este número de cuenta y rellenará el documento del canal web.

>[!NOTE]
>
>Eche un vistazo al archivo **/apps/AEMForms/fetchad/GET.jsp** en crx. Asegúrese de que la variable de cadena webChannelDocument señala a una ruta de documento de comunicación válida.

## Siguientes pasos

[Configuración de la entrega de correo electrónico](../interactive-communications/delivery-of-web-channel-document-tutorial-use.md)