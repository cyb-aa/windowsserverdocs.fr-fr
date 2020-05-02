---
title: bitsadmin setreplyfilename
description: Rubrique de référence pour la commande Bitsadmin setreplyfilename, qui spécifie le chemin d’accès du fichier qui contient le chargement-réponse du serveur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c26d3342-0533-40b1-a13e-e09678232b25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2f0bd184db274dc915817ff3e26ae2c686190c27
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720478"
---
# <a name="bitsadmin-setreplyfilename"></a>bitsadmin setreplyfilename

Spécifie le chemin d’accès du fichier qui contient le chargement-réponse du serveur.

> [!NOTE]
> Cette commande n’est pas prise en charge par BITS 1,2 et versions antérieures.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /setreplyfilename <job> <file_path>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |
| file_path | Emplacement où placer le serveur-répondre. |

## <a name="examples"></a>Exemples

Pour définir le chemin du fichier de nom de fichier de chargement-réponse pour le travail nommé *myDownloadJob*:

```
bitsadmin /setreplyfilename myDownloadJob c:\upload-reply
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
