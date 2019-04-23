---
title: Utilisez au moins version du protocole SMB 3.0 configuré pour une disponibilité continue sur les partages de fichiers qui stockent les fichiers pour les machines virtuelles
description: Version en ligne du texte pour cette règle de Best Practices Analyzer.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a1fa5cf9-8a48-4f63-bb57-d81e63e77b30
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 67f41293433bd8d14096688fbaa23eb43334c738
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877870"
---
# <a name="use-at-least-smb-protocol-version-30-configured-for-continuous-availability-on-file-shares-that-store-files-for-virtual-machines"></a>Utilisez au moins version du protocole SMB 3.0 configuré pour une disponibilité continue sur les partages de fichiers qui stockent les fichiers pour les machines virtuelles

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
*Fichiers d’ordinateur virtuel ou des fichiers de disque dur virtuel sont stockés sur un partage de fichiers réseau qui n’est pas configuré avec la fonctionnalité de disponibilité continue de la version du protocole SMB 3.0.*  
  
## <a name="impact"></a>**Impact**  
*Microsoft ne recommande pas cette configuration, car il pourrait affecter la disponibilité des machines virtuelles à l’aide du serveur. Cela affecte les ordinateurs virtuels suivants :*  
  
\<liste des machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
Faites une des actions suivantes :  
  
-   Déplacer les fichiers vers un partage de fichiers SMB 3.0 qui est configuré pour une disponibilité continue.  
  
-   Reconfigurez le partage de fichiers en cours pour assurer une disponibilité continue.  
  


