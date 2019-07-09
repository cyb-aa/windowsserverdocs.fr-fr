---
title: Initialiser de nouveaux disques
description: Comment initialiser de nouveaux disques avec l’outil Gestion des disques, pour que ces derniers soient prêts à être utilisés. Cet article inclut également des liens vers la résolution des problèmes.
ms.date: 06/07/2019
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 7a275c372e1486b26821f797a7663eecbc3e8784
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66812427"
---
# <a name="initialize-new-disks"></a>Initialiser de nouveaux disques

> **S’applique à :** Windows 10, Windows 8.1, Windows 7, Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si vous ajoutez un tout nouveau disque à votre PC, et s’il n’apparaît pas dans l’Explorateur de fichiers, vous devrez peut-être [ajouter une lettre de lecteur](change-a-drive-letter.md), ou l’initialiser avant de l’utiliser. Vous pouvez uniquement initialiser un lecteur qui n’est pas encore formaté. L’initialisation d’un disque efface toutes les données présentes sur ce dernier et le prépare pour une utilisation par Windows, pour que vous puissiez le formater et stocker des fichiers dessus.

> [!WARNING]
> Si votre disque contient déjà des fichiers importants, ne l’initialisez pas ; vous allez perdre tous les fichiers. Nous vous recommandons plutôt de résoudre les problèmes du disque pour voir si vous pouvez lire les fichiers ; consultez [L’état d’un disque est Non initialisé ou le disque est manquant](troubleshooting-disk-management.md#a-disks-status-is-not-initialized-or-the-disk-is-missing).

## <a name="to-initialize-new-disks"></a>Pour initialiser de nouveaux disques

Voici comment initialiser un nouveau disque à l’aide de l’outil Gestion des disques. Si vous préférez utiliser PowerShell, utilisez plutôt la cmdlet [initialize-disk](https://docs.microsoft.com/powershell/module/storage/initialize-disk).

1. Ouvrez l’outil Gestion des disques avec les autorisations d’administrateur. 
 
    Pour ce faire, dans la zone de recherche de la barre des tâches, tapez **Gestion des disques**, sélectionnez **Gestion des disques** et maintenez la sélection (ou faites un clic droit), puis sélectionnez **Exécuter en tant qu’administrateur** > **Oui**. Si vous ne pouvez pas ouvrir la fonctionnalité en tant qu’administrateur, tapez **Gestion de l’ordinateur**, puis accédez à **Stockage** > **Gestion des disques**.
1. Dans l’outil Gestion des disques, cliquez avec le bouton droit sur le disque à initialiser, puis cliquez sur **Initialiser le disque** (affiché ici). Si le disque est répertorié comme étant *Hors connexion*, commencez par cliquer avec le bouton droit dessus et sélectionnez **En ligne**.

     Notez que certains lecteurs USB ne disposent pas de l’option d’initialisation, ils sont simplement formatés et obtiennent une [lettre de lecteur](change-a-drive-letter.md).

    ![Outil Gestion des disques montrant un disque non formaté avec le menu contextuel Initialiser le disque affiché](media/uninitialized-disk.PNG)
2. Dans la boîte de dialogue **Initialiser le disque** (affichée ici), vérifiez que le disque approprié est sélectionné, puis cliquez sur **OK** pour accepter le style de partition par défaut. Si vous avez besoin de modifier le style des partitions (MBR ou GPT), consultez [À propos des styles de partition - GPT et MBR](#about-partition-styles---gpt-and-mbr).

     L’état du disque passe brièvement à **Initialisation**, puis à **En ligne**. Si l’initialisation échoue pour une raison quelconque, consultez [L’état d’un disque est Non initialisé ou le disque est manquant](troubleshooting-disk-management.md#a-disks-status-is-not-initialized-or-the-disk-is-missing).

    ![La boîte de dialogue Initialiser le disque avec le style de partition GPT sélectionné](media/initialize-disk.PNG)

## <a name="about-partition-styles---gpt-and-mbr"></a>À propos des styles de partition - GPT et MBR

Les disques peuvent être divisés en plusieurs segments, appelés partitions. Chaque partition (même si vous n’en avez qu’une seule) doit avoir un style de partition : MBR ou GPT. Windows utilise le style de partition pour comprendre comment accéder aux données sur le disque.

Comme vous le savez probablement, de nos jours, vous n’avez pas à vous soucier du style de partition, puisque Windows utilise automatiquement le type de disque approprié.

La plupart des PC utilisent le type de disque de table de partition GUID (GPT) pour les disques durs et disques SSD. Le style GPT est plus robuste et tout à fait adapté aux volumes supérieurs à 2 To. Le type de disque d’enregistrement de démarrage principal (MBR), plus ancien, est utilisé par les PC 32 bits, les PC moins récents et les lecteurs amovibles tels que les cartes mémoire.

Pour convertir un disque MBR en GPT ou vice versa, vous devez d’abord supprimer tous les volumes du disque, ce qui permet d’effacer tout ce qui se trouve sur le disque. Pour plus d’informations, consultez [Convert an MBR disk into a GPT disk](change-an-mbr-disk-into-a-gpt-disk.md) (Convertir un disque MBR en disque GPT) ou [Convert a GPT disk into an MBR disk](change-a-gpt-disk-into-an-mbr-disk.md) (Convertir un disque GPT en disque MBR).