---
title: Pasos simplificados para instalar AEM Forms en Windows
description: Pasos rápidos y sencillos para instalar AEM Forms en windows
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Admin
level: Beginner
exl-id: 80288765-0b51-44a9-95d3-3bdb2da38615
source-git-commit: 5c53919dd038c0992e1fe5dd85053f26c03c5111
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 5%

---

# Pasos simplificados para instalar AEM Forms en Windows

>[!NOTE]
>
>No haga doble clic en el jar de inicio rápido de AEM si quiere utilizar AEM Forms.
>
>Además, asegúrese de que no haya espacios en la ruta de la carpeta Instalación de AEM Forms.
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
   * AEM 6.2 necesita: Oracle SE 8 JDK 1.8.x (64 bits)
* 
   * AEM 6.3 y AEM 6.4 necesita: Oracle SE 8 JDK 1.8.x (64 bits)
* AEM 6.5 necesita JDK 8 o JDK 11
* [Requisitos oficiales de JDK](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/introduction/technical-requirements.html?lang=es) se enumeran aquí
* Asegúrese de que JAVA_HOME esté configurado para que apunte al JDK que ha instalado.
   * Para crear la variable JAVA_HOME en Windows, siga los pasos a continuación:
      * Haga clic con el botón derecho en Mi PC y seleccione Propiedades
      * En la ficha Avanzado , seleccione Variables de entorno y cree una nueva variable de sistema llamada JAVA_HOME.
      * Establezca el valor de la variable para que apunte a JDK instalado en su sistema. Por ejemplo c:\program files\java\jdk1.8.0_25

* Cree una carpeta denominada AEMForms en su unidad C
* Busque AEMQuickStart.Jar y muévalo a la carpeta AEMForms
* Copie el archivo license.properties en esta carpeta de AEMForms
* Cree un archivo por lotes llamado &quot;StartAemForms.bat&quot; con el siguiente contenido:
   * java -d64 -Xmx2048M -jar AEM_6.5_Quickstart.jar -gui. Aquí AEM_6.5_Quickstart.jar es el nombre de mi jar de inicio rápido AEM.
   * Puede cambiar el nombre de su jar por cualquier nombre, pero asegúrese de que ese nombre se refleja en el archivo por lotes. Guarde el archivo por lotes en la carpeta AEMForms.

* Abra un nuevo símbolo del sistema y vaya a _c:\aemforms_.

* Ejecute el archivo StartAemForms.bat desde el símbolo del sistema.

* Debe obtener un cuadro de diálogo pequeño que indique el progreso del inicio.

* Una vez finalizado el inicio, abra el archivo sling.properties . Esto se encuentra en c:\AEMForms\crx-quickstart\conf folder.

* Copie las 2 líneas siguientes hacia la parte inferior del archivo
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.&#42;** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider=org.bouncycastle.&#42;**
* Estas dos propiedades son necesarias para que document services funcione
* Guarde el archivo sling.properties
* [Descargue el paquete de complementos de formularios apropiado](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=en)
* Instalación del complemento de formularios en el paquete mediante [gestor de paquetes.](http://localhost:4502/crx/packmgr/index.jsp)
* Después de instalar add on package, es necesario seguir los siguientes pasos

       * **Asegúrese de que todos los paquetes estén en estado activo. (Excepto para el paquete de firmas AEMFD).**
       * **Normalmente tardaría 5 o más minutos en que todos los paquetes llegaran al estado activo.**
   
   * **Una vez que todos los paquetes estén activos (excepto el paquete de firmas AEMFD), reinicie el sistema para completar la instalación de AEM Forms**

## paquete sun.util.calendar a la lista de permitidos

1. Abra la consola web Felix en su [ventana del explorador](http://localhost:4502/system/console/configMgr)
2. Busque y abra la Configuración de Deserialization Firewall: `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
3. Agregar `sun.util.calendar` como nueva entrada en `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`
4. Guarde los cambios.

Felicitaciones!!! Ya ha instalado y configurado AEM Forms en su sistema.
Según sus necesidades, puede configurar  [Extensiones de Reader](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html?lang=en) o [ PDFG](https://experienceleague.adobe.com/docs/experience-manager-64/forms/install-aem-forms/osgi-installation/install-configure-document-services.html?lang=es) en el servidor
