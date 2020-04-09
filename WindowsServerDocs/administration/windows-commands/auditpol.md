---
title: auditpol
description: La rubrique commandes Windows pour **Auditpol**, qui affiche des informations sur et exécute des fonctions pour manipuler les stratégies d’audit.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a02cfb9d-732f-4e77-aeba-f18265daa3af
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 00365b0e46b8bff761cf991dbdbd09d8f5e9c687
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851132"
---
# <a name="auditpol"></a>auditpol

Affiche des informations sur et exécute des fonctions pour manipuler les stratégies d’audit.

Pour obtenir des exemples d’utilisation de cette commande, consultez la section exemples dans chaque rubrique.

## <a name="syntax"></a>Syntaxe

```
auditpol command [<sub-command><options>]
```

#### <a name="parameters"></a>Paramètres

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

## <a name="remarks"></a>Notes

L’outil en ligne de commande de la stratégie d’audit peut être utilisé pour :

- Définir et interroger une stratégie d’audit du système.

- Définir et interroger une stratégie d’audit par utilisateur.

- Définissez et interrogez les options d’audit.

- Définir et interroger le descripteur de sécurité utilisé pour déléguer l’accès à une stratégie d’audit.

- Créez un rapport ou sauvegardez une stratégie d’audit dans un fichier texte de valeurs séparées par des virgules (CSV).

- Charger une stratégie d’audit à partir d’un fichier texte CSV.

- Configurer des listes SACL de ressources globales.

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)