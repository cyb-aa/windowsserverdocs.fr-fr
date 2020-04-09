---
title: Vue d’ensemble du scénario de laboratoire de test
description: 'Cette rubrique fait partie du Guide de laboratoire de test : illustrer DirectAccess avec l’authentification par mot de passe à usage unique et RSA SecurID pour Windows Server 2016'
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: ce584811-b209-48fe-ab2b-4c399bd0bd79
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 39b6212714ebf868b91c1b28f35cc6a5149828ca
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860322"
---
# <a name="overview-of-the-test-lab-scenario"></a>Vue d’ensemble du scénario de laboratoire de test

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

L’accès à distance est un rôle serveur dans les systèmes d’exploitation Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012 qui permet aux utilisateurs distants d’accéder en toute sécurité aux ressources réseau internes à l’aide de DirectAccess ou des réseaux privés virtuels (VPN) avec le Service de routage et d’accès à distance (RRAS). Ce guide contient des instructions pas à pas pour l’extension du [Guide de laboratoire de test : démonstration de la configuration d’un seul serveur DirectAccess avec mixte IPv4 et IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004) pour illustrer la configuration d’un mot de passe à usage unique pour l’accès à distance.  
  
> [!WARNING]  
> La conception de ce guide de laboratoire de test comprend des serveurs d’infrastructure, tels qu’un contrôleur de domaine et une autorité de certification (CA) qui exécutent Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012. L’utilisation de ce guide de laboratoire de test pour configurer des serveurs d’infrastructure qui exécutent d’autres systèmes d’exploitation n’a pas été testée et les instructions de configuration d’autres systèmes d’exploitation ne sont pas incluses dans ce guide.  
  
## <a name="about-this-guide"></a>À propos de ce guide  
L’accès à distance dans Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012 ajoute la prise en charge de l’authentification du client avec un mot de passe à usage unique. Dans le cadre de ce laboratoire de test, seul RSA SecurID est utilisé pour illustrer la fonctionnalité de mot de passe à usage unique avec accès à distance. D’autres solutions à mot de passe à usage unique RADIUS sont également prises en charge, mais elles ne sont pas abordées dans ce laboratoire de test. Ce guide contient des instructions permettant de configurer le rôle serveur Accès à distance et d’en faire la démonstration à l’aide de six serveurs et deux ordinateurs clients. Le laboratoire de test accès à distance terminé avec mot de passe à usage unique simule un intranet, Internet et un réseau privé, et illustre les fonctionnalités d’accès à distance dans différents scénarios de connexion Internet.  
  
> [!IMPORTANT]  
> Pour appuyer cette démonstration, il utilise un nombre minimum d’ordinateurs. La configuration détaillée dans ce guide est fournie à des fins de tests en laboratoire seulement et ne doit pas être utilisée dans un environnement de production.  
  


