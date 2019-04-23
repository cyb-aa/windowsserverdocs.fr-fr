---
title: sauvegarde de l’outil Auditpol
description: Rubrique de commandes de Windows pour **auditpol sauvegarde** -sauvegarde système auditer les paramètres de stratégie, les paramètres de stratégie d’audit par utilisateur pour tous les utilisateurs et toutes les options d’audit dans un fichier texte de valeurs séparées par des virgules (CSV).
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc84e581-aa0f-4c91-b13b-1d970bad5517
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 78594c0445ae482e49d47b3b67bb867e53866017
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867950"
---
# <a name="auditpol-backup"></a>sauvegarde de l’outil Auditpol

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sauvegarde système auditer les paramètres de stratégie, les paramètres de stratégie d’audit par utilisateur pour tous les utilisateurs et toutes les options d’audit dans un fichier texte de valeurs séparées par des virgules (CSV).

## <a name="syntax"></a>Syntaxe
```
auditpol /backup /file:<filename>
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/file|Spécifie le nom du fichier auquel la stratégie d’audit sera sauvegardée.|
|/?|Affiche l'aide à l'invite de commandes.|
## <a name="remarks"></a>Notes
pour les opérations de sauvegarde pour les stratégies d’utilisateur et système, vous devez avoir écrire ou jeu d’autorisations de contrôle total sur cet objet dans le descripteur de sécurité. Vous pouvez également effectuer des opérations de sauvegarde en utilisant le **gérer le journal d’audit et de sécurité** droit d’utilisateur (SeSecurityPrivilege). Toutefois, ce droit permet un accès supplémentaire qui n’est pas nécessaire d’effectuer l’opération de liste.
## <a name="BKMK_examples"></a>Exemples
Pour sauvegarder d’audit par utilisateur pour tous les utilisateurs, système, les paramètres de stratégie d’audit de paramètres de stratégie et d’audit de toutes les options dans un fichier texte au format CSV nommé auditpolicy.csv, type :
```
auditpol /backup /file:C:\auditpolicy.csv 
```
> [!NOTE]
> Si aucun lecteur n’est spécifié, le répertoire actif est utilisé.
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[auditpol restauration](auditpol-restore.md)
