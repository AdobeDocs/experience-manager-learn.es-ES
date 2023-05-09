---
title: Personalizar componente de resumen
description: Amplíe el componente de paso de resumen para incluir la capacidad de navegar al siguiente formulario en el paquete.
feature: Adaptive Forms
version: 6.4,6.5
kt: 6894
thumbnail: 6894.jpg
topic: Development
role: Developer
level: Experienced
exl-id: fb68579d-241c-414d-92f4-13194f4d1923
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 1%

---

# Personalización del paso de resumen

El componente paso de resumen se utiliza para mostrar el resumen del envío del formulario con un vínculo para descargar el formulario firmado. El paso Resumen se suele colocar en el último panel del formulario.
A los efectos de este caso de uso, hemos creado un nuevo componente basado en el componente Resumen predeterminado y hemos ampliado la capacidad para incluir clientlib personalizado.

Este componente se identifica mediante la etiqueta Firmar formulario múltiple

La siguiente captura de pantalla muestra el nuevo componente que se creó para mostrar el mensaje al finalizar la ceremonia de firma

![componente de resumen](assets/summary.PNG)

El nuevo componente se basa en el componente de resumen listo para usar.
![component-prop](assets/componentprop.PNG)

Se ha añadido un botón para desplazarse al siguiente formulario para firmar
![template-code](assets/template-code.PNG)

summary.jsp tiene el siguiente código. Tiene referencia a la biblioteca de cliente identificada por el ID de categoría **getnextform**

```java
<%--
  Guide Summary Component
--%>
<%@include file="/libs/fd/af/components/guidesglobal.jsp"%>
<%@include file="/libs/fd/afaddon/components/summary/summary.jsp"%>
<ui:includeClientLib categories="getnextform"/>
```

## Assets

El componente de resumen personalizado puede ser [descargado desde aquí](assets/custom-summary-step.zip)

## Pasos siguientes

[Obtener el siguiente formulario para firmar](./create-client-lib.md)