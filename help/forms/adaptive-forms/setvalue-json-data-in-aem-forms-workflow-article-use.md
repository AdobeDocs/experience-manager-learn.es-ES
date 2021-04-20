---
title: Configuración del valor del elemento de datos Json en el flujo de trabajo de AEM Forms
seo-title: Configuración del valor del elemento de datos Json en el flujo de trabajo de AEM Forms
description: Como un formulario adaptable se dirige a diferentes usuarios en AEM Workflow, habrá requisitos para ocultar o deshabilitar ciertos campos o paneles en función de la persona que revise el formulario. Para satisfacer estos casos de uso, normalmente establecemos un valor de campo oculto. En función del valor de este campo oculto, se pueden crear reglas comerciales para ocultar o deshabilitar los paneles o campos adecuados.
seo-description: Como un formulario adaptable se dirige a diferentes usuarios en AEM Workflow, habrá requisitos para ocultar o deshabilitar ciertos campos o paneles en función de la persona que revise el formulario. Para satisfacer estos casos de uso, normalmente establecemos un valor de campo oculto. En función del valor de este campo oculto, se pueden crear reglas comerciales para ocultar o deshabilitar los paneles o campos adecuados.
uuid: a4ea6aef-a799-49e5-9682-3fa3b7a442fb
feature: Adaptive Forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.4
discoiquuid: 548fb2ec-cfcf-4fe2-a02a-14f267618d68
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '767'
ht-degree: 1%

---


# Configuración del valor del elemento de datos JSON en el flujo de trabajo de AEM Forms {#setting-value-of-json-data-element-in-aem-forms-workflow}

Como un formulario adaptable se dirige a diferentes usuarios en AEM Workflow, habrá requisitos para ocultar o deshabilitar ciertos campos o paneles en función de la persona que revise el formulario. Para satisfacer estos casos de uso, normalmente establecemos un valor de campo oculto. En función del valor de este campo oculto, se pueden crear reglas comerciales para ocultar o deshabilitar los paneles o campos adecuados.

![Configuración del valor de un elemento en los datos json](assets/capture-3.gif)

En AEM Forms OSGI- tendremos que escribir un paquete OSGi personalizado para establecer el valor del elemento de datos JSON. El paquete se proporciona como parte de este tutorial.

Se utiliza Paso de proceso en el flujo de trabajo de AEM. Asociamos el paquete OSGi &quot;Set Value of Element in Json&quot; con este paso del proceso.

Necesitamos pasar dos argumentos al paquete de valores establecido. El primer argumento es la ruta al elemento cuyo valor debe establecerse. El segundo argumento es el valor que debe configurarse.

Por ejemplo, en la captura de pantalla anterior, estamos configurando el valor del elemento intialStep en &quot;N&quot;

afData.afUnboundData.data.initialStep,N

En nuestro ejemplo, tenemos un sencillo Formulario de tiempo de espera. El iniciador de este formulario rellena su nombre y la hora de las fechas. En el envío, este formulario se envía a &quot;manager&quot; para su revisión. Cuando el administrador abre el formulario, los campos del primer panel se desactivan. Esto porque hemos establecido el valor del elemento de paso inicial en los datos JSON en N.

En función del valor de los campos del paso inicial, se muestra el panel de aprobación donde el &quot;administrador&quot; puede aprobar o rechazar la solicitud.

Por favor, eche un vistazo a las reglas establecidas contra &quot;Paso inicial&quot;. En función del valor del campo initialStep , recuperamos los detalles del usuario mediante el Modelo de datos de formulario, rellenamos los campos correspondientes y ocultamos o desactivamos los paneles adecuados.

Para implementar los recursos en el sistema local:

* [Descargar e implementar DevelopingWitheServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Descargue e implemente el paquete setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Este es el paquete OSGI personalizado que le permite establecer los valores de un elemento en los datos json enviados.

* [Descargue y extraiga el contenido del archivo zip](assets/set-value-jsondata.zip)
   * Apunte el navegador al [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
      * Importe e instale SetValueOfElementInJSONDataWorkflow.zip. Este paquete tiene asociado el modelo de flujo de trabajo y el modelo de datos de formulario de ejemplo.

* Apunte el navegador a [Forms and Documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Haga clic en Crear | Carga de archivo
* Cargar el archivo TimeOffRequestForm.zip
   **Este formulario se creó con AEM Forms 6.4. Asegúrese de que está en AEM Forms 6.4 o superior**
* Abra el [formulario](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled)
* Complete las Fechas de inicio y finalización y envíe el formulario.
* Vaya a [&quot;Inbox&quot;](http://localhost:4502/aem/inbox)
* Abra el formulario asociado a la tarea.
* Observe que los campos del primer panel están desactivados.
* Ahora está visible el panel para aprobar o rechazar la solicitud.

>[!NOTE]
>
>Dado que estamos rellenando previamente el formulario adaptable utilizando el perfil de usuario, asegúrese de que el administrador [información de perfil de usuario ](http://localhost:4502/security/users.html). Como mínimo, asegúrese de haber establecido los valores de los campos Nombre, Apellido y Correo electrónico .
>Puede habilitar el registro de depuración habilitando el registrador para com.aemforms.setvalue.core.SetValueInJson [desde aquí](http://localhost:4502/system/console/slinglog)

>[!NOTE]
>
>El paquete OSGi para establecer el valor de los elementos de datos en los datos JSON admite actualmente la capacidad de establecer un valor de elemento a la vez. Si desea establecer varios valores de elementos, deberá utilizar el paso de proceso varias veces.
>
>Asegúrese de que la ruta del archivo de datos en las opciones de envío del formulario adaptable esté configurada como &quot;Data.xml&quot;. Esto se debe a que el código del paso de proceso busca un archivo llamado Data.xml en la carpeta de carga útil.
