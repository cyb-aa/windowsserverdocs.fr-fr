---
title: Éviter de configurer des machines virtuelles pour autoriser les commandes SCSI non filtrées
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: dd4a3d78-a77f-451e-a383-d5cf45ea17cf
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: ac059bce1704a4e72b2c373d8186dbd4e31f2164
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857792"
---
# <a name="avoid-configuring-virtual-machines-to-allow-unfiltered-scsi-commands"></a>Éviter de configurer des machines virtuelles pour autoriser les commandes SCSI non filtrées

>S’applique à Windows Server 2016


  
*Pour plus d’informations sur les meilleures pratiques et les analyses, consultez* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Opérations|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Un ordinateur virtuel est configuré pour autoriser les commandes SCSI non filtrées.*  
  
## <a name="impact"></a>Impact  
  
*Le contournement du filtrage des commandes SCSI pose un risque pour la sécurité. Cette configuration doit être activée uniquement si elle est requise pour la compatibilité avec les applications de stockage s’exécutant dans le système d’exploitation invité. Les ordinateurs virtuels suivants sont configurés pour autoriser les commandes SCSI non filtrées :*  
  
\<liste des noms de machine virtuelle >  
  
## <a name="resolution"></a>Résolution  
  
*Contactez votre fournisseur de stockage pour déterminer si cette configuration est requise. De même, si le système d’exploitation de gestion ou d’autres systèmes d’exploitation invités sont compromis ou présentent un comportement inhabituel, reconfigurez l’ordinateur virtuel pour bloquer les commandes.*  
  


