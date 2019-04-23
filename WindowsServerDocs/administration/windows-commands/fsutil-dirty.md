---
ms.assetid: 385a2a7c-d6bd-4f11-9c18-fca0413f9e97
title: fsutil dirty
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: c308b0497a5a39a25384b22441b733143df8727b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852130"
---
# <a name="fsutil-dirty"></a>fsutil dirty
>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Les requêtes ou définit le bit d’intégrité d’un volume. Lorsqu’un volume's erroné bit est défini, **autochk** vérifie automatiquement le volume pour les erreurs lors du prochain redémarrage de l’ordinateur.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
fsutil dirty {query | set} <VolumePath>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------------|---------------|
|requête|Interroge le bit d’intégrité du volume spécifié.|
|jeu|Définit le bit d’intégrité du volume spécifié.|
|\<VolumePath>|Spécifie le nom du lecteur suivi d’un signe deux-points ou d’un GUID au format suivant : **Volume{***GUID***}**.|

## <a name="remarks"></a>Notes

-   Bit d’intégrité d’un volume indique que le système de fichiers peut être dans un état incohérent. Le bit d’intégrité peut être défini, car :

    -   Le volume est en ligne et des modifications en attente.

    -   Modifications apportées au volume et l’ordinateur a été arrêté avant que les modifications ont été validées sur le disque.

    -   Dommages ont été détectés sur le volume.

-   Si le bit d’intégrité est défini lorsque l’ordinateur redémarre, **chkdsk** s’exécute pour vérifier l’intégrité du système et à tenter de résoudre les problèmes avec le volume.

## <a name="BKMK_examples"></a>Exemples
Pour interroger le bit d’intégrité sur le lecteur C, tapez :

```
fsutil dirty query c:
```

-   Si le volume a été modifié, la sortie suivante s’affiche :

    `Volume C: is dirty`

-   Si le volume n’est pas modifié, la sortie suivante s’affiche :

    `Volume C: is not dirty`

Pour définir le bit d’intégrité sur le lecteur C, tapez :

```
fsutil dirty set C:
```

#### <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


