---
title: dcgpofix
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 81d5fa65-2aea-49d3-b353-357441846c00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 179d540371870075906bbcbf8ff912e1b883915d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433937"
---
# <a name="dcgpofix"></a>dcgpofix



Recrée les objets de stratégie de groupe (GPO) par défaut pour un domaine. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
DCGPOFix [/ignoreschema] [/target: {Domain | DC | Both}] [/?]
```

### <a name="parameters"></a>Paramètres

|    Paramètre    |                                                                                                 Description                                                                                                 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  /ignoreschema  | Ignore la version de la console de gestion du schéma Active</br>Lorsque vous exécutez cette commande. Sinon, la commande fonctionne uniquement sur la même version de schéma que la version de Windows dans lequel la commande a été fournie. |
| / target {domain |                                                                                                     DC                                                                                                      |
|       /?        |                                                                                    Affiche l'aide à l'invite de commandes.                                                                                     |

## <a name="remarks"></a>Notes

-   Le **dcgpofix** commande est disponible dans Windows Server 2008 R2 et Windows Server 2008, sauf sur les installations Server Core.
-   Bien que la Console de gestion des stratégies de groupe (GPMC) est distribué avec Windows Server 2008 R2 et Windows Server 2008, vous devez installer la gestion des stratégies de groupe en tant que fonctionnalité via le Gestionnaire de serveur.

## <a name="BKMK_Examples"></a>Exemples

Restaurer l’objet de stratégie de domaine par défaut à son état d’origine. Vous perdrez toutes les modifications que vous avez apportées à cet objet de stratégie de groupe. Comme meilleure pratique, vous devez configurer la stratégie de domaine par défaut uniquement pour gérer les paramètres de stratégies de compte par défaut, la stratégie de mot de passe, stratégie de verrouillage de compte et stratégie Kerberos. Dans cet exemple, vous ignorez la version du schéma Active Directory afin que le **dcgpofix** commande n’est pas limité à un même schéma que la version de Windows dans lequel la commande a été fournie.
```
dcgpofix /ignoreschema /target:Domain
```
Restaurer l’objet de stratégie par défaut des contrôleurs de domaine à son état d’origine. Vous perdrez toutes les modifications que vous avez apportées à cet objet de stratégie de groupe. Comme meilleure pratique, vous devez configurer la stratégie par défaut domaine contrôleurs uniquement pour définir les droits d’utilisateur et de stratégies d’audit. Dans cet exemple, vous ignorez la version du schéma Active Directory afin que le **dcgpofix** commande n’est pas limité à un même schéma que la version de Windows dans lequel la commande a été fournie.
```
dcgpofix /ignoreschema /target:DC
```

#### <a name="additional-references"></a>Références supplémentaires

-   [TechCenter stratégie de groupe](https://go.microsoft.com/fwlink/?LinkID=145531)
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)