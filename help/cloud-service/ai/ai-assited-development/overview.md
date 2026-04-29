---
title: Desarrollo asistido por IA
description: Obtenga información sobre el desarrollo asistido por IA que utiliza un IDE o agentes de codificación con tecnología de IA junto con AGENTS.md, habilidades de agente y servidores MCP para ayudar a producir código de alta calidad y listo para la producción para proyectos en AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Developer Tools
role: Developer, Architect
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2026-04-24T00:00:00Z
jira: KT-20899
thumbnail: KT-20899.pngKT-20899
source-git-commit: e3ef450cfe9005ba940ff1897c216681654341b3
workflow-type: tm+mt
source-wordcount: '906'
ht-degree: 0%

---


# Desarrollo asistido por IA

El desarrollo asistido por IA utiliza un IDE o agentes de codificación con tecnología de IA junto con `AGENTS.md`, habilidades de agente y servidores MCP para ayudar a producir código de alta calidad y listo para la producción para proyectos de AEM as a Cloud Service.

Herramientas como [Cursor](https://www.cursor.com/), [Copiloto de GitHub en Visual Studio Code](https://code.visualstudio.com/docs/copilot/overview), [Código Claude](https://code.claude.com/docs/en/overview) y IDE similares con tecnología de IA y agentes de codificación ayudan de varias formas clave:

- **Iteración más rápida**: genere o refactorice el código a partir de las indicaciones de lenguaje natural que describan la característica o el cambio deseados.
- **Material de aprendizaje**: explique rutas de código, configuraciones, conceptos o prácticas recomendadas desconocidas cuando se le solicite.

Sin embargo, estos beneficios dependen en gran medida del _contexto disponible para el agente de codificación_. Los datos genéricos de formación y una sola instantánea del repositorio _no suelen ser suficientes_ para producir de forma fiable código AEM listo para la producción.

## Por qué la IA por sí sola es insuficiente

Sin el contexto adecuado, los modelos de IA (a través de un IDE con tecnología de IA o agente de codificación) pueden:

- **Alucinar API o ciclos de vida**: sugiera código o configuraciones que no se alineen con las prácticas recomendadas o las características más recientes de AEM as a Cloud Service.
- **Pasos sin curso**: omita los pasos necesarios que no estén visibles en el repositorio de código o en los datos de formación.
- **Derivar de los estándares del proyecto**: ignore los patrones establecidos para componentes, servicios OSGi, flujos de trabajo o configuración de Dispatcher.

En esta brecha es donde el _contexto estructurado_ (habilidades del agente y AGENTES.md) y la _visibilidad en tiempo de ejecución_ (servidores MCP) se vuelven esenciales para que el desarrollo asistido por IA sea _productivo_ y _confiable_.

## Cómo ayuda Adobe con el desarrollo asistido por IA

Para los proyectos de AEM as a Cloud Service, Adobe proporciona lo siguiente:

- Aptitudes de agente y AGENTS.md a través de [Adobe Skills for AI Coding Agents](https://github.com/adobe/skills)
- Servidores MCP locales para AEM SDK y Dispatcher local a través del portal [Distribución de software](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=mcp*&1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&1_group.propertyvalues.operation=equals&1_group.propertyvalues.0_values=software-type%3Atooling&orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&orderby.sort=desc&layout=list&p.offset=0&p.limit=3)
- Servidores MCP de AEM alojados por Adobe para flujos de trabajo de contenido y Cloud Manager desde su IDE o aplicación de chat; consulte [Servidores MCP en AEM](../mcp/overview.md)

Las siguientes secciones resumen cada elemento. Utilice las secciones **Configuración** y **Casos de uso** al final de esta página para la instalación y tutoriales para el desarrollo asistido por IA.

## ¿Qué son las aptitudes de agente?

Las habilidades de agente son _conocimiento o experiencia en procedimientos_ para ayudar a los agentes de codificación _a realizar un trabajo real de forma fiable_. Para obtener más información, consulte [Aptitudes de agente](https://agentskills.io).

Para un proyecto de AEM as a Cloud Service, las aptitudes de agente están disponibles en el repositorio de [Aptitudes de Adobe para agentes de codificación de IA](https://github.com/adobe/skills).

## Qué es AGENTS.md

AGENTS.md proporciona el _contexto e instrucciones_ para ayudar a los agentes de codificación _a trabajar en su proyecto_. Para obtener más información, consulte [AGENTS.md](https://agents.md/).

Para un proyecto de AEM as a Cloud Service, la aptitud de arranque de `ensure-agents-md` crea **AGENTS.md** en la raíz del repositorio **cuando falta**. La aptitud inspecciona el proyecto (por ejemplo, la raíz `pom.xml` y los módulos) y genera directrices adaptadas en lugar de utilizar un archivo estático. Si **AGENTS.md** ya existe, **no** se sobrescribirá.

Una vez que exista el archivo, puede editarlo para agregar más contexto e instrucciones para las prácticas recomendadas de su equipo u organización. La aptitud también puede crear **CLAUDE.md** que hace referencia a **AGENTS.md**, de modo que las herramientas basadas en Claude recojan la misma guía.

## Qué son los servidores MCP

Los servidores MCP exponen herramientas y datos al agente de codificación a través del [Protocolo de contexto de modelo](https://modelcontextprotocol.io/), que admite acciones como la depuración, la inspección, la ejecución y la validación de cambios. Un servidor MCP se puede ejecutar en su estación de trabajo (**local**) o como servicio hospedado (**remoto**).

Para **desarrollo local** con AEM SDK y Dispatcher, instale estos **servidores MCP locales** desde el portal [Distribución de software](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=mcp*&1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&1_group.propertyvalues.operation=equals&1_group.propertyvalues.0_values=software-type%3Atooling&orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&orderby.sort=desc&layout=list&p.offset=0&p.limit=3):

- **AEM Quickstart Local MCP server**: Exposes live runtime data from a local AEM SDK instance to support troubleshooting and development. For more information, see [AEM Quickstart MCP Server](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools#aem-quickstart-mcp-server).
- **Dispatcher Local MCP server**: Enables runtime validation and inspection of a local Dispatcher instance. For more information, see [Dispatcher MCP Server](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools#dispatcher-mcp-server).

For Adobe-hosted AEM MCP servers (for example, content, read-only content, and Cloud Manager), see [MCP Servers in AEM](../mcp/overview.md).

## Configuración

<!-- 
CARDS
{target = _self}

* ./setup/agent-skills.md
    {title = Set up AEM Agent Skills}
    {description = Learn how to set up AEM Agent Skills for AI-assisted development.}
    {image = ./assets/agent-skills/select-aem-agent-skills-to-install.png}
    {cta = Install AEM Agent Skills}

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Set up AEM Agent Skills">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup/agent-skills.md" title="Set up AEM Agent Skills" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/agent-skills/select-aem-agent-skills-to-install.png" alt="Set up AEM Agent Skills"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup/agent-skills.md" target="_self" rel="referrer" title="Set up AEM Agent Skills">Set up AEM Agent Skills</a>
                    </p>
                    <p class="is-size-6">Learn how to set up AEM Agent Skills for AI-assisted development.</p>
                </div>
                <a href="./setup/agent-skills.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Install AEM Agent Skills</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Casos de uso

<!-- 
CARDS
{target = _self}

* ./use-cases/component-development.md    
    {title = Create AEM Component with AI-assisted development}
    {description = Learn how to use AI-assisted development to develop AEM components.}
    {image = ./assets/component-development/review-generated-code.png}
    {cta = Create AEM Component}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create AEM Component with AI-assisted development">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/component-development.md" title="Create AEM Component with AI-assisted development" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/component-development/review-generated-code.png" alt="Create AEM Component with AI-assisted development"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/component-development.md" target="_self" rel="referrer" title="Create AEM Component with AI-assisted development">Create AEM Component with AI-assisted development</a>
                    </p>
                    <p class="is-size-6">Learn how to use AI-assisted development to develop AEM components.</p>
                </div>
                <a href="./use-cases/component-development.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Create AEM Component</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## Recursos adicionales

- [Local development with AI Tools](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools)

- [Adobe Skills for AI Coding Agents](https://github.com/adobe/skills)

- [AGENTS.md](https://agents.md/)

- [Agent Skills](https://agentskills.io/home)