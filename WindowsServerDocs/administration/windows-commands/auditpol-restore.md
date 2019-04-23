---
title: restauration de l’outil Auditpol
description: Rubrique de commandes de Windows pour **auditpol restauration** -restaure les paramètres de stratégie d’audit système, les paramètres de stratégie d’audit par utilisateur pour tous les utilisateurs et toutes les options d’audit à partir d’un fichier qui est syntaxiquement cohérent avec la virgule comme séparateur format de fichier CSV (valeurs) utilisé par le /backup option.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ad73e520-484f-4cf1-a7f9-ae7488e9edf6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f1961387083a8a61b27f3e44a2380a6060a02f98
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868980"
---
# <a name="auditpol-restore"></a>restauration de l’outil Auditpol

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Restaure les paramètres de stratégie d’audit système, les paramètres de stratégie d’audit par utilisateur pour tous les utilisateurs et toutes les options d’audit à partir d’un fichier qui est syntaxiquement cohérent avec le format de fichier de valeurs séparées par des virgules (CSV) utilisé par le /backup option.

## <a name="syntax"></a>Syntaxe
```
auditpol /restore /file:<filename>
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/file|Spécifie le fichier à partir de laquelle la stratégie d’audit doit être restaurée. Le fichier doit avoir été créé à l’aide de l’option /backup option ou doit être syntaxiquement cohérent avec le format de fichier CSV utilisé par le /backup option.|
|/?|Affiche l'aide à l'invite de commandes.|
## <a name="remarks"></a>Notes
pour les opérations de restauration pour les stratégies d’utilisateur et système, vous devez avoir écrire ou jeu d’autorisations de contrôle total sur cet objet dans le descripteur de sécurité. Vous pouvez également effectuer l’opération de restauration en utilisant le **gérer le journal d’audit et de sécurité** droit d’utilisateur (SeSecurityPrivilege). SeSecurityPrivilege est utile lorsque vous restaurez le descripteur de sécurité en cas d’erreur par inadvertance ou d’une attaque malveillante.
## <a name="BKMK_examples"></a>Exemples
Pour restaurer les paramètres de stratégie d’audit système, les paramètres de stratégie d’audit par utilisateur pour tous les utilisateurs et toutes les options d’audit à partir d’un fichier nommé auditpolicy.csv qui a été créé à l’aide de l’option /backup de commandes, tapez :
```
auditpol /restore /file:c:\auditpolicy.csv
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[auditpol sauvegarde](auditpol-backup.md)
