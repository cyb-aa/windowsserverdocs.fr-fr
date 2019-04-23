---
title: Personnaliser le volet de Navigation du centre d’administration Active Directory
ms.prod: windows-server-threshold
description: Sécurité de Windows Server
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854840"
---
# <a name="customize-the-active-directory-administrative-center-navigation-pane"></a>Personnaliser le volet de Navigation du centre d’administration Active Directory

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

  Vous pouvez parcourir le volet de navigation du centre d’administration Active Directory à l’aide de l’arborescence, qui est similaire à l’arborescence de la console Active Directory Users and Computers, ou à l’aide de l’affichage de liste.

 Si vous utilisez l’arborescence ou la vue de liste, vous pouvez personnaliser votre volet de navigation du centre d’administration Active Directory à tout moment en ajoutant divers conteneurs à partir du domaine local ou de tout domaine étranger \(, autrement dit, un domaine autre que le domaine local qui possède une approbation établie avec le domaine local\) vers le volet de navigation en tant que nœuds distincts. Personnalisation du volet de navigation du centre d’administration Active Directory peut fournir un accès plus rapide aux objets Active Directory. Pour plus d’informations, consultez [gérer des domaines différents dans le centre d’administration Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

 En outre, pour personnaliser davantage le volet de navigation, vous pouvez renommer ou supprimer ces nœuds ajoutés manuellement, créer des copies de ces nœuds ou les déplacer vers le haut ou le bas dans le volet de navigation.

> [!NOTE]
>  Il n’est pas possible de personnaliser le nœud du domaine local par défaut.

### <a name="to-customize-the-active-directory-administrative-center-navigation-pane"></a>Pour personnaliser le volet de navigation du centre d’administration Active Directory

1.  Dans le volet de navigation du centre d’administration Active Directory avec le bouton droit\-cliquez sur le nœud que vous souhaitez modifier. Vous pouvez modifier la position ou le nom du nœud, ou en créer une copie.

2.  Cliquez sur une des commandes suivantes :

    -   **Changement de nom**

    -   **Créer un nœud dupliqué**

    -   **Supprimer**

    -   **Déplacer vers le haut**

    -   **Déplacer vers le bas**

 À l’aide de l’affichage de liste, vous pouvez tirer parti de la plus récemment utilisé \(MRU\) liste. La liste apparaît automatiquement sous un nœud de navigation lorsque vous visitez au moins un conteneur dans ce nœud de navigation. Vous pouvez également afficher la liste des derniers fichiers utilisés en cours en développant la barre de navigation en haut de la fenêtre du centre d’administration Active Directory. La liste des fichiers récents contient toujours les trois derniers conteneurs visités dans un nœud de navigation particulier. Chaque fois que vous sélectionnez un conteneur particulier, celui-ci est ajouté au début de la liste et le dernier conteneur inclus dans la liste en est supprimé.

  

