---
title: Convertir un disque de table de partition GUID (GPT) en disque d’enregistrement de démarrage principal (MBR)
description: Décrit comment convertir un disque de table de partition GUID (GPT) en disque avec le style de partition d’enregistrement de démarrage principal (MBR).
ms.date: 06/19/2018
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 1ae755b9c41d66ce5f907f600be17547398acc1a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839580"
---
# <a name="convert-a-gpt-disk-into-an-mbr-disk"></a>Convertir un disque GPT en disque MBR

> **S’applique à :** Windows 10, Windows 8.1, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Les disques d’enregistrement de démarrage principal (MBR) utilisent la table de partition BIOS standard. Les disques de table de partition GUID (GPT) utilisent l’interface UEFI (Unified Extensible Firmware Interface). Les disques MBR ne prennent pas en charge plus de quatre partitions par disque. La méthode de partition MBR n’est pas recommandée pour les disques supérieurs à deux téraoctets (To).

Vous pouvez convertir un disque GPT en disque MBR si le disque est vide et ne contient aucun volume.

> [!NOTE]
> Avant de convertir un disque, sauvegardez toutes les données sur ce dernier et fermez tous les programmes qui sont l’accès au disque.

> [!NOTE]
> Vous devez au moins être membre du groupe **Opérateurs de sauvegarde** ou **Administrateurs** pour effectuer ces étapes.

<a id="BKMK_WINUI"></a>

## <a name="converting-using-the-windows-interface"></a>Conversion à l’aide de l’interface Windows

1.  Sauvegardez ou déplacez tous les volumes du disque GPT de base à convertir en disque MBR.

2.  Si le disque contient des partitions ou des volumes, cliquez avec le bouton droit sur chaque élément, puis cliquez sur **Supprimer le volume**.

3.  Cliquez avec le bouton droit sur le disque GPT que vous souhaitez convertir en disque MBR, puis cliquez sur **Convertir en disque MBR**.

<a id="BKMK_CMD"></a>

## <a name="converting-using-a-command-line"></a>Conversion à l’aide d’une ligne de commande

1.  Sauvegardez ou déplacez tous les volumes du disque GPT de base à convertir en disque MBR.

2.  Ouvrez une invite de commandes avec élévation de privilèges en cliquant avec le bouton droit sur **Invite de commande** et en choisissant **Exécuter en tant qu’administrateur**.

3. Entrez `diskpart`. Si le disque ne contient pas de partitions ou de volumes, passez à l’étape 6.

4.  À l’invite **DISKPART**, tapez `list disk`. Notez le numéro du disque à supprimer.

5.  À l’invite **DISKPART**, tapez `select disk <disknumber>`.

6.  À l’invite **DISKPART**, tapez `clean`.

    > [!IMPORTANT]
    > L’exécution de la commande **clean** supprime toutes les partitions ou tous les volumes du disque.

7.  À l’invite **DISKPART**, tapez `convert mbr`.

<br />

| Value | Description |
| --- | --- |
| <p>**disque de la liste**</p> | <p>Affiche une liste des disques ainsi que des informations les concernant, notamment leur taille, la quantité d’espace libre disponible, s’il s’agit d’un disque de base ou d’un disque dynamique et si le disque utilise le style de partition d’enregistrement de démarrage principal (MBR) ou de table de partition GUID (GPT). Le disque marqué d’un astérisque (*) est celui sur lequel se trouve le focus.</p> |
| <p>**Sélectionnez le disque**</p> | <p>Sélectionne le disque spécifié, où <em>numéro_de_disque</em> est le numéro du disque et place le focus sur celui-ci.</p> | <p>**clean**</p> | <p>Supprime toutes les partitions ou tous les volumes du disque sur lequel se trouve le focus.</p> |
| <p>**Convertir le mbr**</p> | <p>Convertit un disque de base vide avec le style de partition de table de partition GUID (GPT) en disque de base avec le style de partition d’enregistrement de démarrage principal (MBR).</p>

## <a name="see-also"></a>Voir aussi

-   [Notation de syntaxe de ligne de commande](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)


