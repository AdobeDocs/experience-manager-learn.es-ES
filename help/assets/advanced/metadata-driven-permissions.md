---
title: Permisos impulsados por metadatos en AEM Assets
description: Los permisos impulsados por metadatos son una función que se utiliza para restringir el acceso en función de las propiedades de los metadatos de los recursos, en lugar de la estructura de carpetas.
version: Experience Manager as a Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-13757
doc-type: Tutorial
last-substantial-update: 2024-05-03T00:00:00Z
exl-id: 57478aa1-c9ab-467c-9de0-54807ae21fb1
duration: 158
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '770'
ht-degree: 0%

---

# Permisos impulsados por metadatos{#metadata-driven-permissions}

Permisos impulsados por metadatos es una función que se utiliza para permitir que las decisiones de control de acceso de los AEM Assets Autor se basen en el contenido del recurso o en las propiedades de los metadatos, en lugar de en la estructura de carpetas. Con esta capacidad, puede definir políticas de control de acceso que evalúen atributos como el estado del recurso, el tipo o cualquier propiedad personalizada que defina.

Veamos un ejemplo... Los creativos cargan su trabajo en los AEM Assets de la carpeta relacionada con la campaña; puede que sea un recurso de trabajo en curso que no se haya aprobado para su uso. Queremos asegurarnos de que los especialistas en marketing solo vean los activos aprobados para esta campaña. Podemos utilizar una propiedad de metadatos para indicar que un recurso se ha aprobado y que los especialistas en marketing pueden utilizarlo.

## Cómo funciona

La activación de permisos impulsados por metadatos implica la definición de qué contenido de recurso o propiedades de metadatos impulsarán las restricciones de acceso, como &quot;estado&quot; o &quot;marca&quot;. Estas propiedades se pueden utilizar para crear entradas de control de acceso que especifiquen qué grupos de usuarios tienen acceso a recursos con valores de propiedad específicos.

## Requisitos previos

Se requiere acceso a un entorno de AEM as a Cloud Service actualizado a la versión más reciente para configurar permisos impulsados por metadatos.

## Configuración de OSGi {#configure-permissionable-properties}

Para implementar Permisos impulsados por metadatos, un desarrollador debe implementar una configuración OSGi en AEM as a Cloud Service que permita contenido de recursos específico o propiedades de metadatos para habilitar permisos impulsados por metadatos.

1. Determine qué propiedades de metadatos o contenido de recursos se utilizarán para el control de acceso. Los nombres de propiedad son los nombres de propiedad JCR en el recurso `jcr:content` o `jcr:content/metadata` del recurso. En nuestro caso, será una propiedad llamada `status`.
1. Cree una configuración OSGi `com.adobe.cq.dam.assetmetadatarestrictionprovider.impl.DefaultRestrictionProviderConfiguration.cfg.json` en su proyecto de AEM Maven.
1. Pegue el siguiente JSON en el archivo creado:

   ```json
   {
     "restrictionPropertyNames":[
       "status",
       "brand"
     ],
     "restrictionContentPropertyNames":[],
     "enabled":true
   }
   ```

1. Reemplace los nombres de propiedad por los valores necesarios.  La propiedad de configuración `restrictionContentPropertyNames` se usa para habilitar permisos en las propiedades de recursos `jcr:content`, mientras que la propiedad de configuración `restrictionPropertyNames` habilita permisos en las propiedades de recursos `jcr:content/metadata` para los recursos.

## Restablecer permisos de recursos base

Antes de agregar entradas de control de acceso basadas en restricciones, se debe agregar una nueva entrada de nivel superior para denegar primero el acceso de lectura a todos los grupos sujetos a la evaluación de permisos para Assets (por ejemplo, &quot;colaboradores&quot; o similares):

1. Vaya a la pantalla __Herramientas → Seguridad → Permisos__
1. Seleccione el grupo __Colaboradores__ (u otro grupo personalizado al que pertenezcan todos los grupos de usuarios)
1. Haga clic en __Agregar ACE__ en la esquina superior derecha de la pantalla
1. Seleccionar `/content/dam` para __ruta__
1. Escriba `jcr:read` para __privilegios__
1. Seleccionar `Deny` para __tipo de permiso__
1. En Restricciones, seleccione `rep:ntNames` e introduzca `dam:Asset` como __Valor de restricción__
1. Haga clic en __Guardar__

![Denegar acceso](./assets/metadata-driven-permissions/deny-access.png)

## Conceder acceso a los recursos mediante metadatos

Ahora se pueden agregar entradas de control de acceso para conceder acceso de lectura a los grupos de usuarios en función de los [valores de propiedad de metadatos de recursos configurados](#configure-permissionable-properties).

1. Vaya a la pantalla __Herramientas → Seguridad → Permisos__
1. Seleccione los grupos de usuarios que deben tener acceso a los recursos
1. Haga clic en __Agregar ACE__ en la esquina superior derecha de la pantalla
1. Seleccionar `/content/dam` (o una subcarpeta) para __ruta__
1. Escriba `jcr:read` para __privilegios__
1. Seleccionar `Allow` para __tipo de permiso__
1. En __Restricciones__, seleccione uno de los [nombres de propiedad de metadatos de recursos configurados en la configuración OSGi](#configure-permissionable-properties)
1. Escriba el valor de propiedad de metadatos requerido en el campo __Valor de restricción__
1. Haga clic en el icono __+__ para agregar la restricción a la entrada de control de acceso
1. Haga clic en __Guardar__

![Permitir el acceso](./assets/metadata-driven-permissions/allow-access.png)

## Permisos impulsados por metadatos en vigor

La carpeta de ejemplo contiene un par de recursos.

![Vista de administrador](./assets/metadata-driven-permissions/admin-view.png)

Una vez configurados los permisos y establecidas las propiedades de los metadatos del recurso en consecuencia, los usuarios (usuario de marketing en nuestro caso) solo verán el recurso aprobado.

![Vista de experto en marketing](./assets/metadata-driven-permissions/marketeer-view.png)

## Ventajas y consideraciones

Los beneficios de los permisos impulsados por metadatos incluyen:

- Control preciso del acceso a los recursos en función de atributos específicos.
- Desvinculación de las políticas de control de acceso de la estructura de carpetas, lo que permite una organización de recursos más flexible.
- Capacidad para definir reglas de control de acceso complejas basadas en múltiples propiedades de contenido o metadatos.

>[!NOTE]
>
> Es importante tener en cuenta lo siguiente:
> 
> - Las propiedades se evalúan teniendo en cuenta las restricciones con __igualdad de cadena__ (`=`) (otros tipos de datos u operadores aún no son compatibles, para propiedades de mayor que (`>`) o fecha)
> - Para permitir varios valores para una propiedad de restricción, se pueden agregar restricciones adicionales a la Entrada de control de acceso seleccionando la misma propiedad en la lista desplegable &quot;Seleccionar tipo&quot; e introduciendo un nuevo valor de restricción (por ejemplo, `status=approved`, `status=wip`) y haciendo clic en &quot;+&quot; para agregar la restricción a la entrada
> ![Permitir varios valores](./assets/metadata-driven-permissions/allow-multiple-values.png)
> - Se admiten __restricciones AND__ mediante varias restricciones en una sola entrada de control de acceso con nombres de propiedad diferentes (por ejemplo, `status=approved`, `brand=Adobe`) que se evaluará como una condición AND; es decir, se concederá acceso de lectura al grupo de usuarios seleccionado a los recursos con `status=approved AND brand=Adobe`
> ![Permitir varias restricciones](./assets/metadata-driven-permissions/allow-multiple-restrictions.png)
> - Se admiten __restricciones OR__ al agregar una nueva entrada de control de acceso con una restricción de propiedad de metadatos que establecerá una condición OR para las entradas; por ejemplo, una sola entrada con restricción `status=approved` y una sola entrada con `brand=Adobe` se evaluará como `status=approved OR brand=Adobe`
> ![Permitir varias restricciones](./assets/metadata-driven-permissions/allow-multiple-aces.png)
