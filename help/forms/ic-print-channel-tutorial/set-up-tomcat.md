---
title: Instalar y configurar el vídeo de Tomcat
seo-title: Install and Configure Tomcat
description: Esta es la primera parte del tutorial de varios pasos para crear su primer documento interactivo de comunicaciones.
uuid: 835e2342-82b6-4f0c-9a6b-467bbbd8527a
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
thumbnail: 37815.jpg
discoiquuid: 5f68be3d-aa35-4a3f-aaea-b8ee213c87ae
topic: Development
role: Developer
level: Beginner
exl-id: faa9ca2d-6cfa-4abf-be5e-3e549202853a
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 0%

---

# Instalación y configuración de Tomcat {#install-and-configure-tomcat}

En esta parte, instalamos TOMCAT e implementamos el archivo sampleRest.war en TOMCAT. El punto final de REST expuesto por este archivo WAR es la base de nuestro modelo de datos de fuentes de datos y formularios.

>[!VIDEO](https://video.tv.adobe.com/v/37815/?quality=9&learn=on)

Para configurar tomcat, siga las siguientes instrucciones:

1. Descargue e instale JDK1.8.
2. Establezca JAVA_HOME para que apunte a JDK1.8.
3. Descargar [tomcat](https://tomcat.apache.org/). Este archivo war ha sido probado con Tomcat versión 8.5.x y 9.0.x.
4. Descargue la versión de tomcat de su preferencia. Puede descargar el zip de ventanas de 64 bits en la sección principal.
5. Descomprima el contenido en su c:\tomcat.
6. Debería ver algo como esto en su unidad c **c:\tomcat\apache-tomcat-8.5.27** según la versión de tomcat
7. Cree una variable de entorno llamada &quot;CATALINA_HOME&quot; y establezca su valor en la carpeta de instalación de tomcat ejemplo c:\tomcat\apache- tomcat-8.5.27
8. Copie el archivo SampleRest.war en la carpeta webapps de la instalación de Tomcat.
9. Inicie la nueva ventana del símbolo del sistema.
10. Vaya a &lt;tomcat install=&quot;&quot; folder=&quot;&quot;>\bin y ejecutar startup.bat
11. Una vez que su tomcat haya comenzado, pruebe el punto final expuesto por WAR File de acuerdo con [haga clic aquí](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. Debe obtener datos de ejemplo como resultado de esta llamada.

Felicitaciones !!!. Ha configurado el archivo tomcat e implementado el archivo SampleRest.war.

En el siguiente vídeo se explica la implementación de la aplicación de ejemplo en Tomcat.
>[!VIDEO](https://video.tv.adobe.com/v/37815)
