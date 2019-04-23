---
title: bitsadmin setnotifycmdline
description: Rubrique de commandes de Windows pour ***-bitsadmin setnotifycmdlineSets la ligne de commande qui s’exécute lorsque le travail terminé le transfert de données ou lorsqu’un travail passe à un état.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 415ae6ef-8549-48b2-9693-2368a6e24075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f1cea4e99cbaaf3881c6f436bdb932090ad6b006
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859070"
---
# <a name="bitsadmin-setnotifycmdline"></a>bitsadmin setnotifycmdline

Définit la ligne de commande qui s’exécute lorsque le travail terminé le transfert de données ou lorsqu’un travail passe à un état.

**BITS 1.2 et versions antérieures**: Non pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetNotifyCmdLine <Job> <ProgramName> [ProgramParameters]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|
|ProgramName|Nom de la commande à exécuter lorsque le travail est terminé.|
|ProgramParameters|Les paramètres que vous souhaitez passer à *nom_programme*.|

## <a name="remarks"></a>Notes

Vous pouvez spécifier NULL pour *nom_programme* et *ProgramParameters*. Si *nom_programme* est NULL, *ProgramParameters* doit être NULL.

> [!IMPORTANT]
> Si *ProgramParameters* n’est pas NULL, alors que le premier paramètre dans *ProgramParameters* doit correspondre à *nom_programme*.

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant définit la ligne de commande utilisé par le service pour exécuter le bloc-notes lorsque le travail nommé *myDownloadJob* se termine.
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe NULL
```
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe "notepad c:\eula.txt"
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)