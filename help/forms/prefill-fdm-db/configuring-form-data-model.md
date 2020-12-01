---
title: Configuración del modelo de datos de formulario
description: Crear modelo de datos de formulario basado en el origen de datos RDBMS
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5812
thumbnail: kt-5812.jpg
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 0%

---



# Configuración del modelo de datos de formulario

## Apache Sling Connection Pooled DataSource

El primer paso para crear un modelo de datos de formulario respaldado por RDBMS es configurar Apache Sling Connection Pooled DataSource. Para configurar la fuente de datos, siga los pasos que se indican a continuación:

* Apunte el explorador para [configMgr](http://localhost:4502/system/console/configMgr)
* Buscar **Apache Sling Connection Pooled DataSource**
* Añada una nueva entrada y proporcione los valores como se muestra en la captura de pantalla.
* ![data-source](assets/data-source.png)
* Guardar los cambios

>[!NOTE]
>El URI de conexión JDBC, el nombre de usuario y la contraseña cambiarán según la configuración de la base de datos MySQL.


## Creación del modelo de datos de formulario

* Apunta a tu explorador para [Integraciones de datos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm)
* Haga clic en _Crear_->_Modelo de datos de formulario_
* Proporcione un nombre y un título significativos al modelo de datos de formulario, como **Empleado**
* Haga clic en _Siguiente_
* Seleccione el origen de datos creado en la sección anterior(foros)
* Haga clic en _Crear_->Editar para abrir el modelo de datos de formulario recién creado en modo de edición
* Expanda el nodo _foros_ para ver el esquema del empleado. Expanda el nodo empleado para ver las dos tablas

## Añadir entidades al modelo

* Asegúrese de que el nodo de empleado está expandido
* Seleccione las entidades newhire y de los beneficiarios y haga clic en _Añadir seleccionados_

## Añadir servicio de lectura a cualquier entidad

* Seleccionar entidad nueva
* Haga clic en _Editar propiedades_
* Seleccione Obtener en la lista desplegable Servicio de lectura
* Haga clic en el icono + para agregar un parámetro al servicio get
* Especifique los valores como se muestra en la captura de pantalla
* ![get-service](assets/get-service.png)
>[!NOTE]
> El servicio get espera un valor asignado a la columna empID de la nueva entidad. Existen varias formas de pasar este valor y en este tutorial el empID se pasará a través del parámetro de solicitud llamado empID.
* Haga clic _Listo_ para guardar los argumentos del servicio get
* Haga clic en _Listo_ para guardar los cambios en el modelo de datos de formulario

## Añadir asociación entre 2 entidades

Las asociaciones definidas entre entidades de base de datos no se crean automáticamente en el modelo de datos de formulario. Las asociaciones entre entidades deben definirse mediante el editor del modelo de datos de formulario. Cada nueva entidad puede tener uno o más beneficiarios, necesitamos definir una relación de uno a muchos entre las entidades newhire y las entidades beneficiarias.
Los siguientes pasos le guiarán por el proceso de creación de la asociación uno a varios

* Seleccione cualquier entidad y haga clic en _Añadir asociación_
* Proporcione un Título y un identificador significativos a la asociación y otras propiedades, como se muestra en la captura de pantalla siguiente
   ![asociación](assets/association-entities-1.png)

* Haga clic en el icono _editar_ en la sección Argumentos

* Especifique los valores como se muestra en esta captura de pantalla
* ![asociación-2](assets/association-entities.png)
* **Estamos vinculando las dos entidades mediante la columna empID de beneficiarios y nuevas entidades.**
* Haga clic en _Listo_ para guardar los cambios

## Probar el modelo de datos de formulario

Nuestro modelo de datos de formulario ahora tiene **_servicio get_** que acepta empID y devuelve los detalles del newhir y sus beneficiarios. Para probar el servicio get, siga los pasos que se indican a continuación.

* Seleccionar entidad nueva
* Haga clic en _Test Model Object_
* Proporcione un empID válido y haga clic en _Prueba_
* Debería obtener los resultados como se muestra en la captura de pantalla siguiente
* ![test-fdm](assets/test-form-data-model.png)
