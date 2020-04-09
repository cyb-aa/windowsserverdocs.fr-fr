---
title: bitsadmin getreplyprogress
description: La rubrique commandes Windows pour **Bitsadmin getreplyprogress**, qui récupère la taille et la progression de la réponse de chargement du serveur.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7f7cb0b4-ad95-44fd-a35d-0ddf5fc0b0d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 195ed669817bc0aca7ebc432e7f3c66ab1548162
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850482"
---
# <a name="bitsadmin-getreplyprogress"></a>bitsadmin getreplyprogress

Récupère la taille et la progression de la réponse de chargement du serveur.

> [!NOTE]
> Cette commande n’est pas prise en charge par BITS 1,2 et versions antérieures.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getreplyprogress <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |


## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère la progression de la réponse de chargement pour la tâche nommée *myDownloadJob*.

```
C:\>bitsadmin /getreplyprogress myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)