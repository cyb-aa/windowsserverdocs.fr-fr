---
title: Auditpol Clear
description: La rubrique commandes Windows pour **Auditpol Clear**, qui supprime la stratégie d’audit par utilisateur pour tous les utilisateurs, réinitialise (désactive) la stratégie d’audit du système pour toutes les sous-catégories et définit toutes les options d’audit sur désactivé.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 05bfa218-2434-4ad1-b33c-e6fcfb2b4f67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 971f4ba54d787be29cb9e7d710f556c50c69a8dc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851202"
---
# <a name="auditpol-clear"></a>Auditpol Clear

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Supprime la stratégie d’audit par utilisateur pour tous les utilisateurs, réinitialise (désactive) la stratégie d’audit système pour toutes les sous-catégories et définit toutes les options d’audit sur désactivé.

## <a name="syntax"></a>Syntaxe

```
auditpol /clear [/y]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| ----------- | --------------- |
| /y | Supprime l’invite pour confirmer si tous les paramètres de stratégie d’audit doivent être effacés. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="remarks"></a>Notes

Pour les opérations Clear pour la stratégie système et la stratégie par utilisateur, vous devez disposer de l’autorisation d’écriture ou de contrôle total sur cet objet défini dans le descripteur de sécurité. Vous pouvez également effectuer l’opération Clear en possédant le droit **d’utilisateur gérer le journal d’audit et de sécurité** (SeSecurityPrivilege). Toutefois, ce droit autorise un accès supplémentaire qui n’est pas nécessaire pour effectuer l’opération d’effacement.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

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
