---
title: sauvegarde Auditpol
description: Rubrique de référence pour la commande de sauvegarde Auditpol, qui sauvegarde les paramètres de stratégie d’audit du système, les paramètres de stratégie d’audit par utilisateur pour tous les utilisateurs et toutes les options d’audit dans un fichier texte de valeurs séparées par des virgules (CSV).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: dc84e581-aa0f-4c91-b13b-1d970bad5517
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ddc6bbbc379453c86df27674b57f29f7c0960772
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719169"
---
# <a name="auditpol-backup"></a>sauvegarde Auditpol

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sauvegarde les paramètres de stratégie d’audit système, les paramètres de stratégie d’audit par utilisateur pour tous les utilisateurs et toutes les options d’audit dans un fichier texte de valeurs séparées par des virgules (CSV).

Pour effectuer des opérations de *sauvegarde* sur les stratégies *système* et *par utilisateur* , vous devez disposer de l’autorisation d' **écriture** ou de **contrôle total** pour cet objet défini dans le descripteur de sécurité. Vous pouvez également effectuer des opérations de *sauvegarde* si vous disposez du droit **d’utilisateur gérer le journal d’audit et de sécurité** (SeSecurityPrivilege). Toutefois, ce droit autorise un accès supplémentaire qui n’est pas nécessaire pour effectuer les opérations de *sauvegarde* globales.

## <a name="syntax"></a>Syntaxe

```
auditpol /backup /file:<filename>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
|-----------|------------- |
| /file | Spécifie le nom du fichier dans lequel la stratégie d’audit sera sauvegardée. |
| /? | Affiche l'aide à l'invite de commandes. |

## <a name="examples"></a>Exemples

Pour sauvegarder les paramètres de stratégie d’audit par utilisateur pour tous les utilisateurs, les paramètres de stratégie d’audit système et toutes les options d’audit dans un fichier texte au format CSV nommé Auditpolicy. csv, tapez :

```
auditpol /backup /file:C:\auditpolicy.csv
```

> [!NOTE]
> Si aucun lecteur n’est spécifié, le répertoire actif est utilisé.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [restauration Auditpol](auditpol-restore.md)

- [commandes Auditpol](auditpol.md)
