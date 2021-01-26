---
title: Personalizar componente de resumen
description: Amplíe el componente de paso de resumen para incluir la capacidad de desplazarse al siguiente formulario en el paquete.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6894
thumbnail: 6894.jpg
translation-type: tm+mt
source-git-commit: 049574ab2536b784d6b303f474dba0412007e18c
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 0%

---


# Personalizar paso de resumen

El componente de paso de resumen se utiliza para mostrar el resumen del envío del formulario con un vínculo para descargar el formulario firmado. El paso de resumen se coloca generalmente en el último panel del formulario.
Para este caso de uso hemos creado un nuevo componente basado en el componente Resumen predeterminado y hemos ampliado la capacidad para incluir clientlib personalizado.

Este componente se identifica mediante la etiqueta Firmar formulario múltiple

La siguiente captura de pantalla muestra el nuevo componente que se creó para mostrar el mensaje al finalizar la ceremonia de firma

![componente de resumen](assets/summary.PNG)

El nuevo componente se basa en el componente de resumen listo para usar.
![component-prop](assets/componentprop.PNG)

Se ha agregado un botón para desplazarse al siguiente formulario para firmar
![template-code](assets/template-code.PNG)

Summary.jsp tiene el siguiente código. Tiene una referencia a la biblioteca del cliente identificada por el identificador de categoría **getnextform**

```java
<%--
  Guide Summary Component
--%>
<%@include file="/libs/fd/af/components/guidesglobal.jsp"%>
<%@include file="/libs/fd/afaddon/components/summary/summary.jsp"%>
<ui:includeClientLib categories="getnextform"/>
```

## Assets

El componente de resumen personalizado se puede [descargar desde aquí](assets/custom-summary-step.zip)


