---
title: Ajouter un volume
description: Rubrique de référence pour la commande Add volume, qui ajoute des volumes au jeu de clichés instantanés, qui est l’ensemble des volumes pour lesquels des clichés instantanés sont ajoutés.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b7d4d35d-8bda-46d2-8df5-eb598cecaaba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a8cfd3d8f7d9f008e3136d8f694dc00370b8b0f2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719208"
---
# <a name="add-volume"></a>Ajouter un volume

Ajoute des volumes au jeu de clichés instantanés, qui est l’ensemble des volumes pour lesquels des clichés instantanés sont ajoutés. Lorsqu’un cliché instantané est créé, une variable d’environnement lie l’alias à l’ID d’ombre, de sorte que l’alias peut ensuite être utilisé pour l’écriture de scripts.

Les volumes sont ajoutés un par un. Chaque fois qu’un volume est ajouté, il est vérifié pour s’assurer que VSS prend en charge la création de clichés instantanés pour ce volume. Cette vérification peut être invalidée par une utilisation ultérieure de la commande **set context** .

Cette commande est nécessaire pour créer des clichés instantanés. S’il est utilisé sans paramètres, **Ajouter un volume** affiche l’aide à l’invite de commandes.

## <a name="syntax"></a>Syntaxe

```
add volume <volume> [provider <providerid>]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| `<volume>` | Spécifie un volume à ajouter au jeu de clichés instantanés. Au moins un volume est requis pour la création de clichés instantanés. |
| `[provider \<providerid>]` | Spécifie l’ID de fournisseur d’un fournisseur inscrit à utiliser pour créer le cliché instantané. Si le **fournisseur** n’est pas spécifié, le fournisseur par défaut est utilisé. |

## <a name="examples"></a>Exemples

Pour afficher la liste actuelle des fournisseurs inscrits, à l' `diskshadow>` invite de commandes, tapez :

```
list providers
```

La sortie suivante affiche un fournisseur unique, qui sera utilisé par défaut :

```
* ProviderID: {b5946137-7b9f-4925-af80-51abd60b20d5}
        Type: [1] VSS_PROV_SYSTEM
        Name: Microsoft Software Shadow Copy provider 1.0
        Version: 1.0.0.7
        CLSID: {65ee1dba-8ff4-4a58-ac1c-3470ee2f376a}
1 provider registered.
```

Pour ajouter le lecteur C : au jeu de clichés instantanés et affecter un alias nommé *system1*, tapez :

```
add volume c: alias System1
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [définir la commande de contexte](set-context.md)
