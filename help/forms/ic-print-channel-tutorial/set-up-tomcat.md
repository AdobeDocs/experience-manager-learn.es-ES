---
title: Instalar y configurar Tomcat
seo-title: Instalar y configurar Tomcat
description: Esta es la primera parte de un tutorial de varios pasos para crear su primer documento de comunicaciones interactivo.En esta parte, instalaremos TOMCAT e implementaremos el archivo sampleRest.war en TOMCAT. El punto final de REST expuesto por este archivo WAR será la base de nuestro modelo de datos de formulario y fuente de datos.
seo-description: Esta es la primera parte de un tutorial de varios pasos para crear su primer documento de comunicaciones interactivo.En esta parte, instalaremos TOMCAT e implementaremos el archivo sampleRest.war en TOMCAT. El punto final de REST expuesto por este archivo WAR será la base de nuestro modelo de datos de formulario y fuente de datos.
uuid: 835e2342-82b6-4f0c-9a6b-467bbbd8527a
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
thumbnail: 37815.jpg
discoiquuid: 5f68be3d-aa35-4a3f-aaea-b8ee213c87ae
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 0%

---


# Instalar y configurar Tomcat {#install-and-configure-tomcat}

En esta parte, instalaremos TOMCAT e implementaremos el archivo sampleRest.war en TOMCAT. El punto final de REST expuesto por este archivo WAR será la base de nuestro modelo de datos de formulario y fuente de datos.

>[!VIDEO](https://video.tv.adobe.com/v/37815/?quality=9&learn=on)

Para configurar tomcat, siga las siguientes instrucciones:

1. Descargue e instale JDK1.8.
2. Establezca JAVA_HOME para que apunte a JDK1.8.
3. Descargue [tomcat](https://tomcat.apache.org/). Este archivo de guerra se ha probado con Tomcat versión 8.5.x y 9.0.x.
4. Descargue la versión de tomcat de sus preferencias. Puede descargar el zip de ventanas de 64 bits en la sección principal.
5. Descomprima el contenido en su c:\tomcat.
6. Debería ver algo como esto en la unidad c **c:\tomcat\apache-tomcat-8.5.27** según la versión de su tomcat
7. Cree una variable de entorno llamada &quot;CATALINA_HOME&quot; y defina su valor en el ejemplo de la carpeta de instalación de tomcat c:\tomcat\apache- tomcat-8.5.27
8. Copie el archivo SampleRest.war en la carpeta webapps de la instalación de Tomcat
9. Ventana nueva del símbolo del sistema de inicio.
10. Vaya a &lt;carpeta de instalación de tomcat>\bin y ejecute startup.bat
11. Una vez que el tomcat haya empezado, pruebe el punto final expuesto por el archivo WAR haciendo [clic aquí](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. Debe obtener datos de muestra como resultado de esta llamada.

Felicitaciones !!!. Ha configurado el comando para implementar el archivo SampleRest.war.

El siguiente vídeo explica la implementación de una aplicación de muestra en Tomcat
>[!VIDEO](https://video.tv.adobe.com/v/37815)