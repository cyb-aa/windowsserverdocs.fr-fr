---
title: diskcopy
description: Rubrique de référence pour la commande diskcopy, qui copie le contenu de la disquette dans le lecteur source sur une disquette formatée ou non dans le lecteur de destination.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5fd21efa-52cc-4e70-a7fe-35125a435106
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/07/2018
ms.openlocfilehash: b8fd482d7c2e5933f269320df2bff75f65195bc2
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82992525"
---
# <a name="diskcopy"></a>diskcopy

Copie le contenu de la disquette dans le lecteur source vers une disquette formatée ou non formatée dans le lecteur de destination. En cas d’utilisation sans paramètre, **diskcopy** utilise le lecteur actif pour le disque source et le disque de destination.

## <a name="syntax"></a>Syntaxe

```
diskcopy [<drive1>: [<drive2>:]] [/v]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `<drive1>` | Spécifie le lecteur qui contient le disque source. |
| /v | Vérifie que les informations sont copiées correctement. Cette option ralentit le processus de copie. |
| /? | Affiche l'aide à l'invite de commandes. |

#### <a name="remarks"></a>Notes 

- **Diskcopy** fonctionne uniquement avec les disques amovibles tels que les disquettes, qui doivent être du même type. Vous ne pouvez pas utiliser **diskcopy** avec un disque dur. Si vous spécifiez un disque dur pour *lecteur1* ou *lecteur2*, **diskcopy** affiche le message d’erreur suivant :

    ```
    Invalid drive specification
    Specified drive does not exist or is nonremovable
    ```

    La commande **diskcopy** vous invite à insérer les disques source et de destination et attend que vous appuyiez sur une touche du clavier avant de continuer.

    Une fois le disque copié, **diskcopy** affiche le message suivant :

    ```
    Copy another diskette (Y/N)?
    ```

    Si vous appuyez sur **o**, **diskcopy** vous invite à insérer les disques source et de destination pour l’opération de copie suivante. Pour arrêter le processus **diskcopy** , appuyez sur **N**.

    Si vous effectuez une copie sur une disquette non formatée dans *lecteur2*, **diskcopy** formate le disque avec le même nombre de côtés et de secteurs par piste que sur le disque de *lecteur1*. **Diskcopy** affiche le message suivant pendant qu’il formate le disque et copie les fichiers :

    ```
    Formatting while copying
    ```

- Si le disque source a un numéro de série de volume, **diskcopy** crée un nouveau numéro de série de volume pour le disque de destination et affiche le nombre lorsque l’opération de copie est terminée.

- Si vous omettez le paramètre *lecteur2* , **diskcopy** utilise le lecteur actuel comme lecteur de destination. Si vous omettez les deux paramètres de lecteur, **diskcopy** utilise le lecteur actif pour les deux. Si le lecteur actif est le même que le *lecteur1*, **diskcopy** vous invite à échanger des disques en fonction des besoins.

- Exécutez **diskcopy** à partir d’un lecteur autre que le lecteur de disquette, par exemple le lecteur C. Si le lecteur de disquette *lecteur1* et le *lecteur2* de disquette sont identiques, **diskcopy** vous invite à changer de disque. Si les disques contiennent plus d’informations que la mémoire disponible ne peut en contenir, **diskcopy** ne peut pas lire toutes les informations à la fois. **Diskcopy** lit à partir du disque source, écrit sur le disque de destination et vous invite à insérer à nouveau le disque source. Ce processus se poursuit jusqu’à ce que vous ayez copié la totalité du disque.

- La fragmentation est la présence de petites zones d’espace disque inutilisé entre des fichiers existants sur un disque. Un disque source fragmenté peut ralentir le processus de recherche, de lecture ou d’écriture de fichiers.

    Étant donné que **diskcopy** effectue une copie exacte du disque source sur le disque de destination, toute fragmentation sur le disque source est transférée vers le disque de destination. Pour éviter de transférer la fragmentation d’un disque à un autre, utilisez la commande [Copy](copy.md) ou la [commande xcopy](xcopy.md) pour copier votre disque. Étant donné que **Copy** et **xcopy** copient les fichiers de façon séquentielle, le nouveau disque n’est pas fragmenté.

    > [!NOTE]
    > Vous ne pouvez pas utiliser **xcopy** pour copier un disque de démarrage.

- codes de sortie de **diskcopy** :

    | Code de sortie | Description |
    | --------- | ----------- |
    | 0 | L’opération de copie a réussi |
    | 1 | Une erreur de lecture/écriture récupérable s’est produite |
    | 3 | Une erreur matérielle irrécupérable s’est produite |
    | 4 | Une erreur d’initialisation s’est produite |

    Pour traiter les codes de sortie retournés par **diskcomp**, vous pouvez utiliser la variable d’environnement *ERRORLEVEL* sur la ligne de commande **If** dans un programme de traitement par lots.

## <a name="examples"></a>Exemples

Pour copier le disque du lecteur B sur le disque du lecteur A, tapez :

```
diskcopy b: a:
```

Pour utiliser le lecteur de disquette A afin de copier une disquette sur une autre, commencez par basculer vers le lecteur C, puis tapez :

diskcopy a :

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande xcopy](xcopy.md)

- [copier (commande)](copy.md)
