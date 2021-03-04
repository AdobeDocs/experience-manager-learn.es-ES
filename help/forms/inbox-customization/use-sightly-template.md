---
title: Personalización de la bandeja de entrada
description: Añadir columnas personalizadas para mostrar datos adicionales del flujo de trabajo mediante una plantilla de aspecto
feature: Formularios adaptables
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5.5
kt: 5830
topic: Desarrollo
role: Desarrollador
level: Con experiencia
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 4%

---

# Uso de una plantilla sutil para mostrar los datos de la bandeja de entrada

Puede utilizar una plantilla sutil para dar formato a los datos que se van a mostrar en las columnas de la bandeja de entrada. En este ejemplo se muestran iconos de coral-ui en función del valor de la columna de ingresos. La siguiente captura de pantalla muestra el uso de iconos en la columna de ingresos
![iconos de ingresos](assets/income-column.PNG)

[La ](assets/sightly-template.zip) plantilla sightly utilizada para mostrar los iconos personalizados de la interfaz de usuario de coral se proporciona como parte de este artículo.

## Plantilla Sightly

A continuación, se muestra la plantilla. El código de la plantilla muestra un icono en función de los ingresos. Los iconos están disponibles como parte de la [biblioteca de iconos de la interfaz de usuario de coral](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html#availableIcons) que viene con AEM.

```java
<template data-sly-template.incomeTemplate="${@ item}>">
    <td is="coral-table-cell" class="payload-income-cell">
         <div data-sly-test="${(item.workflowMetadata && item.workflowMetadata.income)}" data-sly-set.income ="${item.workflowMetadata.income}">
                 <coral-icon icon="confidenceOne" size="M" data-sly-test="${income >=0 && income <10000}"></coral-icon>
                 <coral-icon icon="confidenceTwo" size="M" data-sly-test="${income >=10000 && income <100000}"></coral-icon>
                 <coral-icon icon="confidenceThree" size="M" data-sly-test="${income >=100000 && income <500000}"></coral-icon>
                 <coral-icon icon="confidenceFour" size="M" data-sly-test="${income >=500000}"></coral-icon>
          </div>
    </td>
</template>
```

## Implementación del servicio

El siguiente código es la implementación de servicio para mostrar la columna de ingresos.

La línea 12 asocia la columna con la plantilla sightly

```java
import java.util.Map;
import org.osgi.service.component.annotations.Component;
import com.adobe.cq.inbox.ui.InboxItem;
import com.adobe.cq.inbox.ui.column.Column;
import com.adobe.cq.inbox.ui.column.provider.ColumnProvider;

@Component(service = ColumnProvider.class, immediate = true)
public class IncomeProvider implements ColumnProvider {
@Override
public Column getColumn() {

return new Column("income", "Income", String.class.getName(),"inbox/customization/column-templates.html", "incomeTemplate");
}

@Override
public Object getValue(InboxItem inboxItem) {
Object val = null;

Map workflowMetadata = inboxItem.getWorkflowMetadata();

if (workflowMetadata != null && workflowMetadata.containsKey("income"))
    val = workflowMetadata.get("income");

return val;
}
}
```

## Realizar pruebas en el servidor

>[!NOTE]
>
>Este artículo supone que ha instalado el [ejemplo de flujo de trabajo](assets/review-workflow.zip) y el [formulario de ejemplo](assets/snap-form.zip) del [artículo anterior](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/inbox-customization/add-married-column.md) en esta serie.

* [Inicie sesión en crx como usuario administrador](http://localhost:4502/crx/de/index.jsp)
* [importar plantilla](assets/sightly-template.zip)
* [Iniciar sesión en la consola web de AEM](http://localhost:4502/system/console/bundles)
* [Implementar e iniciar el paquete de personalización de la bandeja de entrada](assets/income-column-customization.jar)
* [Abra la bandeja de entrada](http://localhost:4502/aem/inbox)
* Abra Control de administración haciendo clic en Vista de lista junto al botón Crear
* Agregue la columna de ingresos a la bandeja de entrada y guarde los cambios
* [Obtener una vista previa del formulario](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Seleccione el _estado civil_ y envíe el formulario
* [Ver bandeja de entrada](http://localhost:4502/aem/inbox)

Al enviar el formulario, se activará el flujo de trabajo y se asignará una tarea al usuario &quot;admin&quot;. Debería ver el icono apropiado en la columna de ingresos
