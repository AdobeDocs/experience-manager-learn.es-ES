---
title: Crear MyAccountForm
description: Cree el formulario myaccount para recuperar el formulario parcialmente completado tras la verificación correcta del id. de aplicación y el número de teléfono.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6599
thumbnail: 6599.jpg
translation-type: tm+mt
source-git-commit: 9d4e864f42fa6c0b2f9b895257db03311269ce2e
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---



# Crear MyAccountForm

El formulario **MyAccountForm** se utiliza para recuperar el formulario adaptable parcialmente completado después de que el usuario haya verificado el ID de la aplicación y el número móvil asociado al ID de la aplicación.

![formulario de mi cuenta](assets/6599.JPG)

Cuando el usuario introduce la ID de la aplicación y hace clic en el botón **Buscar aplicación** , el número de móvil asociado con la ID de la aplicación se obtiene de la base de datos mediante la operación Obtener del modelo de datos de formulario.

En este formulario se utiliza la invocación del POST del Modelo de datos de formulario para comprobar el número móvil mediante OTP. La acción de envío del formulario se activa cuando se comprueba correctamente el número de móvil con el siguiente código. Estamos activando el evento de clic del botón de envío denominado **submitForm**.

>[!NOTE]
> Deberá proporcionar la clave de API y los valores secretos de API específicos de su cuenta de [Nexmo](https://dashboard.nexmo.com/) en los campos correspondientes de MyAccountForm

![desencadenador-envío](assets/trigger-submit.JPG)



Este formulario está asociado con una acción de envío personalizada que reenvía el envío del formulario al servlet montado en **/bin/renderaf**

```java
com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/renderaf",null,null);
```

El código en el servlet montado en **/bin/renderaf** reenvía la solicitud para procesar el formulario adaptable de almacenamiento rellenado previamente con los datos guardados.


* MyAccountForm se puede [descargar desde aquí](assets/my-account-form.zip)

* Los formularios de ejemplo se basan en una plantilla [de formulario adaptable](assets/custom-template-with-page-component.zip) personalizada que debe importarse en AEM para que los formularios de ejemplo se procesen correctamente.

* [El controlador](assets/custom-submit-my-account-form.zip) de envío personalizado asociado con el envío MyAccountForm debe importarse en AEM.
