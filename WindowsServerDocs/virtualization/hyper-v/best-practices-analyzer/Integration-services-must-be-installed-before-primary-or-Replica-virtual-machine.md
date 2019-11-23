---
title: Les services d’intégration doivent être installés avant que les machines virtuelles principales ou de réplication puissent utiliser une autre adresse IP après un basculement
description: Version en ligne du texte de cette règle de Best Practices Analyzer, avec des liens vers des informations supplémentaires.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a7fdd185-d6c8-4f58-9b58-2df5827bb056
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 58e744c182fb2013e55e91f58140c6ba14181f9f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393589"
---
# <a name="integration-services-must-be-installed-before-primary-or-replica-virtual-machines-can-use-an-alternate-ip-address-after-a-failover"></a>Les services d’intégration doivent être installés avant que les machines virtuelles principales ou de réplication puissent utiliser une autre adresse IP après un basculement

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Erreur|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Les machines virtuelles participant à la réplication peuvent être configurées pour utiliser une adresse IP spécifique en cas de basculement, mais uniquement si les services d’intégration sont installés dans le système d’exploitation invité de la machine virtuelle.*  
  
## <a name="impact"></a>Impact  
*En cas de basculement (planifié, non planifié ou de test), la machine virtuelle de réplication est mise en ligne à l’aide de la même adresse IP que la machine virtuelle principale. Cette configuration peut entraîner des problèmes de connectivité. Cela a un impact sur les machines virtuelles suivantes :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Utilisez connexion à un ordinateur virtuel pour installer les services d’intégration sur l’ordinateur virtuel.*  
  
À compter de Windows Server 2016, les services d’intégration pour les machines virtuelles Windows sont fournis par le biais de Windows Update. Assurez-vous que ces machines virtuelles sont configurées pour recevoir des mises à jour Windows afin d’obtenir la dernière version d’Integration Services. Le noyau Linux comprend désormais les services d’intégration Linux (LIS) et est mis à jour pour les nouvelles versions, mais les distributions Linux basées sur des noyaux plus anciens peuvent ne pas avoir les améliorations ou les correctifs les plus récents. Pour plus d’informations, consultez [machines virtuelles Linux et FreeBSD prises en charge pour Hyper-V sur Windows](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md).


