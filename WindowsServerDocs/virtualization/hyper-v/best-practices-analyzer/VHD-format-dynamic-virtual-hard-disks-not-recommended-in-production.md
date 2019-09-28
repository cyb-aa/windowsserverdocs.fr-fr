---
title: Les disques durs virtuels dynamiques au format VHD ne sont pas recommandés pour les machines virtuelles qui exécutent des charges de travail serveur dans un environnement de production
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 324a60a0-1d15-4ef2-9f17-23cbd2eb42ce
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: bf5464208fc145a31571f01822bb5ba54efe89c4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364591"
---
# <a name="vhd-format-dynamic-virtual-hard-disks-are-not-recommended-for-virtual-machines-that-run-server-workloads-in-a-production-environment"></a>Les disques durs virtuels dynamiques au format VHD ne sont pas recommandés pour les machines virtuelles qui exécutent des charges de travail serveur dans un environnement de production

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
*Une ou plusieurs machines virtuelles utilisent des disques durs virtuels de taille dynamique au format VHD.*  
  
## <a name="impact"></a>**Impact**  
les disques durs virtuels dynamiques au format @no__t 0VHD peuvent rencontrer des problèmes de cohérence en cas de panne de courant. Des problèmes de cohérence peuvent se produire si le disque physique effectue une mise à jour incomplète ou incorrecte d’un secteur dans un fichier. vhd en cours de modification en cas de panne de courant. Cela affecte les ordinateurs virtuels suivants : *  
  
@no__t 0list de machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
*Shut sur la machine virtuelle et convertir le disque dur virtuel dynamique au format VHD en disque dur virtuel au format VHDX ou en disque dur virtuel fixe. (Le format VHDX a des mécanismes de fiabilité qui permettent de protéger le disque des altérations dues à des défaillances de l’alimentation du système.) Toutefois, ne convertissez pas le disque dur virtuel s’il est susceptible d’être attaché à une version antérieure de Windows à un moment donné. Les versions de Windows antérieures à Windows Server 2012 ne prennent pas en charge le format VHDX.*  
  


