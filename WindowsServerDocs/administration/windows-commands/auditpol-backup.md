---
title: sauvegarde Auditpol
description: Rubrique relative aux commandes Windows pour la **sauvegarde Auditpol**, qui sauvegarde les paramètres de stratégie d’audit du système, les paramètres de stratégie d’audit par utilisateur pour tous les utilisateurs et toutes les options d’audit dans un fichier texte de valeurs séparées par des virgules (CSV).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dc84e581-aa0f-4c91-b13b-1d970bad5517
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8895f312606a6a6c45a77c659a1cd98d115babe3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851212"
---
# <a name="auditpol-backup"></a>sauvegarde Auditpol

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sauvegarde les paramètres de stratégie d’audit système, les paramètres de stratégie d’audit par utilisateur pour tous les utilisateurs et toutes les options d’audit dans un fichier texte de valeurs séparées par des virgules (CSV).

## <a name="syntax"></a>Syntaxe

```
auditpol /backup /file:<filename>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
|-----------|------------- |
| /file | Spécifie le nom du fichier dans lequel la stratégie d’audit sera sauvegardée. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="remarks"></a>Notes

Pour les opérations de sauvegarde pour la stratégie système et la stratégie par utilisateur, vous devez disposer de l’autorisation d’écriture ou de contrôle total sur cet objet défini dans le descripteur de sécurité. Vous pouvez également effectuer des opérations de sauvegarde en possédant le droit **d’utilisateur gérer le journal d’audit et de sécurité** (SeSecurityPrivilege). Toutefois, ce droit autorise un accès supplémentaire qui n’est pas nécessaire pour effectuer l’opération de liste.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour sauvegarder les paramètres de stratégie d’audit par utilisateur pour tous les utilisateurs, les paramètres de stratégie d’audit système et toutes les options d’audit dans un fichier texte au format CSV nommé Auditpolicy. csv, tapez :

```
auditpol /backup /file:C:\auditpolicy.csv
```

> [!NOTE]
> Si aucun lecteur n’est spécifié, le répertoire actif est utilisé.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
- [restauration Auditpol](auditpol-restore.md)
