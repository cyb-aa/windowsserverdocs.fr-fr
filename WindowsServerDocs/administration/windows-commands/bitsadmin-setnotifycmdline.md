---
title: bitsadmin setnotifycmdline
description: Rubrique de référence pour la commande Bitsadmin setnotifycmdline, qui définit la commande de ligne de commande qui s’exécute lorsque le travail termine le transfert de données, ou lorsqu’un travail entre dans un État.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 415ae6ef-8549-48b2-9693-2368a6e24075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b21d7151a5b646a4fe07d073220614f5e3c99539
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720133"
---
# <a name="bitsadmin-setnotifycmdline"></a>bitsadmin setnotifycmdline

Définit la commande de ligne de commande qui s’exécute une fois que le travail a terminé le transfert des données ou une fois qu’un travail a passé un état spécifié.

> [!NOTE]
> Cette commande n’est pas prise en charge par BITS 1,2 et versions antérieures.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /setnotifycmdline <job> <program_name> [program_parameters]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| travail | Nom complet ou GUID du travail. |
| program_name | Nom de la commande à exécuter lorsque le travail est terminé. Vous pouvez définir cette valeur sur NULL, mais si vous le faites, *program_parameters* doit également avoir la valeur null. |
| program_parameters | Paramètres que vous souhaitez passer à *program_name*. Vous pouvez définir cette valeur sur NULL. Si *program_parameters* n’a pas la valeur null, le premier paramètre de *program_parameters* doit correspondre à la *program_name*. |

## <a name="examples"></a>Exemples

Pour exécuter Notepad. exe à l’issue de la tâche nommée *myDownloadJob*:

```
bitsadmin /setnotifycmdline myDownloadJob c:\winnt\system32\notepad.exe NULL
```

Pour afficher le texte du CLUF dans Notepad. exe, à l’issue de la tâche nommée myDownloadJob :

```
bitsadmin /setnotifycmdline myDownloadJob c:\winnt\system32\notepad.exe notepad c:\eula.txt
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
