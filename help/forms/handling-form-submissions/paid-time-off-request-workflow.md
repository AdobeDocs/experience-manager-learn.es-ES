---
title: Flujo de trabajo de solicitud de tiempo de espera pagado simple
description: Ocultar y mostrar paneles de formulario adaptables en AEM flujo de trabajo
uuid: 28ceb72b-24d9-488e-92af-7e85775dc682
feature: integrations
topics: workflow
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
discoiquuid: 1c4822e6-76ce-446b-98cb-408900d68b24
translation-type: tm+mt
source-git-commit: 449202af47b6bbcd9f860d5c5391d1f7096d489e
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 0%

---


# Flujo de trabajo de solicitud de tiempo de espera pagado simple

En este artículo analizaremos un flujo de trabajo sencillo que se utiliza para solicitar el tiempo de espera pagado. Los requisitos comerciales son los siguientes:

* El usuario A solicita tiempo de espera rellenando un formulario adaptable.
* El formulario se dirige a AEM usuario administrador (en la vida real, se dirigirá al administrador del remitente)
* El administrador abre el formulario. El administrador no debe poder editar la información que rellene el remitente.
* La sección Aprobador debe ser visible para el aprobador (en este caso es el usuario administrador AEM).

Para cumplir el requisito anterior, se utiliza un campo oculto denominado **initialstep** en el formulario y su valor predeterminado se establece en Sí.Cuando se envía el formulario, el primer paso del flujo de trabajo establece el valor de initialstep en No. El formulario tiene reglas comerciales para ocultar y mostrar las secciones correspondientes en función del valor del paso inicial.

**Configurar formulario para activar AEM flujo de trabajo**

>[!VIDEO](https://video.tv.adobe.com/v/28406?quality=9&learn=on)

**Recorrido del flujo de trabajo**

>[!VIDEO](https://video.tv.adobe.com/v/28407?quality=9&learn=on)

**Vista por parte del remitente del formulario de solicitud de tiempo de espera**

![initialstep](assets/initialstep.gif)

**Vista del aprobador del formulario**

![vista de aprobador](assets/approversview.gif)

En la vista del aprobador, el aprobador no puede editar los datos enviados. También hay una nueva sección destinada únicamente a aprobadores.

Para probar este flujo de trabajo en su sistema, siga los pasos que se mencionan a continuación:
* [Descargar e implementar DevelopingWitheServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Descargar e implementar el paquete OSGI personalizado SetValue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
* [Importar los recursos relacionados con este artículo a AEM](assets/helpxworkflow.zip)
* Abrir el formulario Solicitud de [tiempo de espera](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Rellene los detalles y envíe
* Open the [inbox](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html). Debería ver una nueva tarea asignada. Abra el formulario. Los datos del remitente deben ser de sólo lectura y debe estar visible una nueva sección del aprobador.
* Explorar el modelo de [flujo de trabajo](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)
* Explore el paso del proceso. Este es el paso que establece el valor de initialstep en No.
