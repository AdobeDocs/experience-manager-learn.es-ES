---
title: Personalizar componente de resumen
description: Amplíe el componente Paso de resumen para incluir la capacidad de desplazarse al siguiente formulario del paquete.
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6894
thumbnail: 6894.jpg
topic: Development
role: Developer
level: Experienced
exl-id: fb68579d-241c-414d-92f4-13194f4d1923
duration: 38
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 1%

---

# Personalizar paso de resumen

El componente Paso de resumen se utiliza para mostrar el resumen del envío del formulario con un vínculo para descargar el formulario firmado. El paso de resumen generalmente se coloca en el último panel del formulario.
Para el propósito de este caso de uso, hemos creado un nuevo componente basado en el componente Resumen predeterminado y hemos ampliado la capacidad para incluir clientlib personalizado.

Este componente se identifica con la etiqueta Firmar formulario múltiple

La siguiente captura de pantalla muestra el nuevo componente que se creó para mostrar el mensaje al finalizar la ceremonia de firma

![componente de resumen](assets/summary.PNG)

El nuevo componente se basa en el componente de resumen predeterminado.
![componente-prop](assets/componentprop.PNG)

Se ha agregado un botón para ir al siguiente formulario que se debe firmar
![código de plantilla](assets/template-code.PNG)

summary.jsp tiene el siguiente código. Tiene referencia a la biblioteca de cliente identificada por el id. de categoría **getnextform**

```java
<%--
  Guide Summary Component
--%>
<%@include file="/libs/fd/af/components/guidesglobal.jsp"%>
<%@include file="/libs/fd/afaddon/components/summary/summary.jsp"%>
<ui:includeClientLib categories="getnextform"/>
```

## Recursos

El componente de resumen personalizado se puede [descargar desde aquí](assets/custom-summary-step.zip)

## Siguientes pasos

[Obtener el siguiente formulario para firmar](./create-client-lib.md)