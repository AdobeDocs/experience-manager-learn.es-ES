---
title: Uso de LDAP con el flujo de trabajo de AEM Forms
description: Asignación de la tarea de flujo de trabajo de AEM Forms al administrador del remitente
feature: Forms adaptable, flujo de trabajo
topic: Integraciones
role: Developer
version: 6.3,6.4,6.5
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '532'
ht-degree: 0%

---


# Uso de LDAP con el flujo de trabajo de AEM Forms

Asignación de la tarea de flujo de trabajo de AEM Forms al administrador del remitente.

Al utilizar Formulario adaptable en AEM flujo de trabajo, le interesa asignar dinámicamente una tarea al administrador del remitente del formulario. Para lograr este caso de uso, tendremos que configurar AEM con Ldap.

Los pasos necesarios para configurar AEM con LDAP se explican en [detalle aquí.](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html)

A los efectos de este artículo, adjunto archivos de configuración utilizados en la configuración de AEM con Adobe Ldap. Estos archivos se incluyen en el paquete que se puede importar mediante el administrador de paquetes.

En la captura de pantalla siguiente, estamos recuperando todos los usuarios que pertenecen a un centro de costes en particular. Si desea recuperar todos los usuarios en su LDAP, no puede utilizar el filtro adicional.

![Configuración LDAP](assets/costcenterldap.gif)

En la captura de pantalla siguiente, asignamos los grupos a los usuarios recuperados de LDAP a AEM. Observe el grupo de usuarios de formularios asignado a los usuarios importados. El usuario debe ser miembro de este grupo para interactuar con AEM Forms. También almacenamos la propiedad manager en el nodo profile/manager en AEM.

![Synchandler](assets/synchandler.gif)

Una vez que haya configurado LDAP e importado usuarios en AEM, podemos crear un flujo de trabajo que asigne la tarea al administrador de los remitentes. A los efectos de este artículo, hemos desarrollado un sencillo flujo de trabajo de aprobación de un solo paso.

El primer paso del flujo de trabajo establece el valor del paso inicial en No. La regla de negocio del formulario adaptable desactivará el panel &quot;Detalles del remitente&quot; y mostrará el panel &quot;Aprobado por&quot; en función del valor del paso inicial.

El segundo paso asigna la tarea al administrador del remitente. Obtenemos el administrador del remitente utilizando el código personalizado.

![Asignar tarea](assets/assigntask.gif)

```java
public String getParticipant(WorkItem workItem, WorkflowSession wfSession, MetaDataMap arg2) throws WorkflowException{
resourceResolver = wfSession.adaptTo(ResourceResolver.class);
UserManager userManager = resourceResolver.adaptTo(UserManager.class);
Authorizable workflowInitiator = userManager.getAuthorizable(workItem.getWorkflow().getInitiator());
.
.
String managerPorperty = workflowInitiator.getProperty("profile/manager")[0].getString();
.
.

}
```

El fragmento de código es responsable de recuperar el id de administrador y asignar la tarea al administrador.

Obtenemos la persona que inició el flujo de trabajo. Luego obtenemos el valor de la propiedad manager.

Dependiendo de cómo se almacene la propiedad manager en su LDAP, es posible que tenga que realizar alguna manipulación de cadenas para obtener el id del administrador.

Lea este artículo para implementar su propio [ ParticipantChooser .](https://helpx.adobe.com/experience-manager/using/dynamic-steps.html)

Para probar esto en su sistema (para empleados de Adobe, puede usar este ejemplo de forma predeterminada)

* [Descargue e implemente el paquete setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Este es el paquete OSGI personalizado para configurar la propiedad del administrador.
* [Descargar e instalar DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Importe los recursos asociados a este artículo en AEM mediante el administrador de paquetes](assets/aem-forms-ldap.zip). Incluidos como parte de este paquete son archivos de configuración LDAP, flujo de trabajo y un formulario adaptable.
* Configure AEM con su LDAP utilizando las credenciales LDAP adecuadas.
* Inicie sesión en AEM con sus credenciales LDAP.
* Abra [timeoffrequestform](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Complete el formulario y envíe.
* El administrador del remitente debe obtener el formulario para su revisión.

>[!NOTE]
>
>Este código personalizado para extraer el nombre del administrador se ha probado con el Adobe LDAP. Si está ejecutando este código contra un LDAP diferente, deberá modificar o escribir su propia implementación de getParticipant para obtener el nombre del administrador.
