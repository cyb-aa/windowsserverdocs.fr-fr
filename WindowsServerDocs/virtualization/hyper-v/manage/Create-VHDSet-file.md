---
title: Créer des fichiers de jeu de disques durs virtuels Hyper-V
description: Étapes de création d’un fichier baVHDset sur Hyper-v 2016
author: jiwool
ms.author: jiwool
manager: senthilr
ms.date: 01/26/2017
ms.topic: article
ms.prod: windows-server
ms.technology: compute-hyper-v
ms.assetid: 444e1496-9e5a-41cf-bfbc-306e2ed8e00a
audience: IT Pros
ms.reviewer: kathydav
ms.openlocfilehash: f5c9b932cabfea8df55ba8622165bbb04b4a4113
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392719"
---
# <a name="create-hyper-v-vhd-set-files"></a>Créer des fichiers de jeu de disques durs virtuels Hyper-V
Les fichiers de définition de VHD sont un nouveau modèle de disque virtuel partagé pour les clusters invités dans Windows Server 2016. Les fichiers de définition de VHD prennent en charge le redimensionnement en ligne des disques virtuels partagés, prennent en charge la réplication Hyper-V et peuvent être inclus dans des points de contrôle cohérents avec les applications. 

Les fichiers de définition de disque dur virtuel utilisent un nouveau type de fichier VHD,. Durs. Les fichiers de définition de VHD stockent des informations de point de contrôle sur le disque virtuel de groupe utilisé dans les clusters invités, sous la forme de métadonnées.

Hyper-V gère tous les aspects de la gestion des chaînes de points de contrôle et de la fusion de l’ensemble de disques durs virtuels partagés. Les logiciels de gestion peuvent exécuter des opérations de disque telles que le redimensionnement en ligne sur des fichiers de jeu de disques durs virtuels de la même façon que pour. Fichiers VHDX. Cela signifie que le logiciel de gestion n’a pas besoin de connaître le format de fichier de l’ensemble de disques durs virtuels.

## <a name="create-a-vhd-set-file-from-hyper-v-manager"></a>Créer un fichier de définition de disque dur virtuel à partir du Gestionnaire Hyper-V

1.  Ouvrez le Gestionnaire Hyper-V. Cliquez sur **Démarrer**, pointez sur **Outils d'administration**, puis cliquez sur **Gestionnaire Hyper-V**.
2.  Dans le volet Action, cliquez sur **Nouveau**, puis cliquez sur **Disque dur**.
3.  Sur la page **choisir le format de disque** , sélectionnez **VHD défini** comme format du disque dur virtuel.
4.  Suivez les pages de l’Assistant pour personnaliser le disque dur virtuel. Vous pouvez cliquer sur **Suivant** pour avancer d'une page dans l'Assistant, ou vous pouvez cliquer sur le nom d'une page dans le volet gauche pour passer directement à cette page.
5.  Une fois que vous avez terminé la configuration du disque dur virtuel, cliquez sur **Terminer**.

## <a name="create-a-vhd-set-file-from-windows-powershell"></a>Créer un fichier de définition de VHD à partir de Windows PowerShell

Utilisez l’applet de commande [New-VHD](https://technet.microsoft.com/library/hh848503.aspx) avec le type de fichier. VHD dans le chemin d’accès du fichier. Cet exemple crée un fichier de définition de VHD nommé base. vhd dont la taille est de 10 Go.

``` PowerShell
PS c:\>New-VHD -Path c:\base.vhds -SizeBytes 10GB
```

## <a name="migrate-a-shared-vhdx-file-to-a-vhd-set-file"></a>Migrer un fichier VHDX partagé vers un fichier de définition de disque dur virtuel

La migration d’un VHDX partagé existant vers un VHD requiert la mise hors connexion de la machine virtuelle. Il s’agit du processus recommandé à l’aide de Windows PowerShell :

1. Supprimez le VHDX de la machine virtuelle. Par exemple, exécutez : 
   ``` PowerShell
   PS c:\>Remove-VMHardDiskDrive existing.vhdx
   ```
  
2. Convertissez le VHDX en VHD. Par exemple, exécutez :
   ``` PowerShell
   PS c:\>Convert-VHD existing.vhdx new.vhds
   ```
  
3. Ajoutez les disques durs virtuels à la machine virtuelle. Par exemple, exécutez :
   ``` PowerShell
   PS c:\>Add-VMHardDiskDrive new.vhds
   ```
  



