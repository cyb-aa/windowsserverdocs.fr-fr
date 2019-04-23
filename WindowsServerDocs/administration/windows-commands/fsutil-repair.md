---
ms.assetid: 62d77150-1d9e-4069-ab4a-299f33024912
title: réparation de fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 18c06b1f426105b098a5dc7e992b1e3becd3a4ca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846680"
---
# <a name="fsutil-repair"></a>réparation de fsutil
>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7

Gère et surveille les NTFS à réparation automatique des opérations de réparation.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe

```
fsutil repair [enumerate] <volumepath> [<LogName>]
fsutil repair [initiate] <VolumePath> <FileReference>
fsutil repair [query] <VolumePath>
fsutil repair [set] <VolumePath> <Flags>
fsutil repair [wait][<WaitType>] <VolumePath>

```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------------|---------------|
|Énumérer|Énumère les entrées du journal de corruption d’un volume.|
|\<volumepath>|Spécifie le volume en tant que le nom du lecteur suivi du signe deux-points.|
|\<LogName>|$Endommagées - l’ensemble des altérations confirmées dans le volume.<br />Vérification des $ - un ensemble des dommages potentiels, non vérifiés dans le volume.|
|lancer|Lance la réparation spontanée NTFS.|
|\<FileReference>|Spécifie l’ID de fichier spécifique au volume NTFS (numéro de référence de fichier). La référence de fichier inclut le numéro de segment du fichier.|
|requête|Interroge l’état de réparation automatique du volume NTFS.|
|jeu|Définit l’état de réparation automatique du volume.|
|\<indicateurs >|Spécifie la méthode de réparation à utiliser lors de la définition de l’état de réparation automatique du volume.<br /><br />Le **indicateurs** paramètre peut être défini sur trois valeurs :<br /><br />-   **0 x 01**: Autorise la réparation générale.<br />-   **0 x 09**: Signale la perte de données sans la réparation.<br />-   **0 x 00**: Désactive NTFS à réparation automatique des opérations de réparation.|
|État|Interroge l’état d’altération du système ou pour un volume donné.|
|attente|Attend que repair(s) terminer. Si NTFS a détecté un problème sur un volume sur lequel il effectue des réparations, cette option permet au système d’attendre que la réparation est terminée avant d’exécuter des scripts en attente.|
|[WaitType {0&#124;1}]|Indique s’il faut attendre que la réparation en cours pour terminer ou pour attendre que toutes les réparations à effectuer. *Type d’attente* peut être définie sur les valeurs suivantes :<br /><br />-   **0**: Attend que toutes les réparations à effectuer. (valeur par défaut)<br />-   **1**: Attend que la réparation en cours terminer.|

## <a name="remarks"></a>Notes

-   Réparation automatique de NTFS pour tente de résoudre les altérations d’en ligne, le système de fichiers NTFS sans nécessiter de **Chkdsk.exe** à exécuter. Cette fonctionnalité a été introduite dans Windows Server 2008. Pour plus d’informations, consultez [NTFS de réparation automatique](https://go.microsoft.com/fwlink/?LinkID=165401).

## <a name="BKMK_examples"></a>Exemples

Pour énumérer les altérations confirmées d’un volume, tapez :

```
fsutil repair enumerate C: $Corrupt 
```

Pour activer la réparation spontanée sur le lecteur C, tapez :

```
fsutil repair set c: 1
```

Pour désactiver la réparation spontanée sur le lecteur C, tapez :

```
fsutil repair set c: 0
```

#### <a name="additional-references"></a>Références supplémentaires
[Clé de la syntaxe de ligne de commande](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[Spontanée de NTFS](https://go.microsoft.com/fwlink/?LinkID=165401)


