---
title: Le nombre de processeurs logiques en cours d’utilisation ne doit pas dépasser le nombre maximal pris en charge
description: Fournit des instructions pour résoudre le problème signalé par cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 66df8b02-91d1-424b-8934-a39c214d530e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: d1275a17cc04494708f5ecfe9b708834b4233641
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847070"
---
# <a name="the-number-of-logical-processors-in-use-must-not-exceed-the-supported-maximum"></a>Le nombre de processeurs logiques en cours d’utilisation ne doit pas dépasser le nombre maximal pris en charge

>S'applique à : Windows Server 2016

Pour plus d'informations sur les meilleures pratiques et les analyses, consultez [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Erreur|  
|**Catégorie**|Stratégie|  
  
Dans les sections suivantes, italique indique le texte qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Le serveur est configuré avec un trop grand nombre de processeurs logiques.*  
  
## <a name="impact"></a>Impact  
  
*Microsoft ne prend pas en charge Hyper-V en cours d’exécution sur cet ordinateur.*  
  
## <a name="resolution"></a>Résolution  
  
*Supprimez certains processeurs à partir de cet ordinateur, ou utiliser msconfig pour limiter le nombre de processeurs disponibles.*  
  
Consultez les instructions suivantes à utiliser Msconfig. Pour plus d’informations sur la suppression des processeurs, consultez les instructions fournies avec l’ordinateur ou contactez le fabricant de matériel. Pour plus d’informations sur les configurations prises en charge maximales pour Hyper-V, consultez [planifier extensibilité Hyper-V dans Windows Server 2016](../plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md).  
  
### <a name="to-limit-the-number-of-available-processors"></a>Pour limiter le nombre de processeurs disponibles  
  
1.  Ouvrez le système (Msconfig.exe) d’application de Configuration. Pour ce faire, cliquez sur **Démarrer**, type **msconfig**, avec le bouton droit le **Configuration système** application de bureau et cliquez sur **exécuter en tant qu’administrateur**.  
  
2.  À partir de la **démarrage** , cliquez sur **les options avancées**.  
  
3.  Sélectionnez **nombre de processeurs** , puis sélectionnez un nombre dans la liste. Cliquez sur **OK**.  
  
4.  Redémarrez l’ordinateur pour l’exécuter à l’aide du nouveau nombre de processeurs.  
  


