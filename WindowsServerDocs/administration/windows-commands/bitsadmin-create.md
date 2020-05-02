---
title: bitsadmin create
description: Rubrique de référence pour la commande Bitsadmin Create, qui crée une tâche de transfert avec le nom complet donné.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9a8c53af-900b-4c24-9265-5b8b08213fac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 728027eb4680805c1f9a2afc32d8d37a14239597
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718218"
---
# <a name="bitsadmin-create"></a>bitsadmin create

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée une tâche de transfert avec le nom complet donné.

> [!NOTE]
> Les types de paramètres **/upload** et **/upload-reply** ne sont pas pris en charge par bits 1,2 et versions antérieures.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /create [type] displayname
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| ------- | -------- |
| type | Il existe trois types de travaux :<ul><li>**Downloader.** Transfère les données d’un serveur vers un fichier local.</li><li>**Amont.** Transfère les données d’un fichier local vers un serveur.</li><li>**/Upload-Reply.** Transfère les données d’un fichier local vers un serveur et reçoit un fichier de réponse du serveur.</li></ul>La valeur par défaut de ce paramètre est **/Download** si elle n’est pas spécifiée. |
| displayname | Nom complet affecté à la tâche nouvellement créée. |

## <a name="examples"></a>Exemples

Pour créer un travail de téléchargement nommé *myDownloadJob*:

```
bitsadmin /create myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin Resume](bitsadmin-resume.md)

- [commande Bitsadmin](bitsadmin.md)
