---
title: Utilisez au moins version du protocole SMB 3.0 pour les partages de fichiers qui stockent les fichiers pour les machines virtuelles.
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4bb832b8-f1aa-4c1f-a0f2-324dd53553ea
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 28e0f3769fd4fc993710d0a0b800dfad7c9ab157
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834340"
---
# <a name="use-at-least-smb-protocol-version-30-for-file-shares-that-store-files-for-virtual-machines"></a>Utilisez au moins version du protocole SMB 3.0 pour les partages de fichiers qui stockent les fichiers pour les machines virtuelles.

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Erreur|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, italique indique le texte de l’interface utilisateur qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*Fichiers d’ordinateur virtuel ou des fichiers de disque dur virtuel sont stockés sur un partage de fichiers qui ne prend pas en charge au moins la version 3.0 du protocole SMB.*  
  
## <a name="impact"></a>**Impact**  
*Microsoft ne prend pas en charge cette configuration. Cela affecte les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
*Déplacer les fichiers vers un partage de fichiers qui utilise au moins la version 3.0 du protocole SMB.*  
  


