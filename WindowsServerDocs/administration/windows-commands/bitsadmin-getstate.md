---
title: bitsadmin getstate
description: La rubrique commandes Windows pour **Bitsadmin GetState**, qui récupère l’état du travail spécifié.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1252d6cf-14ca-44df-beb2-930ff011f297
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 43cd8c8e614cce65f55b16fc5395b1d37de0cf95
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850462"
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
| le travail | Nom complet ou GUID du travail. |

## <a name="output"></a>Sortie

Les valeurs de sortie sont les suivantes :

| État | Description |
| --------------- | ----------- |
| Mis en file d'attente | Le travail est en attente d’exécution. |
| Connexion en cours | BITS contacte le serveur. |
| Porte | BITS transfère des données. |
| Port | Le service BITS a correctement transféré tous les fichiers du travail. |
| Suspended | La tâche est suspendue. |
| Error | Une erreur non récupérable s’est produite. le transfert ne sera pas retenté. |
| Transient_Error | Une erreur récupérable s’est produite. le transfert réessaie lorsque le délai minimal entre deux tentatives expire. |
| Un accusé | Le travail est terminé. |
| Canceled | Le travail a été annulé. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère l’état de la tâche nommée *myDownloadJob*.

```
C:\>bitsadmin /getstate myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
