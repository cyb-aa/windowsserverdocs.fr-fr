---
title: erase
description: Rubrique de référence pour la commande Erase, qui supprime un ou plusieurs fichiers.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 024a4d0f-8679-4e06-b46f-61fdaf5464bc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ec812e9a455cc0060a3f0a6be4d0e7227821a0b
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992376"
---
# <a name="erase"></a>erase

Supprime un ou plusieurs fichiers. Cette commande effectue les mêmes actions que la commande **del** .

> [!WARNING]
> Si vous utilisez **Erase** pour supprimer un fichier de votre disque, vous ne pouvez pas le récupérer.

## <a name="syntax"></a>Syntaxe

```
erase [/p] [/f] [/s] [/q] [/a[:]<attributes>] <names>
del [/p] [/f] [/s] [/q] [/a[:]<attributes>] <names>
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

- Si vous utilisez la `erase /p` commande, le message suivant s’affiche :

    `FileName, Delete (Y/N)?`

    Pour confirmer la suppression, appuyez sur **o**. Pour annuler la suppression et afficher le nom de fichier suivant (si vous avez spécifié un groupe de fichiers), appuyez sur **N**. Pour arrêter la commande d' **effacement** , appuyez sur Ctrl + C.

- Si vous désactivez l’extension de commande, le paramètre **/s** affiche les noms de tous les fichiers qui n’ont pas été trouvés, au lieu d’afficher les noms des fichiers en cours de suppression.

- Si vous spécifiez des dossiers spécifiques `<names>` dans le paramètre, tous les fichiers inclus seront également supprimés. Par exemple, si vous souhaitez supprimer tous les fichiers dans le dossier *\work* , tapez :

  ```
  erase \work
  ```

- Vous pouvez utiliser des caractères génériques (**&#42;** et **?**) pour supprimer plusieurs fichiers à la fois. Toutefois, pour éviter la suppression accidentelle de fichiers, vous devez utiliser des caractères génériques avec précaution. Par exemple, si vous tapez la commande suivante :

  ```
  erase *.*
  ```

  La commande **Erase** affiche l’invite suivante :

  `Are you sure (Y/N)?`

  Pour supprimer tous les fichiers du répertoire actif, appuyez sur **o** , puis sur entrée. Pour annuler la suppression, appuyez sur **N** , puis sur entrée.

  > [!NOTE]
  > Avant d’utiliser des caractères génériques avec la commande **Erase** , utilisez les mêmes caractères génériques avec la commande **dir** pour répertorier tous les fichiers qui seront supprimés.

## <a name="examples"></a>Exemples

Pour supprimer tous les fichiers d’un dossier nommé test sur le lecteur C, tapez l’un des éléments suivants :

```
erase c:\test
erase c:\test\*.*
```

Pour supprimer tous les fichiers avec l’extension de nom de fichier. bat du répertoire actif, tapez :

```
erase *.bat
```

Pour supprimer tous les fichiers en lecture seule dans le répertoire actif, tapez :

```
erase /a:r *.*
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
