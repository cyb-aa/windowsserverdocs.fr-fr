---
title: Évitez d’utiliser des disques durs virtuels avec une taille de secteur inférieur à la taille de secteur du stockage physique qui stocke le fichier de disque dur virtuel
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b7cf994e-bf4b-4b1b-bad6-ecf7d46d105e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: b6ec2e0995180ecf9ae5986447fdd460a1c7a337
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846260"
---
# <a name="avoid-using-virtual-hard-disks-with-a-sector-size-less-than-the-sector-size-of-the-physical-storage-that-stores-the-virtual-hard-disk-file"></a>Évitez d’utiliser des disques durs virtuels avec une taille de secteur inférieur à la taille de secteur du stockage physique qui stocke le fichier de disque dur virtuel

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**D’exploitation** <br />**Système**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*Un ou plusieurs disques durs virtuels ont une taille de secteur physique est inférieure à la taille de secteur physique du stockage sur lequel se trouve le fichier de disque dur virtuel.*  
  
## <a name="impact"></a>**Impact**  
*Problèmes de performances peuvent se produire sur l’ordinateur virtuel ou d’une application qui utilise le disque dur virtuel. Cela affecte les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
*Effectuez l’une des opérations suivantes :*  
  
-   *Effectuer une migration de stockage pour déplacer le disque dur virtuel vers un nouveau système physique*  
  
-   *Utiliser Windows PowerShell ou WMI pour activer un disque dur virtuel d’au format VHDX pour signaler une taille de secteur spécifique*  
  
-   *Utiliser un paramètre de Registre pour activer un disque dur virtuel d’au format VHD signaler une taille de secteur physique de 4 Ko*  
  


