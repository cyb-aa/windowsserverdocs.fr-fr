---
title: Configurer un ordinateur virtuel avec un contrôleur SCSI pour pouvoir brancher et débrancher à chaud le stockage
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 511e1172-aeef-463d-b5dd-2bffae411ff1
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: bd49a911d278a1f07fe9630f39798204d760dd88
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862152"
---
# <a name="configure-a-virtual-machine-with-a-scsi-controller-to-be-able-to-hot-plug-and-hot-unplug-storage"></a>Configurer un ordinateur virtuel avec un contrôleur SCSI pour pouvoir brancher et débrancher à chaud le stockage

>S’applique à Windows Server 2016


  
*Pour plus d’informations sur les meilleures pratiques et les analyses, consultez* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Un ordinateur virtuel qui n’est pas configuré avec un contrôleur SCSI a été détecté.*  
  
## <a name="impact"></a>Impact  
  
*Vous ne serez pas en mesure de brancher ou débrancher à chaud le stockage pour les machines virtuelles suivantes :*  
  
\<liste des noms de machine virtuelle >  
  
La possibilité de brancher à chaud ou de débrancher le stockage permet de gérer plus facilement les besoins de stockage d’un ordinateur virtuel sans temps d’arrêt. Les machines virtuelles sans contrôleurs SCSI doivent être arrêtées pour que vous puissiez ajouter ou supprimer du stockage.  
  
## <a name="resolution"></a>Résolution  
  
*Si vous n’avez pas besoin de brancher à chaud ou de débrancher le stockage pour cette machine virtuelle, aucune action n’est nécessaire. Dans le cas contraire, arrêtez l’ordinateur virtuel et ajoutez un contrôleur SCSI à la configuration.*  
  
Pour utiliser un contrôleur SCSI pour brancher et débrancher à chaud le stockage, le système d’exploitation invité doit exécuter la version actuelle d’Integration Services.  
  


