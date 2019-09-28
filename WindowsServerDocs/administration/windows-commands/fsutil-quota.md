---
ms.assetid: 21225c11-7c72-4ea2-96bd-e63d4beb3be5
title: Fsutil quota
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 5e1c6793ca866ecacd8b00aa7e01d632c2538405
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71376843"
---
# <a name="fsutil-quota"></a>Fsutil quota
>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Gère les quotas de disque sur les volumes NTFS pour un contrôle plus précis du stockage basé sur le réseau.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
fsutil quota [disable] <VolumePath>
fsutil quota [enforce] <VolumePath>
fsutil quota [modify] <VolumePath> <Threshold> <Limit> <UserName>
fsutil quota [query] <VolumePath>
fsutil quota [track] <VolumePath>
fsutil quota [violations]
```

## <a name="parameters"></a>Paramètres

|   Paramètre   |                                                                                    Description                                                                                    |
|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    désactivation    |                                                         Désactive le suivi et l’application du quota sur le volume spécifié.                                                          |
|    oeuvre    |                                                                   Applique l’utilisation du quota sur le volume spécifié.                                                                   |
|    modifier     |                                                              Modifie un quota de disque existant ou crée un quota.                                                              |
|     requête     |                                                                            Répertorie les quotas de disque existants.                                                                            |
|     assurer     |                                                                    Effectue le suivi de l’utilisation du disque sur le volume spécifié.                                                                     |
|  Atteintes   | Recherche dans les journaux système et d’application et affiche un message indiquant que des violations de quota ont été détectées ou qu’un utilisateur a atteint un seuil de quota ou une limite de quota. |
| @no__t 0VolumePath > |                                  Obligatoire. Spécifie le nom du lecteur suivi d’un signe deux-points ou du GUID au format **volume {** <em>GUID</em> **}** .                                  |
| @no__t 0Threshold >  |                            Définit la limite (en octets) à laquelle les avertissements sont émis. Ce paramètre est obligatoire pour la commande **fsutil quota Modify** .                            |
|   @no__t 0Limit >    |                                Définit l’utilisation maximale autorisée du disque (en octets). Ce paramètre est obligatoire pour la commande **fsutil quota Modify** .                                |
|  @no__t 0UserName >  |                                      Spécifie le nom de domaine ou d’utilisateur. Ce paramètre est obligatoire pour la commande **fsutil quota Modify** .                                       |

## <a name="remarks"></a>Notes

-   Les quotas de disque sont implémentés en fonction du volume, et ils permettent d’implémenter les limites de stockage matérielles et logicielles par utilisateur.

-   Vous pouvez utiliser des scripts d’écriture qui utilisent le **quota fsutil** pour définir les limites de quota chaque fois que vous ajoutez un nouvel utilisateur ou pour suivre automatiquement les limites de quota, les compiler dans un rapport et les envoyer automatiquement à l’administrateur système par courrier électronique.

### <a name="BKMK_examples"></a>Illustre
Pour répertorier les quotas de disque existants pour un volume de disque spécifié avec le GUID {928842df-5A01-11de-a85c-806e6f6e6963}, tapez :

```
fsutil quota query Volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

Pour répertorier les quotas de disque existants pour un volume de disque spécifié avec la lettre de lecteur **C :** , tapez :

```
Fsutil quota query C:
```

#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


