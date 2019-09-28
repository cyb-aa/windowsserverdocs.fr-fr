---
title: Activer la publication de hachages pour les serveurs de fichiers membres du domaine
description: Cette rubrique fait partie du Guide de déploiement BranchCache pour Windows Server 2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les filiales.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: a3f1f7c4-d9b2-43e6-8bfa-fac707bbd4d3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1e450b9a2282cb4820b8802aa6d36e822f56ca12
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356593"
---
# <a name="enable-hash-publication-for-domain-member-file-servers"></a>Activer la publication de hachages pour les serveurs de fichiers membres du domaine

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Lorsque vous utilisez Active Directory Domain Services (AD DS), vous pouvez utiliser le stratégie de groupe de domaine pour activer la publication de hachages BranchCache pour plusieurs serveurs de fichiers. Pour ce faire, vous devez créer une unité d’organisation (UO), ajouter des serveurs de fichiers à l’unité d’organisation, créer une publication de hachage BranchCache stratégie de groupe un objet (GPO), puis configurer l’objet de stratégie de groupe.  
  
Consultez les rubriques suivantes pour activer la publication de hachages pour plusieurs serveurs de fichiers.  
  
-   [Créer l’unité d’organisation serveurs de fichiers BranchCache](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [Déplacer les serveurs de fichiers vers l’unité d’organisation serveurs de fichiers BranchCache](../../branchcache/deploy/Move-File-Servers-to-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [Créer l’objet de stratégie de groupe publication de hachages BranchCache](../../branchcache/deploy/Create-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  
-   [Configurer l’objet de stratégie de groupe publication de hachages BranchCache](../../branchcache/deploy/Configure-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  


