---
title: bitsadmin getstate
description: Rubrique relative aux commandes Windows pour Bitsadmin GetState
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1252d6cf-14ca-44df-beb2-930ff011f297
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2cff790c8787b1514e8523a4583184d6f6a59efc
ms.sourcegitcommit: 51e0b575ef43cd16b2dab2db31c1d416e66eebe8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76259104"
---
# <a name="bitsadmin-getstate"></a>bitsadmin getstate


Récupère l’état du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetState <Job>
```

## <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
|    Tâche    | Nom complet ou GUID du travail |

## <a name="remarks"></a>Remarks

Les états possibles sont :

|      Région      | Description |
| --------------- | ----------- |
| EN attente          | Le travail est en attente d’exécution. |
| CONNEXION      | BITS contacte le serveur. |
| PORTE    | BITS transfère des données. |
| PORT     | Le service BITS a correctement transféré tous les fichiers du travail. |
| SUSPENDED       | La tâche est suspendue. |
| ERREUR           | Une erreur non récupérable s’est produite. le transfert ne sera pas retenté. |
| TRANSIENT_ERROR | Une erreur récupérable s’est produite. le transfert réessaie lorsque le délai minimal entre deux tentatives expire. |
| RECONNUES    | La tâche est terminée. |
| ANNULÉE        | Le travail a été annulé. |

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère l’état de la tâche nommée *myDownloadJob*.

```
C:\>bitsadmin /GetState myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
