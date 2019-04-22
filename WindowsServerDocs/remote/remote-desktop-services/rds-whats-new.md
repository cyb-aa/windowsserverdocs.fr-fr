---
title: Quelles sont les nouveautés dans les Services Bureau à distance
description: Fournit la description des nouvelles fonctionnalités services Bureau à distance dans Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ad13fdce251c1f84bac725e9f1ee266c6aae5e13
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816310"
---
# <a name="whats-new-in-remote-desktop-services"></a>Quelles sont les nouveautés dans les Services Bureau à distance

Services de bureau à distance (RDS) basé sur Windows Server 2016 est une plateforme de virtualisation l’activation d’un large éventail de scénarios de client. Améliorations dans la solution de services Bureau à distance globale incorpore le travail effectué par l’équipe de bureau à distance et d’autres partenaires de la technologie chez Microsoft. Les scénarios et les technologies suivantes sont nouvelles ou améliorées dans Windows Server 2016.

Veillez également à consulter notre session de l’Ignite 2016 : [Exploitez les améliorations des services Bureau à distance dans Windows Server 2016](https://channel9.msdn.com/Events/Ignite/2016/BRK3098). Dans cette vidéo, l’équipe de produit examine toutes les fonctionnalités nouvelles et améliorées dans les Services Bureau à distance, y compris la prise en charge du processeur graphique. 

## <a name="app-compatibility---windows-server-2016-and-windows-10"></a>Compatibilité des applications - Windows Server 2016 et Windows 10
Basé sur la même base de Windows 10, Windows Server 2016 a non seulement la même apparence et la convivialité attendent d’un ordinateur de bureau, mais peut également exécuter un grand nombre des mêmes applications. Jumelage de Windows Server 2016 avec les possibilités de graphiques (voir ci-dessous) vous offre un environnement pour tous les utilisateurs d’être productifs. 

## <a name="azure-sql-database---the-new-database-for-your-highly-available-environment"></a>Azure SQL Database - nouvelle base de données pour votre environnement hautement disponible
Le service Broker pour les connexions Bureau à distance est en mesure de stocker toutes les informations de déploiement (par exemple, les États de connexion et les mappages d’hôte/utilisateur) dans une base de données SQL partagé, par exemple une base de données SQL Azure. Abandonner le manuel de déploiement de SQL Server groupe de disponibilité AlwaysOn, saisissez la chaîne de connexion à la base de données SQL Azure et commencer à utiliser votre environnement hautement disponible.

Informations complémentaires : [Utiliser la base de données SQL Azure pour votre environnement de haute disponibilité du service Broker pour les connexions Bureau à distance](https://blogs.technet.microsoft.com/enterprisemobility/2016/05/03/new-windows-server-2016-capability-use-azure-sql-db-for-your-remote-desktop-connection-broker-high-availability-environment/)

## <a name="graphics---solving-graphics-needs-across-various-scenarios"></a>Graphics - résolution de besoins de graphiques entre les différents scénarios
Grâce à affectation discrète d’appareils de Hyper-V, vous pouvez désormais mapper GPU sur un ordinateur hôte directement à une machine virtuelle à être consommés par ses applications exigeant de GPU. Améliorations ont également été apportées dans vGPU RemoteFX, y compris la prise en charge de OpenGL 4.4, OpenCL 1.1, 4 k résolution et de machines virtuelles Windows Server.

Informations complémentaires : [Attribution discrète d’appareils](https://blogs.technet.microsoft.com/virtualization/2015/11/)

## <a name="rd-connection-broker---improved-connection-handling-during-logon-storms"></a>Broker pour les connexions Bureau à distance - gestion lors de vagues d’ouverture de session de connexion améliorée
Avec gestion des connexions améliorée, le Broker pour les connexions Bureau à distance est désormais en mesure de gérer plus de 10 000 demandes d’ouverture de session simultanées, vu parfois pendant « vagues d’ouverture de session ». Le Broker pour les connexions Bureau à distance améliorée permet également de maintenance du déploiement de la plus simple est en mesure de plus rapidement ajouter des serveurs dans l’environnement.

Informations complémentaires : [Amélioration des performances de service Broker pour les connexions Bureau à distance](https://blogs.technet.microsoft.com/enterprisemobility/2015/12/15/improved-remote-desktop-connection-broker-performance-with-windows-server-2016-and-windows-server-2012-r2-hotfix-kb3091411/)

## <a name="rdp-10---new-capabilities-built-into-the-protocol"></a>RDP 10 - nouvelles fonctionnalités intégrées dans le protocole
RDP 10 utilise désormais le codec h.264/AVC 444, optimisation en conséquence sur la vidéo et texte. Cette version, communication à distance du stylet est également pris en charge. Ces fonctionnalités, de démarrer vos sessions à distance se sentir encore plus comme une session locale.  

Informations complémentaires : [Améliorations de RDP 10 AVC/H.264 dans Windows 10 et Windows Server 2016](https://blogs.technet.microsoft.com/enterprisemobility/2016/01/11/remote-desktop-protocol-rdp-10-avch-264-improvements-in-windows-10-and-windows-server-2016-technical-preview/)

## <a name="personal-session-desktops---providing-individual-desktops-to-any-end-user"></a>Session personnelle postes de travail différents postes de travail à n’importe quel utilisateur final
Les postes de travail personnel de session est une nouvelle façon d’avoir votre propre bureau personnel hébergé dans le cloud. Des privilèges d’administrateur et supprime des hôtes de session dédié la complexité des environnements où les utilisateurs souhaitent gérer le bureau, tel que l’il d’hébergement est leur propre.

Informations complémentaires : [Bureaux de Session personnelle](rds-personal-session-desktops.md)
