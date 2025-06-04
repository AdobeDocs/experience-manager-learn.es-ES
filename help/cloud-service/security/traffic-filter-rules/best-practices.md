---
title: Prácticas recomendadas para las reglas de filtrado de tráfico, incluidas las reglas WAF
description: Conozca las prácticas recomendadas para las reglas de filtrado de tráfico, incluidas las reglas WAF.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-26T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
exl-id: 4a7acdd2-f442-44ee-8560-f9cb64436acf
duration: 170
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '411'
ht-degree: 100%

---

# Prácticas recomendadas para las reglas de filtrado de tráfico, incluidas las reglas WAF

Conozca las prácticas recomendadas para las reglas de filtrado de tráfico, incluidas las reglas WAF. Es importante señalar que las prácticas recomendadas descritas en este artículo no son exhaustivas y no pretenden sustituir a sus propias políticas y procedimientos de seguridad.

>[!VIDEO](https://video.tv.adobe.com/v/3425408?quality=12&learn=on)

## Prácticas recomendadas generales

- Para determinar qué reglas son apropiadas para su organización, colabore con su equipo de seguridad.
- Pruebe siempre las reglas en entornos de desarrollo antes de implementarlas en entornos de ensayo y producción.
- Al declarar y validar reglas, empiece con `action` tipo `log` para asegurarse de que la regla no bloquee el tráfico legítimo.
- Para ciertas reglas, la transición de `log` a `block` debe basarse exclusivamente en el análisis de tráfico de sitio suficiente.
- Introduzca reglas de forma incremental y considere la posibilidad de incluir a sus equipos de prueba (control de calidad, rendimiento, pruebas de penetración) en el proceso.
- Analice el impacto de las reglas con regularidad mediante la [herramienta de tablero](https://github.com/adobe/AEMCS-CDN-Log-Analysis-Tooling). Según el volumen de tráfico del sitio, el análisis se puede realizar diariamente, semanalmente o mensualmente.
- Para bloquear el tráfico malintencionado que pueda detectar después del análisis, añada reglas adicionales. Por ejemplo, ciertas direcciones IP que han estado atacando el sitio.
- La creación, la implementación y el análisis de reglas deben ser un proceso continuo e iterativo. No se trata de una actividad aislada.

## Prácticas recomendadas para reglas de filtros de tráfico

Habilite las siguientes reglas de filtro de tráfico para su proyecto de AEM. Sin embargo, los valores deseados para las propiedades `rateLimit` y `clientCountry` deben determinarse en colaboración con el equipo de seguridad.

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
    #  Prevent DoS attacks by blocking client for 5 minutes if they make more than 100 requests in 1 second.
      - name: prevent-dos-attacks
        when:
          reqProperty: path
          like: '*'
        rateLimit:
          limit: 100
          window: 1
          penalty: 300
          groupBy:
            - reqProperty: clientIp
        action: block
    # Block requests coming from OFAC countries
      - name: block-ofac-countries
        when:
          allOf:
              - reqProperty: tier
              - matches: publish
              - reqProperty: clientCountry
                in:
                  - SY
                  - BY
                  - MM
                  - KP
                  - IQ
                  - CD
                  - SD
                  - IR
                  - LR
                  - ZW
                  - CU
                  - CI
```

>[!WARNING]
>
>Para su entorno de producción, colabore con su equipo de seguridad web para determinar los valores apropiados para `rateLimit`

## Prácticas recomendadas para reglas WAF

Una vez que WAF tiene licencia y está habilitado para su programa, las marcas de WAF que coinciden con el tráfico aparecen en los gráficos y los registros de solicitudes, aunque no los haya declarado en una regla. Por lo tanto, siempre es consciente del tráfico malicioso potencialmente nuevo y puede crear reglas según sea necesario. Observe los indicadores de WAF que no se reflejan en las reglas declaradas y considere la posibilidad de declararlos.

Tenga en cuenta las siguientes reglas WAF para su proyecto de AEM. Sin embargo, los valores deseados para la propiedad `action` y `wafFlags` deben determinarse en colaboración con el equipo de seguridad.

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:

    # Traffic Filter rules shown in above section
    ...

    # Enable WAF protections (only works if WAF is enabled for your environment)
      - name: block-waf-flags
        when:
          reqProperty: tier
          matches: "author|publish"
        action:
          type: block
          wafFlags:
            - SANS
            - TORNODE
            - NOUA
            - SCANNER
            - USERAGENT
            - PRIVATEFILE
            - ABNORMALPATH
            - TRAVERSAL
            - NULLBYTE
            - BACKDOOR
            - LOG4J-JNDI
            - SQLI
            - XSS
            - CODEINJECTION
            - CMDEXE
            - NO-CONTENT-TYPE
            - UTF8
    # Disable protection against CMDEXE on /bin
      - name: allow-cdmexe-on-root-bin
        when:
          allOf:
            - reqProperty: tier
              matches: "author|publish"
            - reqProperty: path
              matches: "^/bin/.*"
        action:
          type: allow
          wafFlags:
            - CMDEXE
```

## Resumen

En resumen, este tutorial le ha dotado de los conocimientos y las herramientas necesarias para reforzar la seguridad de sus aplicaciones web en Adobe Experience Manager as a Cloud Service (AEMCS). Con ejemplos prácticos de reglas y perspectivas en el análisis de resultados, puede proteger de forma eficaz su sitio web y sus aplicaciones.



