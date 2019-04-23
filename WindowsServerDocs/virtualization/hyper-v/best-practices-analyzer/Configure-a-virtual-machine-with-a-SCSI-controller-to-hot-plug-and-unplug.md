---
title: Configurer une machine virtuelle avec un contrôleur SCSI pour pouvoir chaud plug et débranchez à chaud de stockage
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 511e1172-aeef-463d-b5dd-2bffae411ff1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 755e7485e54ee58e0acd7ebd75a7ee591aa655f9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843280"
---
# <a name="configure-a-virtual-machine-with-a-scsi-controller-to-be-able-to-hot-plug-and-hot-unplug-storage"></a>Configurer une machine virtuelle avec un contrôleur SCSI pour pouvoir chaud plug et débranchez à chaud de stockage

>S'applique à : Windows Server 2016


  
*Pour plus d’informations sur les meilleures pratiques et les analyses, consultez* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Une machine virtuelle a été trouvée qui n’est pas configuré avec un contrôleur SCSI.*  
  
## <a name="impact"></a>Impact  
  
*Vous ne pourrez pas chaud plug ou débranchez à chaud de stockage pour les ordinateurs virtuels suivants :*  
  
\<liste des noms de machine virtuelle >  
  
La capacité à chaud plug ou chaud débranchez stockage rend plus facile à gérer les besoins de stockage d’une machine virtuelle sans avoir besoin de temps d’arrêt. Machines virtuelles sans contrôleurs SCSI doit être arrêtés avant de pouvoir ajouter ou supprimer du stockage.  
  
## <a name="resolution"></a>Résolution  
  
*Si vous n’avez pas besoin à chaud plug ou débranchez à chaud de stockage pour cette machine virtuelle, aucune action n’est requise. Sinon, arrêtez la machine virtuelle et ajoutez un contrôleur SCSI à la configuration.*  
  
Pour utiliser un contrôleur SCSI à chaud et à chaud Débranchez le stockage, le système d’exploitation invité doit être en cours d’exécution la version actuelle des services d’intégration.  
  


