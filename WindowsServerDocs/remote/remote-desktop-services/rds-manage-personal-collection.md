---
title: Gérer une collection de sessions de bureau personnel dans les services Bureau à distance
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
author: lizap
manager: dongill
ms.openlocfilehash: 286c7ba4bd4428669d135c35c825033d22b8f40e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865720"
---
## <a name="manage-your-personal-desktop-session-collections"></a>Gérer vos collections de sessions de bureau personnel

Utilisez les informations suivantes pour gérer une collection de sessions de bureau personnel des Services Bureau à distance.

### <a name="manually-assign-a-user-to-a-personal-session-host"></a>Affecter manuellement un utilisateur à un hôte de session personnelle
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
 
### <a name="removing-a-user-assignment-from-a-personal-session-host"></a>Suppression d’une affectation de l’utilisateur à partir d’un hôte de Session personnelle
Utilisez le **Remove-RDPersonalSessionDesktopAssignment** applet de commande pour supprimer l’association entre un ordinateur de bureau personnel de session et un utilisateur. L’applet de commande prend en charge les paramètres suivants :

-CollectionName \<string\>

-ConnectionBroker \<chaîne\>

-Force

-Name \<chaîne\>

-Utilisateur \<chaîne\>

**– Forcer** force la commande à exécuter sans demander confirmation de l’utilisateur.

### <a name="query-user-assignments"></a>Affectations d’utilisateur de requête
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
