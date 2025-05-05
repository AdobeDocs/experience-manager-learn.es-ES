---
title: Registro de un servlet mediante un tipo de recurso
description: Asignación de un servlet a un tipo de recurso en AEM Forms CS
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Developer Tools
jira: KT-14581
duration: 90
exl-id: 2a33a9a9-1eef-425d-aec5-465030ee9b74
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 3%

---

# Introducción

Los servlets de enlace por rutas tienen varias desventajas en comparación con los enlaces por tipos de recursos, concretamente:

* Los servlets enlazados a rutas no pueden controlar el acceso mediante las ACL de repositorio JCR predeterminadas
* Los servlets enlazados a rutas solo se pueden registrar en una ruta y no en un tipo de recurso (es decir, sin control de sufijos)
* Si un servlet enlazado a una ruta no está activo, por ejemplo, si el paquete falta o no se inicia, un POST puede dar como resultado resultados inesperados. normalmente se crea un nodo en `/bin/xyz` que posteriormente se superpone al enlace de ruta de los servlets
la asignación no es transparente para un desarrollador que solo busque en el repositorio
Dados estos inconvenientes, se recomienda enfáticamente enlazar los servlets a tipos de recursos en lugar de rutas

## Crear servlet

Inicie el proyecto de banca aem en IntelliJ. Cree un servlet denominado GetFieldChoices en la carpeta de servlets como se muestra en la captura de pantalla siguiente.
![opciones](assets/fetchchoices.png)

## Servlet de ejemplo

El siguiente servlet está enlazado al tipo de recurso Sling: _&#x200B;**azure/fetchchoices**&#x200B;_



```java
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.jcr.Session;
import javax.servlet.Servlet;
import java.io.IOException;
import java.io.Serializable;

@Component(
        service={Servlet.class }
)

        @SlingServletResourceTypes(
                resourceTypes="azure/fetchchoices",
                methods= "GET",
                extensions="json"
                )


public class GetFieldChoices extends SlingAllMethodsServlet implements Serializable {
    private static final long serialVersionUID = 1L;
    private final  transient Logger log = LoggerFactory.getLogger(this.getClass());


   

    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {

        log.debug("The form path I got was "+request.getParameter("formPath"));

    }
}
```

## Creación de recursos en CRX

* Inicie sesión en su SDK local de AEM.
* Cree un recurso con el nombre `fetchchoices` (puede asignar el nombre a este nodo de todos modos que desee) del tipo `cq:Page` en el nodo de contenido.
* Guarde los cambios
* Cree un nodo denominado `jcr:content` de tipo `cq:PageContent` y guarde los cambios
* Agregue las siguientes propiedades al nodo `jcr:content`

| Nombre de la propiedad | Valor de propiedad |
|--------------------|--------------------|
| jcr:title | Servlets de utilidad |
| sling:resourceType | `azure/fetchchoices` |


El valor `sling:resourceType` debe coincidir con resourceTypes=&quot;azure/fetchchoices especificado en el servlet.

Ahora puede invocar el servlet solicitando el recurso con `sling:resourceType` = `azure/fetchchoices` en su ruta completa, con cualquier selector o extensión registrado en el servlet Sling.

```html
http://localhost:4502/content/fetchchoices/jcr:content.json?formPath=/content/forms/af/forrahul/jcr:content/guideContainer
```

La ruta de acceso `/content/fetchchoices/jcr:content` es la ruta de acceso del recurso y la extensión `.json` es la especificada en el servlet

## Sincronizar el proyecto de AEM

1. Abra el proyecto de AEM en su editor favorito. He usado intelliJ para esto.
1. Cree una carpeta llamada `fetchchoices` en `\aem-banking-application\ui.content\src\main\content\jcr_root\content`
1. Haga clic con el botón derecho en la carpeta `fetchchoices` y seleccione `repo | Get Command` (este elemento de menú está configurado en un capítulo anterior de este tutorial).

Esto debe sincronizar este nodo de AEM con el proyecto local de AEM.

La estructura del proyecto de AEM debería tener este aspecto
![solucionador de recursos](assets/mapping-servlet-resource.png)
Actualice el archivo filter.xml en la carpeta aem-banking-application\ui.content\src\main\content\META-INF\vault con la siguiente entrada

```xml
<filter root="/content/fetchchoices" mode="merge"/>
```

Ahora puede insertar los cambios en un entorno de AEM as a Cloud Service con Cloud Manager.

## Siguientes pasos

[Habilitar componentes del portal de Forms](./forms-portal-components.md)
