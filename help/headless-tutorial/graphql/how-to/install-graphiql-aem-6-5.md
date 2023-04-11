---
title: Instalación de GraphiQL IDE en AEM 6.5
description: Aprenda a instalar y configurar el IDE de GraphiQL en AEM 6.5
version: 6.5
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 11614
thumbnail: KT-10253.jpeg
source-git-commit: 38a35fe6b02e9aa8c448724d2e83d1aefd8180e7
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 25%

---


# Instalación de GraphiQL IDE en AEM 6.5

En AEM 6.5, la herramienta GraphiQL IDE debe instalarse manualmente.

1. Vaya a **[Portal de distribución de software](https://experience.adobe.com/#/downloads/content/software-distribution/es-es/aemcloud.html)** > **AEM as a Cloud Service**.
1. Busque “GraphiQL” (asegúrese de incluir la **i** de **GraphiQL**).
1. Descargue el último **Paquete de contenido de GraphiQL v.x.x.x**.

   ![Descargar paquete de GraphiQL](assets/graphiql/software-distribution.png)

   El archivo zip es un paquete AEM que se puede instalar directamente.

1. En el menú Inicio de AEM, vaya a **Herramientas** > **Implementación** > **Paquetes**.
1. Haga clic en **Cargar paquete** y elija el paquete descargado en el paso anterior. Haga clic en **Instalar** para instalar el paquete.

   ![Instalación del paquete GraphiQL](assets/graphiql/install-graphiql-package.png)

1. Vaya a **CRXDE Lite** > **Panel de repositorio** > seleccione `/content/graphiql` (por ejemplo, <http://localhost:4502/crx/de/index.jsp#/content/graphiql>).
1. En el **Propiedades** cambio de ficha valor de `endpoint` propiedad a `/content/_cq_graphql/wknd-shared/endpoint.json`.
   ![Cambio de valor de propiedad de extremo](assets/graphiql/endpoint-prop-value-change.png)

1. Vaya a la **Configuración de la consola web** IU > Buscar **Filtro CSRF** (por ejemplo,<http://localhost:4502/system/console/configMgr/com.adobe.granite.csrf.impl.CSRFFilter)>
1. En el `Excluded Paths` actualización del campo nombre de propiedad, ruta de extremo WKND GraphQL a `/content/cq:graphql/wknd-shared/endpoint`.

![Excluir cambio de valor de propiedad de rutas](assets/graphiql/exclude-paths-value-change.png)

1. Acceda al editor de GraphiQL mediante `//HOST:PORT/content/graphiql.html`y compruebe que puede construir una nueva consulta o ejecutar una consulta existente. (p. ej. <http://localhost:4502/content/graphiql.html>)

![Editor de GraphiQL](assets/graphiql/graphiql-editor.png)

>[!TIP]
>
>Para admitir la ejecución de consultas y el esquema de GraphQL específicos del proyecto, debe realizar los cambios correspondientes en la variable `endpoint` y `Excluded Paths` en los pasos anteriores.
