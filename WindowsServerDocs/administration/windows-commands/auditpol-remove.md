---
title: suppression d’Auditpol
description: La rubrique commandes Windows pour **Auditpol Remove**, qui supprime la stratégie d’audit par utilisateur pour un compte spécifié ou tous les comptes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: be42ec55-235c-44f7-9abd-ed1cf3f5b1f5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1eda43d6708a31b2966022d2ae2c162bbfc888cb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851172"
---
# <a name="auditpol-remove"></a>suppression d’Auditpol

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Supprime la stratégie d’audit par utilisateur pour un compte spécifié ou tous les comptes.

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

## <a name="remarks"></a>Notes

Pour les opérations de suppression pour la stratégie par utilisateur, vous devez disposer de l’autorisation d’écriture ou de contrôle total sur cet objet défini dans le descripteur de sécurité. Vous pouvez également effectuer des opérations de suppression en possédant le droit **d’utilisateur gérer le journal d’audit et de sécurité** (SeSecurityPrivilege). Toutefois, ce droit autorise un accès supplémentaire qui n’est pas nécessaire pour effectuer l’opération de suppression.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

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
