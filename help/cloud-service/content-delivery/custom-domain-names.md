---
title: Opciones de nombre de dominio personalizadas
description: Obtenga información sobre cómo administrar e implementar nombres de dominio personalizados para su sitio web alojado en AEM as a Cloud Service.
version: Cloud Service
feature: Cloud Manager, Custom Domain Names
topic: Architecture, Migration
role: Admin, Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 130
last-substantial-update: 2024-08-09T00:00:00Z
jira: KT-15946
thumbnail: KT-15946.jpeg
source-git-commit: 07225f1ae4455e2fa69c8e488851361c725fe9e8
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 1%

---

# Opciones de nombre de dominio personalizadas

Obtenga información sobre cómo administrar e implementar nombres de dominio para su sitio web alojado en AEM as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/3432632?quality=12&learn=on)

## Antes de empezar

Antes de empezar a implementar nombres de dominio personalizados, asegúrese de comprender los siguientes conceptos:

### Qué es un nombre de dominio

Un nombre de dominio es el nombre de sitio web de nombres descriptivos, como adobe.com, que señala a una ubicación específica (dirección IP como 170.2.14.16) en Internet.

### Nombres de dominio predeterminados en AEM as a Cloud Service

De manera predeterminada, AEM as a Cloud Service se aprovisiona con un nombre de dominio predeterminado, que termina en `*.adobeaemcloud.com`. El certificado SSL de comodines emitido contra `*.adobeaemcloud.com` se aplica automáticamente a todos los entornos y este certificado de comodines es responsabilidad del Adobe.

Los nombres de dominio predeterminados tienen el formato `https://<SERVICE-TYPE>-p<PROGRAM-ID>-e<ENVIRONMENT-ID>.adobeaemcloud.com`.

- `<SERVICE-TYPE>` puede ser **author**, **publish** o **preview**.
- `<PROGRAM-ID>` es el identificador único del programa. Una organización puede tener varios programas.
- `<ENVIRONMENT-ID>` es el identificador único del entorno y cada programa contiene cuatro entornos: **Desarrollo rápido (RDE)**, **dev**, **stage** y **prod**. Cada entorno contiene los tres tipos de servicio mencionados anteriormente, excepto **RDE** que no tiene un entorno de vista previa.

En resumen, una vez aprovisionados todos los entornos de AEM as a Cloud Service, tiene **11** (RDE no tiene un entorno de vista previa) direcciones URL únicas combinadas con el nombre de dominio predeterminado.

### CDN administrada por Adobe frente a CDN administrada por cliente

Para reducir la latencia y mejorar el rendimiento del sitio web, AEM as a Cloud Service está integrado con una red de distribución de contenido (CDN) administrada por el Adobe. La CDN administrada por Adobe se habilita automáticamente para todos los entornos. Consulte [Almacenamiento en caché de AEM as a Cloud Service](../caching/overview.md) para obtener más información.

Sin embargo, los clientes también pueden usar su propia CDN, conocida como **CDN administrada por el cliente**. No es necesario, pero pocos clientes lo utilizan para políticas corporativas u otras razones. En este caso, el cliente es responsable de administrar las configuraciones y los ajustes de CDN.

### Nombres de dominio personalizados

Los nombres de dominio personalizados siempre se prefieren sobre los nombres de dominio predeterminados para fines de marca, autenticidad y desarrollo empresarial. Sin embargo, solo se pueden aplicar a los tipos de servicio **publish** y **preview**, y no a **author**.

Al agregar nombres de dominio personalizados, debe proporcionar un certificado SSL válido para el dominio personalizado dado. El certificado SSL debe ser un certificado válido firmado por una entidad emisora de certificados (CA) de confianza.

Normalmente, los clientes utilizan un nombre de dominio personalizado para entornos Prod (sitio web de AEM as a Cloud Service) y, a veces, para entornos inferiores como **stage** o **dev**.

| AEM tipo de servicio de | ¿Se admite el dominio personalizado? |
|---------------------|:-----------------------:|
| Autor | ✘ |
| Vista previa | ✔ |
| Publicación | ✔ |

## Implementación de nombres de dominio

Para implementar nombres de dominio mediante CDN administrada por Adobe o CDN administrada por cliente, el siguiente diagrama de flujo le guía a través del proceso:

![Diagrama de flujo de administración de nombres de dominio](./assets/domain-name-management-flowchart.png){width="800" zoomable="yes"}

Además, la siguiente tabla le guía dónde administrar las configuraciones específicas:

| Nombre de dominio personalizado con | Añadir certificado SSL a | Agregar nombre de dominio a | Configurar registros DNS en | ¿Necesita la regla CDN de validación de encabezado HTTP? |
|---------------------|:-----------------------:|-----------------------:|-----------------------:|-----------------------:|
| CDN administrado por Adobe | Adobe Cloud Manager | Adobe Cloud Manager | Servicio de alojamiento DNS | ✘ |
| CDN administrado por el cliente | Proveedor de CDN | Proveedor de CDN | Servicio de alojamiento DNS | ✔ |

### Tutoriales paso a paso

Ahora que comprende el proceso de administración de nombres de dominio, puede implementar nombres de dominio personalizados para su sitio web de AEM as a Cloud Service siguiendo los tutoriales a continuación:

**[Nombres de dominio personalizados con CDN administrados por Adobe](./custom-domain-name-with-adobe-managed-cdn.md)**: En este tutorial, aprenderá a agregar un nombre de dominio personalizado a un sitio web de **AEM as a Cloud Service con CDN administrado por Adobe**.
**[Nombres de dominio personalizados con CDN administrado por el cliente](./custom-domain-names-with-customer-managed-cdn.md)**: En este tutorial, aprenderá a agregar un nombre de dominio personalizado a un sitio web de **AEM as a Cloud Service con CDN administrado por el cliente**.

