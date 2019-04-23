---
title: bitsadmin addfile
description: Rubrique de commandes de Windows pour **bitsadmin addfile** -ajoute un fichier pour le travail spécifié.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b31aa93-0364-465b-af36-754968825989
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c3027bdc4f3f8f3e3ca50400b2c5dbf33bf2bc5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861750"
---
# <a name="bitsadmin-addfile"></a>bitsadmin addfile

Ajoute un fichier pour le travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /AddFile <Job> <RemoteURL> <LocalName>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|
|RemoteURL|L’URL du fichier sur le serveur.|
|LocalName|Le nom du fichier sur l’ordinateur local. *LocalName* doit contenir un chemin d’accès absolu au fichier.|

## <a name="BKMK_examples"></a>Exemples

Ajoutez un fichier à la tâche. Répétez cet appel pour chaque fichier que vous souhaitez ajouter. Si vous utilisent plusieurs travaux *myDownloadJob* comme leur nom, vous devez remplacer *myDownloadJob* avec le GUID du travail pour identifier de façon unique le travail.
```
C:\>bitsadmin /addfile myDownloadJob http://downloadsrv/10mb.zip c:\10mb.zip
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)