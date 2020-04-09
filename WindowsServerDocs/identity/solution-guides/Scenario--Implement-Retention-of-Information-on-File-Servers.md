---
ms.assetid: 81c55015-82e5-4ba1-b15e-cc7b49af28fc
title: Scénario implémenter la rétention des informations sur les serveurs de fichiers
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 2f6b55d8a98a0f4fb0c286e16d752a18e61dce1a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861102"
---
# <a name="scenario-implement-retention-of-information-on-file-servers"></a>Scénario : implémenter la rétention des informations sur les serveurs de fichiers

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La période de rétention désigne le temps pendant lequel un document doit être conservé avant qu’il n’expire. La période de rétention peut varier en fonction de l’organisation. Vous pouvez classer des fichiers dans un dossier avec une période de rétention à court, moyen ou long terme, puis définir la durée de chaque période. Vous pouvez conserver un fichier indéfiniment en proclamant sa mise en suspens pour raisons juridiques.  
  
## <a name="scenario-description"></a><a name="BKMK_OVER"></a>Description du scénario  
L’infrastructure de classification des fichiers et le Gestionnaire de ressources du serveur de fichiers se servent des tâches de gestion de fichiers et de la classification des fichiers pour appliquer des périodes de rétention pour un ensemble de fichiers. Vous pouvez attribuer une période de rétention dans un dossier, puis utiliser une tâche de gestion de fichiers pour configurer la durée d’une période de rétention. Lorsque les fichiers du dossier sont sur le point d'expirer, le propriétaire du fichier reçoit un message de notification. Vous pouvez également classer un fichier en suspens pour raisons juridiques pour que la tâche de gestion de fichiers n’entraîne pas l’expiration du fichier.  
  
Vous trouverez des informations de planification pour la configuration de la rétention dans [Planifier la rétention des informations sur les serveurs de fichiers](assetId:///edf13190-7077-455a-ac01-f534064a9e0c).  
  
Pour plus d’informations sur la classification des fichiers en vue de leur conservation légale et sur la configuration d’une période de rétention, consultez [ &#40;&#41;étapes de mise en œuvre de la rétention des informations sur les serveurs de fichiers](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md).  
  
> [!NOTE]  
> Ce scénario traite uniquement de la manière de classifier manuellement un document en mode de conservation légale. Toutefois, il est possible dans Windows Server 2012 de classer automatiquement les documents en vue de leur conservation légale. Vous pouvez pour cela créer un classifieur Windows PowerShell qui compare le propriétaire du fichier à une liste de comptes d'utilisateur qui sont en mode de conservation légale. Si le propriétaire du fichier figure dans la liste des comptes d'utilisateur, le fichier est classifié en mode de conservation légale.  
  
## <a name="in-this-scenario"></a>Dans ce scénario  
Ce scénario fait partie du scénario de contrôle d'accès dynamique. Pour plus d'informations sur le contrôle d'accès dynamique, voir :  
  
-   [Access Control dynamique : vue d’ensemble du scénario](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="features-included-in-this-scenario"></a><a name="BKMK_NEW"></a>Fonctionnalités incluses dans ce scénario  
Le tableau ci-dessous répertorie les fonctionnalités incluses dans ce scénario et détaille la manière dont elles prennent en charge ce dernier.  
  
|Composant|Prise en charge de ce scénario|  
|-----------|---------------------------------|  
|[Vue d’ensemble du serveur de fichiers Gestionnaire des ressources](https://technet.microsoft.com/library/hh831701.aspx)|L'infrastructure de classification des fichiers est une fonctionnalité du Gestionnaire de ressources du serveur de fichiers.|  
|[Vue d’ensemble des services de stockage et de fichiers](https://technet.microsoft.com/library/hh831487.aspx)|Le Gestionnaire de ressources du serveur de fichiers est une fonctionnalité du rôle serveur Services de fichiers.|  
  
  


