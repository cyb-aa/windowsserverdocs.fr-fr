---
title: Éviter de configurer des machines virtuelles pour autoriser les commandes SCSI non filtrées
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: dd4a3d78-a77f-451e-a383-d5cf45ea17cf
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 5deb20862ed0e359febd4a9b58202d53c85058ca
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365271"
---
# <a name="avoid-configuring-virtual-machines-to-allow-unfiltered-scsi-commands"></a>Éviter de configurer des machines virtuelles pour autoriser les commandes SCSI non filtrées

>S'applique à : Windows Server 2016


  
*Pour plus d’informations sur les meilleures pratiques et les analyses, consultez* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Warning|  
|**Catégorie**|Opérations|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Un ordinateur virtuel est configuré pour autoriser les commandes SCSI non filtrées.*  
  
## <a name="impact"></a>Impact  
  
*Bypassing le filtrage des commandes SCSI pose un risque pour la sécurité. Cette configuration doit être activée uniquement si elle est requise pour la compatibilité avec les applications de stockage s’exécutant dans le système d’exploitation invité. Les ordinateurs virtuels suivants sont configurés pour autoriser les commandes SCSI non filtrées :*  
  
@no__t 0list de noms de machines virtuelles >  
  
## <a name="resolution"></a>Résolution :  
  
*Contact votre fournisseur de stockage pour déterminer si cette configuration est requise. De même, si le système d’exploitation de gestion ou d’autres systèmes d’exploitation invités sont compromis ou présentent un comportement inhabituel, reconfigurez l’ordinateur virtuel pour bloquer les commandes.*  
  


