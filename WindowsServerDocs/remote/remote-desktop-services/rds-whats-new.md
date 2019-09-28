---
title: Nouveautés des services Bureau à distance
description: Fournit la description des nouvelles fonctionnalités de Services Bureau à distance dans Windows Server 2016.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 10/11/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 04d52dff-e61b-4633-9908-be8600abc2ba
author: ChristianMontoya
manager: scottman
ms.openlocfilehash: e976f4d4ffa33efb98a744909f8c46ba498ea43b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403838"
---
# <a name="whats-new-in-remote-desktop-services"></a>Nouveautés des services Bureau à distance

Les services Bureau à distance (RDS) s’appuyant sur Windows Server 2016 constituent une plateforme de virtualisation offrant un large éventail de scénarios clients. Les améliorations apportées à la l’ensemble de la solution Services Bureau à distance incorporent le travail réalisé par l’équipe de Bureau à distance et celui d’autres partenaires chez Microsoft. Les technologies et scénarios suivants sont nouveaux ou améliorés dans Windows Server 2016.

Ainsi, pensez également à consulter notre session depuis Ignite 2016 : [Tirez profit des améliorations des services Bureau à distance dans Windows Server 2016](https://channel9.msdn.com/Events/Ignite/2016/BRK3098). Dans cette vidéo, l’équipe de produit passe en revue toutes les fonctionnalités inédites et perfectionnées dans les services Bureau à distance, y compris la prise en charge du vGPU. 

## <a name="app-compatibility---windows-server-2016-and-windows-10"></a>Compatibilité des applications : Windows Server 2016 et Windows 10
S’appuyant sur des fondations communes à Windows 10, Windows Server 2016 offre non seulement le même aspect et le même mode de fonctionnement que vous attendez d’un ordinateur de bureau, mais il est également capable d’exécuter bon nombre des mêmes applications. L’association de Windows Server 2016 à des possibilités graphiques (voir ci-dessous) vous offre un environnement dans lequel tous les utilisateurs gagnent en productivité. 

## <a name="azure-sql-database---the-new-database-for-your-highly-available-environment"></a>Azure SQL Database : la nouvelle base de données de votre environnement hautement disponible
Le service Broker pour les connexions Bureau à distance est en mesure de stocker toutes les informations de déploiement (par exemple, les états de connexion et les mappages d’hôte/utilisateur) dans une base de données SQL partagée, telle qu’une une base de données Azure SQL. Oubliez le manuel de déploiement du Groupe de disponibilité SQL Server Always On, prenez la chaîne de connexion à la base de données SQL Azure et commencez à utiliser votre environnement hautement disponible.

Informations complémentaires : [Utiliser Azure SQL Database pour l’environnement à haute disponibilité du service Broker pour les connexions Bureau à distance](https://blogs.technet.microsoft.com/enterprisemobility/2016/05/03/new-windows-server-2016-capability-use-azure-sql-db-for-your-remote-desktop-connection-broker-high-availability-environment/)

## <a name="graphics---solving-graphics-needs-across-various-scenarios"></a>Graphismes - Résolution des besoins en graphismes dans différents scénarios
Grâce à l’affectation discrète d’appareils d’Hyper-V, vous pouvez désormais mapper des processeurs graphiques (GPU), d’une machine hôte directement à une machine virtuelle, pour qu’ils soient consommés par les applications nécessitant un apport de GPU. Des améliorations ont également été apportées à RemoteFX vGPU, notamment la prise en charge d’OpenGL 4.4, OpenCL 1.1, la résolution 4K et les machines virtuelles Windows Server.

Informations complémentaires : [Affectation de périphérique en mode discret](https://blogs.technet.microsoft.com/virtualization/2015/11/)

## <a name="rd-connection-broker---improved-connection-handling-during-logon-storms"></a>Service Broker pour les connexions Bureau à distance - Avancée en gestion des connexions lors de déferlantes d’ouverture de session
Avec une gestion des connexions améliorée, le service Broker pour les connexions Bureau à distance peut maintenant gérer plus de 10 000 demandes d’ouverture de session simultanées, ce qui est observé parfois pendant les « déferlantes d’ouverture de session ». Le service Broker pour les connexions Bureau à distance perfectionné simplifie également la maintenance du déploiement en pouvant rajouter plus rapidement des serveurs dans l’environnement.

Informations complémentaires : [Performances du service Broker pour les connexions Bureau à distance augmentées](https://blogs.technet.microsoft.com/enterprisemobility/2015/12/15/improved-remote-desktop-connection-broker-performance-with-windows-server-2016-and-windows-server-2012-r2-hotfix-kb3091411/)

## <a name="rdp-10---new-capabilities-built-into-the-protocol"></a>RDP 10 - Nouvelles fonctionnalités intégrées au protocole
RDP 10 utilise désormais le codec H.264/AVC 444, optimisant avec pertinence vidéo et texte. Dans cette version, la communication à distance du stylet est également prise en charge. Avec ces fonctionnalités, vos sessions à distance commencent à ressembler singulièrement à une session locale.  

Informations complémentaires : [Améliorations de RDP 10 AVC/H.264 dans Windows 10 et Windows Server 2016](https://blogs.technet.microsoft.com/enterprisemobility/2016/01/11/remote-desktop-protocol-rdp-10-avch-264-improvements-in-windows-10-and-windows-server-2016-technical-preview/)

## <a name="personal-session-desktops---providing-individual-desktops-to-any-end-user"></a>Sessions de bureaux personnels - Fournir des bureaux individuels aux utilisateurs finaux
La fonctionnalité Session de bureaux personnels est un nouveau moyen de disposer de votre propre bureau personnel, hébergé pour vous dans le cloud. Des privilèges administratifs et des hôtes de session dédiés suppriment la complexité de l’hébergement d’environnements dans lesquels les utilisateurs veulent gérer le bureau comme s’il s’agissait de leur propre bureau.

Informations complémentaires : [Sessions de bureaux personnels](rds-personal-session-desktops.md)
