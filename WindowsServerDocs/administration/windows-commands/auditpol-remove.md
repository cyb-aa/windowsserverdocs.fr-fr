---
title: suppression d’Auditpol
description: Rubrique de référence pour la commande Auditpol Remove, qui supprime la stratégie d’audit par utilisateur pour un compte spécifié ou tous les comptes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: be42ec55-235c-44f7-9abd-ed1cf3f5b1f5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9aedde39d44c7640e6aa2516465e1c8ec7d022c2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719087"
---
# <a name="auditpol-remove"></a>suppression d’Auditpol

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Supprime la stratégie d’audit par utilisateur pour un compte spécifié ou tous les comptes.

Pour effectuer des opérations de *suppression* sur la stratégie *par utilisateur* , vous devez disposer des autorisations d' **écriture** ou de **contrôle total** pour cet objet défini dans le descripteur de sécurité. Vous pouvez également effectuer des opérations de *suppression* si vous disposez du droit **d’utilisateur gérer le journal d’audit et de sécurité** (SeSecurityPrivilege). Toutefois, ce droit autorise un accès supplémentaire qui n’est pas nécessaire pour effectuer les opérations de *suppression* générales.

## <a name="syntax"></a>Syntaxe

```
auditpol /remove [/user[:<username>|<{SID}>]]
[/allusers]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| ------- | -------- |
| /User | Spécifie l’identificateur de sécurité (SID) ou le nom d’utilisateur de l’utilisateur pour lequel la stratégie d’audit par utilisateur doit être supprimée. |
| /ALLUSERS | Supprime la stratégie d’audit par utilisateur pour tous les utilisateurs. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="examples"></a>Exemples

Pour supprimer la stratégie d’audit par utilisateur pour l’utilisateur Mikedan par nom, tapez :

```
auditpol /remove /user:mikedan
```

Pour supprimer la stratégie d’audit par utilisateur pour l’utilisateur Mikedan par SID, tapez :

```
auditpol /remove /user:{S-1-5-21-397123471-12346959}
```

Pour supprimer la stratégie d’audit par utilisateur pour tous les utilisateurs, tapez :

```
auditpol /remove /allusers
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commandes Auditpol](auditpol.md)
