---
title: Uso de setvalue en el flujo de trabajo de AEM Forms
description: Establecer el valor del elemento en los datos enviados por Forms adaptable en AEM Forms OSGI
feature: Formularios adaptables
topic: Desarrollo
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '531'
ht-degree: 1%

---


# Uso de setvalue en el flujo de trabajo de AEM Forms

Establezca el valor de un elemento XML en los datos enviados por Forms adaptable en el flujo de trabajo OSGI de AEM Forms.

![SetValue](assets/setvalue.png)

LiveCycle utilizado para tener un componente de valor establecido que le permitiría establecer el valor de un elemento XML.

En función de este valor, cuando el formulario se rellena con el XML, puede ocultar o desactivar determinados campos o paneles del formulario.

En AEM Forms OSGI- tendremos que escribir un paquete OSGi personalizado para establecer el valor en el XML. El paquete se proporciona como parte de este tutorial.
En AEM flujo de trabajo se utiliza Paso de proceso. Asociamos el paquete OSGi &quot;Set Value of Element in XML&quot; con este paso del proceso.
Necesitamos pasar dos argumentos al paquete de valores establecido. El primer argumento es la XPath del elemento XML cuyo valor debe establecerse. El segundo argumento es el valor que debe configurarse.
Por ejemplo, en la captura de pantalla anterior, estamos configurando el valor del elemento del paso inicial en &quot;N&quot;.
En función de este valor, ciertos paneles de la Forms adaptable se ocultan o muestran.
En nuestro ejemplo, tenemos un sencillo Formulario de tiempo de espera. El iniciador de este formulario rellena su nombre y la hora de las fechas. En el envío, este formulario se dirige a &quot;admin&quot; para su revisión. Cuando el administrador abre el formulario, los campos del primer panel se desactivan. Esto porque hemos establecido el valor del elemento de paso inicial en XML en &quot;N&quot;.

Basándonos en el valor de los campos del paso inicial, mostramos el segundo panel donde el &quot;administrador&quot; puede aprobar o rechazar la solicitud

Eche un vistazo a las reglas establecidas para el campo &quot;Tiempo de espera solicitado por&quot; con el editor de reglas.

Para implementar los recursos en el sistema local, siga los pasos a continuación:

* [Implementar el paquete de usuario Desarrollo con servicio](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Implemente el paquete de muestra](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Este es el paquete OSGI personalizado que le permite establecer los valores de un elemento en los datos xml enviados

* [Descargue y extraiga el contenido del archivo zip](assets/setvalueassets.zip)
* Apunte el navegador al [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Importe e instale setValueWorkflow.zip. Este tiene el modelo de flujo de trabajo de ejemplo.
* Apunte el navegador a [Forms y Documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Haga clic en Crear | Carga de archivo
* Cargue TimeOfRequestForm.zip
* Abra [TimeOffRequestform](http://localhost:4502/content/dam/formsanddocuments/timeoffapplication/jcr:content?wcmmode=disabled)
* Rellene los 3 campos obligatorios y envíe
* Inicie sesión como &quot;admin&quot; en AEM(si aún no lo ha hecho)
* Vaya a [&quot;AEM bandeja de entrada&quot;](http://localhost:4502/aem/inbox)
* Abra el formulario &quot;Solicitud de tiempo de espera de revisión&quot;.
* Observe que los campos del primer panel están desactivados. Esto se debe a que el revisor está abriendo el formulario. Además, observe que el panel para aprobar o rechazar la solicitud ya está visible

>[!NOTE]
>
>Puede habilitar el registro de depuración habilitando el registrador para
>com.aemforms.setvalue.core.SetValueinXml
>señalando su navegador a http://localhost:4502/system/console/slinglog

>[!NOTE]
>
>Asegúrese de que la ruta del archivo de datos en las opciones de envío del formulario adaptable esté configurada como &quot;Data.xml&quot;. Esto se debe a que el paso de proceso busca un archivo llamado Data.xml en la carpeta de carga útil
