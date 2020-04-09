---
title: bitsadmin complete
description: La rubrique commandes Windows pour **Bitsadmin est terminée**, ce qui termine la tâche.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a5e86677-8f7b-43b3-929e-97706c57e7f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 847f6e298ff9701064ce4e577c785f7fc78ea22c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850822"
---
# <a name="bitsadmin-complete"></a>bitsadmin complete

Termine le travail. Les fichiers téléchargés ne sont pas disponibles tant que vous n’utilisez pas ce commutateur. Utilisez ce commutateur après que le travail passe à l’état transféré. Dans le cas contraire, seuls les fichiers qui ont été transférés avec succès sont disponibles.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /complete <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Lorsque l’état du travail est transféré, le service BITS a transféré tous les fichiers du travail. Toutefois, les fichiers ne sont pas disponibles tant que vous n’utilisez pas le commutateur **/Complete** 

Si plusieurs travaux utilisent *myDownloadJob* comme nom, vous devez remplacer *MYDOWNLOADJOB* par le GUID du travail pour identifier le travail de façon unique.

```
C:\>bitsadmin /complete myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)