---
title: Uso de LDAP con AEM Forms Workflow
description: Asignación de la tarea de flujo de trabajo de AEM Forms al responsable del emisor
feature: Adaptive Forms, Workflow
topic: Integrations
role: Developer
version: Experience Manager 6.4, Experience Manager 6.5
level: Intermediate
exl-id: 2e9754ff-49fe-4260-b911-796bcc4fd266
last-substantial-update: 2021-09-18T00:00:00Z
duration: 111
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 0%

---

# Uso de LDAP con AEM Forms Workflow

Asignar una tarea de flujo de trabajo de AEM Forms al administrador del emisor.

Cuando se utiliza un formulario adaptable en el flujo de trabajo de AEM, es posible que desee asignar dinámicamente una tarea al administrador del remitente del formulario. Para aplicar este caso de uso, tendremos que configurar AEM con LDAP.

Los pasos necesarios para configurar AEM con LDAP se explican en [detalle aquí.](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html)

A los efectos de este artículo, adjunto archivos de configuración utilizados en la configuración de AEM con Adobe Ldap. Estos archivos se incluyen en el paquete, que se puede importar mediante el administrador de paquetes.

En la captura de pantalla siguiente, se recuperan todos los usuarios que pertenecen a un centro de coste determinado. Si desea recuperar todos los usuarios de su LDAP, no puede utilizar el filtro adicional.

![Configuración LDAP](assets/costcenterldap.gif)

En la captura de pantalla siguiente, asignamos los grupos a los usuarios recuperados de LDAP en AEM. Observe el grupo forms-users asignado a los usuarios importados. El usuario debe ser miembro de este grupo para interactuar con AEM Forms. También almacenamos la propiedad manager en el nodo profile/manager en AEM.

![Sincronizador](assets/synchandler.gif)

Una vez que haya configurado LDAP y haya importado a los usuarios a AEM, podemos crear un flujo de trabajo que asignará la tarea al administrador de los remitentes. Para los fines de este artículo, hemos desarrollado un flujo de trabajo de aprobación simple de un paso.

El primer paso del flujo de trabajo establece el valor de initialstep en No. La regla de negocio del formulario adaptable deshabilitará el panel &quot;Detalles del remitente&quot; y mostrará el panel &quot;Aprobado por&quot; en función del valor del paso inicial.

El segundo paso asigna la tarea al administrador del emisor. Obtenemos al gerente del remitente usando el código personalizado.

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

El fragmento de código es responsable de recuperar el ID de los administradores y asignar la tarea al administrador.

Obtenemos la persona que inició el flujo de trabajo. Luego obtenemos el valor de la propiedad del administrador.

Según la forma en que se almacene la propiedad del administrador en el LDAP, es posible que tenga que realizar alguna manipulación de cadenas para obtener el ID del administrador.

Lea este artículo para implementar su propio [SelectorDeParticipantes .](https://helpx.adobe.com/experience-manager/using/dynamic-steps.html)

Para probar esto en el sistema (para empleados de Adobe puede usar este ejemplo de forma predeterminada)

* [Descargue e implemente el paquete setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Este es el paquete OSGI personalizado para establecer la propiedad del administrador.
* [Descargar e instalar el paquete Desarrollando con servicio](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Importe el Assets asociado con este artículo en AEM mediante el administrador de paquetes](assets/aem-forms-ldap.zip). Se incluyen como parte de este paquete los archivos de configuración LDAP, el flujo de trabajo y un formulario adaptable.
* Configure AEM con su LDAP utilizando las credenciales de LDAP apropiadas.
* Inicie sesión en AEM con sus credenciales de LDAP.
* Abrir [timeoffrequestform](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Complete el formulario y envíelo.
* El administrador del remitente debe obtener el formulario para su revisión.

>[!NOTE]
>
>Este código personalizado para extraer el nombre del responsable se ha probado con el LDAP de Adobe. Si está ejecutando este código en un LDAP diferente, tendrá que modificar o escribir su propia implementación de getParticipant para obtener el nombre del responsable.
