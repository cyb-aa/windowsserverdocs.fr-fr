---
title: auditpol
description: Rubrique de référence pour la commande Auditpol, qui affiche des informations sur et exécute des fonctions pour manipuler les stratégies d’audit.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a02cfb9d-732f-4e77-aeba-f18265daa3af
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 89fee7ccd3b6671a6f2633c3b5d15d0cbee261fa
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718830"
---
# <a name="auditpol"></a>auditpol

Affiche des informations sur et exécute des fonctions pour manipuler les stratégies d’audit, notamment :

- Définition et interrogation d’une stratégie d’audit du système.

- Définition et interrogation d’une stratégie d’audit par utilisateur.

- Définition et interrogation des options d’audit.

- Définition et interrogation du descripteur de sécurité utilisé pour déléguer l’accès à une stratégie d’audit.

- Création de rapports ou sauvegarde d’une stratégie d’audit dans un fichier texte de valeurs séparées par des virgules (CSV).

- Chargement d’une stratégie d’audit à partir d’un fichier texte CSV.

- Configuration des listes SACL de ressources globales.

## <a name="syntax"></a>Syntaxe

```
auditpol command [<sub-command><options>]
```

### <a name="parameters"></a>Paramètres

| Sous-commande | Description |
| ----------- | ----------- |
| /Get | Affiche la stratégie d’audit actuelle. Pour plus d’informations, consultez extraction de l’outil [Auditpol](auditpol-get.md) pour connaître la syntaxe et les options. |
| /Set | Définit la stratégie d’audit. Pour plus d’informations, consultez [Auditpol Set](auditpol-set.md) pour la syntaxe et les options. |
| /list | Affiche des éléments de stratégie sélectionnables. Pour plus d’informations, consultez la page [liste d’Auditpol](auditpol-list.md) pour connaître la syntaxe et les options. |
| /Backup | Enregistre la stratégie d’audit dans un fichier. Pour plus d’informations, consultez [sauvegarde Auditpol](auditpol-backup.md) pour la syntaxe et les options. |
| /Restore | Restaure la stratégie d’audit à partir d’un fichier qui a été créé précédemment à l’aide d’Auditpol/Backup. Pour plus d’informations, consultez [restauration d’Auditpol](auditpol-restore.md) pour la syntaxe et les options. |
| /Clear | Efface la stratégie d’audit. Pour plus d’informations, consultez l’outil [Auditpol Clear](auditpol-clear.md) pour connaître la syntaxe et les options. |
| /remove | Supprime tous les paramètres de stratégie d’audit par utilisateur et désactive tous les paramètres de stratégie d’audit système. Pour plus d’informations, consultez l’outil [Auditpol Remove](auditpol-remove.md) pour la syntaxe et les options. |
| /resourceSACL | Configure les listes de contrôle d’accès (SACL) du système de ressources globales. **Remarque :** S’applique uniquement à Windows 7 et Windows Server 2008 R2. Pour plus d’informations, consultez [Auditpol resourceSACL](auditpol-resourcesacl.md). |
| /?| Affiche l'aide à l'invite de commandes. |

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
