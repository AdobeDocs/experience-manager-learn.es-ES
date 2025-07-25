---
title: Normalización de solicitudes
description: Obtenga información sobre cómo normalizar solicitudes transformándolas mediante reglas de filtro de tráfico en AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2025-06-04T00:00:00Z
jira: KT-18313
thumbnail: null
source-git-commit: 293157c296676ef1496e6f861ed8c2c24da7e068
workflow-type: ht
source-wordcount: '259'
ht-degree: 100%

---

# Normalización de solicitudes

Obtenga información sobre cómo normalizar solicitudes transformándolas mediante reglas de filtro de tráfico en AEM as a Cloud Service.

## Por qué y cuándo usar las solicitudes de transformación

Las transformaciones de solicitudes son útiles cuando desea normalizar el tráfico entrante y reducir la variación innecesaria causada por los parámetros de consulta o los encabezados innecesarios. Esta técnica se utiliza principalmente para:

- Mejorar la eficacia del almacenamiento en caché eliminando los parámetros de eliminación de caché que no son relevantes para la aplicación de AEM.
- Proteger el origen frente al uso indebido minimizando las permutaciones de solicitudes y mitigando el procesamiento innecesario.
- Limpiar o simplificar las solicitudes antes de reenviarlas a AEM.

Estas transformaciones se aplican generalmente en la capa de CDN, especialmente para los niveles de AEM Publish que atienden al tráfico público.

## Requisitos previos

Antes de continuar, asegúrese de completar la configuración necesaria tal como se describe en el tutorial [Cómo configurar el filtro de tráfico y las reglas WAF](../setup.md). Además, ha clonado e implementado el [proyecto WKND Sites de AEM](https://github.com/adobe/aem-guides-wknd) en su entorno de AEM.

## Ejemplo: la aplicación no necesita anular la configuración de los parámetros de consulta

En este ejemplo, configure una regla que **quita todos los parámetros de consulta excepto** `search` y `campaignId` para reducir la fragmentación de la caché.

- Añada la siguiente regla al archivo `/config/cdn.yaml` del proyecto WKND.

```yaml
kind: "CDN"
version: "1"
metadata:
  envTypes: ["dev", "stage", "prod"]
data:
  requestTransformations:
    rules:
    # Unset all query parameters except those needed for search and campaignId
    - name: unset-all-query-params-except-those-needed
      when:
        reqProperty: tier
        in: ["publish"]
      actions:
        - type: unset
          queryParamMatch: ^(?!search$|campaignId$).*$
```

- Confirme e inserte los cambios en el repositorio de Git de Cloud Manager.

- Implemente los cambios en el entorno de AEM mediante la canalización de configuración de Cloud Manager [creada anteriormente](../setup.md#deploy-rules-using-adobe-cloud-manager).

- Pruebe la regla accediendo a la página del sitio WKND, por ejemplo `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar&otherParam=baz`.

- En los registros de AEM (`aemrequest.log`), debería ver que la solicitud se transforma en `https://publish-pXXXX-eYYYY.adobeaemcloud.com/us/en.html?search=foo&campaignId=bar`, con `otherParam` eliminado.

  ![Transformación de la solicitud WKND](../assets/how-to/aemrequest-log-transformation.png)

