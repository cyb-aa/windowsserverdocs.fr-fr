---
title: bitsadmin setnotifycmdline
description: La rubrique commandes Windows pour * * * *-Bitsadmin setnotifycmdlineSets la commande de ligne de commande qui s’exécute lorsque le travail termine le transfert de données ou lorsqu’un travail entre dans un État.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 7a307fe552e7d8ec5852de953a3a439cb02246ec
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380477"
---
# <a name="bitsadmin-setnotifycmdline"></a>bitsadmin setnotifycmdline

Définit la commande de ligne de commande qui s’exécute lorsque le travail termine le transfert de données ou lorsqu’un travail entre dans un État.

**BITS 1,2 et versions antérieures**: Non pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetNotifyCmdLine <Job> <ProgramName> [ProgramParameters]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|Nom_programme|Nom de la commande à exécuter lorsque le travail est terminé.|
|ProgramParameters|Paramètres que vous souhaitez passer au *nom_programme*.|

## <a name="remarks"></a>Notes

Vous pouvez spécifier NULL pour *nom_programme* et *ProgramParameters*. Si *nom_programme* a la valeur null, *ProgramParameters* doit avoir la valeur null.

> [!IMPORTANT]
> Si *ProgramParameters* n’a pas la valeur null, le premier paramètre dans *ProgramParameters* doit correspondre au *nom_programme*.

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant définit la commande de ligne de commande utilisée par le service pour exécuter le bloc-notes lorsque la tâche nommée *myDownloadJob* se termine.
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe NULL
```
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe "notepad c:\eula.txt"
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)