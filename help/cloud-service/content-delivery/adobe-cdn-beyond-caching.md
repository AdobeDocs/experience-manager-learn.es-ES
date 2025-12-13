---
title: 'CDN de Adobe: funciones avanzadas más allá del almacenamiento en caché'
description: Obtenga información acerca de las funciones avanzadas de Adobe CDN más allá del almacenamiento en caché, como configurar el tráfico en la CDN, configurar tokens y credenciales, páginas de error de CDN y más.
version: Experience Manager as a Cloud Service
feature: Website Performance, CDN Cache
topic: Architecture, Performance, Content Management
role: Developer, User, Leader
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2024-08-21T00:00:00Z
jira: KT-15123
thumbnail: KT-15123.jpeg
exl-id: 8948a900-01e9-49ed-9ce5-3a057f5077e4
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '546'
ht-degree: 0%

---

# CDN de Adobe: funciones avanzadas más allá del almacenamiento en caché

Obtenga información acerca de las funciones avanzadas de la red de distribución de contenido (CDN) de Adobe más allá del almacenamiento en caché, como configurar el tráfico en la CDN, configurar tokens y credenciales, páginas de error de CDN y más.

Más allá del almacenamiento en caché de contenido, Adobe CDN ofrece varias funciones avanzadas que pueden ayudar a optimizar el rendimiento de su sitio web. Estas funciones incluyen:

- Configuración del tráfico en la CDN
- Configuración de las credenciales y la autenticación de CDN
- Páginas de error de CDN

Estas características son **características de autoservicio**. Configurado en el archivo `cdn.yaml` de su proyecto de AEM e implementado mediante la canalización de configuración de Cloud Manager.

>[!VIDEO](https://video.tv.adobe.com/v/3440272?captions=spa&quality=12&learn=on)

## Configuración del tráfico en la CDN

Vamos a comprender las funcionalidades clave relacionadas con _Configurar el tráfico en CDN_:

- **Prevención de ataques DoS:** Adobe CDN absorbe los ataques DoS en el nivel de red, lo que impide que lleguen al servidor de origen.
- **Limitación de velocidad:** Para proteger el servidor de origen de que se vea abrumado por demasiadas solicitudes, puede configurar la limitación de velocidad en la CDN.
- **Firewall de aplicaciones web (WAF):** WAF protege su sitio web de vulnerabilidades comunes de aplicaciones web, como inyección de SQL, scripts entre sitios y mucho más. Se requiere la licencia de seguridad mejorada o la licencia de protección WAF-DDoS para utilizar esta función.
- **Transformación de solicitudes:** Modifique las solicitudes entrantes, como la configuración o desconfiguración de encabezados, la modificación de parámetros de consulta, las cookies y mucho más.
- **Transformación de respuesta:** Modifique las respuestas salientes, como la configuración o desconfiguración de encabezados.
- **Selección de origen:** enrute el tráfico a diferentes servidores de origen (Adobe y no Adobe) según la dirección URL de la solicitud.
- **Redireccionamiento de dirección URL:** Redireccionar solicitudes (HTTP 301/302) a una dirección URL absoluta o relativa diferente.

## Configuración de las credenciales y la autenticación de CDN

Vamos a comprender las funcionalidades clave relacionadas con _Configuración de las credenciales y la autenticación de CDN_:

- **Token de API de purga**: le permite crear su propia clave de purga para purgar un solo recurso, un grupo o todos los recursos de la caché.
- **Autenticación básica**: Un mecanismo de autenticación ligero cuando desea restringir el acceso al sitio web o a una parte de él. Principalmente, se requiere como parte de varios procesos de revisión antes de entrar en funcionamiento.
- **Validación de encabezado HTTP**: se usa cuando una CDN administrada por el cliente enruta el tráfico a Adobe CDN. La CDN de Adobe valida la solicitud entrante en función del valor del encabezado `X-AEM-Edge-Key`. Le permite crear su propio valor para el encabezado `X-AEM-Edge-Key`.

## Páginas de error de CDN

Vamos a comprender las funcionalidades clave relacionadas con _páginas de error de CDN_:

- **Páginas de error de marca**: muestra una página de error de marca a los usuarios en el _escenario improbable_ cuando la CDN de Adobe no puede llegar al servidor de origen.

## Cómo implementar

La implementación de estas funciones avanzadas implica dos pasos:

1. **Actualizar archivo de configuración de CDN**: actualice el archivo `cdn.yaml` en su proyecto de AEM con las configuraciones requeridas. Las configuraciones se añaden como reglas y siguen una sintaxis de regla. Los tres componentes principales de la regla: `name`, `when` y `action`.

2. **Implementar archivo de configuración de CDN**: Implemente el archivo `cdn.yaml` actualizado mediante la canalización de configuración de Cloud Manager. Para obtener más información, consulte [Implementar reglas a través de Cloud Manager](https://experienceleague.adobe.com/es/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager).

### Ejemplo

En el ejemplo siguiente, el sitio WKND de ejemplo está configurado para redirigir la dirección URL `/top3` a `/us/en/top3.html`.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  redirects:
    rules:
      - name: redirect-top3-adventures
        when: { reqProperty: path, equals: "/top3" }
        action:
          type: redirect
          status: 302
          location: /us/en/top3.html
```

## Tutoriales relacionados

[Protección de sitios web con reglas de filtro de tráfico](https://experienceleague.adobe.com/es/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/overview)

[Configurar e implementar la regla CDN de validación de encabezado HTTP](https://experienceleague.adobe.com/es/docs/experience-manager-learn/cloud-service/content-delivery/custom-domain-names-with-customer-managed-cdn#configure-and-deploy-http-header-validation-cdn-rule)

[Cómo purgar la caché de CDN](https://experienceleague.adobe.com/es/docs/experience-manager-learn/cloud-service/caching/how-to/purge-cache)

[Configurar páginas de error de CDN](https://experienceleague.adobe.com/es/docs/experience-manager-learn/cloud-service/content-delivery/custom-error-pages#cdn-error-pages)

[Configuración del tráfico en la red de distribución de contenido](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#client-side-redirectors)

[Configurar credenciales y autenticación de CDN](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-credentials-authentication)

