---
title: liste d’Auditpol
description: La rubrique de référence pour la commande de liste Auditpol, qui répertorie les catégories et sous-catégories de stratégie d’audit, ou répertorie les utilisateurs pour lesquels une stratégie d’audit par utilisateur est définie.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 45502abe-3d6e-4e13-94f0-8e6fcb6db860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 96ee4388c716c066a2e9b55b57dd2e70b4b4f69c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719095"
---
# <a name="auditpol-list"></a>liste d’Auditpol

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Répertorie les catégories et sous-catégories de stratégie d’audit, ou répertorie les utilisateurs pour lesquels une stratégie d’audit par utilisateur est définie.

Pour effectuer des opérations de *liste* sur la stratégie *par utilisateur* , vous devez disposer de l’autorisation de **lecture** pour cet objet défini dans le descripteur de sécurité. Vous pouvez également effectuer des opérations de *liste* si vous disposez du droit **d’utilisateur gérer le journal d’audit et de sécurité** (SeSecurityPrivilege). Toutefois, ce droit autorise un accès supplémentaire qui n’est pas nécessaire pour effectuer les opérations de *liste* globale.

## <a name="syntax"></a>Syntaxe

```
auditpol /list
[/user|/category|subcategory[:<categoryname>|<{guid}>|*]]
[/v] [/r]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| ------- | -------- |
| /User | Récupère tous les utilisateurs pour lesquels la stratégie d’audit par utilisateur a été définie. S’il est utilisé avec le paramètre/v, l’identificateur de sécurité (SID) de l’utilisateur est également affiché. |
| /category | Affiche les noms des catégories comprises par le système. S’il est utilisé avec le paramètre/v, l’identificateur global unique (GUID) de la catégorie est également affiché. |
| /Subcategory | Affiche les noms des sous-catégories et leur GUID associé. |
| /v | Affiche le GUID avec la catégorie ou la sous-catégorie, ou, s’il est utilisé avec/User, affiche le SID de chaque utilisateur. |
| /r | Affiche la sortie sous la forme d’un rapport au format de valeurs séparées par des virgules (CSV). |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="examples"></a>Exemples

Pour répertorier tous les utilisateurs qui ont une stratégie d’audit définie, tapez :

```
auditpol /list /user
```

Pour répertorier tous les utilisateurs qui ont une stratégie d’audit définie et leur SID associé, tapez :

```
auditpol /list /user /v
```

Pour répertorier toutes les catégories et sous-catégories au format de rapport, tapez :

```
auditpol /list /subcategory:* /r
```

Pour répertorier les sous-catégories du suivi détaillé et des catégories d’accès DS, tapez :

```
auditpol /list /subcategory:detailed Tracking,DS Access
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commandes Auditpol](auditpol.md)
