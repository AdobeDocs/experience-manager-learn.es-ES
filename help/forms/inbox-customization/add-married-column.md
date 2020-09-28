---
title: Personalización de la bandeja de entrada
description: Añadir columnas personalizadas para mostrar datos adicionales del flujo de trabajo
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5.5
kt: 5830
translation-type: tm+mt
source-git-commit: ecbd4d21c5f41b2bc6db3b409767b767f00cc5d1
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 2%

---


# Añadir columnas personalizadas

Para mostrar los datos del flujo de trabajo en la bandeja de entrada, es necesario definir y rellenar variables en el flujo de trabajo. El valor de la variable debe configurarse antes de que se asigne una tarea a un usuario. Para ofrecerle un inicio principal, hemos proporcionado un flujo de trabajo de muestra que está listo para implementarse en su servidor AEM.

* [Iniciar sesión en AEM](http://localhost:4502/crx/de/index.jsp)
* [Importar el flujo de trabajo de revisión](assets/review-workflow.zip)
* [Revisar el flujo de trabajo](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

Este flujo de trabajo tiene dos variables definidas (isMarried y sus ingresos) y sus valores se establecen mediante el componente de variable set. Estas variables estarán disponibles como columnas para agregarlas a AEM bandeja de entrada

## Crear servicio

Por cada columna que necesitamos mostrar en nuestra bandeja de entrada, necesitaríamos escribir un servicio. El siguiente servicio nos permite agregar una columna para mostrar el valor de la variable isMarried

```java
import com.adobe.cq.inbox.ui.column.Column;
import com.adobe.cq.inbox.ui.column.provider.ColumnProvider;

import com.adobe.cq.inbox.ui.InboxItem;
import org.osgi.service.component.annotations.Component;

import java.util.Map;

/**
 * This provider does not require any sightly template to be defined.
 * It is used to display the value of 'ismarried' workflow variable as a column in inbox
 */
@Component(service = ColumnProvider.class, immediate = true)
public class MaritalStatusProvider implements ColumnProvider {@Override
public Column getColumn() {
return new Column("married", "Married", Boolean.class.getName());
}

// Return True or False if 'ismarried' is set. Else returns null
private Boolean isMarried(InboxItem inboxItem) {
Boolean ismarried = null;

Map metaDataMap = inboxItem.getWorkflowMetadata();
if (metaDataMap != null) {
if (metaDataMap.containsKey("isMarried")) {
    ismarried = (Boolean) metaDataMap.get("isMarried");
}
}

return ismarried;
}

@Override
public Object getValue(InboxItem inboxItem) {
return isMarried(inboxItem);
}
}
```

>[!NOTE]
>
>Debe incluir AEM 6.5.5 Uber.jar en su proyecto para que funcione el código anterior

![uber-jar](assets/uber-jar.PNG)

## Probar en el servidor

* [Iniciar sesión en AEM consola web](http://localhost:4502/system/console/bundles)
* [Implementación y inicio del paquete de personalización de bandeja de entrada](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [Abra la bandeja de entrada](http://localhost:4502/aem/inbox)
* Abra Control de administración haciendo clic en el icono de Vista _de_ Lista junto al botón _Crear_
* Añadir la columna Casado en la Bandeja de entrada y guardar los cambios
* [Ir a la interfaz de usuario de FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Importar el formulario](assets/snap-form.zip) de ejemplo seleccionando Cargar _archivo_ en el menú _Crear_
* [Obtener una vista previa del formulario](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Seleccione el estado __ marital y envíe el formulario
   [Bandeja de entrada de vista](http://localhost:4502/aem/inbox)

El envío del formulario activará el flujo de trabajo y se asignará una tarea al usuario &quot;admin&quot;. Debería ver un valor debajo de la columna Casado como se muestra en esta captura de pantalla.

![columna casada](assets/married-column.PNG)
