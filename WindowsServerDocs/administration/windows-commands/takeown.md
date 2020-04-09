---
title: takeown
description: Apprenez à obtenir l’accès à un fichier en devenant le propriétaire du fichier.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0683cd65-a6db-4cab-962b-45a0ff61f43c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: af6952b8c4c14a717f7904ee0b77bf6ec9f5030e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833443"
---
# <a name="takeown"></a>takeown

Permet à l’administrateur de récupérer l’accès précédemment refusé à un fichier, faisant de l’administrateur le propriétaire du fichier.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
takeown [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] /f <File name> [/a] [/r [/d {Y|N}]]
```

#### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/s \<> de l’ordinateur|Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local. Ce paramètre s’applique à tous les fichiers et dossiers spécifiés dans la commande.|
|/u [\<> de domaine\]<User name>|Exécute le script avec les autorisations du compte d’utilisateur spécifié. La valeur par défaut est autorisations système.|
|/p [\<> de mot de passe]|Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** .|
|/f \<nom du fichier >|Spécifie le nom de fichier ou le modèle de nom de répertoire. Vous pouvez utiliser le caractère générique * lorsque vous spécifiez le modèle. Vous pouvez également utiliser la syntaxe *nom_partage*\*filename *.|
|/a|Donne la propriété au groupe administrateurs au lieu de l’utilisateur actuel.|
|/r|Effectue une opération récursive sur tous les fichiers dans le répertoire et les sous-répertoires spécifiés.|
|/d {o \| N}|Supprime l’invite de confirmation qui s’affiche lorsque l’utilisateur actuel ne dispose pas de l’autorisation « List Folder » sur un répertoire spécifié, et utilise à la place la valeur par défaut spécifiée. Les valeurs valides pour l’option **/d** sont les suivantes :</br>-Y : appropriation de l’annuaire.</br>-N : ignore le répertoire.</br>Notez que vous devez utiliser cette option conjointement avec l’option **/r** .|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Cette commande est généralement utilisée dans les fichiers de commandes.
-   Si le paramètre **/a** n’est pas spécifié, la propriété du fichier est accordée à l’utilisateur actuellement connecté à l’ordinateur.
-   Modèles mixtes utilisant ( **?** et **&#42;** ) ne sont pas pris en charge par la commande **takeown** .
-   Après avoir supprimé le verrou avec **takeown**, vous devrez peut-être utiliser l’Explorateur Windows ou la commande **cacls** pour vous accorder des autorisations d’accès complètes aux fichiers et aux répertoires avant de pouvoir les supprimer. Pour plus d’informations sur **cacls**, consultez « Références supplémentaires » à la fin de cette rubrique.

## <a name="examples"></a><a name="BKMK_examples"></a>Illustre

Pour prendre possession d’un fichier nommé Lostfile, tapez :
```
takeown /f lostfile
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)