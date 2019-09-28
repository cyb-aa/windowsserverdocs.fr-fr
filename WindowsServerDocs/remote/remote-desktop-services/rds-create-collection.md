---
title: Créer une collection de services Bureau à distance
description: Découvrez comment ajouter des programmes RemoteApp et RDSH à votre déploiement RDS.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 11/08/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ae9767e3-864a-4eb2-96c0-626759ce6d60
author: lizap
manager: dongill
ms.openlocfilehash: cec1ae43b51b17da5617f95e13b839d4ecfbb40c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404010"
---
# <a name="create-a-remote-desktop-services-collection-for-desktops-and-apps-to-run"></a>Créer une collection de services Bureau à distance pour les bureaux et les applications à exécuter

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Suivez les étapes suivantes pour créer une collection de sessions des services Bureau à distance. Une collection de sessions conserve les applications et les postes de travail que vous souhaitez rendre disponibles aux utilisateurs. Une fois la collection créée, publiez-la afin que les utilisateurs puissent y accéder.

Avant de créer une collection, vous devez choisir le type de collection dont vous avez besoin : sessions de bureaux regroupées ou sessions de bureaux personnelles. 

- **Utiliser des sessions de bureaux regroupées pour une virtualisation basée sur les sessions** : tirez parti de la puissance de calcul de Windows Server pour fournir un environnement multisession rentable permettant de gérer les charges de travail quotidiennes de vos utilisateurs.
- **Utiliser des sessions de bureaux personnelles pour créer une infrastructure Bureau virtuel (VDI)** : Tirez parti du client Windows pour fournir les hautes performances, la compatibilité des applications et le caractère familier que vos utilisateurs attendent de leur expérience utilisateur Windows.
 
Avec une session regroupée, plusieurs utilisateurs accèdent à un pool partagé de ressources, alors qu’une session de bureaux personnelle affecte les utilisateurs à leur propre bureau au sein du pool. La session regroupée dispose d’un coût inférieur, tandis que les sessions personnelles permettent aux utilisateurs de personnaliser leur expérience.

Si vous avez besoin de partager des applications hébergées et utilisant beaucoup de ressources graphiques, vous pouvez combiner des bureaux en session personnelle avec le processeur graphique RemoteFX configuré pour des accélérations de graphiques. Sinon, vous pouvez combiner des bureaux en session personnelle avec la nouvelle fonctionnalité d’affectation de périphérique en mode discret (DDA) pour prendre en charge les applications hébergées nécessitant des accélérations graphiques. Consultez l’article [Quelle technologie de virtualisation graphique vous convient](rds-graphics-virtualization.md) pour plus d’informations.


Quel que soit le type de collection que vous choisissez, vous devez remplir ces collections avec RemoteApps (les programmes et les ressources auxquels les utilisateurs peuvent accéder depuis n’importe quel appareil pris en charge et avec lesquels ils peuvent travailler, comme si le programme était exécuté localement).

## <a name="create-a-pooled-desktop-session-collection"></a>Créer une collection de sessions de bureaux regroupées

1.  Dans le gestionnaire de serveur, cliquez sur **Services Bureau à distance > Collections > Tâches > Créer des collections de sessions**.  
2.  Entrez un nom pour la collection, par exemple **ContosoAps**.  
3.  Sélectionnez le serveur hôte de la session Bureau à distance que vous avez créé (par exemple, Contoso-Shr1).  
4.  Acceptez la valeur par défaut pour **Groupes d’utilisateurs**.  
5.  Entrez l’emplacement du partage de fichiers que vous avez créé pour les disques de profil utilisateur pour cette collection (par exemple, **\Contoso-Cb1\UserDisksr**).   
6.  Cliquez sur **Create (Créer)** . Une fois la collection créée, cliquez sur **Fermer**.  


## <a name="create-a-personal-desktop-session-collection"></a>Créer une collection de sessions de bureaux personnelles

Utilisez l’applet de commande New-RDSessionCollection pour créer une collection de sessions de bureaux personnelles. Les trois paramètres suivants fournissent les informations de configuration exigées pour les sessions de bureaux personnelles :

- **-PersonalUnmanaged** : spécifie le type de collection de sessions qui vous permet d’attribuer des utilisateurs à un serveur hôte de session personnelle. Si vous ne spécifiez pas ce paramètre, la collection est créée comme une collection traditionnelle d'hôtes de session Bureau à distance, où les utilisateurs sont affectés à l'hôte de session disponible suivant lorsqu'ils se connectent.
- **-GrantAdministrativePrivilege** : si vous utilisez **-PersonalUnmanaged**, spécifie que l’utilisateur attribué à l’hôte de session dispose des privilèges d’administrateur. Si vous n'utilisez pas ce paramètre, les utilisateurs bénéficient seulement de privilèges d'utilisateur standard.
- **-AutoAssignUser** : si vous utilisez **-PersonalUnmanaged**, spécifie que les nouveaux utilisateurs qui se connectent via le service Broker pour les connexions Bureau à distance sont automatiquement affectés à un hôte de session non attribué. En l'absence d'hôte de session non attribué dans la collection, l'utilisateur reçoit un message d'erreur. Si vous n’utilisez pas ce paramètre, vous devez [affecter manuellement les utilisateurs à un hôte de session](rds-manage-personal-collection.md#manually-assign-a-user-to-a-personal-session-host) avant qu’ils ne se connectent.

Vous pouvez utiliser des applets de commande PowerShell pour gérer vos collections de sessions de bureaux personnelles. Consultez [Gérer vos collections de sessions de bureaux personnelles](rds-manage-personal-collection.md) pour plus d’informations.

## <a name="publish-remoteapp-programs"></a>Publier des programmes RemoteApp
Utilisez les étapes suivantes pour publier les applications et les ressources dans votre collection :

1.  Dans le gestionnaire de serveur, sélectionnez la nouvelle collection (**ContosoApps**).  
2.  Sous les programmes RemoteApp, cliquez sur **Publier des programmes RemoteApp**.  
3. Sélectionnez les programmes que vous souhaitez publier, puis cliquez sur **Publier**.  
