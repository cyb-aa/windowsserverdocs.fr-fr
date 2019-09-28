---
title: restauration Auditpol
description: La rubrique commandes Windows pour la **restauration Auditpol-restaure** les paramètres de stratégie d’audit système, les paramètres de stratégie d’audit par utilisateur pour tous les utilisateurs et toutes les options d’audit à partir d’un fichier dont la syntaxe est conforme au format de fichier de valeurs séparées par des virgules (CSV). utilisé par l’option/backup.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: b91f3745354c695c4ab0c71b429718bff05d8098
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382409"
---
# <a name="auditpol-restore"></a>restauration Auditpol

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Restaure les paramètres de stratégie d’audit du système, les paramètres de stratégie d’audit par utilisateur pour tous les utilisateurs et toutes les options d’audit à partir d’un fichier dont la syntaxe est conforme au format de fichier CSV (Comma-Separated Value) utilisé par l’option/backup.

## <a name="syntax"></a>Syntaxe
```
auditpol /restore /file:<filename>
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/file|Spécifie le fichier à partir duquel la stratégie d’audit doit être restaurée. Le fichier doit avoir été créé à l’aide de l’option/backup ou être syntaxiquement cohérent avec le format de fichier CSV utilisé par l’option/backup.|
|/?|Affiche l'aide à l'invite de commandes.|
## <a name="remarks"></a>Notes
pour les opérations de restauration pour la stratégie système et la stratégie par utilisateur, vous devez disposer de l’autorisation d’écriture ou de contrôle total sur cet objet défini dans le descripteur de sécurité. Vous pouvez également effectuer l’opération de restauration en possédant le droit **d’utilisateur gérer le journal d’audit et de sécurité** (SeSecurityPrivilege). SeSecurityPrivilege est utile lors de la restauration du descripteur de sécurité en cas d’erreur accidentelle ou d’attaque malveillante.
## <a name="BKMK_examples"></a>Illustre
Pour restaurer les paramètres de stratégie d’audit système, les paramètres de stratégie d’audit par utilisateur pour tous les utilisateurs et toutes les options d’audit à partir d’un fichier nommé Auditpolicy. csv qui a été créé à l’aide de la commande/Backup, tapez :
```
auditpol /restore /file:c:\auditpolicy.csv
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)@no__t[sauvegarde Auditpol](auditpol-backup.md)
