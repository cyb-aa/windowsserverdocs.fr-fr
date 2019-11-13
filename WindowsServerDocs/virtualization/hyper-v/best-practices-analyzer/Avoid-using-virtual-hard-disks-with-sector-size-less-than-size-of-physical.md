---
title: Évitez d’utiliser des disques durs virtuels avec une taille de secteur inférieure à la taille de secteur du stockage physique qui stocke le fichier de disque dur virtuel.
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b7cf994e-bf4b-4b1b-bad6-ecf7d46d105e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 944fdce68a2f0b8e9c122f5f9134f0e07de18bbd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366409"
---
# <a name="avoid-using-virtual-hard-disks-with-a-sector-size-less-than-the-sector-size-of-the-physical-storage-that-stores-the-virtual-hard-disk-file"></a>Évitez d’utiliser des disques durs virtuels avec une taille de secteur inférieure à la taille de secteur du stockage physique qui stocke le fichier de disque dur virtuel.

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**D’exploitation** <br />**Requise**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*Un ou plusieurs disques durs virtuels ont une taille de secteur physique inférieure à la taille de secteur physique du stockage sur lequel se trouve le fichier de disque dur virtuel.*  
  
## <a name="impact"></a>**Impact**  
*Des problèmes de performances peuvent se produire sur l’ordinateur virtuel ou l’application qui utilise le disque dur virtuel. Cela a un impact sur les machines virtuelles suivantes :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
*Effectuez l’une des opérations suivantes :*  
  
-   *Effectuer une migration du stockage pour déplacer le disque dur virtuel vers un nouveau système physique*  
  
-   *Utiliser Windows PowerShell ou WMI pour activer un disque dur virtuel au format VHDX pour signaler une taille de secteur spécifique*  
  
-   *Utiliser un paramètre de Registre pour activer un disque dur virtuel au format VHD pour signaler une taille de secteur physique de 4 Ko*  
  


