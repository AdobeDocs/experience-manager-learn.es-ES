---
title: Creación de MyAccountForm
description: Cree el formulario myaccount para recuperar el formulario parcialmente completado tras verificar correctamente el ID de aplicación y el número de teléfono.
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
jira: KT-6599
thumbnail: 6599.jpg
topic: Development
role: User
level: Beginner
exl-id: 1ecd8bc0-068f-4557-bce4-85347c295ce0
duration: 53
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 0%

---

# Creación de MyAccountForm

El formulario **MyAccountForm** se usa para recuperar el formulario adaptable parcialmente completado después de que el usuario haya verificado el id. de aplicación y el número de móvil asociado con el id. de aplicación.

![formulario de mi cuenta](assets/6599.JPG)

Cuando el usuario introduce el ID de aplicación y hace clic en el botón **FetchApplication**, el número móvil asociado al ID de aplicación se recupera de la base de datos mediante la operación Get del modelo de datos de formulario.

Este formulario utiliza la invocación del POST al modelo de datos de formulario para comprobar el número de móvil mediante OTP. La acción de envío del formulario se activa cuando se verifica correctamente el número de móvil con el siguiente código. Activamos el evento de clic del botón de envío denominado **submitForm**.

>[!NOTE]
> Deberá proporcionar los valores Clave de API y Secreto de API específicos de su cuenta de [Nexmo](https://dashboard.nexmo.com/) en los campos apropiados de MyAccountForm

![déclencheur-enviar](assets/trigger-submit.JPG)



Este formulario está asociado con una acción de envío personalizada que reenvía el envío del formulario al servlet montado en **/bin/renderaf**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

El código del servlet montado en **/bin/renderaf** reenvía la solicitud para procesar el formulario adaptable storeafwithattachments rellenado previamente con los datos guardados.


* MyAccountForm se puede [descargar desde aquí](assets/my-account-form.zip)

* AEM Los formularios de ejemplo se basan en [plantilla de formulario adaptable personalizada](assets/custom-template-with-page-component.zip) que debe importarse en los formularios de ejemplo para que se representen correctamente en los formularios de ejemplo.

* AEM [Es necesario importar el controlador de envío personalizado](assets/custom-submit-my-account-form.zip) asociado con el envío de MyAccountForm a la lista de distribución de recursos de la lista de envíos de la lista de distribución de recursos.

## Siguientes pasos

[Prueba de la solución implementando los recursos de ejemplo](./deploy-this-sample.md)
