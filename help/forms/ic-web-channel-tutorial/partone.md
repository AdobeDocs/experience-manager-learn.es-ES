---
title: Instalación y configuración de Tomcat
seo-title: Install and Configure Tomcat
description: Esta es la parte 1 del tutorial de varios pasos para crear su primer documento interactivo de comunicaciones. En esta parte, instalaremos TOMCAT e implementaremos el archivo sampleRest.war en TOMCAT.
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
exl-id: f0f19838-1ade-4eda-b736-a9703a3916c2
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '236'
ht-degree: 0%

---

# Instalación y configuración de Tomcat {#install-and-configure-tomcat}

En esta parte, instalamos TOMCAT e implementamos el archivo sampleRest.war en TOMCAT. El punto final de REST expuesto por este archivo WAR es la base de nuestro modelo de datos de fuentes de datos y formularios.

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
