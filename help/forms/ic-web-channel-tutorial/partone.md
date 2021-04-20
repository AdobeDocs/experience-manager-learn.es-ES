---
title: Instalación y configuración de Tomcat
seo-title: Instalación y configuración de Tomcat
description: Esta es la parte 1 del tutorial de varios pasos para crear su primer documento interactivo de comunicaciones. En esta parte, instalaremos TOMCAT e implementaremos el archivo sampleRest.war en TOMCAT. El punto final de REST expuesto por este archivo WAR será la base de nuestra fuente de datos y del modelo de datos de formulario.
seo-description: Esta es la parte 1 del tutorial de varios pasos para crear su primer documento interactivo de comunicaciones. En esta parte, instalaremos TOMCAT e implementaremos el archivo sampleRest.war en TOMCAT. El punto final de REST expuesto por este archivo WAR será la base de nuestra fuente de datos y del modelo de datos de formulario.
uuid: c6d4c74c-ea16-4c63-92c9-182d087fd88c
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 4f400c22-6c96-4018-851c-70d988ce7c6c
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 1%

---


# Instalar y configurar Tomcat {#install-and-configure-tomcat}

En esta parte, instalaremos TOMCAT e implementaremos el archivo sampleRest.war en TOMCAT. El punto final de REST expuesto por este archivo WAR será la base de nuestra fuente de datos y del modelo de datos de formulario.

Para configurar tomcat, siga las siguientes instrucciones:

1. Descargue e instale JDK1.8.
2. Establezca JAVA_HOME para que apunte a JDK1.8.
3. Descargar [tomcat](https://tomcat.apache.org/). Este archivo war ha sido probado con Tomcat versión 8.5.x y 9.0.x.
4. Descargue la versión de tomcat de su preferencia. Puede descargar el zip de ventanas de 64 bits en la sección principal.
5. Descomprima el contenido en su c:\tomcat.
6. Debería ver algo como esto en su unidad c **c:\tomcat\apache-tomcat-8.5.27** en función de la versión de su tomcat
7. Cree una variable de entorno llamada &quot;CATALINA_HOME&quot; y establezca su valor en la carpeta de instalación de tomcat ejemplo c:\tomcat\apache- tomcat-8.5.27
8. Copie el archivo SampleRest.war en la carpeta webapps de la instalación de Tomcat.
9. Inicie la nueva ventana del símbolo del sistema.
10. Vaya a &lt;tomcat install folder>\bin y ejecute startup.bat
11. Una vez que su tomcat haya comenzado, pruebe el punto final expuesto por WAR File [haciendo clic aquí](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. Debe obtener datos de ejemplo como resultado de esta llamada.

Felicitaciones !!!. Ha configurado el archivo tomcat e implementado el archivo SampleRest.war.
