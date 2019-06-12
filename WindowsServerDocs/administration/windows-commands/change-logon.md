---
title: change logon
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 45c171a1b14cf69abf039d57697cad933a2dd87b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434569"
---
>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="change-logon"></a>change logon
Active ou désactive les ouvertures de session des sessions clientes ou affiche l’état d’ouverture de session actuel.
Cet utilitaire est utile pour la maintenance du système.
Pour obtenir des exemples montrant comment utiliser cette commande, consultez [exemples](#BKMK_examples).
> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour savoir quelles sont les nouveautés dans la version la plus récente, consultez [les nouveautés des Services Bureau à distance dans Windows Server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.
> ## <a name="syntax"></a>Syntaxe
> ```
> change logon {/query | /enable | /disable | /drain | /drainuntilrestart}
> ```
> ## <a name="parameters"></a>Paramètres
> 
> |     Paramètre      |                                                       Description                                                        |
> |--------------------|--------------------------------------------------------------------------------------------------------------------------|
> |       /query       |                             Affiche l’état actuel de l’ouverture de session, activé ou désactivé.                              |
> |      /Enable       |                              Permet les connexions à partir de sessions clientes, mais pas à partir de la console.                              |
> |      /disable      |  Désactive les ouvertures de session suivantes à partir de sessions clientes, mais pas à partir de la console. N’affecte pas les utilisateurs actuellement connectés.   |
> |       /drain       |                 Désactive les ouvertures de session à partir de nouvelles sessions client, mais autorise les reconnexions aux sessions existantes.                 |
> | /drainuntilrestart | Désactive les ouvertures de session à partir de nouvelles sessions client jusqu'à ce que l’ordinateur est redémarré, mais autorise les reconnexions aux sessions existantes. |
> |         /?         |                                           Affiche l'aide à l'invite de commandes.                                           |
> 
> ## <a name="remarks"></a>Notes
> - Seuls les administrateurs peuvent utiliser le **changer d’ouverture de session** commande.
> - Ouvertures de session sont réactivées lorsque vous redémarrez le système. Si vous êtes connecté au serveur hôte de Session Bureau à distance (hôte de Session Bureau à distance) à partir d’une session cliente et désactivez les ouvertures de session et puis fermez la session avant de réactiver les ouvertures de session, vous ne serez pas en mesure de se reconnecter à votre session. Pour réactiver les ouvertures de session des sessions clientes, connectez-vous à la console.
>   ## <a name="BKMK_examples"></a>Exemples
> - Pour afficher l’état d’ouverture de session actuel, tapez :
>   ```
>   change logon /query
>   ```
> - Pour activer les ouvertures de session des sessions clientes, tapez :
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
>   [Services Bureau à distance &#40;Services Terminal Server&#41; référence des commandes](remote-desktop-services-terminal-services-command-reference.md)
