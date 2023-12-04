---
title: Uso de LDAP con AEM Forms Workflow
description: Asignación de la tarea de flujo de trabajo de AEM Forms al responsable del emisor
feature: Adaptive Forms, Workflow
topic: Integrations
role: Developer
version: 6.4,6.5
level: Intermediate
exl-id: 2e9754ff-49fe-4260-b911-796bcc4fd266
last-substantial-update: 2021-09-18T00:00:00Z
duration: 149
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 0%

---

# Uso de LDAP con AEM Forms Workflow

Asignar una tarea de flujo de trabajo de AEM Forms al administrador del emisor.

AEM Cuando se utiliza un formulario adaptable en flujo de trabajo, es posible que desee asignar dinámicamente una tarea al administrador del remitente del formulario. AEM Para llevar a cabo este caso de uso, tendremos que configurar el uso de la función de forma que se pueda configurar con.

AEM Los pasos necesarios para configurar la con LDAP se explican en [detallar aquí.](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html)

A los efectos de este artículo, adjunto archivos de configuración utilizados en la configuración de los archivos de configuración con Adobe Ldap de la configuración de la aplicación de la configuración de la aplicación de la configuración de la aplicación de AEM Ldap. Estos archivos se incluyen en el paquete, que se puede importar mediante el administrador de paquetes.

En la captura de pantalla siguiente, se recuperan todos los usuarios que pertenecen a un centro de coste determinado. Si desea recuperar todos los usuarios de su LDAP, no puede utilizar el filtro adicional.

![Configuración de LDAP](assets/costcenterldap.gif)

AEM En la captura de pantalla siguiente, asignamos los grupos a los usuarios recuperados de LDAP en el. Observe el grupo forms-users asignado a los usuarios importados. El usuario debe ser miembro de este grupo para interactuar con AEM Forms. AEM También almacenamos la propiedad manager en el nodo profile/manager en la dirección de correo electrónico de.

![Synchandler](assets/synchandler.gif)

AEM Una vez que haya configurado LDAP y haya importado usuarios en el servidor de correo electrónico, podemos crear un flujo de trabajo que asigne la tarea al administrador de los remitentes. Para los fines de este artículo, hemos desarrollado un flujo de trabajo de aprobación simple de un paso.

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

Lea este artículo para implementar los suyos propios [Selector de participantes](https://helpx.adobe.com/experience-manager/using/dynamic-steps.html)

Para probar esto en su sistema (para empleados de Adobe puede usar este ejemplo de forma predeterminada)

* [Descargue e implemente el paquete setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Este es el paquete OSGI personalizado para establecer la propiedad del administrador.
* [Descargar e instalar el paquete Desarrollando con servicio](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [AEM Importe los recursos asociados con este artículo a mediante el administrador de paquetes de la interfaz de usuario de](assets/aem-forms-ldap.zip).Incluidos como parte de este paquete están los archivos de configuración LDAP, el flujo de trabajo y un formulario adaptable.
* AEM Configure con el LDAP utilizando las credenciales de LDAP adecuadas.
* AEM Inicie sesión en el servicio de acceso a datos usando sus credenciales de LDAP.
* Abra el [timeoffrequestform](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Complete el formulario y envíelo.
* El administrador del remitente debe obtener el formulario para su revisión.

>[!NOTE]
>
>Este código personalizado para extraer el nombre del responsable se ha probado con el LDAP de Adobe. Si está ejecutando este código en un LDAP diferente, tendrá que modificar o escribir su propia implementación de getParticipant para obtener el nombre del responsable.
