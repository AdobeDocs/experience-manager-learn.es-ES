---
title: Configuración del valor del elemento de datos Json en el flujo de trabajo de AEM Forms
seo-title: Configuración del valor del elemento de datos Json en el flujo de trabajo de AEM Forms
description: Como un formulario adaptable se distribuye a distintos usuarios en AEM flujo de trabajo, habrá requisitos para ocultar o deshabilitar determinados campos o paneles según la persona que revise el formulario. Para satisfacer estos casos de uso, generalmente establecemos un valor de un campo oculto. En función del valor de este campo oculto, se pueden crear reglas comerciales para ocultar o deshabilitar los paneles o campos adecuados.
seo-description: Como un formulario adaptable se distribuye a distintos usuarios en AEM flujo de trabajo, habrá requisitos para ocultar o deshabilitar determinados campos o paneles según la persona que revise el formulario. Para satisfacer estos casos de uso, generalmente establecemos un valor de un campo oculto. En función del valor de este campo oculto, se pueden crear reglas comerciales para ocultar o deshabilitar los paneles o campos adecuados.
uuid: a4ea6aef-a799-49e5-9682-3fa3b7a442fb
feature: adaptive-forms,workflow
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.4
discoiquuid: 548fb2ec-cfcf-4fe2-a02a-14f267618d68
translation-type: tm+mt
source-git-commit: 233ad7184cb48098253a78c07a3913356ac9e774
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 0%

---


# Configuración del valor del elemento de datos JSON en el flujo de trabajo de AEM Forms {#setting-value-of-json-data-element-in-aem-forms-workflow}

Como un formulario adaptable se distribuye a distintos usuarios en AEM flujo de trabajo, habrá requisitos para ocultar o deshabilitar determinados campos o paneles según la persona que revise el formulario. Para satisfacer estos casos de uso, generalmente establecemos un valor de un campo oculto. En función del valor de este campo oculto, se pueden crear reglas comerciales para ocultar o deshabilitar los paneles o campos adecuados.

![Definición del valor de un elemento en los datos json](assets/capture-3.gif)

En AEM Forms OSGI- tendremos que escribir un paquete OSGi personalizado para establecer el valor del elemento de datos JSON. El paquete se proporciona como parte de este tutorial.

Utilizamos Paso de proceso en AEM flujo de trabajo. Asociamos el paquete OSGi &quot;Set Value of Element in Json&quot; con este paso del proceso.

Tenemos que pasar dos argumentos al paquete de valores establecido. El primer argumento es la ruta al elemento cuyo valor debe establecerse. El segundo argumento es el valor que debe configurarse.

Por ejemplo, en la captura de pantalla anterior, estamos estableciendo el valor del elemento intialStep en &quot;N&quot;

afData.afUnboundData.data.initialStep,N

En nuestro ejemplo, tenemos un formulario de solicitud de tiempo de espera sencillo. El iniciador de este formulario rellena su nombre y la hora de las fechas. En el momento del envío, este formulario se envía a &quot;manager&quot; para su revisión. Cuando el administrador abre el formulario, los campos del primer panel se desactivan. Esto se debe a que hemos establecido el valor del elemento de paso inicial en los datos JSON en N.

En función del valor de los campos de paso iniciales, se muestra el panel de aprobación donde el &quot;administrador&quot; puede aprobar o rechazar la solicitud.

Por favor, eche un vistazo a las reglas establecidas con &quot;Paso inicial&quot;. En función del valor del campo initialStep, se recogen los detalles del usuario mediante el Modelo de datos de formulario y se rellenan los campos correspondientes y se ocultan o deshabilitan los paneles adecuados.

Para implementar los recursos en el sistema local:

* [Descargar e implementar DevelopingWitheServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Descargue e implemente el paquete](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) setvalue. Este es el paquete OSGI personalizado que le permite establecer los valores de un elemento en los datos json enviados.

* [Descargar y extraer el contenido del archivo zip](assets/set-value-jsondata.zip)
   * Apunta a tu explorador para [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
      * Importe e instale SetValueOfElementInJSONDataWorkflow.zip. Este paquete tiene el modelo de flujo de trabajo de ejemplo y el modelo de datos de formulario asociados al formulario.

* Apunta a tu explorador para [Forms y Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Haga clic en Crear | Carga de archivos
* Cargar archivo TimeOffRequestForm.zip
   **Este formulario se creó con AEM Forms 6.4. Asegúrese de que está en AEM Forms 6.4 o superior**
* Abra el [formulario](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled)
* Rellene las fechas de Inicio y finalización y envíe el formulario.
* Ir a [&quot;Bandeja de entrada&quot;](http://localhost:4502/aem/inbox)
* Abra el formulario asociado a la tarea.
* Observe que los campos del primer panel están desactivados.
* Observe que el panel para aprobar o rechazar la solicitud ahora está visible.

>[!NOTE]
>
>Dado que se está rellenando previamente el formulario adaptable con el perfil del usuario, asegúrese de que la información de perfil del usuario [administrador ](http://localhost:4502/security/users.html). Como mínimo, asegúrese de haber establecido los valores de los campos Nombre, Apellido y Correo electrónico.
>Puede habilitar el registro de depuración habilitando el registrador para com.aemforms.setvalue.core.SetValueInJson [desde aquí](http://localhost:4502/system/console/slinglog)

>[!NOTE]
>
>El paquete OSGi para establecer el valor de los elementos de datos en los datos JSON admite actualmente la posibilidad de establecer un valor de elemento a la vez. Si desea establecer varios valores de elementos, deberá utilizar el paso de proceso varias veces.
>
>Asegúrese de que la ruta del archivo de datos en las opciones de envío del formulario adaptable está configurada en &quot;Data.xml&quot;. Esto se debe a que el código del paso del proceso busca un archivo llamado Data.xml en la carpeta de carga útil.
