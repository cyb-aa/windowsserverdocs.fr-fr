---
title: liste d’Auditpol
description: La rubrique commandes Windows pour la **liste d’Auditpol**, qui répertorie les catégories et sous-catégories de stratégie d’audit, ou répertorie les utilisateurs pour lesquels une stratégie d’audit par utilisateur est définie.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 45502abe-3d6e-4e13-94f0-8e6fcb6db860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9e0aff46e62ea4e4259360b78aae223dfcd66ef7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851182"
---
# <a name="auditpol-list"></a>liste d’Auditpol

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Répertorie les catégories et/ou sous-catégories de stratégie d’audit, ou répertorie les utilisateurs pour lesquels une stratégie d’audit par utilisateur est définie.

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

## <a name="remarks"></a>Notes

Pour toutes les opérations de liste pour la stratégie par utilisateur, vous devez disposer de l’autorisation de lecture sur cet objet défini dans le descripteur de sécurité. Vous pouvez également effectuer des opérations de liste en possédant le droit **d’utilisateur gérer le journal d’audit et de sécurité** (SeSecurityPrivilege). Toutefois, ce droit autorise un accès supplémentaire qui n’est pas nécessaire pour effectuer l’opération de liste.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

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
