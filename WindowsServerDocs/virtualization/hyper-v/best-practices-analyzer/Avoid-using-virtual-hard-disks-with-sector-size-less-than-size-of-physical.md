---
title: Évitez d’utiliser des disques durs virtuels avec une taille de secteur inférieure à la taille de secteur du stockage physique qui stocke le fichier de disque dur virtuel.
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b7cf994e-bf4b-4b1b-bad6-ecf7d46d105e
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: f7ea02ab83d3d896d2ad3681526133e23725b819
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857702"
---
# <a name="avoid-using-virtual-hard-disks-with-a-sector-size-less-than-the-sector-size-of-the-physical-storage-that-stores-the-virtual-hard-disk-file"></a>Évitez d’utiliser des disques durs virtuels avec une taille de secteur inférieure à la taille de secteur du stockage physique qui stocke le fichier de disque dur virtuel.

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**D’exploitation** <br />**Requise**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*Un ou plusieurs disques durs virtuels ont une taille de secteur physique inférieure à la taille de secteur physique du stockage sur lequel se trouve le fichier de disque dur virtuel.*  
  
## <a name="impact"></a>**Effet**  
*Des problèmes de performances peuvent se produire sur l’ordinateur virtuel ou l’application qui utilise le disque dur virtuel. Cela a un impact sur les machines virtuelles suivantes :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
*Effectuez l’une des opérations suivantes :*  
  
-   *Effectuer une migration du stockage pour déplacer le disque dur virtuel vers un nouveau système physique*  
  
-   *Utiliser Windows PowerShell ou WMI pour activer un disque dur virtuel au format VHDX pour signaler une taille de secteur spécifique*  
  
-   *Utiliser un paramètre de Registre pour activer un disque dur virtuel au format VHD pour signaler une taille de secteur physique de 4 Ko*  
  


