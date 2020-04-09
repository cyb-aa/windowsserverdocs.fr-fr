---
title: tsdiscon
description: La rubrique commandes Windows pour tsdiscon, qui déconnecte une session d’un serveur hôte de session Bureau à distance.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 13139674-7dee-4965-8cac-32f4928e8b9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b008fa920290043b64e7421e91a545123634f1e7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832512"
---
# <a name="tsdiscon"></a>tsdiscon

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Déconnecte une session d’un serveur hôte de session Bureau à distance.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

> [!NOTE]
> Sous Windows Server 2008 R2, les services Terminal Server sont appelés Services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez les nouveautés [de services Bureau à distance dans Windows server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.

## <a name="syntax"></a>Syntaxe
```
tsdiscon [<SessionID> | <SessionName>] [/server:<ServerName>] [/v]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|\<SessionId >|Spécifie l’ID de la session à déconnecter.|
|\<NomSession >|Spécifie le nom de la session à déconnecter.|
|/Server :\<ServerName >|Spécifie le serveur Terminal Server qui contient la session que vous souhaitez déconnecter. Dans le cas contraire, le serveur hôte de session Bureau à distance actuel est utilisé.|
|/v|Affiche des informations sur les actions en cours d’exécution.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
-   Vous devez disposer de l’autorisation contrôle total ou déconnecter l’autorisation accès spécial pour déconnecter un autre utilisateur d’une session.
-   Si aucun ID de session ou nom de session n’est spécifié, **tsdiscon** déconnecte la session active.
-   Toutes les applications qui étaient en cours d’exécution lors de la déconnexion de la session sont exécutées automatiquement lorsque vous vous reconnectez à cette session sans perte de données. Utilisez la **session de réinitialisation** pour mettre fin aux applications en cours d’exécution de la session déconnectée, mais sachez que cela peut entraîner une perte de données au niveau de la session.
-   Le paramètre **/Server** est requis uniquement si vous utilisez **tsdiscon** à partir d’un serveur distant.
-   Impossible de déconnecter la session de la console.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre
- Pour déconnecter la session active, tapez :
  ```
  tsdiscon
  ```
- Pour déconnecter la session 10, tapez :
  ```
  tsdiscon 10
  ```
- Pour déconnecter la session nommée TERM04, tapez :
  ```
  tsdiscon TERM04
  ```
  ## <a name="additional-references"></a>Références supplémentaires
  - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
  la [référence de commande services Bureau à distance (services Terminal Server)](remote-desktop-services-terminal-services-command-reference.md)
