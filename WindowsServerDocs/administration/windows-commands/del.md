---
title: del
description: La rubrique commandes Windows pour del, qui supprime un ou plusieurs fichiers.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 346eede2-2085-44f5-9936-6877b5d5a833
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7069ee50a810296d31e1a034b24955299918020a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846662"
---
# <a name="del"></a>del

Supprime un ou plusieurs fichiers. Cette commande est identique à la commande **Erase** .

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
del [/p] [/f] [/s] [/q] [/a[:]<Attributes>] <Names>
erase [/p] [/f] [/s] [/q] [/a[:]<Attributes>] <Names>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Noms de \<>|Spécifie une liste d’un ou plusieurs fichiers ou répertoires. Les caractères génériques peuvent être utilisés pour supprimer plusieurs fichiers. Si un répertoire est spécifié, tous les fichiers du répertoire sont supprimés.|
|/p|Demande confirmation avant de supprimer le fichier spécifié.|
|/f|Force la suppression des fichiers en lecture seule.|
|/s|Supprime les fichiers spécifiés du répertoire actif et de tous ses sous-répertoires. Affiche les noms des fichiers au fur et à mesure de leur suppression.|
|/q|Spécifie le mode silencieux. Vous n’êtes pas invité à confirmer la suppression.|
|/a [ :]\<les attributs >|Supprime les fichiers en fonction des attributs de fichier suivants :</br>fichiers en lecture seule **r**</br>fichiers masqués **h**</br>**je** ne trouve pas les fichiers indexés</br>fichiers système **s**</br>**fichiers prêts** pour l’archivage</br>points d’analyse **l**</br>-Le préfixe signifie « not »|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

> [!CAUTION]
> Si vous utilisez **del** pour supprimer un fichier de votre disque, vous ne pouvez pas le récupérer.

-   Si vous utilisez **/p**, **del** affiche le nom d’un fichier et envoie le message suivant :

    `FileName, Delete (Y/N)?`

    Pour confirmer la suppression, appuyez sur o. Pour annuler la suppression et afficher le nom de fichier suivant (autrement dit, si vous avez spécifié un groupe de fichiers), appuyez sur N. Pour arrêter la commande **del** , appuyez sur Ctrl + C.
- Si vous désactivez les extensions de commande, **/s** affiche les noms de tous les fichiers qui n’ont pas été trouvés au lieu d’afficher les noms des fichiers en cours de suppression (autrement dit, le comportement est inversé).
- Si vous spécifiez un dossier dans *noms*, tous les fichiers du dossier sont supprimés. Par exemple, la commande suivante supprime tous les fichiers dans le dossier \work :  
  ```
  del \work
  ```  
- Vous pouvez utiliser des caractères génériques **&#42;** (et **?** ) pour supprimer plusieurs fichiers à la fois. Toutefois, pour éviter la suppression accidentelle de fichiers, vous devez utiliser des caractères génériques avec prudence avec la commande **del** . Par exemple, si vous tapez la commande suivante :  
  ```
  del *.*
  ```  
  La commande **del** affiche l’invite suivante :

  `Are you sure (Y/N)?`

  Pour supprimer tous les fichiers du répertoire actif, appuyez sur o, puis sur entrée. Pour annuler la suppression, appuyez sur N, puis sur entrée.

> [!NOTE]
> Avant d’utiliser des caractères génériques avec la commande **del** , utilisez les mêmes caractères génériques avec la commande **dir** pour répertorier tous les fichiers qui seront supprimés.

-   La commande **del** , avec des paramètres différents, est disponible à partir de la console de récupération.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

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
