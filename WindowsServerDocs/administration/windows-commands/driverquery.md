---
title: driverquery
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 92ca4b84-e4e2-405b-9f31-bf6db9f66839
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5d44a1be300b7178bc2271187344c2fc4ab8815e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377652"
---
# <a name="driverquery"></a>driverquery



Permet à un administrateur d’afficher une liste des pilotes de périphériques installés et leurs propriétés. En cas d’utilisation sans paramètre, **driverquery** s’exécute sur l’ordinateur local.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
driverquery [/s <System> [/u [<Domain>\]<Username> [/p <Password>]]] [/fo {table | list | csv}] [/nh] [/v | /si]
```

## <a name="parameters"></a>Paramètres

|         Paramètre         |                                                                                                                                         Description                                                                                                                                          |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /s \<système >        |                                                                                      Spécifie le nom ou l’adresse IP d’un ordinateur distant. N’utilisez pas de barres obliques inverses. La valeur par défaut est l'ordinateur local.                                                                                       |
| /u [\<> de domaine\]<Username> | Exécute la commande avec les informations d’identification du compte *d’utilisateur,* tel que spécifié par l’utilisateur ou le *domaine*\*utilisateur<em>. Par défaut, \*\*/s</em>\* utilise les informations d’identification de l’utilisateur actuellement connecté à l’ordinateur qui émet la commande. **/u** ne peut pas être utilisé, sauf si **/s** est spécifié. |
|      /p \<mot de passe >       |                                                                           Spécifie le mot de passe du compte d’utilisateur spécifié dans le paramètre **/u** . **/p** ne peut pas être utilisé sauf si **/u** est spécifié.                                                                            |
|        /FO {table         |                                                                                                                                             list                                                                                                                                             |
|            /NH            |                                                                                      Omet la ligne d’en-tête des informations de pilote affichées. Non valide si le paramètre **/FO** est défini sur **List**.                                                                                      |
|            /v             |                                                                                                               Affiche la sortie détaillée. **/v** n’est pas valide pour les pilotes signés.                                                                                                               |
|            /Si            |                                                                                                                          Fournit des informations sur les pilotes signés.                                                                                                                          |
|            /?             |                                                                                                                             Affiche l'aide à l'invite de commandes.                                                                                                                             |

## <a name="BKMK_examples"></a>Illustre

Pour afficher la liste des pilotes de périphériques installés sur l’ordinateur local, tapez :
```
driverquery 
```
Pour afficher la sortie au format CSV (valeurs séparées par des virgules), tapez :
```
driverquery /fo csv 
```
Pour masquer la ligne d’en-tête dans la sortie, tapez :
```
driverquery /nh 
```
Pour utiliser la commande **driverquery** sur un serveur distant nommé **Server1** en utilisant vos informations d’identification actuelles sur l’ordinateur local, tapez :
```
driverquery /s server1
```
Pour utiliser la commande **driverquery** sur un serveur distant nommé **Server1** en utilisant les informations d’identification pour **User1** sur le domaine **maindol**, tapez :
```
driverquery /s server1 /u maindom\user1 /p p@ssw3d
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)