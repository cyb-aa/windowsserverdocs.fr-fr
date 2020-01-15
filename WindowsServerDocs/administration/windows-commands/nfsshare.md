---
title: nfsshare
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d205bcfad11d22fea7fc9d0651aca61f234347cf
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75948505"
---
# <a name="nfsshare"></a>nfsshare



Vous pouvez utiliser **nfsshare** pour contrôler les partages NFS (Network File System).

## <a name="syntax"></a>Syntaxe

```
nfsshare <ShareName>=<Drive:Path> [-o <Option=value>...]
nfsshare {<ShareName> | <Drive>:<Path> | * } /delete
```

## <a name="description"></a>Description

Sans arguments, l’utilitaire de ligne de commande **nfsshare** répertorie tous les partages NFS (Network File System) exportés par le serveur pour NFS. Avec *Nom_Partage* comme seul argument, **nfsshare** répertorie les propriétés du partage NFS identifié par *Nom_Partage*. Lorsque *Nom_Partage* et <em>lecteur</em> **:** <em>chemin d’accès</em> sont fournis, **nfsshare** exporte le dossier identifié par <em>lecteur</em> **:** <em>chemin d’accès</em> en tant que *Nom_Partage*. Lorsque l’option **/Delete** est utilisée, le dossier spécifié n’est plus mis à la disposition des clients NFS.

## <a name="options"></a>Options

La commande **nfsshare** accepte les options et les arguments suivants :


|             Terme              |                                                                                                                                                                                                                      Définition                                                                                                                                                                                                                       |
|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         -o anon = {Oui          |                                                                                                                                                                                                                          º                                                                                                                                                                                                                          |
|  -o RW [=\<hôte > [ :<Host>]...]  |                       Fournit un accès en lecture-écriture au répertoire partagé par les hôtes ou les groupes de clients spécifiés par l' *hôte*. Séparez les noms d’hôte et de groupe par un signe deux-points ( **:** ). Si l' *hôte* n’est pas spécifié, tous les ordinateurs hôtes et les groupes de clients (à l’exception de ceux spécifiés avec l’option **RO** ) disposent d’un accès en lecture-écriture. Si aucune des options **RO** et **RW** n’est définie, tous les clients disposent d’un accès en lecture-écriture au répertoire partagé.                       |
|  -o ro [=\<hôte > [ :<Host>]...]  | Fournit un accès en lecture seule au répertoire partagé par les hôtes ou les groupes de clients spécifiés par l' *hôte*. Séparez les noms d’hôte et de groupe par un signe deux-points ( **:** ). Si l' *hôte* n’est pas spécifié, tous les clients (à l’exception de ceux spécifiés avec l’option **RW** ) disposent d’un accès en lecture seule. Si l’option **RO** est définie pour un ou plusieurs clients, mais que l’option **RW** n’est pas définie, seuls les clients spécifiés avec l’option **RO** ont accès au répertoire partagé. |
|       -o Encoding = {Big5       |                                                                                                                                                                                                                        EUC-JP                                                                                                                                                                                                                         |
|       -o anongid =\<GID >       |                                                                                     Spécifie que les utilisateurs anonymes (non mappés) accèdent au répertoire de partage en utilisant *GID* comme identificateur de groupe (GID). La valeur par défaut est-2. Le GID anonyme sera utilisé pour signaler le propriétaire d’un fichier appartenant à un utilisateur non mappé, même si l’accès anonyme est désactivé.                                                                                      |
|      -o anonuid =\<UID >       |                                                                                      Spécifie que les utilisateurs anonymes (non mappés) accèdent au répertoire de partage en utilisant *UID* comme identificateur d’utilisateur (UID). La valeur par défaut est-2. L’UID anonyme sera utilisé pour signaler le propriétaire d’un fichier appartenant à un utilisateur non mappé, même si l’accès anonyme est désactivé.                                                                                      |
| -o root [=\<hôte > [ :<Host>]...] |                                                                         Fournit un accès racine au répertoire partagé par les hôtes ou les groupes de clients spécifiés par l' *hôte*. Séparez les noms d’hôte et de groupe par un signe deux-points ( **:** ). Si l' *hôte* n’est pas spécifié, tous les clients ont un accès racine. Si l’option **racine** n’est pas définie, aucun client n’a accès à la racine du répertoire partagé.                                                                         |
|            /delete            |                                                                                                                                                       Si *Nom_Partage* ou <em>Drive</em> **:** <em>path</em> est spécifié, supprime le partage spécifié. Si \* est spécifié, supprime tous les partages NFS.                                                                                                                                                       |

> [!NOTE]
> Pour afficher la syntaxe complète de cette commande, à l’invite de commandes, tapez :</br>> **nfsshare/ ?**

## <a name="see-also"></a>Articles associés

[Référence des commandes des services pour NFS](services-for-network-file-system-command-reference.md)