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
ms.openlocfilehash: 4bb320d9192848cd7a6719c58bde4111798a799e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844212"
---
# <a name="fsutil-quota"></a>Fsutil quota
>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

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

### <a name="parameters"></a>Paramètres

|   Paramètre   |                                                                                    Description                                                                                    |
|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    désactivation    |                                                         Désactive le suivi et l’application du quota sur le volume spécifié.                                                          |
|    oeuvre    |                                                                   Applique l’utilisation du quota sur le volume spécifié.                                                                   |
|    modify     |                                                              Modifie un quota de disque existant ou crée un quota.                                                              |
|     query     |                                                                            Répertorie les quotas de disque existants.                                                                            |
|     assurer     |                                                                    Effectue le suivi de l’utilisation du disque sur le volume spécifié.                                                                     |
|  atteintes   | Recherche dans les journaux système et d’application et affiche un message indiquant que des violations de quota ont été détectées ou qu’un utilisateur a atteint un seuil de quota ou une limite de quota. |
| \<VolumePath > |                                  Obligatoire. Spécifie le nom du lecteur suivi d’un signe deux-points ou du GUID au format **volume {** <em>GUID</em> **}** .                                  |
| Seuil de \<>  |                            Définit la limite (en octets) à laquelle les avertissements sont émis. Ce paramètre est obligatoire pour la commande **fsutil quota Modify** .                            |
|   Limite de \<>    |                                Définit l’utilisation maximale autorisée du disque (en octets). Ce paramètre est obligatoire pour la commande **fsutil quota Modify** .                                |
|  Nom d’utilisateur \<>  |                                      Spécifie le nom de domaine ou d’utilisateur. Ce paramètre est obligatoire pour la commande **fsutil quota Modify** .                                       |

## <a name="remarks"></a>Notes

-   Les quotas de disque sont implémentés en fonction du volume, et ils permettent d’implémenter les limites de stockage matérielles et logicielles par utilisateur.

-   Vous pouvez utiliser des scripts d’écriture qui utilisent le **quota fsutil** pour définir les limites de quota chaque fois que vous ajoutez un nouvel utilisateur ou pour suivre automatiquement les limites de quota, les compiler dans un rapport et les envoyer automatiquement à l’administrateur système par courrier électronique.

### <a name="examples"></a><a name="BKMK_examples"></a>Illustre
Pour répertorier les quotas de disque existants pour un volume de disque spécifié avec le GUID {928842df-5A01-11de-a85c-806e6f6e6963}, tapez :

```
fsutil quota query Volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

Pour répertorier les quotas de disque existants pour un volume de disque spécifié avec la lettre de lecteur **C :** , tapez :

```
Fsutil quota query C:
```

## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[Fsutil](Fsutil.md)


