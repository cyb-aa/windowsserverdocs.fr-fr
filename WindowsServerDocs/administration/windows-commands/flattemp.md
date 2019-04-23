---
title: flattemp
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3fc14a6fe1a355f7c20c130fba3fb1f17e49b6f1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872930"
---
# <a name="flattemp"></a>flattemp

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active ou désactive les dossiers temporaires communs.
Pour obtenir des exemples montrant comment utiliser cette commande, consultez [exemples](#BKMK_examples).

> [!NOTE]
> Dans Windows Server 2008 R2, les services Terminal Server ont été renommés services Bureau à distance. Pour savoir quelles sont les nouveautés dans la version la plus récente, consultez [les nouveautés des Services Bureau à distance dans Windows Server 2012](https://technet.microsoft.com/library/hh831527) dans la bibliothèque TechNet de Windows Server.

## <a name="syntax"></a>Syntaxe
```
flattemp {/query | /enable | /disable}
```

## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/query|Interroge le paramètre actuel.|
|/Enable|Active les dossiers temporaires communs. Les utilisateurs partagent le dossier temporaire, sauf si le dossier temporaire se trouve dans le dossier de base utilisateur s.|
|/disable|Désactive les dossiers temporaires communs. Chaque dossier temporaire de s utilisateur réside dans un dossier distinct (déterminé par l’utilisateur s ID de Session).|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
-   Le **flattemp** commande est disponible uniquement lorsque vous avez installé le service de rôle Terminal Server sur un ordinateur exécutant Windows Server 2008 ou le service de rôle hôte de Session de bureau à distance sur un ordinateur exécutant Windows Server 2008 R2.
-   Vous devez disposer des informations d’identification administratives pour exécuter **flattemp**.
-   Une fois que chaque utilisateur dispose d’un dossier temporaire unique, utilisez **flattemp /enable** pour activer les dossiers temporaires communs.
-   La méthode par défaut pour la création de dossiers temporaires pour plusieurs utilisateurs (généralement désignés par les variables d’environnement TEMP et TMP) consiste à créer des sous-dossiers dans le **\Temp** dossier, en utilisant le numéro de session en tant que le nom du sous-dossier. Par exemple, si la variable d’environnement TEMP pointe vers C:\Temp, le dossier temporaire affecté pour le numéro de session utilisateur 4 est C:\Temp\4. À l’aide de **flattemp**, vous pouvez pointer directement vers le dossier \Temp et empêcher la formation de sous-dossiers. Cela est utile lorsque vous souhaitez que les dossiers temporaires utilisateur doivent être contenus dans les dossiers de base, que ce soit sur un lecteur local du serveur hôte de Session Bureau à distance par ou sur un lecteur réseau partagé. Vous devez utiliser le **flattemp /enable** commande uniquement lorsque chaque utilisateur dispose d’un dossier temporaire distinct.
-   Vous pouvez rencontrer des erreurs d’application si le dossier temporaire de l’utilisateur se trouve sur un lecteur réseau. Cela se produit lorsque le lecteur réseau partagé est momentanément inaccessible sur le réseau. Étant donné que les fichiers temporaires de l’application sont inaccessibles ou synchronisée, il réagit comme si le disque s’est arrêté. Déplacez le dossier temporaire sur un lecteur réseau n’est pas recommandée. La valeur par défaut est de conserver les dossiers temporaires sur le disque dur local. Si vous rencontrez un comportement inattendu ou des erreurs de disque avec certaines applications, stabilisez votre réseau ou déplacez les dossiers temporaires vers le disque dur local.
-   Si vous désactivez l’utilisation des dossiers temporaires séparés par session, **flattemp** paramètres sont ignorés. Cette option est définie dans l’outil de Configuration de Services Bureau à distance.

## <a name="BKMK_examples"></a>Exemples
-   Pour afficher le paramètre actuel pour les dossiers temporaires communs, tapez :
    ```
    flattemp /query
    ```
-   Pour activer les dossiers temporaires communs, tapez :
    ```
    flattemp /enable
    ```
-   Pour désactiver les dossiers temporaires communs, tapez :
    ```
    flattemp /disable
    ```

## <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

[Services Bureau à distance &#40;Services Terminal Server&#41; référence de la commande](remote-desktop-services-terminal-services-command-reference.md)
