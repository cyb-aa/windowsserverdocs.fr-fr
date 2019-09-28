---
title: L’authentification basée sur les certificats est recommandée pour la réplication
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: d931cc57-414f-4bdf-9ebd-08fd5e22b19d
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 0eac99fddd8bbc6dc585931cd25f2a440be16c76
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365178"
---
# <a name="certificate-based-authentication-is-recommended-for-replication"></a>L’authentification basée sur les certificats est recommandée pour la réplication

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*Une ou plusieurs machines virtuelles sélectionnées pour la réplication sont configurées pour l’authentification Kerberos.*  
  
## <a name="impact"></a>**Impact**  
le trafic réseau de réplication @no__t 0The à partir du serveur principal vers le serveur de réplication n’est pas chiffré. Cela a un impact sur les ordinateurs virtuels suivants : *  
  
@no__t 0list de machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
*If une autre méthode est utilisée pour effectuer le chiffrement, vous pouvez ignorer cette option. Sinon, modifiez les paramètres de l’ordinateur virtuel pour choisir l’authentification basée sur les certificats.*  
  


