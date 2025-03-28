---
title: Añadir columnas personalizadas
description: Añadir columnas personalizadas para mostrar datos adicionales del flujo de trabajo
feature: Adaptive Forms
doc-type: article
version: Experience Manager 6.5
jira: KT-5830
topic: Development
role: Developer
level: Experienced
exl-id: 0b141b37-6041-4f87-bd50-dade8c0fee7d
duration: 75
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '276'
ht-degree: 1%

---

# Añadir columnas personalizadas

Para mostrar los datos del flujo de trabajo en la bandeja de entrada, es necesario definir y rellenar variables en el flujo de trabajo. El valor de la variable debe establecerse antes de asignar una tarea a un usuario. Para ayudarle a empezar, hemos proporcionado un flujo de trabajo de ejemplo listo para implementarlo en su servidor de AEM.

* [Iniciar sesión en AEM](http://localhost:4502/crx/de/index.jsp)
* [Importar el flujo de trabajo de revisión](assets/review-workflow.zip)
* [Revisar el flujo de trabajo](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

Este flujo de trabajo tiene dos variables definidas (isMarried y revenue) y sus valores se establecen mediante el componente de variable set. Estas variables están disponibles como columnas para añadirlas a la bandeja de entrada AEM

## Crear servicio

Para cada columna que necesitemos mostrar en nuestra bandeja de entrada necesitaríamos escribir un servicio. El siguiente servicio nos permite agregar una columna para mostrar el valor de la variable isMarried

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

## Realizar pruebas en el servidor

* [Iniciar sesión en la consola web de AEM](http://localhost:4502/system/console/bundles)
* [Implementar e iniciar el paquete de personalización de bandeja de entrada](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [Abrir la bandeja de entrada](http://localhost:4502/aem/inbox)
* Para abrir el Control de administración, haga clic en el icono _Vista de lista_ junto al botón _Crear_
* Agregue la columna Casado a la bandeja de entrada y guarde los cambios
* [Ir a la interfaz de usuario de FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Importe el formulario de ejemplo](assets/snap-form.zip) seleccionando _Cargar archivos_ en el menú _Crear_
* [Vista previa del formulario](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Seleccione _estado civil_ y envíe el formulario
  [ver bandeja de entrada](http://localhost:4502/aem/inbox)

Al enviar el formulario, se almacenará en déclencheur el flujo de trabajo y se asignará una tarea al usuario &quot;administrador&quot;. Debería ver un valor en la columna Casado, como se muestra en esta captura de pantalla

![columna-casada](assets/married-column.PNG)

## Siguientes pasos

[Mostrar columna Casada](./use-sightly-template.md)
