---
title: Évitez d’utiliser des points de contrôle sur une machine virtuelle qui exécute une charge de travail de serveur dans un environnement de production
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1be75890-d316-495a-b9b7-be75fc1aac10
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 166ef839a40452cc4156144e10e9c666e7ce3472
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856150"
---
# <a name="avoid-using-checkpoints-on-a-virtual-machine-that-runs-a-server-workload-in-a-production-environment"></a>Évitez d’utiliser des points de contrôle sur une machine virtuelle qui exécute une charge de travail de serveur dans un environnement de production

>S'applique à : Windows Server 2016


  
*Pour plus d’informations sur les meilleures pratiques et les analyses, consultez* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Opérations|  

Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.

> [!NOTE]  
> Dans Windows Server 2012 R2, les instantanés d’ordinateur virtuel ont été renommés pour les points de contrôle de machine virtuelle dans le Gestionnaire Hyper-V pour correspondre à la terminologie utilisée dans l’administration de System Center Virtual Machine. Pour plus d’informations, consultez [Checkpoints and Snapshots Overview](https://technet.microsoft.com/library/dn818483.aspx).  
  
## <a name="issue"></a>Problème  
  
*Une machine virtuelle avec un ou plusieurs points de contrôle a été trouvée.*  
  
## <a name="impact"></a>Impact  
  
*Espace disponible peut manquer sur le disque physique qui stocke les fichiers de points de contrôle. Si cela se produit, aucune opération de disque supplémentaire peut être effectuée sur le stockage physique. Toute machine virtuelle qui s’appuie sur le stockage physique pourrait être affectée.*  
  
Si l’espace disque physique est épuisé, toute machine virtuelle en cours d’exécution qui a des points de contrôle ou des disques durs virtuels stockés sur ce disque peut être interrompue automatiquement. Gestionnaire Hyper-V Affiche l’état de ces machines virtuelles en tant que « pause critique ».  
  
## <a name="resolution"></a>Résolution  
  
*Si la machine virtuelle s’exécute une charge de travail de serveur dans un environnement de production, mettre hors connexion de la machine virtuelle, puis utilisez le Gestionnaire Hyper-V pour appliquer ou supprimer les points de contrôle. Pour supprimer des points de contrôle, vous devez arrêter la machine virtuelle pour terminer le processus.*  
  
> [!NOTE]  
> Points de contrôle de production sont désormais disponibles comme alternative aux points de contrôle standards. Pour plus d’informations, consultez [choisir entre les points de contrôle standard ou de production](../manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md).  
  


