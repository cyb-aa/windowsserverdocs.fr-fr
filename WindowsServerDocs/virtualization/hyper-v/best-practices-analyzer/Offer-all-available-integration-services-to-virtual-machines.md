---
title: Offre tous les services d’intégration disponibles pour les machines virtuelles
description: Fournit des instructions pour résoudre le problème signalé par cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2c4b2043-ad81-495e-aa7a-467f813bb3d2
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c2b5137594f78980f87f6520ae4b4af8203aef32
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883780"
---
# <a name="offer-all-available-integration-services-to-virtual-machines"></a>Offre tous les services d’intégration disponibles pour les machines virtuelles

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
  
*Un ou plusieurs services d’intégration disponibles ne sont pas activés sur les machines virtuelles.*  
  
## <a name="impact"></a>Impact  
  
*Certaines fonctionnalités ne seront pas disponibles pour les ordinateurs virtuels suivants :*  
  
\<liste des noms de machine virtuelle >  
  
## <a name="resolution"></a>Résolution  
  
*Si cela est intentionnel, aucune action supplémentaire n’est requise. Sinon, envisagez d’offrir tous les services d’intégration dans les paramètres de ces machines virtuelles.*  
  
La disponibilité de certains services d’intégration peut être gérée via les paramètres de la machine virtuelle.   
  
#### <a name="to-manage-the-availability-of-integration-services-to-a-virtual-machine"></a>Pour gérer la disponibilité des services d’intégration à une machine virtuelle  
  
1.  Ouvrez le Gestionnaire Hyper-V. Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestionnaire Hyper-V**.  
  
2.  Dans le volet de résultats, sous **Machines virtuelles**, sélectionnez la machine virtuelle que vous souhaitez configurer.  
  
3.  Dans le volet **Action**, sous le nom de l'ordinateur virtuel, cliquez sur **Paramètres**.  
  
4.  Sous **gestion**, cliquez sur **Integration Services**.  
  
5.  Dans la liste des services d’intégration, sélectionnez la case à cocher pour chaque service que vous souhaitez proposer à la machine virtuelle. Si une case à cocher n’est pas disponible, que le service d’intégration particulier n’est pas pris en charge par le système d’exploitation invité qui s’exécute dans la machine virtuelle.  
  


