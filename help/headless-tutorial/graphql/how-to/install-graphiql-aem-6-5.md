---
title: AEM Instalación del IDE de GraphiQL en 6.5
description: AEM Obtenga información sobre cómo instalar y configurar el IDE de GraphiQL en 6.5
version: 6.5
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
jira: KT-11614
thumbnail: KT-10253.jpeg
exl-id: 04fcc24c-7433-4443-a109-f01840ef1a89
duration: 68
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 14%

---

# AEM Instalación del IDE de GraphiQL en 6.5

AEM En la versión 6.5, la herramienta IDE de GraphiQL debe instalarse de forma manual.

1. Vaya a **[Portal de distribución de software](https://experience.adobe.com/#/downloads/content/software-distribution/es-es/aemcloud.html)** > **AEM as a Cloud Service**.
1. Busque &quot;GraphiQL&quot; (asegúrese de incluir la variable **i** in **GraphiQL**).
1. Descargue la última versión **Paquete de contenido de GraphiQL v.x.x.x**.

   ![Descargar paquete de GraphiQL](assets/graphiql/software-distribution.png)

   AEM El archivo zip es un paquete de que se puede instalar directamente.

1. AEM En el menú Inicio de la, vaya a **Herramientas** > **Implementación** > **Paquetes**.
1. Haga clic en **Cargar paquete** y elija el paquete descargado en el paso anterior. Haga clic en **Instalar** para instalar el paquete.

   ![Instalación del paquete de GraphiQL](assets/graphiql/install-graphiql-package.png)

1. Vaya a **CRXDE Lite** > **Panel Repositorio** > seleccionar `/content/graphiql` nodo (por ejemplo, <http://localhost:4502/crx/de/index.jsp#/content/graphiql>).
1. En el **Propiedades** cambiar el valor de tabulación de `endpoint` propiedad a `/content/_cq_graphql/wknd-shared/endpoint.json`.
   ![Cambio de valor de propiedad de extremo](assets/graphiql/endpoint-prop-value-change.png)

1. Vaya a **Configuración de la consola web** IU > Buscar **Filtro CSRF** (por ejemplo,<http://localhost:4502/system/console/configMgr/com.adobe.granite.csrf.impl.CSRFFilter)>
1. En el `Excluded Paths` actualización del campo de nombre de propiedad, la ruta del extremo de WKND GraphQL a `/content/cq:graphql/wknd-shared/endpoint`.

![Cambio de valor de propiedad Excluir rutas](assets/graphiql/exclude-paths-value-change.png)

1. Acceso al editor de GraphiQL mediante `//HOST:PORT/content/graphiql.html`y verificar que puede construir una nueva consulta o ejecutar una existente. (p. ej <http://localhost:4502/content/graphiql.html>)

![Editor de GraphiQL](assets/graphiql/graphiql-editor.png)

>[!TIP]
>
>Para admitir la ejecución de consultas y esquemas GraphQL específicos del proyecto, debe realizar los cambios correspondientes para la variable `endpoint` y `Excluded Paths` valores en los pasos anteriores.
