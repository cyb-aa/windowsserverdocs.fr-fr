---
title: Étapes de configuration du laboratoire de test de cluster DirectAccess avec équilibrage de charge réseau
description: Cette rubrique fait partie du Guide de laboratoire de Test - décrire de DirectAccess dans un Cluster avec équilibrage de charge réseau Windows pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e508d3ee-ffa6-463f-a3dd-9e35e745c005
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3a243debf79d2c9d12de511153b74f23c5a44cce
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880740"
---
# <a name="steps-for-configuring-the-directaccess-cluster-nlb-test-lab"></a>Étapes de configuration du laboratoire de test de cluster DirectAccess avec équilibrage de charge réseau

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Les étapes suivantes décrivent comment configurer l’infrastructure d’accès à distance, de configurer les serveurs d’accès à distance et les clients et de tester la connectivité DirectAccess depuis les sous-réseaux du réseau domestique et Internet.  
  
Dans ce test guide de laboratoire vous allez créer un réseau équilibrage de charge (NLB) activé cluster d’accès à distance en effectuant les étapes suivantes :  
  
-   [ÉTAPE 1 terminer la Configuration de DirectAccess](STEP-1-Complete-the-DirectAccess-Configuration.md). Toutes les étapes dans le [Guide de laboratoire de Test : Montrez l’installation du serveur DirectAccess unique avec un environnement mixte IPv4 et IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004).  
  
-   [ÉTAPE 2 : Configurer EDGE1](STEP-2-Configure-EDGE1.md). Configurer le rôle accès à distance sur EDGE1 pour l’équilibrage de charge.  
  
-   [ÉTAPE 3 : Installer et configurer EDGE2](STEP-3-Install-and-Configure-EDGE2.md). EDGE2 agit comme le second serveur d’accès à distance dans un cluster de l’accès à distance.  
  
-   [ÉTAPE 4 : Créer le cluster de l’accès à distance d’équilibrage de charge réseau](STEP-4-Create-the-Network-Load-Balanced-Remote-Access-Cluster.md)-EDGE1 est configuré en tant que le premier serveur dans un cluster de l’accès à distance. EDGE2 est joint au cluster et NLB est configuré pour le cluster.  
  
-   [ÉTAPE 5 : Tester la connectivité DirectAccess à partir d’Internet et via le cluster](STEP-5-Test-DirectAccess-Connectivity-from-the-Internet-and-Through-the-Cluster.md). Une fois la configuration NLB et du cluster terminée vous pouvez tester la connectivité du client DirectAccess via le cluster d’équilibrage de charge.  
  
-   [ÉTAPE 6 : Tester la connectivité du client DirectAccess situé derrière un périphérique NAT](STEP-6-Test-DirectAccess-Client-Connectivity-from-Behind-a-NAT-Device.md). Déplacer l’ordinateur client derrière un périphérique NAT pour simuler la connectivité des clients DirectAccess test derrière un routeur domestique.  
  
-   [ÉTAPE 7 : Tester la connectivité lors du retour au réseau d’entreprise](STEP-7-Test-Connectivity-When-Returning-to-the-Corpnet.md). Vérifiez que l’ordinateur client peut toujours accéder à des ressources d’entreprise lors du retour au réseau d’entreprise.  
  
-   [ÉTAPE 8 : La configuration de capture instantanée](da-cluster-nlb-s8-snapshot.md). À l’issue de ce laboratoire de test, prenez un instantané de l’utilisation de cluster NLB d’accès à distance afin que vous pouvez y revenir plus tard pour tester des scénarios supplémentaires.  
  


