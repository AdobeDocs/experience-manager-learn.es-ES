---
title: Almacenamiento de archivos adjuntos de formulario
description: Extraiga los archivos adjuntos del formulario y guárdelo en una nueva ubicación del repositorio CRX.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6537
thumbnail: 6537.jpg
topic: Development
role: Developer
level: Experienced
exl-id: ec50b9b1-e28c-4d84-ae90-6a21c9700688
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '192'
ht-degree: 1%

---

# Almacenamiento de archivos adjuntos de formulario

Cuando se añaden archivos adjuntos a un formulario adaptable, los archivos adjuntos se almacenan en una ubicación temporal en el repositorio CRX. Para que nuestro caso de uso funcione, necesitamos almacenar los archivos adjuntos del formulario en una nueva ubicación en el repositorio CRX.

El servicio OSGi se crea para almacenar los archivos adjuntos del formulario en una nueva ubicación del repositorio CRX. Se crea un nuevo mapa de archivos con la nueva ubicación de los archivos adjuntos en el CRX y se devuelve a la aplicación que realiza la llamada.
El siguiente es el FileMap que se envía al servlet. La clave es el campo de formulario adaptable y el valor es la ubicación temporal del archivo adjunto. En nuestro servlet extraeremos el archivo adjunto y lo almacenaremos en una nueva ubicación en el repositorio de AEM y actualizaremos el FileMap con la nueva ubicación

```java
{
"guide[0].guide1[0].guideRootPanel[0].idDocumentPanel[0].idcard[0]": "idcard/CA-DriversLicense.pdf",
"guide[0].guide1[0].guideRootPanel[0].documentation[0].yourBankStatements[0].table1603552612235[0].Row1[0].tableItem11[0]": "tableItem11/BankStatement-Sept-2020.pdf"
}
```

El siguiente es el código que extrae los archivos adjuntos de la solicitud y los almacena en **/content/afattachment** carpeta

```java
public String storeAFAttachments(JSONObject fileMap, SlingHttpServletRequest request) {
    JSONObject newFileMap = new JSONObject();
    try {
        Iterator keys = fileMap.keys();
        log.debug("The file map is " + fileMap.toString());
        while (keys.hasNext()) {
            String key = (String) keys.next();
            log.debug("#### The key is " + key);
            String attacmenPath = (String) fileMap.get((key));
            log.debug("The attachment path is " + attacmenPath);
            if (!attacmenPath.contains("/content/afattachments")) {
                String fileName = attacmenPath.split("/")[1];
                log.debug("#### The attachment name  is " + fileName);
                InputStream is = request.getPart(attacmenPath).getInputStream();
                Document aemFDDocument = new Document(is);
                String crxPath = saveDocumentInCrx("/content/afattachments", fileName, aemFDDocument);
                log.debug(" ##### written to crx repository  " + attacmenPath.split("/")[1]);
                newFileMap.put(key, crxPath);
            } else {
                log.debug("$$$$ The attachment was already added " + key);
                log.debug("$$$$ The attachment path is " + attacmenPath);
                int position = attacmenPath.indexOf("//");
                log.debug("$$$$ After substring " + attacmenPath.substring(position + 1));
                log.debug("$$$$ After splitting " + attacmenPath.split("/")[1]);
                newFileMap.put(key, attacmenPath.substring(position + 1));
            }
        }
    } catch (Exception ex) {
        log.debug(ex.getMessage());
    }
    return newFileMap.toString();

}


}
```

Este es el nuevo FileMap con la ubicación actualizada de los archivos adjuntos del formulario

```java
{
"guide[0].guide1[0].guideRootPanel[0].idDocumentPanel[0].idcard[0]": "/content/afattachments/7dc0cbde-404d-49a9-9f7b-9ab5ee7482be/CA-DriversLicense.pdf",
"guide[0].guide1[0].guideRootPanel[0].documentation[0].yourBankStatements[0].table1603552612235[0].Row1[0].tableItem11[0]": "/content/afattachments/81653de9-4967-4736-9ca3-807a11542243/BankStatement-Sept-2020.pdf"
}
```

## Pasos siguientes

[Guardar los datos del formulario](./store-form-data.md)