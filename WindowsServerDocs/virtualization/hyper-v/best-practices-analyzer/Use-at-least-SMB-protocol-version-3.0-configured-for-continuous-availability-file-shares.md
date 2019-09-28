---
title: Utilisez au moins le protocole SMB version 3,0 configuré pour la disponibilité continue sur des partages de fichiers qui stockent des fichiers pour les machines virtuelles
description: Version en ligne du texte de cette règle de Best Practices Analyzer.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a1fa5cf9-8a48-4f63-bb57-d81e63e77b30
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 3a6cbb6052e2e50b7fd78792c5e01885d7672932
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393340"
---
# <a name="use-at-least-smb-protocol-version-30-configured-for-continuous-availability-on-file-shares-that-store-files-for-virtual-machines"></a>Utilisez au moins le protocole SMB version 3,0 configuré pour la disponibilité continue sur des partages de fichiers qui stockent des fichiers pour les machines virtuelles

>S'applique à : Windows Server 2016

Pour plus d’informations sur les bonnes pratiques et les analyses, consultez [Exécuter des analyses Best Practices Analyzer et gérer les résultats des analyses](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriété|Détails|  
|-|-|  
|**Système d'exploitation**|Windows Server 2016|  
|**Produit/fonctionnalité**|Hyper-V|  
|**Va**|Warning|  
|**Catégorie**|Configuration|  
  
Dans les sections suivantes, l’italique indique le texte de l’interface utilisateur qui s’affiche dans l’outil Best Practices Analyzer pour ce problème.  
  
## <a name="issue"></a>**Problème**  
*Les fichiers d’ordinateur virtuel ou les fichiers de disque dur virtuel sont stockés sur un partage de fichiers réseau qui n’est pas configuré avec la fonctionnalité de disponibilité continue de la version 3,0 du protocole SMB.*  
  
## <a name="impact"></a>**Impact**  
*Microsoft ne recommande pas cette configuration, car elle peut avoir un impact sur la disponibilité des machines virtuelles à l’aide du serveur. Cela a un impact sur les ordinateurs virtuels suivants :*  
  
@no__t 0list de machines virtuelles >  
  
## <a name="resolution"></a>**Résolution**  
Faites une des actions suivantes :  
  
-   Déplacez les fichiers vers un partage de fichiers SMB 3,0 configuré pour une disponibilité continue.  
  
-   Reconfigurez le partage de fichiers actuel pour assurer une disponibilité continue.  
  


