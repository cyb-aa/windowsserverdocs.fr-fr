---
title: Activer tous les adaptateurs de réseau virtuel configurés pour une machine virtuelle
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: fcd350b7-4240-4359-aadd-93e7ac4d314e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: fbb1ef5283f6ccf8dfa355a09a86040be80f53e9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844230"
---
# <a name="enable-all-virtual-network-adapters-configured-for-a-virtual-machine"></a>Activer tous les adaptateurs de réseau virtuel configurés pour une machine virtuelle

>S'applique à : Windows Server 2016

Pour plus d'informations sur les meilleures pratiques et les analyses, consultez [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Une ou plusieurs cartes réseau peuvent être désactivées sur une machine virtuelle.*  
  
## <a name="impact"></a>Impact  
  
*Les ordinateurs virtuels suivants ne peut-être pas de connectivité réseau :*  
  
\<liste des noms de machine virtuelle >  
  
## <a name="resolution"></a>Résolution  
  
*Utilisez le Gestionnaire de périphériques dans le système d’exploitation invité pour activer toutes les cartes réseau virtuelles. Si la carte n’est pas nécessaire, utilisez le Gestionnaire Hyper-V pour le supprimer de la machine virtuelle.*  
  


