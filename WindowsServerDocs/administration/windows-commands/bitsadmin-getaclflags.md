---
title: bitsadmin getaclflags
description: La rubrique commandes Windows pour **Bitsadmin GETACLFLAGS**, qui récupère les indicateurs de propagation de la liste de contrôle d’accès (ACL).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 99266def-7479-4430-a61c-98ec433fa88b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d53018e2fa5c659c8cf4b0ec985beda848a8c1af
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850792"
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
| le travail | Nom complet ou GUID du travail. |

## <a name="remarks"></a>Notes

Affiche une ou plusieurs des valeurs d’indicateur suivantes :

- **o** -copier les informations de propriétaire avec le fichier.

- **g** -copier les informations de groupe avec le fichier.

- **d** -copie les informations discrétionnaires sur la liste de contrôle d’accès (DACL) avec le fichier.

- **s** -copier les informations de la liste de contrôle d’accès système (SACL) avec le fichier.

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant récupère les indicateurs de propagation de la liste de contrôle d’accès pour le travail nommé *myDownloadJob*.

```
C:\>bitsadmin /getaclflags myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)