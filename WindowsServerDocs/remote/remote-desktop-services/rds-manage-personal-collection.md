---
title: Gérer une collection de sessions de bureaux personnelles dans RDS
description: Découvrez comment ajouter des programmes RemoteApp et RDSH à votre déploiement RDS.
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
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "63743515"
---
## <a name="manage-your-personal-desktop-session-collections"></a>Gérer vos collections de sessions de bureaux personnelles

Utilisez les informations suivantes pour gérer une collection de sessions de bureaux personnels dans les services Bureau à distance.

### <a name="manually-assign-a-user-to-a-personal-session-host"></a>Affectation manuelle d’un utilisateur à un hôte de session personnel
Utilisez l’applet de commande **Set-RDPersonalSessionDesktopAssignment** pour affecter manuellement un utilisateur à un serveur hôte de session personnel dans la collection. L’applet de commande prend en charge les paramètres suivants :

-CollectionName \<chaîne\>

-ConnectionBroker \<chaîne\> 

-User \<chaîne\>

-Name \<chaîne\>

- **– CollectionName** -Spécifie le nom de la collection de bureaux de session personnelle. Ce paramètre est obligatoire.
- **– ConnectionBroker** -Spécifie le serveur service Broker pour les connexions Bureau à distance pour votre déploiement de bureau à distance. Si vous ne fournissez pas une valeur, l’applet de commande utilise le nom de domaine complet (FQDN) de l’ordinateur local.
- **–User** -Spécifie le compte d’utilisateur à associer avec le bureau de session personnel, au format DOMAINE\Utilisateur. Ce paramètre est obligatoire.
- **–Name** - Spécifie le nom du serveur hôte de session. Ce paramètre est obligatoire. L’hôte de session identifié ici doit être un membre de la collection spécifiée par le paramètre **-CollectionName**.

L’applet de commande **Import-RDPersonalSessionDesktopAssignment** importe les associations entre les comptes d’utilisateur et les bureaux de session personnels à partir d’un fichier texte. L’applet de commande prend en charge les paramètres suivants :

-CollectionName \<chaîne\>

-ConnectionBroker \<chaîne\>

-Path \<chaîne>

**–Path** Spécifie le chemin d’accès et le nom de fichier d’un fichier à importer.
 
### <a name="removing-a-user-assignment-from-a-personal-session-host"></a>Suppression d’une affectation d’utilisateur à partir d’un hôte de session personnel
Utilisez l’applet de commande **Remove-RDPersonalSessionDesktopAssignment** pour supprimer l’association entre un bureau de session personnel et un utilisateur. L’applet de commande prend en charge les paramètres suivants :

-CollectionName \<chaîne\>

-ConnectionBroker \<chaîne\>

-Force

-Name \<chaîne\>

-User \<chaîne\>

**–Force** : force l’exécution de la commande sans demander la confirmation de l’utilisateur.

### <a name="query-user-assignments"></a>Questionner les affectations d’utilisateur
Utilisez l’applet de commande **Get-RDPersonalSessionDesktopAssignment** pour obtenir une liste des bureaux de session personnels et des comptes d’utilisateurs associés. L’applet de commande prend en charge les paramètres suivants :

-CollectionName \<chaîne\>

-ConnectionBroker \<chaîne\>

-User \<chaîne\>

-Name \<chaîne\>

Vous pouvez exécuter l’applet de commande pour effectuer une requête par nom de collection, nom d’utilisateur ou par nom de bureau de session. Si vous spécifiez uniquement le paramètre **–CollectionName**, l’applet de commande renvoie une liste des hôtes de session et des utilisateurs associés. Si vous spécifiez également le paramètre **–User**, l’hôte de session associé à cet utilisateur est renvoyé. Si vous fournissez le paramètre **–Name**, l’utilisateur associé à cet hôte de session est renvoyé. 


L’applet de commande **RDPersonalPersonalDesktopAssignment d’exportation** exporte les associations actuelles entre les utilisateurs et les bureaux virtuels personnels dans un fichier texte. L’applet de commande prend en charge les paramètres suivants :

-CollectionName \<chaîne\>

-ConnectionBroker \<chaîne\>

-Path \<chaîne\>


Toutes les nouvelles applets de commande prennent en charge les paramètres courants : -Verbose, -Debug, -ErrorAction, -ErrorVariable, -OutBuffer et -OutVariable. Pour plus d'informations, consultez [about_CommonParameters](https://go.microsoft.com/fwlink/p/?LinkID=113216).
