---
title: Uso de setvalue en el flujo de trabajo de AEM Forms
seo-title: Uso de setvalue en AEM Forms Workflow
description: Definir el valor del elemento en los datos enviados por Forms adaptable en AEM Forms OSGI
seo-description: Definir el valor del elemento en los datos enviados por Forms adaptable en AEM Forms OSGI
uuid: fe431e48-f05b-4b23-94d2-95d34d863984
feature: adaptive-forms,workflow
topics: developing
audience: implementer
doc-type: article
activity: setup
discoiquuid: dbd87302-f770-4e61-b5ad-3fc5831b4613
translation-type: tm+mt
source-git-commit: f07680e73316efb859a675f4b2212d8c3e03f6a0
workflow-type: tm+mt
source-wordcount: '547'
ht-degree: 0%

---


# Uso de setvalue en el flujo de trabajo de AEM Forms

Establecer el valor de un elemento XML en los datos enviados por Forms adaptable en el flujo de trabajo OSGI de AEM Forms.

![SetValue](assets/setvalue.png)

LiveCycle solía tener un componente de valor establecido que le permitía establecer el valor de un elemento XML.

En función de este valor, cuando el formulario se rellena con el código XML, puede ocultar o desactivar determinados campos o paneles del formulario.

En AEM Forms OSGI- tendremos que escribir un paquete OSGi personalizado para establecer el valor en XML. El paquete se proporciona como parte de este tutorial.
Utilizamos Paso de proceso en AEM flujo de trabajo. Asociamos el paquete OSGi &quot;Set Value of Element in XML&quot; con este paso del proceso.
Tenemos que pasar dos argumentos al paquete de valores establecido. El primer argumento es la XPath del elemento XML cuyo valor debe configurarse. El segundo argumento es el valor que debe configurarse.
Por ejemplo, en la captura de pantalla anterior, estamos configurando el valor del elemento intialstep en &quot;N&quot;.
En función de este valor, determinados paneles del Forms adaptable se ocultan o muestran.
En nuestro ejemplo, tenemos un formulario de solicitud de tiempo de espera sencillo. El iniciador de este formulario rellena su nombre y la hora de las fechas. En el envío, este formulario se envía a &quot;admin&quot; para su revisión. Cuando el administrador abre el formulario, los campos del primer panel se desactivan. Esto se debe a que hemos establecido el valor del elemento de paso inicial en XML en &quot;N&quot;.

En función del valor de los campos del paso inicial, se muestra el segundo panel donde el &quot;administrador&quot; puede aprobar o rechazar la solicitud

Observe las reglas establecidas en el campo &quot;Tiempo de espera solicitado por&quot; mediante el editor de reglas.

Para implementar los recursos en el sistema local, siga los pasos a continuación:

* [Implementar el paquete DevelopmentWithServiceUser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Implemente el paquete](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)de muestra. Este es el paquete OSGI personalizado que le permite establecer los valores de un elemento en los datos XML enviados

* [Descargar y extraer el contenido del archivo zip](assets/setvalueassets.zip)
* Seleccione el explorador para el administrador de [paquetes](http://localhost:4502/crx/packmgr/index.jsp)
* Importe e instale setValueWorkflow.zip. Tiene el modelo de flujo de trabajo de muestra.
* Apunta tu navegador a [Forms y Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Haga clic en Crear | Carga de archivos
* Cargar TimeOfRequestForm.zip
* Abrir el formulario [TimeOffRequest](http://localhost:4502/content/dam/formsanddocuments/timeoffapplication/jcr:content?wcmmode=disabled)
* Rellene los 3 campos obligatorios y envíe
* Inicie sesión como &#39;admin&quot; en AEM(si aún no lo ha hecho)
* Ir a [&quot;Bandeja de entrada AEM&quot;](http://localhost:4502/aem/inbox)
* Abrir el formulario &quot;Revisar el tiempo de inactividad de la solicitud&quot;
* Observe que los campos del primer panel están desactivados. Esto se debe a que el revisor está abriendo el formulario. Además, observe que el panel para aprobar o rechazar la solicitud ahora está visible

>[!NOTE]
>
>Para habilitar el registro de depuración, habilite el registrador para
>com.aemforms.setvalue.core.SetValueinXml
>señalando el explorador a http://localhost:4502/system/console/slinglog

>[!NOTE]
>
>Asegúrese de que la ruta del archivo de datos en las opciones de envío del formulario adaptable está configurada en &quot;Data.xml&quot;. Esto se debe a que el paso del proceso busca un archivo llamado Data.xml en la carpeta de carga útil
