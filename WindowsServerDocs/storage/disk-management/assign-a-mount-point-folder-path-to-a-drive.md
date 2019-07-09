---
title: Attribuer un chemin d’accès de dossier de point de montage à un lecteur
description: Cet article décrit comment attribuer un chemin d’accès de dossier de point de montage à un lecteur (plutôt qu’une lettre de lecteur).
keywords: virtualisation, sécurité, programmes malveillants
ms.date: 06/07/2019
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: d09024b3c7f7a1e55c9e9c2ece56e037fe7e16f2
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66812499"
---
# <a name="assign-a-mount-point-folder-path-to-a-drive"></a>Attribuer un chemin d’accès de dossier de point de montage à un lecteur

> **S’applique à :** Windows 10, Windows 8.1, Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez utiliser l’outil Gestion des disques pour attribuer un chemin d’accès de dossier de point de montage au lecteur (plutôt qu’une lettre de lecteur). Les chemins d’accès de dossier de point de montage sont disponibles uniquement pour les dossiers vides de volumes NTFS de base ou dynamiques.

## <a name="assigning-a-mount-point-folder-path-to-a-drive"></a>Attribution d’un chemin d’accès de dossier de point de montage à un lecteur

> [!NOTE]
> Vous devez au moins être membre du groupe **Opérateurs de sauvegarde** ou **Administrateurs** pour effectuer ces étapes.

#### <a name="to-assign-a-mount-point-folder-path-to-a-drive-by-using-the-windows-interface"></a>Pour attribuer un chemin d’accès de dossier de point de montage à un lecteur à l’aide de l’interface Windows

1.  Dans le Gestionnaire de disque, cliquez avec le bouton droit sur la partition ou le volume auquel vous souhaitez attribuer le chemin d’accès de dossier de point de montage. 
2. Cliquez sur **Modifier la lettre de lecteur et les chemins d’accès**, puis cliquez sur **Ajouter**. 
3. Cliquez sur **Monter dans le dossier NTFS vide suivant**.
4. Tapez le chemin d’un dossier vide sur le volume NTFS, ou cliquez sur **Parcourir** pour le localiser.

#### <a name="to-assign-a-mount-point-folder-path-to-a-drive-using-a-command-line"></a>Pour attribuer un chemin d’accès de dossier de point de montage à un lecteur à l’aide d’une ligne de commande

1.  Ouvrez une invite de commande et tapez `diskpart`.

2.  À l’invite **DISKPART**, tapez `list volume` en veillant à noter le numéro de volume auquel vous voulez attribuer le chemin d’accès.

3.  À l’invite **DISKPART**, tapez `select volume <volumenumber>`. 

4. Sélectionnez le *numéro_de_volume* du volume simple auquel vous souhaitez attribuer le chemin d’accès.

5.  À l’invite **DISKPART**, tapez `assign [mount=<path>]`.

#### <a name="to-remove-a-mount-point-folder-path-to-a-drive"></a>Pour supprimer le chemin d’accès de dossier de point de montage d’un lecteur

-   Pour supprimer le chemin d’accès de dossier de point de montage d’un dossier, cliquez dessus, puis cliquez sur **Supprimer**.

| Valeur | Description |
| --- | --- |
| **list volume** | Affiche une liste des volumes de base et dynamiques sur tous les disques. |
| **select volume**        | Sélectionne le volume spécifié, où <em>numéro_de_volume</em> est le numéro du volume et place le focus sur celui-ci. Si aucun volume n’est spécifié, la commande **select** répertorie le volume actuel avec le focus. Vous pouvez spécifier le volume par numéro, lettre de lecteur ou chemin d’accès de dossier de point de montage. Sur un disque de base, la sélection d’un volume positionne également le focus sur la partition correspondante.|
| **assign** | <ul><li> Attribue une lettre de lecteur ou un chemin d’accès de dossier de point de montage au volume sur lequel se trouve le focus. Si aucune lettre de lecteur ou aucun chemin d’accès de dossier de point de montage n’est spécifié, la lettre de lecteur disponible suivante est attribuée. Si la lettre de lecteur ou le chemin d’accès de dossier de point de montage est déjà en cours d’utilisation, une erreur est générée.</li>  <li>À l’aide de la commande **assign**, vous pouvez modifier la lettre de lecteur associée à un lecteur amovible.</li> <li> Vous ne pouvez pas attribuer de lettres de lecteur aux volumes de démarrage ni aux volumes contenant le fichier de pagination. En outre, il n’est pas possible d’attribuer une lettre de lecteur à une partition OEM, une partition système EFI ou toute autre partition GPT autre qu’une partition de données de base.</li></ul> |
| **mount=** <em>path</em> | Spécifie un dossier NTFS vide existant dans lequel se trouvera le lecteur monté.  |

## <a name="additional-considerations"></a>Considérations supplémentaires

-   Si vous administrez un ordinateur local ou distant, vous pouvez parcourir les dossiers NTFS de cet ordinateur.
-   Les chemins d’accès de dossier de point de montage sont disponibles uniquement pour les dossiers vides de volumes NTFS de base ou dynamiques.
-   Pour modifier un chemin d’accès de dossier de point de montage, supprimez-le, puis créez un nouveau chemin d’accès de dossier avec le nouvel emplacement. Vous ne pouvez pas modifier directement le chemin d’accès de dossier de point de montage.
-   Lorsque vous attribuez un chemin d’accès de dossier de point de montage à un lecteur, utilisez **l’observateur d’événements** pour vérifier la présence éventuelle dans le journal système d’erreurs ou d’avertissements du service de cluster indiquant l’échec de chemins d’accès de dossier de point de montage. Ces erreurs sont répertoriées en tant que **ClusSvc** dans la colonne **Source** et **Ressource disque physique** dans la colonne **Catégorie**.
-   Vous pouvez également créer un lecteur monté à l’aide de la commande [mountvol](https://go.microsoft.com/fwlink/?linkid=64111).

## <a name="see-also"></a>Voir également
-   [Notation de la syntaxe de la ligne de commande](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)


