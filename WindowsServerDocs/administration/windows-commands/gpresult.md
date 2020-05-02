---
title: gpresult
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dfaa3adf-2c83-486c-86d6-23f93c5c883c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5ef183547b0991489a9bb95dc0a7ea938ca1847e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724955"
---
# <a name="gpresult"></a>gpresult

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche les informations du jeu de stratégie résultant (RSoP) pour un utilisateur et un ordinateur distant.
Pour utiliser les rapports RSoP pour des ordinateurs ciblés à distance via le pare-feu, vous devez disposer de règles de pare-feu qui autorisent le trafic réseau entrant sur les ports.

## <a name="syntax"></a>Syntaxe

```
gpresult [/s <system> [/u <USERNAME> [/p [<PASSWOrd>]]]] [/user [<TARGETDOMAIN>\]<TARGETUSER>] [/scope {user | computer}] {/r | /v | /z | [/x | /h] <FILENAME> [/f] | /?}
```

### <a name="parameters"></a>Paramètres

> [!NOTE]
> Excepté quand vous utilisez **/ ?**, vous devez inclure une option output, **/r**, **/v**, **/z**, **/x**ou **/h**.

|                Paramètre                 |                                                                                                     Description                                                                                                      |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              système \</s\>               |                                                  Spécifie le nom ou l’adresse IP d’un ordinateur distant. N’utilisez pas de barres obliques inverses. La valeur par défaut est l'ordinateur local.                                                   |
|             /u \<nom d’utilisateur\>              |                                Utilise les informations d’identification de l’utilisateur spécifié pour exécuter la commande. L’utilisateur par défaut est l’utilisateur qui a ouvert une session sur l’ordinateur qui émet la commande.                                 |
|            /p [\<mot\>de passe]             |            Spécifie le mot de passe du compte d’utilisateur fourni dans le paramètre **/u** . Si **/p** est omis, **gpresult** invite à entrer le mot de passe. **/p** ne peut pas être utilisé avec **/x** ou **/h**.            |
| /User [\<TargetDomain\>\\]\<TARGETUSER\> |                                                                            Spécifie l’utilisateur distant dont les données RSoP doivent être affichées.                                                                             |
|      /Scope {utilisateur &#124; ordinateur}       |                                Affiche les données RSoP pour l’utilisateur ou l’ordinateur. Si **/scope** est omis, **gpresult** affiche les données RSoP pour l’utilisateur et l’ordinateur.                                 |
|        [/x &#124;/h]<FILENAME>         | Enregistre le rapport au format XML (**/x**) ou HTML (**/h**) à l’emplacement et avec le nom de fichier spécifié par le paramètre *filename* . Ne peut pas être utilisé avec **/u**, **/p**, **/r**, **/v**ou **/z**. |
|                    /f                    |                                                           Force **gpresult** à remplacer le nom de fichier spécifié dans l’option **/x** ou **/h** .                                                           |
|                    /r                    |                                                                                             Affiche les données de synthèse RSoP.                                                                                              |
|                    /v                    |                                                    Affiche des informations détaillées sur la stratégie. Cela comprend les paramètres détaillés qui ont été appliqués avec une priorité de 1.                                                    |
|                    /z                    |                                     Affiche toutes les informations disponibles sur stratégie de groupe. Cela comprend les paramètres détaillés qui ont été appliqués avec une priorité de 1 et une valeur supérieure.                                      |
|                    /?                    |                                                                                         Affiche l'aide à l'invite de commandes.                                                                                         |

## <a name="remarks"></a>Notes 
- Stratégie de groupe est l’outil d’administration principal pour définir et contrôler la façon dont les programmes, les ressources réseau et le système d’exploitation fonctionnent pour les utilisateurs et les ordinateurs d’une organisation. Dans un environnement Active Directory, stratégie de groupe s’applique aux utilisateurs ou aux ordinateurs en fonction de leur appartenance à des sites, des domaines ou des unités d’organisation.
- Étant donné que vous pouvez appliquer des paramètres de stratégie qui se chevauchent à un ordinateur ou à un utilisateur, la fonctionnalité stratégie de groupe génère un jeu de paramètres de stratégie résultant lorsque l’utilisateur ouvre une session. **gpresult** affiche le jeu de paramètres de stratégie résultants qui ont été appliqués sur l’ordinateur pour l’utilisateur spécifié lorsque l’utilisateur s’est connecté.
- Comme **/v** et **/z** produisent un grand nombre d’informations, il est utile de rediriger la sortie vers un fichier texte (par exemple, **gpresult/z >Policy. txt**).
- La commande **gpresult** est disponible dans windows server 2012, windows Server 2008 R2, Windows Server 2008, Windows 8, Windows 7 et Windows Vista.
  ## <a name="examples"></a>Exemples
  Pour récupérer les données RSoP pour l’utilisateur distant **targetusername** de l’ordinateur **Srvmain**, et affiche les données RSoP relatives à l’utilisateur uniquement. La commande est exécutée avec les informations d’identification de **maindom\hiropln**l’utilisateur maindom\hiropln <strong>p@ssW23</strong> et est entrée en tant que mot de passe pour cet utilisateur.

  ```
  gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /scope user /r
  ```
  
Pour enregistrer toutes les informations disponibles sur stratégie de groupe pour l’utilisateur distant **targetusername** de l’ordinateur **Srvmain** dans un fichier nommé **Policy. txt**. Aucune donnée n’est incluse sur l’ordinateur. La commande est exécutée avec les informations d’identification de **maindom\hiropln**l’utilisateur maindom\hiropln <strong>p@ssW23</strong> et est entrée en tant que mot de passe pour cet utilisateur.

  ```
  gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /user targetusername /z > policy.txt
  ```
  
Pour afficher les données RSoP de l’ordinateur **Srvmain** et de l’utilisateur connecté. Les données sont incluses à la fois sur l’utilisateur et sur l’ordinateur. La commande est exécutée avec les informations d’identification de **maindom\hiropln**l’utilisateur maindom\hiropln <strong>p@ssW23</strong> et est entrée en tant que mot de passe pour cet utilisateur.

  ```
  gpresult /s srvmain /u maindom\hiropln /p p@ssW23 /r
  ```
  
## <a name="additional-references"></a>Références supplémentaires
- [Site web TechCenter de la stratégie de groupe](https://go.microsoft.com/fwlink/?LinkID=145531)

- - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
