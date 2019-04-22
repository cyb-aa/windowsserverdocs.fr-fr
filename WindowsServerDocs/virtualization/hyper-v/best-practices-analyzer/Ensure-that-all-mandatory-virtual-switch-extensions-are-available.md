---
title: Vérifiez que toutes les extensions de commutateur virtuel obligatoires sont disponibles
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2f2f2698-f5ec-4cad-aa64-d6987e8142a1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 53ceeb9aab6ca7196454fbcd7f0fdae8b34d05d2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825940"
---
# <a name="ensure-that-all-mandatory-virtual-switch-extensions-are-available"></a>Vérifiez que toutes les extensions de commutateur virtuel obligatoires sont disponibles

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*Un ou plusieurs cartes réseau virtuelles sont connectés à un commutateur virtuel avec les extensions obligatoires qui sont désactivés ou pas installé.*  
  
## <a name="impact"></a>Impact  
*Le trafic réseau est bloqué sur un ou plusieurs cartes réseau virtuelles sur les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Tout d’abord, assurez-vous que l’extension obligatoire a été installée sur l’ordinateur hôte et installer l’extension si nécessaire. Ensuite, si l’extension obligatoire est désactivée, utilisez le Gestionnaire de commutateur virtuel ou l’applet de commande Windows PowerShell Enable-VMSwitchExtension pour activer l’extension.*  
  


