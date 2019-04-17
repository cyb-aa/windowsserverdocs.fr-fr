---
ms.assetid: ad3f0480-99f7-428a-ab33-6d165a440840
title: "Scénario Get pour mieux appréhender vos données à l’aide de Classification"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2e5248b5fd5e20b7436de9f796367c018d8ef16e
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-get-insight-into-your-data-by-using-classification"></a>Scénario: Obtenir l’aide à mieux appréhender vos données à l’aide de Classification

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Vis-à-vis des ressources de stockage et de données a continué de croître importance pour la plupart des organisations. Les administrateurs informatiques sont confrontés à un défi de la surveillance des infrastructures de stockage plus volumineuses et complexes, tout en simultanément, assumer avec la responsabilité de garantir que total coût de possession est conservée à des niveaux raisonnables. La gestion des ressources de stockage ne sont pas seulement sur le volume ou la disponibilité des données; il s’agit aussi en appliquant des stratégies de l’entreprise et de savoir comment le stockage est exploité pour favoriser une utilisation efficace et la conformité pour atténuer les risques. Infrastructure de Classification des fichiers fournit une aide à mieux appréhender vos données en automatisant les processus de classification afin que vous pouvez gérer vos données plus efficacement. Les méthodes de classification suivantes sont disponibles avec l’Infrastructure de Classification des fichiers: manuelle, par programmation et automatique. Cette rubrique se concentre sur la méthode de classification automatique des fichiers.  
  
## <a name="BKMK_OVER"></a>Description du scénario  
Infrastructure de Classification des fichiers utilise des règles de classification pour analyser les fichiers et les classer en fonction du contenu du fichier automatiquement. Propriétés de classification sont définies de manière centralisée dans ActiveDirectory afin que ces définitions peuvent être partagées entre les serveurs de fichiers dans l’organisation. Vous pouvez créer des règles de classification qui analyse les fichiers d’une chaîne standard ou d’une chaîne qui correspond à un modèle (expression régulière). Lorsqu’un paramètre de classification configuré est détecté dans un fichier, ce fichier est classifié tel que configuré dans la règle de classification. Voici quelques exemples de règles de classification:  
  
-   Classifier tout fichier qui contient la chaîne «Contoso Confidential» comme ayant un fort impact commercial  
  
-   Classifier tout fichier qui contient au moins 10numéros de sécurité sociale comme possédant des informations d’identification personnelle  
  
Lorsqu’un fichier est classifié, vous pouvez utiliser un fichier de tâche de gestion de prendre des mesures sur tous les fichiers qui sont classées de manière spécifique. Les actions dans une tâche de gestion de fichiers peuvent consister à protéger les droits associés au fichier, faire expirer le fichier et exécuter une action personnalisée (par exemple, pour publier des informations sur un service web).  
  
Vous trouverez des informations de planification pour la configuration de classification automatique des fichiers dans [Plan for Automatic File Classification](assetId:///e3c3bb4b-3034-42b7-b391-8ef5f5851955).  
  
Vous trouverez comment classer automatiquement des fichiers dans [déployer la Classification automatique des fichiers et #40; étapes de démonstration et #41;](Deploy-Automatic-File-Classification--Demonstration-Steps-.md).  
  
## <a name="in-this-scenario"></a>Dans ce scénario  
Ce scénario fait partie du scénario de contrôle d’accès dynamique. Pour plus d’informations sur le contrôle d’accès dynamique, voir:  
  
-   [Contrôle d’accès dynamique: Vue d’ensemble du scénario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_APP"></a>Applications pratiques  
Infrastructure de Classification des fichiers dans Windows Server2012 contribue au contrôle d’accès dynamique en permettant aux propriétaires données commerciales de classifier et d’étiqueter des données facilement. Les informations de classification qui sont stockées dans la stratégie d’accès centralisée vous permettent de définir des stratégies d’accès pour les classes de données qui sont essentielles pour les entreprises.  
  
## <a name="BKMK_NEW"></a>Fonctionnalités incluses dans ce scénario  
Le tableau suivant répertorie les fonctionnalités qui font partie de ce scénario et décrit la prise en charge.  
  
|Fonctionnalité|Comment il prend en charge ce scénario|  
|-----------|---------------------------------|  
|[Vue d’ensemble du Gestionnaire de ressources du serveur de fichiers](https://technet.microsoft.com/library/hh831701.aspx)|Infrastructure de Classification des fichiers est une fonctionnalité qui est incluse dans le Gestionnaire de ressources du serveur de fichiers.|  
|[Vue d’ensemble des Services de stockage et de fichier](https://technet.microsoft.com/library/hh831487.aspx)|Gestionnaire de ressources du serveur de fichiers est une fonctionnalité qui est incluse avec le rôle de serveur Services de fichiers.|  
  


