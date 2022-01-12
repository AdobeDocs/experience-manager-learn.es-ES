---
title: Combinación de datos con la plantilla XDP
description: Realizar una solicitud de POST al punto final con los parámetros necesarios
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
kt: 8185
thumbnail: 332439.jpg
source-git-commit: f712e86600ed18aee43187a5fb105324b14b7b89
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# Realizar la llamada del POST


El siguiente paso es hacer una llamada de POST HTTP al extremo con los parámetros necesarios. La plantilla y los archivos de datos se proporcionan como archivos de recursos. Las propiedades del pdf generado se especifican mediante el parámetro de la opción en la solicitud. La propiedad embedFonts se utiliza para incrustar fuentes personalizadas en el pdf generado.[Siga esta documentación para implementar fuentes personalizadas en la instancia de nube de Forms.](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/intellij-set-up.html?lang=en) Las propiedades se especifican en el archivo de recursos options.json . Desde entonces, el punto final tiene autenticación basada en tokens, pasamos el token de acceso en el encabezado de la solicitud.

El siguiente código se utilizó para generar pdf al combinar datos con la plantilla

```java
public class DocumentGeneration
{
        public String SAVE_LOCATION = "c:\\aspire1";
        public void mergeDataWithXdpTemplate(String postURL)
        {
                HttpPost httpPost = new HttpPost(postURL);
                CredentialUtilites cu = new CredentialUtilites();
                String accessToken = cu.getAccessToken();
                httpPost.addHeader("Authorization", "Bearer " + accessToken);
                ClassLoader classLoader = DocumentGeneration.class.getClassLoader();
                URL templateFile = classLoader.getResource("templates/custom_fonts.xdp");
                File xdpTemplate = new File(templateFile.getPath());
                URL url = classLoader.getResource("datafiles");
                System.out.println(url.getPath());
                File files[] = new File(url.getPath()).listFiles();
                for (int i = 0; i < files.length; i++) {
                        MultipartEntityBuilder builder = MultipartEntityBuilder.create();
                        ContentType strContent = ContentType.create("text/plain", Charset.forName("UTF-8"));
                        builder.setMode(HttpMultipartMode.BROWSER_COMPATIBLE);
                        builder.addBinaryBody("data", files[i]);
                        builder.addBinaryBody("template", xdpTemplate);
                        builder.addBinaryBody("options",GetOptions.getPDFOptions().getBytes(),ContentType.APPLICATION_JSON,"options"
                        try {
                                HttpEntity entity = builder.build();
                                httpPost.setEntity(entity);
                                CloseableHttpClient httpclient = HttpClients.createDefault();
                                CloseableHttpResponse response = httpclient.execute(httpPost);
                                InputStream generatedPDF = response.getEntity().getContent();
                                byte[] bytes = IOUtils.toByteArray(generatedPDF);
                                File saveLocation = new File(SAVE_LOCATION);
                                if (!saveLocation.exists()) {
                                        saveLocation.mkdirs();
                                }
                                File outputFile = new File(SAVE_LOCATION+File.separator+files[i].getName().replace("xml", "pdf"));
                                try (FileOutputStream outputStream = new FileOutputStream(outputFile)) {
                                        outputStream.write(bytes);
                                }
                        } catch (Exception e) {
                                System.out.println("The error is " + e.getMessage());
                        }

                }
                System.out.println("Done generating " + files.length + " files");

        }

}
```
