---
title: bitsadmin setreplyfilename
description: La rubrique commandes Windows pour Bitsadmin setreplyfilename, qui spécifie le chemin d’accès du fichier qui contient la réponse du serveur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c26d3342-0533-40b1-a13e-e09678232b25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fd45174a7deac89cc943fb19d544e372c0198139
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849182"
---
# <a name="bitsadmin-setreplyfilename"></a>bitsadmin setreplyfilename

Spécifie le chemin d’accès du fichier qui contient la réponse du serveur.

**BITS 1,2 et versions antérieures**: non pris en charge.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetReplyFileName <Job> <Path>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|Chemin d'accès|Emplacement où la réponse du serveur doit être placée|

## <a name="remarks"></a>Notes

Valide uniquement pour les travaux de chargement-réponse.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant définit le nom de fichier de réponse pathfor la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /SetReplyFileName myDownloadJob c:\reply
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)