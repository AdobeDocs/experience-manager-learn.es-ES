---
title: AEM Almacenamiento en caché de variantes de página con as a Cloud Service
description: AEM Obtenga información sobre cómo configurar y utilizar el servicio as a Cloud para admitir variantes de página de almacenamiento en caché.
role: Architect, Developer
topic: Development
feature: CDN Cache, Dispatcher
exl-id: fdf62074-1a16-437b-b5dc-5fb4e11f1355
duration: 149
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '551'
ht-degree: 1%

---

# Almacenar en caché variantes de página

AEM Obtenga información sobre cómo configurar y utilizar el servicio as a Cloud para admitir variantes de página de almacenamiento en caché.

## Casos de uso de ejemplo

+ Cualquier proveedor de servicios que ofrezca un conjunto diferente de ofertas de servicios y las opciones de precios correspondientes en función de la ubicación geográfica del usuario y la caché de páginas con contenido dinámico debe administrarse en CDN y Dispatcher.

+ Un cliente minorista tiene tiendas en todo el país y cada tienda tiene diferentes ofertas según su ubicación y la caché de las páginas con contenido dinámico debe administrarse en CDN y Dispatcher.

## Información general de soluciones

+ Identifique la clave de variante y el número de valores que puede tener. En nuestro ejemplo, variamos por estado de EE. UU., por lo que el número máximo es 50. Esto es lo suficientemente pequeño como para no causar problemas con los límites de variante en la CDN. [Revisar sección de limitaciones de variante](#variant-limitations).

+ AEM El código debe establecer la cookie de __&quot;x-aem-variant&quot;__ al estado preferido del visitante (p. ej., `Set-Cookie: x-aem-variant=NY`) en la respuesta HTTP correspondiente de la solicitud HTTP inicial.

+ Las solicitudes posteriores del visitante envían esa cookie (por ejemplo, `"Cookie: x-aem-variant=NY"`) y la cookie se transforma en el nivel de la CDN en un encabezado predefinido (es decir, `x-aem-variant:NY`) que se pasa a Dispatcher.

+ Una regla de reescritura de Apache modifica la ruta de solicitud para incluir el valor del encabezado en la dirección URL de la página como un selector de Apache Sling (p. ej., `/page.variant=NY.html`). AEM Esto permite que Publicación de la sirva contenido diferente en función del selector y que Dispatcher almacene en caché una página por variante.

+ AEM La respuesta enviada por Dispatcher debe contener un encabezado de respuesta HTTP `Vary: x-aem-variant`. Esto indica a la CDN que almacene diferentes copias de caché para diferentes valores de encabezado.

>[!TIP]
>
>Siempre que se establece una cookie (p. ej. Set-Cookie: x-aem-variant=NY) la respuesta no debe ser almacenable en caché (debe tener Cache-Control: private o Cache-Control: no-cache)

## Flujo de solicitud HTTP

![Flujo de solicitud de caché de variante](./assets/variant-cache-request-flow.png)

>[!NOTE]
>
>El flujo de solicitud HTTP inicial anterior debe tener lugar antes de que se solicite cualquier contenido que utilice variantes.

## Uso

1. Para mostrar la función, utilizaremos [WKND](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=es)La implementación de como ejemplo.

1. Implementación de un [SlingServletFilter](https://sling.apache.org/documentation/the-sling-engine/filters.html) AEM en el conjunto de `x-aem-variant` en la respuesta HTTP, con un valor de variante.

1. AEM El CDN de CDN se transforma automáticamente `x-aem-variant` en un encabezado HTTP con el mismo nombre.

1. Añada una regla mod_rewrite de servidor web Apache a su `dispatcher` proyecto, que modifica la ruta de solicitud para incluir el selector de variantes.

1. Implemente el filtro y reescriba las reglas mediante Cloud Manager.

1. Pruebe el flujo de solicitud general.

## Ejemplos de código

+ Ejemplo de SlingServletFilter para establecer `x-aem-variant` AEM Cookie con un valor en la.

  ```
  package com.adobe.aem.guides.wknd.core.servlets.filters;
  
  import javax.servlet.*;
  import java.io.IOException;
  
  import org.apache.sling.api.SlingHttpServletRequest;
  import org.apache.sling.api.SlingHttpServletResponse;
  import org.apache.sling.servlets.annotations.SlingServletFilter;
  import org.apache.sling.servlets.annotations.SlingServletFilterScope;
  import org.osgi.service.component.annotations.Component;
  import org.slf4j.Logger;
  import org.slf4j.LoggerFactory;
  
  
  // Invoke filter on  HTTP GET /content/wknd.*.foo|bar.html|json requests.
  // This code and scope is for example purposes only, and will not interfere with other requests.
  @Component
  @SlingServletFilter(scope = {SlingServletFilterScope.REQUEST},
          resourceTypes = {"cq:Page"},
          pattern = "/content/wknd/.*",
          extensions = {"html", "json"},
          methods = {"GET"})
  public class PageVariantFilter implements Filter {
      private static final Logger log = LoggerFactory.getLogger(PageVariantFilter.class);
      private static final String VARIANT_COOKIE_NAME = "x-aem-variant";
  
      @Override
      public void init(FilterConfig filterConfig) throws ServletException { }
  
      @Override
      public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
          SlingHttpServletResponse slingResponse = (SlingHttpServletResponse) servletResponse;
          SlingHttpServletRequest slingRequest = (SlingHttpServletRequest) servletRequest;
  
          // Check is the variant was previously set
          final String existingVariant = slingRequest.getCookie(VARIANT_COOKIE_NAME).getValue();
  
          if (existingVariant == null) {
              // Variant has not been set, so set it now
              String newVariant = "NY"; // Hard coding as an example, but should be a calculated value
              slingResponse.setHeader("Set-Cookie", VARIANT_COOKIE_NAME + "=" + newVariant + "; Path=/; HttpOnly; Secure; SameSite=Strict");
              log.debug("x-aem-variant cookie is set with the value {}", newVariant);
          } else {
              log.debug("x-aem-variant previously set with value {}", existingVariant);
          }
  
          filterChain.doFilter(servletRequest, slingResponse);
      }
  
      @Override
      public void destroy() { }
  }
  ```

+ Ejemplo de regla de reescritura en __dispatcher/src/conf.d/rewrite.rules__ que se administra como código fuente en Git e implementa mediante Cloud Manager.

  ```
  ...
  
  RewriteCond %{REQUEST_URI} ^/us/.*  
  RewriteCond %{HTTP:x-aem-variant} ^.*$  
  RewriteRule ^([^?]+)\.(html.*)$ /content/wknd$1.variant=%{HTTP:x-aem-variant}.$2 [PT,L] 
  
  ...
  ```

## Limitaciones de variante

+ AEM DN puede gestionar hasta 200 variaciones. Eso significa que `x-aem-variant` puede tener hasta 200 valores únicos. Para obtener más información, consulte [Límites de configuración de CDN](https://docs.fastly.com/en/guides/resource-limits).

+ Debe asegurarse de que la clave de variante elegida nunca supere este número.  Por ejemplo, un ID de usuario no es una buena clave, ya que fácilmente superaría los 200 valores para la mayoría de los sitios web, mientras que los estados o territorios de un país son mejores si hay menos de 200 estados en ese país.

>[!NOTE]
>
>Cuando las variantes superen las 200, la CDN responderá con la respuesta &quot;Demasiadas variantes&quot; en lugar del contenido de la página.
