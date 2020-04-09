---
title: Personnaliser le volet de navigation Centre d’administration Active Directory
ms.prod: windows-server
description: Sécurité de Windows Server
ms.assetid: c9933d16-e153-435a-b5b7-3e594db42d5c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 0053fc4e4f3e22a8fd98e242e38fc7c4e2002867
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857002"
---
# <a name="customize-the-active-directory-administrative-center-navigation-pane"></a>Personnaliser le volet de navigation Centre d’administration Active Directory

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

  Vous pouvez parcourir le volet de navigation Centre d’administration Active Directory à l’aide de l’arborescence, qui est similaire à l’arborescence de la console Active Directory utilisateurs et ordinateurs, ou à l’aide de la vue liste.

 Que vous utilisiez l’arborescence ou le mode liste, vous pouvez personnaliser votre Centre d’administration Active Directory volet de navigation à tout moment en ajoutant divers conteneurs à partir du domaine local ou de tout domaine étranger \(autrement dit, un domaine autre que le domaine local qui a une approbation établie avec le domaine local\) au volet de navigation comme des nœuds distincts. La personnalisation du volet de navigation Centre d’administration Active Directory peut fournir un accès plus rapide aux objets Active Directory. Pour plus d’informations, consultez [gérer des domaines différents dans Centre d’administration Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

 En outre, pour personnaliser davantage le volet de navigation, vous pouvez renommer ou supprimer ces nœuds ajoutés manuellement, créer des copies de ces nœuds ou les déplacer vers le haut ou le bas dans le volet de navigation.

> [!NOTE]
>  Il n’est pas possible de personnaliser le nœud du domaine local par défaut.

### <a name="to-customize-the-active-directory-administrative-center-navigation-pane"></a>Pour personnaliser le volet de navigation Centre d’administration Active Directory

1. Dans le volet de navigation Centre d’administration Active Directory, cliquez avec le bouton\-droit sur le nœud que vous souhaitez modifier. Vous pouvez modifier la position ou le nom du nœud, ou en créer une copie.

2. Cliquez sur l’une des commandes suivantes :

   -   **Renommer**

   -   **Créer un nœud dupliqué**

   -   **Supprimer**

   -   **Monter**

   -   **Descendre**

   En utilisant le mode liste, vous pouvez tirer parti de la dernière liste de \(MRU\). La liste MRU apparaît automatiquement sous un nœud de navigation lorsque vous visitez au moins un conteneur dans ce nœud de navigation. Vous pouvez également afficher la liste des fichiers récents en développant la barre de navigation en haut de la fenêtre de Centre d’administration Active Directory. La liste MRU contient toujours les trois derniers conteneurs que vous avez visités dans un nœud de navigation particulier. Chaque fois que vous sélectionnez un conteneur particulier, celui-ci est ajouté au début de la liste et le dernier conteneur inclus dans la liste en est supprimé.

  

