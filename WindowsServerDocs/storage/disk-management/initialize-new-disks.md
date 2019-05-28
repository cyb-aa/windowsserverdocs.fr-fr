---
title: Initialiser les nouveaux disques
description: Guide pratique pour initialiser de nouveaux disques avec gestion des disques, les soyez prêt à utiliser. Inclut également des liens vers la résolution des problèmes.
ms.date: 10/24/2018
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e009780d83220b528ba7dac6e2561be36e662f71
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192762"
---
# <a name="initialize-new-disks"></a>Initialiser les nouveaux disques

> **S’applique à :** Windows 10, Windows 8.1, Windows 7, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si vous ajoutez un nouveau disque à votre PC, et il n’apparaît pas dans l’Explorateur de fichiers, vous devrez peut-être [ajouter une lettre de lecteur](change-a-drive-letter.md), ou initialiser celui-ci avant de l’utiliser. Vous pouvez initialiser uniquement un lecteur qui n’est pas encore mis en forme. Initialisation d’un disque efface toutes les données et le prépare pour une utilisation par Windows, après quoi vous pouvez mettre en forme et stocker des fichiers.

> [!WARNING]
> Si votre disque a déjà des fichiers qui vous intéressent, ne l’initialiser, vous allez perdre tous les fichiers. Au lieu de cela, nous recommandons le disque de résolution des problèmes pour voir si vous pouvez lire les fichiers - consultez [l’état d’un disque n’est pas initialisé ou le disque est manquant entièrement](troubleshooting-disk-management.md#a-disks-status-is-not-initialized-or-the-disk-is-missing).

## <a name="to-initialize-new-disks"></a>Pour initialiser les nouveaux disques

Voici comment initialiser un nouveau disque à l’aide de la gestion des disques. Si vous préférez utiliser PowerShell, utilisez le [initialize-disk](https://docs.microsoft.com/powershell/module/storage/initialize-disk) applet de commande à la place.

1. Ouvrez Gestion des disques avec les autorisations d’administrateur. 
 
    Pour ce faire, dans la zone de recherche dans la barre des tâches, tapez **gestion des disques**, sélectionnez et maintenez (ou avec le bouton droit) **gestion des disques**, puis sélectionnez **exécuter en tant qu’administrateur**  >  **Oui**. Si vous ne pouvez pas l’ouvrir en tant qu’administrateur, tapez **gestion de l’ordinateur** au lieu de cela, puis accédez à **stockage** > **gestion des disques**.
1. Dans Gestion des disques, cliquez sur le disque que vous souhaitez initialiser, puis cliquez sur **initialiser le disque** (illustré ici). Si le disque est répertorié en tant que *hors connexion*, tout d’abord faites un clic droit et sélectionnez **Online**.

     Notez que certains lecteurs USB n’ont pas l’option à initialiser, ils simplement obtient mis en forme et un [lettre de lecteur](change-a-drive-letter.md).

    ![Gestion des disques montrant un disque non formaté avec le menu de raccourci d’initialiser le disque s’affiché](media\uninitialized-disk.PNG)
2. Dans le **initialiser le disque** boîte de dialogue (illustré ici), de vérification pour vous assurer que le disque correct est sélectionné, puis cliquez sur **OK** pour accepter le style de partition par défaut. Si vous avez besoin de modifier le style des partition (MBR ou GPT) consultez [sur les styles de partition - GPT et MBR](#about-partition-styles---gpt-and-mbr).

     L’état du disque passe brièvement à **initialisation** puis au **Online** état. Si l’initialisation échoue pour une raison quelconque, consultez [l’état d’un disque n’est pas initialisé ou le disque est manquant entièrement](troubleshooting-disk-management.md#a-disks-status-is-not-initialized-or-the-disk-is-missing).

    ![La boîte de dialogue initialiser le disque avec le style de partition GPT sélectionné](media\initialize-disk.PNG)

## <a name="about-partition-styles---gpt-and-mbr"></a>Sur les styles de partition - GPT et MBR

Disques peuvent être divisés en plusieurs segments, appelées partitions. Chaque partition - même si vous avez uniquement un - doit avoir un style de partition - MBR ou GPT. Windows utilise le style de partition pour comprendre comment accéder aux données sur le disque.

Aussi fascinant que ce n’est probablement pas, le point important est que ces jours, vous n’avez généralement à vous soucier de style de partition, Windows utilise automatiquement le type de disque approprié.

La plupart des PC utilisent le type de disque de Table de Partition GUID (GPT) pour les disques durs et des disques SSD. GPT est plus robuste, tout en permettant de volumes supérieure à 2 To. Le type de disque de démarrage principal (MBR) plus ancien est utilisé par les ordinateurs 32 bits, PC les plus anciens et les lecteurs amovibles tels que les cartes de mémoire.

Pour convertir un disque MBR en GPT ou vice versa, vous devez d’abord supprimer tous les volumes à partir du disque, effacer toutes les opérations sur le disque. Pour plus d’informations, consultez [convertir un disque MBR en disque GPT](change-an-mbr-disk-into-a-gpt-disk.md), ou [convertir un disque GPT en disque MBR](change-a-gpt-disk-into-an-mbr-disk.md).