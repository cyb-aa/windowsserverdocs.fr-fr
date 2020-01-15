---
title: Étendre un volume de base
description: Vous pouvez ajouter de l’espace à un volume existant dans Windows. Pour cela, vous étendez le volume dans un espace vide sur le disque. Toutefois, cela n’est possible que si l’espace vide ne compte aucun volume (n’est pas alloué) et qu’il se situe immédiatement après le volume à étendre, sans aucun autre volume entre les deux. Cet article explique comment procéder.
ms.date: 12/19/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c72b242437c4c308da77a25e06f3d76e4c65f480
ms.sourcegitcommit: bfe9c5f7141f4f2343a4edf432856f07db1410aa
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75351904"
---
# <a name="extend-a-basic-volume"></a>Étendre un volume de base

> **S’applique à :** Windows 10, Windows 8.1, Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez utiliser Gestion des disques pour ajouter de l’espace à un volume existant. Pour cela, vous étendez le volume dans un espace vide sur le disque. Toutefois, cela n’est possible que si l’espace vide ne compte aucun volume (n’est pas alloué) et qu’il se situe immédiatement après le volume à étendre, sans aucun autre volume entre les deux, comme le montre l’image suivante. Le volume à étendre doit également être formaté avec le système de fichiers NTFS ou ReFS.

:::image type="content" source="media/extend-volume-space-highlighted.png" alt-text="Gestion des disques indiquant l’espace libre dans lequel étendre un volume.":::

## <a name="to-extend-a-volume-by-using-disk-management"></a>Pour étendre un volume à l’aide de Gestion des disques

Voici comment étendre un volume dans un espace vide immédiatement après le volume sur le lecteur :

1. Ouvrez l’outil Gestion des disques avec les autorisations d’administrateur.

   Pour réaliser facilement cette opération, tapez **Gestion de l’ordinateur** dans la zone de recherche de la barre des tâches, sélectionnez **Gestion de l’ordinateur** et maintenez la sélection (ou faites un clic droit), puis sélectionnez **Exécuter en tant qu’administrateur** > **Oui**. Après l’ouverture de Gestion de l’ordinateur, accédez à **Stockage** > **Gestion des disques**.
2. Sélectionnez le volume à étendre et maintenez la sélection (ou faites un clic droit), puis sélectionnez **Étendre le volume**.

   Si **Étendre le volume** est grisé, vérifiez ce qui suit :
    - Vous avez ouvert Gestion des disques ou Gestion de l’ordinateur avec des autorisations administrateur.
    - Un espace non alloué se trouve directement après le volume (à droite de celui-ci), comme indiqué dans le graphique ci-dessus. Si un autre volume se trouve entre l’espace non alloué et le volume à étendre, vous avez le choix entre supprimer tous les fichiers qu’il contient (en veillant à sauvegarder ou à déplacer d’abord tous les fichiers importants), utiliser une application de partitionnement de disque non-Microsoft capable de déplacer des volumes sans détruire les données, et ignorer l’extension du volume et créer à la place un volume distinct dans l’espace non alloué.
    - Le volume est formaté avec le système de fichiers NTFS ou ReFS. Les autres systèmes de fichiers ne peuvent pas être étendus. Vous devez donc déplacer ou sauvegarder les fichiers sur le volume, puis formater le volume avec le système de fichiers NTFS ou ReFS.
    - Si le disque fait plus de 2 To, vérifiez qu’il utilise le schéma de partitionnement GPT. Pour utiliser plus de 2 To sur un disque, vous devez l’initialiser à l’aide du schéma de partitionnement GPT. Pour effectuer la conversion en GPT, consultez [Convertir un disque MBR en disque GPT](change-an-mbr-disk-into-a-gpt-disk.md).
    - Si vous ne parvenez toujours pas à étendre le volume, essayez d’effectuer une recherche dans la [section Fichiers, dossiers et stockage du site Microsoft Community](https://answers.microsoft.com/en-us/windows/forum/windows_10-files?sort=lastreplydate&dir=desc&tab=All&status=all&mod=&modAge=&advFil=&postedAfter=&postedBefore=&threadType=all&isFilterExpanded=true&tm=1514405359639). Si ne trouvez pas de réponse, postez une question sur ce site pour que Microsoft ou d’autres membres de la communauté puissent vous aider ou [contactez le support Microsoft](https://support.microsoft.com/contactus/).

3. Sélectionnez **Suivant**, puis dans la page **Sélectionner des disques** de l’Assistant (illustrée ici), spécifiez de combien étendre le volume. Il est généralement recommandé de choisir la valeur par défaut qui utilise tout l’espace libre disponible, mais vous pouvez opter pour une valeur inférieure si vous souhaitez créer des volumes supplémentaires dans l’espace libre.

   :::image type="content" source="media/extend-volume-wizard.png" alt-text="Assistant Extension du volume montrant un volume étendu occupant tout l’espace disponible":::

4. Sélectionnez **Suivant**, puis **Terminer** pour étendre le volume.

## <a name="to-extend-a-volume-by-using-powershell"></a>Pour étendre un volume à l’aide de PowerShell

1. Sélectionnez le bouton Démarrer et maintenez la sélection (ou faites un clic droit), puis sélectionnez Windows PowerShell (admin).
2. Entrez la commande suivante pour redimensionner le volume à la taille maximale en spécifiant la lettre de lecteur du volume à étendre dans la variable *$drive_letter* :

   ```PowerShell
   # Variable specifying the drive you want to extend
   $drive_letter = "C"

   # Script to get the partition sizes and then resize the volume
   $size = (Get-PartitionSupportedSize -DriveLetter $drive_letter)
   Resize-Partition -DriveLetter $drive_letter -Size $size.SizeMax
   ```

## <a name="see-slso"></a>Voir aussi

- [Resize-Partition](https://docs.microsoft.com/powershell/module/storage/resize-partition)
- [extend (Diskpart)](https://docs.microsoft.com/windows-server/administration/windows-commands/extend)
