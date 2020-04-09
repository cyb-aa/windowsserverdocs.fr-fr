---
title: gpupdate
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2fd4e567-2ce1-4637-b611-c2f0895e5708
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 23f9bf243210db7c47b9b08bc363f5dfa7815fc9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842422"
---
# <a name="gpupdate"></a>gpupdate

Met à jour les paramètres de stratégie de groupe. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#examples).

## <a name="syntax"></a>Syntaxe

```
gpupdate [/target:{Computer | User}] [/force] [/wait:<VALUE>] [/logoff] [/boot] [/sync] [/?]
```

#### <a name="parameters"></a>Paramètres

|     Paramètre     |                                                                                                                                                                                                                                                                                                                             Description                                                                                                                                                                                                                                                                                                                             |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /target : {ordinateur\|utilisateur} | Spécifie que seuls les paramètres de stratégie de l’ordinateur ou de l’utilisateur sont mis à jour. Par défaut, les paramètres de stratégie de l’utilisateur et de l’ordinateur sont mis à jour.                                                                                                                                                                                                                                                                                                                                |
|      /Force       |                                                                                                                                                                                                                                                                                   Réapplique tous les paramètres de stratégie. Par défaut, seuls les paramètres de stratégie modifiés sont appliqués.                                                                                                                                                                                                                                                                                    |
|  /Wait :\<valeur >   | Définit le nombre de secondes à attendre que le traitement de la stratégie se termine avant de revenir à l’invite de commandes. Lorsque la limite de temps est dépassée, l’invite de commandes s’affiche, mais le traitement de la stratégie se poursuit. La valeur par défaut est 600 secondes. La valeur **0** signifie qu’il n’est pas attendu. La valeur **-1** signifie qu’elle est indéfinie.</br>Dans un script, en utilisant cette commande avec une limite de temps spécifiée, vous pouvez exécuter **gpupdate** et continuer avec les commandes qui ne dépendent pas de l’achèvement de **gpupdate**. Vous pouvez également utiliser cette commande sans limite de temps spécifiée pour laisser l’exécution de l’option **gpupdate** avant l’exécution d’autres commandes qui dépendent de celle-ci. |
|      /logoff      |                                                                                                                                   Provoque une déconnexion après la mise à jour des paramètres de stratégie de groupe. Cela est requis pour les extensions côté client stratégie de groupe qui ne traitent pas la stratégie sur un cycle de mise à jour en arrière-plan, mais qui traitent la stratégie quand un utilisateur ouvre une session. Par exemple, l’installation de logiciels ciblés par l’utilisateur et la redirection de dossiers. Cette option n’a aucun effet si aucune extension nommée ne nécessite de fermeture de session.                                                                                                                                    |
|       /Boot       |                                                                                                                                       Entraîne le redémarrage de l’ordinateur après l’application des paramètres de stratégie de groupe. Cela est requis pour les extensions côté client stratégie de groupe qui ne traitent pas la stratégie sur un cycle de mise à jour en arrière-plan, mais qui traitent la stratégie au démarrage de l’ordinateur. Par exemple, l’installation de logiciels ciblés sur l’ordinateur. Cette option n’a aucun effet si aucune extension appelée ne nécessite un redémarrage.                                                                                                                                        |
|       /Sync       |                                                                                                                                                                              Fait en sorte que la prochaine application de stratégie de premier plan soit exécutée de façon synchrone. La stratégie de premier plan est appliquée au démarrage de l’ordinateur et à l’ouverture de session utilisateur. Vous pouvez spécifier cette option pour l’utilisateur, l’ordinateur ou les deux à l’aide du paramètre **/target** . Les paramètres **/force** et **/Wait** sont ignorés si vous les spécifiez.                                                                                                                                                                               |
|        /?         |                                                                                                                                                                                                                                                                                                                Affiche l'aide à l'invite de commandes.                                                                                                                                                                                                                                                                                                                 |

## <a name="remarks"></a>Notes

-   La commande **gpupdate** est disponible dans windows Server 2008 R2, windows Server 2008, Windows 7 édition intégrale, Windows 7 professionnel, Windows Vista Édition intégrale, Windows Vista entreprise et Windows Vista professionnel.

## <a name="examples"></a>Exemples

Forcez une mise à jour en arrière-plan de tous les paramètres stratégie de groupe, qu’ils aient ou non été modifiés.

```
gpupdate /force
```

## <a name="additional-references"></a>Références supplémentaires

-   [TechCenter stratégie de groupe](https://go.microsoft.com/fwlink/?LinkID=145531)
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
