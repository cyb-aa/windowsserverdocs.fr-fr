---
title: bitsadmin getaclflags
description: Rubrique de référence pour la commande Bitsadmin GETACLFLAGS, qui récupère les indicateurs de propagation de la liste de contrôle d’accès (ACL).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 99266def-7479-4430-a61c-98ec433fa88b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a9ca541b488c3c83e7a64a138bae0914001778e3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718173"
---
# <a name="bitsadmin-getaclflags"></a>bitsadmin getaclflags

Récupère les indicateurs de propagation de la liste de contrôle d’accès (ACL), en indiquant si les éléments sont hérités par les objets enfants.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /getaclflags <job>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| travail | Nom complet ou GUID du travail. |

### <a name="remarks"></a>Notes 

Retourne une ou plusieurs des valeurs d’indicateur suivantes :

- **o** -copier les informations de propriétaire avec le fichier.

- **g** -copier les informations de groupe avec le fichier.

- **d** -copie les informations discrétionnaires sur la liste de contrôle d’accès (DACL) avec le fichier.

- **s** -copier les informations de la liste de contrôle d’accès système (SACL) avec le fichier.

## <a name="examples"></a>Exemples

Pour récupérer les indicateurs de propagation de la liste de contrôle d’accès pour le travail nommé *myDownloadJob*:

```
bitsadmin /getaclflags myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
