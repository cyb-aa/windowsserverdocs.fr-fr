---
title: diskcomp
description: Rubrique de référence pour la commande diskcomp, qui compare le contenu de deux disquettes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4f56f534-a356-4daa-8b4f-38e089341e42
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fcb810f4cd18d51f8151b27a6f447c86130624fd
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992519"
---
# <a name="diskcomp"></a>diskcomp

Compare le contenu de deux disquettes. En cas d’utilisation sans paramètre, **diskcomp** utilise le lecteur actuel pour comparer les deux disques.

## <a name="syntax"></a>Syntaxe

```
diskcomp [<drive1>: [<drive2>:]]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `<drive1>` | Spécifie le lecteur contenant l’une des disquettes. |
| /? | Affiche l'aide à l'invite de commandes. |

#### <a name="remarks"></a>Notes 

- La commande **diskcomp** fonctionne uniquement avec les disquettes. Vous ne pouvez pas utiliser **diskcomp** avec un disque dur. Si vous spécifiez un disque dur pour *lecteur1* ou *lecteur2*, **diskcomp** affiche le message d’erreur suivant :

  ```
  Invalid drive specification
  Specified drive does not exist
  or is nonremovable
  ```

- Si toutes les pistes sur les deux disques comparées sont identiques (il ignore le numéro de volume d’un disque), **diskcomp** affiche le message suivant :

  ```
  Compare OK
  ```

  Si les pistes ne sont pas les mêmes, **diskcomp** affiche un message similaire à ce qui suit :

  ```
  Compare error on
  side 1, track 2
  ```

  Lorsque **diskcomp** termine la comparaison, il affiche le message suivant :

  ```
  Compare another diskette (Y/N)?
  ```

  Si vous appuyez sur **Y**, **diskcomp** vous invite à insérer le disque pour la comparaison suivante. Si vous appuyez sur **N**, **diskcomp** arrête la comparaison.

- Si vous omettez le paramètre *lecteur2* , **diskcomp** utilise le lecteur actif pour *lecteur2*. Si vous omettez les deux paramètres de lecteur, **diskcomp** utilise le lecteur actif pour les deux. Si le lecteur actuel est le même que *lecteur1*, **diskcomp** vous invite à échanger des disques si nécessaire.

- Si vous spécifiez le même lecteur de disquette pour *lecteur1* et *lecteur2*, **diskcomp** les compare à l’aide d’un lecteur et vous invite à insérer les disques en fonction des besoins. Vous devrez peut-être permuter les disques plusieurs fois, en fonction de la capacité des disques et de la quantité de mémoire disponible.

- **Diskcomp** ne peut pas comparer un disque à une seule face et un disque à double face, ni un disque à haute densité avec un disque à double densité. Si le disque de *lecteur1* n’est pas du même type que le disque dans *lecteur2*, **diskcomp** affiche le message suivant :

  ```
  Drive types or diskette types not compatible
  ```

- **Diskcomp** ne fonctionne pas sur un lecteur réseau ou sur un lecteur créé par la commande **subst** . Si vous tentez d’utiliser **diskcomp** avec un lecteur de l’un de ces types, **diskcomp** affiche le message d’erreur suivant :

  ```
  Invalid drive specification
  ```

- Si vous utilisez **diskcomp** avec un disque que vous avez créé à l’aide de la commande **copier**, **diskcomp** peut afficher un message similaire à ce qui suit :

  ```
  Compare error on
  side 0, track 0
  ```

  Ce type d’erreur peut se produire même si les fichiers sur les disques sont identiques. Bien que **copie** les informations en double, elles ne sont pas nécessairement placées au même emplacement sur le disque de destination.

- codes de sortie de **diskcomp** :

  | Code de sortie | Description |
  | --------- | ----------- |
  | 0 | Les disques sont identiques |
  | 1 | Des différences ont été trouvées |
  | 3 | Une erreur matérielle s’est produite |
  | 4 | Une erreur d’initialisation s’est produite |

  Pour traiter les codes de sortie retournés par **diskcomp**, vous pouvez utiliser la variable d’environnement *ERRORLEVEL* sur la ligne de commande **If** dans un programme de traitement par lots.

## <a name="examples"></a>Exemples

Si votre ordinateur ne possède qu’un seul lecteur de disquette (par exemple, le lecteur A) et que vous souhaitez comparer deux disques, tapez :

```
diskcomp a: a:
```

**Diskcomp** vous invite à insérer chaque disque, si nécessaire.

Pour illustrer le traitement d’un code de sortie **diskcomp** dans un programme de traitement par lots qui utilise la variable d’environnement *ERRORLEVEL* sur la ligne de commande **If** :

```
rem Checkout.bat compares the disks in drive A and B
echo off
diskcomp a: b:
if errorlevel 4 goto ini_error
if errorlevel 3 goto hard_error
if errorlevel 1 goto no_compare
if errorlevel 0 goto compare_ok
:ini_error
echo ERROR: Insufficient memory or command invalid
goto exit
:hard_error
echo ERROR: An irrecoverable error occurred
goto exit
:break
echo You just pressed CTRL+C to stop the comparison
goto exit
:no_compare
echo Disks are not the same
goto exit
:compare_ok
echo The comparison was successful; the disks are the same
goto exit
:exit
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
