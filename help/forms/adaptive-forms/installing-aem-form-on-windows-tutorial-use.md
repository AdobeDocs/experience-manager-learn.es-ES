---
title: Pasos simplificados para la instalación de AEM Forms en Windows
seo-title: Pasos simplificados para la instalación de AEM Forms en Windows
description: Pasos rápidos y sencillos para instalar AEM Forms en windows
seo-description: Pasos rápidos y sencillos para instalar AEM Forms en windows
uuid: a148b8f0-83db-47f6-89d3-c8a9961be289
feature: formularios adaptables
topics: administration
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 1182ef4d-5838-433b-991d-e24ab805ae0e
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '614'
ht-degree: 4%

---


# Pasos simplificados para la instalación de AEM Forms en Windows

>[!NOTE]
>
>Nunca haga doble clic en el jar de inicio rápido de AEM si quiere utilizar AEM Forms.
>
>Además, asegúrese de que no haya espacios en la ruta de la carpeta de instalación de AEM Forms.
>
>Por ejemplo, no instale AEM Forms en c:\jack and jill\AEM Forms folder

>[!NOTE]
>
>Si va a instalar AEM Forms 6.5, asegúrese de que ha instalado los siguientes redistribuibles de Microsoft Visual C+++ de 32 bits.
>
>* Microsoft Visual C++ 2008 redistribuible
>* Microsoft Visual C++ 2010 redistribuible
>* Microsoft Visual C++ 2012 redistribuible
>* Microsoft Visual C++ 2013 redistribuible (a partir de la versión 6.5)


Aunque recomendamos seguir la [documentación oficial](https://helpx.adobe.com/es/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html) para instalar AEM Forms. Se pueden seguir los siguientes pasos para instalar y configurar AEM Forms en el entorno Windows:

* Asegúrese de tener instalado el JDK apropiado
   * AEM 6.2 que necesita: Oracle SE 8 JDK 1.8.x (64 bits)
* 
   * AEM 6.3 y AEM 6.4 que necesita: Oracle SE 8 JDK 1.8.x (64 bits)
* AEM 6.5 necesita JDK 8 o JDK 11
* [Los ](https://helpx.adobe.com/experience-manager/6-3/sites/deploying/using/technical-requirements.html) requisitos oficiales de JDK se enumeran aquí
* Asegúrese de que JAVA_HOME esté configurado para que apunte al JDK que ha instalado.
   * Para crear la variable JAVA_HOME en Windows, siga los pasos a continuación:
      * Haga clic con el botón derecho en Mi PC y seleccione Propiedades
      * En la ficha Avanzado , seleccione Variables de entorno y cree una nueva variable de sistema llamada JAVA_HOME.
      * Establezca el valor de la variable para que apunte a JDK instalado en su sistema. Por ejemplo c:\program files\java\jdk1.8.0_25

* Cree una carpeta denominada AEMForms en su unidad C
* Busque AEMQuickStart.Jar y muévalo a la carpeta AEMForms
* Copie el archivo license.properties en esta carpeta de AEMForms
* Cree un archivo por lotes llamado &quot;StartAemForms.bat&quot; con el siguiente contenido:
   * java -d64 -Xmx2048M -jar AEM_6.3_Quickstart.jar -gui.Aquí AEM_6.3_Quickstart.jar es el nombre de mi jar de inicio rápido de AEM.
   * Puede cambiar el nombre de su jar por cualquier nombre, pero asegúrese de que ese nombre se refleja en el archivo por lotes. Guarde el archivo por lotes en la carpeta AEMForms.

* Abra un nuevo símbolo del sistema y vaya a c:\aemforms.

* Ejecute el archivo StartAemForms.bat desde el símbolo del sistema.

* Debe obtener un cuadro de diálogo pequeño que indique el progreso del inicio.

* Una vez finalizado el inicio, abra el archivo sling.properties . Esto se encuentra en c:\AEMForms\crx-quickstart\conf folder.

* Copie las 2 líneas siguientes hacia la parte inferior del archivo
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.*** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider=org.bouncycastle.***
* Estas dos propiedades son necesarias para que document services funcione
* Guarde el archivo sling.properties

* [Iniciar sesión en Package Share](http://localhost:4502/crx/packageshare/login.html)

   * Necesitará AdobeId para iniciar sesión en el uso compartido de paquetes
   * Buscar AEM Forms Agregar en el paquete apropiado para su versión de AEM Forms y sistema operativo
   * O [puede descargar el paquete de complementos de formularios apropiado](https://helpx.adobe.com/es/aem-forms/kb/aem-forms-releases.html)
   * Después de instalar el add on package, es necesario seguir los siguientes pasos

      * **Asegúrese de que todos los paquetes estén en estado activo. (Excepto para el paquete de firmas AEMFD).**
      * **Normalmente tardaría 5 o más minutos en que todos los paquetes llegaran al estado activo.**
   * **Una vez que todos los paquetes estén activos (excepto el paquete de firmas AEMFD), reinicie el sistema para completar la instalación de AEM Forms**


* Añada el paquete `sun.util.calendar` a la lista de permitidos:

   1. Abra la consola web Felix en su [ventana del navegador](http://localhost:4502/system/console/configMgr)
   2. Busque y abra la Configuración de Deserialization Firewall: `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
   3. Agregue `sun.util.calendar` como una nueva entrada en `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`
   4. Guarde los cambios.

Felicitaciones!!! Ya ha instalado y configurado AEM Forms en su sistema.
Según sus necesidades, puede configurar [Reader Extensions](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html) o [ PDFG](https://helpx.adobe.com/experience-manager/6-3/forms/using/install-configure-pdf-generator.html) en el servidor
