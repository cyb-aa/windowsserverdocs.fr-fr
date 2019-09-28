---
title: auditpol
description: 'Rubrique relative aux commandes Windows pour **Auditpol** : affiche des informations sur et exécute des fonctions pour manipuler les stratégies d’audit.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 5e249a9e2a07505f052b774208c514b4d16879b8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382380"
---
# <a name="auditpol"></a>auditpol



Affiche des informations sur et exécute des fonctions pour manipuler les stratégies d’audit.

Pour obtenir des exemples d’utilisation de cette commande, consultez la section exemples dans chaque rubrique.

## <a name="syntax"></a>Syntaxe

```
Auditpol command [<sub-command><options>]
```

## <a name="parameters"></a>Paramètres

|Sous-commande|Description|
|-----------|-----------|
|/Get|Affiche la stratégie d’audit actuelle.</br>Pour [obtenir](auditpol-get.md) la syntaxe et les options, consultez auditpol.|
|/Set|Définit la stratégie d’audit.</br>Consultez l' [ensemble d’Auditpol](auditpol-set.md) pour connaître la syntaxe et les options.|
|/List|Affiche des éléments de stratégie sélectionnables.</br>Consultez [liste d’Auditpol](auditpol-list.md) pour connaître la syntaxe et les options.|
|/Backup|Enregistre la stratégie d’audit dans un fichier.</br>Consultez [sauvegarde Auditpol](auditpol-backup.md) pour connaître la syntaxe et les options.|
|/Restore|Restaure la stratégie d’audit à partir d’un fichier qui a été créé précédemment à l’aide d’Auditpol/Backup.</br>Consultez [restauration d’Auditpol](auditpol-restore.md) pour connaître la syntaxe et les options.|
|/Clear|Efface la stratégie d’audit.</br>Consultez l’option [Auditpol Clear](auditpol-clear.md) pour connaître la syntaxe et les options.|
|/remove|Supprime tous les paramètres de stratégie d’audit par utilisateur et désactive tous les paramètres de stratégie d’audit système.</br>Consultez l’outil [Auditpol Remove](auditpol-remove.md) pour connaître la syntaxe et les options.|
|/resourceSACL|Configure les listes de contrôle d’accès (SACL) du système de ressources globales.</br>Remarque : S’applique uniquement à Windows 7 et Windows Server 2008 R2.</br>Consultez [Auditpol resourceSACL](auditpol-resourcesacl.md).|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

L’outil en ligne de commande de la stratégie d’audit peut être utilisé pour :
-   Définir et interroger une stratégie d’audit du système.
-   Définir et interroger une stratégie d’audit par utilisateur.
-   Définissez et interrogez les options d’audit.
-   Définir et interroger le descripteur de sécurité utilisé pour déléguer l’accès à une stratégie d’audit.
-   Créez un rapport ou sauvegardez une stratégie d’audit dans un fichier texte de valeurs séparées par des virgules (CSV).
-   Charger une stratégie d’audit à partir d’un fichier texte CSV.
-   Configurer des listes SACL de ressources globales.

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)