---
title: AuditPol clair
description: Rubrique de commandes de Windows pour **auditpol clair** -supprime la stratégie d’audit par utilisateur pour tous les utilisateurs, les réinitialisations (désactive), le système de stratégie d’audit pour toutes les sous-catégories et définit toutes les l’audit d’options sur désactivé.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 05bfa218-2434-4ad1-b33c-e6fcfb2b4f67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 86b56386ba9bed2486cdf8cdbb4486fcec6c6265
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435150"
---
# <a name="auditpol-clear"></a>AuditPol clair

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Supprime la stratégie d’audit par utilisateur pour tous les utilisateurs, les réinitialisations (désactive), le système de stratégie d’audit pour toutes les sous-catégories et définit toutes les l’audit d’options sur désactivé.

## <a name="syntax"></a>Syntaxe
```
auditpol /clear [/y]
```
## <a name="parameters"></a>Paramètres

| Paramètre |                                   Description                                    |
|-----------|----------------------------------------------------------------------------------|
|    /y     | Supprime l’invite pour confirmer si tous les paramètres de stratégie d’audit doivent être effacés. |
|    /?     |                       Affiche l'aide à l'invite de commandes.                       |

## <a name="remarks"></a>Notes
pour les opérations d’effacement pour les stratégies d’utilisateur et système, vous devez avoir écrire ou jeu d’autorisations de contrôle total sur cet objet dans le descripteur de sécurité. Vous pouvez également effectuer l’opération d’effacement par possédant le **gérer le journal d’audit et de sécurité** droit d’utilisateur (SeSecurityPrivilege). Toutefois, ce droit permet un accès supplémentaire qui n’est pas nécessaire d’effectuer l’opération d’effacement.
## <a name="BKMK_examples"></a>Exemples
Pour supprimer la stratégie d’audit par utilisateur pour tous les utilisateurs, réinitialisation (désactiver) la stratégie d’audit système pour toutes les sous-catégories et définissez les paramètres de stratégie d’audit sur désactivé, à l’invite de confirmation, tapez :
```
auditpol /clear
```
Pour supprimer la stratégie d’audit par utilisateur pour tous les utilisateurs, réinitialiser les paramètres de stratégie d’audit système pour toutes les sous-catégories et définir l’audit de tous les paramètres de stratégie sur désactivé, sans une invite de confirmation, tapez :
```
auditpol /clear /y
```
> [!NOTE]
> L’exemple précédent est utile lorsque vous utilisez un script pour effectuer cette opération.
> #### <a name="additional-references"></a>Références supplémentaires
> [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
