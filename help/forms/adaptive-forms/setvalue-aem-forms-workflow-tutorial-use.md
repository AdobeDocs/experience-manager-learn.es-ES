---
title: Uso de setvalue en el flujo de trabajo de AEM Forms
description: Establecer el valor del elemento en los datos enviados por el Forms adaptable en el OSGI de AEM Forms
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 3919efee-6998-48e8-85d7-91b6943d23f9
last-substantial-update: 2020-01-09T00:00:00Z
duration: 134
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '503'
ht-degree: 0%

---

# Uso de setvalue en el flujo de trabajo de AEM Forms

Establezca el valor de un elemento XML en los datos enviados por Forms adaptable en el flujo de trabajo OSGI de AEM Forms.

![SetValue](assets/setvalue.png)

LiveCycle utilizado para tener un componente de valor establecido que le permitiría establecer el valor de un elemento XML.

En función de este valor, cuando el formulario se rellena con el XML, puede ocultar o deshabilitar determinados campos o paneles del formulario.

En AEM Forms OSGI: tendremos que escribir un paquete OSGi personalizado para establecer el valor en el XML. El paquete se proporciona como parte de este tutorial.
AEM Utilizamos el paso Proceso en el flujo de trabajo de la. Asociamos el paquete OSGi &quot;Set Value of Element in XML&quot; con este paso del proceso.
Necesitamos pasar dos argumentos al paquete de valor establecido. El primer argumento es la XPath del elemento XML cuyo valor debe establecerse. El segundo argumento es el valor que debe establecerse.
Por ejemplo, en la captura de pantalla anterior, estamos configurando el valor del elemento intialstep en &quot;N&quot;.
En función de este valor, ciertos paneles de la Forms adaptable se ocultan o muestran.
En nuestro ejemplo, tenemos un formulario de solicitud de tiempo libre simple. El iniciador de este formulario rellena su nombre y las fechas de descanso. Al enviar, este formulario va a &quot;admin&quot; para su revisión. Cuando el administrador abre el formulario, los campos del primer panel están desactivados. Esto se debe a que hemos establecido el valor del elemento de paso inicial en el XML en &quot;N&quot;.

En función del valor de los campos de paso inicial, se muestra el segundo panel en el que el &quot;administrador&quot; puede aprobar o rechazar la solicitud

Eche un vistazo a las reglas configuradas en el campo &quot;Tiempo libre solicitado por&quot; con el editor de reglas.

Para implementar los recursos en el sistema local, siga los pasos a continuación:

* [Implementar el paquete Develingwithserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Implementar el paquete de muestra](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Este es el paquete OSGI personalizado que le permite establecer los valores de un elemento en los datos xml enviados

* [Descargue y extraiga el contenido del archivo zip](assets/setvalueassets.zip)
* Dirija el explorador a [administrador de paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Importe e instale el archivo setValueWorkflow.zip. Tiene el modelo de flujo de trabajo de ejemplo.
* Dirija el explorador a [Forms y documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Haga clic en Crear | Carga de archivos
* Cargue TimeOfRequestForm.zip
* Abra el [TimeOffRequestForm](http://localhost:4502/content/dam/formsanddocuments/timeoffapplication/jcr:content?wcmmode=disabled)
* Rellene los 3 campos obligatorios y realice el envío
* AEM Inicie sesión como &quot;admin&quot; en para el (si aún no lo ha hecho).
* Ir a [AEM &quot;Bandeja de entrada de&quot;](http://localhost:4502/aem/inbox)
* Abra el formulario &quot;Solicitud de tiempo libre de revisión&quot;
* Observe que los campos del primer panel están deshabilitados. Esto se debe a que el revisor está abriendo el formulario. Observe que el panel para aprobar o rechazar la solicitud ahora está visible

>[!NOTE]
>
>Puede habilitar el registro de depuración habilitando el registrador para
>com.aemforms.setvalue.core.SetValueinXml
>al dirigir el explorador a http://localhost:4502/system/console/slinglog

>[!NOTE]
>
>Asegúrese de que la ruta del archivo de datos en las opciones de envío del formulario adaptable esté establecida en &quot;Data.xml&quot;. Esto se debe a que el paso del proceso busca un archivo llamado Data.xml en la carpeta de carga útil
