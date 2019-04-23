---
title: Une ou plusieurs cartes réseau doivent être configurés comme source pour la mise en miroir de Port
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 147fd00f-1440-44d1-94e3-3a8af63aa7ed
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 0843c1f302b96d334e8d6649ac5503bbe2b12d02
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831790"
---
# <a name="one-or-more-network-adapters-should-be-configured-as-the-source-for-port-mirroring"></a>Une ou plusieurs cartes réseau doivent être configurés comme source pour la mise en miroir de Port

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*Un ou plusieurs machines virtuelles ont une carte réseau configurée en tant que destination pour la mise en miroir de Port, mais il n’existe aucune source correspondante sur le commutateur virtuel.*  
  
## <a name="impact"></a>**Impact**  
*Mise en miroir de port ne fonctionnera pas correctement pour les commutateurs virtuels suivants et les machines virtuelles :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
*Utiliser Windows PowerShell ou le Gestionnaire Hyper-V pour terminer ou corrigez la configuration de la mise en miroir de Port.*  
  


