---
title: shadow
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f81d9717-6883-4e14-9508-4b2a87e48ea7 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 125b2971d5d4783ea0b974c45b988cbd1c58a2bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877110"
---
# <a name="shadow"></a>shadow

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous permet de contrôler à distance une session active d’un autre utilisateur sur un serveur hôte de Session Bureau à distance (hôte de Session Bureau à distance).
Pour obtenir des exemples montrant comment utiliser cette commande, consultez [exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe
```
shadow {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]
```

### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|\<SessionName>|Spécifie le nom de la session que vous souhaitez contrôler à distance.|
|\<SessionID>|Spécifie l’ID de la session que vous souhaitez contrôler à distance. Utilisez **utilisateur de la requête** pour afficher la liste des sessions et leur ID de session.|
|/ Server :\<nom_serveur >|Spécifie le serveur hôte de Session Bureau à distance qui contient la session que vous souhaitez contrôler à distance. Par défaut, le serveur rd Session Host4 actuel est utilisé.|
|/v|Affiche des informations sur les actions en cours d’exécution.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
-   Vous pouvez afficher ou contrôler activement la session. Si vous choisissez de contrôler activement la session d’un utilisateur, vous serez en mesure de clavier d’entrée et les actions de la souris à la session.
-   Vous pouvez toujours contrôler à distance vos propres sessions (à l’exception de la session active), mais vous devez disposer l’autorisation contrôle total ou l’autorisation d’accès spéciale de contrôle à distance pour contrôler à distance une autre session.
-   Vous pouvez également lancer le contrôle à distance à l’aide du Gestionnaire des Services Bureau à distance.
-   Avant le début de la surveillance, le serveur vous avertit l’utilisateur que la session va être contrôlée à distance, à moins que cet avertissement est désactivé. Votre session peut sembler être figé pendant quelques secondes, pendant qu’il attend une réponse de l’utilisateur. Pour configurer le contrôle à distance pour les utilisateurs et les sessions, utilisez l’outil de Configuration de Services Bureau à distance ou les extensions de Services Bureau à distance et les utilisateurs locaux et groupes active directory les utilisateurs et les ordinateurs.
-   Votre session doit être capable de prendre en charge la résolution vidéo utilisée lors de la session que vous contrôlez à distance ou de l’opération échoue.
-   La session de console peuvent ni contrôler à distance une autre session ni peut il être contrôlé à distance par une autre session.
-   Lorsque vous souhaitez mettre fin au contrôle à distance (occultation), appuyez sur CTRL + * (à l’aide de \* du pavé numérique uniquement).

## <a name="BKMK_examples"></a>Exemples
-   Pour observer la session 93, tapez :
    ```
    shadow 93
    ```
-   Pour observer la session ACCTG01, tapez :
    ```
    shadow ACCTG01
    ```

#### <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
[des Services Bureau à distance &#40;Services Terminal Server&#41; référence de la commande](remote-desktop-services-terminal-services-command-reference.md)
