---
title: Crear el MyAccountForm
description: Cree el formulario myaccount para recuperar el formulario parcialmente completado tras la verificación correcta del id de la aplicación y el número de teléfono.
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

# Crear el MyAccountForm

El formulario **MyAccountForm** se utiliza para recuperar el formulario adaptable parcialmente completado después de que el usuario haya verificado el id de la aplicación y el número de móvil asociado al id de la aplicación.

![formulario de mi cuenta](assets/6599.JPG)

Cuando el usuario introduce el ID de aplicación y hace clic en el botón **FetchApplication** , el número de móvil asociado con el id de aplicación se obtiene de la base de datos mediante la operación Get del modelo de datos de formulario.

Este formulario utiliza la invocación del POST del Modelo de datos de formulario para comprobar el número de móvil mediante OTP. La acción de envío del formulario se activa cuando se verifica correctamente el número de móvil con el siguiente código. Se activa el evento click del botón de envío denominado **submitForm**.

>[!NOTE]
> Deberá proporcionar la clave de API y los valores secretos de la API específicos de su [Nexmo](https://dashboard.nexmo.com/) en los campos correspondientes de MyAccountForm

![Enviar déclencheur](assets/trigger-submit.JPG)



Este formulario está asociado con una acción de envío personalizada que reenvía el envío del formulario al servlet montado en **/bin/renderaf**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

El código del servlet montado en **/bin/renderaf** reenvía la solicitud para procesar el almacén con archivos adjuntos, formulario adaptable rellenado previamente con los datos guardados.


* MyAccountForm puede ser [descargado desde aquí](assets/my-account-form.zip)

* Los formularios de ejemplo se basan en [plantilla de formulario adaptable personalizada](assets/custom-template-with-page-component.zip) que debe importarse en AEM para que los formularios de ejemplo se representen correctamente.

* [Controlador de envío personalizado](assets/custom-submit-my-account-form.zip) asociado con el envío MyAccountForm debe importarse en AEM.

## Pasos siguientes

[Probar la solución implementando los recursos de ejemplo](./deploy-this-sample.md)
