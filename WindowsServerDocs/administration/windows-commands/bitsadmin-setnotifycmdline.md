---
title: bitsadmin setnotifycmdline
description: La rubrique commandes Windows pour **Bitsadmin setnotifycmdline**, qui définit la commande de ligne de commande qui s’exécute lorsque le travail termine le transfert de données, ou lorsqu’un travail entre dans un État.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 415ae6ef-8549-48b2-9693-2368a6e24075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b268b68cbd355a7fe7f993d678a98f6fcb99f0ab
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122895"
---
# <a name="bitsadmin-setnotifycmdline"></a>bitsadmin setnotifycmdline

Définit la commande de ligne de commande qui s’exécute lorsque le travail termine le transfert de données ou lorsqu’un travail entre dans un état spécifié.

> [!NOTE]
> Cette commande n’est pas prise en charge par BITS 1,2 et versions antérieures.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /setnotifycmdline <job> <program_name> [program_parameters]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| le travail | Nom complet ou GUID du travail. |
| program_name | Nom de la commande à exécuter lorsque le travail est terminé. Vous pouvez définir cette valeur sur NULL, mais si vous le faites, *program_parameters* doit également avoir la valeur null. |
| program_parameters | Paramètres que vous souhaitez passer à *program_name*. Vous pouvez définir cette valeur sur NULL. Si *program_parameters* n’a pas la valeur null, le premier paramètre de *program_parameters* doit correspondre à la *program_name*. |

## <a name="examples"></a>Exemples

L’exemple suivant définit la commande de ligne de commande utilisée par le service pour exécuter Notepad. exe après la fin de la tâche nommée *myDownloadJob* .

```
C:\>bitsadmin /setnotifycmdline myDownloadJob c:\winnt\system32\notepad.exe NULL
```

```
C:\>bitsadmin /setnotifycmdline myDownloadJob c:\winnt\system32\notepad.exe notepad c:\eula.txt
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)