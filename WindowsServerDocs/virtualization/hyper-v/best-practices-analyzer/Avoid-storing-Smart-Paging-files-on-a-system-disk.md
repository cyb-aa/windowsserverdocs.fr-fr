---
title: Éviter de stocker les fichiers de pagination intelligente sur un disque système
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9b57c9b8-76c5-43c7-bfa6-2c95b3cb6510
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 111cf3b3b95f50d6d36a6b30b5a0bb46e255ee28
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857732"
---
# <a name="avoid-storing-smart-paging-files-on-a-system-disk"></a>Éviter de stocker les fichiers de pagination intelligente sur un disque système

>S’applique à Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Avertissement|  
|**Catégorie**|Opérations|  
  
Dans les sections suivantes, l’italique indique le texte qui apparaît dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>Problème  
*La configuration de la mémoire pour un ou plusieurs ordinateurs virtuels peut nécessiter l’utilisation de la pagination intelligente si la machine virtuelle est redémarrée, et que l’emplacement spécifié pour le fichier de pagination intelligente est le disque système du serveur exécutant Hyper-V.*  
  
## <a name="impact"></a>Impact  
*L’utilisation du disque système pour la pagination intelligente peut entraîner des problèmes sur le serveur exécutant Hyper-V. Cela affecte les machines virtuelles suivantes :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>Résolution  
*Reconfigurez les machines virtuelles pour stocker les fichiers de pagination intelligente sur un disque non-système.*  
  


