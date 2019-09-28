---
title: bitsadmin addfile
description: 'Rubrique relative aux commandes Windows pour **Bitsadmin AddFile** : ajoute un fichier au travail spécifié.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 8dfddda92e506dbfca2a47394a310edf16fe78aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382043"
---
# <a name="bitsadmin-addfile"></a>bitsadmin addfile

Ajoute un fichier au travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /AddFile <Job> <RemoteURL> <LocalName>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|RemoteURL|URL du fichier sur le serveur.|
|LocalName|Nom du fichier sur l’ordinateur local. *LocalName* doit contenir un chemin d’accès absolu au fichier.|

## <a name="BKMK_examples"></a>Illustre

Ajoutez un fichier au travail. Répétez cet appel pour chaque fichier que vous souhaitez ajouter. Si plusieurs travaux utilisent *myDownloadJob* comme nom, vous devez remplacer *MYDOWNLOADJOB* par le GUID du travail pour identifier le travail de façon unique.
```
C:\>bitsadmin /addfile myDownloadJob http://downloadsrv/10mb.zip c:\10mb.zip
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)