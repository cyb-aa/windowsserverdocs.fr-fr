---
title: "Personnaliser le volet de Navigation du centre d’administration ActiveDirectory"
ms.prod: windows-server-threshold
description: "Sécurité de Windows Server"
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.assetid: c9933d16-e153-435a-b5b7-3e594db42d5c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: e7b1128d93912f724225905bedd38131f8aab0b2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="customize-the-active-directory-administrative-center-navigation-pane"></a>Personnaliser le volet de Navigation du centre d’administration ActiveDirectory

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

  Vous pouvez parcourir le volet de navigation du centre d’administration ActiveDirectory à l’aide de l’arborescence, qui est similaire à l’arborescence de la console ActiveDirectory Users and Computers, ou à l’aide de l’affichage de liste.

 Si vous utilisez le mode arborescence ou l’affichage de liste, vous pouvez personnaliser votre volet de navigation du centre d’administration ActiveDirectory à tout moment en ajoutant divers conteneurs du domaine local ou de tout domaine étranger \ (autrement dit, un domaine autre que le domaine local a une établi la relation d’approbation avec le domain\ local) au volet de navigation en tant que nœuds distincts. Personnalisation du volet de navigation du centre d’administration ActiveDirectory peut fournir un accès plus rapide aux objets ActiveDirectory. Pour plus d’informations, voir [gérer des domaines différents dans le centre d’administration ActiveDirectory ](manage-different-domains-in-active-directory-administrative-center.md).

 En outre, pour personnaliser davantage le volet de navigation, vous pouvez renommer ou supprimer ces nœuds ajoutés manuellement, créer des copies de ces nœuds ou les déplacer vers le haut ou vers le bas dans le volet de navigation.

> [!NOTE]
>  Vous ne pouvez pas personnaliser le nœud de domaine local par défaut.

### <a name="to-customize-the-active-directory-administrative-center-navigation-pane"></a>Pour personnaliser le volet de navigation du centre d’administration ActiveDirectory

1.  Dans le volet de navigation centre d’administration ActiveDirectory, cliquez droit sur le nœud que vous souhaitez modifier. Vous pouvez modifier la position ou le nom du nœud, ou vous pouvez créer une copie.

2.  Cliquez sur une des commandes suivantes:

    -   **Changement de nom**

    -   **Créer un nœud dupliqué**

    -   **Supprimer**

    -   **Déplacer vers le haut**

    -   **Déplacer vers le bas**

 À l’aide de l’affichage de liste, vous pouvez tirer parti de la liste \(MRU\) des derniers fichiers utilisés. La liste apparaît automatiquement sous un nœud de navigation lorsque vous visitez au moins un conteneur dans ce nœud de navigation. Vous pouvez également afficher la liste actuelle en développant la barre de navigation en haut de la fenêtre du centre d’administration ActiveDirectory. La liste contient toujours les trois derniers conteneurs visités dans un nœud de navigation particulier. Chaque fois que vous sélectionnez un conteneur particulier, ce conteneur est ajouté en haut de la liste et le dernier conteneur inclus dans la liste en est supprimé.

  

