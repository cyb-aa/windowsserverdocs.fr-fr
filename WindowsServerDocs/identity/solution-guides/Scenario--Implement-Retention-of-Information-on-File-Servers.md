---
ms.assetid: 81c55015-82e5-4ba1-b15e-cc7b49af28fc
title: Scénario implémentent rétention d’informations sur les serveurs de fichiers
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 59fd7f0a0a4d9ed8f5cec57b17be21e1aa4cd592
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880250"
---
# <a name="scenario-implement-retention-of-information-on-file-servers"></a>Scénario : implémenter la rétention des informations sur les serveurs de fichiers

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La période de rétention désigne le temps pendant lequel un document doit être conservé avant qu’il n’expire. La période de rétention peut varier en fonction de l’organisation. Vous pouvez classer des fichiers dans un dossier avec une période de rétention à court, moyen ou long terme, puis définir la durée de chaque période. Vous pouvez conserver un fichier indéfiniment en proclamant sa mise en suspens pour raisons juridiques.  
  
## <a name="BKMK_OVER"></a>Description du scénario  
L’infrastructure de classification des fichiers et le Gestionnaire de ressources du serveur de fichiers se servent des tâches de gestion de fichiers et de la classification des fichiers pour appliquer des périodes de rétention pour un ensemble de fichiers. Vous pouvez attribuer une période de rétention dans un dossier, puis utiliser une tâche de gestion de fichiers pour configurer la durée d’une période de rétention. Lorsque les fichiers du dossier sont sur le point d'expirer, le propriétaire du fichier reçoit un message de notification. Vous pouvez également classer un fichier en suspens pour raisons juridiques pour que la tâche de gestion de fichiers n’entraîne pas l’expiration du fichier.  
  
Vous trouverez des informations sur la configuration de la rétention dans [Plan for Retention of Information on File Servers](assetId:///edf13190-7077-455a-ac01-f534064a9e0c).  
  
Vous trouverez les étapes pour classer les fichiers de conservation légale et la configuration d’une période de rétention dans [déployer implémentation de rétention des informations sur les serveurs de fichiers &#40;étapes de démonstration&#41;](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md).  
  
> [!NOTE]  
> Ce scénario traite uniquement de la manière de classifier manuellement un document en mode de conservation légale. Toutefois, il est possible dans Windows Server 2012 pour classifier automatiquement les documents de conservation légale. Vous pouvez pour cela créer un classifieur Windows PowerShell qui compare le propriétaire du fichier à une liste de comptes d'utilisateur qui sont en mode de conservation légale. Si le propriétaire du fichier figure dans la liste des comptes d'utilisateur, le fichier est classifié en mode de conservation légale.  
  
## <a name="in-this-scenario"></a>Dans ce scénario  
Ce scénario fait partie du scénario de contrôle d'accès dynamique. Pour plus d'informations sur le contrôle d'accès dynamique, voir :  
  
-   [Contrôle d’accès dynamique : Vue d’ensemble du scénario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="BKMK_NEW"></a>Fonctionnalités incluses dans ce scénario  
Le tableau ci-dessous répertorie les fonctionnalités incluses dans ce scénario et détaille la manière dont elles prennent en charge ce dernier.  
  
|Fonctionnalité|Prise en charge de ce scénario|  
|-----------|---------------------------------|  
|[Vue d’ensemble de la file Server Resource Manager](https://technet.microsoft.com/library/hh831701.aspx)|L'infrastructure de classification des fichiers est une fonctionnalité du Gestionnaire de ressources du serveur de fichiers.|  
|[Présentation des Services de stockage et de fichier](https://technet.microsoft.com/library/hh831487.aspx)|Le Gestionnaire de ressources du serveur de fichiers est une fonctionnalité du rôle serveur Services de fichiers.|  
  
  


