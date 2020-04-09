---
title: gérer-TPM BDE
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 11a8530d-edd7-4fe3-ae81-b943766760fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b6495bfbfedea7219ae175145f72fc12314ce7ae
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839762"
---
# <a name="manage-bde-tpm"></a>Manage-bde : TPM

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
> 
> [!IMPORTANT]
> Cette commande n’est pas prise en charge pour une utilisation sur des ordinateurs exécutant Windows 8, Windows Server 2012 ou des systèmes d’exploitation ultérieurs. Pour ces ordinateurs, vous pouvez utiliser les [applets de commande de gestion du module de plateforme sécurisée pour Windows PowerShell](https://docs.microsoft.com/powershell/module/trustedplatformmodule/).
> Si vous utilisez cette commande sur un ordinateur exécutant Windows 7 ou Windows Server 2008, vous pouvez toujours configurer le Module de plateforme sécurisée (TPM) de l’ordinateur à l’aide de cette commande. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).
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
> ## <a name="examples"></a><a name=BKMK_Examples></a>Illustre
> L’exemple suivant illustre l’utilisation de la commande **-TPM** pour activer le module de plateforme sécurisée.
> ```
> manage-bde  tpm -turnon
> ```
> L’exemple suivant illustre l’utilisation de la commande **TPM** pour prendre possession du module de plateforme sécurisée et définir le mot de passe du propriétaire sur 0wnerP@ss.
> ```
> manage-bde  tpm  takeownership 0wnerP@ss
> ```
> ## <a name="additional-references"></a>Références supplémentaires
> -   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
> -   [manage-bde](manage-bde.md)
