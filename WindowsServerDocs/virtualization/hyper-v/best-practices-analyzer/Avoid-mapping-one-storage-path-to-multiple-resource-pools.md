---
title: Évitez de mapper un chemin d’accès de stockage à plusieurs pools de ressources
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 24992453-762b-4892-9a50-55d237b9b7f2
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 7c012836309f722e55c28b2ddbe3d54de641b4af
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823960"
---
# <a name="avoid-mapping-one-storage-path-to-multiple-resource-pools"></a>Évitez de mapper un chemin d’accès de stockage à plusieurs pools de ressources

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Opérations|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.
  
## <a name="issue"></a>**Problème**  
*Un chemin d’accès du fichier de stockage est mappé à plusieurs pools de ressources.*  
  
## <a name="impact"></a>**Impact**  
*Pour le type de pool de stockage spécifié, les pools de parent et enfant suivants partagent le même chemin d’accès de stockage :*  
  
\<liste des pools >  
  
## <a name="resolution"></a>**Résolution**  
*Utiliser Windows PowerShell pour reconfigurer les pools de ressources de stockage afin que plusieurs pools n’utilisent pas le même chemin d’accès de stockage.*  
  


