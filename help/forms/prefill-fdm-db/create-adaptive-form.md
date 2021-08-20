---
title: Crear formulario adaptable
description: Cree y configure formularios adaptables para utilizar el servicio de cumplimentación previa del modelo de datos de formulario
feature: Formularios adaptables
version: 6.4,6.5
kt: 5813
thumbnail: kt-5813.jpg
topic: Desarrollo
role: User
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '609'
ht-degree: 2%

---


# Crear formulario adaptable

Hasta ahora hemos creado lo siguiente

* Base de datos con 2 tablas: `newhire` y `beneficiaries`
* Fuente de datos obtenida de una conexión Apache Sling configurada
* Modelo de datos de formulario basado en RDBMS

El siguiente paso es crear y configurar un formulario adaptable para utilizar el modelo de datos de formulario.  Para obtener el inicio inicial, puede [descargar e importar](assets/fdm-demo-af.zip) formulario de ejemplo. El formulario de ejemplo tiene una sección para mostrar los detalles del empleado y otra sección para enumerar los beneficiarios del empleado.

## Asociación del formulario con el modelo de datos del formulario

El formulario de ejemplo proporcionado con este curso no está asociado con ningún modelo de datos de formulario. Para configurar el formulario para que utilice el modelo de datos de formulario, debemos hacer lo siguiente:

* Seleccione el formulario FDMDemo
* Haga clic en _Propiedades_->_Modelo de formulario_
* Seleccione Modelo de datos de formulario en la lista desplegable
* Busque y seleccione el Modelo de datos de formulario creado en la lección anterior.
* Haga clic en _Guardar y cerrar_

## Configurar el servicio de prerelleno

El primer paso es asociar el servicio de rellenado previo para el formulario. Para asociar el servicio de precarga, siga los pasos que se mencionan a continuación

* Seleccione el formulario `FDMDemo`
* Haga clic en _Editar_ para abrir el formulario en modo de edición
* Seleccione Contenedor de formulario en la jerarquía de contenido y haga clic en el icono de la llave inglesa para abrir su hoja de propiedades
* Seleccione _Servicio de relleno previo del modelo de datos de formulario_ en la lista desplegable Servicio de relleno previo
* Haga clic en la anchura azul para guardar los cambios

* ![prefill-service](assets/fdm-prefill.png)

## Configurar detalles del empleado

El siguiente paso es enlazar los campos de texto del formulario adaptable a los elementos del modelo de datos de formulario. Tendrá que abrir la hoja de propiedades de los campos siguientes y establecer su bindRef como se muestra a continuación


| Nombre del campo | Enlace a referencia |
|------------|--------------------|
| Nombre | /newhire/FirstName |
| Apellidos | /newhire/lastName |

>[!NOTE]
>
>Siéntase libre de agregar campos de texto adicionales y enlazarlos a los elementos del modelo de datos de formulario correspondientes

## Tabla Configurar beneficiarios

El siguiente paso es mostrar a los beneficiarios de los empleados en forma tabular. El formulario de ejemplo proporcionado tiene una tabla con 4 columnas y una sola fila. Es necesario configurar la tabla para que crezca según el número de beneficiarios.

* Abra el formulario en modo de edición.
* Expandir panel raíz->Sus beneficiarios->Tabla
* Seleccione Fila1 y haga clic en el icono de la llave inglesa para abrir su hoja de propiedades.
* Establezca la referencia de enlace como **/newhire/GetEmployeeBeneficiaries**
* Establezca Repetir configuración: Mínimo en 1 y Recuento máximo en 5.
* La configuración de Row1 debería ser similar a la captura de pantalla siguiente
   ![row-configure](assets/configure-row.PNG)
* Haga clic en la plantilla azul para guardar los cambios

## Enlazar celdas de fila

Finalmente, es necesario enlazar las celdas de la fila a los elementos del Modelo de datos de formulario.

* Expandir panel raíz->Sus beneficiarios->Tabla->Fila1
* Establezca la referencia de enlace de cada celda de fila según la tabla siguiente

| Celda de fila | Referencia de enlace |
|------------|----------------------------------------------|
| Nombre | /newhire/GetEmployeeBeneficiaries/firstname |
| Apellidos | /newhire/GetEmployeeBeneficiaries/lastname |
| Relación | /newhire/GetEmployeeBeneficiaries/relation |
| Porcentaje | /newhire/GetEmployeeBeneficiaries/percentage |

* Haga clic en la plantilla azul para guardar los cambios

## Probar el formulario

Ahora, es necesario abrir el formulario con el empID apropiado en la dirección URL. Los siguientes 2 vínculos rellenarán los formularios con información de la base de datos
[Formulario con empID=207](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=207)
[Formulario con empID=208](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=208)

## Solución de problemas

Mi formulario está en blanco y no tiene datos

* Asegúrese de que el modelo de datos de formulario devuelve los resultados correctos.
* El formulario está asociado con el modelo de datos de formulario correcto
* Compruebe los enlaces de los campos
* Compruebe el archivo de registro de stdout. Debería ver que empID se escribe en el archivo. Si no ve este valor, es posible que el formulario no esté utilizando la plantilla personalizada que se proporcionó.

La tabla no se rellena

* Comprobar el enlace Fila1
* Asegúrese de que la configuración de repetición para Fila1 esté correctamente configurada (Mín. =1 y Máx. = 5 o más)

