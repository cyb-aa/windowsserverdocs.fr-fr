---
title: Utiliser toutes les fonctions virtuelles pour la mise en réseau lorsqu’ils sont disponibles
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: bf895484-6a0d-4aa4-9a42-9fac739e875d
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 3ad120ffa689f1f7dcae832432e216ebda57e62f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877790"
---
# <a name="use-all-virtual-functions-for-networking-when-they-are-available"></a>Utiliser toutes les fonctions virtuelles pour la mise en réseau lorsqu’ils sont disponibles

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Certaines fonctionnalités d’accélération matérielle ne sont pas utilisées*  
  
## <a name="impact"></a>Impact  
*Cette configuration peut entraîner l’utilisation globale du processeur être plus élevé que nécessaire. Performances de mise en réseau ne peut pas être optimale sur les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Envisagez de configurer la carte réseau virtuelle pour SR-IOV, si le matériel physique prend en charge SR-IOV et si cette configuration n’est pas en conflit avec les fonctionnalités de mise en réseau requises par la machine virtuelle.*  
  


