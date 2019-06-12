---
ms.assetid: 21225c11-7c72-4ea2-96bd-e63d4beb3be5
title: quota de fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 1e844d73348ee31f309f44895831ded9a2e6365c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439063"
---
# <a name="fsutil-quota"></a>quota de fsutil
>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Gère les quotas de disque sur les volumes NTFS pour fournir un contrôle plus précis du stockage basé sur le réseau.

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
|    désactivation    |                                                         Désactive le suivi des quotas et mise en œuvre sur le volume spécifié.                                                          |
|    appliquer    |                                                                   Impose l’utilisation du quota sur le volume spécifié.                                                                   |
|    modifier     |                                                              Modifie un quota de disque existant ou crée un nouveau quota.                                                              |
|     requête     |                                                                            Répertorie les quotas de disque.                                                                            |
|     Piste     |                                                                    Utilisation sur le volume spécifié du disque de pistes.                                                                     |
|  violations   | Recherche dans les journaux système et d’application et affiche un message pour indiquer que les violations des quotas ont été détectées ou qu’un utilisateur a atteint une limite de quota ou de seuil de quota. |
| \<VolumePath> |                                  Obligatoire. Spécifie le nom du lecteur suivi d’un signe deux-points ou le GUID dans le format **Volume {** <em>GUID</em> **}** .                                  |
| \<Seuil >  |                            Définit la limite (en octets) à laquelle les avertissements sont émis. Ce paramètre est obligatoire pour le **fsutil quota modifier** commande.                            |
|   \<Limit>    |                                Définit l’utilisation de disque maximal autorisé (en octets). Ce paramètre est obligatoire pour le **fsutil quota modifier** commande.                                |
|  \<UserName>  |                                      Spécifie le nom de domaine ou utilisateur. Ce paramètre est obligatoire pour le **fsutil quota modifier** commande.                                       |

## <a name="remarks"></a>Notes

-   Quotas de disque sont implémentés sur par volume, et ils permettent les deux limites de stockage de matériels et logiciels à mettre en oeuvre sur une base par utilisateur.

-   Vous pouvez utiliser d’écrire des scripts qui utilisent **fsutil quota** pour définir les limites de quota chaque fois que vous ajoutez un nouvel utilisateur ou suivre automatiquement des limites de quota, les compiler dans un rapport et les envoient à l’administrateur système par courrier électronique.

### <a name="BKMK_examples"></a>Exemples
Pour répertorier les quotas de disque existant pour un volume de disque qui est spécifié avec le GUID, {928842df-5a01-11de-a85c-806e6f6e6963}, tapez :

```
fsutil quota query Volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

Pour répertorier les quotas de disque existant pour un volume de disque qui est spécifié par la lettre de lecteur, **C:** , type :

```
Fsutil quota query C:
```

#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


