---
title: writer
description: La rubrique commandes Windows pour l’enregistreur, qui vérifie qu’un enregistreur ou un composant est inclus ou exclut un enregistreur ou un composant de la procédure de sauvegarde ou de restauration.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7cf98295-411d-4705-8573-f898ff45c140
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fb13de162b8e5eb8150d145a4afacccf47bb25f0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828982"
---
# <a name="writer"></a>writer



Vérifie qu’un writer ou un composant est inclus ou exclut un enregistreur ou un composant de la procédure de sauvegarde ou de restauration. En cas d’utilisation sans paramètre, le **writer** affiche l’aide à l’invite de commandes.

## <a name="syntax"></a>Syntaxe

```
writer verify [<Writer> | <Component>]
writer exclude [<Writer> | <Component>]
```

### <a name="parameters"></a>Paramètres

| Paramètre  |                                                                                      Description                                                                                      |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   vérifier   | Vérifie que l’enregistreur ou le composant spécifié est inclus dans la procédure de sauvegarde ou de restauration. La procédure de sauvegarde ou de restauration échoue si l’enregistreur ou le composant n’est pas inclus. |
|  exclude   |                                                   Exclut le writer ou le composant spécifié de la procédure de sauvegarde ou de restauration.                                                    |
| [\<rédacteur > |                                                                                     <Component>]                                                                                      |

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour vérifier un enregistreur en spécifiant son GUID (pour cet exemple, 4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f), tapez :
```
writer verify {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
```
Pour exclure un enregistreur avec le nom System Writer, tapez :
```
writer exclude System Writer
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)