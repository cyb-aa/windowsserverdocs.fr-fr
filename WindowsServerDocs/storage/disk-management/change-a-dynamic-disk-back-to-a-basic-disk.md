---
title: Convertir un disque dynamique en disque de base
description: Décrit comment convertir un disque dynamique en disque de base.
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: d018b417aeba21e2b1303fd9e72b58b4e2fc334f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836050"
---
# <a name="change-a-dynamic-disk-back-to-a-basic-disk"></a>Convertir un disque dynamique en disque de base

> **S’applique à :** Windows 10, Windows 8.1, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique décrit comment supprimer le contenu d’un disque dynamique et le reconvertir en disque de base. Disques dynamiques ont été déconseillés à partir de Windows, et nous ne recommandons pas leur utilisation plus. Au lieu de cela, nous recommandons à l’aide de disques de base ou à l’aide la plus récente [espaces de stockage](https://support.microsoft.com/help/12438/windows-10-storage-spaces) technologie lorsque vous souhaitez les disques de pool ensemble dans des volumes plus importants. Si vous voulez mettre en miroir le volume de démarrage de Windows, vous pouvez envisager d’utiliser un contrôleur RAID de matériel, tel que celui qui est inclus sur de nombreuses cartes mères.

> [!WARNING]
> Pour reconvertir un disque dynamique en disque de base, vous devez supprimer tous les volumes du disque, ce qui supprimera définitivement toutes les données qu’il contient. Avant de poursuivre, sauvegardez les données que vous souhaitez conserver.

## <a name="changing-a-dynamic-disk-back-to-a-basic-disk"></a>Reconversion d’un disque dynamique en disque de base

-   [À l’aide de l’interface Windows](#BKMK_WINUI)
-   [À l’aide d’une ligne de commande](#BKMK_CMD)

> [!NOTE]
> Vous devez au moins être membre du groupe **Opérateurs de sauvegarde** ou **Administrateurs** pour effectuer ces étapes.

<a href="" id="BKMK_WINUI"></a>
#### <a name="to-change-a-dynamic-disk-back-to-a-basic-disk-using-the-windows-interface"></a>Pour reconvertir un disque dynamique en disque de base à l’aide de l’interface Windows
1.  Sauvegardez tous les volumes du disque dynamique que vous souhaitez reconvertir en disque de base.

2.  Dans Gestion des disques, cliquez avec le bouton droit sur chaque volume du disque dynamique à convertir en disque de base, puis cliquez sur **Supprimer le volume** pour chaque volume du disque.

3.  Lorsque tous les volumes du disque ont été supprimés, cliquez avec le bouton droit sur le disque, puis cliquez sur **Convertir en disque de base**.


<a href="" id="BKMK_CMD"></a>
#### <a name="to-change-a-dynamic-disk-back-to-a-basic-disk-using-a-command-line"></a>Pour reconvertir un disque dynamique en disque de base à l’aide d’une ligne de commande

1.  Sauvegardez tous les volumes du disque dynamique que vous souhaitez reconvertir en disque de base.

2.  Ouvrez une invite de commande et tapez `diskpart`.

3.  À l’invite **DISKPART**, tapez `list disk`. Notez le numéro du disque que vous souhaitez convertir en disque de base.

4.  À l’invite **DISKPART**, tapez `select disk <disknumber>`.

5.  À l’invite **DISKPART**, tapez `detail disk <disknumber>`.

6.  Pour chaque volume du disque, à l’invite **DISKPART**, tapez `select volume= <volumenumber>`, puis tapez `delete volume`.

7.  À l’invite **DISKPART**, tapez `select disk <disknumber>`, en spécifiant le numéro de disque du disque à convertir en disque de base.

8.  À l’invite **DISKPART**, tapez `convert basic`.
 
<br /> <br />

| Value  | Description |
| --- |---|
| <p>**disque de la liste**</p>                         | <p>Affiche une liste des disques ainsi que des informations les concernant, notamment leur taille, la quantité d’espace libre disponible, s’il s’agit d’un disque de base ou d’un disque dynamique et si le disque utilise le style de partition d’enregistrement de démarrage principal (MBR) ou de table de partition GUID (GPT). Le disque marqué d’un astérisque (*) est celui sur lequel se trouve le focus.</p> |
| <p>**Sélectionnez le disque** <em>numéro_disque</em></p>   | <p>Sélectionne le disque spécifié, où <em>numéro_de_disque</em> est le numéro du disque et place le focus sur celui-ci.</p>  |
| <p>**disque de détail** <em>numéro_disque</em></p>   | <p>Affiche les propriétés du disque sélectionné et des volumes figurant sur le disque.</p>  |
| <p>**Sélectionnez le volume** <em>numéro_disque</em></p> | <p>Sélectionne le volume spécifié, où <em>numéro_de_disque</em> est le numéro du volume et place le focus sur celui-ci. Si aucun volume n’est spécifié, la commande **select** répertorie le volume actuel avec le focus. Vous pouvez spécifier le volume par numéro, lettre de lecteur ou chemin d’accès de dossier de point de montage. Sur un disque de base, la sélection d’un volume positionne également le focus sur la partition correspondante.</p> |
| <p>**delete volume**</p>                     | <p>Supprime le volume sélectionné. Vous ne pouvez pas supprimer le volume système, le volume de démarrage ou un volume qui contient le fichier de pagination ou le vidage sur incident (vidage mémoire) actif.</p> |
| <p>**convertir la base**</p> | <p>Convertit un disque dynamique vide en disque de base.</p>  |

## <a name="additional-considerations"></a>Considérations supplémentaires

-   Pour que le disque puisse être reconverti en disque de base, il ne doit pas contenir de volumes ou de données. Si vous voulez conserver vos données, sauvegardez-les ou déplacez-les sur un autre volume avant de convertir le disque en disque de base.
-   Une fois que vous avez converti un disque dynamique en disque de base, vous pouvez uniquement créer des partitions et des lecteurs logiques sur ce disque.

## <a name="see-also"></a>Voir aussi

-   [Notation de syntaxe de ligne de commande](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)


