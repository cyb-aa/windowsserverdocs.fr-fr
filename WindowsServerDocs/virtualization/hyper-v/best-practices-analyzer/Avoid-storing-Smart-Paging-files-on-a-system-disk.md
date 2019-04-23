---
title: Évitez de stocker des fichiers sur un disque de système de pagination intelligente
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9b57c9b8-76c5-43c7-bfa6-2c95b3cb6510
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6abc84b406de7e7c33628ccee4e3af706efe5c70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886170"
---
# <a name="avoid-storing-smart-paging-files-on-a-system-disk"></a>Évitez de stocker des fichiers sur un disque de système de pagination intelligente

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Niveau de gravité**|Warning|  
|**Catégorie**|Opérations|  
  
Dans les sections suivantes, italique indique le texte qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*La configuration de la mémoire pour un ou plusieurs machines virtuelles peut nécessiter l’utilisation de la pagination intelligente si l’ordinateur virtuel est redémarré, et l’emplacement spécifié pour le fichier de pagination intelligente est le disque du système du serveur exécutant Hyper-V.*  
  
## <a name="impact"></a>Impact  
*Utilisation du disque du système pour la pagination intelligente peut entraîner le serveur exécutant Hyper-V à rencontrer des problèmes. Cela affecte les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Reconfigurer les machines virtuelles pour stocker les fichiers de pagination intelligente sur un disque non-système.*  
  


