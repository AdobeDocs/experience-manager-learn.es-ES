---
title: Servidores MCP en AEM
description: Aprenda a utilizar los servidores MCP (AEM Model Context Protocol) de sus aplicaciones preferidas basadas en chat o IDE con tecnología de IA para optimizar y acelerar el trabajo de contenido de AEM.
version: Experience Manager as a Cloud Service
role: Leader, User, Developer
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2026-03-04T00:00:00Z
jira: KT-20473
exl-id: 7f2e4e37-6440-423e-9ba9-9228fe03600b
source-git-commit: 30b98e82e78120bf9fb13c9d41780af4c07665d8
workflow-type: tm+mt
source-wordcount: '877'
ht-degree: 0%

---

# Servidores MCP en AEM

Aprenda a utilizar los servidores _Model Context Protocol (MCP) de AEM_ de sus aplicaciones preferidas basadas en chat o IDE con tecnología de IA para optimizar y acelerar su trabajo de contenido de AEM. Puede describir lo que desea en un lenguaje natural en lugar de escribir código de API de bajo nivel o navegar por la interfaz de usuario de AEM.

## Lista de servidores MCP de AEM

Todos los servidores MCP de AEM están disponibles en `https://mcp.adobeaemcloud.com/adobe/mcp/`. Consulte [Uso de MCP con AEM as a Cloud Service](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/using-mcp-with-aem-as-a-cloud-service) para obtener más información.

- **Contenido** (`/content`): acceso completo para crear, leer, actualizar y eliminar páginas, fragmentos y recursos.
- **Contenido (de solo lectura)** (`/content-readonly`): de solo lectura para enumerar y obtener páginas, fragmentos y recursos (sin cambios).
- **Cloud Manager** (`/cloudmanager`): para administrar programas, entornos, repositorios y canalizaciones de Adobe Cloud Manager.

>[!TIP]
>
>Las herramientas que expone cada servidor pueden cambiar con el tiempo. Para ver lo que está disponible ahora, pídale a su IA que enumere todas las herramientas de MCP de AEM (por ejemplo, `List all AEM MCP tools available from this server and describe what they do`) o escriba el símbolo del sistema `tools/list` en su IDE.

## Patrones de uso para el servidor MCP

Antes de empezar a utilizar los servidores MCP de AEM, vamos a comprender los dos patrones de uso principales de los servidores MCP:

- **Centrado en personas** — Estás en el asiento del conductor. Si se pregunta, la IA sugiere o ejecuta herramientas en el IDE.
- **Agente**: una aplicación auténtica (agente o subagente) llama al servidor por su cuenta, eligiendo herramientas y trabajando hacia un objetivo con poca entrada humana.

Así se comparan estos dos patrones de uso:

| Aspecto | Centrado en el ser humano | Agente |
| ------ | ------------- | ------- |
| **Quién conduce acciones** | Tú. <br> La API sugiere o ejecuta herramientas en el IDE o en una aplicación basada en chat. | La IA. <br> Elige qué herramientas usar y continúa con una guía mínima. |
| **Autoridad de decisión** | Tú manten el control. Usted aprueba o déclencheur cada paso. | La IA tiene más libertad. Las acciones de alto impacto pueden necesitar protecciones o aprobaciones. |
| **Patrón de uso típico** | **Por desarrollador**, lo usa desde su propio IDE o aplicación basada en chat, un desarrollador por sesión, ideal para el trabajo diario de desarrollo. | **Compartido** a través de una aplicación agéntica, como servicios compartidos y puertas de enlace para muchos usuarios o agentes. |
| **Más adecuado para** | Revisar contenido, realizar actualizaciones guiadas, explorar o repetir tareas mientras se mantiene al margen. | Flujos de trabajo activos, trabajos por lotes, canalizaciones y objetivos en los que el sistema debe ejecutarse con una intervención mínima. |

### Cuando se utiliza MCP en sistemas operativos

Los servidores MCP están diseñados para **clientes MCP operados por humanos** con experiencia de usuario interactiva y supervisión humana. La especificación de herramientas de MCP recomienda _un ser humano en el bucle_ que puede aprobar o rechazar invocaciones de herramientas.

Si utiliza servidores MCP en un sistema autónomo o agéntico, trátelo como un nivel de compatibilidad independiente. No codificar **los nombres de herramientas** en _indicadores_, _listas de permitidos_ o _lógica de enrutamiento_. En MCP, _tool name_ es un identificador programático, _description_ es la sugerencia orientada al modelo para LLM. Prefiera la funcionalidad o la selección basada en la descripción.

Implemente la detección de tiempo de ejecución a través de `tools/list`, controle los cambios en la lista de herramientas (`notifications/tools/list_changed`) y alinéelo con el proveedor del servidor MCP en la incorporación y el control de versiones si necesita garantías de estabilidad más allá de la línea de base del protocolo.

## Entidades de MCP y su asignación

MCP se basa en tres entidades: **host**, **cliente** y **servidor**. La [especificación MCP](https://modelcontextprotocol.io/docs/getting-started/intro) los define formalmente. Sin embargo, la siguiente tabla explica cada uno en términos simples y su asignación al utilizar servidores MCP de AEM.

| Componente | Definición estándar | Al usar servidores MCP de AEM |
| --------- | ------------------- | ---------------- |
| **Host** | La aplicación que ejecuta todo, recopila el contexto, habla con la IA, gestiona los permisos y crea clientes. | Su **IDE** (cursor) o aplicación basada en chat es el host. Ejecuta el cliente MCP y decide qué herramientas y servidores puede utilizar su sesión. |
| **Cliente** | Una sola conexión desde el host a un servidor. Pasa mensajes de un lado a otro y mantiene el acceso de ese servidor separado de los demás. | El cliente **MCP** vive en su aplicación IDE o basada en chat. Cuando agrega el servidor MCP de contenido de AEM en la configuración, el IDE o la aplicación basada en chat crea un cliente que habla con ese servidor. Sus mensajes y llamadas de herramienta pasan a través de este cliente. |
| **Servidor** | Servicio que expone herramientas, datos y avisos a través de MCP. Puede ejecutarse en su equipo o de forma remota. | Los **servidores MCP de AEM** alojados por Adobe ofrecen herramientas para crear, leer, actualizar y eliminar páginas, fragmentos de contenido y recursos para que la IA de su IDE o aplicación basada en chat pueda funcionar con su entorno de AEM. |

En pocas palabras, **Host** es su aplicación basada en IDE o en chat, **Cliente** es la conexión desde el IDE o la aplicación basada en chat a AEM, **Servidor** es el servidor MCP de AEM alojado por Adobe que hace el trabajo.

## Configuración

Los servidores MCP de AEM están diseñados para funcionar con un conjunto definido de aplicaciones compatibles con MCP.
Para configurar los servidores MCP de AEM en su IDE o aplicación basada en chat preferido, consulte [Aplicaciones MCP compatibles](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/mcp-support/using-mcp-with-aem-as-a-cloud-service#supported-mcp-applications) para obtener más información.

## Casos de uso

<!-- 
CARDS
{target = _self}

* ./accelerate-content-operations-with-aem-mcp-server.md    
  {title = Accelerate Content Operations with AEM MCP Server}
  {description = Learn how to use the AEM Content MCP Server from Cursor IDE to streamline and accelerate your AEM content work.}
  {image = ../assets/content-mcp-server/update-adventure-price-prompt-response.png}
  {cta = Learn Content MCP Server}

* ./cloud-manager.md
  {title = Cloud Manager MCP Server}
  {description = Learn how to use the AEM Cloud Manager MCP Server from Cursor IDE to streamline and accelerate your AEM cloud manager work.}
  {image = ../assets/cm-mcp-server/start-pipeline.png}
  {cta = Learn Cloud Manager MCP Server}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Accelerate Content Operations with AEM MCP Server">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./accelerate-content-operations-with-aem-mcp-server.md" title="Acelerar las operaciones de contenido con AEM MCP Server" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/content-mcp-server/update-adventure-price-prompt-response.png" alt="Acelerar las operaciones de contenido con AEM MCP Server"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./accelerate-content-operations-with-aem-mcp-server.md" target="_self" rel="referrer" title="Acelerar las operaciones de contenido con AEM MCP Server">Acelerar las operaciones de contenido con AEM MCP Server</a>
                    </p>
                    <p class="is-size-6">Aprenda a utilizar AEM Content MCP Server desde Cursor IDE para optimizar y acelerar su trabajo de contenido de AEM.</p>
                </div>
                <a href="./accelerate-content-operations-with-aem-mcp-server.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Servidor MCP de contenido de aprendizaje</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Cloud Manager MCP Server">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./cloud-manager.md" title="Servidor MCP de Cloud Manager" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/cm-mcp-server/start-pipeline.png" alt="Servidor MCP de Cloud Manager"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./cloud-manager.md" target="_self" rel="referrer" title="Servidor MCP de Cloud Manager">Servidor MCP de Cloud Manager</a>
                    </p>
                    <p class="is-size-6">Aprenda a utilizar el servidor MCP de AEM Cloud Manager desde el IDE del cursor para optimizar y acelerar su trabajo de Cloud Manager de AEM.</p>
                </div>
                <a href="./cloud-manager.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Conozca Cloud Manager MCP Server</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
