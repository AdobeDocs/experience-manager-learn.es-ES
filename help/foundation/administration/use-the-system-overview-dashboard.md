---
title: Uso del panel Información general del sistema en AEM
description: En versiones anteriores de los administradores de AEM tenían que ver varias ubicaciones para obtener una imagen completa de la instancia de AEM. El objetivo de la Información general del sistema es solucionarlo proporcionando una vista de alto nivel de la configuración, el hardware y el estado de la instancia de AEM desde un único panel.
version: 6.4, 6.5
topics: administration, operations, monitoring
activity: use
audience: administrator, architect, developer, implementer
doc-type: technical video
contentOwner: dgordon
topic: Administration
role: Administrator
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 1%

---


# Uso del panel Información general del sistema

La [!UICONTROL Información general del sistema] de Adobe Experience Manager (AEM) proporciona una vista de alto nivel de la configuración, el hardware y el estado de la instancia de AEM, todo ello desde un solo panel.

>[!VIDEO](https://video.tv.adobe.com/v/21340?quality=12&learn=on)

1. Se puede acceder a la Información general del sistema desde: **Inicio de AEM** > **[!UICONTROL Herramientas]** > **[!UICONTROL Operaciones]** > **[!UICONTROL Información general del sistema]**

   Directamente en **`<server-host>/libs/granite/operations/content/systemoverview.html`**

1. La información de [!UICONTROL System Overview] se puede exportar haciendo clic en el botón [!UICONTROL Download]. La información también se expone a través del siguiente [!DNL REST] punto final:
1. A continuación se muestra un ejemplo de salida del JSON que se exporta desde [!UICONTROL System Overview]:

   ```json
   {
       "Health Checks": {
           "1 Critical": "System Maintenance",
           "3 Warn": "Replication Queue, Log Errors, Sling/Granite Content Access Check"
       },
       "Instance": {
           "Adobe Experience Manager": "6.4.0",
           "Run Modes": "s7connect, crx3, non-composite, author, samplecontent, crx3tar",
           "Instance Up Since": "2018-01-22 10:50:37"
       },
       "Repository": {
           "Apache Jackrabbit Oak": "1.8.0",
           "Node Store": "Segment Tar",
           "Repository Size": "0.26 GB",
           "File Data Store": "crx-quickstart/repository/datastore"
       },
       "Maintenance Tasks": {
           "Failed": "AuditLog Maintenance Task, Project Purge, Workflow Purge",
           "Succeeded": "Data Store Garbage Collection, Lucene Binaries Cleanup, Revision Clean Up, Version Purge, Purge of ad-hoc tasks"
       },
       "System Information": {
           "Mac OS X": "10.12.6",
           "System Load Average": "2.29",
           "Usable Disk Space": "89.44 GB",
           "Maximum Heap": "0.97 GB"
       },
       "Estimated Node Counts": {
           "Total": "232448",
           "Tags": "62",
           "Assets": "267",
           "Authorizables": "210",
           "Pages": "1592"
       },
       "Replication Agents": {
           "Blocked": "publish, publish2",
           "Idle": "offloading_b3deb296-9b28-4f60-8587-c06afa2e632c, offloading_outbox, offloading_reverse_b3deb296-9b28-4f60-8587-c06afa2e632c, publish_reverse, scene7, screens, screens2, test_and_target"
       },
       "Distribution Agents": {
           "Blocked": "publish"
       },
       "Workflows": {
           "Running Workflows": "15",
           "Failed Workflows": "15",
           "Failed Jobs": "150"
       },
       "Sling Jobs": {
           "Failed": "305"
       }
   }
   ```
