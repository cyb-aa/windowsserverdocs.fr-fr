---
title: Bitsadmin getsecurityflags
description: La rubrique commandes Windows pour **Bitsadmin getsecurityflags**, qui signale les indicateurs de sécurité http pour la redirection d’URL et les vérifications effectuées sur le certificat de serveur pendant le transfert.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c2e73519-34f4-487b-af11-97d5d08ef9bb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 360f8d5514e5251dd9e4a6a6b60335dc34fe4415
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850472"
---
# <a name="bitsadmin-getsecurityflags"></a>Bitsadmin getsecurityflags

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Signale les indicateurs de sécurité HTTP pour la redirection d’URL et les vérifications effectuées sur le certificat de serveur pendant le transfert.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getsecurityflags <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| -------------- | -------------- |
| le travail | Nom complet ou GUID du travail. |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère les indicateurs de sécurité d’un travail nommé *myDownloadJob*.

```
C:\>bitsadmin /getsecurityflags myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)