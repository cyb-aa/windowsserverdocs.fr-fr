---
title: gérer-bde tpm
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 11a8530d-edd7-4fe3-ae81-b943766760fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bc3cfa583866335d214282be08366854dec77d0f
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280006"
---
# <a name="manage-bde-tpm"></a>gérer-bde : module de plateforme sécurisée

> S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
> 
> [!IMPORTANT]
> Cette commande n’est pas pris en charge pour une utilisation sur les ordinateurs exécutant Windows 8, Windows Server 2012 ou des systèmes d’exploitation ultérieurs. Pour ces ordinateurs, vous pouvez utiliser la [applets de commande de gestion du module de plateforme sécurisée pour Windows PowerShell](https://docs.microsoft.com/powershell/module/trustedplatformmodule/).
> Si vous utilisez cette commande sur l’ordinateur qui exécute Windows 7 ou Windows Server 2008, vous pouvez toujours configurer Trusted Platform Module (TPM l’ordinateur) à l’aide de cette commande. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).
> ## <a name="syntax"></a>Syntaxe
> ```
> manage-bde -tpm [-turnon] [-takeownership <OwnerPassword>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
> ```
> ### <a name="parameters"></a>Paramètres
> 
> |    Paramètre    |                                                                              Description                                                                               |
> |-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |     -Activer     |              Active et Active le module TPM, le mot de passe du propriétaire TPM à définir. Vous pouvez également utiliser **-t** comme une version abrégée de cette commande.              |
> | -takeownership  |                      Prend possession du module TPM en définissant un mot de passe de propriétaire. Vous pouvez également utiliser **-o** comme une version abrégée de cette commande.                       |
> | <OwnerPassword> |                                                      Représente le mot de passe de propriétaire que vous spécifiez pour le module de plateforme sécurisée.                                                       |
> |  -computername  | Spécifie que bde.exe gérer permet de modifier la protection BitLocker sur un autre ordinateur. Vous pouvez également utiliser **- cn** comme une version abrégée de cette commande. |
> |     <Name>      |    Représente le nom de l’ordinateur sur lequel modifier la protection BitLocker. Valeurs acceptées incluent le nom NetBIOS de l’ordinateur et l’adresse IP de l’ordinateur.     |
> |    -? ou /?     |                                                               Affiche un résumé aide à l’invite de commandes.                                                               |
> |   -help ou-h   |                                                             Affiche une aide à l’invite de commande complète.                                                              |
> 
> ## <a name="BKMK_Examples"></a>Exemples
> L’exemple suivant illustre l’utilisation de la **- tpm** commande pour activer le TPM.
> ```
> manage-bde  tpm -turnon
> ```
> L’exemple suivant illustre l’utilisation de la **tpm** commande pour prendre possession du module TPM et de définir le mot de passe de propriétaire sur 0wnerP@ss.
> ```
> manage-bde  tpm  takeownership 0wnerP@ss
> ```
> ## <a name="additional-references"></a>Références supplémentaires
> -   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
> -   [manage-bde](manage-bde.md)
