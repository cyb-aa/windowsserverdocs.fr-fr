---
ms.assetid: 385a2a7c-d6bd-4f11-9c18-fca0413f9e97
title: Fsutil Dirty
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 01b5490ef7c57e48a43cae15902e03a33794a826
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377013"
---
# <a name="fsutil-dirty"></a>Fsutil Dirty
>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Interroge ou définit le bit d’intégrité d’un volume. Lorsque le bit d’intégrité d’un volume est défini, **Autochk** vérifie automatiquement si le volume ne comporte pas d’erreurs lors du prochain redémarrage de l’ordinateur.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
fsutil dirty {query | set} <VolumePath>
```

## <a name="parameters"></a>Paramètres

|   Paramètre   |                                                 Description                                                  |
|---------------|--------------------------------------------------------------------------------------------------------------|
|     requête     |                                  Interroge le bit d’intégrité du volume spécifié.                                   |
|      set      |                                    Définit le bit d’intégrité du volume spécifié.                                    |
| \<VolumePath > | Spécifie le nom du lecteur suivi d’un signe deux-points ou d’un GUID au format suivant : **volume {** <em>GUID</em> **}** . |

## <a name="remarks"></a>Notes

-   Le bit d’intégrité d’un volume indique que le système de fichiers est peut-être dans un état incohérent. Le bit d’intégrité peut être défini pour les raisons suivantes :

    -   Le volume est en ligne et il contient des modifications en suspens.

    -   Des modifications ont été apportées au volume et l’ordinateur a été arrêté avant que les modifications n’aient été validées sur le disque.

    -   Une altération a été détectée sur le volume.

-   Si le bit d’intégrité est défini au redémarrage de l’ordinateur, **chkdsk** s’exécute pour vérifier l’intégrité du système de fichiers et tenter de résoudre les problèmes liés au volume.

## <a name="BKMK_examples"></a>Illustre
Pour interroger le bit d’intégrité sur le lecteur C, tapez :

```
fsutil dirty query c:
```

-   Si le volume est incorrect, la sortie suivante s’affiche :

    `Volume C: is dirty`

-   Si le volume n’est pas modifié, la sortie suivante s’affiche :

    `Volume C: is not dirty`

Pour définir le bit d’intégrité sur le lecteur C, tapez :

```
fsutil dirty set C:
```

#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


