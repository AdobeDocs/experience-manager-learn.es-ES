---
title: Integrar AEM Forms con SendGrid
description: Aproveche la plataforma de envío de correo electrónico basada en la nube de SengGrid mediante AEM Forms.
feature: Adaptive Forms
version: 6.4,6.5
kt: 13605
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2023-07-14T00:00:00Z
source-git-commit: cc24ebca488ea286e8a4605edfb39420c1c10022
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 1%

---

# Integrar AEM Forms con SendGrid

Le damos la bienvenida a esta guía técnica, donde exploraremos el proceso de envío de correos electrónicos con plantillas dinámicas de SendGrid desde AEM Forms. Esta guía pretende proporcionarle una comprensión clara de cómo aprovechar las plantillas dinámicas para personalizar el contenido del correo electrónico de forma eficaz.

Las plantillas dinámicas permiten crear plantillas de correo electrónico que pueden mostrar contenido diferente a los destinatarios en función de los datos capturados en el formulario adaptable. Al utilizar variables de personalización, puede ofrecer experiencias de correo electrónico segmentadas y personalizadas que resuenen con su audiencia.

Además, profundizaremos en el uso del archivo Swagger, que le permite personalizar aún más sus correos electrónicos incluyendo el nombre y la dirección de correo electrónico del cliente, así como seleccionando la plantilla de correo electrónico dinámica adecuada.

Siga las instrucciones paso a paso de este documento para aprovechar el poder de las plantillas dinámicas de SendGrid y AEM Forms, y elevar sus comunicaciones por correo electrónico a nuevos niveles de participación y relevancia. ¡Vamos a empezar!

## Requisitos previos

Antes de continuar enviando correos electrónicos mediante plantillas dinámicas de SendGrid desde AEM Forms, asegúrese de que ha cumplido los siguientes requisitos previos:

1. **Cuenta de SendGrid**: Regístrese para obtener una cuenta de SendGrid en [https://sendgrid.com](https://sendgrid.com) para acceder a sus servicios de envío de correo electrónico. Necesitará las credenciales de la cuenta para integrar SendGrid con AEM Forms.
1. **Familiaridad con la creación de fuentes de datos**: Tiene conocimientos prácticos sobre la creación de fuentes de datos en AEM Forms. Si es necesario, consulte la documentación sobre [creación de fuentes de datos](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) para obtener instrucciones detalladas.
1. **Familiaridad con el modelo de datos de formulario**: Comprenda el concepto de modelo de datos de formulario en AEM Forms. Si es necesario, revise la documentación de [crear modelos de datos de formulario](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html?lang=es) para asegurarse de que tiene la comprensión necesaria.

Al cumplir estos requisitos previos, estará equipado con los conocimientos y recursos esenciales para enviar correos electrónicos de forma eficaz mediante las plantillas dinámicas SendGrid de AEM Forms.

## Recursos de muestra

Los recursos de ejemplo proporcionados con este artículo incluyen:

* **[Archivo Swagger](assets/SendGridWithDynamicTemplate.yaml)**: Este archivo le permite enviar correos electrónicos mediante una plantilla de correo electrónico dinámica. Proporciona las especificaciones y configuraciones necesarias para integrarse con SendGrid y AEM Forms para una entrega de correo electrónico sin problemas.

No dude en utilizar el archivo Swagger proporcionado como referencia o punto de partida para implementar la funcionalidad de correo electrónico con plantillas dinámicas.

## Instrucciones de prueba

Para probar la funcionalidad descrita en esta guía, siga estos pasos:

1. Descargue la [archivo swagger](assets/SendGridWithDynamicTemplate.yaml) se proporciona en la carpeta de recursos.
2. Cree un origen de datos Restful utilizando el archivo swagger descargado y las credenciales de SendGrid.
3. Cree un modelo de datos de formulario basado en la fuente de datos Restful.
4. Invoque el `mail/send` Operación del POST del modelo de datos de formulario según sus necesidades. Por ejemplo, puede almacenar en déclencheur el correo electrónico al hacer clic en el botón o incluirlo como parte del flujo de trabajo de AEM Forms.

La carga útil de ejemplo para el servicio es la siguiente: Reemplace los valores de marcador de posición con sus propios datos:

```json
{
    "sendgridpayload": {
        "from": {
            "email": "gs@xyz.com"
        },
        "personalizations": [{
            "to": [{
                "email": "johndoe@xyz.com"
            }],
            "dynamic_template_data": {
                "customerName": "John Doe"
            }
        }],
        "template_id": "d-72aau292a3bd60b5300c"
    }
}
```

Asegúrese de que la variable `template_id` corresponde al ID de la plantilla de correo electrónico dinámica SendGrid, y las direcciones de correo electrónico son válidas y verificadas por SendGrid. Los valores del `personalizations` Esta sección le permite personalizar el correo electrónico mediante los datos introducidos por el usuario desde el formulario adaptable.

Al seguir estos pasos y personalizar la carga útil proporcionada, puede probar de forma eficaz la integración de las plantillas dinámicas de SendGrid con AEM Forms.

