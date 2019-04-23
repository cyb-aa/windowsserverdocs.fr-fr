---
title: tsdiscon
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 13139674-7dee-4965-8cac-32f4928e8b9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2033cbc9b3d9127249656c3e0dcf95d872229797
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842540"
---
# <a name="tsdiscon"></a>tsdiscon

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Déconnecte une session à partir d’un serveur hôte de Session Bureau à distance (hôte de Session Bureau à distance).
Pour obtenir des exemples montrant comment utiliser cette commande, consultez [exemples](#BKMK_examples).

> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour savoir quelles sont les nouveautés dans la version la plus récente, consultez [les nouveautés des Services Bureau à distance dans Windows Server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.

## <a name="syntax"></a>Syntaxe
```
tsdiscon [<SessionID> | <SessionName>] [/server:<ServerName>] [/v]
```

## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|\<SessionId>|Spécifie l’ID de la session à déconnecter.|
|\<SessionName>|Spécifie le nom de la session à déconnecter.|
|/ Server :\<nom_serveur >|Spécifie le serveur terminal server qui contient la session que vous souhaitez déconnecter. Sinon, le serveur hôte de Session Bureau à distance en cours est utilisé.|
|/v|Affiche des informations sur les actions en cours d’exécution.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
-   Vous devez avoir l’autorisation contrôle total ou déconnecter l’autorisation d’accès spécial pour déconnecter un autre utilisateur d’une session.
-   Si aucun ID de session ou le nom de la session n’est spécifié, **tsdiscon** déconnecte la session en cours.
-   Toutes les applications qui étaient en cours d’exécution lorsque vous a déconnecté la session sont exécutent automatiquement lorsque vous vous reconnectez à la session sans perte de données. Utilisez **réinitialiser la session** pour mettre fin à l’exécution des applications de la session déconnectée, mais sachez que cela peut provoquer une perte de données lors de la session.
-   Le **/server** paramètre est obligatoire uniquement si vous utilisez **tsdiscon** à partir d’un serveur distant.
-   La session de console ne peut pas être déconnectée.

## <a name="BKMK_examples"></a>Exemples
-   Pour déconnecter la session active, tapez :
    ```
    tsdiscon
    ```
-   Pour déconnecter la session 10, tapez :
    ```
    tsdiscon 10
    ```
-   Pour déconnecter la session nommée TERM04, tapez :
    ```
    tsdiscon TERM04
    ```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
[des Services Bureau à distance &#40;Services Terminal Server&#41; référence de la commande](remote-desktop-services-terminal-services-command-reference.md)
