---
title: Personalización de la bandeja de entrada
description: Añadir columnas personalizadas para mostrar datos adicionales del flujo de trabajo
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
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 3%

---


# Añadir columnas personalizadas

Para mostrar los datos de flujo de trabajo en la bandeja de entrada, es necesario definir y rellenar las variables en el flujo de trabajo. El valor de la variable debe configurarse antes de que se asigne una tarea a un usuario. Para ayudarle a empezar, hemos proporcionado un flujo de trabajo de muestra que está listo para implementarse en su servidor AEM.

* [Iniciar sesión en AEM](http://localhost:4502/crx/de/index.jsp)
* [Importación del flujo de trabajo de revisión](assets/review-workflow.zip)
* [Consulte el flujo de trabajo](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

Este flujo de trabajo tiene dos variables definidas (isMarried e revenue) y sus valores se establecen mediante el componente de variable set . Estas variables estarán disponibles como columnas para agregarlas a la bandeja de entrada de AEM

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

* [Iniciar sesión en la consola web de AEM](http://localhost:4502/system/console/bundles)
* [Implementar e iniciar el paquete de personalización de la bandeja de entrada](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [Abra la bandeja de entrada](http://localhost:4502/aem/inbox)
* Abra Control de administración haciendo clic en el icono _Vista de lista_ situado junto al botón _Crear_
* Agregue la columna Casado a la bandeja de entrada y guarde los cambios
* [Vaya a la interfaz de usuario de FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Importe el ](assets/snap-form.zip) formulario de ejemplo seleccionando  _File_ Uploadfrom  __ Createmenu
* [Obtener una vista previa del formulario](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Seleccione el _estado civil_ y envíe el formulario
   [ver bandeja de entrada](http://localhost:4502/aem/inbox)

Al enviar el formulario, se activará el flujo de trabajo y se asignará una tarea al usuario &quot;admin&quot;. Debería ver un valor debajo de la columna Casado como se muestra en esta captura de pantalla.

![mary-column](assets/married-column.PNG)
