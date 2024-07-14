---
title: Ciclo de vida del componente OSGi
description: Obtenga información acerca del ciclo vital del componente OSGi, incluido cómo enlazar un servicio OSGi a los eventos de ciclo vital de activación, modificación y desactivación.
role: Developer
level: Beginner
topic: Development
feature: OSGI
jira: KT-8228
thumbnail: 335475.jpeg
exl-id: 5a65dbcd-649b-464c-9c78-d31c2b6c49c3
duration: 377
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '58'
ht-degree: 6%

---

# Ciclo de vida del componente OSGi

Obtenga información acerca del ciclo vital del componente OSGi, incluido cómo enlazar un servicio OSGi a:

+ Activar
+ Modificado
+ y Desactivar

...eventos de ciclo vital.

>[!VIDEO](https://video.tv.adobe.com/v/335475?quality=12&learn=on)

## Recursos

+ [@Activate documentos Java](https://javadoc.io/static/com.adobe.aem/aem-sdk-api/2021.7.5658.20210723T140305Z-210600/org/osgi/service/component/annotations/Activate.html)
+ [@Modified documentos Java](https://javadoc.io/static/com.adobe.aem/aem-sdk-api/2021.7.5658.20210723T140305Z-210600/org/osgi/service/component/annotations/Modified.html)
+ [@Deactivate documentos Java](https://javadoc.io/static/com.adobe.aem/aem-sdk-api/2021.7.5658.20210723T140305Z-210600/org/osgi/service/component/annotations/Deactivate.html)

## Código

### ActivitiesImpl.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/adventures/impl/ActivitiesImpl.java`

```java
package com.adobe.aem.wknd.examples.core.adventures.impl;

import java.util.Random;

import com.adobe.aem.wknd.examples.core.adventures.Activities;

import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Deactivate;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(
    service = { Activities.class }
)
public class ActivitiesImpl implements Activities {
    private static final Logger log = LoggerFactory.getLogger(ActivitiesImpl.class);

    private String[] activities;

    private final Random random = new Random();

    /**
     * @return the name of a random WKND adventure activity
     */
    public String getRandomActivity() {
        int randomIndex = random.nextInt(activities.length);
        return activities[randomIndex];
    }    

    @Activate
    protected void activate() {
        this.activities = new String[] { 
            "Running", "Cycling",  "Skateboarding"
        };

        log.info("Activated ActivitiesImpl with activities [ {} ]", String.join(", ", this.activities));
    }

    @Deactivate
    protected void deactivate() {
        log.info("ActivitiesImpl has been deactivated!");
    }
}
```
