---
title: Crear cuenta de Adobe Target Cloud Service
description: Recorrido paso a paso sobre cómo integrar Adobe Experience Manager como Cloud Service con Adobe Target mediante la autenticación de Cloud Service y Adobe IMS
feature: cloud-services
topics: integrations, administration, development
audience: administrator, developer
doc-type: technical video
activity: setup
version: cloud-service
kt: 6044
thumbnail: 41244.jpg
translation-type: tm+mt
source-git-commit: 25ca90f641aaeb93fc9319692f3b099d6b528dd1
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 0%

---


# Crear cuenta de Adobe Target Cloud Service {#adobe-target-cloud-service}

Instrucciones paso a paso sobre cómo integrar Adobe Experience Manager como Cloud Service con Adobe Target mediante la autenticación IMS de Cloud Service y Adobe.

>[!VIDEO](https://video.tv.adobe.com/v/41244?quality=12&learn=on)

>[!CAUTION]
>
>Hay un problema conocido con la configuración de Cloud Services de Adobe Target que se muestra en el vídeo. En su lugar, se recomienda seguir los mismos pasos en el vídeo, pero utilizar la configuración [de Cloud Services de Adobe Target](https://docs.adobe.com/content/help/en/experience-manager-learn/aem-target-tutorial/aem-target-implementation/using-aem-cloud-services.html)heredados.

## Problemas comunes

Al exportar fragmento de experiencia a Adobe destinatario, si la integración de Destinatario en la consola de administración no tiene el permiso correcto, puede que reciba un error como se muestra a continuación.

**Error** en la interfaz de usuario de la API de![Destinatario](assets/error-target-offer.png)

**Error** de registro en la consola de API de![Destinatario](assets/target-console-error.png)


**Solución**

1. Navegar al [Admin Console](https://adminconsole.adobe.com/)
2. Seleccione Productos > Adobe Target > Perfil de productos
3. En la ficha Integraciones, seleccione la integración (proyecto de E/S de Adobe)
4. Asignar la función de editor o aprobador.

![Error de API de destinatario](assets/target-permissions.png)

Añadir el permiso correcto para la integración de Destinatario debería ayudarle a resolver el problema anterior.