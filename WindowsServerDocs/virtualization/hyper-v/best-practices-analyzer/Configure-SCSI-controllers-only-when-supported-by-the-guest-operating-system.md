---
title: Configurer les contrôleurs SCSI uniquement lors de la prise en charge par le système d’exploitation invité
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 861f194f-467e-4b07-a1c5-55b35f6327c4
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 3dc48602ab6c71c60fdb734ca98cf1359f58d87c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830390"
---
# <a name="configure-scsi-controllers-only-when-supported-by-the-guest-operating-system"></a>Configurer les contrôleurs SCSI uniquement lors de la prise en charge par le système d’exploitation invité

>S'applique à : Windows Server 2016


  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Un ordinateur virtuel est configuré avec un contrôleur SCSI qui ne peut pas être utilisé, car le système d’exploitation invité ne prend pas en charge les contrôleurs SCSI.*  
  
## <a name="impact"></a>Impact  
  
*Machines virtuelles ne peuvent pas utiliser le stockage attaché au contrôleur SCSI. Cela affecte les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
  
*Arrêter la machine virtuelle et utilisez le Gestionnaire Hyper-V pour supprimer le contrôleur SCSI à partir de la machine virtuelle. Ensuite, redémarrez la machine virtuelle.*  
  


