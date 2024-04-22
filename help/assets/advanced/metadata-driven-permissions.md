---
title: Permisos impulsados por metadatos en AEM Assets
description: Los permisos impulsados por metadatos son una función que se utiliza para restringir el acceso en función de las propiedades de los metadatos de los recursos, en lugar de la estructura de carpetas.
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-13757
thumbnail: xx.jpg
doc-type: Tutorial
source-git-commit: 3b500873ee7307df590ac66dea541a1adf14d726
workflow-type: tm+mt
source-wordcount: '685'
ht-degree: 0%

---

# Permisos impulsados por metadatos{#metadata-driven-permissions}

Permisos impulsados por metadatos es una función que se utiliza para permitir que las decisiones de control de acceso en AEM Assets Author se basen en propiedades de metadatos de recursos en lugar de en la estructura de carpetas. Con esta capacidad, puede definir políticas de control de acceso que evalúen atributos como el estado del recurso, el tipo o cualquier propiedad de metadatos personalizada que defina.

Veamos un ejemplo... Los creativos cargan su trabajo en AEM Assets en la carpeta relacionada con la campaña, podría ser un recurso de trabajo en curso que no se haya aprobado para su uso. Queremos asegurarnos de que los especialistas en marketing solo vean los activos aprobados para esta campaña. Podemos utilizar la propiedad de metadatos para indicar que un recurso se ha aprobado y que los especialistas en marketing pueden utilizarlo.

## Cómo funciona

La activación de permisos impulsados por metadatos implica la definición de qué propiedades de metadatos de recurso generarán restricciones de acceso, como &quot;estado&quot; o &quot;marca&quot;. Estas propiedades se pueden utilizar para crear entradas de control de acceso que especifiquen qué grupos de usuarios tienen acceso a recursos con valores de propiedad específicos.

## Requisitos previos

AEM Se requiere acceso a un entorno as a Cloud Service de la actualizado a la versión más reciente para configurar permisos impulsados por metadatos.


## Pasos de desarrollo

Para implementar permisos impulsados por metadatos:

1. Determine qué propiedades de metadatos de recurso se utilizarán para el control de acceso. En nuestro caso, va a ser una propiedad llamada `status`.
1. Crear una configuración de OSGi `com.adobe.cq.dam.assetmetadatarestrictionprovider.impl.DefaultRestrictionProviderConfiguration.cfg.json` en el proyecto.
1. Pegue el siguiente JSON en el archivo creado

   ```json
   {
     "restrictionPropertyNames":[
       "status"
     ],
     "restrictionPaths":[
       "/content/dam"
     ]
   }
   ```

1. Reemplace los nombres de propiedad y las rutas de restricción por los valores necesarios.


Antes de agregar entradas de control de acceso basadas en restricciones, se debe agregar una nueva entrada de nivel superior para denegar primero el acceso de lectura a todos los grupos sujetos a la evaluación de permisos para recursos (por ejemplo, &quot;colaboradores&quot; o similares):

1. Vaya a la pantalla Herramientas → Seguridad → Permisos
1. Seleccione el grupo &quot;Colaboradores&quot; (u otro grupo personalizado al que pertenezcan todos los grupos de usuarios)
1. Haga clic en &quot;Agregar ACE&quot; en la esquina superior derecha de la pantalla
1. Seleccione /content/dam para la ruta
1. Introducir jcr:read para privilegios
1. Seleccione Denegar para el tipo de permiso
1. En Restricciones, seleccione rep:ntNames e introduzca dam:Asset como Valor de restricción
1. Haga clic en Guardar

![Denegar acceso](./assets/metadata-driven-permissions/deny-access.png)

Ahora se pueden agregar entradas de control de acceso para conceder acceso de lectura a grupos de usuarios basados en valores de propiedad de metadatos de recursos.

1. Vaya a la pantalla Herramientas → Seguridad → Permisos
1. Seleccione el grupo que desee
1. Haga clic en &quot;Agregar ACE&quot; en la esquina superior derecha de la pantalla
1. Seleccione /content/dam (o una subcarpeta) para Ruta
1. Introducir jcr:read para privilegios
1. Seleccione Permitir por tipo de permiso
1. En Restricciones, seleccione uno de los nombres de propiedad de metadatos de recursos configurados (las propiedades definidas en la configuración OSGi se incluirán aquí)
1. Introduzca el valor de propiedad de metadatos requerido en el campo Restriction Value
1. Haga clic en el icono &quot;+&quot; para añadir la restricción a la entrada de control de acceso
1. Haga clic en Guardar

![Permitir el acceso](./assets/metadata-driven-permissions/allow-access.png)

La carpeta de ejemplo contiene un par de recursos.

![Vista de administrador](./assets/metadata-driven-permissions/admin-view.png)

Una vez configurados los permisos y establecidas las propiedades de los metadatos del recurso según corresponda, los usuarios (usuario de marketing en nuestro caso) solo verán el recurso aprobado.

![Vista de experto en marketing](./assets/metadata-driven-permissions/marketeer-view.png)

## Ventajas y consideraciones

Las ventajas de los permisos impulsados por metadatos incluyen:

- Control preciso del acceso a los recursos en función de atributos específicos.
- Desvinculación de las políticas de control de acceso de la estructura de carpetas, lo que permite una organización de recursos más flexible.
- Capacidad para definir reglas de control de acceso complejas basadas en varias propiedades de metadatos.

>[!NOTE]
>
> Es importante tener en cuenta lo siguiente:
> 
> - Las propiedades de metadatos se evalúan teniendo en cuenta las restricciones mediante la igualdad de cadenas (otros tipos de datos aún no compatibles, como la fecha)
> - Para permitir varios valores para una propiedad de restricción, se pueden agregar restricciones adicionales a la Entrada de control de acceso seleccionando la misma propiedad en la lista desplegable &quot;Seleccionar tipo&quot; e introduciendo un nuevo valor de restricción (por ejemplo, `status=approved`, `status=wip`) y haciendo clic en &quot;+&quot; para añadir la restricción a la entrada
> ![Permitir varios valores](./assets/metadata-driven-permissions/allow-multiple-values.png)
> - Varias restricciones en una única entrada de control de acceso con nombres de propiedad diferentes (p. ej., `status=approved`, `brand=Adobe`) se evaluará como una condición AND, es decir, el grupo de usuarios seleccionado tendrá acceso de lectura a los recursos con `status=approved AND brand=Adobe`
> ![Permitir varias restricciones](./assets/metadata-driven-permissions/allow-multiple-restrictions.png)
> - Si se agrega una nueva entrada de control de acceso con una restricción de propiedad de metadatos, se establecerá una condición OR para las entradas, por ejemplo, una sola entrada con restricción `status=approved` y una sola entrada con `brand=Adobe` se evaluará como `status=approved OR brand=Adobe`
> ![Permitir varias restricciones](./assets/metadata-driven-permissions/allow-multiple-aces.png)
