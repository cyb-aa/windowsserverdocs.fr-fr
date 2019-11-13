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

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Les étapes suivantes décrivent comment configurer l’infrastructure d’accès à distance, configurer le client et le serveur d’accès à distance, et tester la connectivité DirectAccess à partir des sous-réseaux HomeNet et Internet.  
  
Dans ce guide de laboratoire de test, vous allez créer un accès à distance avec un environnement OTP en procédant comme suit :  
  
-   [Étape 1 : terminez la configuration de DirectAccess](assetId:///4dbf877f-02fb-439b-907a-f5b3f1d8afa6). Effectuez toutes les étapes décrites dans le [Guide de laboratoire de test : démonstration de la configuration d’un seul serveur DirectAccess avec mixte IPv4 et IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004).  
  
-   [Étape 2 : configurer App1](assetId:///c1bb590f-91d4-4ed5-bceb-b0e36eabd4ff). Configurez APP1 avec des modèles de certificat OTP pour une utilisation par EDGE1.  
  
-   [Étape 3 : configurer DC1](assetId:///904a6edc-a771-45ed-9630-a34a680bb522). Vérifiez le nom d’utilisateur principal défini sur DC1.  
  
-   [Étape 7 : installer et configurer RSA](assetId:///baa4c28c-add7-42e2-8afd-ccc7a559406a). Installez et configurez RSA, un serveur RADIUS et un serveur de mot de passe à usage unique et configurez EDGE1 pour OTP.  
  
-   [Étape 11 : vérifier l’intégrité du mot de passe à usage unique sur Edge1](assetId:///3b397a4a-8478-47f2-a932-9e8e048c14ba). Assurez-vous que l’état du mot de passe à usage unique est sain sur le serveur d’accès à distance.  
  
-   [Étape 8 : tester la connectivité DirectAccess à partir du sous-réseau HomeNet](assetId:///ba1652a6-0692-4add-91ca-34a84956ba14). Testez la fonctionnalité de mot de passe à usage unique DirectAccess derrière un périphérique NAT.  
  
-   [Étape 10 : tester la connectivité DirectAccess à partir d’Internet](assetId:///321149eb-5f23-4a0b-b8fb-1244540126e9). Tester la connectivité du client DirectAccess à partir d’Internet.  
  
-   [Étape 12 : instantané de la configuration](assetId:///8a51ed3c-9c32-402f-85d1-617ce46845b4). À l’issue du laboratoire de test, prenez un instantané de la configuration DirectAccess en cours d’utilisation avec un mot de passe à usage unique pour pouvoir y revenir ultérieurement et tester des scénarios supplémentaires.  
  


