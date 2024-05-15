---
title: Rellenar previamente formularios adaptables basados en componentes principales
description: Aprenda a rellenar previamente formularios adaptables con datos
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14675
duration: 23
exl-id: a94deebd-e86e-4360-b0ed-193f13197ee2
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '119'
ht-degree: 0%

---

# Prueba de la solución

Una vez implementado el código, cree un formulario adaptable basado en los componentes principales. Asocie el formulario adaptable al servicio de relleno previo como se muestra en la captura de pantalla siguiente.
![prefill-service](assets/pre-fill-service.png)

Cada vez que se procesa el formulario, se ejecuta el servicio de rellenado previo asociado y el formulario se rellena con los datos devueltos por el servicio de rellenado previo.

Por ejemplo, para rellenar previamente el formulario con los datos asociados al GUID **d815a2b3-5f4c-4422-8197-d0b73479bf0e**, se utiliza la siguiente URL.
El código del servicio de relleno previo extraerá el valor del parámetro guid y recuperará los datos asociados con el guid de la fuente de datos.

```html
http://localhost:4502/content/dam/formsanddocuments/contactus/jcr:content?wcmmode=disabled&guid=d815a2b3-5f4c-4422-8197-d0b73479bf0e
```
