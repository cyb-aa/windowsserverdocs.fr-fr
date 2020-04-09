---
title: dcgpofix
description: La rubrique commandes Windows pour Dcgpofix, qui recrée les objets de stratégie de groupe par défaut pour un domaine.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 81d5fa65-2aea-49d3-b353-357441846c00
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1532d4c8c0266c92b4efce57a6744552a54e51b0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846702"
---
# <a name="dcgpofix"></a>dcgpofix

Recrée les objets de stratégie de groupe par défaut pour un domaine. Pour obtenir des exemples d’utilisation de cette commande, consultez [exemples](#BKMK_Examples).

## <a name="syntax"></a>Syntaxe

```
DCGPOFix [/ignoreschema] [/target: {Domain | DC | Both}] [/?]
```

#### <a name="parameters"></a>Paramètres

|    Paramètre    |                                                                                                 Description                                                                                                 |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  /ignoreschema  | Ignore la version du schéma Active Directory® MC</br>Lorsque vous exécutez cette commande. Dans le cas contraire, la commande fonctionne uniquement sur la même version de schéma que la version de Windows dans laquelle la commande a été expédiée. |
| /Target {domaine |                                                                                                     Contrôleur de domaine                                                                                                      |
|       /?        |                                                                                    Affiche l'aide à l'invite de commandes.                                                                                     |

## <a name="remarks"></a>Notes

-   La commande **dcgpofix** est disponible dans windows Server 2008 R2 et windows Server 2008, sauf sur les installations Server Core.
-   Bien que la Console de gestion des stratégies de groupe (GPMC) soit distribuée avec Windows Server 2008 R2 et Windows Server 2008, vous devez installer la gestion des stratégie de groupe en tant que fonctionnalité via Gestionnaire de serveur.

## <a name="examples"></a><a name=BKMK_Examples></a>Illustre

Restaure l’état d’origine de l’objet de stratégie de groupe de la stratégie de domaine par défaut. Vous perdrez toutes les modifications que vous avez apportées à cet objet de stratégie de groupe. Il est recommandé de configurer l’objet de stratégie de groupe stratégie de domaine par défaut uniquement pour gérer les paramètres par défaut des stratégies de compte, la stratégie de mot de passe, la stratégie de verrouillage de compte et la stratégie Kerberos. Dans cet exemple, vous ignorez la version du schéma Active Directory afin que la commande **dcgpofix** ne soit pas limitée au même schéma que la version de Windows dans laquelle la commande a été expédiée.
```
dcgpofix /ignoreschema /target:Domain
```
Restaure l’état d’origine de l’objet de stratégie de groupe stratégie des contrôleurs de domaine par défaut. Vous perdrez toutes les modifications que vous avez apportées à cet objet de stratégie de groupe. En guise de meilleure pratique, vous devez configurer l’objet de stratégie de groupe stratégie des contrôleurs de domaine par défaut uniquement pour définir des droits d’utilisateur et des stratégies d’audit. Dans cet exemple, vous ignorez la version du schéma Active Directory afin que la commande **dcgpofix** ne soit pas limitée au même schéma que la version de Windows dans laquelle la commande a été expédiée.
```
dcgpofix /ignoreschema /target:DC
```

## <a name="additional-references"></a>Références supplémentaires

-   [TechCenter stratégie de groupe](https://go.microsoft.com/fwlink/?LinkID=145531)
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)