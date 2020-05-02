---
title: tskill
description: Rubrique de référence pour tskill, qui met fin à un processus s’exécutant dans une session sur un serveur hôte de session Bureau à distance.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 08986e6a-6900-4ece-85a1-8f73b14db1b3 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 13bd18a84dccbbeee88c24b9b07208b3174bc558
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721242"
---
# <a name="tskill"></a>tskill

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Met fin à un processus en cours d’exécution dans une session sur un serveur hôte de session Bureau à distance.


> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez les nouveautés [de services Bureau à distance dans Windows server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.

## <a name="syntax"></a>Syntaxe
```
tskill {<ProcessID> | <ProcessName>} [/server:<ServerName>] [/id:<SessionID> | /a] [/v]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|\<ID de l'>|Spécifie l’ID du processus que vous souhaitez terminer.|
|\<> ProcessName|Spécifie le nom du processus que vous souhaitez terminer. Ce paramètre peut inclure des caractères génériques.|
|/Server :\<NomServeur>|Spécifie le serveur Terminal Server qui contient le processus que vous souhaitez terminer. Si **/Server** n’est pas spécifié, le serveur hôte de session Bureau à distance actuel est utilisé.|
|/ID :\<SessionID>|Termine le processus en cours d’exécution dans la session spécifiée.|
|/a|Met fin au processus qui s’exécute dans toutes les sessions.|
|/v|Affiche des informations sur les actions en cours d’exécution.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes 
- Vous pouvez utiliser **tskill** pour mettre fin uniquement aux processus qui vous appartiennent, sauf si vous êtes un administrateur. Les administrateurs ont un accès complet à toutes les fonctions **tskill** et peuvent terminer les processus qui s’exécutent dans d’autres sessions utilisateur.
- Lorsque tous les processus qui s’exécutent dans une session se terminent, la session se termine également.
- Si vous utilisez les paramètres *ProcessName* et **/Server :**<em>ServerName</em> , vous devez également spécifier le paramètre **/ID :**<em>SessionID</em> ou **/a** .

## <a name="examples"></a>Exemples
- Pour terminer le processus 6543, tapez :
  ```
  tskill 6543
  ```
- Pour terminer l’Explorateur de processus en cours d’exécution sur la session 5, tapez :
  ```
  tskill explorer /id:5
  ```
  ## <a name="additional-references"></a>Références supplémentaires
  - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  [services Bureau à distance la référence de commande (services Terminal Server)](remote-desktop-services-terminal-services-command-reference.md)
