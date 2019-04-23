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
ms.openlocfilehash: 50df88a3fddbceb1595f328bdd4e3c6f526e3a2c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881690"
---
# <a name="nfsshare"></a>nfsshare



Vous pouvez utiliser **nfsshare** au contrôle de partage de système de fichiers réseau (NFS).

## <a name="syntax"></a>Syntaxe

```
nfsshare <ShareName>=<Drive:Path> [-o <Option=value>...]
nfsshare {<ShareName> | <Drive>:<Path> | * } /delete
```

## <a name="description"></a>Description

Sans arguments, le **nfsshare** utilitaire de ligne de commande répertorie tous les partages de système de fichiers réseau (NFS) exportés par serveur pour NFS. Avec *nom_partage* comme seul argument, **nfsshare** répertorie les propriétés du partage NFS identifié par *nom_partage*. Lorsque *nom_partage* et *lecteur ***:*** chemin d’accès* sont fournis, **nfsshare** exporte le dossier identifié par *lecteur ***:*** Chemin d’accès* comme *nom_partage*. Lorsque le **/delete** option est utilisée, le dossier spécifié n’est plus accessible aux clients NFS.

## <a name="options"></a>Options

Le **nfsshare** commande accepte les options et arguments suivants :

|Terme|Définition|
|----|----------|
|-o anon = {yes | no}|Spécifie si les utilisateurs anonymes (non mappés) peuvent accéder au répertoire partagé. La valeur par défaut est **aucun**.|
|o - rw [=\<hôte > [ :<Host>]...]|Fournit l’accès en lecture-écriture vers le répertoire partagé par les ordinateurs hôtes ou d’un client spécifiés par les groupes *hôte*. Séparez les noms d’hôtes et de groupe par un signe deux-points (**:**). Si *hôte* n’est pas spécifié, tous les hôtes et les groupes de clients (à l’exception de celles spécifiées avec le **ro** option) ont accès en lecture-écriture. Si ni le **ro** ni le **rw** option est définie, tous les clients ont accès en lecture-écriture vers le répertoire partagé.|
|-o ro [=\<hôte > [ :<Host>]...]|Fournit un accès en lecture seule vers le répertoire partagé par les ordinateurs hôtes ou d’un client groupes spécifiés par *hôte*. Séparez les noms d’hôtes et de groupe par un signe deux-points (**:**). Si *hôte* n’est pas spécifié, tous les clients (à l’exception de celles spécifiées avec le **rw** option) ont accès en lecture seule. Si le **ro** option est définie pour un ou plusieurs clients, mais la **rw** option n’est pas définie, seuls les clients spécifiés avec le **ro** option ont accès au répertoire partagé.|
|encodage -o = {big5|euc-jp|euc-kr|euc-tw|gb2312-80|ksc5601|shift-jis}|Spécifie l’encodage par défaut utilisé pour les noms de répertoires et de fichiers et, si utilisé, doit être définie sur une des opérations suivantes :</br>-   **Big5** (chinois)</br>-   **euc-jp** (japonais)</br>-   **euc-kr** (coréen)</br>-   **euc-tw** (chinois)</br>-   **GB2312-80** (chinois simplifié)</br>-   **KSC5601** (coréen)</br>-   **shift-jis** (japonais)</br>S’il s’agit d’option n’est pas défini, le schéma d’encodage par défaut est ANSI ou sur des systèmes configurés pour les paramètres régionaux non anglais, le codage de schéma pour les paramètres régionaux par défaut. Les schémas d’encodage par défaut pour les paramètres régionaux indiqués sont les suivantes :</br>-Japonais : SHIFT-JIS</br>-Coréen : KS_C_5601-1987</br>-Chinois simplifié : GB2312-80</br>Chinois traditionnel : BIG5|
|-o anongid=\<gid>|Spécifie que les utilisateurs anonymes (non mappés) accéder à l’annuaire de partage à l’aide *gid* comme identificateur de groupe (GID). La valeur par défaut est -2. Le GID anonyme sera utilisé lors du signalement le propriétaire d’un fichier appartenant à un utilisateur non mappé, même si l’accès anonyme est désactivé.|
|-o  anonuid=\<uid>|Spécifie que les utilisateurs anonymes (non mappés) accéder à l’annuaire de partage à l’aide *uid* en tant que leur identificateur d’utilisateur (UID). La valeur par défaut est -2. L’UID anonyme servira lors du signalement le propriétaire d’un fichier appartenant à un utilisateur non mappé, même si l’accès anonyme est désactivé.|
|racine de -o [=\<hôte > [ :<Host>]...]|Fournit l’accès racine au répertoire partagé par les ordinateurs hôtes ou d’un client spécifiés par les groupes *hôte*. Séparez les noms d’hôtes et de groupe par un signe deux-points (**:**). Si *hôte* n’est pas spécifié, tous les clients ont accès à la racine. Si le **racine** option n’est pas définie, aucun client ne dispose d’un accès racine au répertoire partagé.|
|/delete|Si *nom_partage* ou *lecteur ***:*** chemin d’accès* est spécifié, supprime le partage spécifié. Si * est spécifié, supprime tous les partages NFS.|

> [!NOTE]
> Pour afficher la syntaxe complète de cette commande, à l’invite de commandes, tapez :</br>> **nfsshare /?**

##

[Services for Network File System commande référence](services-for-network-file-system-command-reference.md) Voir aussi