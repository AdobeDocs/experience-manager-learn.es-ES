---
title: Uso de ldap con Aem Forms Workflow
seo-title: Uso de ldap con Aem Forms Workflow
description: Asignación de la tarea de flujo de trabajo de AEM Forms al administrador del remitente
seo-description: Asignación de la tarea de flujo de trabajo de AEM Forms al administrador del remitente
feature: adaptive-forms,workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
uuid: 3e32c3a7-387f-4652-8a94-4e6aa6cd5ab8
discoiquuid: 671872b3-3de0-40da-9691-f8b7e88a9443
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '543'
ht-degree: 0%

---


# Uso de LDAP con AEM Forms Workflow

Asignación de la tarea de flujo de trabajo de AEM Forms al administrador del remitente.

Cuando se utiliza Formulario adaptable en AEM flujo de trabajo, se desea asignar dinámicamente una tarea al administrador del remitente del formulario. Para lograr este caso de uso, tendremos que configurar AEM con Ldap.

Los pasos necesarios para configurar AEM con LDAP se explican en [detalle aquí.](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html)

A los efectos de este artículo, estoy adjuntando archivos de configuración utilizados para configurar AEM con Adobe Ldap. Estos archivos se incluyen en el paquete que se puede importar mediante el administrador de paquetes.

En la captura de pantalla de abajo, buscamos a todos los usuarios que pertenecen a un centro de costes en particular. Si desea recuperar todos los usuarios en su LDAP, puede que no utilice el filtro adicional.

![Configuración LDAP](assets/costcenterldap.gif)

En la siguiente captura de pantalla, asignamos los grupos a los usuarios recuperados de LDAP en AEM. Observe el grupo de usuarios de formularios asignado a los usuarios importados. El usuario debe ser miembro de este grupo para interactuar con AEM Forms. También almacenamos la propiedad manager bajo el nodo perfil/administrador en AEM.

![Synchandler](assets/synchandler.gif)

Una vez que haya configurado LDAP e importado usuarios en AEM, podemos crear un flujo de trabajo que asignará la tarea al administrador de los remitentes. A los efectos de este artículo, hemos desarrollado un sencillo flujo de trabajo de aprobación de un solo paso.

El primer paso en el flujo de trabajo establece el valor del primer paso en No. La regla comercial del formulario adaptable desactivará el panel &quot;Detalles del remitente&quot; y mostrará el panel &quot;Aprobado por&quot; en función del valor del paso inicial.

El segundo paso asigna la tarea al administrador del remitente. Obtenemos el administrador del remitente usando el código personalizado.

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

El fragmento de código es responsable de recuperar la identificación del administrador y asignar la tarea al administrador.

Obtenemos la persona que inició el flujo de trabajo. Luego obtenemos el valor de la propiedad manager.

Según cómo se almacene la propiedad manager en su LDAP, es posible que tenga que realizar alguna manipulación de cadenas para obtener la identificación del administrador.

Lea este artículo para implementar su propio [ selector de participantes.](https://helpx.adobe.com/experience-manager/using/dynamic-steps.html)

Para probar esto en el sistema (para empleados de Adobe puede utilizar este ejemplo de forma predeterminada)

* [Descargue e implemente el paquete](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) setvalue. Este es el paquete OSGI personalizado para establecer la propiedad del administrador.
* [Descargar e instalar DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Importe los recursos asociados a este artículo en AEM mediante el administrador](assets/aem-forms-ldap.zip) de paquetes. Como parte de este paquete se incluyen los archivos de configuración LDAP, el flujo de trabajo y un formulario adaptable.
* Configure AEM con su LDAP utilizando las credenciales LDAP apropiadas.
* Inicie sesión en AEM con sus credenciales LDAP.
* Abra [timeoffrequestform](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Rellene el formulario y envíelo.
* El administrador del remitente debe obtener el formulario para su revisión.

>[!NOTE]
>
>Este código personalizado para extraer el nombre del administrador se ha probado con Adobe LDAP. Si está ejecutando este código con un LDAP diferente, deberá modificar o escribir su propia implementación de getParticipant para obtener el nombre del administrador.
