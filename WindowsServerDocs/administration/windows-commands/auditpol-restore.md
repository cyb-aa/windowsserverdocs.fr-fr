---
title: restauration Auditpol
description: Rubrique de référence pour la commande de restauration Auditpol, qui restaure les paramètres de stratégie d’audit du système, les paramètres de stratégie d’audit par utilisateur pour tous les utilisateurs et toutes les options d’audit d’un fichier dont la syntaxe est conforme au format de fichier de valeurs séparées par des virgules (CSV) utilisé par l’option/backup.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ad73e520-484f-4cf1-a7f9-ae7488e9edf6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 64605a985c1cff13b842a99ae4ea52485bfc8220
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719056"
---
# <a name="auditpol-restore"></a>restauration Auditpol

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Restaure les paramètres de stratégie d’audit du système, les paramètres de stratégie d’audit par utilisateur pour tous les utilisateurs et toutes les options d’audit à partir d’un fichier dont la syntaxe est conforme au format de fichier CSV (Comma-Separated Value) utilisé par l’option/backup.

Pour effectuer des opérations de *restauration* sur les stratégies *système* et *par utilisateur* , vous devez disposer de l’autorisation d' **écriture** ou de **contrôle total** pour cet objet défini dans le descripteur de sécurité. Vous pouvez également effectuer des opérations de *restauration* si vous disposez du droit **d’utilisateur gérer le journal d’audit et de sécurité** (SeSecurityPrivilege), qui est utile lors de la restauration du descripteur de sécurité en cas d’erreur ou d’attaque malveillante.

## <a name="syntax"></a>Syntaxe

```
auditpol /restore /file:<filename>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| ------- | -------- |
| /file | Spécifie le fichier à partir duquel la stratégie d’audit doit être restaurée. Le fichier doit avoir été créé à l’aide de l’option/backup ou être syntaxiquement cohérent avec le format de fichier CSV utilisé par l’option/backup. |
| /? |Affiche l'aide à l'invite de commandes. |

## <a name="examples"></a>Exemples

Pour restaurer les paramètres de stratégie d’audit système, les paramètres de stratégie d’audit par utilisateur pour tous les utilisateurs et toutes les options d’audit à partir d’un fichier nommé Auditpolicy. csv qui a été créé à l’aide de la commande/Backup, tapez :

```
auditpol /restore /file:c:\auditpolicy.csv
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [sauvegarde Auditpol](auditpol-backup.md)

- [commandes Auditpol](auditpol.md)
