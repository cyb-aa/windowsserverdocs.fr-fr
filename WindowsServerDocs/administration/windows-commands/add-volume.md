---
title: Ajouter un volume
description: Rubrique relative aux commandes Windows pour **Ajouter un volume**, qui ajoute des volumes au jeu de clichés instantanés, qui est l’ensemble des volumes pour lesquels des clichés instantanés sont ajoutés.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b7d4d35d-8bda-46d2-8df5-eb598cecaaba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 806ab273dbb63eb7341520f56a07691fe3fac214
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851352"
---
# <a name="add-volume"></a>Ajouter un volume

Ajoute des volumes au jeu de clichés instantanés, qui est l’ensemble des volumes pour lesquels des clichés instantanés sont ajoutés. Cette commande est nécessaire pour créer des clichés instantanés. S’il est utilisé sans paramètres, **Ajouter un volume** affiche l’aide à l’invite de commandes.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
add volume <Volume> [provider <ProviderID>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
| `<Volume>` | Spécifie un volume à ajouter au jeu de clichés instantanés. Au moins un volume est requis pour la création de clichés instantanés.|
| `[provider \<ProviderID>]` | Spécifie l’ID de fournisseur d’un fournisseur inscrit à utiliser pour créer le cliché instantané. Si le **fournisseur** n’est pas spécifié, le fournisseur par défaut est utilisé.|

## <a name="remarks"></a>Notes

-   Les volumes sont ajoutés un par un.

-   Chaque fois qu’un volume est ajouté, il est vérifié pour s’assurer que VSS prend en charge la création de clichés instantanés de ce volume. Cette vérification principale peut toutefois être invalidée en utilisant ultérieurement la commande **set context** .

-   Lorsqu’un cliché instantané est créé, une variable d’environnement lie l’alias à l’ID d’ombre, de sorte que l’alias peut ensuite être utilisé pour l’écriture de scripts.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour afficher la liste actuelle des fournisseurs inscrits, à l’invite `DISKSHADOW>`, tapez :

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

Pour ajouter le lecteur C au jeu de clichés instantanés et affecter un alias nommé System1, tapez :

```
add volume c: alias System1
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)