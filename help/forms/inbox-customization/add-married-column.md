---
title: Añadir columnas personalizadas
description: Añadir columnas personalizadas para mostrar datos adicionales del flujo de trabajo
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
kt: 5830
topic: Development
role: Developer
level: Experienced
exl-id: 0b141b37-6041-4f87-bd50-dade8c0fee7d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 2%

---

# Añadir columnas personalizadas

Para mostrar los datos de flujo de trabajo en la bandeja de entrada, es necesario definir y rellenar las variables en el flujo de trabajo. El valor de la variable debe configurarse antes de que se asigne una tarea a un usuario. Para ayudarle a empezar, hemos proporcionado un flujo de trabajo de muestra que está listo para implementarse en su servidor AEM.

* [Iniciar sesión en AEM](http://localhost:4502/crx/de/index.jsp)
* [Importación del flujo de trabajo de revisión](assets/review-workflow.zip)
* [Consulte el flujo de trabajo](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

Este flujo de trabajo tiene dos variables definidas (isMarried e revenue) y sus valores se establecen mediante el componente de variable set . Estas variables están disponibles como columnas para agregarlas a AEM bandeja de entrada

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
>Debe incluir AEM 6.5.5 Uber.jar en el proyecto para que funcione el código anterior

![uber-jar](assets/uber-jar.PNG)

## Realizar pruebas en el servidor

* [Iniciar sesión en AEM consola web](http://localhost:4502/system/console/bundles)
* [Implementar e iniciar el paquete de personalización de la bandeja de entrada](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [Abra la bandeja de entrada](http://localhost:4502/aem/inbox)
* Abra Control de administración haciendo clic en _Vista de lista_ junto a _Crear_ botón
* Agregue la columna Casado a la bandeja de entrada y guarde los cambios
* [Vaya a la interfaz de usuario de FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Importación del formulario de ejemplo](assets/snap-form.zip) seleccionando _Carga de archivo_ from _Crear_ menú
* [Obtener una vista previa del formulario](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Seleccione el _estado civil_ y enviar el formulario
   [ver bandeja de entrada](http://localhost:4502/aem/inbox)

Si se envía el formulario, se déclencheur el flujo de trabajo y se asigna una tarea al usuario &quot;admin&quot;. Debería ver un valor debajo de la columna Casado como se muestra en esta captura de pantalla.

![mary-column](assets/married-column.PNG)
