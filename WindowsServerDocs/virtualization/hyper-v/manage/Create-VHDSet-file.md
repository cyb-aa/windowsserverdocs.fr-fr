---
title: Créer des fichiers de disque dur virtuel d’Hyper-V a
description: Étapes pour créer un fichier VHDset sur Hyper-v 2016
author: jiwool
ms.author: jiwool
manager: senthilr
ms.date: 01/26/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: compute-hyper-v
ms.assetid: 444e1496-9e5a-41cf-bfbc-306e2ed8e00a
audience: IT Pros
ms.reviewer: kathydav
ms.openlocfilehash: a5a6f79d362b9058ca29d979457a1dcdfc0c9f82
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445693"
---
# <a name="create-hyper-v-vhd-set-files"></a>Créer des fichiers de disque dur virtuel d’Hyper-V a
Fichiers de disque dur virtuel défini sont un nouveau modèle de disque virtuel partagé pour les clusters invités dans Windows Server 2016. Fichiers de disque dur virtuel défini prennent en charge le redimensionnement en ligne des disques virtuels partagés, prennent en charge la réplication Hyper-V et peuvent être inclus dans les points de contrôle cohérents au niveau applicatif. 

Fichiers de disque dur virtuel défini utilisent un nouveau type de fichier de disque dur virtuel. DISQUES DURS VIRTUELS. Fichiers de disque dur virtuel défini stockent les informations de point de contrôle sur le disque virtuel de groupe utilisé dans les clusters invités, sous la forme de métadonnées.

Hyper-V gère tous les aspects de la gestion des chaînes de point de contrôle et le disque dur virtuel partagé de fusion d’un jeu. Logiciel de gestion peut exécuter des opérations de disque telles que le redimensionnement en ligne sur les fichiers de disque dur virtuel défini dans la même manière que pour. Fichiers VHDX. Cela signifie que les logiciels de gestion n’a pas besoin connaître le format de fichier de disque dur virtuel défini.

## <a name="create-a-vhd-set-file-from-hyper-v-manager"></a>Créer un fichier de disque dur virtuel à partir du Gestionnaire Hyper-V

1.  Ouvrez le Gestionnaire Hyper-V. Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestionnaire Hyper-V**.
2.  Dans le volet Action, cliquez sur **Nouveau**, puis cliquez sur **Disque dur**.
3.  Sur le **choisir le Format de disque** page, sélectionnez **disque dur virtuel défini** en tant que le format du disque dur virtuel.
4.  Suivez les instructions les pages de l’Assistant pour personnaliser le disque dur virtuel. Vous pouvez cliquer sur **Suivant** pour avancer d'une page dans l'Assistant, ou vous pouvez cliquer sur le nom d'une page dans le volet gauche pour passer directement à cette page.
5.  Une fois que vous avez terminé la configuration du disque dur virtuel, cliquez sur **Terminer**.

## <a name="create-a-vhd-set-file-from-windows-powershell"></a>Créer un fichier de disque dur virtuel à partir de Windows PowerShell

Utilisez le [New-VHD](https://technet.microsoft.com/library/hh848503.aspx) applet de commande, avec le type de fichier. Disques durs virtuels dans le chemin d’accès de fichier. Cet exemple crée un fichier de disque dur virtuel défini nommé base.vhds taille est de 10 gigaoctets.

``` PowerShell
PS c:\>New-VHD -Path c:\base.vhds -SizeBytes 10GB
```

## <a name="migrate-a-shared-vhdx-file-to-a-vhd-set-file"></a>Migrer un fichier VHDX partagé vers un fichier de disque dur virtuel

Migration d’un VHDX partagé existant vers un VHD nécessite la mise hors connexion de la machine virtuelle. Voici le processus recommandé à l’aide de Windows PowerShell :

1. Supprimer le VHDX à partir de la machine virtuelle. Par exemple, exécutez : 
   ``` PowerShell
   PS c:\>Remove-VMHardDiskDrive existing.vhdx
   ```
  
2. Convertir le disque VHDX en un VHD. Par exemple, exécutez :
   ``` PowerShell
   PS c:\>Convert-VHD existing.vhdx new.vhds
   ```
  
3. Ajoutez les disques durs virtuels à la machine virtuelle. Par exemple, exécutez :
   ``` PowerShell
   PS c:\>Add-VMHardDiskDrive new.vhds
   ```
  



