---
title: Propriétés avancées de carte réseau
description: Vous pouvez gérer des cartes réseau et toutes les fonctionnalités par le biais de Windows PowerShell ou le panneau de configuration réseau.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/20/2018
ms.openlocfilehash: d1a5fb57bf71fd981e001cfd9ac595ab5bc3cfc5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819900"
---
# <a name="nic-advanced-properties"></a>Propriétés avancées de carte réseau

Vous pouvez gérer des cartes réseau et toutes les fonctionnalités par le biais de Windows PowerShell à l’aide du [NetAdapter](https://docs.microsoft.com/powershell/module/netadapter/?view=win10-ps&viewFallbackFrom=winserverr2-ps) applet de commande.  Vous pouvez également gérer des cartes réseau et toutes les fonctionnalités à l’aide du Panneau de configuration réseau (ncpa.cpl). 

1. Dans **Windows PowerShell**, exécutez le `Get‑NetAdapterAdvancedProperties` applet de commande par rapport à deux différents marque/modèle de cartes réseau.

   ![Get-NetAdapterAdvancedProperty m1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-m1.png)

   ![Get-NetAdapterAdvancedProperty c1](../../media/network-offload-and-optimization/Get-NetAdapterAdvancedProperty-c1.png)

   Il existe des similitudes et différences entre ces deux cartes réseau avancées Propriétés répertorie.

2. Dans le **le panneau de configuration réseau** (ncpa.cpl), procédez comme suit :

   a. et avec le bouton droit de la carte réseau.

   ![Boîte de dialogue Connexions réseau](../../media/network-offload-and-optimization/network-connections-dialog.png)

   b. Dans la boîte de dialogue Propriétés, cliquez sur **configurer**.

    ![Propriétés de C1](../../media/network-offload-and-optimization/c1-properties.png)

   c. Cliquez sur le **avancé** onglet pour afficher les propriétés avancées.<p>Les éléments dans cette liste est en corrélation avec les éléments dans le `Get-NetAdapterAdvancedProperties` sortie.

   ![Propriétés de la carte réseau de Chelsio](../../media/network-offload-and-optimization/chelsio-network-adapter-properties.png)

---