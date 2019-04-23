---
title: waitfor
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a48ef70d-4d28-4035-b6b0-7d7b46ac2157
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c4a91dd3822cf4d8dd904f473f146a2f0ee54c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840160"
---
# <a name="waitfor"></a>waitfor



Envoie ou attend un signal sur un système. **WAITFOR** est utilisé pour synchroniser des ordinateurs sur un réseau.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
waitfor [/s <Computer> [/u [<Domain>\]<User> [/p [<Password>]]]] /si <SignalName>
waitfor [/t <Timeout>] <SignalName>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|/s \<ordinateur >|Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local. Ce paramètre s’applique à tous les fichiers et dossiers spécifiés dans la commande.|
|/u [\<domaine >\]<User>|Exécute le script en utilisant les informations d’identification du compte d’utilisateur spécifié. Par défaut, **waitfor** utilise les informations d’identification de l’utilisateur actuel.|
|/p [\<Password>]|Spécifie le mot de passe du compte d’utilisateur qui est spécifié dans le **/u** paramètre.|
|/si|Envoie le signal spécifié sur le réseau.|
|/t \<délai d’attente >|Spécifie le nombre de secondes à attendre un signal. Par défaut, **waitfor** attend indéfiniment.|
|\<SignalName>|Spécifie le signal qui **waitfor** attend ou envoie. *NomSignal* ne respecte pas la casse.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   Les noms de signal ne peut pas dépasser 225 caractères. Caractères valides sont 0-9 a-z, A-Z et l’ASCII étendu le jeu de caractères (128 à 255).
-   Si vous n’utilisez pas **/s**, le signal est diffusé à tous les systèmes dans un domaine. Si vous utilisez **/s**, le signal est envoyé uniquement au système spécifié.
-   Vous pouvez exécuter plusieurs instances de **waitfor** sur un seul ordinateur, mais chaque instance de **waitfor** doit attendre un signal différent. Seule une instance de **waitfor** peut attendre un signal donné sur un ordinateur donné.
-   Vous pouvez activer manuellement un signal à l’aide de la **/si** option de ligne de commande.
-   **WAITFOR** s’exécute uniquement sur Windows XP et les serveurs qui exécutent un système d’exploitation de Windows Server 2003, mais il peut envoyer des signaux à n’importe quel ordinateur exécutant un système d’exploitation de Windows.
-   Les ordinateurs peuvent recevoir uniquement les signaux si elles se trouvent dans le même domaine que l’ordinateur qui envoie le signal.
-   Vous pouvez utiliser **waitfor** lorsque vous testez les générations logicielles. Par exemple, l’ordinateur de compilation peut envoyer un signal à plusieurs ordinateurs qui exécutent **waitfor** une fois la compilation terminée avec succès. À la réception du signal, le fichier de commandes qui inclut **waitfor** peut indiquer aux ordinateurs pour démarrer immédiatement l’installation de logiciels ou de tests en cours d’exécution sur la version compilée.

## <a name="BKMK_examples"></a>Exemples

Pour attendre jusqu'à la réception du signal « espresso\build007 », tapez :
```
waitfor espresso\build007
```
Par défaut, **waitfor** attend indéfiniment un signal.

Pour attendre 10 secondes pour le signal « espresso\compile007 » à être reçu avant l’expiration, tapez :
```
waitfor /t 10 espresso\build007
```
Pour activer manuellement le signal « espresso\build007 », tapez :
```
waitfor /si espresso\build007
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)