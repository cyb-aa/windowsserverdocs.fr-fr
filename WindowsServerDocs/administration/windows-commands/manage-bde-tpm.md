---
title: gérer-TPM BDE
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 11a8530d-edd7-4fe3-ae81-b943766760fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: afb3859a1795959c90e71391b2926164165ef9ce
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724077"
---
# <a name="manage-bde-tpm"></a>Manage-bde : TPM

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
> 
> [!IMPORTANT]
> Cette commande n’est pas prise en charge pour une utilisation sur des ordinateurs exécutant Windows 8, Windows Server 2012 ou des systèmes d’exploitation ultérieurs. Pour ces ordinateurs, vous pouvez utiliser les [applets de commande de gestion du module de plateforme sécurisée pour Windows PowerShell](https://docs.microsoft.com/powershell/module/trustedplatformmodule/).
> Si vous utilisez cette commande sur un ordinateur exécutant Windows 7 ou Windows Server 2008, vous pouvez toujours configurer le Module de plateforme sécurisée (TPM) de l’ordinateur à l’aide de cette commande.
> ## <a name="syntax"></a>Syntaxe
> ```
> manage-bde -tpm [-turnon] [-takeownership <OwnerPassword>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
> ```
> #### <a name="parameters"></a>Paramètres
> 
> |    Paramètre    |                                                                              Description                                                                               |
> |-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |     -turnon     |              Active et active le module de plateforme sécurisée, ce qui permet de définir le mot de passe du propriétaire du module de plateforme sécurisée. Vous pouvez également utiliser **-t** comme version abrégée de cette commande.              |
> | -TakeOwnership  |                      Prend possession du module de plateforme sécurisée en définissant un mot de passe propriétaire. Vous pouvez également utiliser **-o** comme version abrégée de cette commande.                       |
> | <OwnerPassword> |                                                      Représente le mot de passe propriétaire que vous spécifiez pour le module de plateforme sécurisée.                                                       |
> |  -ComputerName  | Spécifie que Manage-bde. exe sera utilisé pour modifier la protection BitLocker sur un autre ordinateur. Vous pouvez également utiliser **-CN** comme version abrégée de cette commande. |
> |     <Name>      |    Représente le nom de l’ordinateur sur lequel modifier la protection BitLocker. Les valeurs acceptées incluent le nom NetBIOS de l’ordinateur et l’adresse IP de l’ordinateur.     |
> |    -? ou /?     |                                                               Affiche une brève aide à l’invite de commandes.                                                               |
> |   -Help ou-h   |                                                             Affiche l’aide complète à l’invite de commandes.                                                              |
> 
> ## <a name="examples"></a>Exemples
> Pour illustrer l’utilisation de la commande **-TPM** pour activer le module de plateforme sécurisée.
> ```
> manage-bde  tpm -turnon
> ```
> Pour illustrer l’utilisation de la commande **TPM** pour prendre possession du module de plateforme sécurisée et 0wnerP@ssdéfinir le mot de passe du propriétaire sur.
> ```
> manage-bde  tpm  takeownership 0wnerP@ss
> ```
> ## <a name="additional-references"></a>Références supplémentaires
> -   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
> -   [manage-bde](manage-bde.md)
