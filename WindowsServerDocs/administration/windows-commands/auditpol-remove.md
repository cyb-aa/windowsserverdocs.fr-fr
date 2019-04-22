---
title: suppression de l’outil Auditpol
description: Rubrique de commandes de Windows pour **auditpol supprimer** -supprime la stratégie d’audit par utilisateur pour un compte spécifié ou tous les comptes.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 566827e93d9f8c9dc0d00f4f704513369fbb44ad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818300"
---
# <a name="auditpol-remove"></a>suppression de l’outil Auditpol

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Supprime la stratégie d’audit par utilisateur pour un compte spécifié ou tous les comptes.

## <a name="syntax"></a>Syntaxe
```
auditpol /remove [/user[:<username>|<{SID}>]]
[/allusers]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/User|Spécifie l’identificateur de sécurité (SID) ou nom d’utilisateur pour l’utilisateur pour lequel l’utilisateur par la stratégie d’audit doit être supprimé.|
|/allusers|Supprime la stratégie d’audit par utilisateur pour tous les utilisateurs.|
|/?|Affiche l'aide à l'invite de commandes.|
## <a name="remarks"></a>Notes
pour les opérations de suppression de la stratégie par utilisateur, vous devez disposer d’autorisation d’écriture ou un contrôle total sur cet objet défini dans le descripteur de sécurité. Vous pouvez également effectuer des opérations de suppression effectuées par possédant le **gérer le journal d’audit et de sécurité** droit d’utilisateur (SeSecurityPrivilege). Toutefois, ce droit permet un accès supplémentaire qui n’est pas nécessaire d’effectuer l’opération de suppression.
## <a name="BKMK_examples"></a>Exemples
Pour supprimer la stratégie d’audit par utilisateur pour l’utilisateur MarcelRug par nom, tapez :
```
auditpol /remove /user:mikedan
```
Pour supprimer la stratégie d’audit par utilisateur pour MarcelRug utilisateur en SID, tapez :
```
auditpol /remove /user:{S-1-5-21-397123471-12346959}
```
Pour supprimer la stratégie d’audit par utilisateur pour tous les utilisateurs, tapez :
```
auditpol /remove /allusers
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
