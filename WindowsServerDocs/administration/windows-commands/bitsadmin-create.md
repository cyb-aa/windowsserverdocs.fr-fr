---
title: bitsadmin create
description: Rubrique relative aux commandes Windows pour **Bitsadmin Create**, qui crée une tâche de transfert avec le nom complet donné.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9a8c53af-900b-4c24-9265-5b8b08213fac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a922d9f15aff0a9bd064a7e987920adf3a9107d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850812"
---
# <a name="bitsadmin-create"></a>bitsadmin create

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée une tâche de transfert avec le nom complet donné. Les travaux de téléchargement transfèrent les données d’un serveur vers un fichier local. Les travaux de chargement transfèrent les données d’un fichier local vers un serveur. Les travaux de chargement-réponse transfèrent les données d’un fichier local vers un serveur et reçoivent un fichier de réponse du serveur.

Utilisez le commutateur [Bitsadmin Resume](bitsadmin-resume.md) pour activer le travail dans la file d’attente de transfert.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /create [type] displayname
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| ------- | -------- |
| type | -  **/Download** transfère les données d’un serveur vers un fichier local.<p>-  **/upload** transfère les données d’un fichier local vers un serveur.<p>-  **/upload-reply** transfère les données d’un fichier local vers un serveur et reçoit un fichier de réponse du serveur.<p>Par défaut, ce paramètre est défini sur **/Download** lorsqu’il n’est pas spécifié sur la ligne de commande. En outre, les types de  **/Upload** et **/upload-reply** ne sont pas disponibles en bits 1,2 et versions antérieures. |
| displayname | Nom complet affecté à la tâche nouvellement créée. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Crée une tâche de téléchargement nommée *myDownloadJob*.

```
C:\>bitsadmin /create myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
