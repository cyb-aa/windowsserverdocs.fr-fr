---
title: Personnaliser le volet de navigation Centre d’administration Active Directory
ms.prod: windows-server
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
ms.openlocfilehash: 63038014207acd3846cb8db20c7836718615df51
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403756"
---
# <a name="customize-the-active-directory-administrative-center-navigation-pane"></a>Personnaliser le volet de navigation Centre d’administration Active Directory

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

  Vous pouvez parcourir le volet de navigation Centre d’administration Active Directory à l’aide de l’arborescence, qui est similaire à l’arborescence de la console Active Directory utilisateurs et ordinateurs, ou à l’aide de la vue liste.

 Que vous utilisiez l’arborescence ou le mode liste, vous pouvez personnaliser votre Centre d’administration Active Directory volet de navigation à tout moment en ajoutant divers conteneurs à partir du domaine local ou de tout domaine étranger \(that est un domaine autre que le domaine local qui dispose d’une approbation établie avec le domaine local @ no__t-1 dans le volet de navigation en tant que nœuds distincts. La personnalisation du volet de navigation Centre d’administration Active Directory peut fournir un accès plus rapide aux objets Active Directory. Pour plus d’informations, consultez [gérer des domaines différents dans Centre d’administration Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

 En outre, pour personnaliser davantage le volet de navigation, vous pouvez renommer ou supprimer ces nœuds ajoutés manuellement, créer des copies de ces nœuds ou les déplacer vers le haut ou le bas dans le volet de navigation.

> [!NOTE]
>  Il n’est pas possible de personnaliser le nœud du domaine local par défaut.

### <a name="to-customize-the-active-directory-administrative-center-navigation-pane"></a>Pour personnaliser le volet de navigation Centre d’administration Active Directory

1. Dans le volet de navigation Centre d’administration Active Directory, cliquez avec le bouton droit sur @ no__t-0click le nœud que vous souhaitez modifier. Vous pouvez modifier la position ou le nom du nœud, ou en créer une copie.

2. Cliquez sur l’une des commandes suivantes :

   -   **Renommer**

   -   **Créer un nœud dupliqué**

   -   **Supprimer**

   -   **Monter**

   -   **Descendre**

   En utilisant le mode liste, vous pouvez tirer parti de la dernière liste \(MRU @ no__t-1. La liste MRU apparaît automatiquement sous un nœud de navigation lorsque vous visitez au moins un conteneur dans ce nœud de navigation. Vous pouvez également afficher la liste des fichiers récents en développant la barre de navigation en haut de la fenêtre de Centre d’administration Active Directory. La liste MRU contient toujours les trois derniers conteneurs que vous avez visités dans un nœud de navigation particulier. Chaque fois que vous sélectionnez un conteneur particulier, celui-ci est ajouté au début de la liste et le dernier conteneur inclus dans la liste en est supprimé.

  

