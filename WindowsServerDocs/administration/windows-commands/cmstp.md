---
title: cmstp
description: La rubrique commandes Windows pour cmstp, qui installe ou supprime un profil de service de gestionnaire de connexions.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 34aad544-11c3-4e85-8bbf-5bc5a971da93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8516cbce34fb8129aa623b75f5c295a7b8c28331
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847502"
---
# <a name="cmstp"></a>cmstp

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Installe ou supprime un profil de service de gestionnaire de connexions. Utilisé sans paramètre facultatif, **cmstp** installe un profil de service avec les paramètres par défaut appropriés au système d’exploitation et aux autorisations de l’utilisateur. 

## <a name="syntax"></a>Syntaxe
Syntaxe 1 :
```
ServiceProfileFileName .exe /q:a /c:cmstp.exe ServiceProfileFileName .inf [/nf] [/ni] [/ns] [/s] [/su] [/u]
```
Syntaxe 2 :
```
cmstp.exe [/nf] [/ni] [/ns] [/s] [/su] [/u]  [Drive:][path]ServiceProfileFileName.inf
```
#### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|< ServiceProfileFileName >. exe|Spécifie, par son nom, le package d’installation qui contient le profil que vous souhaitez installer.<p>Obligatoire pour la syntaxe 1, mais n’est pas valide pour la syntaxe 2.|
|/q : a|Spécifie que le profil doit être installé sans demander confirmation à l’utilisateur. Le message de vérification que l’installation a réussi s’affiche toujours.<p>Obligatoire pour la syntaxe 1, mais n’est pas valide pour la syntaxe 2.|
|[Lecteur :] [chemin] <ServiceProfileFileName>. inf|Obligatoire. Spécifie, par son nom, le fichier de configuration qui détermine la façon dont le profil doit être installé.<p>Le paramètre [lecteur :] [chemin] n’est pas valide pour la syntaxe 1.|
|/nf|Spécifie que les fichiers de prise en charge ne doivent pas être installés.|
|/ni|Spécifie qu’une icône de bureau ne doit pas être créée. Ce paramètre n’est valide que pour les ordinateurs exécutant Windows 95, Windows 98, Windows NT 4,0 ou Windows Millennium Edition.|
|/ns|Spécifie qu’un raccourci sur le Bureau ne doit pas être créé. Ce paramètre est valide uniquement pour les ordinateurs qui exécutent un membre de la famille Windows Server 2003, Windows 2000 ou Windows XP.|
|/s|Spécifie que le profil de service doit être installé ou désinstallé en mode silencieux (sans demander de réponse de l’utilisateur ou en affichant un message de vérification).|
|/su|Spécifie que le profil de service doit être installé pour un seul utilisateur plutôt que pour tous les utilisateurs. Ce paramètre n’est valide que pour les ordinateurs exécutant Windows Server 2003, Windows 2000 ou Windows XP.|
|/au|Spécifie que le profil de service doit être installé pour tous les utilisateurs. Ce paramètre n’est valide que pour les ordinateurs exécutant Windows Server 2003, Windows 2000 ou Windows XP.|
|/u|Spécifie que le profil de service doit être désinstallé.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
**/s** est le seul paramètre que vous pouvez utiliser en association avec **/u**.
La syntaxe 1 est la syntaxe classique utilisée dans une application d’installation personnalisée. Pour utiliser cette syntaxe, vous devez exécuter **cmstp** à partir du répertoire qui contient le fichier <ServiceProfileFileName>. exe.

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre
Pour installer le profil de service fiction sans aucun fichier de prise en charge, tapez :
```
fiction.exe /c:cmstp.exe fiction.inf /nf
```
Pour installer le profil de service fiction en mode silencieux pour un seul utilisateur, tapez :
```
fiction.exe /c:cmstp.exe fiction.inf /s /su
```
Pour désinstaller sans assistance le profil de service fiction, tapez :
```
fiction.exe /c:cmstp.exe fiction.inf /s /u
```
## <a name="additional-references"></a>Références supplémentaires
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
