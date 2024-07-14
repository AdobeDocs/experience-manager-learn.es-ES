---
title: Buscar el JSON del formulario adaptable para incrustar
description: Utilice la API para recuperar el json del formulario adaptable
feature: Adaptive Forms
version: 6.5
jira: KT-13285
topic: Development
role: User
level: Intermediate
exl-id: ee534724-54ea-48e1-8c92-de1c56a928d4
duration: 50
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 1%

---

# Recuperar el JSON del formulario

Inicie sesión en la instancia de autor de AEM Forms y cree un nuevo formulario adaptable con la plantilla **En blanco con componentes principales**. Publish el formulario en la instancia de publicación.

Para incrustar el formulario, primero recuperamos el json del formulario adaptable realizando una llamada GET en el servidor de publicación.

El siguiente fragmento de código recupera el json del formulario adaptable llamado **contact**

```javascript
const getForm = async () => {
        
        const resp = await fetch('/adobe/forms/af/L2NvbnRlbnQvZm9ybXMvYWYvZmlyc3RoZWFkbGVzcw==');
        // Get the form id manually using the listform api
        let formJSON = await resp.json();
        console.log("The contact form json is "+formJSON);
        setForm(formJSON.afModelDefinition)
      }
```

A continuación, se muestra el código completo del componente de función Contact

```javascript
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import Panel from './components/Panel';
import { useState,useEffect } from "react";
import { AdaptiveForm } from "@aemforms/af-react-renderer";

export default function Contact(){
   const [selectedForm, setForm] = useState("");
   const extendMappings = {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
      };
    const getForm = async () => {
        
        const resp = await fetch('/adobe/forms/af/dor/L2NvbnRlbnQvZm9ybXMvYWYvcmlzaGk=');
        let formJSON = await resp.json();
        setForm(formJSON.afModelDefinition)
      
      }
      useEffect( ()=>{
        getForm();
        

    },[]);
    return(
        
        <div>
            <h1>Get in touch with us!!!!</h1>
            <AdaptiveForm mappings={extendMappings} formJson={selectedForm} />
      
          
        </div>
    )
}
```

El código anterior utiliza componentes html nativos asignados a los componentes utilizados en el formulario adaptable. Por ejemplo, se asigna el componente de formulario adaptable de entrada de texto al componente TextField. Los componentes nativos utilizados en el artículo [ se pueden descargar desde aquí](./assets/native-components.zip)

## Siguientes pasos

[Seleccione un formulario de la lista desplegable](./select-form-from-drop-down-list.md)
