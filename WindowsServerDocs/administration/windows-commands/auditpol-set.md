---
title: Auditpol défini
description: Rubrique de référence pour la commande d’ensemble Auditpol, qui définit la stratégie d’audit par utilisateur, la stratégie d’audit du système ou les options d’audit.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f4947486-87bd-48cb-ba81-7230c8e70895
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 73868d6044d8742d4d9e0ce76e0668402f230f86
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718886"
---
# <a name="auditpol-set"></a>Auditpol défini

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Définit la stratégie d’audit par utilisateur, la stratégie d’audit du système ou les options d’audit.

Pour effectuer des opérations de *définition* sur les stratégies *système* et *par utilisateur* , vous devez disposer de l’autorisation d' **écriture** ou de **contrôle total** pour cet objet défini dans le descripteur de sécurité. Vous pouvez également effectuer des opérations de *définition* si vous disposez du droit **d’utilisateur gérer le journal d’audit et de sécurité** (SeSecurityPrivilege). Toutefois, ce droit autorise un accès supplémentaire qui n’est pas nécessaire pour effectuer les opérations de *jeu* global.

## <a name="syntax"></a>Syntaxe

```
auditpol /set
[/user[:<username>|<{sid}>][/include][/exclude]]
[/category:<name>|<{guid}>[,:<name|<{guid}> ]]
[/success:<enable>|<disable>][/failure:<enable>|<disable>]
[/subcategory:<name>|<{guid}>[,:<name|<{guid}> ]]
[/success:<enable>|<disable>][/failure:<enable>|<disable>]
[/option:<option name> /value: <enable>|<disable>]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| /User | Principal de sécurité pour lequel la stratégie d’audit par utilisateur spécifiée par la catégorie ou la sous-catégorie est définie. L’option catégorie ou sous-catégorie doit être spécifiée sous la forme d’un identificateur de sécurité (SID) ou d’un nom. |
| /include | Spécifié avec/User ; indique que la stratégie par utilisateur de l’utilisateur entraîne la génération d’un audit même s’il n’est pas spécifié par la stratégie d’audit système. Ce paramètre est la valeur par défaut et est appliqué automatiquement si les paramètres/include et/Exclude ne sont pas explicitement spécifiés. |
| /Exclude | Spécifié avec/User ; indique que la stratégie par utilisateur de l’utilisateur entraîne la suppression d’un audit indépendamment de la stratégie d’audit du système. Ce paramètre est ignoré pour les utilisateurs qui sont membres du groupe Administrateurs local. |
| /category | Une ou plusieurs catégories d’audit spécifiées par un identificateur global unique (GUID) ou un nom. Si aucun utilisateur n’est spécifié, la stratégie système est définie. |
| /Subcategory | Une ou plusieurs sous-catégories d’audit spécifiées par le GUID ou le nom. Si aucun utilisateur n’est spécifié, la stratégie système est définie. |
| /Success | Spécifie l’audit des réussites. Ce paramètre est la valeur par défaut et est appliqué automatiquement si les paramètres/Success et/Failure ne sont pas explicitement spécifiés. Ce paramètre doit être utilisé avec un paramètre indiquant s’il faut activer ou désactiver le paramètre. |
| /Failure | Spécifie l’audit des échecs. Ce paramètre doit être utilisé avec un paramètre indiquant s’il faut activer ou désactiver le paramètre. |
| /option | Définit la stratégie d’audit pour les options CrashOnAuditFail, FullprivilegeAuditing, AuditBaseObjects ou AuditBasedirectories. |
| /SD | Définit le descripteur de sécurité utilisé pour déléguer l’accès à la stratégie d’audit. Le descripteur de sécurité doit être spécifié à l’aide du langage SDDL (Security Descriptor Definition Language). Le descripteur de sécurité doit avoir une liste de contrôle d’accès discrétionnaire (DACL). |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="examples"></a>Exemples

Pour définir la stratégie d’audit par utilisateur pour toutes les sous-catégories de la catégorie suivi détaillé pour l’utilisateur Mikedan afin que toutes les tentatives réussies de l’utilisateur soient auditées, tapez :

```
auditpol /set /user:mikedan /category:detailed Tracking /include /success:enable
```

Pour définir la stratégie d’audit par utilisateur pour les catégories spécifiées par le nom et le GUID, et les sous-catégories spécifiées par le GUID afin de supprimer l’audit pour toutes les tentatives ayant réussi ou échoué, tapez :

```
auditpol /set /user:mikedan /exclude /category:Object Access,System,{6997984b-797a-11d9-bed3-505054503030}
/subcategory:{0ccee9210-69ae-11d9-bed3-505054503030},:{0ccee9211-69ae-11d9-bed3-505054503030}, /success:enable /failure:enable
```

Pour définir la stratégie d’audit par utilisateur pour l’utilisateur spécifié pour toutes les catégories pour la suppression de toutes les tentatives, tapez :
```
auditpol /set /user:mikedan /exclude /category:* /success:enable
```

Pour définir la stratégie d’audit système pour toutes les sous-catégories de la catégorie suivi détaillé afin d’inclure l’audit uniquement pour les tentatives réussies, tapez :

```
auditpol /set /category:detailed Tracking /success:enable
```

> [!NOTE]
> Le paramètre d’échec n’est pas modifié.

Pour définir la stratégie d’audit du système pour l’accès aux objets et les catégories système (qui sont implicites, car les sous-catégories sont répertoriées) et les sous-catégories spécifiées par les GUID pour la suppression des échecs de tentative et l’audit des tentatives réussies, tapez :

```
auditpol /set /subcategory:{0ccee9210-69ae-11d9-bed3-505054503030},{0ccee9211-69ae-11d9-bed3-505054503030}, /failure:disable /success:enable
```

Pour définir les options d’audit sur l’état activé pour l’option CrashOnAuditFail, tapez :

```
auditpol /set /option:CrashOnAuditFail /value:enable
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commandes Auditpol](auditpol.md)
