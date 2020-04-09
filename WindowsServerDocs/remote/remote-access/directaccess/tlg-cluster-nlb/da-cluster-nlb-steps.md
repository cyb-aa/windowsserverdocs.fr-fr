---
title: Étapes de configuration du laboratoire de test de cluster DirectAccess avec équilibrage de charge réseau
description: Cette rubrique fait partie du Guide de laboratoire de test-démonstration de DirectAccess dans un cluster avec Windows NLB pour Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: e508d3ee-ffa6-463f-a3dd-9e35e745c005
ms.author: lizross
author: eross-msft
ms.openlocfilehash: c4f683cd9745bdb61ee95d89bcfc37622a4acf2c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853012"
---
# <a name="steps-for-configuring-the-directaccess-cluster-nlb-test-lab"></a>Étapes de configuration du laboratoire de test de cluster DirectAccess avec équilibrage de charge réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Les étapes suivantes décrivent comment configurer l’infrastructure d’accès à distance, configurer les serveurs et les clients d’accès à distance et tester la connectivité DirectAccess à partir d’Internet et de sous-réseaux HomeNet.  
  
Dans ce guide de laboratoire de test, vous allez créer un cluster d’accès à distance avec équilibrage de la charge réseau (NLB) en procédant comme suit :  
  
-   [Étape 1 : terminer la configuration de DirectAccess](STEP-1-Complete-the-DirectAccess-Configuration.md). Effectuez toutes les étapes décrites dans le [Guide de laboratoire de test : démonstration de la configuration d’un seul serveur DirectAccess avec mixte IPv4 et IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004).  
  
-   [Étape 2 : configurer Edge1](STEP-2-Configure-EDGE1.md). Configurez le rôle accès à distance sur EDGE1 pour l’équilibrage de charge.  
  
-   [Étape 3 : installer et configurer EDGE2](STEP-3-Install-and-Configure-EDGE2.md). EDGE2 joue le rôle de second serveur d’accès à distance dans un cluster d’accès à distance.  
  
-   [Étape 4 : créer le cluster d’accès à distance avec équilibrage de la charge réseau](STEP-4-Create-the-Network-Load-Balanced-Remote-Access-Cluster.md)-Edge1 est configuré en tant que premier serveur dans un cluster d’accès à distance. EDGE2 est joint au cluster et l’équilibrage de la charge réseau est configuré pour le cluster.  
  
-   [Étape 5 : tester la connectivité DirectAccess à partir d’Internet et via le cluster](STEP-5-Test-DirectAccess-Connectivity-from-the-Internet-and-Through-the-Cluster.md). Une fois l’équilibrage de la charge réseau et la configuration du cluster terminés, vous pouvez tester la connectivité du client DirectAccess via le cluster à charge équilibrée.  
  
-   [Étape 6 : tester la connectivité des clients DirectAccess derrière un périphérique NAT](STEP-6-Test-DirectAccess-Client-Connectivity-from-Behind-a-NAT-Device.md). Déplacez l’ordinateur client derrière un périphérique NAT pour simuler le test de la connectivité du client DirectAccess derrière un routeur d’hébergement.  
  
-   [Étape 7 : tester la connectivité lors du retour au corpnet](STEP-7-Test-Connectivity-When-Returning-to-the-Corpnet.md). Assurez-vous que l’ordinateur client peut toujours accéder aux ressources de l’entreprise lors du retour à Corpnet.  
  
-   [Étape 8 : instantané de la configuration](da-cluster-nlb-s8-snapshot.md). À l’issue du laboratoire de test, prenez un instantané du cluster NLB d’accès à distance opérationnel pour pouvoir y revenir ultérieurement et tester des scénarios supplémentaires.  
  


