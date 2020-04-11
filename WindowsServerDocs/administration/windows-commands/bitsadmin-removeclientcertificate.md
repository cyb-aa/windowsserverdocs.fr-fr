---
title: Bitsadmin removeclientcertificate
description: Rubrique relative aux commandes Windows pour **Bitsadmin removeclientcertificate**, qui supprime le certificat client du travail.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b417c3e5-aadd-4fcc-968f-45d8b67ca516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 312226b73b91385436e15c4afbb49df161258768
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123105"
---
# <a name="bitsadmin-removeclientcertificate"></a>Bitsadmin removeclientcertificate

Supprime le certificat client du travail.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /removeclientcertificate <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a>Exemples

L’exemple suivant supprime le certificat client du travail nommé *myDownloadJob*.

```
C:\>bitsadmin /removeclientcertificate myDownloadJob 
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)