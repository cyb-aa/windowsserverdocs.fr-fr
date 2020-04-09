---
title: Les contrôleurs de stockage doivent être activés sur les machines virtuelles pour fournir l’accès au stockage attaché
description: Fournit des instructions pour résoudre le problème signalé par cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 532548a1-8ffe-4b5b-902e-ed2f0819012b
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: b530a5868633e6007f311f3d15c94b7ec4ded52c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858792"
---
# <a name="storage-controllers-should-be-enabled-in-virtual-machines-to-provide-access-to-attached-storage"></a>Les contrôleurs de stockage doivent être activés sur les machines virtuelles pour fournir l’accès au stockage attaché

>S’applique à Windows Server 2016

Pour plus d'informations sur les meilleures pratiques et les analyses, consultez [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  

Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.

## <a name="issue"></a>Problème  
  
*Un ou plusieurs contrôleurs de stockage peuvent être désactivés sur un ordinateur virtuel.*  
  
## <a name="impact"></a>Impact  
  
*Les machines virtuelles ne peuvent pas utiliser le stockage connecté à un contrôleur de stockage désactivé. Cela a un impact sur les machines virtuelles suivantes :*  
  
\<liste des noms de machine virtuelle >  
  
## <a name="resolution"></a>Résolution  
  
*Utilisez Device Manager dans le système d’exploitation invité pour activer tous les contrôleurs de stockage. Si le contrôleur de stockage n’est pas requis, utilisez le Gestionnaire Hyper-V pour le supprimer de la machine virtuelle.*  
  
Pour obtenir des instructions sur l’utilisation de Device Manager, consultez l’aide du système d’exploitation invité. Pour obtenir des instructions sur la façon de supprimer le contrôleur de stockage, consultez la procédure suivante.  
  
#### <a name="to-remove-a-scsi-storage-controller-from-the-virtual-machine"></a>Pour supprimer un contrôleur de stockage SCSI de l’ordinateur virtuel  
  
1.  Ouvrez le Gestionnaire Hyper-V. Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestionnaire Hyper-V**.  
  
2.  Dans le volet de résultats, sous **machines virtuelles**, sélectionnez la machine virtuelle que vous souhaitez configurer.  
  
3.  Si la machine virtuelle est en cours d’exécution, arrêtez la machine virtuelle. Cliquez avec le bouton droit sur la machine virtuelle, puis cliquez sur **arrêter**.  
  
4.  Dans le volet **Action**, sous le nom de l’ordinateur virtuel, cliquez sur **Paramètres**.  
  
5.  Dans le volet gauche de la boîte de dialogue **paramètres** , sous **matériel**, cliquez sur **contrôleur SCSI**.  
  
6.  Dans le volet droit, cliquez sur **supprimer**.  
  


