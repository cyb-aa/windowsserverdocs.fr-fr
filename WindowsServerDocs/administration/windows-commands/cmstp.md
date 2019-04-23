---
title: cmstp
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 34aad544-11c3-4e85-8bbf-5bc5a971da93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ee5c5189b4eab21994def221dd757b0061ef22d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836380"
---
# <a name="cmstp"></a>cmstp

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Installe ou supprime un profil de service Gestionnaire de connexions. Utilisé sans paramètres facultatifs, **cmstp** installe un profil de service avec les paramètres par défaut appropriées pour le système d’exploitation et les autorisations de l’utilisateur. 
## <a name="syntax"></a>Syntaxe
Syntaxe 1 :
```
ServiceProfileFileName .exe /q:a /c:"cmstp.exe ServiceProfileFileName .inf [/nf] [/ni] [/ns] [/s] [/su] [/u]"
```
Syntaxe 2 :
```
cmstp.exe [/nf] [/ni] [/ns] [/s] [/su] [/u]  [Drive:][path]ServiceProfileFileName.inf"
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|< ServiceProfileFileName >.exe|Spécifie le nom, le package d’installation qui contient le profil que vous souhaitez installer.<br /><br />Requis pour la syntaxe 1 mais non valide pour la syntaxe 2.|
|/q:a|Spécifie que le profil doit être installé sans solliciter l’utilisateur. Continuera d’apparaître le message de vérification que l’installation a réussi.<br /><br />Requis pour la syntaxe 1 mais non valide pour la syntaxe 2.|
|[Lecteur :] [chemin] <ServiceProfileFileName>.inf|Obligatoire. Spécifie le nom, le fichier de configuration qui détermine comment le profil doit être installé.<br /><br />Le [lecteur :] paramètre [path] n’est pas valide pour la syntaxe 1.|
|/nf|Spécifie que les fichiers de support ne doivent pas être installés.|
|/ni|Spécifie qu’une icône de bureau ne doit pas être créée. Ce paramètre est uniquement valide pour les ordinateurs exécutant Windows 95, Windows 98, Windows NT 4.0 ou Windows Millennium edition.|
|/ns|Spécifie qu’un raccourci bureau ne doit pas être créé. Ce paramètre est uniquement valide pour les ordinateurs exécutant un membre de la famille Windows Server 2003, Windows 2000 ou le Windows XP.|
|/s|Spécifie que le profil de service doit être installé ou désinstallé en mode silencieux (sans demander de réponse de l’utilisateur ou afficher le message de vérification).|
|/su|Spécifie que le profil de service doit être installé pour un seul utilisateur plutôt que pour tous les utilisateurs. Ce paramètre est uniquement valide pour les ordinateurs exécutant un Windows Server 2003, Windows 2000 ou Windows XP.|
|/au|Indique que le profil de service doit être installé pour tous les utilisateurs. Ce paramètre est uniquement valide pour les ordinateurs exécutant Windows Server 2003, Windows 2000 ou Windows XP.|
|/u|Spécifie que le profil de service doit être désinstallé.|
|/?|Affiche l'aide à l'invite de commandes.|
## <a name="remarks"></a>Notes
**/s** est le seul paramètre que vous pouvez utiliser en combinaison avec **/u**.
La syntaxe 1 est la syntaxe généralement utilisée dans une application d’installation personnalisée. Pour utiliser cette syntaxe, vous devez exécuter **cmstp** à partir du répertoire qui contient le <ServiceProfileFileName>fichier .exe.
## <a name="BKMK_Examples"></a>Exemples
Pour installer le profil de service Fiction sans les fichiers de support, tapez :
```
fiction.exe /c:"cmstp.exe fiction.inf /nf"
```
Pour installer silencieusement le profil de service Fiction pour un seul utilisateur, tapez :
```
fiction.exe /c:"cmstp.exe fiction.inf /s /su"
```
Pour désinstaller sans assistance le profil de service Fiction, tapez :
```
fiction.exe /c:"cmstp.exe fiction.inf /s /u"
```
## <a name="additional-references"></a>Références supplémentaires
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
