---
title: Option Set
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 9b9174f219654e99eb9441abe3342c31b5089ef5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384048"
---
# <a name="set-option"></a>Option Set



Définit les options de création de clichés instantanés. S’il est utilisé sans paramètres, l' **option Set** affiche l’aide à l’invite de commandes.

## <a name="syntax"></a>Syntaxe

```
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
```

## <a name="parameters"></a>Paramètres

|     Paramètre     |                                                                                                  Description                                                                                                  |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   [différentielle   |                                                                                                     duplex                                                                                                     |
|  transportables  |                       Spécifie que le cliché instantané ne doit pas encore être importé. Le fichier Metadata. cab peut ensuite être utilisé pour importer le cliché instantané sur le même ordinateur ou sur un autre ordinateur.                       |
| [rollbackrecover] |                     Signale aux rédacteurs d’utiliser la *récupération automatique* pendant l’événement **PostSnapshot** . Cela est utile si le cliché instantané sera utilisé pour la restauration (par exemple, avec l’exploration de données).                      |
|   [txfrecover]    |                                                               Demande le service VSS pour rendre le cliché instantané cohérent au niveau transactionnel pendant la création.                                                                |
|  [noautorecover]  | Arrête les enregistreurs et le système de fichiers d’effectuer les modifications de récupération du cliché instantané dans un état cohérent au niveau transactionnel. **Noautorecover** ne peut pas être utilisé avec **txfrecover** ou **rollbackrecover**. |

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)