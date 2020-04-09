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
ms.openlocfilehash: 38cf00dc48ff036e7d710fb7436f00fd9381dd0b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849872"
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

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant supprime le certificat client du travail nommé *myDownloadJob*.

```
C:\>bitsadmin /removeclientcertificate myDownloadJob 
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)