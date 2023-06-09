---
title: Haga clic en la tarjeta para mostrar el formulario
description: Explorar en profundidad el formulario desde la vista de tarjeta
feature: Adaptive Forms
version: 6.5
kt: 13372
topic: Development
role: User
level: Intermediate
source-git-commit: 529e98269a08431152686202a8a2890712b9c835
workflow-type: tm+mt
source-wordcount: '52'
ht-degree: 0%

---

# Mostrar el formulario al hacer clic en la tarjeta

El siguiente código se utilizó para mostrar el formulario cuando el usuario hace clic en una tarjeta. La ruta del formulario que se va a mostrar se extrae de la dirección URL mediante la función useParams.

```javascript
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import Panel from './components/Panel';
import { useState,useEffect } from "react";
import {Link, useParams} from 'react-router-dom';
import { AdaptiveForm } from "@aemforms/af-react-renderer";
export default function DisplayForm()
{
   const [selectedForm, setForm] = useState("");
   const params = useParams();
   const extendMappings =
    {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
    };
    
    
   const getAFForm = async () =>
    {
           
        const resp = await fetch(`/adobe/forms/af/${params.formID}`);
        let formJSON = await resp.json();
        console.log("The contact form json is "+formJSON);
        setForm(formJSON.afModelDefinition)
    }
    
    useEffect( ()=>{
        getAFForm()
        

    },[]);
    return(
       <div>
           <AdaptiveForm mappings={extendMappings} formJson={selectedForm}/>
        </div>
    )
}
```
