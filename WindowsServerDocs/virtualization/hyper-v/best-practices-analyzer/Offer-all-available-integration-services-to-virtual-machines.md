---
title: Offrir tous les services d’intégration disponibles aux machines virtuelles
description: Fournit des instructions pour résoudre le problème signalé par cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2c4b2043-ad81-495e-aa7a-467f813bb3d2
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 5ec2dc73cea8b8356d832bf9fdb960985df2df6c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393601"
---
# <a name="offer-all-available-integration-services-to-virtual-machines"></a>Offrir tous les services d’intégration disponibles aux machines virtuelles

>S’applique à Windows Server 2016

Pour plus d'informations sur les meilleures pratiques et les analyses, consultez [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Un ou plusieurs services d’intégration disponibles ne sont pas activés sur les ordinateurs virtuels.*  
  
## <a name="impact"></a>Impact  
  
*Certaines fonctionnalités ne seront pas disponibles pour les machines virtuelles suivantes :*  
  
\<liste des noms de machine virtuelle >  
  
## <a name="resolution"></a>Résolution  
  
*Si cela est intentionnel, aucune action supplémentaire n’est requise. Dans le cas contraire, envisagez d’offrir tous les services d’intégration dans les paramètres de ces machines virtuelles.*  
  
La disponibilité de certains services d’intégration peut être gérée par le biais des paramètres de la machine virtuelle.   
  
#### <a name="to-manage-the-availability-of-integration-services-to-a-virtual-machine"></a>Pour gérer la disponibilité d’Integration Services sur une machine virtuelle  
  
1.  Ouvrez le Gestionnaire Hyper-V. Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestionnaire Hyper-V**.  
  
2.  Dans le volet de résultats, sous **machines virtuelles**, sélectionnez la machine virtuelle que vous souhaitez configurer.  
  
3.  Dans le volet **Action**, sous le nom de l'ordinateur virtuel, cliquez sur **Paramètres**.  
  
4.  Sous **gestion**, cliquez sur **Integration Services**.  
  
5.  Dans la liste des services d’intégration, activez la case à cocher de chaque service que vous souhaitez proposer à la machine virtuelle. Si une case à cocher n’est pas disponible, ce service d’intégration spécifique n’est pas pris en charge par le système d’exploitation invité qui s’exécute sur l’ordinateur virtuel.  
  


