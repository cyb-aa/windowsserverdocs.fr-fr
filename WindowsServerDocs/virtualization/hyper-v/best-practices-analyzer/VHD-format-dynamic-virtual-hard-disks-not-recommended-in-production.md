---
title: Les disques durs virtuels dynamiques au format VHD ne sont pas recommandés pour les machines virtuelles qui exécutent des charges de travail serveur dans un environnement de production
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 324a60a0-1d15-4ef2-9f17-23cbd2eb42ce
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 195bc4c85d380c8f0b15b27d042b30491f635d5d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854172"
---
# <a name="vhd-format-dynamic-virtual-hard-disks-are-not-recommended-for-virtual-machines-that-run-server-workloads-in-a-production-environment"></a>Les disques durs virtuels dynamiques au format VHD ne sont pas recommandés pour les machines virtuelles qui exécutent des charges de travail serveur dans un environnement de production

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.
  
## <a name="issue"></a>**Problème**  
*Une ou plusieurs machines virtuelles utilisent des disques durs virtuels de taille dynamique au format VHD.*  
  
## <a name="impact"></a>**Effet**  
*Les disques durs virtuels dynamiques au format VHD peuvent rencontrer des problèmes de cohérence en cas de panne de courant. Des problèmes de cohérence peuvent se produire si le disque physique effectue une mise à jour incomplète ou incorrecte d’un secteur dans un fichier. vhd en cours de modification en cas de panne de courant. Cela affecte les machines virtuelles suivantes :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
*Arrêtez l’ordinateur virtuel et convertissez le disque dur virtuel dynamique au format VHD en disque dur virtuel au format VHDX ou en disque dur virtuel fixe. (Le format VHDX a des mécanismes de fiabilité qui permettent de protéger le disque des altérations dues à des défaillances de l’alimentation du système.) Toutefois, ne convertissez pas le disque dur virtuel s’il est susceptible d’être attaché à une version antérieure de Windows à un moment donné. Les versions de Windows antérieures à Windows Server 2012 ne prennent pas en charge le format VHDX.*  
  


