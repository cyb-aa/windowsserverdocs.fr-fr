---
title: cmstp
description: Rubrique de référence pour cmstp, qui installe ou supprime un profil de service de gestionnaire de connexions.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 34aad544-11c3-4e85-8bbf-5bc5a971da93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 11d2ec5b09cfd9440eb22d66578061ddfb157539
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82712084"
---
# <a name="cmstp"></a>cmstp

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Installe ou supprime un profil de service de gestionnaire de connexions. Utilisé sans paramètre facultatif, **cmstp** installe un profil de service avec les paramètres par défaut appropriés au système d’exploitation et aux autorisations de l’utilisateur.

## <a name="syntax"></a>Syntaxe

Syntaxe 1 : il s’agit de la syntaxe standard utilisée dans une application d’installation personnalisée. Pour utiliser cette syntaxe, vous devez exécuter **cmstp** à partir du répertoire qui contient `<serviceprofilefilename>.exe` le fichier.

```
<serviceprofilefilename>.exe /q:a /c:cmstp.exe <serviceprofilefilename>.inf [/nf] [/s] [/u]
```

Syntaxe 2
```
cmstp.exe [/nf] [/s] [/u] [drive:][path]serviceprofilefilename.inf
```

#### <a name="parameters"></a>Paramètres
| Paramètre | Description |
| --------- | ----------- |
| `<serviceprofilefilename>.exe` | Spécifie, par son nom, le package d’installation qui contient le profil que vous souhaitez installer.<p>Obligatoire pour la syntaxe 1, mais n’est pas valide pour la syntaxe 2. |
| /q : a | Spécifie que le profil doit être installé sans demander confirmation à l’utilisateur. Le message de vérification que l’installation a réussi s’affiche toujours.<p>Obligatoire pour la syntaxe 1, mais n’est pas valide pour la syntaxe 2. |
| [lecteur :] d`<serviceprofilefilename>.inf` | Obligatoire. Spécifie, par son nom, le fichier de configuration qui détermine la façon dont le profil doit être installé.<p>Le paramètre [lecteur :] [chemin] n’est pas valide pour la syntaxe 1. |
| /nf | Spécifie que les fichiers de prise en charge ne doivent pas être installés. |
| /s | Spécifie que le profil de service doit être installé ou désinstallé en mode silencieux (sans demander de réponse de l’utilisateur ou en affichant un message de vérification). Il s’agit du seul paramètre que vous pouvez utiliser en association avec **/u**.|
| /U | Spécifie que le profil de service doit être désinstallé. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="examples"></a>Exemples

Pour installer le profil de service *fiction* sans aucun fichier de prise en charge, tapez :

```
fiction.exe /c:cmstp.exe fiction.inf /nf
```

Pour installer le profil de service *fiction* en mode silencieux pour un seul utilisateur, tapez :

```
fiction.exe /c:cmstp.exe fiction.inf /s /su
```

Pour désinstaller sans assistance le profil de service *fiction* , tapez :

```
fiction.exe /c:cmstp.exe fiction.inf /s /u
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
