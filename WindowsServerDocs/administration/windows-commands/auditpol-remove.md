---
title: suppression d’Auditpol
description: Rubrique relative aux commandes Windows pour **Auditpol Remove** -supprime la stratégie d’audit par utilisateur pour un compte spécifié ou tous les comptes.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: be42ec55-235c-44f7-9abd-ed1cf3f5b1f5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d652cb3bf7156bc1b1198616c9f6e57f588acd8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382482"
---
# <a name="auditpol-remove"></a>suppression d’Auditpol

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

supprime la stratégie d’audit par utilisateur pour un compte spécifié ou tous les comptes.

## <a name="syntax"></a>Syntaxe
```
auditpol /remove [/user[:<username>|<{SID}>]]
[/allusers]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/User|Spécifie l’identificateur de sécurité (SID) ou le nom d’utilisateur de l’utilisateur pour lequel la stratégie d’audit par utilisateur doit être supprimée.|
|/ALLUSERS|supprime la stratégie d’audit par utilisateur pour tous les utilisateurs.|
|/?|Affiche l'aide à l'invite de commandes.|
## <a name="remarks"></a>Notes
pour les opérations de suppression pour la stratégie par utilisateur, vous devez disposer de l’autorisation d’écriture ou de contrôle total sur cet objet défini dans le descripteur de sécurité. Vous pouvez également effectuer des opérations de suppression en possédant le droit **d’utilisateur gérer le journal d’audit et de sécurité** (SeSecurityPrivilege). Toutefois, ce droit autorise un accès supplémentaire qui n’est pas nécessaire pour effectuer l’opération de suppression.
## <a name="BKMK_examples"></a>Illustre
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
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
