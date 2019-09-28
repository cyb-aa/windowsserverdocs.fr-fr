---
title: Propriétés avancées de carte réseau
description: Vous pouvez gérer des cartes réseau et toutes les fonctionnalités via Windows PowerShell ou le panneau de configuration réseau.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/20/2018
ms.openlocfilehash: 1395cefca5d9ef696eed3f2735334954b9ee02a5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405716"
---
# <a name="nic-advanced-properties"></a>Propriétés avancées de carte réseau

Vous pouvez gérer les cartes réseau et toutes les fonctionnalités via Windows PowerShell à l’aide de l’applet de commande [NetAdapter](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps&viewFallbackFrom=winserverr2-ps) .  Vous pouvez également gérer des cartes réseau et toutes les fonctionnalités à l’aide du panneau de configuration réseau (ncpa. cpl). 

1. Dans **Windows PowerShell**, exécutez l’applet de commande `Get‑NetAdapterAdvancedProperties` sur deux marque/modèle de cartes réseau différentes.

   ![Acquérir-NetAdapterAdvancedProperty M1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-m1.png)

   ![NetAdapterAdvancedProperty C1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-c1.png)

   Ces deux listes de propriétés avancées de carte réseau présentent des similarités et des différences.

2. Dans le **panneau de configuration réseau** (ncpa. cpl), procédez comme suit :

   a. Cliquez avec le bouton droit sur la carte réseau.

   ![Boîte de dialogue Connexions réseau](../../media/network-offload-and-optimization/network-connections-dialog.png)

   b. Dans la boîte de dialogue Propriétés, cliquez sur **configurer**.

    ![Propriétés C1](../../media/network-offload-and-optimization/c1-properties.png)

   c. Cliquez sur l’onglet **avancé** pour afficher les propriétés avancées.<p>Les éléments de cette liste sont mis en corrélation avec les éléments de la sortie `Get-NetAdapterAdvancedProperties`.

   ![Propriétés de la carte réseau Chelsio](../../media/network-offload-and-optimization/chelsio-network-adapter-properties.png)

---
