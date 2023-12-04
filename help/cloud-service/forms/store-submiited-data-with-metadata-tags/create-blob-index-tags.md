---
title: Tutorial para añadir etiquetas de metadatos especificadas por el usuario
description: Crear un envío personalizado para almacenar los datos del formulario con etiquetas de metadatos en Azure
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14501
duration: 71
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '115'
ht-degree: 1%

---

# Añadir etiquetas de índice de blob

A medida que los conjuntos de datos se hacen más grandes, puede resultar difícil encontrar un objeto específico en un mar de datos. Las etiquetas de índice de blob proporcionan funcionalidades de detección y administración de datos mediante atributos de etiqueta de índice de valor clave. Puede categorizar y buscar objetos dentro de un solo contenedor o en todos los contenedores de su cuenta de almacenamiento. Por ejemplo, etiqueta de índice blob _**CustomerType=Platinum**_, donde Platinum es el valor del campo CustomerType.

![index-tags](assets/blob-with-index-tags1.png)
El siguiente código crea la cadena de etiquetas de datos de índice de blob con sus valores correspondientes de los datos enviados

```java
@Override
    public String getMetaDataTags(String submittedFormName,String formPath,Session session,String formData) {

        JsonObject jsonObject = JsonParser.parseString(formData).getAsJsonObject();
        List<String>metaDataTags = new ArrayList<String>();
        metaDataTags.add("formName="+submittedFormName);
        Map< String, String > map = new HashMap< String, String >();
        map.put("path", formPath);
        map.put("1_property", "Searchable");
        map.put("1_property.value","true");
        Query query = queryBuilder.createQuery(PredicateGroup.create(map),session);
        query.setStart(0);
        query.setHitsPerPage(20);
        SearchResult result = query.getResult();
        logger.debug("Get result hits " + result.getHits().size());
        for (Hit hit: result.getHits()) {
            try {
                    logger.debug(hit.getPath());
                    String jsonElementName = (String) hit.getProperties().get("name");
                    String fieldName = hit.getProperties().get("name").toString();
                    if(jsonObject.get(jsonElementName).isJsonArray())
                    {
                        
                        JsonArray arrayOfValues = jsonObject.get(jsonElementName).getAsJsonArray();
                        StringBuilder valuesString = new StringBuilder();
                        for(int j=0;j<arrayOfValues.size();j++)
                        {
                            valuesString.append(arrayOfValues.get(j).getAsString());
                            if(j < arrayOfValues.size() -1)
                            {
                                valuesString.append(" and ");
                            }
                        }

                        
                        metaDataTags.add(fieldName + "=" + valuesString.toString());

                    }
                    else
                    {
                        logger.debug("The searchable field name is " + fieldName + "the json element name is " + jsonElementName);
                        metaDataTags.add(fieldName + "=" + jsonObject.get(jsonElementName).getAsString());
                    }

            } catch (RepositoryException e) {
                throw new RuntimeException(e);
            }
        }
        return String.join("&",metaDataTags);
    }
```

## Pasos siguientes

[Crear controlador de envío personalizado](./create-custom-submit.md)


