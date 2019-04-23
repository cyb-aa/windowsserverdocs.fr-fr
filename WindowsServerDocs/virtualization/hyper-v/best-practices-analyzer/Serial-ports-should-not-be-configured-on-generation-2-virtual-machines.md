---
title: Ports série ne doivent pas être configurés sur les ordinateurs virtuels de génération 2
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 87061193-dd3f-4398-aa5d-4cee83cadfa3
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 58c3fc5f975b85ce17ac5f7cca4930ec9e851e07
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877380"
---
# <a name="serial-ports-should-not-be-configured-on-generation-2-virtual-machines"></a>Ports série ne doivent pas être configurés sur les ordinateurs virtuels de génération 2

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*Génération d’un ou plusieurs 2 machines virtuelles ont un port série configuré.*  
  
## <a name="impact"></a>**Impact**  
*Performances peuvent être affectées pour les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
*Si cela est intentionnel, aucune action supplémentaire n’est requise. Sinon, envisagez d’utiliser le Gestionnaire Hyper-V ou Windows PowerShell pour supprimer la chaîne de connexion à partir des ports série sur l’ordinateur virtuel.*  
  


