---
title: Migration de rôles et de fonctionnalités
description: Informations sur la migration des rôles et des fonctionnalités vers une version plus récente de Windows Server.
ms.prod: windows-server
ms.date: 08/28/2019
ms.technology: server-general
ms.topic: article
ms.assetid: 0f78ef4c-dd12-4b1b-8c6e-251dd803c5d1
author: jasongerend
ms.author: jgerend
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 33c1aa654e4c660b4fe2f3305bfaf78b5191220a
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "70119198"
---
# <a name="migrating-roles-and-features-in-windows-server"></a>Migration de rôles et de fonctionnalités dans Windows Server

> S'applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette page contient des liens vers des informations et des outils qui vous guident tout au long du processus de migration des rôles et des fonctionnalités vers une version plus récente de Windows Server. Vous pouvez migrer les serveurs de fichiers et le stockage avec le [service de migration de stockage](../storage/storage-migration-service/overview.md), tandis que de nombreux autres rôles et fonctionnalités peuvent être migrés avec les outils de migration de Windows Server, un ensemble d’applets de commande PowerShell qui ont été introduits dans Windows. Server 2008 R2 pour la migration des rôles et des fonctionnalités.

Les guides de migration prennent en charge les migrations de fonctionnalités et de rôles spécifiés d'un serveur vers un autre (pas les mises à niveau sur place). Sauf indication contraire dans les guides, les migrations sont prises en charge entre des ordinateurs physiques et virtuels, et entre les options d'installation complète de Windows Server et les serveurs qui exécutent l'option d'installation minimale.

## <a name="before-you-begin"></a>Avant de commencer

Avant de commencer la migration des rôles et des fonctionnalités, vérifiez que les serveurs source et cible exécutent les derniers service packs disponibles pour leurs systèmes d'exploitation. 

> [!NOTE]
> Chaque fois que vous migrez ou effectuez une mise à niveau vers une version de Windows Server, vous devez passer en revue et comprendre la [stratégie de support](https://support.microsoft.com/lifecycle) et le cycle de vie de cette version et planifier la procédure en conséquence. Vous pouvez [rechercher les informations relatives au cycle de vie](https://support.microsoft.com/lifecycle) de la version de Windows Server qui vous intéresse.

## <a name="windows-server-2019"></a>Windows Server 2019

Pour migrer les serveurs de fichiers et le stockage vers Windows Server 2019 ou Windows Server 2016 , nous vous recommandons d’utiliser le [service de migration de stockage](../storage/storage-migration-service/overview.md). Pour migrer d’autres rôles, reportez-vous aux conseils pour Windows Server 2016 et Windows Server 2012 R2.

## <a name="windows-server-2016"></a>Windows Server 2016

Voici les guides de migration pour Windows Server 2016. Notez que dans de nombreux cas, vous pouvez également utiliser les guides de migration de Windows Server 2012 R2.

- [Services Bureau à distance](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/migrate-rds-role-services)
- [Serveur web (IIS)](https://www.iis.net/downloads/microsoft/web-deploy)
- [Windows Server Update Services](https://technet.microsoft.com/library/hh852339.aspx)
- [MultiPoint Services](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/multipoint-services/multipoint-services-migrate)

Pour migrer des serveurs de fichiers vers Windows Server 2019 ou Windows Server 2016, nous vous recommandons d’utiliser le [service de migration de stockage](../storage/storage-migration-service/overview.md).

## <a name="windows-server-2012-r2"></a>Windows Server 2012 R2

Suivez les étapes décrites dans ces guides pour migrer des rôles et des fonctionnalités de serveurs qui exécutent Windows Server 2003, Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 ou Windows Server 2012 R2 vers Windows Server 2012 R2. Les Outils de migration de Windows Server inclus dans Windows Server 2012 R2 prennent en charge les migrations entre sous-réseaux.

- [Installer, utiliser et supprimer les Outils de migration de Windows Server](https://technet.microsoft.com/library/jj134202.aspx)
- [Guide de migration des services de certificats Active Directory pour Windows Server 2012 R2](https://technet.microsoft.com/library/dn486797.aspx)
- [Migration du service de rôle Services AD FS (Active Directory Federation Services) vers Windows Server 2012 R2](https://technet.microsoft.com/library/dn486815.aspx)
- [Guide de mise à niveau et de migration des services AD RMS (Active Directory Rights Management Services)](https://technet.microsoft.com/library/cc754277.aspx)
- [Migrer le rôle Services de fichiers et de stockage vers Windows Server 2012 R2](https://technet.microsoft.com/library/dn479292.aspx)
- [Migration de Hyper-V vers Windows Server 2012 R2 depuis Windows Server 2012](https://technet.microsoft.com/library/dn486799.aspx)
- [Migrer un serveur NPS (Network Policy Server) vers Windows Server 2012](https://technet.microsoft.com/library/hh831652)
- [Migrer les Services Bureau à distance vers Windows Server 2012 R2](https://technet.microsoft.com/library/dn479239.aspx)
- [Migrer WSUS (Windows Server Update Services) vers Windows Server 2012 R2](https://technet.microsoft.com/library/hh852339.aspx)
- [Migrer des rôles de cluster vers Windows Server 2012 R2](https://technet.microsoft.com/library/dn530779.aspx)
- [Migrer le serveur DHCP vers Windows Server 2012 R2](https://technet.microsoft.com/library/dn495425.aspx)

Un livre électronique contenant les guides de migration de Windows Server 2012 R2 et Windows Server 2012 est désormais disponible. Pour plus d'informations et pour télécharger le livre électronique, reportez-vous à la [Galerie de livres électroniques des technologies Microsoft](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#MigrateRoles).

## <a name="windows-server-2012"></a>Windows Server 2012

Suivez les étapes décrites dans ces guides pour migrer des rôles et des fonctionnalités à partir de serveurs qui exécutent Windows Server 2003, Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012 vers Windows Server 2012. Les Outils de migration de Windows Server inclus dans Windows Server 2012 prennent en charge les migrations entre sous-réseaux.

- [Installer, utiliser et supprimer les Outils de migration de Windows Server](https://technet.microsoft.com/library/jj134202)
- [Migrer les services de rôle Services AD FS (Active Directory Federation Services) vers Windows Server 2012](https://technet.microsoft.com/library/jj647765)
- [Migrer l'autorité HRA (Health Registration Authority) vers Windows Server 2012](https://technet.microsoft.com/library/hh831513)
- [Migrer Hyper-V vers Windows Server 2012 depuis Windows Server 2008 R2](https://technet.microsoft.com/library/jj574113)
- [Migrer la configuration IP vers Windows Server 2012](https://technet.microsoft.com/library/jj574133)
- [Migrer un serveur NPS (Network Policy Server) vers Windows Server 2012](https://technet.microsoft.com/library/hh831652)
- [Migrer les services d'impression et de numérisation de document vers Windows Server 2012](https://technet.microsoft.com/library/jj134150)
- [Migrer l'accès à distance vers Windows Server 2012](https://technet.microsoft.com/library/hh831423)
- [Migrer WSUS (Windows Server Update Services) vers Windows Server 2012](https://technet.microsoft.com/library/hh852339)
- [Mettre à niveau des contrôleurs de domaine Active Directory vers Windows Server 2012](https://technet.microsoft.com/library/hh994618.aspx)
- [Migrer des services et applications en cluster vers Windows Server 2012](https://technet.microsoft.com/library/dn486790.aspx)
 

Pour obtenir des ressources supplémentaires se rapportant à la migration, consultez [Migrer des rôles et des fonctionnalités vers Windows Server 2012](https://technet.microsoft.com/library/jj134039).

## <a name="windows-server-2008-r2"></a>Windows Server 2008 R2

Suivez les étapes décrites dans ces guides pour migrer des rôles et des fonctionnalités à partir des serveurs qui exécutent Windows Server 2003, Windows Server 2008 ou Windows Server 2008 R2 vers Windows Server 2008 R2. Les Outils de migration de Windows Server inclus dans Windows Server 2008 R2 ne prennent pas en charge les migrations entre sous-réseaux.

- [Installation, accès et suppression des Outils de migration de Windows Server](https://technet.microsoft.com/library/dd379545)
- [Guide de migration des services de certificats Active Directory](https://technet.microsoft.com/library/ee126170)
- [Guide de migration des serveurs AD DS (Active Directory Domain Services) et DNS (Domain Name System)](https://technet.microsoft.com/library/dd379558)
- [Guide de migration BranchCache](https://technet.microsoft.com/library/dd548365)
- [Guide de migration du serveur DHCP (Dynamic Host Configuration Protocol)](https://technet.microsoft.com/library/dd379535)
- [Guide de migration des services de fichiers](https://technet.microsoft.com/library/dd379487)
- [Guide de migration de l'autorité HRA (Health Registration Authority)](https://technet.microsoft.com/library/ee791829)
- [Guide de migration d'Hyper-V](https://technet.microsoft.com/library/ee849855)
- [Guide de migration des configurations IP](https://technet.microsoft.com/library/dd379537)
- [Guide de migration Utilisateurs et groupes locaux](https://technet.microsoft.com/library/dd379531)
- [Guide de migration du serveur NPS (Network Policy Server)](https://technet.microsoft.com/library/ee791849)
- [Guide de migration des services d'impression](https://technet.microsoft.com/library/dd379488)
- [Guide de migration des services Bureau à distance](https://technet.microsoft.com/library/ff849223)
- [Guide de migration du RRAS](https://technet.microsoft.com/library/ee822825)
- [Tâches courantes et informations relatives à la migration de Windows Server](https://technet.microsoft.com/library/ff400258)
- [Guide de migration de Windows Server Update Services 3.0 SP2](https://technet.microsoft.com/library/ee822826)
 
Pour obtenir des ressources supplémentaires se rapportant à la migration, consultez [Migrer des rôles et des fonctionnalités vers Windows Server 2008 R2](https://technet.microsoft.com/library/dd365353).
