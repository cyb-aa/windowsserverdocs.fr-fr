---
title: Auditpol Clear
description: Rubrique de référence pour la commande d’effacement Auditpol, qui supprime la stratégie d’audit par utilisateur pour tous les utilisateurs, réinitialise (désactive) la stratégie d’audit système pour toutes les sous-catégories et définit toutes les options d’audit sur désactivé.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 05bfa218-2434-4ad1-b33c-e6fcfb2b4f67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a3d4765907f1dd614f5d0a61585ea09069652ecb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719149"
---
# <a name="auditpol-clear"></a>Auditpol Clear

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Supprime la stratégie d’audit par utilisateur pour tous les utilisateurs, réinitialise (désactive) la stratégie d’audit système pour toutes les sous-catégories et définit toutes les options d’audit sur désactivé.

Pour effectuer des opérations *claires* sur les stratégies *système* et *par utilisateur* , vous devez disposer de l’autorisation d' **écriture** ou de **contrôle total** pour cet objet défini dans le descripteur de sécurité. Vous pouvez également effectuer des opérations *claires* si vous disposez du droit **d’utilisateur gérer le journal d’audit et de sécurité** (SeSecurityPrivilege). Toutefois, ce droit autorise un accès supplémentaire qui n’est pas nécessaire pour effectuer les opérations d' *effacement* globales.

## <a name="syntax"></a>Syntaxe

```
auditpol /clear [/y]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| ----------- | --------------- |
| /y | Supprime l’invite pour confirmer si tous les paramètres de stratégie d’audit doivent être effacés. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="examples"></a>Exemples

Pour supprimer la stratégie d’audit par utilisateur pour tous les utilisateurs, réinitialiser (désactiver) la stratégie d’audit du système pour toutes les sous-catégories et définir tous les paramètres de stratégie d’audit sur désactivé, à l’invite de confirmation, tapez :

```
auditpol /clear
```

Pour supprimer la stratégie d’audit par utilisateur pour tous les utilisateurs, réinitialisez les paramètres de stratégie d’audit du système pour toutes les sous-catégories et définissez tous les paramètres de stratégie d’audit sur désactivé, sans invite de confirmation, tapez :

```
auditpol /clear /y
```

> [!NOTE]
> L’exemple précédent est utile lors de l’utilisation d’un script pour effectuer cette opération.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commandes Auditpol](auditpol.md)
