---
title: Visualización del código QR en el formulario adaptable
description: Mostrar código QR en un formulario adaptable
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-15603
last-substantial-update: 2024-05-28T00:00:00Z
source-git-commit: e20d9f80cc7e1c6f5f6c81233d9a5178551e2fa2
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 0%

---

# Componente de código QR de muestra

La incrustación de un código QR en un formulario adaptable puede mejorar en gran medida la comodidad y la eficacia para que los usuarios accedan a información adicional relacionada con el formulario.

El componente de muestra utiliza [QRCode.js](https://davidshimjs.github.io/qrcodejs/).

QRCode.js es una biblioteca javascript para realizar QRCode. Es compatible con Cross-browser with HTML5 Canvas y table tag en DOM.

El componente genera el código QR en función del valor especificado en la propiedad de configuración del componente.
![imagen](assets/qr-code-url.png)

El siguiente código se utilizó en el body.jsp del componente qr-code-generator.

La &quot;url&quot; es la url que debe incrustarse en el código qr. Esta URL se especifica en las propiedades de configuración del componente de código QR.

```java
<%@include file="/libs/foundation/global.jsp"%>
<body>
    <h2>Scan the QR Code for more information related to this form</h2>
    <div data-url="<%=properties.get("url")%>">
    </div>
    <div id="qrcode">
    </div>
</body>
```



El siguiente código utiliza el método makeCode de la biblioteca QRCode.js en la biblioteca de cliente del componente qr-code-generator. El código QR generado se anexa al div identificado por id **&quot;qrcode&quot;**.

```javascript
$(document).ready(function()
  {
      var qrcode = new QRCode("qrcode");
      qrcode.makeCode(document.querySelector("[data-url]").getAttribute("data-url"));
      
 });
```

## Implementación de los recursos en el servidor local

* [Descargue e instale el componente de código QR mediante el Administrador de paquetes.](assets/qrcode.zip)
* [Descargue e instale el formulario adaptable de ejemplo mediante el Administrador de paquetes.](assets/form-with-qr-code.zip)
* [Previsualización del formulario](http://localhost:4502/content/dam/formsanddocuments/qrcode/w9form/jcr:content?wcmmode=disabled). La sección de ayuda del formulario tiene el código QR.


