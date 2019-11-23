---
title: Évitez de mapper un chemin de stockage à plusieurs pools de ressources
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 24992453-762b-4892-9a50-55d237b9b7f2
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e89c0382d20d586d8c0b50396ddbd56d6fdadf0b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365262"
---
# <a name="avoid-mapping-one-storage-path-to-multiple-resource-pools"></a>Évitez de mapper un chemin de stockage à plusieurs pools de ressources

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Opérations|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.
  
## <a name="issue"></a>**Problème**  
*Un chemin d’accès au fichier de stockage est mappé à plusieurs pools de ressources.*  
  
## <a name="impact"></a>**Impact**  
*Pour le type de pool de stockage spécifié, les pools parents et enfants suivants partagent le même chemin de stockage :*  
  
\<liste des pools >  
  
## <a name="resolution"></a>**Résolution**  
*Utilisez Windows PowerShell pour reconfigurer les pools de ressources de stockage de sorte que plusieurs pools n’utilisent pas le même chemin de stockage.*  
  


