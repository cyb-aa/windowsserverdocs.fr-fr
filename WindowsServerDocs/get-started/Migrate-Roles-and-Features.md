---
title: Migration de rôles et fonctionnalités
description: ''
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.date: 04/03/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0f78ef4c-dd12-4b1b-8c6e-251dd803c5d1
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 7469171005164d9ff823dad7de230d877c874dc9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840880"
---
# <a name="migrating-roles-and-features-in-windows-server"></a>Migration de rôles et de fonctionnalités dans Windows Server

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette page contient des liens vers des informations et des outils qui vous guident tout au long du processus de migration des rôles et des fonctionnalités vers Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012. Beaucoup de rôles et de fonctionnalités peuvent être migrés à l’aide des Outils de migration de Windows Server, un ensemble de cinq applets de commande Windows PowerShell qui a été introduit dans Windows Server 2008 R2 pour faciliter la migration des éléments et des données des rôles et des fonctionnalités.

Les guides de migration prennent en charge les migrations de fonctionnalités et de rôles spécifiés d’un serveur à un autre (pas les mises à niveau sur place). Sauf indication contraire dans les guides, les migrations sont prises en charge entre des ordinateurs physiques et virtuels et entre les options d’installation complète de Windows Server et les serveurs qui exécutent l’option d’installation minimale.  

## <a name="before-you-begin"></a>Avant de commencer

Avant de commencer la migration des rôles et des fonctionnalités, vérifiez que les serveurs source et cible exécutent les derniers service packs disponibles pour leurs systèmes d’exploitation.
Un livre électronique contenant les guides de migration de Windows Server 2012 R2 et Windows Server 2012 est désormais disponible. Pour plus d’informations et pour télécharger le livre électronique, reportez-vous à la [Galerie de livres électroniques des technologies Microsoft](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#MigrateRoles). 

>[!NOTE]
>Chaque fois que vous migrez ou effectuez une mise à niveau vers une version de Windows Server, vous devez passer en revue et comprendre la [politique de support](https://support.microsoft.com/lifecycle) et le cycle de vie de cette version et planifier la procédure en conséquence. Vous pouvez [rechercher les informations de cycle de vie](https://support.microsoft.com/lifecycle) pour la version de Windows Server qui vous intéresse.
 
## <a name="windows-server-2016"></a>Windows Server 2016

### <a name="migration-guides"></a>Guides de migration
Des guides de migration mis à jour pour Windows Server 2016 sont en cours de développement. Vérifiez la disponibilité de nouvelles mises à jour à cet emplacement ultérieurement. Dans de nombreux cas, les étapes décrites dans les guides de migration de Windows Server 2012 R2 sont encore pertinentes pour Windows Server 2016.

- [Services Bureau à distance](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/migrate-rds-role-services)
- [Serveur Web (IIS)](https://www.iis.net/downloads/microsoft/web-deploy)
- [Windows Server Update Services](https://technet.microsoft.com/library/hh852339.aspx)
- [MultiPoint Services](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/multipoint-services/multipoint-services-migrate)
 
## <a name="windows-server-2012-r2"></a>Windows Server 2012 R2

### <a name="migration-guides"></a>Guides de migration
Suivez les étapes décrites dans ces guides pour migrer des rôles et des fonctionnalités de serveurs qui exécutent Windows Server 2003, Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 ou Windows Server 2012 R2 vers Windows Server 2012 R2. Les Outils de migration de Windows Server inclus dans Windows Server 2012 R2 prennent en charge les migrations entre sous-réseaux.

- [Installer, utiliser et supprimer les outils de Migration de Windows Server](https://technet.microsoft.com/library/jj134202.aspx)
- [Guide de Migration de Services de certificats Active Directory pour Windows Server 2012 R2](https://technet.microsoft.com/library/dn486797.aspx)
- [Migration du Service de rôle Active Directory Federation Services vers Windows Server 2012 R2](https://technet.microsoft.com/library/dn486815.aspx)
- [Active Directory Rights Management Services Guide de Migration et mise à niveau](https://technet.microsoft.com/library/cc754277.aspx)
- [Migrer des Services de fichiers et stockage vers Windows Server 2012 R2](https://technet.microsoft.com/library/dn479292.aspx)
- [Migration de Hyper-V vers Windows Server 2012 R2 depuis Windows Server 2012](https://technet.microsoft.com/library/dn486799.aspx)
- [Migrer le serveur NPS vers Windows Server 2012](https://technet.microsoft.com/library/hh831652)
- [Migrer des Services Bureau à distance vers Windows Server 2012 R2](https://technet.microsoft.com/library/dn479239.aspx)
- [Migrer Windows Server Update Services vers Windows Server 2012 R2](https://technet.microsoft.com/library/hh852339.aspx)
- [Migrer des rôles de Cluster vers Windows Server 2012 R2](https://technet.microsoft.com/library/dn530779.aspx)
- [Migrer le serveur DHCP vers Windows Server 2012 R2](https://technet.microsoft.com/library/dn495425.aspx)
 
## <a name="windows-server-2012"></a>Windows Server 2012

### <a name="migration-guides"></a>Guides de migration
Suivez les étapes décrites dans ces guides pour migrer des rôles et des fonctionnalités à partir de serveurs qui exécutent Windows Server 2003, Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012 vers Windows Server 2012. Les Outils de migration de Windows Server inclus dans Windows Server 2012 prennent en charge les migrations entre sous-réseaux.

- [Installer, utiliser et supprimer les outils de Migration de Windows Server](https://technet.microsoft.com/library/jj134202)
- [Migrer des Services de rôle Active Directory Federation Services vers Windows Server 2012](https://technet.microsoft.com/library/jj647765)
- [Migrer l’autorité HRA vers Windows Server 2012](https://technet.microsoft.com/library/hh831513)
- [Migrer Hyper-V vers Windows Server 2012 à partir de Windows Server 2008 R2](https://technet.microsoft.com/library/jj574113)
- [Migrer la Configuration IP vers Windows Server 2012](https://technet.microsoft.com/library/jj574133)
- [Migrer le serveur NPS vers Windows Server 2012](https://technet.microsoft.com/library/hh831652)
- [Migrer d’impression et de documenter les Services vers Windows Server 2012](https://technet.microsoft.com/library/jj134150)
- [Migrer l’accès à distance vers Windows Server 2012](https://technet.microsoft.com/library/hh831423)
- [Migrer Windows Server Update Services vers Windows Server 2012](https://technet.microsoft.com/library/hh852339)
- [Mettre à niveau des contrôleurs de domaine Active Directory vers Windows Server 2012](https://technet.microsoft.com/library/hh994618.aspx)
- [Migration des Services en cluster et des Applications vers Windows Server 2012](https://technet.microsoft.com/library/dn486790.aspx)
 

Pour obtenir des ressources supplémentaires se rapportant à la migration, consultez [Migrer des rôles et des fonctionnalités vers Windows Server 2012](https://technet.microsoft.com/library/jj134039).

## <a name="windows-server-2008-r2"></a>Windows Server 2008 R2

### <a name="migration-guides"></a>Guides de migration
Suivez les étapes décrites dans ces guides pour migrer des rôles et des fonctionnalités à partir des serveurs qui exécutent Windows Server 2003, Windows Server 2008 ou Windows Server 2008 R2 vers Windows Server 2008 R2. Les Outils de migration de Windows Server inclus dans Windows Server 2008 R2 ne prennent pas en charge les migrations entre sous-réseaux.

- [Installation, accès et suppression des outils de Migration de Windows Server](https://technet.microsoft.com/library/dd379545)
- [Guide de Migration de Services de certificats Active Directory](https://technet.microsoft.com/library/ee126170)
- [Les Services de domaine Active Directory et Guide de Migration de serveur domaine nom DNS (système)](https://technet.microsoft.com/library/dd379558)
- [Guide de Migration de BranchCache](https://technet.microsoft.com/library/dd548365)
- [Guide de Migration de serveur Dynamic Host Configuration Protocol (DHCP)](https://technet.microsoft.com/library/dd379535)
- [Guide de Migration de Services de fichiers](https://technet.microsoft.com/library/dd379487)
- [Guide de Migration HRA](https://technet.microsoft.com/library/ee791829)
- [Guide de Migration d’Hyper-V](https://technet.microsoft.com/library/ee849855)
- [Guide de Migration de Configuration IP](https://technet.microsoft.com/library/dd379537)
- [Utilisateur local et le Guide de Migration](https://technet.microsoft.com/library/dd379531)
- [Guide de Migration NPS](https://technet.microsoft.com/library/ee791849)
- [Guide de Migration des Services d’impression](https://technet.microsoft.com/library/dd379488)
- [Guide de Migration des Services Bureau à distance](https://technet.microsoft.com/library/ff849223)
- [Guide de Migration RRAS](https://technet.microsoft.com/library/ee822825)
- [Informations et les tâches courantes de la Migration Windows Server](https://technet.microsoft.com/library/ff400258)
- [Guide de Migration de Windows Server Update Services 3.0 SP2](https://technet.microsoft.com/library/ee822826)  
Pour obtenir des ressources supplémentaires se rapportant à la migration, consultez [Migrer des rôles et des fonctionnalités vers Windows Server 2008 R2](https://technet.microsoft.com/library/dd365353).
