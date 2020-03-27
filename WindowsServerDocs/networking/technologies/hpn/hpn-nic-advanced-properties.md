---
title: Propriétés avancées de carte réseau
description: Vous pouvez gérer des cartes réseau et toutes les fonctionnalités via Windows PowerShell ou le panneau de configuration réseau.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/20/2018
ms.openlocfilehash: 78d320f4309d60fa0396cbd723feafa07a65aea1
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317003"
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
