---
title: Convertir un disque dynamique en disque de base
description: Décrit comment convertir un disque dynamique en disque de base.
ms.date: 12/18/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 8ad14225592d627b6ff88b9e2286b686aa549392
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "75351943"
---
# <a name="change-a-dynamic-disk-back-to-a-basic-disk"></a>Convertir un disque dynamique en disque de base

> **S’applique à :** Windows 10, Windows 8.1, Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique décrit comment supprimer le contenu d’un disque dynamique et le reconvertir en disque de base. Les disques dynamiques sont déconseillés pour Windows, et nous ne recommandons plus leur utilisation. Au lieu de cela, nous vous recommandons d’utiliser des disques de bas, ou la technologie des [espaces de stockage](https://support.microsoft.com/help/12438/windows-10-storage-spaces) la plus récente si vous voulez créer un pool de disques pour obtenir des volumes de plus grande capacité. Si vous voulez mettre en miroir le volume de démarrage de Windows, vous pouvez envisager d’utiliser un contrôleur RAID de matériel, tel que celui qui est inclus sur de nombreuses cartes mères.

> [!WARNING]
> Pour reconvertir un disque dynamique en disque de base, vous devez supprimer tous les volumes du disque, ce qui supprimera définitivement toutes les données qu’il contient. Avant de poursuivre, sauvegardez les données que vous souhaitez conserver.

## <a name="to-change-a-dynamic-disk-back-to-a-basic-disk-by-using-disk-management"></a>Pour reconvertir un disque dynamique en disque de base avec Gestion des disques

1.  Sauvegardez tous les volumes du disque dynamique que vous souhaitez reconvertir en disque de base.

2. Ouvrez l’outil Gestion des disques avec les autorisations d’administrateur.

   Pour réaliser facilement cette opération, tapez **Gestion de l’ordinateur** dans la zone de recherche de la barre des tâches, sélectionnez **Gestion de l’ordinateur** et maintenez la sélection (ou faites un clic droit), puis sélectionnez **Exécuter en tant qu’administrateur** > **Oui**. Après l’ouverture de Gestion de l’ordinateur, accédez à **Stockage** > **Gestion des disques**.

2.  Dans Gestion des disques, sélectionnez chaque volume du disque dynamique à convertir en disque de base et maintenez la sélection (ou faites un clic droit), puis cliquez sur **Supprimer le volume**.

3.  Lorsque tous les volumes du disque ont été supprimés, cliquez avec le bouton droit sur le disque, puis cliquez sur **Convertir en disque de base**.

## <a name="to-change-a-dynamic-disk-back-to-a-basic-disk-by-using-a-command-line"></a>Pour reconvertir un disque dynamique en disque de base à l’aide d’une ligne de commande

1.  Sauvegardez tous les volumes du disque dynamique que vous souhaitez reconvertir en disque de base.

2.  Ouvrez une invite de commande et tapez `diskpart`.

3.  À l’invite **DISKPART**, tapez `list disk`. Notez le numéro du disque que vous souhaitez convertir en disque de base.

4.  À l’invite **DISKPART**, tapez `select disk <disknumber>`.

5.  À l’invite **DISKPART**, tapez `detail disk`.

6.  Pour chaque volume du disque, à l’invite **DISKPART**, tapez `select volume= <volumenumber>`, puis tapez `delete volume`.

7.  À l’invite **DISKPART**, tapez `select disk <disknumber>`, en spécifiant le numéro de disque du disque à convertir en disque de base.

8.  À l’invite **DISKPART**, tapez `convert basic`.

| Value  | Description |
| --- | --- |
| **list disk**                         | Affiche une liste des disques ainsi que des informations les concernant, notamment leur taille, la quantité d’espace libre disponible, s’il s’agit d’un disque de base ou d’un disque dynamique et si le disque utilise le style de partition d’enregistrement de démarrage principal (MBR) ou de table de partition GUID (GPT). Le disque marqué d’un astérisque (*) est celui sur lequel se trouve le focus. |
| **select disk** <em>numéro_de_disque</em>   | Sélectionne le disque spécifié, où <em>numéro_de_disque</em> est le numéro du disque et place le focus sur celui-ci.  |
| **detail disk** <em>numéro_de_disque</em>   | Affiche les propriétés du disque sélectionné et des volumes figurant sur le disque.  |
| **select volume** <em>numéro_de_disque</em> | Sélectionne le volume spécifié, où <em>numéro_de_disque</em> est le numéro du volume et place le focus sur celui-ci. Si aucun volume n’est spécifié, la commande **select** répertorie le volume actuel avec le focus. Vous pouvez spécifier le volume par numéro, lettre de lecteur ou chemin d’accès de dossier de point de montage. Sur un disque de base, la sélection d’un volume positionne également le focus sur la partition correspondante. |
| **delete volume**                     | Supprime le volume sélectionné. Vous ne pouvez pas supprimer le volume système, le volume de démarrage ou un volume qui contient le fichier de pagination ou le vidage sur incident (vidage mémoire) actif. |
| **convert basic** | Convertit un disque dynamique vide en disque de base.  |

## <a name="additional-considerations"></a>Considérations supplémentaires

-   Pour que le disque puisse être reconverti en disque de base, il ne doit pas contenir de volumes ou de données. Si vous voulez conserver vos données, sauvegardez-les ou déplacez-les sur un autre volume avant de convertir le disque en disque de base.
-   Une fois que vous avez converti un disque dynamique en disque de base, vous pouvez uniquement créer des partitions et des lecteurs logiques sur ce disque.

## <a name="see-also"></a>Voir aussi

-   [Notation de la syntaxe de la ligne de commande](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)