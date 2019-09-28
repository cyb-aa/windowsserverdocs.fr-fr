---
title: bitsadmin getstate
description: 'Rubrique relative aux commandes Windows pour * * * *- '
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
ms.openlocfilehash: 55be37a6b1b44b81ed9002e5e3b9eb1fd46bd0dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381232"
---
# <a name="bitsadmin-getstate"></a>bitsadmin getstate



Récupère l’état du travail spécifié.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetState <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="remarks"></a>Notes

Les États possibles sont les suivants :

|-----|-----| | En file d’attente | Le travail est en attente d’exécution. | | CONNEXION en | BITS contacte le serveur. | | TRANSFERT en | BITS transfère des données. | | SUSPENDU | Le travail est en pause. | | ERREUR | Une erreur non récupérable s’est produite. le transfert ne sera pas retenté. | | TRANSIENT_ERROR | Une erreur récupérable s’est produite. le transfert réessaie lorsque le délai minimal entre deux tentatives expire. | | ACCUSÉ de réception | La tâche est terminée. | | ANNULÉ | Le travail a été annulé. |

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant récupère l’état de la tâche nommée *myDownloadJob*.
```
C:\>bitsadmin /GetState myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)