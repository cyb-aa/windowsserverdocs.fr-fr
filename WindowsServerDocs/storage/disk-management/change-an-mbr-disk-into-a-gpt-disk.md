---
title: Convertir un enregistrement de démarrage principal (MBR) en disque de table de partition GUID (GPT)
description: Décrit comment convertir un enregistrement de démarrage principal (MBR) en disque de table de partition GUID (GPT)
ms.date: 06/07/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 6bd97802fbef342520e92a857a1a53acf3e8d7a3
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "71385938"
---
# <a name="convert-an-mbr-disk-into-a-gpt-disk"></a>Convertir un disque MBR en disque GPT

> **S’applique à :** Windows 10, Windows 8.1, Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Les disques d’enregistrement de démarrage principal (MBR) utilisent la table de partition BIOS standard. Les disques de table de partition GUID (GPT) utilisent l’interface UEFI (Unified Extensible Firmware Interface). L’un des avantages des disques GPT réside dans le fait que vous pouvez avoir plus de quatre partitions sur chaque disque. Le format GPT est également nécessaire pour les disques supérieurs à deux téraoctets (To).

Vous pouvez convertir un disque MBR en disque GPT si le disque est vide et ne contient pas de partitions ou de volumes.

> [!NOTE]
> Avant de convertir un disque, sauvegardez toutes les données qu’il contient et fermez tous les programmes qui y accèdent.

> [!NOTE]
> Vous devez au moins être membre du groupe **Opérateurs de sauvegarde** ou **Administrateurs** pour effectuer ces étapes.

## <a name="converting-using-the-windows-interface"></a>Conversion à l’aide de l’interface Windows

1.  Sauvegardez ou déplacez les données du disque MBR de base à convertir en disque GPT.

2.  Si le disque contient des partitions ou des volumes, cliquez avec le bouton droit sur chaque élément, puis cliquez sur **Supprimer la partition** ou **Supprimer le volume**.

3.  Cliquez avec le bouton droit sur le disque MBR à convertir en disque GPT, puis cliquez sur **Convertir en disque GPT**.

## <a name="converting-using-a-command-line"></a>Conversion à l’aide d’une ligne de commande

Utilisez les étapes suivantes pour convertir un disque MBR vide en disque GPT. Il existe également un outil MBR2GPT.EXE que vous pouvez utiliser, mais il est un peu compliqué. Consultez l’article [Convertir une partition MBR au format GPT](https://docs.microsoft.com/windows/deployment/mbr-to-gpt) pour plus d’informations.

1.  Sauvegardez ou déplacez les données du disque MBR de base à convertir en disque GPT.

2.  Ouvrez une invite de commandes avec élévation de privilèges en cliquant avec le bouton droit sur **Invite de commande** et en choisissant **Exécuter en tant qu’administrateur**.

3. Entrez `diskpart`. Si le disque ne contient pas de partitions ou de volumes, passez à l’étape 6.

4.  À l’invite **DISKPART**, tapez `list disk`. Notez le numéro du disque que vous souhaitez convertir.

5.  À l’invite **DISKPART**, tapez `select disk <disknumber>`.

6.  À l’invite **DISKPART**, tapez `clean`.

    > [!NOTE]
    > L’exécution de la commande **clean** supprime toutes les partitions ou tous les volumes du disque.

7.  À l’invite **DISKPART**, tapez `convert gpt`.

| Value  | Description  |
| ----- | ---- |
| **list disk** | Affiche une liste des disques ainsi que des informations les concernant, notamment leur taille, la quantité d’espace libre disponible, s’il s’agit d’un disque de base ou d’un disque dynamique et si le disque utilise le style de partition d’enregistrement de démarrage principal (MBR) ou de table de partition GUID (GPT). Le disque marqué d’un astérisque (*) est celui sur lequel se trouve le focus. |
| **select disk** *numéro_de_disque* | Sélectionne le disque spécifié, où *numéro_de_disque* est le numéro du disque et place le focus sur celui-ci. |
| **clean** | Supprime toutes les partitions ou tous les volumes du disque sur lequel se trouve le focus.  |
| **convert gpt**| Convertit un disque de base vide avec le style de partition d’enregistrement de démarrage principal (MBR) en disque de base avec le style de partition de table de partition GUID (GPT). |

## <a name="see-also"></a>Voir aussi

-   [Notation de la syntaxe de la ligne de commande](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)