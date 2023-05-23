---
title: Creación de MyAccountForm
description: Cree el formulario myaccount para recuperar el formulario parcialmente completado tras verificar correctamente el ID de aplicación y el número de teléfono.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
topic: Development
role: User
level: Beginner
exl-id: 1ecd8bc0-068f-4557-bce4-85347c295ce0
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 0%

---

# Creación de MyAccountForm

El formulario **MyAccountForm** se utiliza para recuperar el formulario adaptable parcialmente completado después de que el usuario haya verificado el id de aplicación y el número móvil asociado al id de aplicación.

![mi formulario de cuenta](assets/6599.JPG)

Cuando el usuario introduce el ID de aplicación y hace clic en el **FetchApplication** , el número móvil asociado con el ID de aplicación se recupera de la base de datos mediante la operación Get del modelo de datos de formulario.

Este formulario utiliza la invocación del POST al modelo de datos de formulario para comprobar el número de móvil mediante OTP. La acción de envío del formulario se activa cuando se verifica correctamente el número de móvil con el siguiente código. Activamos el evento de clic del botón de envío denominado **submitForm**.

>[!NOTE]
> Deberá proporcionar los valores Clave de API y Secreto de API específicos de su [Siguiente](https://dashboard.nexmo.com/) cuenta en los campos correspondientes de MyAccountForm

![déclencheur-enviar](assets/trigger-submit.JPG)



Este formulario está asociado a una acción de envío personalizada que reenvía el envío del formulario al servlet montado en **/bin/renderaf**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

El código del servlet montado en **/bin/renderaf** reenvía la solicitud para procesar el formulario adaptable storeafwithattachments rellenado previamente con los datos guardados.


* MyAccountForm puede ser [descargado desde aquí](assets/my-account-form.zip)

* Los formularios de ejemplo se basan en [plantilla de formulario adaptable personalizada](assets/custom-template-with-page-component.zip) AEM que debe importarse en para que los formularios de ejemplo se procesen correctamente.

* [Controlador de envío personalizado](assets/custom-submit-my-account-form.zip) AEM asociado al envío de MyAccountForm debe importarse a la dirección de correo electrónico de la cuenta de.

## Pasos siguientes

[Prueba de la solución implementando los recursos de ejemplo](./deploy-this-sample.md)
