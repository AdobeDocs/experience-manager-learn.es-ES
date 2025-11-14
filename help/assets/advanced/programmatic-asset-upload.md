---
title: Carga de recursos mediante programación en AEM as a Cloud Service
description: Obtenga información sobre cómo cargar recursos en AEM as a Cloud Service mediante la biblioteca @adobe/aem-upload de Node.js.
version: Experience Manager as a Cloud Service
topic: Development, Content Management
feature: Asset Management
role: Developer, Architect
level: Intermediate
last-substantial-update: 2025-11-14T00:00:00Z
doc-type: Tutorial
jira: KT-19571
thumbnail: KT-19571.png
source-git-commit: bf996405c360c77475d9f76d5de9bcd4fde3c163
workflow-type: tm+mt
source-wordcount: '1585'
ht-degree: 1%

---


# Carga de recursos mediante programación en AEM as a Cloud Service

Aprenda a cargar recursos en el entorno de AEM as a Cloud Service mediante la aplicación cliente que usa la biblioteca [aem-upload](https://github.com/adobe/aem-upload) Node.js.

## Qué aprenderá

En este tutorial, aprenderá lo siguiente:

+ Cómo usar el método de _carga binaria directa_ para cargar recursos al entorno de AEM as a Cloud Service (RDE, Dev, Stage, Prod) usando la biblioteca [aem-upload](https://github.com/adobe/aem-upload) Node.js.
+ Configurar y ejecutar la aplicación [aem-asset-upload-sample](./assets/programmatic-asset-upload/aem-asset-upload-sample.zip) para cargar recursos en el entorno de AEM as a Cloud Service.
+ Revise el código de la aplicación de ejemplo y comprenda los detalles de la implementación.
+ Conozca las prácticas recomendadas para la carga programática de recursos en el entorno de AEM as a Cloud Service.

## Entender el enfoque de _carga binaria directa_

El método de _carga binaria directa_ le permite cargar archivos de su sistema de origen _directamente al almacenamiento en la nube_ en el entorno de AEM as a Cloud Service mediante una _URL con firma previa_. Esto elimina la necesidad de enrutar los datos binarios a través de los procesos Java de AEM, lo que resulta en cargas más rápidas y una carga del servidor reducida.

Antes de ejecutar la aplicación de ejemplo, vamos a comprender el flujo de carga binaria directa.

En el flujo de carga binaria directa, los datos binarios se cargan directamente en el almacenamiento en la nube con direcciones URL prefirmadas. AEM as a Cloud Service es responsable del procesamiento ligero, como la generación de las direcciones URL prefirmadas y la notificación al servicio AEM Asset Compute de la finalización de la carga. El siguiente diagrama de flujo lógico ilustra el flujo de carga binario directo.

![Flujo de carga binaria directa](./assets/programmatic-asset-upload/direct-binary-asset-upload-flow.png)

### La biblioteca de carga de aem

La biblioteca [aem-upload](https://github.com/adobe/aem-upload) Node.js resume los detalles de implementación del enfoque _carga binaria directa_. Proporciona dos clases para organizar el proceso de carga:

+ **FileSystemUpload**: utilícelo al cargar archivos del sistema de archivos local, incluida la compatibilidad con estructuras de directorio
+ **DirectBinaryUpload**: utilícelo para tener un control más preciso sobre el proceso de carga binaria, como la carga desde secuencias o búferes

>[!CAUTION]
>
>NO hay ningún equivalente de la biblioteca [aem-upload](https://github.com/adobe/aem-upload) en Java. La aplicación cliente debe escribirse en Node.js para utilizar el método _direct binary upload_. Para obtener más información, consulte la página [API de Experience Manager Assets y operaciones](https://experienceleague.adobe.com/es/docs/experience-manager-cloud-service/content/assets/admin/developer-reference-material-apis#use-cases-and-apis).

## Aplicación de ejemplo

Utilice la aplicación [aem-asset-upload-sample](./assets/programmatic-asset-upload/aem-asset-upload-sample.zip) para conocer el proceso programático de carga de recursos. La aplicación de ejemplo muestra el uso de las clases `FileSystemUpload` y `DirectBinaryUpload` de la biblioteca [aem-upload](https://github.com/adobe/aem-upload).

### Requisitos previos

Antes de ejecutar la aplicación de ejemplo, asegúrese de que tiene los siguientes requisitos previos:

+ Entorno de creación de AEM as a Cloud Service, como el entorno de desarrollo rápido (RDE), el entorno de desarrollo, etc.
+ Node.js (última versión de LTS)
+ Comprensión básica de Node.js y npm

>[!CAUTION]
>
> NO puede utilizar AEM as a Cloud Service SDK (también conocido como instancia local de AEM) para probar el proceso de carga de recursos mediante programación. Debe utilizar un entorno de AEM as a Cloud Service como un entorno de desarrollo rápido (RDE), un entorno de desarrollo, etc.

### Descargar la aplicación de ejemplo

1. Descargue el archivo zip de la aplicación [aem-asset-upload-sample](./assets/programmatic-asset-upload/aem-asset-upload-sample.zip) y extráigalo.

   ```bash
   $ unzip aem-asset-upload-sample.zip
   ```

1. Abra la carpeta extraída en su editor de código favorito.

   ```bash
   $ cd aem-asset-upload-sample
   $ code .
   ```

1. Mediante el terminal del editor de código, instale las dependencias.

   ```bash
   $ npm install
   ```

   ![Aplicación de ejemplo](./assets/programmatic-asset-upload/install-dependencies.png)

### Configurar la aplicación de ejemplo

Antes de ejecutar la aplicación de ejemplo, debe configurarla con los detalles necesarios del entorno de AEM as a Cloud Service, como la URL del autor de AEM, el _método de autenticación_ y la ruta de la carpeta de recursos.

La biblioteca _aem-upload_ Node.js admite [varios métodos de autenticación](https://github.com/adobe/aem-upload). La siguiente tabla resume los _métodos de autenticación_ admitidos y su propósito.

| | Autenticación básica | [Token de desarrollo local](https://experienceleague.adobe.com/es/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/local-development-access-token) | [Credenciales de servicio](https://experienceleague.adobe.com/es/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials) | [OAuth S2S](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/) | [Aplicación web OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation#oauth-web-app-credential) | [SPA de OAuth](https://developer.adobe.com/developer-console/docs/guides/authentication/UserAuthentication/implementation#oauth-single-page-app-credential) |
|---|---|---|---|---|---|---|
| ¿Es compatible? | &check; | &check; | &check; | &cross; | &cross; | &cross; |
| Función | Desarrollo local | Desarrollo local | Producción | N/D | N/D | N/D |

Para configurar la aplicación de ejemplo, siga los pasos a continuación:

1. Copie el archivo `env.example` en el archivo `.env`.

   ```bash
   $ cp env.example .env
   ```

1. Abra el archivo `.env` y actualice la variable de entorno `AEM_URL` con la dirección URL del autor de AEM as a Cloud Service.

1. Elija el método de autenticación de las siguientes opciones y actualice las variables de entorno correspondientes.

>[!BEGINTABS]

>[!TAB Autenticación básica]

Para utilizar la autenticación básica, debe crear un usuario en el entorno de AEM as a Cloud Service.

1. Inicie sesión en su entorno de AEM as a Cloud Service.

1. Vaya a **Herramientas** > **Seguridad** > **Usuarios** y haga clic en el botón **Crear**.

   ![Crear usuario](./assets/programmatic-asset-upload/create-user.png)

1. Introduzca los detalles del usuario

   ![Detalles del usuario](./assets/programmatic-asset-upload/user-details.png)

1. En la ficha **Grupos**, agregue el grupo **Usuarios de DAM**. Haga clic en el botón **Guardar y cerrar**.

   ![Agregar grupo de usuarios DAM](./assets/programmatic-asset-upload/add-dam-users-group.png)

1. Actualice las variables de entorno `AEM_USERNAME` y `AEM_PASSWORD` con el nombre de usuario y la contraseña del usuario creado.

>[!TAB Token de desarrollo local]

Para obtener el token de desarrollo local, debes usar el Developer Console **AEM**. El token generado es del tipo token web JSON (JWT).

1. Inicie sesión en [Adobe Cloud Manager](https://experience.adobe.com/#/@aem/cloud-manager) y vaya a la página de detalles de **Entorno** que desee. Haga clic en **&quot;...&quot;** y seleccione **Developer Console**.

   ![Developer Console](./assets/programmatic-asset-upload/developer-console.png)

1. Inicie sesión en AEM Developer Console y use el botón _Nueva consola_ para cambiar a la consola más reciente.

1. En la sección **Herramientas**, seleccione **Integraciones** y haga clic en el botón **Obtener token local**.

   ![Obtener token local](./assets/programmatic-asset-upload/get-local-token.png)

1. Copie el valor del token y actualice la variable de entorno `AEM_BEARER_TOKEN` con el valor del token.

Tenga en cuenta que el token de desarrollo local es válido durante 24 horas y se emite para el usuario que generó el token.

>[!TAB Credenciales de servicio]

Para obtener las credenciales del servicio, debes usar el Developer Console **AEM**. Se usa para generar el token de tipo JSON Web Token (JWT) usando el módulo [jwt-auth](https://www.npmjs.com/package/@adobe/jwt-auth) npm.

1. Inicie sesión en [Adobe Cloud Manager](https://experience.adobe.com/#/@aem/cloud-manager) y vaya a la página de detalles de **Entorno** que desee. Haga clic en **&quot;...&quot;** y seleccione **Developer Console**.

   ![Developer Console](./assets/programmatic-asset-upload/developer-console.png)

1. Inicie sesión en AEM Developer Console y use el botón _Nueva consola_ para cambiar a la consola más reciente.

1. En la sección **Herramientas**, seleccione **Integraciones** y haga clic en el botón **Crear nueva cuenta técnica**.

   ![Obtener credenciales de servicio](./assets/programmatic-asset-upload/get-service-credentials.png)

1. Haga clic en la opción **Ver** para copiar las credenciales del servicio JSON.

   ![Credenciales de servicio](./assets/programmatic-asset-upload/service-credentials.png)

1. Cree un archivo `service-credentials.json` en la raíz de la aplicación de ejemplo y pegue las credenciales del servicio JSON en el archivo.

1. Actualice la variable de entorno `AEM_SERVICE_CREDENTIALS_FILE` con la ruta al archivo service-credentials.json.

1. Asegúrese de que el usuario de credenciales de servicio tenga los permisos necesarios para cargar recursos en el entorno de AEM as a Cloud Service. Para obtener más información, consulte la página [Configurar el acceso en AEM](https://experienceleague.adobe.com/es/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials#configure-access-in-aem).

>[!ENDTABS]

Este es un archivo de muestra `.env` completo con los tres métodos de autenticación configurados.

```
# AEM Environment Configuration
# Copy this file to .env and fill in your AEM as a Cloud Service details

# AEM as a Cloud Service Author URL (without trailing slash)
# Example: https://author-p12345-e67890.adobeaemcloud.com
AEM_URL=https://author-p63947-e1733365.adobeaemcloud.com

# Upload Configuration
# Target folder in AEM DAM where assets will be uploaded
TARGET_FOLDER=/content/dam

# DirectBinaryUpload Remote URLs (required for DirectBinaryUpload example)
# URLs for remote files to upload in the DirectBinaryUpload example
# These demonstrate uploading from remote sources (URLs, CDNs, APIs)
REMOTE_FILE_URL_1=https://placehold.co/600x400/red/white?text=Adobe+Experience+Manager+Assets

################################################################
# Authentication - Choose one of the following methods:
################################################################

# Method 1: Service Credentials (RECOMMENDED for production)
# Download service credentials JSON from AEM Developer Console and save it locally
# Then provide the path to the file here
AEM_SERVICE_CREDENTIALS_FILE=./service-credentials.json

# Method 2: Bearer Token Authentication (for manual testing)
AEM_BEARER_TOKEN=eyJhbGciOiJSUzI1NiIsIng1dSI6Imltc19uYTEta2V5LWF0LTEuY2VyIiwia2lkIjoiaW1zX25hM....fsdf-Rgt5hm_8FHutTyNQnkj1x1SUs5OkqUfJaGBaKBKdqQ

# Method 3: Basic Authentication (for development/testing only)
AEM_USERNAME=asset-uploader-local-user
AEM_PASSWORD=asset-uploader-local-user

# Optional: Enable detailed logging
DEBUG=false
```

### Ejecute la aplicación de ejemplo

La aplicación de ejemplo muestra tres formas diferentes de cargar recursos de muestra en el entorno de AEM as a Cloud Service.

1. **FileSystemUpload** - Cargar archivos desde un sistema de archivos local con compatibilidad con la estructura de directorios y creación automática de carpetas
1. **DirectBinaryUpload** - Carga un [archivo remoto](https://placehold.co/600x400/red/white?text=Adobe+Experience+Manager+Assets). El binario de archivo se almacena en búfer antes de cargarse en el entorno de AEM as a Cloud Service.
1. **Carga por lotes**: carga varios archivos desde un sistema de archivos local por lotes con lógica de reintento automática y recuperación de errores. En segundo plano, utiliza la clase `FileSystemUpload` para cargar archivos desde el sistema de archivos local.

Los recursos que se van a cargar se encuentran en la carpeta `sample-assets` y contienen las subcarpetas `img`, `video` y `doc`, cada una de las cuales contiene algunos recursos de ejemplo.

1. Para ejecutar la aplicación de ejemplo, utilice el siguiente comando:

```bash
$ npm start
```

1. Escriba la opción _number_ que desee entre las siguientes opciones:

```
╔════════════════════════════════════════════════════════════╗
║      AEM Asset Upload Sample Application                   ║
║      Demonstrating @adobe/aem-upload library               ║
╚════════════════════════════════════════════════════════════╝

Choose an upload method:

1. FileSystemUpload - Upload files from local filesystem with auto-folder creation
2. DirectBinaryUpload - Upload from remote URLs/streams to AEM
3. Batch Upload - Upload multiple files in batches with retry logic
4. Exit
```

Las siguientes pestañas muestran la ejecución de la aplicación de ejemplo, su salida y los recursos cargados en el entorno de AEM as a Cloud Service para cada método de carga.

>[!BEGINTABS]

>[!TAB FileSystemUpload]

1. Salida de la aplicación de ejemplo para la opción `FileSystemUpload`:

```bash
...
Upload Summary:
──────────────────────────────────────────────────
Total files: 5
Successful: 5
Failed: 0
Total time: 2.67s
──────────────────────────────────────────────────
✓ 
All files uploaded successfully!
```

1. Assets cargado con la opción `FileSystemUpload` en el entorno de AEM as a Cloud Service:

   ![Recursos cargados en el entorno de AEM as a Cloud Service mediante la clase FileSystemUpload](./assets/programmatic-asset-upload/uploaded-assets-aem-file-system-upload.png)

>[!TAB DirectBinaryUpload]

1. Salida de la aplicación de ejemplo para la opción `DirectBinaryUpload`:

```bash
...
Upload Summary:
──────────────────────────────────────────────────
Total files: 1
Successful: 1
Total time: 561ms
──────────────────────────────────────────────────

✅ Successfully uploaded to AEM: https://author-p63947-e1733365.adobeaemcloud.com/ui#/aem/assets.html/content/dam?appId=aemshell
  → remote-file-1.png
    Source: https://placehold.co/600x400/red/white?text=Adobe+Experience+Manager+Assets
✓ 
All files uploaded successfully!
```

1. Assets cargado con la opción `DirectBinaryUpload` en el entorno de AEM as a Cloud Service:

![Recursos cargados en el entorno de AEM as a Cloud Service mediante la clase DirectBinaryUpload](./assets/programmatic-asset-upload/uploaded-assets-aem-direct-binary-upload.png)

>[!TAB Carga por lotes]

1. Salida de la aplicación de ejemplo para la opción `Batch Upload`:

```bash
...
ℹ Found 4 item(s) to upload in batches (directories + files)
ℹ Batch size: 2 (small for demo, use 10-50 for production)

...

✓ Batch 2 completed in 2.79s

Upload Summary:
──────────────────────────────────────────────────
Total files: 5
Successful: 5
Failed: 0
Total time: 4.50s
──────────────────────────────────────────────────
✓ 
All files uploaded successfully!
```

1. Assets cargado con la opción `Batch Upload` en el entorno de AEM as a Cloud Service:

![Recursos cargados en el entorno de AEM as a Cloud Service mediante la clase BatchUpload](./assets/programmatic-asset-upload/uploaded-assets-aem-batch-upload.png)

>[!ENDTABS]

## Revise el código de aplicación de ejemplo

El punto de entrada principal de la aplicación de ejemplo es el archivo `index.js`. Contiene la función `promptUser` que solicita al usuario una opción y ejecuta el ejemplo seleccionado.

```javascript
/**
 * Prompts user for choice and executes the selected example
 */
function promptUser() {
  rl.question(chalk.bold('Enter your choice (1-4): '), async (answer) => {
    console.log('');

    try {
      switch (answer.trim()) {
        case '1':
          console.log(chalk.bold.green('\n▶ Running FileSystemUpload Example...\n'));
          await filesystemUpload.main();
          break;

        case '2':
          console.log(chalk.bold.green('\n▶ Running DirectBinaryUpload Example...\n'));
          await directBinaryUpload.main();
          break;

        case '3':
          console.log(chalk.bold.green('\n▶ Running Batch Upload Example...\n'));
          await batchUpload.main();
          break;

        case '4':
          rl.close();
          return;

        default:
          console.log(chalk.red('\n✗ Invalid choice. Please enter 1, 2, 3, or 4.\n'));
      }

      // After example completes, ask if user wants to continue
      rl.question(chalk.bold('\nPress Enter to return to menu or Ctrl+C to exit...'), () => {
        displayMenu();
        promptUser();
      });

    } catch (error) {
      console.error(chalk.red('\n✗ Error:'), error.message);
      rl.question(chalk.bold('\nPress Enter to return to menu...'), () => {
        displayMenu();
        promptUser();
      });
    }
  });
}
```

Para obtener el código completo, consulte el archivo `index.js` de la aplicación de ejemplo.

Las siguientes pestañas muestran los detalles de implementación de cada método de carga.

>[!BEGINTABS]

>[!TAB FileSystemUpload]

La clase `FileSystemUpload` se usa para cargar archivos del sistema de archivos local con compatibilidad con la estructura de directorios y creación automática de carpetas.

```javascript
...
// Initialize FileSystemUpload
const upload = new FileSystemUpload();

const startTime = Date.now();
const spinner = createSpinner('Preparing upload...');

// Upload options for this specific upload
// For FileSystemUpload, the url should include the target folder path
const fullUrl = `${options.url}${targetFolder}`;

const uploadOptions = new FileSystemUploadOptions()
  .withUrl(fullUrl)
  .withDeepUpload(true);  // Enable recursive upload of subdirectories

// Add HTTP options including headers (auth is already in headers from config)
uploadOptions.withHttpOptions({
  headers: {
    ...options.headers,
    'X-Upload-Source': 'FileSystemUpload-Example'
  }
});

spinner.stop();

// Attach progress event handlers to the upload instance
handleUploadProgress(upload);

// Perform the upload and wait for completion
// Upload the contents (subdirectories and files) not the parent folder
const uploadResult = await upload.upload(uploadOptions, uploadPaths);
const totalTime = Date.now() - startTime;

// Analyze results using shared function
const analysis = analyzeUploadResult(uploadResult);

// Display summary
displayUploadSummary(analysis, totalTime);
...
```

Para obtener el código completo, consulte el archivo `examples/filesystem-upload.js` de la aplicación de ejemplo.

>[!TAB DirectBinaryUpload]

La clase `DirectBinaryUpload` se usa para cargar un archivo remoto en el entorno de AEM as a Cloud Service.

```javascript
...
/**
 * Creates upload file objects for DirectBinaryUpload from remote URLs
 * @param {Array<Object>} remoteFiles - Array of objects with url, fileName, targetFolder
 * @returns {Array<Object>} Array of upload file objects
 */
async function createUploadFilesFromUrls(remoteFiles) {
  const uploadFiles = [];
  
  for (const remoteFile of remoteFiles) {
    logInfo(`Fetching: ${remoteFile.fileName} from ${remoteFile.url}`);
    try {
      const fileBuffer = await fetchRemoteFile(remoteFile.url);
      uploadFiles.push({
        fileName: remoteFile.fileName,
        fileSize: fileBuffer.length,
        blob: fileBuffer,  // DirectBinaryUpload uses 'blob' for buffers
        targetFolder: remoteFile.targetFolder,
        targetFile: `${remoteFile.targetFolder}/${remoteFile.fileName}`,
        sourceUrl: remoteFile.url  // Track source URL for display in summary
      });
      logSuccess(`Downloaded: ${remoteFile.fileName} (${formatBytes(fileBuffer.length)})`);
    } catch (error) {
      logError(`Failed to fetch ${remoteFile.fileName}: ${error.message}`);
    }
  }
  
  return uploadFiles;
}

...

    // Initialize DirectBinaryUpload
    const upload = new DirectBinaryUpload();

    // Fetch remote files and create upload objects
    const uploadFiles = await createUploadFilesFromUrls(remoteFiles);

...    

    // Upload options for each file
    const uploadOptions = new DirectBinaryUploadOptions()
        .withUrl(fullUrl)
        .withUploadFiles([uploadFile]);
    
    // Add HTTP options (auth is already in headers from config)
    uploadOptions
        .withHttpOptions({
        headers: {
            ...options.headers,
            'X-Upload-Source': 'DirectBinaryUpload-Example'
        }
        })
        .withMaxConcurrent(5);

    // Upload individual file and wait for completion
    const uploadResult = await upload.uploadFiles(uploadOptions);
```

Para obtener el código completo, consulte el archivo `examples/direct-binary-upload.js` de la aplicación de ejemplo.

>[!TAB Carga por lotes]

Divide los archivos en lotes y los carga en lotes con lógica de reintento automática y recuperación de errores. En segundo plano, utiliza la clase `FileSystemUpload` para cargar archivos desde el sistema de archivos local.

```javascript
...
async function uploadInBatches(paths, options, targetFolder, batchSize = 2) {
  const allResults = [];
  const totalPaths = paths.length;
  const totalBatches = Math.ceil(totalPaths / batchSize);

  logInfo(`Processing ${totalPaths} item(s) in ${totalBatches} batch(es)`);

  for (let i = 0; i < totalPaths; i += batchSize) {
    const batchNumber = Math.floor(i / batchSize) + 1;
    const batch = paths.slice(i, i + batchSize);
    
    console.log(`\n${'='.repeat(50)}`);
    logInfo(`Batch ${batchNumber}/${totalBatches} - Uploading ${batch.length} item(s)`);
    console.log('='.repeat(50));

    const batchStartTime = Date.now();
    let retryCount = 0;
    const maxRetries = 3;
    let batchResults = null;

    // Retry logic for failed batches
    while (retryCount <= maxRetries) {
      try {
        // Create a fresh upload instance for each retry to avoid duplicate event listeners
        const upload = new FileSystemUpload();
        
        const fullUrl = `${options.url}${targetFolder}`;
        
        const uploadOptions = new FileSystemUploadOptions()
          .withUrl(fullUrl)
          .withDeepUpload(true);  // Enable recursive upload of subdirectories
        
        // Add HTTP options including headers (auth is already in headers from config)
        uploadOptions.withHttpOptions({
          headers: {
            ...options.headers,
            'X-Upload-Source': 'Batch-Upload-Example',
            'X-Batch-Number': batchNumber
          }
        });

        // Track progress - attach listeners to upload instance
        upload.on('foldercreated', (data) => {
          logSuccess(`Created folder: ${data.folderName} at ${data.targetFolder}`);
        });
        
        let currentFile = '';
        upload.on('filestart', (data) => {
          currentFile = data.fileName;
          logInfo(`Starting: ${currentFile}`);
        });

        upload.on('fileprogress', (data) => {
          const percentage = ((data.transferred / data.fileSize) * 100).toFixed(1);
          process.stdout.write(
            `\r  Progress: ${percentage}% - ${formatBytes(data.transferred)}/${formatBytes(data.fileSize)}`
          );
        });

        upload.on('fileend', (data) => {
          process.stdout.write('\n');
          logSuccess(`Completed: ${data.fileName}`);
        });

        upload.on('fileerror', (data) => {
          // Only show in DEBUG mode (may be retries)
          if (process.env.DEBUG === 'true') {
            process.stdout.write('\n');
            const errorMsg = data.error?.message || data.message || 'Unknown error';
            logWarning(`Error (may retry): ${data.fileName} - ${errorMsg}`);
          }
        });

        // Perform upload and wait for batch completion
        const uploadResult = await upload.upload(uploadOptions, batch);
        
        const batchEndTime = Date.now();
        const batchTime = batchEndTime - batchStartTime;
        
        logSuccess(`Batch ${batchNumber} completed in ${formatTime(batchTime)}`);
        
        // Extract detailed results from the upload result
        batchResults = uploadResult.detailedResult || [];
        break; // Success, exit retry loop

      } catch (error) {
        retryCount++;
        if (retryCount <= maxRetries) {
          logWarning(`Batch ${batchNumber} failed. Retry ${retryCount}/${maxRetries}...`);
          await new Promise(resolve => setTimeout(resolve, 2000 * retryCount)); // Exponential backoff
        } else {
          logError(`Batch ${batchNumber} failed after ${maxRetries} retries: ${error.message}`);
          // Mark all files in batch as failed
          batchResults = batch.map(file => ({
            fileName: path.basename(file),
            error: error,
            success: false
          }));
        }
      }
    }

    if (batchResults) {
      allResults.push(...batchResults);
    }
  }

  return allResults;
}
```

Para obtener el código completo, consulte el archivo `examples/batch-upload.js` de la aplicación de ejemplo.

>[!ENDTABS]

Además, el archivo `README.md` de la aplicación de ejemplo contiene la documentación detallada de la aplicación de ejemplo.

## Prácticas recomendadas

1. **Elija el método de autenticación adecuado:**
Utilice credenciales de servicio para entornos de producción, token de desarrollo local y autenticación básica solo para desarrollo y pruebas. Asegúrese de que el usuario de credenciales de servicio tenga los permisos necesarios para cargar recursos en el entorno de AEM as a Cloud Service.

1. **Elija el método de carga correcto:**
Utilice FileSystemUpload para archivos locales con creación automática de carpetas, DirectBinaryUpload para secuencias, búferes o direcciones URL remotas con control preciso y patrón de carga por lotes para entornos de producción con más de 1000 archivos que requieren lógica de reintento.

1. **Estructura correctamente los objetos del archivo DirectBinaryUpload**
Utilice la propiedad blob (no el búfer) con los campos obligatorios: { fileName, fileSize, blob: buffer, targetFolder } y recuerde que DirectBinaryUpload NO crea carpetas automáticamente.

1. **Aplicación de muestra como referencia:**
La aplicación de ejemplo es una buena referencia para los detalles de implementación del proceso de carga de recursos mediante programación. Puede utilizarlo como punto de partida para su propia implementación.
