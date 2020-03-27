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
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 8a0d77208425774c8ea2b4fc663fc43e2a02ee5b
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314534"
---
# <a name="steps-for-configuring-the-test-lab"></a>Étapes de configuration du laboratoire de test

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Les étapes suivantes décrivent comment configurer l’infrastructure d’accès à distance, configurer les serveurs et les clients d’accès à distance et tester la connectivité DirectAccess à partir d’Internet et de sous-réseaux HomeNet.  
  
Dans ce guide de laboratoire de test, vous allez créer un déploiement de l’accès à distance multisite en procédant comme suit :  
  
-   [Étape 1 : terminez la configuration de base](assetId:///9eb4a9ba-9118-4ea3-8963-e643ec81c3ed). Effectuez toutes les étapes décrites dans le [Guide de laboratoire de test : démonstration de la configuration d’un seul serveur DirectAccess avec mixte IPv4 et IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004).  
  
-   [Étape 2 : installer et configurer ROUTEUR1](assetId:///e4b1a298-d5b0-410e-970b-c5358a9378f9). ROUTEUR1 offre des fonctionnalités de routage et de transfert entre les sous-réseaux corpnet et 2-Corpnet.  
  
-   [Étape 3 : installez et configurez client2](assetId:///6cbee1b5-f6f6-443f-8fa9-31cc5c05a0ee). CLIENT2 est un ordinateur client Windows 7 utilisé pour illustrer la compatibilité descendante d’un déploiement de l’accès à distance Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
-   [Étape 4 : configurer App1](assetId:///a0ee655e-c01e-4bf3-a7b3-064e9614f810). Configurez APP1 avec ROUTEUR1 en tant que passerelle par défaut et 2-DC1 en tant que serveur DNS secondaire.  
  
-   [Étape 5 : configurez DC1](assetId:///205ca795-93ce-4e53-aa6b-b44c87f0e14a). Configurez DC1 avec un site de Active Directory supplémentaire et des groupes de sécurité supplémentaires pour les ordinateurs clients Windows 7.  
  
-   [Étape 6 : installer et configurer 2-DC1](assetId:///16752f61-edbf-4ff4-9d7a-e2077b66a127). Dans un déploiement multisite, vous avez au moins deux domaines et sites. 2-DC1 fournit le contrôleur de domaine et les services DNS pour le domaine corp2.corp.contoso.com.  
  
-   [Étape 7 : installez et configurez 2-App1](assetId:///7d04b54e-590a-4d33-9766-415789859f29). 2-APP1 est un serveur Web et un serveur de fichiers dans le réseau 2-Corpnet.  
  
-   [Étape 8 : configurer INET1](assetId:///8ecc0b63-8626-4939-8d26-3d51d051d231). INET1 simule Internet dans ce guide de laboratoire de test. Vous devez configurer une entrée DNS qui correspond à l’adresse IP publique de 2-EDGE1.  
  
-   [Étape 9 : configurer Edge1](assetId:///562744dc-30f6-42fa-bd5f-60a013b2179e). Configurez le serveur DNS 2-corpnet et le routage sur EDGE1.  
  
-   [Étape 10 : installer et configurer 2-Edge1](assetId:///1938c4f3-ca96-475d-9f2e-6bea3b7a4130). Deux serveurs d’accès à distance sont requis dans un déploiement multisite. 2-EDGE1 fournit des services d’accès à distance pour le deuxième domaine.  
  
-   [Étape 11 : configurer le déploiement multisite](assetId:///537e4b68-043f-49c9-94d8-15ce8c4b18e2). Après avoir configuré les deux serveurs d’accès à distance, vous pouvez configurer votre déploiement multisite.  
  
-   [Étape 12 : tester la connectivité DirectAccess](assetId:///aa293b5d-4b6f-4004-95f3-0ab54804b15c). Testez la connectivité DirectAccess à partir des deux ordinateurs clients à partir du sous-réseau Internet via EDGE1 et 2-EDGE1.  
  
-   [Étape 13 : tester la connectivité DirectAccess derrière un périphérique NAT](assetId:///41f8195b-00a1-4991-9db8-3703514dbe0c). Tester la connectivité DirectAccess derrière un périphérique NAT.  
  
-   [Étape 14 : instantané de la configuration](assetId:///7b56d5c9-c334-463e-9e29-d652ca110d84). À l’issue du laboratoire de test, prenez un instantané du déploiement multisite de l’accès à distance actif pour pouvoir y revenir ultérieurement et tester des scénarios supplémentaires.  
  


