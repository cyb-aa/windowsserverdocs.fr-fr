---
title: Étapes de configuration du laboratoire de test de cluster DirectAccess avec équilibrage de charge réseau
description: Cette rubrique fait partie du Guide de laboratoire de test-démonstration de DirectAccess dans un cluster avec Windows NLB pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e508d3ee-ffa6-463f-a3dd-9e35e745c005
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 108c63298ad3382f5ece790258f2d278bb03b78b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388402"
---
# <a name="steps-for-configuring-the-directaccess-cluster-nlb-test-lab"></a>Étapes de configuration du laboratoire de test de cluster DirectAccess avec équilibrage de charge réseau

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Les étapes suivantes décrivent comment configurer l’infrastructure d’accès à distance, configurer les serveurs et les clients d’accès à distance et tester la connectivité DirectAccess à partir d’Internet et de sous-réseaux HomeNet.  
  
Dans ce guide de laboratoire de test, vous allez créer un cluster d’accès à distance avec équilibrage de la charge réseau (NLB) en procédant comme suit :  
  
-   [Étape 1 : terminer la configuration de DirectAccess](STEP-1-Complete-the-DirectAccess-Configuration.md). Effectuez toutes les étapes décrites dans le Guide de laboratoire [Test : Démonstration de la configuration d’un seul serveur DirectAccess avec mixte IPv4 et IPv6 @ no__t-0.  
  
-   [ÉTAPE 2 : Configurez EDGE1 @ no__t-0. Configurez le rôle accès à distance sur EDGE1 pour l’équilibrage de charge.  
  
-   [ÉTAPE 3 : Installez et configurez EDGE2 @ no__t-0. EDGE2 joue le rôle de second serveur d’accès à distance dans un cluster d’accès à distance.  
  
-   [ÉTAPE 4 : Créer le cluster d’accès à distance avec équilibrage de la charge réseau @ no__t-0-EDGE1 est configuré en tant que premier serveur dans un cluster d’accès à distance. EDGE2 est joint au cluster et l’équilibrage de la charge réseau est configuré pour le cluster.  
  
-   [ÉTAPE 5 : Testez la connectivité DirectAccess à partir d’Internet et par le biais du cluster @ no__t-0. Une fois l’équilibrage de la charge réseau et la configuration du cluster terminés, vous pouvez tester la connectivité du client DirectAccess via le cluster à charge équilibrée.  
  
-   [ÉTAPE 6 : Tester la connectivité du client DirectAccess derrière un périphérique NAT @ no__t-0. Déplacez l’ordinateur client derrière un périphérique NAT pour simuler le test de la connectivité du client DirectAccess derrière un routeur d’hébergement.  
  
-   [ÉTAPE 7 : Tester la connectivité lors du retour au corpnet @ no__t-0. Assurez-vous que l’ordinateur client peut toujours accéder aux ressources de l’entreprise lors du retour à Corpnet.  
  
-   [ÉTAPE 8 : Capture instantanée de la configuration @ no__t-0. À l’issue du laboratoire de test, prenez un instantané du cluster NLB d’accès à distance opérationnel pour pouvoir y revenir ultérieurement et tester des scénarios supplémentaires.  
  


