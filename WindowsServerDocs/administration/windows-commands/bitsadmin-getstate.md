---
title: bitsadmin getstate
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f7ed7529fda264efaceb6b4b36e36e728c211f3f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889620"
---
# <a name="bitsadmin-getstate"></a>bitsadmin getstate



Récupère l’état de la tâche spécifiée.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetState <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="remarks"></a>Notes

Les états possibles sont :

|-----|-----| | EN FILE D’ATTENTE | La tâche est en attente d’exécution. | | CONNEXION | BITS contacte le serveur. | | TRANSFERT | BITS transfère des données. | | SUSPENDU | Le travail est suspendu. | | ERREUR | Une erreur non récupérable s’est produite ; le transfert ne sera pas relancé. | | TRANSIENT_ERROR | Une erreur récupérable s’est produite ; le transfert tente à nouveau lorsque le délai minimum de nouvelle tentative expire. | | ACCUSÉ DE RÉCEPTION | La tâche a été terminée. | | ANNULÉ | Le travail a été annulé. |

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère l’état du travail *myDownloadJob*.
```
C:\>bitsadmin /GetState myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)