---
title: Informes de recursos en AEM Assets
description: 'AEM Assets proporciona un entorno de sistema de informes de nivel empresarial que se amplía para repositorios de gran tamaño a través de una experiencia intuitiva del usuario. '
feature: reports
topics: authoring, operations, performance, metadata
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
kt: 648
translation-type: tm+mt
source-git-commit: 10784dce34443adfa1fc6dc324242b1c021d2a17
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 2%

---


# Informes de recursos{#using-reports-in-aem-assets}

AEM Assets proporciona un entorno de sistema de informes de nivel empresarial que se amplía para repositorios de gran tamaño a través de una experiencia intuitiva del usuario.

>[!VIDEO](https://video.tv.adobe.com/v/22140/?quality=12&learn=on)

## Fórmulas de Microsoft Excel {#excel-formulas}

En el vídeo se utilizan las siguientes fórmulas para generar el gráfico Recursos por tamaño en Microsoft Excel.

### Normalización del tamaño del recurso en bytes {#asset-size-normalization-to-bytes}

```
=IF(RIGHT(D2,2)="KB",
      LEFT(D2,(LEN(D2)-2))*1024,
  IF(RIGHT(D2,2)="MB",
      LEFT(D2,(LEN(D2)-2))*1024*1024,
  IF(RIGHT(D2,2)="GB",
      LEFT(D2,(LEN(D2)-2))*1024*1024*1024,
  IF(RIGHT(D2,2)="TB",
      LEFT(D2,(LEN(D2)-2))*1024*1024*1024*1024, 0))))
```

### Recuento de recursos por tamaño {#asset-count-by-size}

#### Menos de 200 KB {#less-than-kb}

```
=COUNTIFS(E2:E1000,"< 200000")
```

#### 200 KB a 500 KB {#kb-to-kb}

```
=COUNTIFS(E2:E1000,">= 200000", E2:E1000,"<= 500000")
```

#### Buenos más de 500 KB {#greater-than-kb}

```
=COUNTIFS(E2:E1000,"> 500000")
```

## Recursos adicionales{#additional-resources}

Descargar [Todos los recursos del archivo de Excel con Chart](./assets/asset-reports/all-assets.xlsx)
