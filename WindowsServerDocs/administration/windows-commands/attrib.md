---
title: attrib
description: Rubrique de référence pour la commande Attrib, qui affiche, définit ou supprime les attributs affectés aux fichiers ou aux répertoires.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5e763ca5-21a2-45d2-b26d-a9c44c99091a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c525fe1dc5b78032f20358492a1bfde4df909add
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719192"
---
# <a name="attrib"></a>attrib

Affiche, définit ou supprime les attributs affectés aux fichiers ou aux répertoires. S’il est utilisé sans paramètres, **Attrib** affiche les attributs de tous les fichiers dans le répertoire actif.

## <a name="syntax"></a>Syntaxe

```
attrib [{+|-}r] [{+|-}a] [{+|-}s] [{+|-}h] [{+|-}i] [<drive>:][<path>][<filename>] [/s [/d] [/l]]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `{+|-}r` | Définit (**+**) ou efface (**-**) l’attribut de fichier en lecture seule. |
| `{+\|-}a` | Définit (**+**) ou efface (**-**) l’attribut de fichier d’archive. Cet ensemble d’attributs marque les fichiers qui ont été modifiés depuis la dernière sauvegarde. Notez que la commande **xcopy** utilise des attributs d’archive. |
| `{+\|-}s` | Définit (**+**) ou efface (**-**) l’attribut de fichier système. Si un fichier utilise cet attribut, vous devez effacer l’attribut pour pouvoir modifier les autres attributs du fichier. |
| `{+\|-}h` | Définit (**+**) ou efface (**-**) l’attribut de fichier masqué. Si un fichier utilise cet attribut, vous devez effacer l’attribut pour pouvoir modifier les autres attributs du fichier. |
| `{+\|-}i` | Définit (**+**) ou efface (**-**) l’attribut de fichier non indexé de contenu. |
| `[<drive>:][<path>][<filename>]` | Spécifie l’emplacement et le nom du répertoire, du fichier ou du groupe de fichiers dont vous souhaitez afficher ou modifier les attributs.<p>Vous pouvez utiliser l' **?** et **&#42;** caractères génériques dans le paramètre *filename* pour afficher ou modifier les attributs d’un groupe de fichiers. |
| /s | Applique **Attrib** et toutes les options de ligne de commande aux fichiers correspondants dans le répertoire actif et tous ses sous-répertoires. |
| /d | Applique **Attrib** et toutes les options de ligne de commande aux répertoires. |
| /l | Applique **Attrib** et toutes les options de ligne de commande au lien symbolique, plutôt qu’à la cible du lien symbolique. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="examples"></a>Exemples

Pour afficher les attributs d’un fichier nommé News86 qui se trouve dans le répertoire actif, tapez :

```
attrib news86
```

Pour affecter l’attribut de lecture seule au fichier nommé Report. txt, tapez :

```
attrib +r report.txt
```

Pour supprimer l’attribut lecture seule des fichiers du répertoire public et de ses sous-répertoires sur un disque du lecteur b :, tapez :

```
attrib -r b:\public\*.* /s
```

Pour définir l’attribut Archive pour tous les fichiers sur le lecteur a :, puis désactivez l’attribut Archive pour les fichiers avec l’extension. bak, tapez :

```
attrib +a a:*.* & attrib -a a:*.bak
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande xcopy](xcopy.md)
