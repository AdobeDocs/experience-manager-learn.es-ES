---
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 1%

---


# Integración de Adobe Analytics con Real-time Customer Data Platform

{{analytics-description}}

{{real-time-cdp-description}}

La integración de Adobe Analytics con Adobe Real-time Customer Data Platform (RTCDP) puede ofrecer varios beneficios a las empresas que buscan mejorar sus experiencias de cliente y sus esfuerzos de marketing. Estas son algunas de las ventajas clave:

+ **Segmentación y personalización de audiencias mejoradas**: Marketing preciso en dispositivos y canales, mensajes personalizados para una participación optimizada.
+ **Optimización de la página de aterrizaje mejorada**: experiencias adaptadas basadas en el dispositivo y el comportamiento, que mejoran la satisfacción y la conversión del usuario.
+ **Activación de audiencia perfecta**: utilice los perfiles del cliente para una segmentación eficaz a través de los canales preferidos, entregando los mensajes relevantes.

Al combinar Adobe Analytics y Real-Time CDP, las empresas pueden llevar sus esfuerzos de marketing al siguiente nivel, ofreciendo experiencias personalizadas, aumentando la participación de los clientes y optimizando las conversiones en varios puntos de contacto digitales.

<table>
    <thead>
        <tr>
            <th>aplicaciones de Experience Cloud</th>
            <th>Se integra mediante</th>
            <th>Cuándo se usa</th>
            <th>Casos de uso comunes</th>
        </tr>
    </thead>
    <tr>
        <td><a href="../../integrations/tutorials/analytics-real-time-cdp/experience-platform-source-connector.md" target="_blank" rel="noreferrer">Analytics con Real-Time CDP</a></td>
        <td>Conector de origen del Experience Platform</td>
        <td>
            <ul>
                <li>Si desea introducir datos de Analytics en Experience Platform desde sus grupos de informes.</li>
                <li>Cuando la disponibilidad de los datos para el perfil del cliente puede estar entre 2 y 30 minutos desde el momento de la recopilación de datos, y la disponibilidad para el lago de datos es de hasta 90 minutos.</li>
            </ul>
        </td>
        <td>
            <ul>
                <li>Flujo de trabajo iniciado directamente por la interfaz de usuario.</li>
                <li>Asignación de la interfaz de usuario para copiar props y eVars de Analytics en nuevos campos XDM.</li>
                <li>La forma más rápida de obtener valor de Perfil del cliente en tiempo real y Customer Journey Analytics.</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="https://adobe.com" target="_blank" rel="noreferrer">TODO: Analytics con Real-Time CDP</a></td>
        <td>Experience Platform Edge</td>
        <td>
            <ul>
                <li>Cuando se implementa una estrategia de análisis a largo plazo.</li>
                <li>Cuando sea un cliente nuevo o existente que necesite disponibilidad de datos de Analytics en el perfil del cliente para admitir casos de uso de personalización de la misma página y de la siguiente.</li>
            </ul>
        </td>
        <td>
            <ul>
                <li>Proporciona el bueno nivel de control para los datos recopilados que se deben utilizar para admitir casos de uso.</li>
                <li>Los datos del lado del cliente se asignan fácilmente a campos XDM.</li>
                <li>La disponibilidad de datos más rápida para el Perfil del cliente en tiempo real.</li>
            </ul>
        </td>
    </tr>            
</table>
