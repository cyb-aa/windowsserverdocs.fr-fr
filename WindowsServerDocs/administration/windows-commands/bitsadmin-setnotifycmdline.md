---
title: bitsadmin setnotifycmdline
description: La rubrique commandes Windows pour Bitsadmin setnotifycmdline, qui définit la commande de ligne de commande qui s’exécute lorsque le travail termine le transfert de données ou lorsqu’un travail entre dans un État.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 415ae6ef-8549-48b2-9693-2368a6e24075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 761a7003e44e8dc15cb2dd2f1ce5a1a23be53286
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849332"
---
# <a name="bitsadmin-setnotifycmdline"></a>bitsadmin setnotifycmdline

Définit la commande de ligne de commande qui s’exécute lorsque le travail termine le transfert de données ou lorsqu’un travail entre dans un État.

**BITS 1,2 et versions antérieures**: non pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetNotifyCmdLine <Job> <ProgramName> [ProgramParameters]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|Nom_programme|Nom de la commande à exécuter lorsque le travail est terminé.|
|ProgramParameters|Paramètres que vous souhaitez passer au *nom_programme*.|

## <a name="remarks"></a>Notes

Vous pouvez spécifier NULL pour *nom_programme* et *ProgramParameters*. Si *nom_programme* a la valeur null, *ProgramParameters* doit avoir la valeur null.

> [!IMPORTANT]
> Si *ProgramParameters* n’a pas la valeur null, le premier paramètre dans *ProgramParameters* doit correspondre au *nom_programme*.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant définit la commande de ligne de commande utilisée par le service pour exécuter le bloc-notes lorsque la tâche nommée *myDownloadJob* se termine.
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe NULL
```
```
C:\>bitsadmin /SetNotifyCmdLine myDownloadJob c:\winnt\system32\notepad.exe notepad c:\eula.txt
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)