---
title: tsprof
description: Rubrique de référence pour TSPROF, qui copie les informations de configuration utilisateur Services Bureau à distance d’un utilisateur vers un autre.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 27047868-b706-4208-b7e0-1437a2325dd3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b5a4980455eb2901db949a06f0c6dfec9ecf5793
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721228"
---
# <a name="tsprof"></a>tsprof

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copie les informations de configuration d’utilisateur Services Bureau à distance d’un utilisateur vers un autre.
Les informations de configuration de l’utilisateur Services Bureau à distance s’affichent dans la Services Bureau à distance extensions pour utilisateurs et groupes locaux et utilisateurs et ordinateurs Active Directory.

**TSPROF** peut également définir le chemin du profil d’un utilisateur.



> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez les nouveautés [de services Bureau à distance dans Windows server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.

## <a name="syntax"></a>Syntaxe
```
tsprof /update {/domain:<DomainName> | /local} /profile:<path> <UserName>
tsprof /copy {/domain:<DomainName> | /local} [/profile:<path>] <Src_usr> <Dest_usr>
tsprof /q {/domain:<DomainName> | /local} <UserName>
```

### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/update|Met à jour les informations de chemin de profil pour <*nom d’utilisateur*> dans le domaine <*nom_domaine*> à <*profilePath*>.|
|/Domain :\<NomDomaine>|Spécifie le nom du domaine dans lequel l’opération est appliquée.|
|/local|Applique l’opération uniquement aux comptes d’utilisateurs locaux.|
|/Profile :\<chemin d’accès>|Spécifie le chemin d’accès au profil tel qu’il s’affiche dans les Services Bureau à distance extensions dans utilisateurs et groupes locaux et utilisateurs et ordinateurs Active Directory.|
|\<Nom d’utilisateur>|Spécifie le nom de l’utilisateur pour lequel vous souhaitez mettre à jour ou interroger le chemin d’accès du profil de serveur.|
|/Copy|Copie les informations de configuration \<utilisateur de *SOURCEUSER*> vers \< *DestinationUser*> et met à jour les informations de \<chemin d’accès du profil pour *DestinationUser*> à \< *profilePath*>. \< *SOURCEUSER*> et \< *DestinationUser*> doivent être locaux ou se trouver dans le domaine \< *nom_domaine*>.|
|\<Src_usr>|Spécifie le nom de l’utilisateur à partir duquel vous souhaitez copier les informations de configuration de l’utilisateur.|
|\<Dest_usr>|Spécifie le nom de l’utilisateur pour lequel vous souhaitez copier les informations de configuration de l’utilisateur.|
|/q|Affiche le chemin d’accès du profil actuel de l’utilisateur pour lequel vous souhaitez interroger le chemin d’accès du profil de serveur.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 
-   La commande **TSPROF** n’est disponible que si vous avez installé le service de rôle Terminal Server sur un ordinateur exécutant le service de rôle hôte de session Bureau à distance windows Server 2008 ou sur un ordinateur exécutant windows Server 2008 R2.

## <a name="examples"></a>Exemples
-   Pour copier les informations de configuration de l’utilisateur de LocalUser1 vers LocalUser2, tapez :
    ```
    tsprof /copy /local LocalUser1 LocalUser2
    ```
-   Pour définir le chemin du profil de Services Bureau à distance pour LocalUser1 sur un répertoire appelé c:\Profiles, tapez :
    ```
    tsprof /update /local /profile:c:\profiles LocalUser1
    ```

## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[services Bureau à distance la référence de commande (services Terminal Server)](remote-desktop-services-terminal-services-command-reference.md)
