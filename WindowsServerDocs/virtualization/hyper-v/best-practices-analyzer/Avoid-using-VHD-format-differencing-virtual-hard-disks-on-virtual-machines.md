---
title: Évitez d’utiliser des disques durs virtuels de différenciation au format VHD sur des machines virtuelles qui exécutent des charges de travail serveur dans un environnement de production
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 272de33d-2708-4679-8564-ee28848a2839
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: a11959266db4c9f3da73123c41a211198f27b9a5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857722"
---
# <a name="avoid-using-vhd-format-differencing-virtual-hard-disks-on-virtual-machines-that-run-server-workloads-in-a-production-environment"></a>Évitez d’utiliser des disques durs virtuels de différenciation au format VHD sur des machines virtuelles qui exécutent des charges de travail serveur dans un environnement de production

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
*Une ou plusieurs machines virtuelles utilisent des disques durs virtuels de différenciation au format VHD.*  
  
## <a name="impact"></a>**Effet**  
*Les disques durs virtuels de différenciation au format VHD peuvent rencontrer des problèmes de cohérence en cas de panne de courant. Des problèmes de cohérence peuvent se produire si le disque physique effectue une mise à jour incomplète ou incorrecte d’un secteur dans un fichier. vhd en cours de modification en cas de panne de courant. Cela affecte les machines virtuelles suivantes :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
*Arrêtez l’ordinateur virtuel et convertissez la chaîne de disques durs virtuels de différenciation au format VHD au format VHDX ou fusionnez la chaîne sur un disque dur virtuel fixe. (Le format VHDX a des mécanismes de fiabilité qui permettent de protéger le disque des altérations dues à des pannes d’alimentation.) Toutefois, ne convertissez pas le disque dur virtuel s’il est susceptible d’être attaché à une version antérieure de Windows à un moment donné. Les versions de Windows antérieures à Windows Server 2012 ne prennent pas en charge le format VHDX.*  
  


