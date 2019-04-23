---
title: Configurer des machines virtuelles pour utiliser SR-IOV uniquement lors de la prise en charge par le système d’exploitation invité
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 33cf5b68-e43e-47ef-adbc-6b266c1d4dce
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e5c2acb21fe8b11e8f020c6d2ab1742116c23b28
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833360"
---
# <a name="configure-virtual-machines-to-use-sr-iov-only-when-supported-by-the-guest-operating-system"></a>Configurer des machines virtuelles pour utiliser SR-IOV uniquement lors de la prise en charge par le système d’exploitation invité

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
*Un ou plusieurs ordinateurs virtuels sont configurés pour utiliser la virtualisation d’e/s de racine unique (SR-IOV), mais le système d’exploitation invité ne prend pas en charge SR-IOV*  
  
## <a name="impact"></a>Impact  
*Fonctions virtuelles SR-IOV ne seront pas allouées pour les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Désactivez SR-IOV sur toutes les machines virtuelles qui exécutent les systèmes d’exploitation invités qui ne prennent pas en charge SR-IOV.*  
  
SR-IOV est pris en charge uniquement dans des invités de Windows 64 bits. Pour plus d’informations, consultez [compatibilité des fonctionnalités par génération et invité Hyper-V](../Hyper-V-feature-compatibility-by-generation-and-guest.md).  
  


