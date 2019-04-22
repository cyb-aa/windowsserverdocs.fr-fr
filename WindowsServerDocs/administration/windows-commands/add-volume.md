---
title: Ajouter un volume
description: Rubrique de commandes de Windows pour **ajouter un volume** -ajoute des volumes pour le cliché instantané, qui est l’ensemble de volumes pour être des clichés instantanés copié.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b7d4d35d-8bda-46d2-8df5-eb598cecaaba
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8960ffafdf49d4512e1df2dfcc046bdfbe56e224
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819470"
---
# <a name="add-volume"></a>Ajouter un volume



Ajoute des volumes pour les clichés instantanés copie définir, qui est l’ensemble de volumes de clichés instantanés. Cette commande n’est nécessaire pour créer des clichés instantanés. Si utilisée sans paramètres, **ajouter un volume** affiche l’aide à l’invite de commandes.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
add volume <Volume> [provider <ProviderID>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|\<Volume>|Spécifie un volume à ajouter à l’ensemble de la copie de clichés instantanés. Au moins un volume est requis pour la création de clichés instantanés.|
|[fournisseur \<ProviderID >]|Spécifie l’ID de fournisseur d’un fournisseur inscrit à utiliser pour créer le cliché instantané. Si **fournisseur** n’est pas spécifié, le fournisseur par défaut est utilisé.|

## <a name="remarks"></a>Notes

-   Les volumes sont ajoutés à la fois.
-   Chaque fois qu’un volume est ajouté, il est vérifié pour s’assurer que VSS prend en charge la création de clichés instantanés de ce volume. Cette vérification principale risquent d’être invalidée, toutefois, par une utilisation ultérieure de la **définir le contexte** commande.
-   Lorsqu’un cliché instantané est créé, une variable d’environnement lie l’alias à l’ID du cliché instantané, donc l’alias peut ensuite servir pour le script.

## <a name="BKMK_examples"></a>Exemples

Pour afficher la liste actuelle des fournisseurs inscrits, à le `DISKSHADOW>` invite, tapez :
```
list providers
```
La sortie suivante affiche un seul fournisseur, qui sera utilisé par défaut :
```
* ProviderID: {b5946137-7b9f-4925-af80-51abd60b20d5}
        Type: [1] VSS_PROV_SYSTEM
        Name: Microsoft Software Shadow Copy provider 1.0
        Version: 1.0.0.7
        CLSID: {65ee1dba-8ff4-4a58-ac1c-3470ee2f376a}
1 provider registered.
```
Pour ajouter le lecteur C à l’ensemble de la copie de clichés instantanés et attribuer un alias nommé System1, tapez :
```
add volume c: alias System1
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)