---
title: Usuarios de servicio
description: Aprenda a crear y utilizar usuarios de servicios en su código de AEM para proporcionar acceso controlado y programático al repositorio de AEM.
version: Cloud Service
topic: Development
feature: OSGI, Security
role: Developer
level: Intermediate
kt: 9113
thumbnail: 337530.jpeg
source-git-commit: 5452ab85523fc10d0aaa55e95d42c37ec33fd2ed
workflow-type: tm+mt
source-wordcount: '70'
ht-degree: 2%

---


# Usuarios de servicio

Aprenda a crear y utilizar usuarios de servicios en su código de AEM para proporcionar acceso controlado y programático al repositorio de AEM.

>[!VIDEO](https://video.tv.adobe.com/v/337530/?quality=12&learn=on)

## Medios

+ [Documentación sobre la inicialización del repositorio de Sling (repositorio)](https://sling.apache.org/documentation/bundles/repository-initialization.html)
+ [Documentación de autenticación del servicio Sling](https://sling.apache.org/documentation/the-sling-engine/service-authentication.html)

## Código

### ContentStatisticsImpl.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/statistics/impl/ContentStatisticsImpl.java`

```java
package com.adobe.aem.wknd.examples.core.statistics.impl;

import com.adobe.aem.wknd.examples.core.statistics.ContentStatistics;
import org.apache.sling.api.resource.LoginException;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.apache.sling.api.resource.ResourceResolverFactory;
import org.apache.sling.serviceusermapping.ServiceUserMapped;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.jcr.query.Query;
import java.util.Collections;
import java.util.Iterator;
import java.util.Map;

@Component(
        reference = {
                @Reference(
                        name = SERVICE_USER_SUB_SERVICE_ID,
                        service = ServiceUserMapped.class,
                        target = "(subServiceName=wknd-examples-statistics)"
                )
        }
)
public class ContentStatisticsImpl implements ContentStatistics {
    private static final Logger log = LoggerFactory.getLogger(ContentStatisticsImpl.class);

    @Reference
    private ResourceResolverFactory resourceResolverFactory;

    @Override
    public int getAssetsCount(final ResourceResolver resourceResolver) {
        return getNumberOfAssets(resourceResolver);
    }

    @Override
    public int getAllAssetsCount() {
        // Create the Map that specifies the SubService ID
        final Map<String, Object> authInfo = Collections.singletonMap(
                ResourceResolverFactory.SUBSERVICE,
                "wknd-examples-statistics");

        // Get the auto-closing Service resource resolver
        // Remember, any ResourceResolver you get, you must ensure is closed!
        try (ResourceResolver serviceResolver = resourceResolverFactory.getServiceResourceResolver(authInfo)) {
            if (serviceResolver != null) {
                // Do some work w your service resource resolver
                return queryAndCountAssets(serviceResolver);
            } else {
                log.warn("Could not obtain a User for the Service: {} ", SERVICE_USER_SUB_SERVICE_ID);
            }
        } catch (LoginException e) {
            log.error("Login Exception when obtaining a User for the Bundle Service: {} ", e.getMessage());
        }

        return -1;
    }

    private int queryAndCountAssets(ResourceResolver resourceResolver) {
        final String ASSETS_QUERY = "SELECT * FROM [dam:Asset] WHERE isdescendantnode(\"/content/dam\")";

        Iterator<Resource> resources = resourceResolver.findResources(ASSETS_QUERY, Query.JCR_SQL2);

        int count = 0;
        while (resources.hasNext()) { count++; resources.next(); }

        log.info("User [ {} ] found [ {} ] assets", resourceResolver.getUserID(), count);

        return count;
    }

    @Activate
    protected void activate() {
        // We can use service users in the context's where no natural Sling security context is available to us,
        // which is usually outside the context of a Sling HTTP request.
        log.debug("Begin calling getAllAssetsCount() from the @Activate method");
        int count = getAllAssetsCount();
        log.debug("Finished calling getAllAssetsCount() from the @Activate method with count [ {} ]", count);

    }
}
```

### org.apache.sling.jcr.repoinit.RepositoryInitializer-wknd-examples-statistics.config

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.author/org.apache.sling.jcr.repoinit.RepositoryInitializer-wknd-examples-statistics.config`

```
scripts=["
    create service user wknd-examples-statistics-service with forced path system/cq:services/wknd-examples

    set principal ACL for wknd-examples-statistics-service
        allow jcr:read on /content/dam
    end
"]
```

### org.apache.sling.serviceusermapping.impl.ServiceUserMapperImpl.amended-wknd-examples.cfg.json

`/ui.config/src/main/content/jcr_root/apps/wknd-examples/osgiconfig/config.author/org.apache.sling.serviceusermapping.impl.ServiceUserMapperImpl.amended-wknd-examples.cfg.json`

```javascript
{
  "user.mapping": [
    "wknd-examples.core:wknd-examples-statistics=[wknd-examples-statistics-service]"
  ]
}
```