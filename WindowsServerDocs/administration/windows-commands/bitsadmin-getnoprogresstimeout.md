---
title: bitsadmin getnoprogresstimeout
description: La rubrique commandes Windows pour **Bitsadmin getnoprogresstimeout**, qui récupère la durée, en secondes, pendant laquelle le service essaiera de transférer le fichier après une erreur temporaire se produit.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9cd9b19b-cbb4-4352-8419-978080f016b6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cf2cfd77b494e221b60c8816ff46eed5f9252f39
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850602"
---
# <a name="bitsadmin-getnoprogresstimeout"></a>bitsadmin getnoprogresstimeout

Récupère la durée, en secondes, pendant laquelle le service tente de transférer le fichier après qu’une erreur temporaire s’est produite.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getnoprogresstimeout <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère la valeur de délai d’attente de progression pour le travail nommé *myDownloadJob*.

```
C:\>bitsadmin /getnoprogresstimeout myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)