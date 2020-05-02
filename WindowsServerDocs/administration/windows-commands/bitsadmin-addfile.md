---
title: bitsadmin addfile
description: Rubrique de référence pour la commande Bitsadmin AddFile, qui ajoute un fichier au travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1b31aa93-0364-465b-af36-754968825989
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eaa7d77c9d6160bbd2bdf6a1431232af22bc3e37
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718495"
---
# <a name="bitsadmin-addfile"></a>bitsadmin addfile

Ajoute un fichier au travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /addfile <job> <remoteURL> <localname>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| travail | Nom complet ou GUID du travail. |
| remoteURL | URL du fichier sur le serveur. |
| localname | Nom du fichier sur l’ordinateur local. *LocalName* doit contenir un chemin d’accès absolu au fichier. |

## <a name="examples"></a>Exemples

Pour ajouter un fichier à la tâche :

```
bitsadmin /addfile myDownloadJob http://downloadsrv/10mb.zip c:\10mb.zip
```

Répétez cet appel pour chaque fichier à ajouter. Si plusieurs travaux utilisent *myDownloadJob* comme nom, vous devez remplacer *MYDOWNLOADJOB* par le GUID du travail pour identifier le travail de façon unique.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
