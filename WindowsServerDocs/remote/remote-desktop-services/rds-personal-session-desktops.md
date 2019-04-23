---
title: Utiliser les bureaux personnels session Services Bureau à distance
description: Apprendre à partager personnalisés et affecté des postes de travail via RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 09/16/2016
manager: dongill
ms.openlocfilehash: 41f6a28c1b754a5277a0966a87dae08a6aa34e08
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875950"
---
# <a name="use-personal-session-desktops-with-remote-desktop-services"></a>Utiliser les bureaux personnels session Services Bureau à distance

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez déployer des bureaux personnels dans un environnement informatique en nuage basé sur le serveur à l’aide de postes de travail de session personnelle.  (Un environnement informatique en nuage a une séparation entre les serveurs d’infrastructure Hyper-V et des ordinateurs virtuels invités, tels que Microsoft Azure Cloud ou de la plateforme de Cloud Microsoft.) La fonctionnalité de bureau personnel session étend le scénario de déploiement de postes de travail basés sur des sessions dans les Services Bureau à distance pour créer un nouveau type de collection de sessions où chaque utilisateur est affecté à leur propre hôte de session personnelle avec des droits administratifs. 

Utilisez les informations suivantes pour créer et gérer une collection de bureaux de session personnelle.

## <a name="create-a-personal-session-desktop-collection"></a>Créer une collection de bureaux de session personnelle

Utilisez l’applet de commande New-RDSessionCollection pour créer une collection de bureaux de session personnelle. Les trois paramètres suivants fournissent les informations de configuration requises pour les bureaux de session personnelle :

- **-PersonalUnmanaged** -Spécifie le type de collection de sessions qui vous permet d’affecter des utilisateurs à un serveur hôte de session personnelle. Si vous ne spécifiez pas ce paramètre, la collection est créée comme une collection d’hôte de Session Bureau à distance traditionnelle, où les utilisateurs sont affectés à l’hôte de session disponible suivant lors de leur connexion.
- **-GrantAdministrativePrivilege** : Si vous utilisez **- PersonalUnmanaged**, spécifie que l’utilisateur affecté à l’hôte de session disposer des privilèges d’administrateur. Si vous n’utilisez pas ce paramètre, les utilisateurs bénéficient des privilèges d’utilisateur standard uniquement.
- **-AutoAssignUser** : Si vous utilisez **- PersonalUnmanaged**, spécifie que les nouveaux utilisateurs qui se connectent via le service Broker pour les connexions Bureau à distance sont automatiquement affectés à un hôte de session non attribués. S’il n’y a aucun hôte de session non affectés dans la collection, l’utilisateur verra un message d’erreur. Si vous n’utilisez pas ce paramètre, vous devez [affecter manuellement des utilisateurs à un hôte de session](#manually-assign-a-user-to-a-personal-session-host) se connecter.

## <a name="manually-assign-a-user-to-a-personal-session-host"></a>Affecter manuellement un utilisateur à un hôte de session personnelle
Utilisez le **Set-RDPersonalSessionDesktopAssignment** applet de commande pour affecter manuellement un utilisateur à un serveur hôte de session personnelle dans la collection. L’applet de commande prend en charge les paramètres suivants :

-CollectionName \<string\>

-ConnectionBroker \<chaîne\> 

-Utilisateur \<chaîne\>

-Name \<chaîne\>

- **– CollectionName** -Spécifie le nom de la collection de bureaux de session personnelle. Ce paramètre est obligatoire.
- **– ConnectionBroker** -Spécifie le serveur de Broker de connexion Bureau à distance (RD Connection Broker) pour votre déploiement de bureau à distance. Si vous ne fournissez pas une valeur, l’applet de commande utilise le nom de domaine complet (FQDN) de l’ordinateur local.
- **– Utilisateur** -Spécifie le compte d’utilisateur à associer avec le bureau de session personnelle, au format domaine\utilisateur. Ce paramètre est obligatoire.
- **– Nom** -Spécifie le nom du serveur hôte de session. Ce paramètre est obligatoire. L’hôte de session identifié ici doit être un membre de la collection qui le **- CollectionName** spécifie de paramètre.

Le **Import-RDPersonalSessionDesktopAssignment** applet de commande importe les associations entre les comptes d’utilisateur et les bureaux de session personnelle à partir d’un fichier texte. L’applet de commande prend en charge les paramètres suivants :

-CollectionName \<string\>

-ConnectionBroker \<chaîne\>

-Chemin d’accès \<chaîne >

**– Chemin d’accès** Spécifie le chemin d’accès et le nom d’un fichier à importer.
 
## <a name="removing-a-user-assignment-from-a-personal-session-host"></a>Suppression d’une affectation de l’utilisateur à partir d’un hôte de Session personnelle
Utilisez le **Remove-RDPersonalSessionDesktopAssignment** applet de commande pour supprimer l’association entre un ordinateur de bureau personnel de session et un utilisateur. L’applet de commande prend en charge les paramètres suivants :

-CollectionName \<string\>

-ConnectionBroker \<chaîne\>

-Force

-Name \<chaîne\>

-Utilisateur \<chaîne\>

**– Forcer** force la commande à exécuter sans demander confirmation de l’utilisateur.

## <a name="query-user-assignments"></a>Affectations d’utilisateur de requête
Utilisez le **Get-RDPersonalSessionDesktopAssignment** pour obtenir une liste des postes de travail de session personnelle et comptes d’utilisateurs associés. L’applet de commande prend en charge les paramètres suivants :

-CollectionName \<string\>

-ConnectionBroker \<chaîne\>

-Utilisateur \<chaîne\>

-Name \<chaîne\>

Vous pouvez exécuter l’applet de commande de requête par nom de collection, nom d’utilisateur, ou par nom de session Bureau. Si vous spécifiez uniquement le **– CollectionName** paramètre, l’applet de commande renvoie une liste des hôtes de session et les utilisateurs associés. Si vous spécifiez également le **– utilisateur** paramètre, l’hôte de session associé à cet utilisateur est renvoyé. Si vous fournissez la **– nom** paramètre, l’utilisateur associé à cet hôte de session est renvoyé. 


Le **RDPersonalPersonalDesktopAssignment d’exportation** applet de commande exporte les associations entre les utilisateurs et les bureaux virtuels personnels actuelles dans un fichier texte. L’applet de commande prend en charge les paramètres suivants :

-CollectionName \<string\>

-ConnectionBroker \<chaîne\>

-Chemin d’accès \<chaîne\>


Toutes les nouvelles applets de commande prennent en charge les paramètres communs :-Verbose,-Debug, - ErrorAction, - ErrorVariable,-OutBuffer et - OutVariable. Pour plus d'informations, consultez [about_CommonParameters](https://go.microsoft.com/fwlink/p/?LinkID=113216).

## <a name="hardware-accelerated-graphics"></a>Graphiques à accélération matérielle
Windows Server 2016 étend la technologie de carte (vGPU) graphiques 3D RemoteFX pour prendre en charge OpenGL et prend en charge des machines virtuelles invitées de Windows Server 2016 mode mono-utilisateur. Vous pouvez combiner des postes de travail personnel de session avec les nouvelles fonctionnalités de vGPU pour prendre en charge les applications hébergées nécessitant graphique accélérée. Ou bien, vous pouvez combiner des bureaux de session personnelle avec la nouvelle fonctionnalité d’attribution de périphérique discrètes (DDA) pour les applications hébergées nécessitant graphique accélérée prennent également en charge.
