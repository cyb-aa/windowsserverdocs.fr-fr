---
title: "Attribuez un chemin d’accès de dossier de point de montage à un lecteur."
description: "Cet article décrit comment attribuer un chemin d’accès de dossier de point de montage à un lecteur (plutôt qu’une lettre de lecteur)."
keywords: "virtualisation, sécurité, programmes malveillants"
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 01aaf73f694a6a7a9c516e4358f22dec0d1b4bc4
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="assign-a-mount-point-folder-path-to-a-drive"></a>Attribuer un chemin d’accès de dossier de point de montage à un lecteur

> **S’applique à:** Windows10, Windows8.1, WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012

Vous pouvez utiliser l’outil Gestion des disques pour attribuer un chemin d’accès de dossier de point de montage au lecteur (plutôt qu’une lettre de lecteur). Les chemins d’accès de dossier de point de montage sont disponibles uniquement pour les dossiers vides sur des volumes NTFS de base ou dynamiques.

## <a name="assigning-a-mount-point-folder-path-to-a-drive"></a>Attribution d’un chemin d’accès de dossier de point de montage à un lecteur

-   [Utilisation de l’interface Windows](#BKMK_WINUI)
-   [Utilisation d’une ligne de commande](#BKMK_CMD)

> [!NOTE]
> Vous devez au moins être membre du groupe **Opérateurs de sauvegarde** ou **Administrateurs** pour effectuer ces étapes.

**Pour attribuer un chemin d’accès de dossier de point de montage à un lecteur à l’aide de l’interface Windows:**
<a id="BKMK_WINUI"></a>

1.  Dans le Gestionnaire de disque, cliquez avec le bouton droit sur la partition ou le volume auquel vous souhaitez attribuer le chemin d’accès de dossier de point de montage. 
2. Cliquez sur **Modifier la lettre de lecteur et les chemins d’accès**, puis cliquez sur **Ajouter**. 
3. Cliquez sur **Monter dans le dossier NTFS vide suivant**.
4. Tapez le chemin d’un dossier vide sur le volume NTFS, ou cliquez sur **Parcourir** pour le localiser.

<a id="BKMK_CMD"></a>
#### <a name="to-assign-a-mount-point-folder-path-to-a-drive-using-a-command-line"></a>Pour attribuer un chemin d’accès de dossier de point de montage à un lecteur à l’aide d’une ligne de commande
1.  Ouvrez une invite de commande et tapez `diskpart`.

2.  À l’invite **DISKPART**, tapez `list volume` en veillant à noter le numéro de volume auquel vous voulez attribuer le chemin d’accès.

3.  À l’invite **DISKPART**, tapez `select volume <volumenumber>`. 

4. Sélectionnez le *numéro_de_volume* du volume simple auquel vous souhaitez attribuer le chemin d’accès.

5.  À l’invite **DISKPART**, tapez `assign [mount=<path>]`.

#### <a name="to-remove-a-mount-point-folder-path-to-a-drive"></a>Pour supprimer le chemin d’accès de dossier de point de montage d’un lecteur

-   Pour supprimer le chemin d’accès de dossier de point de montage d’un dossier, cliquez dessus, puis cliquez sur **Supprimer**.

<br />

| Valeur | Description |
| --- | --- |
| <p>**list volume**</p> | <p>Affiche une liste des volumes de base et dynamiques sur tous les disques.</p> |
| <p>**select volume**</p>        | <p>Sélectionne le volume spécifié, où <em>numéro_de_volume</em> est le numéro du volume et positionne le focus sur celui-ci. Si aucun volume n’est spécifié, la commande **select** répertorie le volume actuel avec le focus. Vous pouvez spécifier le volume par numéro, lettre de lecteur ou chemin d’accès de dossier de point de montage. Sur un disque de base, la sélection d’un volume positionne également le focus sur la partition correspondante.</p>|
| <p>**assign**</p> | <p><ul><li> Attribue une lettre de lecteur ou un chemin d’accès de dossier de point de montage au volume sur lequel se trouve le focus. Si aucune lettre de lecteur ou aucun chemin d’accès de dossier de point de montage n’est spécifié, la lettre de lecteur disponible suivante est attribuée. Si la lettre de lecteur ou le chemin d’accès de dossier de point de montage est déjà en cours d’utilisation, une erreur est générée.</li> </p> <p><li>À l’aide de la commande **assign**, vous pouvez modifier la lettre de lecteur associée à un lecteur amovible.</li> </p><p><li> Vous ne pouvez pas attribuer de lettres de lecteur aux volumes de démarrage ni aux volumes contenant le fichier de pagination. En outre, il n’est pas possible d’attribuer une lettre de lecteur à une partition OEM, une partition système EFI ou toute autre partition GPT autre qu’une partition de données de base.</p></li></ul> |
| <p>**mount=** <em>chemin</em></p> | <p>Spécifie un dossier NTFS vide existant dans lequel se trouvera le lecteur monté.</p>  |

## <a name="additional-considerations"></a>Autres éléments à prendre en considération

-   Si vous administrez un ordinateur local ou distant, vous pouvez parcourir les dossiers NTFS de cet ordinateur.
-   Les chemins d’accès de dossier de point de montage sont disponibles uniquement pour les dossiers vides de volumes NTFS de base ou dynamiques.
-   Pour modifier un chemin d’accès de dossier de point de montage, supprimez-le, puis créez un nouveau chemin d’accès de dossier avec le nouvel emplacement. Vous ne pouvez pas modifier directement le chemin d’accès de dossier de point de montage.
-   Lorsque vous attribuez un chemin d’accès de dossier de point de montage à un lecteur, utilisez l’**observateur d’événements** pour vérifier la présence éventuelle dans le journal système d’erreurs ou d’avertissements du service de cluster indiquant l’échec de chemins d’accès de dossier de point de montage. Ces erreurs sont répertoriées en tant que **ClusSvc** dans la colonne **Source** et **Ressource disque physique** dans la colonne **Catégorie**.
-   Vous pouvez également créer un lecteur monté à l’aide de la commande [mountvol](http://go.microsoft.com/fwlink/?linkid=64111).

## <a name="see-also"></a>Articles associés
-   [Notation de la syntaxe de la ligne de commande](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)


