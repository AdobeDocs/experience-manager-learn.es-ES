---
title: AEM Función URL mnemónicas de Dispatcher
description: AEM Comprenda cómo trata la con las URL de vanidad y las técnicas adicionales mediante el uso de reglas de reescritura para asignar contenido más cerca del borde del envío.
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 53baef9c-aa4e-4f18-ab30-ef9f4f5513ee
duration: 286
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '988'
ht-degree: 0%

---

# URL mnemónicas de Dispatcher

[Tabla de contenidos](./overview.md)

[&lt;- Anterior: Vaciado de Dispatcher](./disp-flushing.md)

## Información general

AEM Este documento le ayuda a comprender cómo se trata el tema de las direcciones URL mnemónicas y algunas técnicas adicionales mediante el uso de reglas de reescritura para asignar contenido más cerca del borde del envío

## ¿Qué son las URL mnemónicas?

Cuando tiene contenido que se aloja en una estructura de carpetas lógica, no siempre vive en una dirección URL de referencia fácil. Las URL mnemónicas son como accesos directos. Direcciones URL únicas o más cortas que hacen referencia al lugar donde se encuentra el contenido real.

Un ejemplo: `/aboutus` apuntado `/content/we-retail/us/en/about-us.html`

AEM AEM Los autores de tienen la opción de establecer propiedades de URL mnemónicas en un fragmento de contenido de y publicarlo.

Para que funcione, debe ajustar los filtros de Dispatcher para permitir el elemento mnemónico. Esto no es razonable con el ajuste de los archivos de configuración de Dispatcher a la velocidad que los autores tendrían para configurar estas entradas de página mnemónicas.

Por este motivo, el módulo de Dispatcher tiene una función para permitir automáticamente cualquier elemento enumerado como mnemónico en el árbol de contenido.


## Funcionamiento

### Creación de URL mnemónicas

AEM El autor visita una página en, hace clic en las propiedades de la página y agrega entradas en la barra de herramientas de, que se encuentra en la barra de herramientas de _URL de vanidad_ sección. Al guardar los cambios y activar la página, el elemento mnemónico se asigna a la página.

Los autores también pueden seleccionar la _Redirigir URL de vanidad_ casilla de verificación al añadir _URL de vanidad_ , esto hace que las direcciones url mnemónicas se comporten como redirecciones 302. Significa que se indica al explorador que vaya a la nueva URL (a través de ) `Location` encabezado de respuesta) y el explorador realiza una nueva solicitud a la nueva URL.

#### IU táctil:

![AEM Menú desplegable del cuadro de diálogo para la interfaz de usuario de creación de en la pantalla del editor del sitio](assets/disp-vanity-url/aem-page-properties-drop-down.png "aem-page-properties-drop-down")

![página de aem: página de diálogo propiedades](assets/disp-vanity-url/aem-page-properties.png "aem-page-properties")

#### Buscador de contenido clásico:

![AEM propiedades de página de la barra de tareas de iu clásica de siteadmin](assets/disp-vanity-url/aem-page-properties-sidekick.png "aem-page-properties-sidekick")

![Cuadro de diálogo Propiedades de página de IU clásica](assets/disp-vanity-url/aem-page-properties-classic.png "aem-page-properties-classic")


>[!NOTE]
>
>Comprenda que esto es propenso a problemas del área de nombres. Las entradas mnemónicas son globales para todas las páginas. Esta es solo una de las deficiencias que debe planificar para encontrar algunas soluciones que explicaremos más adelante.


## Asignación/resolución de recursos

Cada entrada mnemónica es una entrada de mapa sling para una redirección interna.

AEM Los mapas se pueden ver en la consola de instancias de Félix de la página de inicio de la aplicación ( `/system/console/jcrresolver` )

Esta es una captura de pantalla de una entrada de mapa creada por una entrada mnemónica:
![captura de pantalla de consola de una entrada mnemónica en las reglas de resolución de recursos](assets/disp-vanity-url/vanity-resource-resolver-entry.png "vanity-resource-resolver-entry")

AEM En el ejemplo anterior, cuando solicitamos a la instancia de que visite `/aboutus` se resuelve en `/content/we-retail/us/en/about-us.html`

## Filtros de permiso automático de Dispatcher

Dispatcher en un estado seguro filtra las solicitudes en la ruta `/` a través de Dispatcher porque esa es la raíz del árbol JCR.

Es importante asegurarse de que los editores solo permitan contenido desde `/content` y otras rutas seguras, etc., y no rutas como `/system`.

Aquí están las URL mnemónicas activas en la carpeta base de `/` entonces, ¿cómo les permitimos llegar a los editores mientras se mantienen seguros?

AEM El Dispatcher simple tiene un mecanismo de autorización de filtro automático y debe instalar un paquete de y luego configurar el Dispatcher para que se oriente a esa página del paquete.

[https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components](https://experience.adobe.com/#/downloads/content/software-distribution/es/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components)

Dispatcher tiene una sección de configuración en su archivo de granja de servidores:

```
/vanity_urls { 
    /url    "/libs/granite/dispatcher/content/vanityUrls.html" 
    /file   "/tmp/vanity_urls" 
    /delay  300 
}
```

AEM Esta configuración indica a Dispatcher que recupere esta URL de su instancia de que se abre cada 300 segundos para recuperar la lista de los elementos que queremos permitir.

Almacena su caché de la respuesta en la `/file` argumento así en este ejemplo `/tmp/vanity_urls`

AEM Por lo tanto, si visita la instancia de en la URI, verá lo que arroja:
![captura de pantalla del contenido representado en /libs/granite/dispatcher/content/vanityUrls.html](assets/disp-vanity-url/vanity-url-component.png "vanity-url-component")

Es literalmente una lista, súper simple

## Reescribir reglas como reglas mnemónicas

AEM ¿Por qué mencionamos el uso de reglas de reescritura en lugar del mecanismo predeterminado integrado en el código de tiempo de ejecución, tal como se describe más arriba?

Se explican sencillamente los problemas de área de nombres, rendimiento y lógica de nivel superior que se pueden gestionar mejor.

Veamos un ejemplo de la entrada mnemónica `/aboutus` a su contenido `/content/we-retail/us/en/about-us.html` uso de Apache `mod_rewrite` para lograr esto.

```
RewriteRule ^/aboutus /content/we-retail/us/en/about-us.html [PT,L,NC]
```

Esta regla busca la vanidad `/aboutus` y obtenga la ruta completa del procesador con el indicador PT (Pasar).

También detiene el procesamiento de todas las demás reglas del indicador L (Última), lo que significa que no tiene que atravesar una enorme lista de reglas como la resolución JCR.

AEM Además de no tener que representar la solicitud y esperar a que el editor de la responda a estos dos elementos de este método, se hace mucho más eficaz.

A continuación, la cereza del pastel aquí es el indicador NC (sin distinción de mayúsculas y minúsculas), que significa si un cliente escribe la URI con `/AboutUs` en lugar de `/aboutus` todavía funciona.

Para crear una regla de reescritura, debe crear un archivo de configuración en Dispatcher (por ejemplo: `/etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules`) e inclúyalo en el `.vhost` que administra el dominio que necesita aplicar estas url mnemónicas.

Este es un ejemplo de fragmento de código de inclusión dentro de `/etc/httpd/conf.d/enabled_vhosts/we-retail.vhost`

```
<VirtualHost *:80> 
 ServerName weretail.com 
 ServerAlias www.weretail.com 
        ........ SNIP ........ 
 <IfModule mod_rewrite.c> 
  ReWriteEngine on 
  LogLevel warn rewrite:info 
  Include /etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules 
 </IfModule> 
        ........ SNIP ........ 
</VirtualHost>
```

## Qué método y dónde

AEM Usar para controlar las entradas mnemónicas tiene las siguientes ventajas

- Los autores pueden crearlos sobre la marcha
- Se alojan con el contenido y se pueden empaquetar con este

Uso de `mod_rewrite` para controlar las entradas personales tiene las siguientes ventajas

- Resolución de contenido más rápida
- Más cerca del límite de las solicitudes de contenido de usuario final
- Más extensibilidad y opciones para controlar cómo se asigna el contenido a otras condiciones
- Puede distinguir entre mayúsculas y minúsculas

Utilice ambos métodos, pero aquí están los consejos y criterios que debe utilizar cuando:

- AEM Si el elemento mnemónico es temporal y tiene un tráfico planificado bajo, use la función integrada de la tarjeta de acceso de la plataforma de datos de la plataforma de datos de
- Si el elemento mnemónico es un punto final básico que no cambia con frecuencia y tiene uso frecuente, utilice un `mod_rewrite` regla.
- Si el área de nombres mnemónica (por ejemplo: `/aboutus`AEM ) se debe reutilizar para muchas marcas en la misma instancia de y, a continuación, utilizar reglas de reescritura.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

AEM Si desea utilizar la función mnemónica y evitar el área de nombres, puede crear una convención de nombres. Usar direcciones URL mnemónicas anidadas como `/brand1/aboutus`, `brand2/aboutus`, `brand3/aboutus`.
</div>

[Siguiente -> Registro común](./common-logs.md)
