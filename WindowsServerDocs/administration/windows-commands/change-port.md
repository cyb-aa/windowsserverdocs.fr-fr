---
title: change port
description: Rubrique de référence pour la commande changer de port, qui répertorie ou modifie les mappages de port COM pour qu’ils soient compatibles avec les applications MS-DOS.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3d772c90-e849-4e74-b9ec-b6cae1159336 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8dcf1097ea037aff9269edafea6e640054a697e3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716076"
---
# <a name="change-port"></a>change port

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Répertorie ou modifie les mappages de port COM pour qu’ils soient compatibles avec les applications MS-DOS.

> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez [Nouveautés de services Bureau à distance dans Windows Server](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Syntaxe

```
change port [<portX>=<portY| /d <portX | /query]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
|-----------------|----------------------------------------|
| <portX>=<portY> | Mappe COM `<*portX*>` à`<*portY*>` |
| /d<portX> | Supprime le mappage pour COM.`<*portX*>` |
| Query | Affiche les mappages de port actuels. |
| /? | Affiche l'aide à l'invite de commandes. |

#### <a name="remarks"></a>Notes 

- La plupart des applications MS-DOS prennent uniquement en charge les ports série COM1 à COM4. La commande de **changement de port** mappe un port série à un numéro de port différent, ce qui permet aux applications qui ne prennent pas en charge les ports com à numéros élevés d’accéder au port série. Le remappage fonctionne uniquement pour la session active et n’est pas conservé si vous vous déconnectez d’une session et que vous vous reconnectez.

- Utilisez **modifier le port** sans aucun paramètre pour afficher les ports com disponibles et leurs mappages actuels.

## <a name="examples"></a>Exemples

- Pour mapper COM12 à COM1 pour une utilisation par une application MS-DOS, tapez :
  
  ```
  change port com12=com1
  ```

- Pour afficher les mappages de port actuels, tapez :
  
  ```
  change port /query
  ```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [modifier, commande](change.md)

- [Référence des commandes des services Bureau à distance (services Terminal Server)](remote-desktop-services-terminal-services-command-reference.md)
