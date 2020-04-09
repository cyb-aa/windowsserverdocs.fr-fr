---
title: attrib
description: La rubrique commandes Windows pour **Attrib**, qui affiche, définit ou supprime les attributs affectés aux fichiers ou aux répertoires.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5e763ca5-21a2-45d2-b26d-a9c44c99091a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94a6f307450b06dff81180b70f864bd9e1ed5885
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851252"
---
# <a name="attrib"></a>attrib

Affiche, définit ou supprime les attributs affectés aux fichiers ou aux répertoires. S’il est utilisé sans paramètres, **Attrib** affiche les attributs de tous les fichiers dans le répertoire actif.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
attrib [{+|-}r] [{+|-}a] [{+|-}s] [{+|-}h] [{+|-}i] [<Drive>:][<Path>][<FileName>] [/s [/d] [/l]]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `{+|-}r` | Définit ( **+** ) ou efface ( **-** ) l’attribut de fichier en lecture seule. |
| `{+\|-}a` | Définit ( **+** ) ou efface ( **-** ) l’attribut de fichier d’archive. |
| `{+\|-}s` | Définit ( **+** ) ou efface ( **-** ) l’attribut de fichier système. |
| `{+\|-}h` | Définit ( **+** ) ou efface ( **-** ) l’attribut de fichier masqué. |
| `{+\|-}i` | Définit ( **+** ) ou efface ( **-** ) l’attribut de fichier qui n’est pas un index de contenu. |
| `[<Drive>:][<Path>][<FileName>]` | Spécifie l’emplacement et le nom du répertoire, du fichier ou du groupe de fichiers dont vous souhaitez afficher ou modifier les attributs. Vous pouvez utiliser l' **?** et **&#42;** les caractères génériques dans le paramètre *filename* pour afficher ou modifier les attributs d’un groupe de fichiers. |
| /s | Applique **Attrib** et toutes les options de ligne de commande aux fichiers correspondants dans le répertoire actif et tous ses sous-répertoires. |
| /d | Applique **Attrib** et toutes les options de ligne de commande aux répertoires. |
| /l | Applique **Attrib** et toutes les options de ligne de commande au lien symbolique, plutôt qu’à la cible du lien symbolique. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="remarks"></a>Notes

- Vous pouvez utiliser des caractères génériques ( **?** et **&#42;** ) avec le paramètre *filename* pour afficher ou modifier les attributs d’un groupe de fichiers.

- Si l’attribut System (**s**) ou Hidden (**h**) est défini pour un fichier, vous devez effacer l’attribut pour pouvoir modifier les autres attributs de ce fichier.

- L’attribut archive (**a**) marque les fichiers qui ont été modifiés depuis la dernière sauvegarde. Notez que la commande **xcopy** utilise des attributs d’archive.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour afficher les attributs d’un fichier nommé News86 qui se trouve dans le répertoire actif, tapez :

```
attrib news86 
```

Pour affecter l’attribut de lecture seule au fichier nommé Report. txt, tapez :

```
attrib +r report.txt 
```

Pour supprimer l’attribut lecture seule des fichiers du répertoire public et de ses sous-répertoires sur un disque du lecteur B, tapez :

```
attrib -r b:\public\*.* /s 
```

Pour définir l’attribut Archive pour tous les fichiers sur le lecteur A, puis désactivez l’attribut Archive pour les fichiers avec l’extension. bak, tapez :

```
attrib +a a:*.* & attrib -a a:*.bak 
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)