---
title: "Gérer les disques durs virtuels (VHD)"
description: "Cet article explique comment gérer les disques durs virtuels"
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 2e371710752d59ebc7a1f8aa2dad3d9189872c47
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="manage-virtual-hard-disks-vhd"></a>Gérer les disques durs virtuels (VHD)

> **S’applique à:** Windows10, Windows8.1, WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012

Cette rubrique décrit comment créer, attacher et détacher des disques durs virtuels avec l’outil Gestion des disques. Les disques durs virtuels (VHD) sont des fichiers de disques durs virtuels qui, une fois montés, apparaissent et fonctionnent quasiment de la même façon qu’un disque dur physique. Ils sont généralement utilisés avec les ordinateurs virtuels Hyper-V. 

## <a name="viewing-vhds-in-disk-management"></a>Affichage des disques durs virtuels dans Gestion des disques

Les disques durs virtuels apparaissent de la même façon que les disques physiques dans l’outil Gestion des disques. Lorsqu’un disque dur virtuel a été attaché (c’est-à-dire, mis à la disposition du système pour utilisation), il s’affiche en bleu. Si le disque est détaché (autrement dit, s’il n’est pas disponible), son icône est grisée.

## <a name="creating-a-vhd"></a>Création d’un disque dur virtuel

> [!NOTE]
> Vous devez au moins être membre du groupe **Opérateurs de sauvegarde** ou **Administrateurs** pour effectuer ces étapes.

**Pour créer un disque dur virtuel**

1.  Dans le menu **Action**, sélectionnez **Créer un disque dur virtuel**.

2.  Dans la boîte de dialogue **Créer et attacher un disque virtuel**, spécifiez l’emplacement sur l’ordinateur physique où le fichier de disque dur virtuel doit être stocké ainsi que la taille du disque dur virtuel.

3.  Dans **Format de disque dur virtuel**, sélectionnez **Taille dynamique** ou **Taille fixe**, puis cliquez sur **OK**.

## <a name="attaching-and-detaching-a-vhd"></a>Attachement et détachement d’un disque dur virtuel

Pour rendre un disque dur virtuel disponible (un disque que vous venez de créer ou un disque dur virtuel existant): 

1. Dans le menu **Action**, sélectionnez **Attacher un disque dur virtuel**.

2. Spécifiez l’emplacement du disque dur virtuel à l’aide d’un chemin d’accès complet.

Pour détacher le disque dur virtuel, afin de le rendre indisponible: cliquez avec le bouton droit sur le disque, sélectionnez **Détacher un disque dur virtuel**, puis cliquez sur **OK**. Le détachement d’un disque dur virtuel ne supprime pas le disque ni les données stockées sur celui-ci.

## <a name="additional-considerations"></a>Autres éléments à prendre en considération

-   Le chemin d’accès spécifié pour l’emplacement du disque dur virtuel doit être un chemin d’accès complet et ne peut pas se trouver dans le répertoire \\Windows.
-   La taille minimale d’un disque dur virtuel est de 3mégaoctets (Mo).
-   Un disque dur virtuel peut uniquement être un disque de base.
-   Dans la mesure où un disque dur virtuel est initialisé lors de sa création, la création d’un disque dur virtuel de taille fixe volumineux peut prendre du temps.
