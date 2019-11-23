---
title: tsprof
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 77d0752f74d2f6031f83f805273650747d24cfee
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392320"
---
# <a name="tsprof"></a>tsprof

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copie les informations de configuration d’utilisateur Services Bureau à distance d’un utilisateur vers un autre.
Les informations de configuration de l’utilisateur Services Bureau à distance s’affichent dans la Services Bureau à distance extensions pour utilisateurs et groupes locaux et utilisateurs et ordinateurs Active Directory.

**TSPROF** peut également définir le chemin du profil d’un utilisateur.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez les nouveautés [de services Bureau à distance dans Windows server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.

## <a name="syntax"></a>Syntaxe
```
tsprof /update {/domain:<DomainName> | /local} /profile:<path> <UserName>
tsprof /copy {/domain:<DomainName> | /local} [/profile:<path>] <Src_usr> <Dest_usr>
tsprof /q {/domain:<DomainName> | /local} <UserName>
```

## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/Update|Met à jour les informations de chemin de profil pour <*nom d’utilisateur*> dans le domaine <*nom_domaine*> à <*profilePath*>.|
|/Domain :\<DomainName >|Spécifie le nom du domaine dans lequel l’opération est appliquée.|
|/local|Applique l’opération uniquement aux comptes d’utilisateurs locaux.|
|/Profile :\<chemin d’accès >|Spécifie le chemin d’accès au profil tel qu’il s’affiche dans les Services Bureau à distance extensions dans utilisateurs et groupes locaux et utilisateurs et ordinateurs Active Directory.|
|Nom d’utilisateur \<>|Spécifie le nom de l’utilisateur pour lequel vous souhaitez mettre à jour ou interroger le chemin d’accès du profil de serveur.|
|/Copy|Copie les informations de configuration de l’utilisateur de \<*SourceUser*> vers \<*DestinationUser*> et met à jour les informations de chemin d’accès du profil pour \<*DestinationUser*> sur \<*profilePath*>. Les \<*SourceUser*> et \<*DestinationUser*> doivent être locaux ou se trouver dans le domaine \<*nom_domaine*>.|
|\<Src_usr >|Spécifie le nom de l’utilisateur à partir duquel vous souhaitez copier les informations de configuration de l’utilisateur.|
|\<Dest_usr >|Spécifie le nom de l’utilisateur pour lequel vous souhaitez copier les informations de configuration de l’utilisateur.|
|/q|Affiche le chemin d’accès du profil actuel de l’utilisateur pour lequel vous souhaitez interroger le chemin d’accès du profil de serveur.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
-   La commande **TSPROF** n’est disponible que si vous avez installé le service de rôle Terminal Server sur un ordinateur exécutant le service de rôle hôte de session Bureau à distance windows Server 2008 ou sur un ordinateur exécutant windows Server 2008 R2.

## <a name="BKMK_examples"></a>Illustre
-   Pour copier les informations de configuration de l’utilisateur de LocalUser1 vers LocalUser2, tapez :
    ```
    tsprof /copy /local LocalUser1 LocalUser2
    ```
-   Pour définir le chemin du profil de Services Bureau à distance pour LocalUser1 sur un répertoire nommé « c:\Profiles », tapez :
    ```
    tsprof /update /local /profile:c:\profiles LocalUser1
    ```

#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[ &#40;services Bureau à distance&#41; référence de commande des services Terminal Server](remote-desktop-services-terminal-services-command-reference.md)
