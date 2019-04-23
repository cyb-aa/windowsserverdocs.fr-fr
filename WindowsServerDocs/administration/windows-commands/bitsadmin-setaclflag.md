---
title: bitsadmin setaclflag
description: Rubrique de commandes de Windows pour **bitsadmin setaclflag** -définit des indicateurs de propagations de liste de contrôle d’accès.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6e3bcda0-827d-4dfd-8384-d1da018f3e10
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 89d825a4bc4512022fed98a3188537d3977fa3c3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867400"
---
# <a name="bitsadmin-setaclflag"></a>bitsadmin setaclflag

Définit des indicateurs de propagations de liste (ACL) pour le travail le contrôle d’accès. Les indicateurs indiquent que vous souhaitez conserver le propriétaire et les informations ACL avec le fichier en cours de téléchargement. Par exemple, pour conserver le propriétaire et le groupe avec le fichier, définissez **indicateurs** à `OG`.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetAclFlags <Job> <Flags>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom d’affichage ou le GUID du travail|
|Flags|Spécifiez une ou plusieurs des valeurs d’indicateur suivantes :</br>-O: Copier les informations relatives au propriétaire avec le fichier.</br>-G : Copier les informations de groupe avec le fichier.</br>-D: Copier les informations de la DACL par fichier.</br>-S : copie SACL informations avec le fichier.|

## <a name="remarks"></a>Notes

Le commutateur SetAclFlags est utilisé pour gérer les informations de liste de contrôle propriétaire et l’accès quand une tâche télécharge des données à partir d’un partage Windows (SMB).

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant définit le contrôle d’accès d’indicateurs de propagation de liste pour le travail nommé *myDownloadJob* pour conserver les informations de propriétaire et le groupe avec les fichiers téléchargés.
```
C:\>bitsadmin /setaclflags myDownloadJob OG
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)