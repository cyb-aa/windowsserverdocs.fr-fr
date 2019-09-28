---
title: Auditpol Clear
description: Rubrique relative aux commandes Windows pour **Auditpol Clear** -supprime la stratégie d’audit par utilisateur pour tous les utilisateurs, réinitialise (désactive) la stratégie d’audit système pour toutes les sous-catégories et définit toutes les options d’audit sur désactivé.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 4fd2cce2b860ee41725b698dcd36ca38b2c4c6a8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382418"
---
# <a name="auditpol-clear"></a>Auditpol Clear

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

supprime la stratégie d’audit par utilisateur pour tous les utilisateurs, réinitialise (désactive) la stratégie d’audit système pour toutes les sous-catégories et définit toutes les options d’audit sur désactivé.

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
pour les opérations Clear pour la stratégie système et la stratégie par utilisateur, vous devez disposer de l’autorisation d’écriture ou de contrôle total sur cet objet défini dans le descripteur de sécurité. Vous pouvez également effectuer l’opération Clear en possédant le droit **d’utilisateur gérer le journal d’audit et de sécurité** (SeSecurityPrivilege). Toutefois, ce droit autorise un accès supplémentaire qui n’est pas nécessaire pour effectuer l’opération d’effacement.
## <a name="BKMK_examples"></a>Illustre
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
> #### <a name="additional-references"></a>Références supplémentaires
> [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
