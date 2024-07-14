---
title: Usar sightly template para mostrar los datos de la bandeja de entrada
description: Agregue columnas personalizadas para mostrar datos adicionales del flujo de trabajo mediante la plantilla sightly
feature: Adaptive Forms
doc-type: article
version: 6.5
jira: KT-5830
topic: Development
role: Developer
level: Experienced
exl-id: d09b46ed-3516-44cf-a616-4cb6e9dfdf41
duration: 68
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 0%

---

# Usar sightly template para mostrar los datos de la bandeja de entrada

Puede utilizar sightly template para dar formato a los datos que se van a mostrar en las columnas de la bandeja de entrada. En este ejemplo, se muestran los iconos de la interfaz de usuario de coral en función del valor de la columna de ingresos. La siguiente captura de pantalla muestra el uso de iconos en la columna de ingresos
![iconos de ingresos](assets/income-column.PNG)

[La plantilla sightly](assets/sightly-template.zip) utilizada para mostrar los iconos personalizados de la interfaz de usuario de coral se proporciona como parte de este artículo.

## Plantilla de Sightly

A continuación se muestra la plantilla de sightly. El código de la plantilla muestra un icono en función de los ingresos. AEM Los iconos están disponibles como parte de la [biblioteca de iconos de la interfaz de usuario de coral](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html#availableIcons) que se incluye con el código de la interfaz de usuario de la biblioteca .

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

El siguiente código es la implementación del servicio para mostrar la columna de ingresos.

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
>Este artículo supone que ha instalado [el flujo de trabajo de ejemplo](assets/review-workflow.zip) y [el formulario de ejemplo](assets/snap-form.zip) de [artículo anterior](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/inbox-customization/add-married-column.html) de esta serie.

* [Inicie sesión en crx como usuario administrador](http://localhost:4502/crx/de/index.jsp)
* [importar plantilla de sightly](assets/sightly-template.zip)
* AEM [Iniciar sesión en la consola web de la](http://localhost:4502/system/console/bundles)
* [Implementar e iniciar el paquete de personalización de bandeja de entrada](assets/income-column-customization.jar)
* [Abrir la bandeja de entrada](http://localhost:4502/aem/inbox)
* Abra Control de administración haciendo clic en Vista de lista junto al botón Crear
* Agregue la columna de ingresos a la bandeja de entrada y guarde los cambios
* [Vista previa del formulario](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Seleccione _estado civil_ y envíe el formulario
* [Ver bandeja de entrada](http://localhost:4502/aem/inbox)

Al enviar el formulario, se almacenará en déclencheur el flujo de trabajo y se asignará una tarea al usuario &quot;administrador&quot;. Debería ver el icono correspondiente debajo de la columna de ingresos
