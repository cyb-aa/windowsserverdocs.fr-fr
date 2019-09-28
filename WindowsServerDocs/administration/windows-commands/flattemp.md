---
title: flattemp
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 059a0960-1fd9-4382-87fe-a85d5dccdaea
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b1a458e8742ca354eeca821e93590386bca56dff
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377128"
---
# <a name="flattemp"></a>flattemp

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Active ou désactive les dossiers temporaires plats.
Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_examples).

> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour découvrir les nouveautés de la dernière version, consultez les nouveautés [de services Bureau à distance dans Windows server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.

## <a name="syntax"></a>Syntaxe
```
flattemp {/query | /enable | /disable}
```

## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|Query|Interroge le paramètre actuel.|
|/Enable|Active les dossiers temporaires plats. Les utilisateurs partageront le dossier temporaire, sauf si le dossier temporaire se trouve dans le dossier de démarrage de l’utilisateur.|
|/Disable|Désactive les dossiers temporaires plats. Chaque dossier temporaire de l’utilisateur se trouve dans un dossier distinct (déterminé par l’ID de session de l’utilisateur).|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
-   La commande **flattemp** n’est disponible que si vous avez installé le service de rôle Terminal Server sur un ordinateur exécutant windows Server 2008 ou le service de rôle hôte de session Bureau à distance sur un ordinateur exécutant windows Server 2008 R2.
-   Vous devez disposer d’informations d’identification d’administration pour exécuter **flattemp**.
-   Une fois que chaque utilisateur dispose d’un dossier temporaire unique, utilisez **flattemp/Enable** pour activer les dossiers temporaires plats.
-   La méthode par défaut de création de dossiers temporaires pour plusieurs utilisateurs (généralement désignée par les variables d’environnement TEMP et TMP) consiste à créer des sous-dossiers dans le dossier **\temp** , en utilisant logonID comme nom de sous-dossier. Par exemple, si la variable d’environnement TEMP pointe vers C:\Temp, le dossier temporaire affecté à l’utilisateur logonID 4 est C:\Temp\4. À l’aide de **flattemp**, vous pouvez pointer directement vers le dossier \temp et empêcher les sous-dossiers de former. Cela est utile lorsque vous souhaitez que les dossiers temporaires de l’utilisateur soient contenus dans les dossiers de démarrage, qu’il s’agisse d’un lecteur local du serveur hôte de session Bureau à distance ou d’un lecteur réseau partagé. Vous devez utiliser la commande **flattemp/Enable** uniquement lorsque chaque utilisateur dispose d’un dossier temporaire distinct.
-   Vous pouvez rencontrer des erreurs d’application si le dossier temporaire de l’utilisateur se trouve sur un lecteur réseau. Cela se produit lorsque le lecteur réseau partagé est momentanément inaccessible sur le réseau. Étant donné que les fichiers temporaires de l’application sont inaccessibles ou ne sont pas synchronisés, ils répondent comme si le disque s’est arrêté. Le déplacement du dossier temporaire sur un lecteur réseau n’est pas recommandé. La valeur par défaut consiste à conserver les dossiers temporaires sur le disque dur local. Si vous rencontrez des erreurs de comportement ou d’altération du disque inattendues avec certaines applications, stabilisez votre réseau ou replacez les dossiers temporaires sur le disque dur local.
-   Si vous désactivez l’utilisation de dossiers temporaires distincts par session, les paramètres **flattemp** sont ignorés. Cette option est définie dans l’outil de configuration Services Bureau à distance.

## <a name="BKMK_examples"></a>Illustre
-   Pour afficher le paramètre actuel des dossiers temporaires plats, tapez :
    ```
    flattemp /query
    ```
-   Pour activer les dossiers temporaires plats, tapez :
    ```
    flattemp /enable
    ```
-   Pour désactiver les dossiers temporaires plats, tapez :
    ```
    flattemp /disable
    ```

## <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[Services Bureau à distance &#40;la référence&#41; des commandes des services Terminal Server](remote-desktop-services-terminal-services-command-reference.md)
