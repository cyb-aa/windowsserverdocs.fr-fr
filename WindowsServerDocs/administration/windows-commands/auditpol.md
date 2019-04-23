---
title: auditpol
description: Rubrique de commandes de Windows pour **auditpol** - affiche des informations sur et effectue des fonctions pour manipuler des stratégies d’audit.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a02cfb9d-732f-4e77-aeba-f18265daa3af
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d7e8364be977e868ac161704e67c37ec5c400e49
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849220"
---
# <a name="auditpol"></a>auditpol



Affiche des informations et effectue des fonctions pour manipuler des stratégies d’audit.

Pour obtenir des exemples d’utilisation de cette commande, consultez la section exemples dans chaque rubrique.

## <a name="syntax"></a>Syntaxe

```
Auditpol command [<sub-command><options>]
```

## <a name="parameters"></a>Paramètres

|Sous-commande|Description|
|-----------|-----------|
|/get|Affiche la stratégie d’audit actuelle.</br>Consultez [Auditpol get](auditpol-get.md) pour la syntaxe et les options.|
|/set|Définit la stratégie d’audit.</br>Consultez [Auditpol set](auditpol-set.md) pour la syntaxe et les options.|
|/list|Affiche les éléments de la stratégie peut être sélectionné.</br>Consultez [Auditpol liste](auditpol-list.md) pour la syntaxe et les options.|
|/backup|Enregistre la stratégie d’audit dans un fichier.</br>Consultez [Auditpol sauvegarde](auditpol-backup.md) pour la syntaxe et les options.|
|/restore|Restaure la stratégie d’audit à partir d’un fichier qui a été créé précédemment à l’aide d’auditpol /backup.</br>Consultez [Auditpol restauration](auditpol-restore.md) pour la syntaxe et les options.|
|/clear|Efface la stratégie d’audit.</br>Consultez [Auditpol clair](auditpol-clear.md) pour la syntaxe et les options.|
|/remove|Supprime tous les paramètres de stratégie d’audit par utilisateur et désactive tous les paramètres de stratégie d’audit système.</br>Consultez [Auditpol remove](auditpol-remove.md) pour la syntaxe et les options.|
|/resourceSACL|Configure les listes de contrôle d’accès système (SACL) ressources globales.</br>Remarque: S’applique uniquement à Windows 7 et Windows Server 2008 R2.</br>Consultez [Auditpol resourceSACL](auditpol-resourcesacl.md).|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

L’outil de ligne de commande de stratégie d’audit peut être utilisé pour :
-   Définir et interroger une stratégie d’audit système.
-   Définir et interroger une stratégie d’audit par utilisateur.
-   Définir et interroger des options d’audit.
-   Définir et interroger le descripteur de sécurité utilisé pour déléguer l’accès à une stratégie d’audit.
-   Signaler ou sauvegarder une stratégie d’audit dans un fichier texte de valeurs séparées par des virgules (CSV).
-   Charger une stratégie d’audit à partir d’un fichier texte CSV.
-   Configurer les ressources globales SACL.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)