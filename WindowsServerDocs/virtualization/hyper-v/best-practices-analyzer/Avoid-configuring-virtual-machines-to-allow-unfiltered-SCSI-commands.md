---
title: Évitez de configurer des machines virtuelles pour autoriser les commandes SCSI non filtrées
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: dd4a3d78-a77f-451e-a383-d5cf45ea17cf
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: f401ce4d72f88d72529a95acea2a999df93679b6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888270"
---
# <a name="avoid-configuring-virtual-machines-to-allow-unfiltered-scsi-commands"></a>Évitez de configurer des machines virtuelles pour autoriser les commandes SCSI non filtrées

>S'applique à : Windows Server 2016


  
*Pour plus d’informations sur les meilleures pratiques et les analyses, consultez* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Opérations|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Un ordinateur virtuel est configuré pour autoriser les commandes SCSI non filtrées.*  
  
## <a name="impact"></a>Impact  
  
*En ignorant les commandes SCSI filtrage risque de poser un risque de sécurité. Cette configuration doit être activée uniquement s’il est nécessaire pour la compatibilité avec les applications de stockage en cours d’exécution dans le système d’exploitation invité. Les ordinateurs virtuels suivants sont configurés pour autoriser les commandes SCSI non filtrées :*  
  
\<liste des noms de machine virtuelle >  
  
## <a name="resolution"></a>Résolution  
  
*Contactez votre fournisseur de stockage pour déterminer si cette configuration est requise. En outre, si le système d’exploitation de gestion ou d’autres systèmes d’exploitation invités sont compromises ou comportement inhabituel, reconfigurer la machine virtuelle pour bloquer les commandes.*  
  


