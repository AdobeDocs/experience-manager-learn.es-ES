---
title: Configurar el modelo de datos de formulario
description: Crear un modelo de datos de formulario basado en una fuente de datos RDBMS
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-5812
thumbnail: kt-5812.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 5fa4638f-9faa-40e0-a20d-fdde3dbb528a
duration: 103
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '491'
ht-degree: 0%

---

# Configurar el modelo de datos de formulario

## Fuente de datos obtenida de una conexión Apache Sling

El primer paso para crear un modelo de datos de formulario respaldado por RDBMS es configurar una fuente de datos obtenida de una conexión Apache Sling. Para configurar la fuente de datos, siga los pasos que se indican a continuación:

* Dirija su explorador a [configMgr](http://localhost:4502/system/console/configMgr)
* Busque **Fuente de datos obtenida de una conexión Apache Sling**
* Añada una nueva entrada y proporcione los valores que se muestran en la captura de pantalla.
* ![origen de datos](assets/data-source.png)
* Guarde los cambios

>[!NOTE]
>El URI de conexión JDBC, el nombre de usuario y la contraseña cambiarán según la configuración de la base de datos MySQL.


## Creación del modelo de datos de formulario

* Dirija su navegador a [Integraciones de datos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm)
* Haga clic en _Crear_->_Modelo de datos de formulario_
* Proporcione un nombre y un título significativos al modelo de datos de formulario, como **Empleado**
* Haga clic en _Siguiente_
* Seleccionar la fuente de datos creada en la sección (foros) anterior
* Haga clic en _Crear_->Editar para abrir el modelo de datos de formulario recién creado en el modo Edición
* Expanda el nodo _forums_ para ver el esquema de los empleados. Expanda el nodo de empleados para ver las 2 tablas

## Añadir entidades al modelo

* Asegúrese de que el nodo del empleado esté expandido
* Seleccione las nuevas entidades de beneficiarios y haga clic en _Agregar seleccionados_

## Añadir servicio de lectura a la nueva entidad

* Seleccionar nueva entidad
* Haz clic en _Editar propiedades_
* Seleccione obtener de la lista desplegable Servicio de lectura
* Haga clic en el icono + para añadir un parámetro al servicio obtener
* Especifique los valores como se muestra en la captura de pantalla
* ![get-service](assets/get-service.png)
>[!NOTE]
> El servicio de obtención espera un valor asignado a la columna empID de la nueva entidad. Existen varias formas de pasar este valor y, en este tutorial, el empID se pasa a través del parámetro de solicitud denominado empID.
* Haga clic en _Listo_ para guardar los argumentos del servicio conseguir
* Haga clic en _Listo_ para guardar los cambios del modelo de datos de formulario

## Agregar asociación entre 2 entidades

Las asociaciones definidas entre entidades de base de datos no se crean automáticamente en el modelo de datos de formulario. Las asociaciones entre entidades deben definirse con el editor del modelo de datos de formulario. Cada nueva entidad puede tener uno o más beneficiarios, necesitamos definir una asociación de uno a varios entre la nueva entidad y las entidades beneficiarias.
Los siguientes pasos le guiarán por el proceso de creación de la asociación &quot;uno a varios&quot;

* Seleccione una nueva entidad y haga clic en _Agregar asociación_
* Proporcione un título y un identificador significativos a la asociación y a otras propiedades, como se muestra en la captura de pantalla siguiente
  ![asociación](assets/association-entities-1.png)

* Haga clic en el icono _edit_ de la sección Argumentos

* Especifique los valores como se muestra en esta captura de pantalla
* ![asociación-2](assets/association-entities.png)
* **Estamos vinculando las dos entidades mediante la columna empID de beneficiarios y entidades nuevas.**
* Haga clic en _Listo_ para guardar los cambios

## Prueba del modelo de datos de formulario

Nuestro modelo de datos de formulario ahora tiene el servicio **_get_** que acepta empID y devuelve los detalles del nuevo equipo y sus beneficiarios. Para probar el servicio conseguir, siga los pasos que se indican a continuación.

* Seleccionar nueva entidad
* Haz clic en _Probar objeto de modelo_
* Proporcione un empID válido y haga clic en _Probar_
* Debe obtener resultados como se muestra en la captura de pantalla siguiente
* ![test-fdm](assets/test-form-data-model.png)

## Siguientes pasos

[Obtenga empID desde la dirección URL](./get-request-parameter.md)