---
title: Pasos simplificados para instalar AEM Forms en Windows
seo-title: Pasos simplificados para instalar AEM Forms en Windows
description: Pasos rápidos y sencillos para instalar AEM Forms en Windows
seo-description: Pasos rápidos y sencillos para instalar AEM Forms en Windows
uuid: a148b8f0-83db-47f6-89d3-c8a9961be289
feature: adaptive-forms
topics: administration
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 1182ef4d-5838-433b-991d-e24ab805ae0e
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '613'
ht-degree: 2%

---


# Pasos simplificados para instalar AEM Forms en Windows

>[!NOTE]
>
>No haga doble en el tarro de Inicio rápido de AEM si desea utilizar AEM Forms.
>
>Además, asegúrese de que no haya espacios en la ruta de la carpeta Instalación de AEM Forms.
>
>Por ejemplo, no instale AEM Forms en c:\jack and jill\AEM Forms folder

>[!NOTE]
>
>Si va a instalar AEM Forms 6.5, asegúrese de que ha instalado los siguientes redistribuibles de Microsoft Visual C++ de 32 bits.
>
>* Microsoft Visual C++ 2008 redistribuible
>* Microsoft Visual C++ 2010 redistribuible
>* Microsoft Visual C++ 2012 redistribuible
>* Microsoft Visual C++ 2013 redistribuible (a partir de 6.5)


Aunque recomendamos seguir la documentación [](https://helpx.adobe.com/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html) oficial para instalar AEM Forms. Se pueden seguir los siguientes pasos para instalar y configurar AEM Forms en el entorno de Windows:

* Asegúrese de tener instalado el JDK correspondiente
   * AEM 6.2 necesita: Oracle SE 8 JDK 1.8.x (64 bits)
* 
   * AEM 6.3 y AEM 6.4 necesita: Oracle SE 8 JDK 1.8.x (64 bits)
* AEM 6.5 necesita JDK 8 o JDK 11
* [Los requisitos](https://helpx.adobe.com/experience-manager/6-3/sites/deploying/using/technical-requirements.html) oficiales de JDK se enumeran aquí
* Asegúrese de que JAVA_HOME está configurado para que apunte al JDK que ha instalado.
   * Para crear la variable JAVA_HOME en Windows, siga los pasos a continuación:
      * Haga clic con el botón derecho en Mi PC y seleccione Propiedades
      * En la ficha Avanzadas, seleccione Variables de Entorno y cree una nueva variable de sistema llamada JAVA_HOME.
      * Establezca el valor de la variable para que apunte a JDK instalado en su sistema. Por ejemplo: c:\program files\java\jdk1.8.0_25

* Cree una carpeta llamada AEMForms en la unidad C
* Busque AEMQuickStart.Jar y muévala a la carpeta AEMForms
* Copie el archivo license.properties en esta carpeta AEMForms
* Cree un archivo por lotes llamado &quot;StartAemForms.bat&quot; con el siguiente contenido:
   * java -d64 -Xmx2048M -jar AEM_6.3_Quickstart.jar -gui.Aquí AEM_6.3_Quickstart.jar es el nombre de mi tarro AEM de inicio rápido.
   * Puede cambiar el nombre del archivo .jar por cualquier nombre, pero asegúrese de que el nombre se refleja en el archivo de lote.Guarde el archivo de lote en la carpeta AEMForms.

* Abra un nuevo símbolo del sistema y vaya a c:\aemforms.

* Ejecute el archivo StartAemForms.bat desde el símbolo del sistema.

* Debe obtener un cuadro de diálogo pequeño que indique el progreso del inicio.

* Una vez que se haya completado el inicio, abra el archivo sling.properties. Esta ubicación se encuentra en c:\AEMForms\crx-quickstart\conf folder.

* Copie las 2 líneas siguientes hacia la parte inferior del archivo
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.*** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider=org.bouncycastle.***
* Estas dos propiedades son necesarias para que los servicios de documento funcionen
* Guarde el archivo sling.properties

* [Iniciar sesión en Package Share](http://localhost:4502/crx/packageshare/login.html)

   * Necesitará AdobeId para iniciar sesión en Package Share
   * Busque AEM Forms Añada en el paquete adecuado para su versión de AEM Forms y sistema operativo
   * O [puede descargar el paquete de complemento de formularios correspondiente](https://helpx.adobe.com/es/aem-forms/kb/aem-forms-releases.html)
   * Después de instalar el complemento en el paquete, deben seguirse los siguientes pasos

      * **Asegúrese de que todos los paquetes están en estado activo. (Excepto para el paquete de firmas AEMFD).**
      * **Normalmente tardaría 5 o más minutos en que todos los paquetes llegaran al estado activo.**
   * **Una vez que todos los paquetes estén activos (excepto el paquete de firmas AEMFD), reinicie el sistema para completar la instalación de AEM Forms**


* Añadir `sun.util.calendar` paquete a la lista de permitidos:

   1. Abrir la consola web Felix en la ventana [del navegador](http://localhost:4502/system/console/configMgr)
   2. Buscar y abrir Configuración del cortafuegos de deserialización: `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
   3. Añadir `sun.util.calendar` como una nueva entrada en `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`
   4. Guarde los cambios.

Felicitaciones!!! Ya ha instalado y configurado AEM Forms en su sistema.
Según sus necesidades, puede configurar las extensiones [](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html) Reader o [ PDFG](https://helpx.adobe.com/experience-manager/6-3/forms/using/install-configure-pdf-generator.html) en el servidor
