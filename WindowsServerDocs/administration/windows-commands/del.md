---
title: del
description: Rubrique de référence pour la commande del, qui supprime un ou plusieurs fichiers.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 346eede2-2085-44f5-9936-6877b5d5a833
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f32efd6e29a715cdc67b5a1ddcb166922d1cfcc9
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993127"
---
# <a name="del"></a>del

Supprime un ou plusieurs fichiers. Cette commande effectue les mêmes actions que la commande **Erase** .

La commande **del** peut également être exécutée à partir de la console de récupération Windows, à l’aide de différents paramètres. Pour plus d’informations, consultez [environnement de récupération Windows (WinRE)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference).

> [!WARNING]
> Si vous utilisez **del** pour supprimer un fichier de votre disque, vous ne pouvez pas le récupérer.

## <a name="syntax"></a>Syntaxe

```
del [/p] [/f] [/s] [/q] [/a[:]<attributes>] <names>
erase [/p] [/f] [/s] [/q] [/a[:]<attributes>] <names>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `<names>` | Spécifie une liste d’un ou plusieurs fichiers ou répertoires. Les caractères génériques peuvent être utilisés pour supprimer plusieurs fichiers. Si un répertoire est spécifié, tous les fichiers du répertoire sont supprimés. |
| /p | Demande confirmation avant de supprimer le fichier spécifié. |
| /f | Force la suppression des fichiers en lecture seule. |
| /s | Supprime les fichiers spécifiés du répertoire actif et de tous ses sous-répertoires. Affiche les noms des fichiers au fur et à mesure de leur suppression. |
| /q | Spécifie le mode silencieux. Vous n’êtes pas invité à confirmer la suppression. |
| /a [ :]`<attributes>` | Supprime les fichiers en fonction des attributs de fichier suivants :<ul><li>fichiers en lecture seule **r**</li><li>fichiers masqués **h**</li><li>**je** ne trouve pas les fichiers indexés</li><li>fichiers système **s**</li><li>**fichiers prêts** pour l’archivage</li><li>points d’analyse **l**</li><li>**-** Utilisé comme préfixe signifiant « not »</li></ul>. |
| /? | Affiche l'aide à l'invite de commandes. |

#### <a name="remarks"></a>Notes 

- Si vous utilisez la `del /p` commande, le message suivant s’affiche :

    `FileName, Delete (Y/N)?`

    Pour confirmer la suppression, appuyez sur **o**. Pour annuler la suppression et afficher le nom de fichier suivant (si vous avez spécifié un groupe de fichiers), appuyez sur **N**. Pour arrêter la commande **del** , appuyez sur Ctrl + C.

- Si vous désactivez l’extension de commande, le paramètre **/s** affiche les noms de tous les fichiers qui n’ont pas été trouvés, au lieu d’afficher les noms des fichiers en cours de suppression.

- Si vous spécifiez des dossiers spécifiques `<names>` dans le paramètre, tous les fichiers inclus seront également supprimés. Par exemple, si vous souhaitez supprimer tous les fichiers dans le dossier *\work* , tapez :

  ```
  del \work
  ```

- Vous pouvez utiliser des caractères génériques (**&#42;** et **?**) pour supprimer plusieurs fichiers à la fois. Toutefois, pour éviter la suppression accidentelle de fichiers, vous devez utiliser des caractères génériques avec précaution. Par exemple, si vous tapez la commande suivante :

  ```
  del *.*
  ```

  La commande **del** affiche l’invite suivante :

  `Are you sure (Y/N)?`

  Pour supprimer tous les fichiers du répertoire actif, appuyez sur **o** , puis sur entrée. Pour annuler la suppression, appuyez sur **N** , puis sur entrée.

  > [!NOTE]
  > Avant d’utiliser des caractères génériques avec la commande **del** , utilisez les mêmes caractères génériques avec la commande **dir** pour répertorier tous les fichiers qui seront supprimés.

## <a name="examples"></a>Exemples

Pour supprimer tous les fichiers d’un dossier nommé test sur le lecteur C, tapez l’un des éléments suivants :

```
del c:\test
del c:\test\*.*
```

Pour supprimer tous les fichiers avec l’extension de nom de fichier. bat du répertoire actif, tapez :

```
del *.bat
```

Pour supprimer tous les fichiers en lecture seule dans le répertoire actif, tapez :

```
del /a:r *.*
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [Environnement de récupération Windows (WinRE)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference)
