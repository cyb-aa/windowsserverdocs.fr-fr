---
title: Contrôleurs de stockage doivent être activés dans les machines virtuelles pour fournir l’accès au stockage attaché
description: Fournit des instructions pour résoudre le problème signalé par cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 532548a1-8ffe-4b5b-902e-ed2f0819012b
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 42803a0eef84bf006e9f9e7ed6297ea21b4eb7b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849160"
---
# <a name="storage-controllers-should-be-enabled-in-virtual-machines-to-provide-access-to-attached-storage"></a>Contrôleurs de stockage doivent être activés dans les machines virtuelles pour fournir l’accès au stockage attaché

>S'applique à : Windows Server 2016

Pour plus d'informations sur les meilleures pratiques et les analyses, consultez [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Configuration|  

Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.

## <a name="issue"></a>Problème  
  
*Un ou plusieurs contrôleurs de stockage peuvent être désactivés dans une machine virtuelle.*  
  
## <a name="impact"></a>Impact  
  
*Machines virtuelles ne peuvent pas utiliser le stockage connecté à un contrôleur de stockage désactivé. Cela affecte les ordinateurs virtuels suivants :*  
  
\<liste des noms de machine virtuelle >  
  
## <a name="resolution"></a>Résolution  
  
*Utilisez le Gestionnaire de périphériques dans le système d’exploitation invité pour activer tous les contrôleurs de stockage. Si le contrôleur de stockage n’est pas obligatoire, utilisez le Gestionnaire Hyper-V pour le supprimer de la machine virtuelle.*  
  
Pour obtenir des instructions sur l’utilisation du Gestionnaire de périphériques, consultez l’aide dans le système d’exploitation invité. Pour obtenir des instructions sur la façon de supprimer le contrôleur de stockage, consultez la procédure suivante.  
  
#### <a name="to-remove-a-scsi-storage-controller-from-the-virtual-machine"></a>Pour supprimer un contrôleur de stockage SCSI à partir de la machine virtuelle  
  
1.  Ouvrez le Gestionnaire Hyper-V. Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestionnaire Hyper-V**.  
  
2.  Dans le volet de résultats, sous **Machines virtuelles**, sélectionnez la machine virtuelle que vous souhaitez configurer.  
  
3.  Si la machine virtuelle est en cours d’exécution, arrêtez la machine virtuelle. Avec le bouton droit de la machine virtuelle et cliquez sur **arrêter**.  
  
4.  Dans le volet **Action**, sous le nom de l'ordinateur virtuel, cliquez sur **Paramètres**.  
  
5.  Dans le volet gauche de la **paramètres** boîte de dialogue **matériel**, cliquez sur **contrôleur SCSI**.  
  
6.  Dans le volet droit, cliquez sur **supprimer**.  
  


