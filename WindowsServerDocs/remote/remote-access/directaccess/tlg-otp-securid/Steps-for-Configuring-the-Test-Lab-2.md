---
title: Étapes de configuration du laboratoire de test
description: 'Cette rubrique fait partie du Guide de laboratoire de test : illustrer DirectAccess avec l’authentification par mot de passe à usage unique et RSA SecurID pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0a40183c-afd1-43ca-b306-05745640a37d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ef5ce37983b8565fab8287eeaae7423be0c269f6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404724"
---
# <a name="steps-for-configuring-the-test-lab"></a>Étapes de configuration du laboratoire de test

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Les étapes suivantes décrivent comment configurer l’infrastructure d’accès à distance, configurer le client et le serveur d’accès à distance, et tester la connectivité DirectAccess à partir des sous-réseaux HomeNet et Internet.  
  
Dans ce guide de laboratoire de test, vous allez créer un accès à distance avec un environnement OTP en procédant comme suit :  
  
-   [ÉTAPE 1 : Effectuez la configuration DirectAccess @ no__t-0. Effectuez toutes les étapes décrites dans le Guide de laboratoire [Test : Démonstration de la configuration d’un seul serveur DirectAccess avec mixte IPv4 et IPv6 @ no__t-0.  
  
-   [ÉTAPE 2 : Configurez APP1 @ no__t-0. Configurez APP1 avec des modèles de certificat OTP pour une utilisation par EDGE1.  
  
-   [ÉTAPE 3 : Configurez DC1 @ no__t-0. Vérifiez le nom d’utilisateur principal défini sur DC1.  
  
-   [ÉTAPE 7 : Installez et configurez RSA @ no__t-0. Installez et configurez RSA, un serveur RADIUS et un serveur de mot de passe à usage unique et configurez EDGE1 pour OTP.  
  
-   [ÉTAPE 11 : Vérifiez l’intégrité du mot de passe à usage unique sur EDGE1 @ no__t-0. Assurez-vous que l’état du mot de passe à usage unique est sain sur le serveur d’accès à distance.  
  
-   [ÉTAPE 8 : Testez la connectivité DirectAccess à partir du sous-réseau HomeNet @ no__t-0. Testez la fonctionnalité de mot de passe à usage unique DirectAccess derrière un périphérique NAT.  
  
-   [ÉTAPE 10 : Tester la connectivité DirectAccess à partir d’Internet @ no__t-0. Tester la connectivité du client DirectAccess à partir d’Internet.  
  
-   [ÉTAPE 12 : Capture instantanée de la configuration @ no__t-0. À l’issue du laboratoire de test, prenez un instantané de la configuration DirectAccess en cours d’utilisation avec un mot de passe à usage unique pour pouvoir y revenir ultérieurement et tester des scénarios supplémentaires.  
  


