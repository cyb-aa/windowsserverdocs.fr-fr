---
title: Initialiser les nouveaux disques
description: "Cet article décrit comment initialiser les nouveaux disques"
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 587553746d45eceab654efd4d120038088d32991
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="initialize-new-disks"></a>Initialiser les nouveaux disques

> **S’applique à:** Windows10, Windows8.1, WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012

> [!NOTE]
> Vous devez au moins être membre du groupe **Opérateurs de sauvegarde** ou **Administrateurs** pour effectuer ces étapes.

## <a name="to-initialize-new-disks"></a>Pour initialiser les nouveaux disques
1.  Dans Gestion des disques, cliquez avec le bouton droit sur le disque à initialiser, puis cliquez sur **Initialiser le disque**.

2.  Dans la boîte de dialogue **Initialiser le disque**, sélectionnez le(s) disque(s) à initialiser. Vous avez le choix entre le style de partition d’enregistrement de démarrage principal (MBR) et de table de partition GUID (GPT).

## <a name="additional-considerations"></a>Autres éléments à prendre en considération

-   Les nouveaux disques apparaissent en tant que **Non initialisé**. Avant de pouvoir utiliser un disque, vous devez l’initialiser. Si vous démarrez Gestion des disques après avoir ajouté un disque, l’Assistant Initialisation de disque s’affiche pour vous aider à initialiser le disque.

> [!NOTE]
> Le nouveau disque est initialisé en tant que disque de base.

