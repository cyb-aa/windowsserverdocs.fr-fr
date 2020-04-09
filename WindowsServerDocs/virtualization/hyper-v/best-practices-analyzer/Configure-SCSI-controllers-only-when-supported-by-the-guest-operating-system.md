---
title: Configurer des contrôleurs SCSI uniquement lorsqu’ils sont pris en charge par le système d’exploitation invité
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 861f194f-467e-4b07-a1c5-55b35f6327c4
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: cf206d9568ef7634d724f3fce450985c34ebfac5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862162"
---
# <a name="configure-scsi-controllers-only-when-supported-by-the-guest-operating-system"></a>Configurer des contrôleurs SCSI uniquement lorsqu’ils sont pris en charge par le système d’exploitation invité

>S’applique à Windows Server 2016


  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Un ordinateur virtuel est configuré avec un contrôleur SCSI qui ne peut pas être utilisé, car le système d’exploitation invité ne prend pas en charge les contrôleurs SCSI.*  
  
## <a name="impact"></a>Impact  
  
*Les machines virtuelles ne peuvent pas utiliser le stockage attaché au contrôleur SCSI. Cela a un impact sur les machines virtuelles suivantes :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
  
*Arrêtez l’ordinateur virtuel et utilisez le Gestionnaire Hyper-V pour supprimer le contrôleur SCSI de l’ordinateur virtuel. Ensuite, redémarrez la machine virtuelle.*  
  


