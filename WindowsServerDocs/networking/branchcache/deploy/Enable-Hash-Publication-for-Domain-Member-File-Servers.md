---
title: Activer la publication de hachages pour les serveurs de fichiers membres du domaine
description: Cette rubrique fait partie de BranchCache déploiement Guide pour Windows Server 2016, qui montre comment déployer BranchCache en mode cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les succursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: a3f1f7c4-d9b2-43e6-8bfa-fac707bbd4d3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 174e83c950d2aff8afba4f05641a74861b9a7938
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865460"
---
# <a name="enable-hash-publication-for-domain-member-file-servers"></a>Activer la publication de hachages pour les serveurs de fichiers membres du domaine

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Lorsque vous utilisez des Services de domaine Active Directory (AD DS), vous pouvez utiliser la stratégie de groupe de domaine pour activer la publication de hachages BranchCache pour plusieurs serveurs de fichiers. Vous devez donc créer une unité d’organisation (UO), ajouter des serveurs de fichiers à l’unité d’organisation, créer une objet de stratégie de groupe (GPO) de la publication de hachages BranchCache, puis configurez l’objet de stratégie de groupe.  
  
Consultez les rubriques suivantes pour activer la publication de hachages pour plusieurs serveurs de fichiers.  
  
-   [Créer l’unité d’organisation serveurs de fichiers de BranchCache](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [Déplacez les serveurs de fichiers à l’unité d’organisation serveurs de fichiers de BranchCache](../../branchcache/deploy/Move-File-Servers-to-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [Créer l’objet de stratégie de groupe de Publication de hachage BranchCache](../../branchcache/deploy/Create-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  
-   [Configurer l’objet de stratégie de groupe de Publication de hachage BranchCache](../../branchcache/deploy/Configure-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  


