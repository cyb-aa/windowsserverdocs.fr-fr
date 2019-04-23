---
title: bitsadmin getaclflags
description: Rubrique de commandes de Windows pour **bitsadmin getaclflags** -récupère les indicateurs de propagations de liste de contrôle l’accès.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 185445a97168344f910abc0e644718296de2c712
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861450"
---
# <a name="bitsadmin-getaclflags"></a>bitsadmin getaclflags

Récupère les indicateurs de propagations accès contrôle liste (ACL).

## <a name="syntax"></a>Syntaxe

```
bitsadmin /GetAclFlags <Job>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|

## <a name="remarks"></a>Notes

Affiche une ou plusieurs des valeurs d’indicateur suivantes :
-   O : Copier les informations relatives au propriétaire avec le fichier.
-   G : Copier les informations de groupe avec le fichier.
-   D: Copier les informations de la DACL par fichier.
-   %S : Copier les informations de la liste SACL avec le fichier.

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant récupère les indicateurs de propagation de liste de contrôle l’accès pour le travail nommé *myDownloadJob*.
```
C:\>bitsadmin /getaclflags myDownloadJob
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)