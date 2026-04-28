---
title: Filtrado de la aplicación Angular
description: A simple Angular app that filters WKND adventures modeled using Content Fragments.
version: Experience Manager as a Cloud Service
feature: Content Fragments, GraphQL API
topic: Headless, Content Management
role: Developer
level: Beginner
jira: KT-11133
thumbnail: KT-11133.jpg
index: false
hide: true
hidefromtoc: true
exl-id: c238dd83-65d3-4b04-b90e-19ed250b8e36
duration: 26
source-git-commit: f95907146983d2315d48f793d38ebb1172a7bae4
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 5%

---

# Filtrado de la aplicación Angular

Explore AEM Headless GraphQL APIs ability to filter data using a [Angular](https://angular.io/) app. This Angular app creates a list of WKND adventures filterable by Activity Type.

This code demonstrates using Adobe&#39;s [AEM Headless Client for JavaScript](https://github.com/adobe/aem-headless-client-js/blob/main/api-reference.md) to invoke persisted GraphQL queries from Angular. This app uses the `wknd-shared/adventures-all` persisted query to collect all adventures, and derive a list of available Activity Types. When a user selects an Activity Type, the selected type is passed to the `wknd-shared/adventures-by-activity` persisted query and retrieves the adventure details for only those adventures of the specified Activity Type.

This code:

+ Connects to an AEM Publish service, and does not require authentication
+ Uses the WKND&#39;s persisted queries: `wknd-shared/adventures-all` and `wknd-shared/adventures-by-activity`
