---
title: Services Bureau à distance - Sécuriser le stockage des données
description: Informations de planification pour sécuriser le stockage des données, au moyen de disques de profil utilisateur (UPD) dans les services Bureau à distance.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 37b7f68e-7c3a-4190-a52f-99ae96885fae
author: lizap
ms.author: elizapo
ms.date: 11/21/2016
manager: dongill
ms.openlocfilehash: 8b7fa596f88f5cb361e0c681ffec3bcc72403d03
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403931"
---
# <a name="remote-desktop-services---secure-data-storage-with-upds"></a>Services Bureau à distance - Sécuriser le stockage des données à l’aide de disques de profil utilisateur

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Sécurisez le stockage des ressources de l’entreprise, les paramètres et les données de personnalisation des utilisateurs, localement ou dans Azure. Les hôtes de session Bureau à distance utilisent l’authentification AD et permet aux utilisateurs d’accéder de façon sécurisée aux ressources dont ils ont besoin dans un environnement personnalisé. 

La garantie que les utilisateurs bénéficient d’une expérience harmonisée, quel que soit le point de terminaison à partir duquel ils accèdent à leurs ressources distantes, représente un aspect important de la gestion d’un déploiement des services Bureau à distance. Les disques de profil utilisateur (UPD) permettent aux personnalisations, paramètres d’application et données utilisateur de suivre l’utilisateur au sein d’une collection unique. Un UDP est un fichier de disque dur virtuel (VHD) par collection, par utilisateur, qui est enregistré dans un partage centralisé, monté sur la session d’un utilisateur quand celui-ci se connecte ; l’UDP est traité comme un lecteur local pendant la durée de cette session. 

Côté utilisateur, l’UDP offre une expérience familière : l’utilisateur enregistre ses documents dans son dossier Documents (sur ce qui semble être un lecteur local), il modifie ses paramètres d’application comme d’habitude et personnalise son environnement Windows. Toutes ces données, y compris la ruche du Registre, sont stockées sur l’UDP et conservées en permanence dans un partage réseau centralisé. Les UDP ne sont disponibles pour l’utilisateur que lorsque celui-ci est connecté activement à un bureau ou à RemoteApp. Les disques de profil utilisateur peuvent être déplacés seulement au sein d’une collection, car l’intégralité du répertoire `C:\Users\<username\>` de l’utilisateur (y compris AppData\Local) est stocké sur l’UPD.

Vous pouvez utiliser des [applets de commande PowerShell](https://technet.microsoft.com/library/jj215443.aspx) pour désigner le chemin du partage centralisé, la taille de chaque UPD et les dossiers qui doivent être inclus ou exclus du profil utilisateur enregistré sur l’UPD. Vous pouvez également activer les disques de profil utilisateur via le Gestionnaire de serveur, en accédant à **Services Bureau à distance** > **Collections** > **Collection de bureaux** > **Propriétés de la collection de bureaux** > **Disques de profil utilisateur**. Notez que vous activez ou désactivez les UPD pour tous les utilisateurs d’une collection entière, pas seulement pour des utilisateurs spécifiques dans cette collection. Les UPD doivent être stockés sur un partage de fichiers centralisé, pour lequel les serveurs de la collection disposent d’autorisations de contrôle total. 

Vous pouvez obtenir une haute disponibilité pour vos UDP en les stockant dans Azure par le biais des [espaces de stockage direct](rds-storage-spaces-direct-deployment.md). 