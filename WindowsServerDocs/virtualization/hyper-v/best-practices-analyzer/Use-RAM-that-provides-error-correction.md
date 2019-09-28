---
title: Utiliser la RAM qui permet de corriger les erreurs
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 67eb6cef-b045-4748-90e1-406af5345d6a
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c6f232ba44631e35190688d6c48a3bc53224bc17
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393452"
---
# <a name="use-ram-that-provides-error-correction"></a>Utiliser la RAM qui permet de corriger les erreurs

>S'applique à : Windows Server 2016

Pour plus d'informations sur les meilleures pratiques et les analyses, consultez [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Error|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
  
*La RAM utilisée sur cet ordinateur n’est pas une RAM de correction des erreurs (ECC).*  
  
## <a name="impact"></a>Impact  
  
*Microsoft ne prend pas en charge Windows Server 2016 sur les ordinateurs sans erreur de correction de la RAM.*  
  
## <a name="resolution"></a>Résolution :  
  
*Vérifiez que le serveur est listé dans le catalogue Windows Server et qu’il est qualifié pour Hyper-V.*  
  
Pour vérifier si le serveur est listé, consultez le [Catalogue Windows Server](https://www.windowsservercatalog.com/).  
  


