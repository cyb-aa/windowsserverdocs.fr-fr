---
title: Éviter de stocker les fichiers de pagination intelligente sur un disque système
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9b57c9b8-76c5-43c7-bfa6-2c95b3cb6510
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e3ddb662d14545693e26eb680527d93eb65d5d13
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365239"
---
# <a name="avoid-storing-smart-paging-files-on-a-system-disk"></a>Éviter de stocker les fichiers de pagination intelligente sur un disque système

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Warning|  
|**Catégorie**|Opérations|  
  
Dans les sections suivantes, l’italique indique le texte qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*La configuration de la mémoire pour un ou plusieurs ordinateurs virtuels peut nécessiter l’utilisation de la pagination intelligente si la machine virtuelle est redémarrée, et que l’emplacement spécifié pour le fichier de pagination intelligente est le disque système du serveur exécutant Hyper-V.*  
  
## <a name="impact"></a>Impact  
*Use du disque système pour la pagination intelligente peut entraîner des problèmes au niveau du serveur exécutant Hyper-V. Cela affecte les ordinateurs virtuels suivants :*  
  
@no__t 0list de machines virtuelles >  
  
## <a name="resolution"></a>Résolution :  
*Reconfigurez les machines virtuelles pour stocker les fichiers de pagination intelligente sur un disque non-système.*  
  


