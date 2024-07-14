---
title: Configurar el valor del elemento de datos Json en el flujo de trabajo de AEM Forms
description: AEM Dado que un formulario adaptable se enruta a diferentes usuarios en el flujo de trabajo de, existen requisitos para ocultar o deshabilitar determinados campos o paneles en función de la persona que revisa el formulario. Para satisfacer estos casos de uso, normalmente se establece el valor de un campo oculto. En función del valor de este campo oculto, se pueden crear reglas empresariales para ocultar o deshabilitar los paneles o campos adecuados.
feature: Adaptive Forms
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: fbe6d341-7941-46f5-bcd8-58b99396d351
last-substantial-update: 2021-06-09T00:00:00Z
duration: 126
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '656'
ht-degree: 0%

---

# Configurar el valor del elemento de datos JSON en el flujo de trabajo de AEM Forms {#setting-value-of-json-data-element-in-aem-forms-workflow}

AEM Dado que un formulario adaptable se enruta a diferentes usuarios en el flujo de trabajo de, existen requisitos para ocultar o deshabilitar determinados campos o paneles en función de la persona que revisa el formulario. Para satisfacer estos casos de uso, normalmente se establece el valor de un campo oculto. En función del valor de este campo oculto, se pueden crear reglas empresariales para ocultar o deshabilitar los paneles o campos adecuados.

![Estableciendo valor de un elemento en los datos json](assets/capture-3.gif)

En AEM Forms OSGi: debemos crear un paquete OSGi personalizado para establecer el valor del elemento de datos JSON. El paquete se proporciona como parte de este tutorial.

AEM Utilizamos el paso Proceso en el flujo de trabajo de la. Asociamos el paquete OSGi &quot;Set Value of Element in Json&quot; con este paso del proceso.

Necesitamos pasar dos argumentos al paquete de valor establecido. El primer argumento es la ruta al elemento cuyo valor debe establecerse. El segundo argumento es el valor que debe establecerse.

Por ejemplo, en la captura de pantalla anterior, estamos configurando el valor del elemento intialStep en &quot;N&quot;

afData.afUnboundData.data.initialStep,N

En nuestro ejemplo, tenemos un formulario de solicitud de tiempo libre simple. El iniciador de este formulario rellena su nombre y las fechas de descanso. Al realizar el envío, este formulario se envía al &quot;responsable&quot; para que lo revise. Cuando el administrador abre el formulario, los campos del primer panel están desactivados. Esto se debe a que hemos establecido el valor del elemento de paso inicial en los datos JSON en N.

En función del valor de los campos de paso inicial, se muestra el panel del aprobador, donde el &quot;responsable&quot; puede aprobar o rechazar la solicitud.

Consulte las reglas establecidas para &quot;Paso inicial&quot;. En función del valor del campo initialStep, recuperamos los detalles del usuario mediante el modelo de datos de formulario, rellenamos los campos adecuados y ocultamos/deshabilitamos los paneles correspondientes.

Para implementar los recursos en el sistema local:

* [Descargar e implementar el paquete DevelopersWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Descargue e implemente el paquete setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Este es el paquete OSGI personalizado que le permite establecer los valores de un elemento en los datos json enviados.

* [Descargue y extraiga el contenido del archivo zip](assets/set-value-jsondata.zip)
   * Dirija su navegador a [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
      * Importe e instale SetValueOfElementInJSONDataWorkflow.zip. Este paquete tiene el modelo de flujo de trabajo de ejemplo y el modelo de datos de formulario asociados al formulario.

* Dirija su navegador a [Forms y documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Haga clic en Crear | Carga de archivos
* Cargar archivo TimeOffRequestForm.zip
  **Este formulario se creó con AEM Forms 6.4. Asegúrese de usar AEM Forms 6.4 o superior**
* Abrir [formulario](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled)
* Complete las Fechas de inicio y finalización y envíe el formulario.
* Ir a [&quot;Bandeja de entrada&quot;](http://localhost:4502/aem/inbox)
* Abra el formulario asociado a la tarea.
* Observe que los campos del primer panel están deshabilitados.
* Observe que el panel para aprobar o rechazar la solicitud ahora está visible.

>[!NOTE]
>
>Dado que estamos rellenando previamente el formulario adaptable mediante el perfil de usuario, asegúrese de proporcionar al administrador [información de perfil de usuario](http://localhost:4502/security/users.html). Como mínimo, asegúrese de haber establecido los valores de los campos Nombre, Apellidos y Correo electrónico.
>Puede habilitar el registro de depuración habilitando el registrador para com.aemforms.setvalue.core.SetValueInJson [desde aquí](http://localhost:4502/system/console/slinglog)

>[!NOTE]
>
>El paquete OSGi para establecer el valor de los elementos de datos en datos JSON admite actualmente la capacidad de establecer un valor de elemento a la vez. Si desea establecer varios valores de elemento, deberá utilizar el paso Procesar varias veces.
>
>Asegúrese de que la ruta del archivo de datos en las opciones de envío del formulario adaptable esté establecida en &quot;Data.xml&quot;. Esto se debe a que el código del paso del proceso busca un archivo llamado Data.xml en la carpeta de carga útil.
