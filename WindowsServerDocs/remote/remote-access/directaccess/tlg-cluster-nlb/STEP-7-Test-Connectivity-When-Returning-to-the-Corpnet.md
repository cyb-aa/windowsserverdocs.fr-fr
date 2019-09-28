---
title: ÉTAPE 7 tester la connectivité lors du retour au corpnet
description: Cette rubrique fait partie du Guide de laboratoire de test-démonstration de DirectAccess dans un cluster avec Windows NLB pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5a7252d0-6db8-4a9d-98ee-75082ecd2929
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fa89745d6efcae3591bba2aa5a694ee651bc9912
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404859"
---
# <a name="step-7-test-connectivity-when-returning-to-the-corpnet"></a>ÉTAPE 7 tester la connectivité lors du retour au corpnet

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

La plupart de vos utilisateurs se déplacent entre les sites distants et le Corpnet. il est donc important que lorsqu’ils retournent au corpnet, ils puissent accéder aux ressources sans avoir à modifier la configuration. L’accès à distance rend cela possible, car lorsque le client DirectAccess revient au réseau d’entreprise, il est en mesure d’établir une connexion avec le serveur emplacement réseau. Une fois la connexion HTTPs établie avec succès sur le serveur emplacement réseau, le client DirectAccess désactive la configuration du client DirectAccess et utilise une connexion directe à Corpnet.  
  
### <a name="test-connectivity-on-client1"></a>Tester la connectivité sur CLIENT1  
  
1. Arrêtez CLIENT1, puis Déconnectez CLIENT1 du sous-réseau HomeNet ou du commutateur virtuel et connectez-le au sous-réseau Corpnet ou au commutateur virtuel. Activer CLIENT1 et ouvrir une session en tant que CORP\User1.  
  
2. Ouvrez une fenêtre Windows PowerShell avec élévation de privilèges, tapez **ipconfig/all**et appuyez sur entrée. La sortie indiquera que CLIENT1 a une adresse IP locale et qu’il n’existe aucun tunnel 6to4, Teredo ou IP-HTTPs actif.  
  
3. Testez la connectivité au partage réseau sur APP2. Dans l’écran d' **Accueil** , tapez<strong>\\ \ APP2\Files</strong>, puis appuyez sur entrée. Vous serez en mesure d’ouvrir le fichier dans ce dossier.  
  


