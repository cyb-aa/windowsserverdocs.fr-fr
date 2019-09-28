---
title: Étapes de configuration du laboratoire de test
description: 'Cette rubrique fait partie du Guide de laboratoire de test : illustrer un déploiement multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc7205b4-a822-4038-ab67-ec0a870737f2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dd8b8864dff98e51bf55aad9307523df4a0c30bf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404697"
---
# <a name="steps-for-configuring-the-test-lab"></a>Étapes de configuration du laboratoire de test

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Les étapes suivantes décrivent comment configurer l’infrastructure d’accès à distance, configurer les serveurs et les clients d’accès à distance et tester la connectivité DirectAccess à partir d’Internet et de sous-réseaux HomeNet.  
  
Dans ce guide de laboratoire de test, vous allez créer un déploiement de l’accès à distance multisite en procédant comme suit :  
  
-   [ÉTAPE 1 : Terminez la configuration de base @ no__t-0. Effectuez toutes les étapes décrites dans le Guide de laboratoire [Test : Démonstration de la configuration d’un seul serveur DirectAccess avec mixte IPv4 et IPv6 @ no__t-0.  
  
-   [ÉTAPE 2 : Installez et configurez ROUTEUR1 @ no__t-0. ROUTEUR1 offre des fonctionnalités de routage et de transfert entre les sous-réseaux corpnet et 2-Corpnet.  
  
-   [ÉTAPE 3 : Installez et configurez CLIENT2 @ no__t-0. CLIENT2 est un ordinateur client Windows 7 utilisé pour illustrer la compatibilité descendante d’un déploiement de l’accès à distance Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
-   [ÉTAPE 4 : Configurez APP1 @ no__t-0. Configurez APP1 avec ROUTEUR1 en tant que passerelle par défaut et 2-DC1 en tant que serveur DNS secondaire.  
  
-   [ÉTAPE 5 : Configurez DC1 @ no__t-0. Configurez DC1 avec un site de Active Directory supplémentaire et des groupes de sécurité supplémentaires pour les ordinateurs clients Windows 7.  
  
-   [ÉTAPE 6 : Installez et configurez 2-DC1 @ no__t-0. Dans un déploiement multisite, vous avez au moins deux domaines et sites. 2-DC1 fournit le contrôleur de domaine et les services DNS pour le domaine corp2.corp.contoso.com.  
  
-   [ÉTAPE 7 : Installez et configurez 2-APP1 @ no__t-0. 2-APP1 est un serveur Web et un serveur de fichiers dans le réseau 2-Corpnet.  
  
-   [ÉTAPE 8 : Configurez INET1 @ no__t-0. INET1 simule Internet dans ce guide de laboratoire de test. Vous devez configurer une entrée DNS qui correspond à l’adresse IP publique de 2-EDGE1.  
  
-   [ÉTAPE 9 : Configurez EDGE1 @ no__t-0. Configurez le serveur DNS 2-corpnet et le routage sur EDGE1.  
  
-   [ÉTAPE 10 : Installez et configurez 2-EDGE1 @ no__t-0. Deux serveurs d’accès à distance sont requis dans un déploiement multisite. 2-EDGE1 fournit des services d’accès à distance pour le deuxième domaine.  
  
-   [ÉTAPE 11 : Configurer le déploiement multisite @ no__t-0. Après avoir configuré les deux serveurs d’accès à distance, vous pouvez configurer votre déploiement multisite.  
  
-   [ÉTAPE 12 : Tester la connectivité DirectAccess @ no__t-0. Testez la connectivité DirectAccess à partir des deux ordinateurs clients à partir du sous-réseau Internet via EDGE1 et 2-EDGE1.  
  
-   [ÉTAPE 13 : Tester la connectivité DirectAccess derrière un périphérique NAT @ no__t-0. Tester la connectivité DirectAccess derrière un périphérique NAT.  
  
-   [ÉTAPE 14 : Capture instantanée de la configuration @ no__t-0. À l’issue du laboratoire de test, prenez un instantané du déploiement multisite de l’accès à distance actif pour pouvoir y revenir ultérieurement et tester des scénarios supplémentaires.  
  


