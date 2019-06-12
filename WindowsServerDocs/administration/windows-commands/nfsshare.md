---
title: nfsshare
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 437a2615-335a-442f-9713-d50d5f3983a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc77825d63875861839ecdb22bee5a62375aaa13
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437042"
---
# <a name="nfsshare"></a>nfsshare



Vous pouvez utiliser **nfsshare** au contrôle de partage de système de fichiers réseau (NFS).

## <a name="syntax"></a>Syntaxe

```
nfsshare <ShareName>=<Drive:Path> [-o <Option=value>...]
nfsshare {<ShareName> | <Drive>:<Path> | * } /delete
```

## <a name="description"></a>Description

Sans arguments, le **nfsshare** utilitaire de ligne de commande répertorie tous les partages de système de fichiers réseau (NFS) exportés par serveur pour NFS. Avec *nom_partage* comme seul argument, **nfsshare** répertorie les propriétés du partage NFS identifié par *nom_partage*. Lorsque *nom_partage* et <em>lecteur</em> **:** <em>chemin d’accès</em> sont fournis, **nfsshare** exporte le dossier identifié par <em>lecteur</em> **:** <em>chemin d’accès</em> comme *nom_partage*. Lorsque le **/delete** option est utilisée, le dossier spécifié n’est plus accessible aux clients NFS.

## <a name="options"></a>Options

Le **nfsshare** commande accepte les options et arguments suivants :


|             Terme              |                                                                                                                                                                                                                      Définition                                                                                                                                                                                                                       |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         -o anon = {yes          |                                                                                                                                                                                                                          no}                                                                                                                                                                                                                          |
|  o - rw [=\<hôte > [ :<Host>]...]  |                       Fournit l’accès en lecture-écriture vers le répertoire partagé par les ordinateurs hôtes ou d’un client spécifiés par les groupes *hôte*. Séparez les noms d’hôtes et de groupe par un signe deux-points ( **:** ). Si *hôte* n’est pas spécifié, tous les hôtes et les groupes de clients (à l’exception de celles spécifiées avec le **ro** option) ont accès en lecture-écriture. Si ni le **ro** ni le **rw** option est définie, tous les clients ont accès en lecture-écriture vers le répertoire partagé.                       |
|  -o ro [=\<hôte > [ :<Host>]...]  | Fournit un accès en lecture seule vers le répertoire partagé par les ordinateurs hôtes ou d’un client groupes spécifiés par *hôte*. Séparez les noms d’hôtes et de groupe par un signe deux-points ( **:** ). Si *hôte* n’est pas spécifié, tous les clients (à l’exception de celles spécifiées avec le **rw** option) ont accès en lecture seule. Si le **ro** option est définie pour un ou plusieurs clients, mais la **rw** option n’est pas définie, seuls les clients spécifiés avec le **ro** option ont accès au répertoire partagé. |
|       encodage -o = {big5       |                                                                                                                                                                                                                        euc-jp                                                                                                                                                                                                                         |
|       -o anongid=\<gid>       |                                                                                     Spécifie que les utilisateurs anonymes (non mappés) accéder à l’annuaire de partage à l’aide *gid* comme identificateur de groupe (GID). La valeur par défaut est -2. Le GID anonyme sera utilisé lors du signalement le propriétaire d’un fichier appartenant à un utilisateur non mappé, même si l’accès anonyme est désactivé.                                                                                      |
|      -o  anonuid=\<uid>       |                                                                                      Spécifie que les utilisateurs anonymes (non mappés) accéder à l’annuaire de partage à l’aide *uid* en tant que leur identificateur d’utilisateur (UID). La valeur par défaut est -2. L’UID anonyme servira lors du signalement le propriétaire d’un fichier appartenant à un utilisateur non mappé, même si l’accès anonyme est désactivé.                                                                                      |
| racine de -o [=\<hôte > [ :<Host>]...] |                                                                         Fournit l’accès racine au répertoire partagé par les ordinateurs hôtes ou d’un client spécifiés par les groupes *hôte*. Séparez les noms d’hôtes et de groupe par un signe deux-points ( **:** ). Si *hôte* n’est pas spécifié, tous les clients ont accès à la racine. Si le **racine** option n’est pas définie, aucun client ne dispose d’un accès racine au répertoire partagé.                                                                         |
|            /delete            |                                                                                                                                                       Si *nom_partage* ou <em>lecteur</em> **:** <em>chemin d’accès</em> est spécifié, supprime le partage spécifié. Si \* est spécifié, supprime tous les partages NFS.                                                                                                                                                       |

> [!NOTE]
> Pour afficher la syntaxe complète de cette commande, à l’invite de commandes, tapez :</br>> **nfsshare /?**

# #

[Services for Network File System commande référence](services-for-network-file-system-command-reference.md) Voir aussi