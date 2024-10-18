---
title: Crear un servicio de envío personalizado para administrar el envío de formularios adaptables sin encabezado
description: Devolver respuesta personalizada en función de los datos enviados
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-13520
exl-id: c23275d7-daf7-4a42-83b6-4d04b297c470
duration: 115
source-git-commit: b4df652fcda0af5d01077b97aa7fa17cfe2abf4b
workflow-type: tm+mt
source-wordcount: '453'
ht-degree: 0%

---

# Creación de envíos personalizados

AEM Forms proporciona una serie de opciones de envío listas para usar que satisfacen la mayoría de los casos de uso. Además de estas acciones de envío predefinidas, AEM Forms le permite escribir su propio controlador de envío personalizado para procesar el envío del formulario según sus necesidades.

Para escribir un servicio de envío personalizado, se siguieron los siguientes pasos

## AEM Crear proyecto de

Si ya tiene un proyecto as a Cloud Service de AEM Forms, [puede pasar a escribir un servicio de envío personalizado](#Write-the-custom-submit-service)

* Cree una carpeta llamada cloudmanager en la unidad c.
* Vaya a la carpeta recién creada
* Copie y pegue el contenido de [este archivo de texto](./assets/creating-maven-project.txt) en la ventana del símbolo del sistema. Es posible que tenga que cambiar DarchetypeVersion=41 en función de la [última versión](https://github.com/adobe/aem-project-archetype/releases). La última versión era 41 en el momento de escribir este artículo.
* Ejecute el comando pulsando la tecla Intro. Si todo funciona correctamente, debería ver el mensaje de éxito de la versión.

## Escribir el servicio de envío personalizado{#Write-the-custom-submit-service}

AEM Inicie IntelliJ y abra el proyecto de. Cree una nueva clase Java llamada **HandleRegistrationFormSubmission**, como se muestra en la captura de pantalla siguiente
![servicio de envío personalizado](./assets/custom-submit-service.png)

El siguiente código se escribió para implementar el servicio

```java
package com.aem.bankingapplication.core;
import java.util.HashMap;
import java.util.Map;
import com.google.gson.Gson;
import org.osgi.service.component.annotations.Component;
import com.adobe.aemds.guide.model.FormSubmitInfo;
import com.adobe.aemds.guide.service.FormSubmitActionService;
import com.adobe.aemds.guide.utils.GuideConstants;
import com.google.gson.JsonObject;
import org.slf4j.*;

@Component(
        service=FormSubmitActionService.class,
        immediate = true
)
public class HandleRegistrationFormSubmission implements FormSubmitActionService {
    private static final String serviceName = "Core Custom AF Submit";
    private static Logger logger = LoggerFactory.getLogger(HandleRegistrationFormSubmission.class);



    @Override
    public String getServiceName() {
        return serviceName;
    }

    @Override
    public Map<String, Object> submit(FormSubmitInfo formSubmitInfo) {
        logger.error("in my custom submit service");
        Map<String, Object> result = new HashMap<>();
        logger.error("in my custom submit service");
        String data = formSubmitInfo.getData();
        JsonObject formData = new Gson().fromJson(data,JsonObject.class);
        logger.error("The form data is "+formData);
        JsonObject jsonObject = new JsonObject();
        jsonObject.addProperty("firstName",formData.get("firstName").getAsString());
        jsonObject.addProperty("lastName",formData.get("lastName").getAsString());
        result.put(GuideConstants.FORM_SUBMISSION_COMPLETE, Boolean.TRUE);
        result.put("json",jsonObject.toString());
        return result;
    }

}
```

## Cree un nodo CRX en aplicaciones

Expanda el nodo ui.apps y cree un nuevo paquete llamado **HandleRegistrationFormSubmission** en el nodo apps, como se muestra en la captura de pantalla siguiente
![crx-node](./assets/crx-node.png)
Cree un archivo llamado .content.xml en **HandleRegistrationFormSubmission**. Copie y pegue el siguiente código en el archivo .content.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
    jcr:description="Handle Registration Form Submission"
    jcr:primaryType="sling:Folder"
    guideComponentType="fd/af/components/guidesubmittype"
    guideDataModel="xfa,xsd,basic"
    submitService="Core Custom AF Submit"/>
```

El valor del elemento **submitService** debe coincidir con **serviceName = &quot;Core Custom AF Submit&quot;** en la implementación de FormSubmitActionService.

## Implemente el código en la instancia local de AEM Forms

Antes de insertar los cambios en el repositorio de Cloud Manager, se recomienda implementar el código en la instancia de autor local preparada para la nube para probar el código. Asegúrese de que la instancia de autor se esté ejecutando.
AEM Para implementar el código en la instancia de autor preparada para la nube, vaya a la carpeta raíz del proyecto de la y ejecute el siguiente comando

```
mvn clean install -PautoInstallSinglePackage
```

Esto implementará el código como un paquete único en la instancia de autor

## Inserte el código en Cloud Manager e implemente el código

Después de comprobar el código en la instancia local, insértelo en la instancia de la nube.
Inserte los cambios en el repositorio local de Git y luego en el repositorio de Cloud Manager. AEM Puede hacer referencia a los artículos [Configuración de Git](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/setup-git.html), [inserción del proyecto de la en el repositorio de Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/push-project-to-cloud-manager-git.html) e [implementación en el entorno de desarrollo](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/deploy-to-dev-environment.html).

Una vez que la canalización se haya ejecutado correctamente, debe poder asociar la acción de envío del formulario al controlador de envío personalizado, como se muestra en la captura de pantalla siguiente
![acción de envío](./assets/configure-submit-action.png)

## Siguientes pasos

[Mostrar la respuesta personalizada en la aplicación react](./handle-response-react-app.md)
