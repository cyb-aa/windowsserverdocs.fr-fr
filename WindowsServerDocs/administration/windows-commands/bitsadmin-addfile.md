---
title: bitsadmin addfile
description: La rubrique commandes Windows pour **Bitsadmin AddFile**, qui ajoute un fichier au travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1b31aa93-0364-465b-af36-754968825989
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 330e79eb2ba5a824cea54094f64ceb6f9cfd66b9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850962"
---
# <a name="bitsadmin-addfile"></a>bitsadmin addfile

Ajoute un fichier au travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /AddFile <Job> <RemoteURL> <LocalName>
```

#### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| Tâche | Nom complet ou GUID du travail. |
| RemoteURL | URL du fichier sur le serveur. |
| LocalName | Nom du fichier sur l’ordinateur local. *LocalName* doit contenir un chemin d’accès absolu au fichier. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Ajoutez un fichier au travail. Répétez cet appel pour chaque fichier que vous souhaitez ajouter. Si plusieurs travaux utilisent *myDownloadJob* comme nom, vous devez remplacer *MYDOWNLOADJOB* par le GUID du travail pour identifier le travail de façon unique.

```
C:\>bitsadmin /addfile myDownloadJob http://downloadsrv/10mb.zip c:\10mb.zip
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)&copy;