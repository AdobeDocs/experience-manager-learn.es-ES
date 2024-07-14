---
title: Flujo de trabajo de solicitud de tiempo libre pago simple
description: AEM Ocultar y mostrar paneles de formularios adaptables en flujo de trabajo de la
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
discoiquuid: 1c4822e6-76ce-446b-98cb-408900d68b24
topic: Development
role: Developer
level: Experienced
exl-id: 9342bd2f-2ba9-42ee-9224-055649ac3c90
last-substantial-update: 2020-07-07T00:00:00Z
duration: 592
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '305'
ht-degree: 0%

---

# Flujo de trabajo de solicitud de tiempo libre pago simple

En este artículo, analizamos un flujo de trabajo sencillo utilizado para solicitar tiempo libre pagado. Los requisitos comerciales son los siguientes:

* El usuario A solicita tiempo libre rellenando un formulario adaptable.
* AEM El formulario se enruta a usuario administrador (en la vida real, se enruta al administrador del remitente).
* El administrador abre el formulario. El administrador no debe poder editar la información que haya rellenado el remitente.
* AEM La sección Aprobador debe ser visible para el aprobador (en este caso es el usuario administrador).

Para cumplir el requisito anterior, se utiliza un campo oculto denominado **initialstep** en el formulario y su valor predeterminado se establece en Yes.When el formulario se envía, el primer paso del flujo de trabajo establece el valor de initialstep en No. El formulario tiene reglas empresariales para ocultar y mostrar las secciones adecuadas en función del valor del paso inicial.

**Configurar formulario para Déclencheur AEM el flujo de trabajo de la**

>[!VIDEO](https://video.tv.adobe.com/v/28406?quality=12&learn=on)

**Recorrido del flujo de trabajo**

>[!VIDEO](https://video.tv.adobe.com/v/28407?quality=12&learn=on)

**Vista del remitente del formulario de solicitud de tiempo libre**

![inicialstep](assets/initialstep.gif)

**Vista del aprobador del formulario**

![aprobador](assets/approversview.gif)

En la vista del aprobador, el aprobador no puede editar los datos enviados. También hay una nueva sección destinada únicamente a aprobadores.

Para probar este flujo de trabajo en su sistema, siga los pasos que se mencionan a continuación:
* [Descargar e implementar el paquete DevelopersWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Descargar e implementar el paquete OSGI personalizado SetValue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
* AEM [Importe los recursos relacionados con este artículo en el sitio de trabajo {1000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000](assets/helpxworkflow.zip)
* Abrir el [formulario de solicitud de tiempo libre](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Complete los detalles y envíe
* Abra la [bandeja de entrada](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html). Debería ver una nueva tarea asignada. Abra el formulario. Los datos del remitente deben ser de solo lectura y debe estar visible una nueva sección de aprobadores.
* Explorar el [modelo de flujo de trabajo](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)
* Explore el paso del proceso. Este es el paso que establece el valor de initialstep en No.
