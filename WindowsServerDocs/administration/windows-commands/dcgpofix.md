---
title: dcgpofix
description: Rubrique de référence pour la commande dcgpofix, qui recrée les objets de stratégie de groupe par défaut pour un domaine.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 81d5fa65-2aea-49d3-b353-357441846c00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f11b7db8110cd2d7dcf08cd250eba411e7ff21a8
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993169"
---
# <a name="dcgpofix"></a>dcgpofix

Recrée les objets de stratégie de groupe par défaut pour un domaine. Pour accéder au Console de gestion des stratégies de groupe (GPMC), vous devez installer la gestion des stratégie de groupe en tant que fonctionnalité via Gestionnaire de serveur.

>[!IMPORTANT]
> Il est recommandé de configurer l’objet de stratégie de groupe stratégie de domaine par défaut uniquement pour gérer les paramètres par défaut des **stratégies de compte** , la stratégie de mot de passe, la stratégie de verrouillage de compte et la stratégie Kerberos. En outre, vous devez configurer l’objet de stratégie de groupe stratégie des contrôleurs de domaine par défaut uniquement pour définir des droits d’utilisateur et des stratégies d’audit.

## <a name="syntax"></a>Syntaxe

```
dcgpofix [/ignoreschema] [/target: {domain | dc | both}] [/?]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| /ignoreschema | Ignore la version du schéma de Active Directory lorsque vous exécutez cette commande. Dans le cas contraire, la commande fonctionne uniquement sur la même version de schéma que la version de Windows dans laquelle la commande a été expédiée. |
| `/target {domain | dc | both` | Spécifie s’il faut cibler la stratégie de domaine par défaut, la stratégie des contrôleurs de domaine par défaut ou les deux types de stratégies. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="examples"></a>Exemples

Pour gérer les paramètres par défaut des **stratégies de compte** , la stratégie de mot de passe, la stratégie de verrouillage de compte et la stratégie Kerberos, tout en ignorant la version de schéma Active Directory, tapez :

```
dcgpofix /ignoreschema /target:domain
```

Pour configurer l’objet de stratégie de groupe stratégie des contrôleurs de domaine par défaut uniquement pour définir des droits d’utilisateur et des stratégies d’audit, tout en ignorant la version du schéma Active Directory, tapez :

```
dcgpofix /ignoreschema /target:dc
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)