---
title: change logon
description: Rubrique de référence pour la commande modifier l’ouverture de session, qui active ou désactive les ouvertures de session à partir des sessions clientes ou affiche l’état actuel de l’ouverture de session.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 41466260-aee9-4333-bcb6-178112c22afd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a2ebe75f6efa8c3bcfc0018d1f4e6051bb9ebb7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716130"
---
# <a name="change-logon"></a>change logon

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active ou désactive les ouvertures de session à partir des sessions clientes ou affiche l’état actuel de l’ouverture de session. Cet utilitaire est utile pour la maintenance du système. Vous devez être administrateur pour exécuter cette commande.

> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez [Nouveautés de services Bureau à distance dans Windows Server](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Syntaxe

```
change logon {/query | /enable | /disable | /drain | /drainuntilrestart}
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| Query | Affiche l’état actuel de l’ouverture de session, qu’il soit activé ou désactivé. |
| /Enable | Active les ouvertures de session à partir des sessions clientes, mais pas à partir de la console. |
| /Disable | Désactive les ouvertures de session suivantes à partir des sessions clientes, mais pas à partir de la console. N’affecte pas les utilisateurs actuellement connectés. |
| /drain | Désactive les ouvertures de session à partir de nouvelles sessions clientes, mais autorise les reconnexions aux sessions existantes. |
| /drainuntilrestart | Désactive les ouvertures de session à partir de nouvelles sessions client jusqu’à ce que l’ordinateur soit redémarré, mais autorise les reconnexions aux sessions existantes. |
| /? | Affiche l'aide à l'invite de commandes. |

#### <a name="remarks"></a>Notes 

- Les ouvertures de session sont réactivées lorsque vous redémarrez le système.

- Si vous êtes connecté au serveur hôte de session Bureau à distance à partir d’une session cliente et que vous désactivez les ouvertures de session et que vous vous déconnectez avant de réactiver les ouvertures de session, vous ne pourrez pas vous reconnecter à votre session. Pour réactiver les ouvertures de session à partir des sessions clientes, ouvrez une session sur la console.

### <a name="examples"></a>Exemples

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

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [modifier, commande](change.md)

- [Référence des commandes des services Bureau à distance (services Terminal Server)](remote-desktop-services-terminal-services-command-reference.md)
