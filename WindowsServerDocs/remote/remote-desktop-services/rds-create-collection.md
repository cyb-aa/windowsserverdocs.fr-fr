---
title: Créer une collection de Services Bureau à distance
description: Découvrez comment ajouter et programmes RemoteApp et RDSH à votre déploiement des services Bureau à distance.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 52806e28c4ef87453995728623efe2954a76dfd9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839240"
---
# <a name="create-a-remote-desktop-services-collection-for-desktops-and-apps-to-run"></a>Créer une collection de Services Bureau à distance pour les ordinateurs de bureau et des applications à exécuter

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Utilisez les étapes suivantes pour créer une collection de sessions des Services Bureau à distance. Une collection de sessions conserve les applications et les postes de travail que vous souhaitez rendre disponibles aux utilisateurs. Après avoir créé la collection, publiez-le pour que les utilisateurs puissent y accéder.

Avant de créer une collection, vous devez décider quel type de collection que vous avez besoin : regroupées sessions de bureau ou des sessions de bureau personnelles. 

- **Utiliser des sessions de bureau pool pour la virtualisation basée sur session**: Exploiter la puissance de calcul de Windows Server pour fournir un environnement de session multiples rentable pour susciter de charges de travail quotidien de vos utilisateurs
- **Utiliser des sessions de bureau personnelles pour créer une infrastructure de bureau virtuelle (VDI)**: Tirez parti de client Windows pour fournir les hautes performances, la compatibilité des applications et la connaissance de vos utilisateurs s’attendent de leur expérience de bureau Windows.
 
Avec une session regroupée, plusieurs utilisateurs accéder à un pool partagé de ressources, alors qu’avec une session de bureau personnelle, les utilisateurs sont affectés leur propre bureau à partir du pool. La session de bureaux fournit un coût inférieur, tandis que les sessions personnelles permettent aux utilisateurs de personnaliser leur expérience.

Si vous avez besoin pour les applications de partage hébergé qui sont des graphiques, vous pouvez combiner des postes de travail de session personnelle avec RemoteFX vGPU configuré pour des accélérations de graphiques. Ou bien, vous pouvez combiner des bureaux de session personnelle avec la nouvelle fonctionnalité d’attribution de périphérique discrètes (DDA) pour les applications hébergées nécessitant graphique accélérée prennent également en charge. Découvrez [quelle technologie de virtualisation de graphiques vous convient](rds-graphics-virtualization.md) pour plus d’informations.


Quel que soit le type de collection que vous choisissez, vous devez remplir ces collections avec RemoteApps - programmes et les ressources que les utilisateurs peuvent accéder à partir de n’importe quel appareil pris en charge et travailler avec comme si le programme s’exécutait localement.

## <a name="create-a-pooled-desktop-session-collection"></a>Créer une collection de sessions de bureau mis en pool

1.  Dans le Gestionnaire de serveur, cliquez sur **Services Bureau à distance > Collections > tâches > créer des Collections de sessions**.  
2.  Entrez un nom pour la collection, par exemple **ContosoAps**.  
3.  Sélectionnez le serveur hôte de Session Bureau à distance que vous avez créé (par exemple, Contoso-Shr1).  
4.  Acceptez la valeur par défaut **groupes d’utilisateurs**.  
5.  Entrez l’emplacement du partage de fichiers que vous avez créé pour les disques de profil utilisateur pour cette collection (par exemple, **\Contoso-Cb1\UserDisksr**).   
6.  Cliquez sur **Create (Créer)**. Lorsque la collection est créée, cliquez sur **fermer**.  


## <a name="create-a-personal-desktop-session-collection"></a>Créer une collection de sessions de bureau personnel

Utilisez l’applet de commande New-RDSessionCollection pour créer une collection de bureaux de session personnelle. Les trois paramètres suivants fournissent les informations de configuration requises pour les bureaux de session personnelle :

- **-PersonalUnmanaged** -Spécifie le type de collection de sessions qui vous permet d’affecter des utilisateurs à un serveur hôte de session personnelle. Si vous ne spécifiez pas ce paramètre, la collection est créée comme une collection d’hôte de Session Bureau à distance traditionnelle, où les utilisateurs sont affectés à l’hôte de session disponible suivant lors de leur connexion.
- **-GrantAdministrativePrivilege** : Si vous utilisez **- PersonalUnmanaged**, spécifie que l’utilisateur affecté à l’hôte de session disposer des privilèges d’administrateur. Si vous n’utilisez pas ce paramètre, les utilisateurs bénéficient des privilèges d’utilisateur standard uniquement.
- **-AutoAssignUser** : Si vous utilisez **- PersonalUnmanaged**, spécifie que les nouveaux utilisateurs qui se connectent via le service Broker pour les connexions Bureau à distance sont automatiquement affectés à un hôte de session non attribués. S’il n’y a aucun hôte de session non affectés dans la collection, l’utilisateur verra un message d’erreur. Si vous n’utilisez pas ce paramètre, vous devez [affecter manuellement des utilisateurs à un hôte de session](rds-manage-personal-collection.md#manually-assign-a-user-to-a-personal-session-host) se connecter.

Vous pouvez utiliser les applets de commande PowerShell pour gérer vos collections de sessions de bureau personnel. Consultez [gérer vos collections de sessions de bureau personnel](rds-manage-personal-collection.md) pour plus d’informations.

## <a name="publish-remoteapp-programs"></a>Publier des programmes RemoteApp
Pour publier les applications et les ressources dans votre collection, utilisez les étapes suivantes :

1.  Dans le Gestionnaire de serveur, sélectionnez la nouvelle collection (**ContosoApps**).  
2.  Sous programmes RemoteApp, cliquez sur **programmes RemoteApp publier**.  
3. Sélectionnez les programmes que vous souhaitez publier, puis cliquez sur **publier**.  
