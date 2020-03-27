---
title: Activer la publication de hachages pour les serveurs de fichiers membres du domaine
description: Cette rubrique fait partie du Guide de déploiement BranchCache pour Windows Server 2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les filiales.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: a3f1f7c4-d9b2-43e6-8bfa-fac707bbd4d3
ms.author: lizross
author: eross-msft
ms.openlocfilehash: dd39a8d7f08e3ac3e6249017a042c343d9179566
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319285"
---
# <a name="enable-hash-publication-for-domain-member-file-servers"></a>Activer la publication de hachages pour les serveurs de fichiers membres du domaine

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Lorsque vous utilisez Active Directory Domain Services (AD DS), vous pouvez utiliser le stratégie de groupe de domaine pour activer la publication de hachages BranchCache pour plusieurs serveurs de fichiers. Pour ce faire, vous devez créer une unité d’organisation (UO), ajouter des serveurs de fichiers à l’unité d’organisation, créer une publication de hachage BranchCache stratégie de groupe un objet (GPO), puis configurer l’objet de stratégie de groupe.  
  
Consultez les rubriques suivantes pour activer la publication de hachages pour plusieurs serveurs de fichiers.  
  
-   [Créer l’unité d’organisation serveurs de fichiers BranchCache](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [Déplacer les serveurs de fichiers vers l’unité d’organisation serveurs de fichiers BranchCache](../../branchcache/deploy/Move-File-Servers-to-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [Créer l’objet de stratégie de groupe publication de hachages BranchCache](../../branchcache/deploy/Create-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  
-   [Configurer l’objet de stratégie de groupe publication de hachages BranchCache](../../branchcache/deploy/Configure-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  


