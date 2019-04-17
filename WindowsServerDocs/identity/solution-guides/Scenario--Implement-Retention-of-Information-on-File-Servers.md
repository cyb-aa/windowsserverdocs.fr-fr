---
ms.assetid: 81c55015-82e5-4ba1-b15e-cc7b49af28fc
title: "Scénario implémenter la rétention des informations sur les serveurs de fichiers"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 62339170895190435adb9a97d565877ff78d97d7
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: fr-FR
---
# <a name="scenario-implement-retention-of-information-on-file-servers"></a>Scénario: Implémenter la rétention des informations sur les serveurs de fichiers

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Une période de rétention est la durée pendant laquelle un document doit être conservé avant qu’il a expiré. En fonction de l’organisation, la période de rétention peut être différente. Vous pouvez classer des fichiers dans un dossier avec une période de rétention à court, moyen ou long terme et puis définir la durée de chaque période. Vous devrez conserver un fichier indéfiniment en les mettant en légale.  
  
## <a name="BKMK_OVER"></a>Description du scénario  
Infrastructure de Classification des fichiers et Gestionnaire de ressources du serveur de fichiers utilise des tâches de gestion de fichiers et de classification des fichiers pour appliquer des périodes de rétention pour un ensemble de fichiers. Vous pouvez attribuer une période de rétention sur un dossier et ensuite utiliser une tâche de gestion de fichiers pour configurer la durée pendant laquelle une période de rétention en dernier. Lorsque les fichiers dans le dossier sont sur le point d’expirer, le propriétaire du fichier reçoit un e-mail de notification. Vous pouvez également classer un fichier comme étant juridiques, afin que la tâche de gestion de fichiers n’expire pas le fichier.  
  
Vous trouverez des informations de planification pour la configuration de rétention dans [planifier de rétention des informations sur les serveurs de fichiers](assetId:///edf13190-7077-455a-ac01-f534064a9e0c).  
  
Vous trouverez les étapes pour classer les fichiers de conservation légale et de la configuration d’une période de rétention dans [déployer implémentation de rétention des informations sur les serveurs de fichiers et #40; étapes de démonstration et #41;](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md).  
  
> [!NOTE]  
> Ce scénario traite uniquement comment classifier manuellement un document de conservation légale. Toutefois, il est possible dans Windows Server2012 pour classifier automatiquement les documents de conservation légale. Pour ce faire consiste à créer un classifieur Windows PowerShell qui compare le propriétaire du fichier à une liste des comptes d’utilisateur qui sont sous légale. Si le propriétaire du fichier est une partie de la liste de comptes d’utilisateur, le fichier est classifié de conservation légale.  
  
## <a name="in-this-scenario"></a>Dans ce scénario  
Ce scénario fait partie du scénario de contrôle d’accès dynamique. Pour plus d’informations sur le contrôle d’accès dynamique, voir:  
  
-   [Contrôle d’accès dynamique: Vue d’ensemble du scénario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_NEW"></a>Fonctionnalités incluses dans ce scénario  
Le tableau suivant répertorie les fonctionnalités qui font partie de ce scénario et décrit la prise en charge.  
  
|Fonctionnalité|Comment il prend en charge ce scénario|  
|-----------|---------------------------------|  
|[Vue d’ensemble du Gestionnaire de ressources du serveur de fichiers](https://technet.microsoft.com/library/hh831701.aspx)|Infrastructure de Classification des fichiers est une fonctionnalité qui est incluse dans le Gestionnaire de ressources du serveur de fichiers.|  
|[Vue d’ensemble des Services de stockage et de fichier](https://technet.microsoft.com/library/hh831487.aspx)|Gestionnaire de ressources du serveur de fichiers est une fonctionnalité qui est incluse avec le rôle de serveur Services de fichiers.|  
  
  


