---
title: Crear configuración OSGi personalizada
description: Configuración personalizada de OSGi para capturar detalles específicos de document cloud
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: desarrollo
thumbnail: 7818.jpg
kt: 7818
source-git-commit: 84499d5a7c8adac87196f08c6328e8cb428c0130
workflow-type: tm+mt
source-wordcount: '64'
ht-degree: 3%

---

# Introducción

Cree una configuración OSGi personalizada para capturar las credenciales de su cuenta de document cloud


Para realizar una configuración OSGi personalizada, primero necesitamos crear una interfaz cuyos métodos públicos representen los campos de la configuración.

![doc-cloud-config](assets/doc-cloud-configuration.JPG)


Cree una interfaz denominada DocumentCloudConfiguration y pegue el siguiente código en ella.

```java
package com.aemforms.doccloud.core;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name="Document Cloud Service Configuration", description = "Connect AEM Forms With Document Cloud")
public @interface DocumentCloudConfiguration {
	  @AttributeDefinition(name="Client ID", description="Client ID")
	  String getClientID() default "";
	  
	  @AttributeDefinition(name="Client Secret", description="Client Secret")
	  public String getClientSecret() default "26dc4de1-f7f0-46b6-9e8e-86270ad34f58";
	  
	  @AttributeDefinition(name="Technical Account ID", description="Technical Account ID")
	  public String getTechnicalAccount() default "42FF05A7606F71BB0A495FBE@techacct.adobe.com";

	  @AttributeDefinition(name="Organization ID", description="Organization ID")
	  String getOrganizationID() default "299805FF5FC9199D0A495EBC@AdobeOrg";
	  
	  @AttributeDefinition(name="Metascope", description="Metascope")
	  String getMetascope() default "ent_documentcloud_sdk";


}
```
