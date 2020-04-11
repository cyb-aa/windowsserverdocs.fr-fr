---
title: bitsadmin setreplyfilename
description: La rubrique commandes Windows pour **Bitsadmin setreplyfilename**, qui spécifie le chemin d’accès du fichier qui contient le chargement-réponse du serveur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c26d3342-0533-40b1-a13e-e09678232b25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6c476073cb22ff66bcefc75a45fcd0526cdf3d25
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122735"
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
| le travail | Nom complet ou GUID du travail. |
| file_path | Emplacement où placer le serveur-répondre. |

## <a name="examples"></a>Exemples

L’exemple suivant définit le chemin d’accès de fichier de chargement-réponse pour le travail nommé *myDownloadJob*.

```
C:\>bitsadmin /setreplyfilename myDownloadJob c:\upload-reply
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)