---
title: Option Set
description: Rubrique de référence relative à l’option Set, qui définit les options de création de clichés instantanés.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4d8d4921-9fdd-4a3c-bb0f-9df5458c4b84
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a4aba049e29cd74450467cf28057a2ff4e4a7094
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721900"
---
# <a name="set-option"></a>Option Set

Définit les options de création de clichés instantanés. S’il est utilisé sans paramètres, l' **option Set** affiche l’aide à l’invite de commandes.

## <a name="syntax"></a>Syntaxe

```
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
```

### <a name="parameters"></a>Paramètres

|     Paramètre     |                                                                                                  Description                                                                                                  |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   [différentielle   |                                                                                                     duplex                                                                                                     |
|  transportables  |                       Spécifie que le cliché instantané ne doit pas encore être importé. Le fichier Metadata. cab peut ensuite être utilisé pour importer le cliché instantané sur le même ordinateur ou sur un autre ordinateur.                       |
| [rollbackrecover] |                     Signale aux rédacteurs d’utiliser la *récupération automatique* pendant l’événement **PostSnapshot** . Cela est utile si le cliché instantané sera utilisé pour la restauration (par exemple, avec l’exploration de données).                      |
|   [txfrecover]    |                                                               Demande le service VSS pour rendre le cliché instantané cohérent au niveau transactionnel pendant la création.                                                                |
|  [noautorecover]  | Arrête les enregistreurs et le système de fichiers d’effectuer les modifications de récupération du cliché instantané dans un état cohérent au niveau transactionnel. **Noautorecover** ne peut pas être utilisé avec **txfrecover** ou **rollbackrecover**. |

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)