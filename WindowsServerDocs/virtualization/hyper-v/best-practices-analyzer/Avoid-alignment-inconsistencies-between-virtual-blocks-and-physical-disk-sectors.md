---
title: Éviter les incohérences d’alignement entre les blocs virtuels et les secteurs de disque physique sur des disques durs virtuels dynamiques ou des disques de différenciation
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a17c8fd2-af81-485b-bfea-bd1ef3e43923
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 3f090f015f2179ba372e56d580477ef8d72d977f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857802"
---
# <a name="avoid-alignment-inconsistencies-between-virtual-blocks-and-physical-disk-sectors-on-dynamic-virtual-hard-disks-or-differencing-disks"></a>Éviter les incohérences d’alignement entre les blocs virtuels et les secteurs de disque physique sur des disques durs virtuels dynamiques ou des disques de différenciation

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Des incohérences d’alignement ont été détectées pour un ou plusieurs disques durs virtuels.*  
  
### <a name="impact"></a>Impact  
*Si les disques durs virtuels sont stockés sur un disque physique ayant une taille de secteur de 4 Ko, l’ordinateur virtuel ou les applications qui utilisent le disque dur virtuel peuvent rencontrer des problèmes de performances. Cela affecte les machines virtuelles suivantes :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Utilisez l’Assistant Création d’un disque dur virtuel pour créer un nouveau disque dur virtuel au format VHD ou VHDX et spécifiez le disque dur virtuel existant comme disque source. Le nouveau disque dur virtuel sera créé avec l’alignement entre les blocs virtuels et le disque physique.*  
  


