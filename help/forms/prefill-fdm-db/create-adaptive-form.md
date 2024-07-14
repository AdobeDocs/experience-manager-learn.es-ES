---
title: Crear formulario adaptable
description: Crear y configurar formularios adaptables para utilizar el servicio de rellenado previo del modelo de datos de formulario
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-5813
thumbnail: kt-5813.jpg
topic: Development
role: User
level: Beginner
exl-id: c8d4eed8-9e2b-458c-90d8-832fc9e0ad3f
duration: 124
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 2%

---

# Crear formulario adaptable

Hasta ahora hemos creado lo siguiente

* Base de datos con 2 tablas: `newhire` y `beneficiaries`
* Fuente de datos obtenida de una conexión Apache Sling configurada
* Modelo de datos de formulario basado en RDBMS

El siguiente paso es crear y configurar un formulario adaptable para utilizar el modelo de datos de formulario.  Para comenzar, puedes [descargar e importar](assets/fdm-demo-af.zip) formularios de ejemplo. El formulario de ejemplo tiene una sección para mostrar los detalles del empleado y otra para enumerar los beneficiarios del empleado.

## Asociar un formulario al modelo de datos de formulario

El formulario de ejemplo proporcionado con este curso no está asociado a ningún modelo de datos de formulario. Para configurar el formulario para que utilice el modelo de datos de formulario, se debe hacer lo siguiente:

* Seleccione el formulario FDMDemo
* Haga clic en _Propiedades_->_Modelo de formulario_
* Seleccione Modelo de datos de formulario de la lista desplegable
* Busque y seleccione el modelo de datos de formulario creado en la lección anterior.
* Haz clic en _Guardar y cerrar_

## Configurar el servicio de relleno previo

El primer paso es asociar el servicio de relleno previo al formulario. Para asociar el servicio de relleno previo, siga los pasos que se mencionan a continuación

* Seleccionar el formulario `FDMDemo`
* Haga clic en _Editar_ para abrir el formulario en modo de edición
* Seleccione Contenedor de formulario en la jerarquía de contenido y haga clic en el icono de llave inglesa para abrir su hoja de propiedades
* Seleccione _Servicio de prerrellenado del modelo de datos de formulario_ en la lista desplegable Servicio de prerrellenado
* Haga clic en el ☑ azul para guardar los cambios

* ![prefill-service](assets/fdm-prefill.png)

## Configurar detalles del empleado

El siguiente paso es enlazar los campos de texto del formulario adaptable a los elementos del modelo de datos de formulario. Debe abrir la hoja de propiedades de los siguientes campos y establecer su bindRef como se muestra a continuación


| Nombre del campo | Ref. enlace |
|------------|--------------------|
| Nombre | /newhire/FirstName |
| LastName | /newhire/lastName |

>[!NOTE]
>
>No dude en añadir campos de texto adicionales y enlazarlos a elementos adecuados del modelo de datos de formulario

## Configuración de la tabla de beneficiarios

El siguiente paso es mostrar los beneficiarios del empleado en forma de tabla. El formulario de ejemplo proporcionado tiene una tabla con 4 columnas y una sola fila. Necesitamos configurar la tabla para que crezca según el número de beneficiarios.

* Abra el formulario en modo de edición.
* Expanda el Panel Raíz->Sus Beneficiarios->Tabla
* Seleccione Row1 y haga clic en el icono de la llave inglesa para abrir su hoja de propiedades.
* Establecer la referencia de enlace en **/new/GetEmployeeBeneficiaries**
* Establezca la Configuración de repetición: Recuento mínimo en 1 y Recuento máximo en 5.
* La configuración de Row1 debe ser similar a la captura de pantalla siguiente
  ![row-configure](assets/configure-row.PNG)
* Haga clic en el ☑ azul para guardar los cambios

## Enlazar celdas de fila

Por último, es necesario enlazar las celdas de fila a los elementos del modelo de datos de formulario.

* Expanda el Panel raíz->Sus beneficiarios->Tabla->Fila1
* Establezca la Referencia de enlace de cada celda de fila según la tabla siguiente

| Celda de fila | Referencia de enlace |
|------------|----------------------------------------------|
| Nombre | /new/GetEmployeeBeneficiaries/firstname |
| Apellidos | /new/GetEmployeeBeneficiaries/lastname |
| Relación | /new/GetEmployeeBeneficiaries/relation |
| Porcentaje | /new/getEmployeeBeneficiaries/percentage |

* Haga clic en el ☑ azul para guardar los cambios

## Prueba del formulario

Ahora necesitamos abrir el formulario con el empID apropiado en la URL. Los dos vínculos siguientes rellenarán los formularios con información de la base de datos
[Formulario con empID=207](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=207)
[Formulario con empID=208](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=208)

## Resolución de problemas

Mi formulario está en blanco y no contiene datos

* Asegúrese de que el modelo de datos de formulario devuelve los resultados correctos.
* El formulario está asociado al modelo de datos de formulario correcto
* Compruebe los enlaces de campo
* Compruebe el archivo de registro stdout. Debería ver que empID se escribe en el archivo. Si no ve este valor, es posible que el formulario no esté utilizando la plantilla personalizada proporcionada.

La tabla no está rellenada

* Compruebe el enlace Fila1
* Asegúrese de que la configuración de repetición de Fila1 está establecida correctamente (Mín. =1 y Máx. = 5 o más)
