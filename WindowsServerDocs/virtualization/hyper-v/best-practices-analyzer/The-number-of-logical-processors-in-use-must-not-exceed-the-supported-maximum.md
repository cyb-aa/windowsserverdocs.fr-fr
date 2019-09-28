---
title: Le nombre de processeurs logiques en cours d’utilisation ne doit pas dépasser la valeur maximale prise en charge
description: Fournit des instructions pour résoudre le problème signalé par cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 66df8b02-91d1-424b-8934-a39c214d530e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 380daf333c041c8702228a60c26ab6e76e4cf3e1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393395"
---
# <a name="the-number-of-logical-processors-in-use-must-not-exceed-the-supported-maximum"></a>Le nombre de processeurs logiques en cours d’utilisation ne doit pas dépasser la valeur maximale prise en charge

>S'applique à : Windows Server 2016

Pour plus d'informations sur les meilleures pratiques et les analyses, consultez [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Error|  
|**Catégorie**|Stratégie|  
  
Dans les sections suivantes, l’italique indique le texte qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*Le serveur est configuré avec un trop grand nombre de processeurs logiques.*  
  
## <a name="impact"></a>Impact  
  
*Microsoft ne prend pas en charge l’exécution d’Hyper-V sur cet ordinateur.*  
  
## <a name="resolution"></a>Résolution :  
  
*Supprimez certains processeurs de cet ordinateur ou utilisez Msconfig pour limiter le nombre de processeurs disponibles.*  
  
Consultez les instructions suivantes pour utiliser msconfig. Pour plus d’informations sur la suppression de processeurs, consultez les instructions fournies avec l’ordinateur ou contactez le fabricant du matériel. Pour plus d’informations sur les configurations maximales prises en charge pour Hyper-V, consultez [planifier l’extensibilité d’Hyper-v dans Windows Server 2016](../plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md).  
  
### <a name="to-limit-the-number-of-available-processors"></a>Pour limiter le nombre de processeurs disponibles  
  
1.  Ouvrez l’application de configuration du système (Msconfig. exe). Pour ce faire, cliquez sur **Démarrer**, tapez **msconfig**, cliquez avec le bouton droit sur l’application de bureau **Configuration système** , puis cliquez sur **exécuter en tant qu’administrateur**.  
  
2.  Dans l’onglet **démarrage** , cliquez sur **Options avancées**.  
  
3.  Sélectionnez **nombre de processeurs** , puis sélectionnez un nombre dans la liste. Cliquez sur **OK**.  
  
4.  Redémarrez l’ordinateur pour l’exécuter à l’aide du nouveau nombre de processeurs.  
  


