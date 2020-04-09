---
title: waitfor
description: La rubrique commandes Windows pour WAITFOR, qui envoie ou attend un signal sur un système. **WAITFOR** est utilisé pour synchroniser les ordinateurs sur un réseau.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a48ef70d-4d28-4035-b6b0-7d7b46ac2157
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4542fc9d231b8150ab89e07e173d9671d6b7a3f3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829932"
---
# <a name="waitfor"></a>waitfor



Envoie ou attend un signal sur un système. **WAITFOR** est utilisé pour synchroniser les ordinateurs sur un réseau.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
waitfor [/s <Computer> [/u [<Domain>\]<User> [/p [<Password>]]]] /si <SignalName>
waitfor [/t <Timeout>] <SignalName>
```

### <a name="parameters"></a>Paramètres

|       Paramètre       |                                                                                         Description                                                                                          |
|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /s \<> de l’ordinateur     | Spécifie le nom ou l’adresse IP d’un ordinateur distant (n’utilisez pas de barres obliques inverses). La valeur par défaut est l'ordinateur local. Ce paramètre s’applique à tous les fichiers et dossiers spécifiés dans la commande. |
| /u [\<> de domaine\]<User> |                              Exécute le script à l’aide des informations d’identification du compte d’utilisateur spécifié. Par défaut, **WAITFOR** utilise les informations d’identification de l’utilisateur actuel.                               |
|   /p [\<> de mot de passe]    |                                                    Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** .                                                     |
|          /Si          |                                                                        Envoie le signal spécifié sur le réseau.                                                                        |
|     /t \<délai d’expiration >     |                                              Spécifie le nombre de secondes d’attente d’un signal. Par défaut, **WAITFOR** attend indéfiniment.                                               |
|     \<SignalName >     |                                                Spécifie le signal que **WAITFOR** attend ou envoie. *SignalName* ne respecte pas la casse.                                                 |
|          /?           |                                                                             Affiche l'aide à l'invite de commandes.                                                                             |

## <a name="remarks"></a>Notes

-   Les noms de signal ne peuvent pas dépasser 225 caractères. Les caractères valides sont les suivants : a-z, A-Z, 0-9 et le jeu de caractères étendus ASCII (128-255).
-   Si vous n’utilisez pas le **commutateur/s**, le signal est diffusé à tous les systèmes d’un domaine. Si vous utilisez **/s**, le signal est envoyé uniquement au système spécifié.
-   Vous pouvez exécuter plusieurs instances de **WAITFOR** sur un seul ordinateur, mais chaque instance de **WAITFOR** doit attendre un signal différent. Une seule instance de **WAITFOR** peut attendre un signal donné sur un ordinateur donné.
-   Vous pouvez activer un signal manuellement à l’aide de l’option de ligne de commande **/si** .
-   **WAITFOR** s’exécute uniquement sur Windows XP et les serveurs qui exécutent un système d’exploitation windows Server 2003, mais il peut envoyer des signaux à n’importe quel ordinateur exécutant un système d’exploitation Windows.
-   Les ordinateurs ne peuvent recevoir des signaux que s’ils se trouvent dans le même domaine que l’ordinateur qui envoie le signal.
-   Vous pouvez utiliser **WAITFOR** lorsque vous testez des builds logicielles. Par exemple, l’ordinateur de compilation peut envoyer un signal à plusieurs ordinateurs exécutant **WAITFOR** une fois la compilation terminée. À la réception du signal, le fichier de commandes qui comprend **WAITFOR** peut demander aux ordinateurs de commencer immédiatement l’installation d’un logiciel ou d’exécuter des tests sur la build compilée.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour attendre la réception du signal espresso\build007, tapez :
```
waitfor espresso\build007
```
Par défaut, **WAITFOR** attend indéfiniment un signal.

Pour attendre la réception de 10 secondes du signal espresso\compile007 avant le dépassement du délai d’attente, tapez :
```
waitfor /t 10 espresso\build007
```
Pour activer manuellement le signal espresso\build007, tapez :
```
waitfor /si espresso\build007
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)