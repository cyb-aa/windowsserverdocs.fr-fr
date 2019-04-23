---
title: récupérer
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8cc3a73d-9456-41a0-b375-2b4cc37c3992
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 261bfd79d74323ad324246e21b84a5eb798ebcdf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867180"
---
# <a name="recover"></a>récupérer



Actualise l’état de tous les disques dans un groupe de disques, tenter de récupérer les disques dans un groupe de disques non valide et resynchronise les volumes en miroir et volumes RAID-5 qui contient des données obsolètes.

> [!IMPORTANT]
> Cette commande DiskPart n’est pas disponible dans n’importe quelle édition de Windows Vista.

## <a name="syntax"></a>Syntaxe

```
recover [noerr]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|NOERR|Pour les scripts uniquement. Lorsqu’une erreur est rencontrée, DiskPart continue à traiter les commandes comme si l’erreur ne s’est pas produite. Sans ce paramètre, une erreur provoque la fermeture avec un code d’erreur de DiskPart.|

## <a name="remarks"></a>Notes

-   Cette commande fonctionne sur un groupe de disques.
-   Cette commande s’applique uniquement aux groupes de disques dynamiques. Si cette commande est utilisée sur un groupe avec un disque de base, il ne retourne pas une erreur, mais aucune action n’est entreprise.
-   Cette commande fonctionne sur les disques qui ont échoué ou ayant échoué. Il fonctionne également sur les volumes qui ont échoué, échec, ou dans un état Échec de la redondance.
-   Un disque qui fait partie d’un groupe de disques doit être sélectionné pour cette commande réussisse. Utilisez le **sélectionnez disque** commande pour sélectionner un disque et de déplacer le focus vers elle.

## <a name="BKMK_examples"></a>Exemples

Pour récupérer le groupe de disques qui contient le disque qui a le focus, tapez :
```
recover
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

