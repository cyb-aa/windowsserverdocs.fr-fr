---
title: tsprof
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 27047868-b706-4208-b7e0-1437a2325dd3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e17d60126125fcd4b10373133dd61ca0db030290
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836070"
---
# <a name="tsprof"></a>tsprof

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copie les informations de configuration utilisateur des Services Bureau à distance à partir d’un seul utilisateur à un autre.
Les informations de configuration utilisateur des Services Bureau à distance s’affiche dans les extensions de Services Bureau à distance et les utilisateurs locaux et groupes active directory les utilisateurs et les ordinateurs.

**tsprof** peut également définir le chemin d’accès de profil pour un utilisateur.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour savoir quelles sont les nouveautés dans la version la plus récente, consultez [les nouveautés des Services Bureau à distance dans Windows Server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.

## <a name="syntax"></a>Syntaxe
```
tsprof /update {/domain:<DomainName> | /local} /profile:<path> <UserName>
tsprof /copy {/domain:<DomainName> | /local} [/profile:<path>] <Src_usr> <Dest_usr>
tsprof /q {/domain:<DomainName> | /local} <UserName>
```

## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/update|Mises à jour du profil des informations de chemin d’accès pour <*nom d’utilisateur*> dans le domaine <*DomainName*> à <*CheminProfil*>.|
|/ Domain :\<DomainName >|Spécifie le nom du domaine dans lequel l’opération est appliquée.|
|/local|S’applique à l’opération uniquement aux comptes d’utilisateurs locaux.|
|/ profile :\<chemin d’accès >|Spécifie le chemin d’accès de profil tel qu’affiché dans les extensions de Services Bureau à distance dans utilisateurs et groupes et locaux active directory les utilisateurs et les ordinateurs.|
|\<UserName>|Spécifie le nom de l’utilisateur pour lequel vous souhaitez mettre à jour ou interroger le chemin d’accès du profil de serveur.|
|/ Copy|Copie les informations de configuration utilisateur de \< *SourceUser*> à \< *DestinationUser*> et met à jour les informations de chemin d’accès de profil pour \<  *DestinationUser*> à \< *CheminProfil*>. Les deux \< *SourceUser*> et \< *DestinationUser*> doit être local ou doit être dans le domaine \< *DomainName*> .|
|\<Src_usr>|Spécifie le nom de l’utilisateur à partir de laquelle vous souhaitez copier les informations de configuration utilisateur.|
|\<Dest_usr>|Spécifie le nom de l’utilisateur auquel vous souhaitez copier les informations de configuration utilisateur.|
|/q|Affiche le chemin du profil actuel de l’utilisateur pour lequel vous souhaitez interroger le chemin d’accès du profil de serveur.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
-   Le **tsprof** commande est disponible uniquement lorsque vous avez installé le service de rôle Terminal Server sur un ordinateur exécutant le service de rôle hôte de Session Bureau à distance ou de Windows Server 2008 sur un ordinateur exécutant Windows Server 2008 R2.

## <a name="BKMK_examples"></a>Exemples
-   Pour copier les informations de configuration utilisateur d’Utilisateurlocal1 dans Utilisateurlocal2, tapez :
    ```
    tsprof /copy /local LocalUser1 LocalUser2
    ```
-   Pour définir le chemin du profil des Services Bureau à distance pour Utilisateurlocal1 sur un répertoire nommé « c:\profiles », tapez :
    ```
    tsprof /update /local /profile:c:\profiles LocalUser1
    ```

#### <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
[des Services Bureau à distance &#40;Services Terminal Server&#41; référence de la commande](remote-desktop-services-terminal-services-command-reference.md)
