---
title: Option SET
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4d8d4921-9fdd-4a3c-bb0f-9df5458c4b84
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c4756627d19d296d02fa11ac67ef80080ddf318
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441366"
---
# <a name="set-option"></a>Option SET



Définit les options de création de clichés instantanés. Si utilisée sans paramètres, **définir option** affiche l’aide à l’invite de commandes.

## <a name="syntax"></a>Syntaxe

```
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
```

## <a name="parameters"></a>Paramètres

|     Paramètre     |                                                                                                  Description                                                                                                  |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   [différentielle   |                                                                                                     plex]                                                                                                     |
|  [transportable]  |                       Spécifie que le cliché instantané doit ne pas être a encore été importé. Le fichier .cab de métadonnées peut être utilisé ultérieurement pour importer le cliché instantané sur le même ou un autre ordinateur.                       |
| [rollbackrecover] |                     Signale des writers à utiliser *récupération automatique* pendant la **PostSnapshot** événement. Cela est utile si le cliché instantané doit être utilisé pour la restauration (par exemple, avec l’exploration de données).                      |
|   [txfrecover]    |                                                               Demandes VSS pour rendre le cliché instantané transactionnellement cohérente lors de la création.                                                                |
|  [noautorecover]  | Les enregistreurs s’arrête et le système de fichiers à partir d’apporter toute modification de récupération pour le cliché instantané à un état transactionnellement cohérent. **Noautorecover** ne peut pas être utilisé avec **txfrecover** ou **rollbackrecover**. |

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)