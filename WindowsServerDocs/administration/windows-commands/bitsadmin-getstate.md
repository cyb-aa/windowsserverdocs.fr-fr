---
title: bitsadmin getstate
description: Rubrique de référence pour la commande Bitsadmin GetState, qui récupère l’état du travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1252d6cf-14ca-44df-beb2-930ff011f297
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ab014c96c6d5d62232243d704d41d33cfcfc50f0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717539"
---
# <a name="bitsadmin-getstate"></a>bitsadmin getstate

Récupère l’état du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getstate <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |

#### <a name="output"></a>Output

Les valeurs de sortie retournées peuvent être :

| State | Description |
| --------------- | ----------- |
| Mis en file d'attente. | Le travail est en attente d’exécution. |
| Connecting | BITS contacte le serveur. |
| Transferring | BITS transfère des données. |
| Port | Le service BITS a correctement transféré tous les fichiers du travail. |
| Interrompu | La tâche est suspendue. |
| Error | Une erreur non récupérable s’est produite. le transfert ne sera pas retenté. |
| Transient_Error | Une erreur récupérable s’est produite. le transfert réessaie lorsque le délai minimal entre deux tentatives expire. |
| Reconnu | Le travail est terminé. |
| Opération annulée | Le travail a été annulé. |

## <a name="examples"></a>Exemples

Pour récupérer l’état de la tâche nommée *myDownloadJob*:

```
bitsadmin /getstate myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
