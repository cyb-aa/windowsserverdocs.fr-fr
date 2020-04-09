---
title: Bitsadmin GetClientCertificate
description: La rubrique commandes Windows pour **Bitsadmin GetClientCertificate**, qui récupère le certificat client à partir du travail.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4fc8f408-085e-43a0-9fa8-3d798ef107b1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7c29d5c64fd172cfdd2d5d93c5ed22d519077806
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850762"
---
# <a name="bitsadmin-getclientcertificate"></a>Bitsadmin GetClientCertificate

Récupère le certificat client du travail.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getclientcertificate <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère le certificat client pour le travail nommé *myDownloadJob*.

```
C:\>bitsadmin /getclientcertificate myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)