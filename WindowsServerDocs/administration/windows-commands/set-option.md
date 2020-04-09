---
title: Option Set
description: La rubrique commandes Windows pour set option, qui définit les options de création de clichés instantanés.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4d8d4921-9fdd-4a3c-bb0f-9df5458c4b84
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a956c74619f3a6c33dfcaa62ee4c810cd93b21f1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834492"
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