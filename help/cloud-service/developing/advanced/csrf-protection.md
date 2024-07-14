---
title: Protección CSRF
description: AEM Obtenga información sobre cómo generar y agregar tokens de CSRF de a las solicitudes de POST, PUT AEM y eliminación permitidas a los usuarios autenticados para que las utilicen de forma.
version: Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
doc-type: Code Sample
last-substantial-update: 2023-07-14T00:00:00Z
jira: KT-13651
thumbnail: KT-13651.jpeg
exl-id: 747322ed-f01a-48ba-a4a0-483b81f1e904
duration: 125
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 0%

---

# Protección CSRF

AEM Obtenga información sobre cómo generar y agregar tokens de CSRF de a las solicitudes de POST, PUT AEM y eliminación permitidas a los usuarios autenticados para que las utilicen de forma.

AEM La autenticación requiere que se envíe un token CSRF válido para las solicitudes HTTP __autenticado__ __POST__, __PUT o __DELETE AEM__ a los servicios Author y Publish de la.

El token CSRF no es necesario para __solicitudes GET__ o __solicitudes anónimas__.

Si no se envía un token CSRF con una solicitud de POST, PUT o DELETE AEM AEM, devuelve un mensaje de respuesta prohibida 403 y, a continuación, registra el error siguiente en el registro de la:

```log
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] isValidRequest: empty CSRF token - rejecting
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] doFilter: the provided CSRF token is invalid
```

AEM Consulte la [documentación de para obtener más información sobre cómo proteger la CSRF de la](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html).


## Biblioteca de cliente CSRF

AEM proporciona una biblioteca de cliente que se puede utilizar para generar y agregar tokens XHR de CSRF y solicitudes de POST de formularios mediante el parche de funciones de prototipo principales. La funcionalidad la proporciona la categoría de biblioteca de cliente `granite.csrf.standalone`.

Para utilizar este enfoque, agregue `granite.csrf.standalone` como dependencia a la biblioteca de cliente que se carga en su página. Por ejemplo, si está usando la categoría de biblioteca de cliente `wknd.site`, agregue `granite.csrf.standalone` como dependencia a la biblioteca de cliente que se carga en su página.

## Envío de formulario personalizado con protección CSRF

Si el uso de la biblioteca de cliente [`granite.csrf.standalone` ](#csrf-client-library) no se ajusta a su caso de uso, puede agregar manualmente un token CSRF a un envío de formulario. El siguiente ejemplo muestra cómo agregar un token CSRF a un envío de formulario.

AEM Este fragmento de código muestra cómo, en el envío del formulario, se puede recuperar el token CSRF de la base de datos de formulario, y cómo se puede agregar a una entrada de formulario denominada `:cq_csrf_token`. Dado que el token CSRF tiene una duración corta, es mejor recuperar y establecer el token CSRF inmediatamente antes de enviar el formulario, lo que garantiza su validez.

```javascript
// Attach submit handler event to form onSubmit
document.querySelector('form').addEventListener('submit', async (event) => {
    event.preventDefault();

    const form = event.target;
    const response = await fetch('/libs/granite/csrf/token.json');
    const json = await response.json();
    
    // Create a form input named ``:cq_csrf_token`` with the CSRF token.
    let csrfTokenInput = form.querySelector('input[name=":cq_csrf_token"]');
    if (!csrfTokenInput?.value) {
        // If the form does not have a CSRF token input, add one.
        form.insertAdjacentHTML('beforeend', `<input type="hidden" name=":cq_csrf_token" value="${json.token}">`);
    } else {
        // If the form already has a CSRF token input, update the value.
        csrfTokenInput.value = json.token;
    }
    // Submit the form with the hidden input containing the CSRF token
    form.submit();
});
```

## Recuperar con protección CSRF

Si el uso de la biblioteca de cliente [`granite.csrf.standalone` ](#csrf-client-library) no se puede permitir para su caso de uso, puede agregar manualmente un token CSRF a un XHR o a las solicitudes de recuperación. El siguiente ejemplo muestra cómo añadir un token CSRF a un XHR realizado con fetch.

AEM Este fragmento de código muestra cómo recuperar un token CSRF de la base de datos y cómo agregarlo a un encabezado de solicitud HTTP `CSRF-Token` de una solicitud de recuperación. Dado que el token CSRF tiene una corta duración, es mejor recuperarlo y configurarlo inmediatamente antes de realizar la solicitud de captura, lo que garantiza su validez.

```javascript
/**
 * Get CSRF token from AEM.
 * CSRF token expire after a few minutes, so it is best to get the token before each request.
 * 
 * @returns {Promise<string>} that resolves to the CSRF token.
 */
async function getCsrfToken() {
    const response = await fetch('/libs/granite/csrf/token.json');
    const json = await response.json();
    return json.token;
}

// Fetch from AEM with CSRF token in a header named 'CSRF-Token'.
await fetch('/path/to/aem/endpoint', {
    method: 'POST',
    headers: {
        'CSRF-Token': await getCsrfToken()
    },
});
```

## Configuración de Dispatcher

AEM Cuando se utilizan tokens CSRF en el servicio de Publish, se debe actualizar la configuración de Dispatcher para permitir solicitudes de GET al punto de conexión del token CSRF. La siguiente configuración permite realizar solicitudes de GET AEM al extremo del token CSRF en el servicio de Publish de. Si no se agrega esta configuración, el extremo del token CSRF devuelve una respuesta 404 No encontrado.

* `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
/0120 { /type "allow" /method "GET" /url "/libs/granite/csrf/token.json" }
...
```
