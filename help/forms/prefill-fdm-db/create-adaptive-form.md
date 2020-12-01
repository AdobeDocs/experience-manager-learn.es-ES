---
title: Crear formulario adaptable
description: Crear y configurar formularios adaptables para utilizar el servicio de cumplimentación previa del modelo de datos de formulario
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5813
thumbnail: kt-5813.jpg
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 1%

---


# Crear formulario adaptable

Hasta ahora hemos creado lo siguiente

* Base de datos con 2 tablas: `newhire` y `beneficiaries`
* Origen de datos agrupados de conexión Apache Sling configurada
* Modelo de datos de formulario basado en RDBMS

El siguiente paso es crear y configurar un formulario adaptable para utilizar el modelo de datos de formulario.  Para obtener el inicio principal, puede [descargar e importar](assets/fdm-demo-af.zip) formulario de ejemplo. El formulario de ejemplo tiene una sección para mostrar los detalles del empleado y otra sección para la lista de los beneficiarios del empleado.

## Asociar formulario con modelo de datos de formulario

El formulario de ejemplo proporcionado con este curso no está asociado con ningún modelo de datos de formulario. Para configurar el formulario para que utilice el modelo de datos de formulario, debemos hacer lo siguiente:

* Seleccione el formulario FDMDemo
* Haga clic en _Propiedades_->_Modelo de formulario_
* Seleccione Modelo de datos de formulario en la lista desplegable
* Busque y seleccione el modelo de datos de formulario creado en la lección anterior.
* Haga clic en _Guardar y cerrar_

## Configurar el servicio Prefill

El primer paso es asociar el servicio de cumplimentación previa para el formulario. Para asociar el servicio de cumplimentación previa, siga los pasos mencionados a continuación

* Seleccione el formulario `FDMDemo`
* Haga clic en _Editar_ para abrir el formulario en modo de edición
* Seleccione Contenedor de formulario en la jerarquía de contenido y haga clic en el icono de la llave inglesa para abrir la hoja de propiedades
* Seleccione _Servicio de cumplimentación previa del modelo de datos de formulario_ en la lista desplegable Servicio de cumplimentación previa
* Haga clic en la anchura azul para guardar los cambios

* ![prefill-service](assets/fdm-prefill.png)

## Configurar detalles de empleado

El siguiente paso es enlazar los campos de texto del formulario adaptable a los elementos del modelo de datos de formulario. Tendrá que abrir la hoja de propiedades de los campos siguientes y establecer su bindRef como se muestra a continuación


| Nombre del campo | Vincular referencia |
|------------|--------------------|
| Nombre | /newhire/FirstName |
| Apellidos | /newhire/lastName |

>[!NOTE]
>
>Puede agregar campos de texto adicionales y enlazarlos a los elementos del modelo de datos de formulario correspondientes

## Tabla de configuración de beneficiarios

El siguiente paso es mostrar a los beneficiarios del empleado en forma de tabla. El formulario de ejemplo proporcionado tiene una tabla con 4 columnas y una sola fila. Necesitamos configurar la tabla para que crezca según el número de beneficiarios.

* Abra el formulario en modo de edición.
* Expandir panel raíz->Beneficiarios->Tabla
* Seleccione Fila1 y haga clic en el icono de la llave inglesa para abrir su hoja de propiedades.
* Establezca la referencia de enlace en **/newhire/GetEmployeeBeneficiaries**
* Configure Repetir configuración - Recuento mínimo en 1 y Recuento máximo en 5.
* La configuración de Fila1 debe tener el aspecto de la captura de pantalla siguiente
   ![row-configure](assets/configure-row.PNG)
* Haga clic en la plantilla azul para guardar los cambios

## Enlazar celdas de fila

Finalmente, es necesario enlazar las celdas de fila a los elementos del Modelo de datos de formulario.

* Expandir panel raíz->Sus beneficiarios->Tabla->Fila1
* Configure la Referencia de enlace de cada celda de fila según la tabla siguiente

| Celda de fila | Referencia de enlace |
|------------|----------------------------------------------|
| Nombre | /newhire/GetEmployeeBeneficiaries/firstname |
| Apellidos | /newhire/GetEmployeeBeneficiaries/lastname |
| Relación | /newhire/GetEmployeeBeneficiaries/relation |
| Porcentaje | /newhire/GetEmployeeBeneficiaries/percent |

* Haga clic en la plantilla azul para guardar los cambios

## Probar el formulario

Ahora necesitamos abrir el formulario con el empID adecuado en la dirección URL. Los dos vínculos siguientes rellenarán formularios con información de la base de datos
[Formulario con empID=207](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=207)
[Formulario con empID=208](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=208)

## Solución de problemas

Mi formulario está en blanco y no tiene ningún dato

* Asegúrese de que el modelo de datos de formulario devuelve los resultados correctos.
* El formulario está asociado al modelo de datos de formulario correcto
* Comprobación de los enlaces de campo
* Consulte el archivo de registro de stdout. Debería ver que empID se está escribiendo en el archivo.Si no ve este valor, es posible que el formulario no utilice la plantilla personalizada proporcionada.

La tabla no está rellenada

* Comprobar el enlace Fila1
* Asegúrese de que la configuración de repetición para Fila1 esté correctamente establecida (Mín. = 1 y Máx. = 5 o más)

