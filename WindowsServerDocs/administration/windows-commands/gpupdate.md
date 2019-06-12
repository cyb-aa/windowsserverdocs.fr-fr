---
title: gpupdate
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fd4e567-2ce1-4637-b611-c2f0895e5708
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dba8a0fb7d9a4e95f91ed1c1e140d965f5f9e2fb
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811115"
---
# <a name="gpupdate"></a>gpupdate

Met à jour les paramètres de stratégie de groupe. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#examples).

## <a name="syntax"></a>Syntaxe

```
gpupdate [/target:{Computer | User}] [/force] [/wait:<VALUE>] [/logoff] [/boot] [/sync] [/?]
```

### <a name="parameters"></a>Paramètres

|     Paramètre     |                                                                                                                                                                                                                                                                                                                             Description                                                                                                                                                                                                                                                                                                                             |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| / target : {ordinateur |                                                                                                                                                                                                                                                                                                                                Utilisateur}                                                                                                                                                                                                                                                                                                                                |
|      /force       |                                                                                                                                                                                                                                                                                   Réapplique tous les paramètres de stratégie. Par défaut, seuls les paramètres de stratégie qui ont été modifiés sont appliqués.                                                                                                                                                                                                                                                                                    |
|  / wait :\<valeur >   | Définit le nombre de secondes à attendre pour terminer avant de retourner à l’invite de commandes de traitement de stratégie. Lorsque la limite de temps est dépassée, l’invite de commandes s’affiche, mais continue de traitement de la stratégie. La valeur par défaut est 600 secondes. La valeur **0** signifie de ne pas attendre. La valeur **-1** signifie pour attendre indéfiniment.</br>Dans un script, à l’aide de cette commande avec une limite de temps spécifiée, que vous pouvez exécuter **gpupdate** et continuer avec les commandes qui ne dépendent pas de l’achèvement de **gpupdate**. Ou bien, vous pouvez utiliser cette commande avec aucune limite de temps spécifié pour vous permettre de **gpupdate** terminer en cours d’exécution avant l’exécutent des autres commandes qui en dépendent. |
|      /logoff      |                                                                                                                                   Entraîne une fermeture de session une fois que les paramètres de stratégie de groupe sont mis à jour. Cela est obligatoire pour les extensions côté client de stratégie de groupe qui ne sont pas exécutées sur un cycle de mise à jour en arrière-plan, mais sont exécutées lorsqu’un utilisateur ouvre une session. Exemples : Installation du logiciel utilisateur ciblé et la Redirection de dossiers. Cette option n’a aucun effet s’il existe aucune nécessitant une fermeture de session.                                                                                                                                    |
|       /boot       |                                                                                                                                       Entraîne un redémarrage de l’ordinateur une fois que les paramètres de stratégie de groupe sont appliqués. Cela est nécessaire pour les extensions côté client de stratégie de groupe qui ne sont pas exécutées sur un cycle de mise à jour en arrière-plan, mais sont exécutées au démarrage de l’ordinateur. Exemples d’Installation du logiciel ciblé sur un ordinateur. Cette option n’a aucun effet s’il existe aucune nécessitant un redémarrage.                                                                                                                                        |
|       /sync       |                                                                                                                                                                              Entraîne l’application de stratégie de premier plan suivante être effectuée de façon synchrone. Stratégie de premier plan est appliquée à l’ordinateur le démarrage et l’utilisateur d’ouverture de session. Vous pouvez le spécifier pour l’utilisateur, ordinateur ou les deux, à l’aide de la **/target** paramètre. Le **/force** et **/wait** paramètres sont ignorés si vous les spécifiez.                                                                                                                                                                               |
|        /?         |                                                                                                                                                                                                                                                                                                                Affiche l'aide à l'invite de commandes.                                                                                                                                                                                                                                                                                                                 |

## <a name="remarks"></a>Notes

-   Le **gpupdate** commande est disponible dans Windows Server 2008 R2, Windows Server 2008, Windows 7 Édition intégrale, Windows 7 Professionnel, Windows Vista Édition intégrale, Windows Vista Enterprise et Windows Vista Professionnel.

## <a name="examples"></a>Exemples

Forcer une mise à jour d’arrière-plan de tous les paramètres de stratégie de groupe, qu’elles ont changé.

```
gpupdate /force
```

#### <a name="additional-references"></a>Références supplémentaires

-   [TechCenter stratégie de groupe](https://go.microsoft.com/fwlink/?LinkID=145531)
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)