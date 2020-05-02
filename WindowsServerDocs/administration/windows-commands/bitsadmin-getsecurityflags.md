---
title: bitsadmin getsecurityflags
description: Rubrique de référence pour la commande Bitsadmin getsecurityflags, qui signale les indicateurs de sécurité HTTP pour la redirection d’URL et les vérifications effectuées sur le certificat de serveur pendant le transfert.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c2e73519-34f4-487b-af11-97d5d08ef9bb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 41b710f9897f24eb4161d9379dc3b1f89b141472
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717549"
---
# <a name="bitsadmin-getsecurityflags"></a>bitsadmin getsecurityflags

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Signale les indicateurs de sécurité HTTP pour la redirection d’URL et les vérifications effectuées sur le certificat de serveur pendant le transfert.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getsecurityflags <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| travail | Nom complet ou GUID du travail. |

## <a name="examples"></a>Exemples

Pour récupérer les indicateurs de sécurité d’un travail nommé *myDownloadJob*:

```
bitsadmin /getsecurityflags myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
