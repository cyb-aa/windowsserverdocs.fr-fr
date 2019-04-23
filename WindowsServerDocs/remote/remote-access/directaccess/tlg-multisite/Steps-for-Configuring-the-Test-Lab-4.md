---
title: Étapes de configuration du laboratoire de test
description: 'Cette rubrique fait partie du Guide de laboratoire de Test : illustrer un déploiement Multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc7205b4-a822-4038-ab67-ec0a870737f2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2bda808336624a5d80ed44cbadf543fc134c6190
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827880"
---
# <a name="steps-for-configuring-the-test-lab"></a>Étapes de configuration du laboratoire de test

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Les étapes suivantes décrivent comment configurer l’infrastructure d’accès à distance, de configurer les serveurs d’accès à distance et les clients et de tester la connectivité DirectAccess depuis les sous-réseaux du réseau domestique et Internet.  
  
Dans ce guide de laboratoire de test, vous allez générer un déploiement multisite de l’accès à distance en effectuant les étapes suivantes :  
  
-   [ÉTAPE 1 : Terminer la Configuration de Base](assetId:///9eb4a9ba-9118-4ea3-8963-e643ec81c3ed). Toutes les étapes dans le [Guide de laboratoire de Test : Montrez l’installation du serveur DirectAccess unique avec un environnement mixte IPv4 et IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004).  
  
-   [ÉTAPE 2 : Installer et configurer ROUTEUR1](assetId:///e4b1a298-d5b0-410e-970b-c5358a9378f9). ROUTEUR1 fournit le routage et de transfert des fonctionnalités entre les sous-réseaux du réseau d’entreprise et 2-réseau d’entreprise.  
  
-   [ÉTAPE 3 : Installer et configurer CLIENT2](assetId:///6cbee1b5-f6f6-443f-8fa9-31cc5c05a0ee). CLIENT2 est un ordinateur client Windows 7 qui est utilisé pour illustrer la descendante compatibilité d’un déploiement de Windows Server 2016, Windows Server 2012 R2 ou accès à distance de Windows Server 2012.  
  
-   [ÉTAPE 4 : Configurer APP1](assetId:///a0ee655e-c01e-4bf3-a7b3-064e9614f810). Configurer APP1 avec ROUTEUR1 comme passerelle par défaut et 2-DC1 en tant que le serveur DNS auxiliaire.  
  
-   [ÉTAPE 5 : Configurer DC1](assetId:///205ca795-93ce-4e53-aa6b-b44c87f0e14a). Configurer DC1 avec un site Active Directory supplémentaire et des groupes de sécurité supplémentaires pour les ordinateurs clients Windows 7.  
  
-   [ÉTAPE 6 : Installer et configurer 2-DC1](assetId:///16752f61-edbf-4ff4-9d7a-e2077b66a127). Dans un déploiement multisite, vous avez deux ou plusieurs domaines et sites. 2-DC1 fournit le contrôleur de domaine et les services DNS pour le domaine corp2.corp.contoso.com.  
  
-   [ÉTAPE 7 : Installer et configurer 2-APP1](assetId:///7d04b54e-590a-4d33-9766-415789859f29). 2-APP1 est un serveur web et le fichier dans le réseau de 2-réseau d’entreprise.  
  
-   [ÉTAPE 8 : Configurer INET1](assetId:///8ecc0b63-8626-4939-8d26-3d51d051d231). INET1 simule Internet dans ce guide de laboratoire de test. Vous devez configurer une entrée DNS qui correspond à l’adresse IP publique de 2-EDGE1.  
  
-   [ÉTAPE 9 : Configurer EDGE1](assetId:///562744dc-30f6-42fa-bd5f-60a013b2179e). Configurez le serveur DNS 2-réseau d’entreprise et le routage sur EDGE1.  
  
-   [ÉTAPE 10 : Installer et configurer 2-EDGE1](assetId:///1938c4f3-ca96-475d-9f2e-6bea3b7a4130). Deux serveurs d’accès à distance sont requis dans un déploiement multisite. 2-EDGE1 fournit des services d’accès à distance pour le deuxième domaine.  
  
-   [ÉTAPE 11 : Configurer un déploiement multisite](assetId:///537e4b68-043f-49c9-94d8-15ce8c4b18e2). Après avoir configuré les deux serveurs d’accès à distance, vous pouvez configurer votre déploiement multisite.  
  
-   [ÉTAPE 12 : Tester la connectivité de DirectAccess](assetId:///aa293b5d-4b6f-4004-95f3-0ab54804b15c). Tester la connectivité DirectAccess des deux ordinateurs clients à partir du sous-réseau Internet via EDGE1 et EDGE1-2.  
  
-   [ÉTAPE 13 : Tester la connectivité DirectAccess derrière un périphérique NAT](assetId:///41f8195b-00a1-4991-9db8-3703514dbe0c). Tester la connectivité DirectAccess derrière un périphérique NAT.  
  
-   [ÉTAPE 14 : La configuration de capture instantanée](assetId:///7b56d5c9-c334-463e-9e29-d652ca110d84). À l’issue de ce laboratoire de test, prenez un instantané de l’utilisation de déploiement multisite de l’accès à distance afin que vous pouvez y revenir plus tard pour tester des scénarios supplémentaires.  
  


