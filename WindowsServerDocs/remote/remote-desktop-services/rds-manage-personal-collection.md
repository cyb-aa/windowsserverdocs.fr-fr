---
title: Gérer une collection de sessions de bureaux personnelles dans RDS
description: Découvrez comment ajouter des programmes RemoteApp et RDSH à votre déploiement RDS.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 11/08/2016
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 7088d164ecdd7211894b004ed580eecb33d1ba60
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861062"
---
# <a name="manage-your-personal-desktop-session-collections"></a>Gérer vos collections de sessions de bureaux personnelles

Utilisez les informations suivantes pour gérer une collection de sessions de bureaux personnels dans les services Bureau à distance.

## <a name="manually-assign-a-user-to-a-personal-session-host"></a>Affectation manuelle d’un utilisateur à un hôte de session personnel
Utilisez l'applet de commande **Set-RDPersonalSessionDesktopAssignment** pour affecter manuellement un utilisateur à un serveur hôte de session personnel dans la collection. L’applet de commande prend en charge les paramètres suivants :

-CollectionName \<chaîne\>

-ConnectionBroker \<chaîne\> 

-User \<chaîne\>

-Name \<chaîne\>

- **– CollectionName** : spécifie le nom de la collection de bureaux de session personnels. Ce paramètre est obligatoire.
- **– ConnectionBroker** : spécifie le serveur du service Broker pour les connexions Bureau à distance de votre déploiement Bureau à distance. Si vous ne fournissez aucune valeur, l'applet de commande utilise le nom de domaine complet (FQDN) de l'ordinateur local.
- **–User** : spécifie le compte d'utilisateur à associer au bureau de session personnel, au format DOMAINE\Utilisateur. Ce paramètre est obligatoire.
- **–Name** : spécifie le nom du serveur hôte de session. Ce paramètre est obligatoire. L'hôte de session identifié ici doit être membre de la collection spécifiée par le paramètre **-CollectionName**.

L'applet de commande **Import-RDPersonalSessionDesktopAssignment** importe les associations entre les comptes d'utilisateur et les bureaux de session personnels à partir d'un fichier texte. L’applet de commande prend en charge les paramètres suivants :

-CollectionName \<chaîne\>

-ConnectionBroker \<chaîne\>

-Path \<chaîne>

**–Path** Spécifie le chemin d’accès et le nom de fichier d’un fichier à importer.
 
## <a name="removing-a-user-assignment-from-a-personal-session-host"></a>Suppression d'une affectation d'utilisateur à partir d'un hôte de session personnel
Utilisez l'applet de commande **Remove-RDPersonalSessionDesktopAssignment** pour supprimer l'association entre un bureau de session personnel et un utilisateur. L’applet de commande prend en charge les paramètres suivants :

-CollectionName \<chaîne\>

-ConnectionBroker \<chaîne\>

-Force

-Name \<chaîne\>

-User \<chaîne\>

**–Force** : force l’exécution de la commande sans demander la confirmation de l’utilisateur.

## <a name="query-user-assignments"></a>Interroger les affectations d'utilisateur
Utilisez l'applet de commande **Get-RDPersonalSessionDesktopAssignment** pour obtenir la liste des bureaux de session personnels et des comptes d'utilisateurs associés. L’applet de commande prend en charge les paramètres suivants :

-CollectionName \<chaîne\>

-ConnectionBroker \<chaîne\>

-User \<chaîne\>

-Name \<chaîne\>

Vous pouvez exécuter l'applet de commande pour lancer une interrogation par nom de collection, par nom d'utilisateur ou par nom de bureau de session. Si vous spécifiez uniquement le paramètre **–CollectionName**, l'applet de commande renvoie la liste des hôtes de session et des utilisateurs associés. Si vous spécifiez également le paramètre **–User**, l'hôte de session associé à cet utilisateur est renvoyé. Si vous fournissez le paramètre **–Name**, l'utilisateur associé à cet hôte de session est renvoyé. 


L'applet de commande **Export-RDPersonalPersonalDesktopAssignment** exporte les associations actuelles entre les utilisateurs et les bureaux virtuels personnels dans un fichier texte. L’applet de commande prend en charge les paramètres suivants :

-CollectionName \<chaîne\>

-ConnectionBroker \<chaîne\>

-Path \<chaîne\>


Les nouvelles applets de commande prennent toutes en charge les paramètres courants : -Verbose, -Debug, -ErrorAction, -ErrorVariable, -OutBuffer et -OutVariable. Pour plus d’informations, consultez [about_CommonParameters](https://go.microsoft.com/fwlink/p/?LinkID=113216).
