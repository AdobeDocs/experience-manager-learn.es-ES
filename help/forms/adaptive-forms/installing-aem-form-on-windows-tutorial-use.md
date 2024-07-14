---
title: Pasos simplificados para instalar AEM Forms en Windows
description: Pasos rápidos y sencillos para instalar AEM Forms en Windows
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Admin
level: Beginner
exl-id: 80288765-0b51-44a9-95d3-3bdb2da38615
last-substantial-update: 2020-06-09T00:00:00Z
duration: 113
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 1%

---

# Pasos simplificados para instalar AEM Forms en Windows

>[!NOTE]
>
>AEM Nunca haga doble clic en el JAR de inicio rápido de la si desea utilizar AEM Forms.
>
>Además, asegúrese de que no haya espacios en la ruta de la carpeta de instalación de AEM Forms.
>
>Por ejemplo, no instale AEM Forms en las carpetas c:\jack y jill\AEM Forms

>[!NOTE]
>
>Si va a instalar AEM Forms 6.5, asegúrese de que ha instalado los siguientes redistribuibles de 32 bits de Microsoft Visual C++.
>
>* Microsoft Visual C++ 2008 redistribuible
>* Microsoft Visual C++ 2010 redistribuible
>* Microsoft Visual C++ 2012 redistribuible
>* Microsoft Visual C++ 2013 redistribuible (a partir de 6.5)

Aunque recomendamos seguir la [documentación oficial](https://helpx.adobe.com/es/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html) para instalar AEM Forms. Se pueden seguir los siguientes pasos para instalar y configurar AEM Forms en el entorno de Windows:

* Asegúrese de que tiene instalado el JDK adecuado
   * AEM 6.2 que necesita: Oracle SE 8 JDK 1.8.x (64 bits)
   * AEM AEM 6.3 y 6.4 de la que usted necesita: Oracle SE 8 JDK 1.8.x (64bit)
   * AEM.5 necesita JDK 8 o JDK 11
   * [Los requisitos oficiales de JDK](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/introduction/technical-requirements.html?lang=es) se enumeran aquí
* Asegúrese de que JAVA_HOME está configurado para que apunte al JDK instalado.
   * Para crear la variable JAVA_HOME en ventanas, siga los pasos a continuación:
      * Haga clic con el botón derecho en Mi PC y seleccione Propiedades
      * En la pestaña Avanzadas, seleccione Variables de entorno y cree una nueva variable del sistema llamada JAVA_HOME.
      * Configure el valor de la variable para que apunte al JDK instalado en el sistema. Por ejemplo, c:\archivos de programa\java\jdk1.8.0_25

* Cree una carpeta llamada AEM Forms en la unidad C
* Busque AEMQuickStart.Jar y muévalo a la carpeta AEMForms
* Copie el archivo license.properties en esta carpeta de AEM Forms
* Cree un archivo por lotes llamado &quot;StartAemForms.bat&quot; con el siguiente contenido:
   * `java -d64 -Xmx2048M -jar AEM_6.5_Quickstart.jar -gui`
      * AEM AEM Aquí 6.5_Quickstart.jar es el nombre de mi jar de inicio rápido de la aplicación de inicio rápido de la aplicación.
   * Puede cambiar el nombre del JAR por cualquier nombre, pero asegúrese de que ese nombre se refleje en el archivo por lotes. Guarde el archivo por lotes en la carpeta de AEM Forms.

* Abra un nuevo símbolo del sistema y vaya a _c:\aemforms_.

* Ejecute el archivo StartAemForms.bat desde el símbolo del sistema.

* Debe obtener un pequeño cuadro de diálogo que indique el progreso del inicio.

* Una vez completado el inicio, abra el archivo sling.properties. Se encuentra en la carpeta c:\AEMForms\crx-quickstart\conf.

* Copie las 2 líneas siguientes hacia la parte inferior del archivo
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.&#42;** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider=org.bouncycastle.&#42;**
* Estas dos propiedades son necesarias para que funcionen los servicios de documentos
* Guarde el archivo sling.properties
* [Descargar el paquete de complementos de formularios correspondiente](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=es)
* Instale el paquete de complementos de formularios usando [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp).
* Después de instalar add on package, deben seguirse los siguientes pasos

   * **Asegúrese de que todos los paquetes estén en estado activo. (Excepto el paquete de firmas de AEMFD).**
   * **Normalmente, todos los paquetes tardarían 5 minutos o más en estar activos.**

   * **Una vez que todos los paquetes estén activos (excepto el paquete de firmas de AEMFD), reinicie el sistema para completar la instalación de AEM Forms**

## paquete sun.util.calendar en la lista de permitidos

1. Abra la consola web Felix en la [ventana del explorador](http://localhost:4502/system/console/configMgr)
1. Busque y abra Configuración del firewall de deserialización: `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
1. Agregar `sun.util.calendar` como nueva entrada en `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`
1. Guarde los cambios.

Felicitaciones!!! Ya ha instalado y configurado AEM Forms en su sistema.
Según sus necesidades, puede configurar [Extensiones de Reader](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html) o [PDFG](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/install-configure-document-services.html) en su servidor
