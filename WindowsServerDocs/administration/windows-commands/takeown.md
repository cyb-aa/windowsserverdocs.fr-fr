---
title: takeown
description: Découvrez comment accéder à un fichier en devenir le propriétaire du fichier.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0683cd65-a6db-4cab-962b-45a0ff61f43c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4b5a4874edf9fa4406d4643e686fed2b725699dd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854360"
---
# <a name="takeown"></a>takeown

Permet à l’administrateur de récupérer l’accès précédemment refusé à un fichier, faisant de l’administrateur le propriétaire du fichier.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
takeown [/s <Computer> [/u [<Domain>\]<User name> [/p [<Password>]]]] /f <File name> [/a] [/r [/d {Y|N}]]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/s \<ordinateur >|Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local. Ce paramètre s’applique à tous les fichiers et dossiers spécifiés dans la commande.|
|/u [\<domaine >\]<User name>|Exécute le script avec les autorisations du compte d’utilisateur spécifié. La valeur par défaut est les autorisations du système.|
|/p [\<Password>]|Spécifie le mot de passe du compte d’utilisateur qui est spécifié dans le **/u** paramètre.|
|/f \<nom de fichier >|Spécifie le nom de répertoire ou le nom de fichier modèle. Vous pouvez utiliser le caractère générique * lorsque vous spécifiez le modèle. Vous pouvez également utiliser la syntaxe *nom_partage*\*nom de fichier *.|
|/a|Donne la propriété au groupe Administrateurs au lieu de l’utilisateur actuel.|
|/r|Effectue une opération récursive sur tous les fichiers dans le répertoire spécifié et les sous-répertoires.|
|/d {Y \| N}|Supprime l’invite de confirmation qui s’affiche lorsque l’utilisateur actuel ne dispose pas de l’autorisation « Lister le dossier » sur un répertoire spécifié et utilise à la place la valeur par défaut spécifiée. Les valeurs valides pour le **/d** option sont les suivantes :</br>-Y : Prendre possession du répertoire.</br>-N : Ignorer le répertoire.</br>Notez que vous devez utiliser cette option conjointement avec le **/r** option.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Cette commande est généralement utilisée dans les fichiers de commandes.
-   Si le **/a** paramètre n’est pas spécifié, la propriété de fichier est donnée à l’utilisateur actuellement connecté à l’ordinateur.
-   À l’aide de modèles mixtes (**?** et **&#42;**) ne sont pas pris en charge par **takeown** commande.
-   Après avoir supprimé le verrou avec **takeown**, vous devrez peut-être utiliser l’Explorateur Windows ou le **cacls** commande pour bénéficier de toutes les autorisations nécessaires pour les fichiers et répertoires avant de les supprimer. Pour plus d’informations sur **cacls**, consultez « Références supplémentaires » à la fin de cette rubrique.

## <a name="BKMK_examples"></a>Exemples

Pour prendre possession d’un fichier nommé Lostfile, tapez :
```
takeown /f lostfile
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)