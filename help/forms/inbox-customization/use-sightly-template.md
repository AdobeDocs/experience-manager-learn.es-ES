---
title: Personalización de la bandeja de entrada
description: Añadir columnas personalizadas para mostrar datos adicionales del flujo de trabajo mediante una plantilla de diseño
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5.5
kt: 5830
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 2%

---

# Uso de una plantilla ligera para mostrar datos de bandeja de entrada

Puede utilizar una plantilla ligera para dar formato a los datos que se van a mostrar en las columnas de la bandeja de entrada. En este ejemplo se muestran los iconos corales-ui en función del valor de la columna de ingresos. La siguiente captura de pantalla muestra el uso de iconos en los iconos de la columna![de ingresos](assets/income-column.PNG)

[La plantilla](assets/sightly-template.zip) de suavizado utilizada para mostrar los iconos personalizados de corales ui se proporciona como parte de este artículo.

## Plantilla Sightly

A continuación se muestra la plantilla de Sightly. El código de la plantilla muestra un icono en función de los ingresos. Los iconos están disponibles como parte de la biblioteca [de iconos de](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html#availableIcons) corales ui que se incluye con AEM.

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

La línea 12 asocia la columna con la plantilla de revisión

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

## Probar en el servidor

>[!NOTE]
>
>En este artículo se asume que ha instalado en esta serie el flujo de trabajo [de](assets/review-workflow.zip) muestra y el formulario [de](assets/snap-form.zip) ejemplo del artículo [](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/inbox-customization/add-married-column.md) anterior.

* [Iniciar sesión en crx como usuario administrador](http://localhost:4502/crx/de/index.jsp)
* [importar plantilla](assets/sightly-template.zip)
* [Iniciar sesión en AEM consola web](http://localhost:4502/system/console/bundles)
* [Implementación y inicio del paquete de personalización de bandeja de entrada](assets/income-column-customization.jar)
* [Abra la bandeja de entrada](http://localhost:4502/aem/inbox)
* Abra el Control de administración haciendo clic en la Vista de Lista al lado del botón Crear
* Añadir la columna de ingresos a la Bandeja de entrada y guardar los cambios
* [Obtener una vista previa del formulario](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Seleccione el estado __ marital y envíe el formulario
* [Bandeja de entrada de vista](http://localhost:4502/aem/inbox)

El envío del formulario activará el flujo de trabajo y se asignará una tarea al usuario &quot;admin&quot;. Debería ver el icono correspondiente en la columna de ingresos
