---
title: Buscar el JSON del formulario adaptable para incrustar
description: Utilice la API para recuperar el json del formulario adaptable
feature: Adaptive Forms
version: 6.5
kt: 13285
topic: Development
role: User
level: Intermediate
source-git-commit: c6e83a627743c40355559d9cdbca2b70db7f23ed
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 0%

---


# Recuperar el JSON del formulario

Inicie sesión en la instancia de autor de AEM Forms y cree un nuevo formulario adaptable con el **En blanco con componentes principales** plantilla. Publique el formulario en la instancia de publicación.

Para incrustar el formulario, primero recuperamos el json del formulario adaptable realizando una llamada GET en el servidor de publicación.

El siguiente fragmento de código recupera el json del formulario adaptable llamado **contacto**

```javascript
const getForm = async () => {
        const resp = await fetch('/content/forms/af/contactus/jcr:content/guideContainer.model.json');
        let formJSON = await resp.json();
        console.log(formJSON);
        setForm(formJSON);
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
        
        const resp = await fetch('/content/forms/af/contactus/jcr:content/guideContainer.model.json');
        let formJSON = await resp.json();
        console.log(formJSON);
        setForm(formJSON);
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

El código anterior utiliza componentes html nativos asignados a los componentes utilizados en el formulario adaptable. Por ejemplo, se asigna el componente de formulario adaptable de entrada de texto al componente TextField. Los componentes nativos utilizados en el artículo [se puede descargar desde aquí](./assets/native-components.zip)