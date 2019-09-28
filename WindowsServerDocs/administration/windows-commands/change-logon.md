---
title: change logon
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 41466260-aee9-4333-bcb6-178112c22afd Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c04eaffe366dce079aed53351589c1b5026954e3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379641"
---
>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

# <a name="change-logon"></a>change logon
Active ou désactive les ouvertures de session à partir des sessions clientes ou affiche l’état actuel de l’ouverture de session.
Cet utilitaire est utile pour la maintenance du système.
Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_examples).
> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez les nouveautés [de services Bureau à distance dans Windows server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.
> ## <a name="syntax"></a>Syntaxe
> ```
> change logon {/query | /enable | /disable | /drain | /drainuntilrestart}
> ```
> ## <a name="parameters"></a>Paramètres
> 
> |     Paramètre      |                                                       Description                                                        |
> |--------------------|--------------------------------------------------------------------------------------------------------------------------|
> |       Query       |                             Affiche l’état actuel de l’ouverture de session, qu’il soit activé ou désactivé.                              |
> |      /Enable       |                              Active les ouvertures de session à partir des sessions clientes, mais pas à partir de la console.                              |
> |      /Disable      |  Désactive les ouvertures de session suivantes à partir des sessions clientes, mais pas à partir de la console. N’affecte pas les utilisateurs actuellement connectés.   |
> |       /drain       |                 Désactive les ouvertures de session à partir de nouvelles sessions clientes, mais autorise les reconnexions aux sessions existantes.                 |
> | /drainuntilrestart | Désactive les ouvertures de session à partir de nouvelles sessions client jusqu’à ce que l’ordinateur soit redémarré, mais autorise les reconnexions aux sessions existantes. |
> |         /?         |                                           Affiche l'aide à l'invite de commandes.                                           |
> 
> ## <a name="remarks"></a>Notes
> - Seuls les administrateurs peuvent utiliser la commande **modifier l’ouverture de session** .
> - Les ouvertures de session sont réactivées lorsque vous redémarrez le système. Si vous êtes connecté au serveur de l’hôte de session Bureau à distance (hôte de session Bureau à distance) à partir d’une session cliente et que vous désactivez les ouvertures de session, puis fermez la session avant de réactiver les ouvertures de session, vous ne pourrez pas vous reconnecter à votre session. Pour réactiver les ouvertures de session à partir des sessions clientes, ouvrez une session sur la console.
>   ## <a name="BKMK_examples"></a>Illustre
> - Pour afficher l’état actuel de l’ouverture de session, tapez :
>   ```
>   change logon /query
>   ```
> - Pour activer les ouvertures de session à partir des sessions clientes, tapez :
>   ```
>   change logon /enable
>   ```
> - Pour désactiver les ouvertures de session client, tapez :
>   ```
>   change logon /disable
>   ```
>   #### <a name="additional-references"></a>Références supplémentaires
>   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
>   [modifier](change.md)
>   [ &#40;services Bureau à distance&#41; référence de commande des services Terminal Server](remote-desktop-services-terminal-services-command-reference.md)
