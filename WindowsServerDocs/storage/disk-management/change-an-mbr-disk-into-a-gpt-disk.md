---
title: "Convertir un enregistrement de démarrage principal (MBR) en disque de table de partition GUID (GPT)"
description: "Décrit comment convertir un enregistrement de démarrage principal (MBR) en disque de table de partition GUID (GPT)"
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: f28d151d3b1d6b19b9ca330c5ff8576a53acbfa5
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="change-a-master-boot-record-mbr-disk-into-a-guid-partition-table-gpt-disk"></a>Convertir un disque d’enregistrement de démarrage principal (MBR) en disque de table de partition GUID (GPT)

> **S’applique à:** Windows10, Windows8.1, WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012

Les disques d’enregistrement de démarrage principal (MBR) utilisent la table de partition BIOS standard. Les disques de table de partition GUID (GPT) utilisent l’interface UEFI (Unified Extensible Firmware Interface). L’un des avantages des disques GPT réside dans le fait que vous pouvez avoir plus de quatre partitions sur chaque disque. Le format GPT est également nécessaire pour les disques supérieurs à deux téraoctets (To).

Vous pouvez convertir un disque MBR en disque GPT si le disque est vide et ne contient pas de partitions ou de volumes.
Vous ne pouvez pas utiliser le style de partition GPT sur un support amovible, ou sur des disques de cluster connectés à des bus SCSI ou Fibre Channel partagés utilisés par le service de cluster.

> [!NOTE]
> Avant de convertir des disques, fermez tous les programmes en cours d’exécution sur ces disques.

## <a name="changing-an-mbr-disk-into-a-gpt-disk"></a>Conversion d’un disque MBR en disque GPT

-   [Utilisation de l’interface Windows](#BKMK_WINUI)
-   [Utilisation d’une ligne de commande](#BKMK_CMD)

> [!NOTE]
> Vous devez au moins être membre du groupe **Opérateurs de sauvegarde** ou **Administrateurs** pour effectuer ces étapes.

<a id="BKMK_WINUI"></a>
#### <a name="to-change-an-mbr-disk-into-a-gpt-disk-using-the-windows-interface"></a>Pour convertir un disque MBR en disque GPT à l’aide de l’interface Windows

1.  Sauvegardez ou déplacez les données du disque MBR de base à convertir en disque GPT.

2.  Si le disque contient des partitions ou des volumes, cliquez avec le bouton droit sur chaque élément, puis cliquez sur **Supprimer la partition** ou **Supprimer le volume**.

3.  Cliquez avec le bouton droit sur le disque MBR à convertir en disque GPT, puis cliquez sur **Convertir en disque GPT**.

<a id="BKMK_CMD"></a>
#### <a name="to-change-an-mbr-disk-into-a-gpt-disk-using-a-command-line"></a>Pour convertir un disque MBR en disque GPT à l’aide d’une ligne de commande

1.  Sauvegardez ou déplacez les données du disque MBR de base à convertir en disque GPT.

2.  Ouvrez une invite de commandes avec élévation de privilèges en cliquant avec le bouton droit sur **Invite de commande** et en choisissant **Exécuter en tant qu’administrateur**.

3. Tapez `diskpart`. Si le disque ne contient pas de partitions ou de volumes, passez à l’étape6.

4.  À l’invite **DISKPART**, tapez `list disk`. Notez le numéro du disque que vous souhaitez convertir.

5.  À l’invite **DISKPART**, tapez `select disk <disknumber>`.

6.  À l’invite **DISKPART**, tapez `clean`.

> [!NOTE]
> L’exécution de la commande **clean** supprime toutes les partitions ou tous les volumes du disque.

7.  À l’invite **DISKPART**, tapez `convert gpt`.

<br />

| Valeur  | Description  |
| ----- | ----|
| <p>**list disk**</p> | <p>Affiche une liste des disques ainsi que des informations les concernant, notamment leur taille, la quantité d’espace libre disponible, s’il s’agit d’un disque de base ou d’un disque dynamique et si le disque utilise le style de partition d’enregistrement de démarrage principal (MBR) ou de table de partition GUID (GPT). Le disque marqué d’un astérisque (*) est celui sur lequel se trouve le focus.</p> |
| <p>**select disk** <em>numéro_de_disque</em></p> | <p>Sélectionne le disque spécifié, où <em>numéro_de_disque</em> est le numéro du disque et place le focus sur celui-ci.</p> |
| <p>**clean**</p> | <p>Supprime toutes les partitions ou tous les volumes du disque sur lequel se trouve le focus.</p>  |
| <p>**convert gpt**</p>| <p>Convertit un disque de base vide avec le style de partition d’enregistrement de démarrage principal (MBR) en disque de base avec le style de partition de table de partition GUID (GPT).</p> |

## <a name="see-also"></a>Articles associés

-   [Notation de la syntaxe de la ligne de commande](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)


