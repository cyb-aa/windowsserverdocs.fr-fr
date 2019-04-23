---
ms.assetid: a0c6dbfe-d898-496d-9356-825f7fbd90ec
title: fsutil 8dot3name
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 4e8d67de42342f5a0828df9f57ca6d7e2870586e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873190"
---
# <a name="fsutil-8dot3name"></a>fsutil 8dot3name

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Interroge ou modifie les paramètres pour le comportement de nom court (nom 8dot3), qui incluent :

-   Interroger le paramètre actuel pour le comportement de nom court

-   Analyse le chemin d’accès du répertoire spécifié pour les clés de Registre qui peuvent être impactés si les noms courts ont été supprimés du chemin d’accès du répertoire spécifié

-   Modifiez le paramètre qui contrôle le comportement de nom court. Ce paramètre peut être appliqué à un volume spécifié ou le paramètre de volume par défaut.

-   Supprimer les noms courts pour tous les fichiers contenus dans un répertoire

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
fsutil 8dot3name [query] [<VolumePath>]
fsutil 8dot3name [scan] [/s] [/l [<log file>] ] [/v] <DirectoryPath>
fsutil 8dot3name [set] { <DefaultValue> | <VolumePath> {1|0}}
fsutil 8dot3name [strip] [/t] [/s] [/f] [/l [<log file.] ] [/v] <DirectoryPath>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------------|---------------|
|requête [<VolumePath>]|Interroge le système de fichiers pour l’état du comportement de la création de nom court 8dot3.<br /><br />Si un *VolumePath* n’est pas spécifié en tant que paramètre, le paramètre de comportement de la création de 8dot3name par défaut pour tous les volumes s’affiche.|
|scan <DirectoryPath>|Analyse les fichiers qui se trouvent dans le texte spécifié *DirectoryPath* pour les clés de Registre qui peuvent être impactés si les noms courts 8dot3 ont été supprimés à partir des noms de fichier.|
|set { <DefaultValue> &#124; <VolumePath>}|Modifie le comportement de système de fichiers pour la création de noms 8dot3 dans les cas suivants :<br /><br /><ul><li>Lorsque *DefaultValue* est spécifié, la clé de Registre **HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreationNtfsDisable8dot3NameCreationNtfsDisable8dot3NameCreation**, est défini sur le *DefaultValue*.<br /><br />    Le *DefaultValue* peut avoir les valeurs suivantes :<br /><br /><ul><li>**0**: Permet la création de noms 8dot3 pour tous les volumes sur le système.</li><li>**1**: Désactive la création de noms 8dot3 pour tous les volumes sur le système.</li><li>**2**: Définit la création de noms 8dot3 sur par volume.</li><li>**3**: Désactive la création de noms 8dot3 pour tous les volumes à l’exception du volume système.</li></ul></li><li>Quand un *VolumePath* est spécifié, les volumes spécifiés sur le disque indicateur 8dot3name propriétés sont définies pour permettre la création de nom 8dot3 pour un volume spécifié (**0**), ou désactiver la création de noms 8dot3 sur le spécifié le volume (**1**).<br /><br />    Vous devez définir le comportement du système de fichiers par défaut pour la création de noms 8dot3 sur la valeur **2** avant de pouvoir activer ou désactiver la création de noms 8dot3 pour un volume spécifié.</li></ul>|
|strip <DirectoryPath>|Supprime les noms de fichiers 8dot3 pour tous les fichiers qui se trouvent dans le texte spécifié *DirectoryPath*. Le nom de fichier 8dot3 n’est pas supprimé tous les fichiers dans lequel le *DirectoryPath* combiné avec le fichier de nom contient plus de 260 caractères.<br /><br />Cette commande répertorie, mais ne modifie pas les clés de Registre qui pointent vers les fichiers ayant des noms de fichiers 8dot3 supprimés définitivement.<br /><br />Pour plus d’informations sur les effets de la suppression définitive les noms de fichiers 8dot3 à partir de fichiers, consultez [notes](Fsutil-8dot3name.md#BKMK_remarks).|
|<VolumePath>|Spécifie le nom du lecteur suivi d’un signe deux-points ou le GUID dans le format **Volume {***GUID***}**.|
|/f|Spécifie que tous les fichiers qui se trouvent dans le texte spécifié *DirectoryPath* ont les noms de fichiers 8dot3 supprimé même s’il existe des clés de Registre qui pointent vers les fichiers en utilisant le nom de fichier 8dot3. Dans ce cas, l’opération supprime les noms de fichiers 8dot3, mais ne modifie pas les clés de Registre qui pointent vers les fichiers qui utilisent les noms de fichiers 8dot3. **Avertissement :** Il est recommandé de sauvegarder votre répertoire ou de volume avant d’utiliser le **/f** paramètre, car elle peut entraîner des échecs d’application inattendue, y compris l’incapacité à désinstaller des programmes.|
|/l [<log file>]|Spécifie un fichier journal dans lequel les informations sont écrites.<br /><br />Si le **/l** paramètre n’est pas spécifié, toutes les informations sont écrit dans le fichier de journal par défaut :<br /><br />**% temp%\8dot3_removal_log@ (***GMT AAAA-MM-JJ HH-MM-SS***) .log**|
|/s|Spécifie que l’opération doit être appliquée dans les sous-répertoires de l’objet *DirectoryPath*.|
|/t|Spécifie que la suppression 8dot3 des noms de fichiers doit être exécutée en mode test. Toutes les opérations à l’exception de la suppression réelle des noms de fichiers 8dot3 sont effectuées. Vous pouvez utiliser le mode de test pour découvrir quel Registre clés pointent vers les fichiers qui utilisent les noms de fichiers 8dot3.|
|/v|Spécifie que toutes les informations sont écrites dans le fichier journal sont également affichées sur la ligne de commande.|

## <a name="BKMK_remarks"></a>Remarques

-   Supprime définitivement les noms de fichiers 8dot3 et ne modifiez ne pas les clés de Registre qui pointent vers les noms de fichiers 8dot3 peuvent entraîner les échecs d’application inattendue, notamment l’impossibilité de désinstaller une application. Il est recommandé de qu'abord sauvegarder votre répertoire ou de volume avant d’essayer de supprimer des noms de fichiers 8dot3.

## <a name="BKMK_examples"></a>Exemples
Pour obtenir le comportement de noms 8dot3 disable pour un volume de disque qui est spécifié avec le GUID, {928842df-5a01-11de-a85c-806e6f6e6963}, tapez :

```
fsutil 8dot3name query Volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

Vous pouvez également interroger le comportement de noms 8dot3 à l’aide de la **comportement** sous-commande.

Pour supprimer des noms de fichier 8dot3 dans le *D:\MyData* répertoire et tous les sous-répertoires, lors de l’écriture des informations dans le fichier journal qui est spécifié en tant que *mylogfile.log*, type :

```
fsutil 8dot3name strip /l mylogfile.log /s d:\MyData
```

### <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[fsutil 8dot3name](Fsutil-8dot3name.md)

[comportement de fsutil](Fsutil-behavior.md)


