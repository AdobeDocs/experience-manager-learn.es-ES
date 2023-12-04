---
title: Instalación y configuración de Tomcat
description: Esta es la parte 1 del tutorial de varios pasos para crear su primer documento de comunicaciones interactivas. En esta parte, instalaremos TOMCAT e implementaremos el archivo sampleRest.war en TOMCAT.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 4f400c22-6c96-4018-851c-70d988ce7c6c
topic: Development
role: Developer
level: Beginner
exl-id: f0f19838-1ade-4eda-b736-a9703a3916c2
duration: 66
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 0%

---

# Instalación y configuración de Tomcat {#install-and-configure-tomcat}

En esta parte, instalamos TOMCAT e implementamos el archivo sampleRest.war en TOMCAT. El extremo REST expuesto por este archivo WAR es la base de nuestra fuente de datos y el modelo de datos de formulario.

Para configurar tomcat, siga las siguientes instrucciones:

1. Descargue e instale JDK1.8.
2. Establezca JAVA_HOME para que apunte a JDK1.8.
3. Descargar [gato](https://tomcat.apache.org/). Este archivo de guerra se ha probado con las versiones 8.5.x y 9.0.x de Tomcat.
4. Descargue la versión tomcat de su preferencia. Puede descargar el zip de Windows de 64 bits en la sección principal.
5. Descomprima el contenido en su c:\tomcat.
6. Debería ver algo como esto en su unidad C **c:\tomcat\apache-tomcat-8.5.27** dependiendo de la versión de su tomcat
7. Cree una variable de entorno llamada &quot;CATALINA_HOME&quot; y establezca su valor en el ejemplo de la carpeta de instalación de tomcat c:\tomcat\apache- tomcat-8.5.27
8. Copie el archivo SampleRest.war en la carpeta webapps de su instalación de Tomcat
9. Iniciar nueva ventana del símbolo del sistema.
10. Vaya a &lt;tomcat install=&quot;&quot; folder=&quot;&quot;>\bin y ejecute startup.bat
11. Una vez que haya comenzado el tomcat, pruebe el punto final expuesto por el archivo WAR mediante [haciendo clic aquí](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. Debe obtener datos de ejemplo como resultado de esta llamada.

Felicitaciones !!!. Ha configurado tomcat e implementado el archivo SampleRest.war.

## Pasos siguientes

[Configurar la fuente de datos RESTful](./parttwo.md)
