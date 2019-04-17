---
title: "Convertir un disque de table de partition GUID (GPT) en disque d’enregistrement de démarrage principal (MBR)"
description: "Décrit comment convertir un disque de table de partition GUID (GPT) en disque avec le style de partition d’enregistrement de démarrage principal (MBR)."
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 8efe6617c46267f7e165550de82b18421ab563ca
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="change-a-guid-partition-table-gpt-disk-into-a-master-boot-record-mbr-disk"></a>Convertir un disque de table de partition GUID (GPT) en disque d’enregistrement de démarrage principal (MBR)

> **S’applique à:** Windows10, Windows8.1, WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012

Les disques d’enregistrement de démarrage principal (MBR) utilisent la table de partition BIOS standard. Les disques de table de partition GUID (GPT) utilisent l’interface UEFI (Unified Extensible Firmware Interface). Les disques MBR ne prennent pas en charge plus de quatre partitions par disque. La méthode de partition MBR n’est pas recommandée pour les disques supérieurs à deux téraoctets (To).

Vous pouvez convertir un disque GPT en disque MBR si le disque est vide et ne contient aucun volume.

> [!NOTE]
> Avant de convertir des disques, fermez tous les programmes en cours d’exécution sur ces disques.

<a name="changing-a-gpt-disk-into-an-mbr-disk"></a>Conversion d’un disque GPT en disque MBR
-------------------------------------------------------

-   [Utilisation de l’interface Windows](#BKMK_WINUI)
-   [Utilisation d’une ligne de commande](#BKMK_CMD)

> [!NOTE]
> Vous devez au moins être membre du groupe **Opérateurs de sauvegarde** ou **Administrateurs** pour effectuer ces étapes.

 <a id="BKMK_WINUI"></a>
#### <a name="to-change-a-gpt-disk-into-an-mbr-disk-using-the-windows-interface"></a>Pour convertir un disque GPT en disque MBR à l’aide de l’interface Windows:

1.  Sauvegardez ou déplacez tous les volumes du disque GPT de base à convertir en disque MBR.

2.  Si le disque contient des partitions ou des volumes, cliquez avec le bouton droit sur chaque élément, puis cliquez sur **Supprimer le volume**.

3.  Cliquez avec le bouton droit sur le disque GPT que vous souhaitez convertir en disque MBR, puis cliquez sur **Convertir en disque MBR**.

 <a id="BKMK_CMD"></a>
#### <a name="to-change-a-gpt-disk-into-an-mbr-disk-using-a-command-line"></a>Pour convertir un disque GPT en disque MBR à l’aide d’une ligne de commande

1.  Sauvegardez ou déplacez tous les volumes du disque GPT de base à convertir en disque MBR.

2.  Ouvrez une invite de commandes avec élévation de privilèges en cliquant avec le bouton droit sur **Invite de commande** et en choisissant **Exécuter en tant qu’administrateur**.

3. Tapez `diskpart`. Si le disque ne contient pas de partitions ou de volumes, passez à l’étape6.

4.  À l’invite **DISKPART**, tapez `list disk`. Notez le numéro du disque à supprimer.

5.  À l’invite **DISKPART**, tapez `select disk <disknumber>`.

6.  À l’invite **DISKPART**, tapez `clean`.

> [!IMPORTANT]
> L’exécution de la commande **clean** supprime toutes les partitions ou tous les volumes sur le disque.

7.  À l’invite **DISKPART**, tapez `convert mbr`.

<br />

| Valeur | Description |
| --- | --- |
| <p>**list disk**</p> | <p>Affiche une liste des disques ainsi que des informations les concernant, notamment leur taille, la quantité d’espace libre disponible, s’il s’agit d’un disque de base ou d’un disque dynamique et si le disque utilise le style de partition d’enregistrement de démarrage principal (MBR) ou de table de partition GUID (GPT). Le disque marqué d’un astérisque (*) est celui sur lequel se trouve le focus.</p> |
| <p>**select disk**</p> | <p>Sélectionne le disque spécifié, où <em>numéro_de_disque</em> est le numéro du disque et place le focus sur celui-ci.</p> | <p>**clean**</p> | <p>Supprime toutes les partitions ou tous les volumes du disque sur lequel se trouve le focus.</p> |
| <p>**convert mbr**</p> | <p>Convertit un disque de base vide avec le style de partition de table de partition GUID (GPT) en disque de base avec le style de partition d’enregistrement de démarrage principal (MBR).</p>

## <a name="see-also"></a>Articles associés

-   [Notation de la syntaxe de la ligne de commande](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)


