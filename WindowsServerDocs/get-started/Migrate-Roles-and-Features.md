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
ms.sourcegitcommit: 07ac08dea2b8f2763c2614a999dc7967018aa0b4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/07/2018
ms.locfileid: "6121358"
---
# Migration de rôles et de fonctionnalités dans WindowsServer

>S’applique à: WindowsServer2016, WindowsServer2012R2, WindowsServer2012

Cette page contient des liens vers des informations et des outils qui vous guident tout au long du processus de migration des rôles et des fonctionnalités vers WindowsServer2016, WindowsServer2012R2 et WindowsServer2012. Beaucoup de rôles et de fonctionnalités peuvent être migrés à l’aide des Outils de migration de WindowsServer, un ensemble de cinq applets de commande WindowsPowerShell qui a été introduit dans WindowsServer2008R2 pour faciliter la migration des éléments et des données des rôles et des fonctionnalités.

Les guides de migration prennent en charge les migrations de fonctionnalités et de rôles spécifiés d’un serveur à un autre (pas les mises à niveau sur place). Sauf indication contraire dans les guides, les migrations sont prises en charge entre des ordinateurs physiques et virtuels et entre les options d’installation complète de WindowsServer et les serveurs qui exécutent l’option d’installation minimale.  

## Avant de commencer

Avant de commencer la migration des rôles et des fonctionnalités, vérifiez que les serveurs source et cible exécutent les derniers service packs disponibles pour leurs systèmes d’exploitation.
Un livre électronique contenant les guides de migration de WindowsServer2012R2 et WindowsServer2012 est désormais disponible. Pour plus d’informations et pour télécharger le livre électronique, reportez-vous à la [Galerie de livres électroniques des technologies Microsoft](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#MigrateRoles). 

>[!NOTE]
>Chaque fois que vous migrez ou effectuez une mise à niveau vers une version de WindowsServer, vous devez passer en revue et comprendre la [politique de support](https://support.microsoft.com/lifecycle) et le cycle de vie de cette version et planifier la procédure en conséquence. Vous pouvez [rechercher les informations de cycle de vie](https://support.microsoft.com/lifecycle) pour la version de WindowsServer qui vous intéresse.
 
## WindowsServer2016

### Guides de migration
Des guides de migration mis à jour pour WindowsServer2016 sont en cours de développement. Vérifiez la disponibilité de nouvelles mises à jour à cet emplacement ultérieurement. Dans de nombreux cas, les étapes décrites dans les guides de migration de WindowsServer2012R2 sont encore pertinentes pour WindowsServer2016.

- [Services Bureau à distance](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/migrate-rds-role-services)
- [Serveur Web (IIS)](https://www.iis.net/downloads/microsoft/web-deploy)
- [WindowsServerUpdateServices](https://technet.microsoft.com/library/hh852339.aspx)
- [MultiPointServices](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/multipoint-services/multipoint-services-migrate)
 
## WindowsServer2012R2

### Guides de migration
Suivez les étapes décrites dans ces guides pour migrer des rôles et des fonctionnalités de serveurs qui exécutent WindowsServer2003, WindowsServer2008, WindowsServer2008R2, WindowsServer2012 ou WindowsServer2012R2 vers WindowsServer2012R2. Les Outils de migration de WindowsServer inclus dans WindowsServer2012R2 prennent en charge les migrations entre sous-réseaux.

- [Installer, utiliser et supprimer les Outils de migration de WindowsServer](https://technet.microsoft.com/library/jj134202.aspx)
- [Guide de migration des services de certificats ActiveDirectory pour WindowsServer2012R2](https://technet.microsoft.com/library/dn486797.aspx)
- [Migration du service de rôle Services ADFS (Active Directory Federation Services) vers WindowsServer2012R2](https://technet.microsoft.com/library/dn486815.aspx)
- [Guide de mise à niveau et de migration des services ActiveDirectoryRightsManagement](https://technet.microsoft.com/library/cc754277.aspx)
- [Migrer le rôle Services de fichiers et de stockage vers WindowsServer2012R2](https://technet.microsoft.com/library/dn479292.aspx)
- [Migrer Hyper-V vers WindowsServer2012R2 depuis WindowsServer2012](https://technet.microsoft.com/library/dn486799.aspx)
- [Migrer un serveur NPS (Network Policy Server) vers WindowsServer2012](https://technet.microsoft.com/library/hh831652)
- [Migrer les Services Bureau à distance vers WindowsServer2012R2](https://technet.microsoft.com/library/dn479239.aspx)
- [Migrer WSUS (WindowsServerUpdateServices) vers WindowsServer2012R2](https://technet.microsoft.com/library/hh852339.aspx)
- [Migrer des rôles de cluster vers WindowsServer2012R2](https://technet.microsoft.com/library/dn530779.aspx)
- [Migrer le serveur DHCP vers WindowsServer2012R2](https://technet.microsoft.com/library/dn495425.aspx)
 
## WindowsServer2012

### Guides de migration
Suivez les étapes décrites dans ces guides pour migrer des rôles et des fonctionnalités à partir de serveurs qui exécutent WindowsServer2003, WindowsServer2008, WindowsServer2008R2 ou WindowsServer2012 vers WindowsServer2012. Les Outils de migration de WindowsServer inclus dans WindowsServer2012 prennent en charge les migrations entre sous-réseaux.

- [Installer, utiliser et supprimer les Outils de migration de WindowsServer](https://technet.microsoft.com/library/jj134202)
- [Migrer les services de rôle des services ADFS (ActiveDirectoryFederationServices) vers WindowsServer2012](https://technet.microsoft.com/library/jj647765)
- [Migrer l’autorité HRA (Health Registration Authority) vers WindowsServer2012](https://technet.microsoft.com/library/hh831513)
- [Migrer Hyper-V vers WindowsServer2012 depuis WindowsServer2008R2](https://technet.microsoft.com/library/jj574113)
- [Migrer la configurationIP vers WindowsServer2012](https://technet.microsoft.com/library/jj574133)
- [Migrer un serveur NPS (Network Policy Server) vers WindowsServer2012](https://technet.microsoft.com/library/hh831652)
- [Migrer les services d’impression et de numérisation de document vers WindowsServer2012](https://technet.microsoft.com/library/jj134150)
- [Migrer l’accès à distance vers WindowsServer2012](https://technet.microsoft.com/library/hh831423)
- [Migrer WSUS (WindowsServerUpdateServices) vers WindowsServer2012](https://technet.microsoft.com/library/hh852339)
- [Mettre à niveau des contrôleurs de domaine ActiveDirectory vers WindowsServer2012](https://technet.microsoft.com/library/hh994618.aspx)
- [Migration des services et applications en cluster vers WindowsServer2012](https://technet.microsoft.com/library/dn486790.aspx)
 

Pour obtenir des ressources supplémentaires se rapportant à la migration, consultez [Migrer des rôles et des fonctionnalités vers WindowsServer2012](https://technet.microsoft.com/library/jj134039).

## WindowsServer2008R2

### Guides de migration
Suivez les étapes décrites dans ces guides pour migrer des rôles et des fonctionnalités à partir des serveurs qui exécutent WindowsServer2003, WindowsServer2008 ou WindowsServer2008R2 vers WindowsServer2008R2. Les Outils de migration de WindowsServer inclus dans WindowsServer2008R2 ne prennent pas en charge les migrations entre sous-réseaux.

- [Installation, accès et suppression des Outils de migration de WindowsServer](https://technet.microsoft.com/library/dd379545)
- [Guide de migration des services de certificats Active Directory](https://technet.microsoft.com/library/ee126170)
- [Guide de migration des serveurs ADDS (Services de domaine Active Directory) et DNS (Domain Name System)](https://technet.microsoft.com/library/dd379558)
- [Guide de migration BranchCache](https://technet.microsoft.com/library/dd548365)
- [Guide de migration du serveur DHCP (Dynamic Host Configuration Protocol)](https://technet.microsoft.com/library/dd379535)
- [Guide de migration des services de fichiers](https://technet.microsoft.com/library/dd379487)
- [Guide de migration du HRA](https://technet.microsoft.com/library/ee791829)
- [Guide de migration d’Hyper-V](https://technet.microsoft.com/library/ee849855)
- [Guide de migration des configurationsIP](https://technet.microsoft.com/library/dd379537)
- [Guide de migration Utilisateurs et groupes locaux](https://technet.microsoft.com/library/dd379531)
- [Guide de migration du serveur NPS (Network Policy Server)](https://technet.microsoft.com/library/ee791849)
- [Guide de migration des services d’impression](https://technet.microsoft.com/library/dd379488)
- [Guide de migration des services Bureau à distance](https://technet.microsoft.com/library/ff849223)
- [Guide de migration du RRAS](https://technet.microsoft.com/library/ee822825)
- [Tâches courantes et informations relatives à la migration de WindowsServer](https://technet.microsoft.com/library/ff400258)
- [Guide de migration de WindowsServerUpdate Services3.0 SP2](https://technet.microsoft.com/library/ee822826)
 
Pour obtenir des ressources supplémentaires se rapportant à la migration, consultez [Migrer des rôles et des fonctionnalités vers WindowsServer2008R2](https://technet.microsoft.com/library/dd365353).
