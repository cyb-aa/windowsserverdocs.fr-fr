---
title: change logon
description: La rubrique commandes Windows pour la modification de l’ouverture de session, qui active ou désactive les ouvertures de session des sessions clientes, ou affiche l’état actuel de l’ouverture de session.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 41466260-aee9-4333-bcb6-178112c22afd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a101fc567981716536ecad8e754b81a43eb2c91
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848152"
---
# <a name="change-logon"></a>change logon

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active ou désactive les ouvertures de session à partir des sessions clientes ou affiche l’état actuel de l’ouverture de session. Cet utilitaire est utile pour la maintenance du système.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

> [!NOTE]
> Sous Windows Server 2008 R2, les services Terminal Server sont appelés Services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez les nouveautés [de services Bureau à distance dans Windows server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.

## <a name="syntax"></a>Syntaxe
```
change logon {/query | /enable | /disable | /drain | /drainuntilrestart}
```
### <a name="parameters"></a>Paramètres

|     Paramètre      |                                                       Description                                                        |
|--------------------|--------------------------------------------------------------------------------------------------------------------------|
|       Query       |                             Affiche l’état actuel de l’ouverture de session, qu’il soit activé ou désactivé.                              |
|      /Enable       |                              Active les ouvertures de session à partir des sessions clientes, mais pas à partir de la console.                              |
|      /Disable      |  Désactive les ouvertures de session suivantes à partir des sessions clientes, mais pas à partir de la console. N’affecte pas les utilisateurs actuellement connectés.   |
|       /drain       |                 Désactive les ouvertures de session à partir de nouvelles sessions clientes, mais autorise les reconnexions aux sessions existantes.                 |
| /drainuntilrestart | Désactive les ouvertures de session à partir de nouvelles sessions client jusqu’à ce que l’ordinateur soit redémarré, mais autorise les reconnexions aux sessions existantes. |
|         /?         |                                           Affiche l'aide à l'invite de commandes.                                           |

## <a name="remarks"></a>Notes
- Seuls les administrateurs peuvent utiliser la commande **modifier l’ouverture de session** .
- Les ouvertures de session sont réactivées lorsque vous redémarrez le système. Si vous êtes connecté au serveur de l’hôte de session Bureau à distance (hôte de session Bureau à distance) à partir d’une session cliente et que vous désactivez les ouvertures de session, puis fermez la session avant de réactiver les ouvertures de session, vous ne pourrez pas vous reconnecter à votre session. Pour réactiver les ouvertures de session à partir des sessions clientes, ouvrez une session sur la console.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

- Pour afficher l’état actuel de l’ouverture de session, tapez :
  ```
  change logon /query
  ```
- Pour activer les ouvertures de session à partir des sessions clientes, tapez :
  ```
  change logon /enable
  ```
- Pour désactiver les ouvertures de session client, tapez :
  ```
  change logon /disable
  ```
  
## <a name="additional-references"></a>Références supplémentaires
- - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
- [change](change.md)
- [Référence des commandes des services Bureau à distance (services Terminal Server)](remote-desktop-services-terminal-services-command-reference.md)
