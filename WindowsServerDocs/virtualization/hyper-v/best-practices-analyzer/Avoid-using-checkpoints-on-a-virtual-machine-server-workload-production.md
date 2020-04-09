---
title: Évitez d’utiliser des points de contrôle sur un ordinateur virtuel qui exécute une charge de travail serveur dans un environnement de production
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1be75890-d316-495a-b9b7-be75fc1aac10
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: f0e9d40fa6e28b515621402b853012cb59086a07
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857712"
---
# <a name="avoid-using-checkpoints-on-a-virtual-machine-that-runs-a-server-workload-in-a-production-environment"></a>Évitez d’utiliser des points de contrôle sur un ordinateur virtuel qui exécute une charge de travail serveur dans un environnement de production

>S’applique à Windows Server 2016


  
*Pour plus d’informations sur les meilleures pratiques et les analyses, consultez* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Opérations|  

Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.

> [!NOTE]  
> Dans Windows Server 2012 R2, les captures instantanées d’ordinateur virtuel ont été renommées en points de contrôle d’ordinateur virtuel dans le Gestionnaire Hyper-V afin de correspondre à la terminologie utilisée dans System Center Virtual Machine Management. Pour plus d’informations, consultez [vue d’ensemble des points de contrôle et des instantanés](https://technet.microsoft.com/library/dn818483.aspx).  
  
## <a name="issue"></a>Problème  
  
*Un ordinateur virtuel avec un ou plusieurs points de contrôle a été trouvé.*  
  
## <a name="impact"></a>Impact  
  
*L’espace disponible est peut-être insuffisant sur le disque physique qui stocke les fichiers de points de contrôle. Si cela se produit, aucune opération de disque supplémentaire ne peut être effectuée sur le stockage physique. Tout ordinateur virtuel qui s’appuie sur le stockage physique peut être affecté.*  
  
En cas d’insuffisance de l’espace disque physique, toute machine virtuelle en cours d’exécution qui possède des points de contrôle ou des disques durs virtuels stockés sur ce disque peut être suspendue automatiquement. Le Gestionnaire Hyper-V affiche l’état de ces machines virtuelles comme étant en pause critique.  
  
## <a name="resolution"></a>Résolution  
  
*Si la machine virtuelle exécute une charge de travail de serveur dans un environnement de production, déconnecter l’ordinateur virtuel, puis utiliser le Gestionnaire Hyper-V pour appliquer ou supprimer les points de contrôle. Pour supprimer des points de contrôle, vous devez arrêter l’ordinateur virtuel pour terminer le processus.*  
  
> [!NOTE]  
> Les points de contrôle de production sont désormais disponibles à la place des points de contrôle standard. Pour plus d’informations, consultez [choisir entre des points de contrôle standard ou de production](../manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md).  
  


