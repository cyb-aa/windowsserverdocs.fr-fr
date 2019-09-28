---
title: bitsadmin getaclflags
description: La rubrique commandes Windows pour **Bitsadmin GETACLFLAGS** -récupère les indicateurs de propagation de la liste de contrôle d’accès.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99266def-7479-4430-a61c-98ec433fa88b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ad98cd742161ae06be5cba7acde7b810eaf199d6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381790"
---
# <a name="bitsadmin-getaclflags"></a>bitsadmin getaclflags

Récupère les indicateurs de propagation de la liste de contrôle d’accès (ACL).

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetAclFlags <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|

## <a name="remarks"></a>Notes

Affiche une ou plusieurs des valeurs d’indicateur suivantes :
-   SORTIES Copiez les informations de propriétaire avec le fichier.
-   ACTIVÉE Copier les informations de groupe avec le fichier.
-   D: Copiez les informations DACL avec le fichier.
-   X Copiez les informations SACL avec le fichier.

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant récupère les indicateurs de propagation de la liste de contrôle d’accès pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin /getaclflags myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)