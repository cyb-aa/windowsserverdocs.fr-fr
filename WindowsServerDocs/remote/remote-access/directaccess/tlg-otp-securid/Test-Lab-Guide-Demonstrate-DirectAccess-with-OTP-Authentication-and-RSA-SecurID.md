---
title: 'Guide de laboratoire de test : illustrer DirectAccess avec l’authentification OTP et RSA SecurID'
description: Cette rubrique fait partie du Guide de laboratoire de Test - démontrer DirectAccess avec l’authentification OTP et RSA SecurID pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 10c7a49c-5671-4bec-b562-13fdd67f4629
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a6e4b03ceb331899ac622bffe44061a7d92f5a09
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866230"
---
# <a name="test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid"></a>Guide du laboratoire de test : Décrire DirectAccess avec l’authentification OTP et RSA SecurID

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Accès à distance est un rôle de serveur dans le système d’exploitation Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012 qui permet aux utilisateurs distants un accès sécurisé aux ressources réseau internes à l’aide de DirectAccess ou des réseaux privés virtuels (VPN) avec le service de routage Service et accès distant (RRAS). Ce guide contient des instructions pas à pas permettant d’élargir le [Guide de laboratoire de Test : Démontrer DirectAccess configuration d’un serveur dans un environnement mixte IPv4 et IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004) pour illustrer une configuration de mot de passe à usage unique (OTP) d’accès à distance.  
  
> [!WARNING]  
> La conception de ce guide de laboratoire de test inclut des serveurs d’infrastructure, comme un contrôleur de domaine et une autorité de certification (CA) qui exécutent Windows Server 2012 R2 ou Windows Server 2012. À l’aide de ce guide de laboratoire de test pour configurer les serveurs d’infrastructure qui exécutent d’autres systèmes d’exploitation n’a pas été testée et des instructions pour configurer d’autres systèmes d’exploitation ne sont pas incluses dans ce guide.  
  
## <a name="about-this-guide"></a>À propos de ce guide  
Accès à distance dans Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012 prend en charge l’authentification du client avec l’OTP. Dans le cadre de ce laboratoire de test uniquement RSA SecurID est utilisé pour illustrer des fonctionnalités d’OTP avec accès à distance. Autres RADIUS basé OTP solutions sont également pris en charge, mais sont en dehors de la portée de ce laboratoire de test. Ce guide contient des instructions permettant de configurer le rôle serveur Accès à distance et d’en faire la démonstration à l’aide de six serveurs et deux ordinateurs clients. L’accès à distance terminé avec le laboratoire de test OTP simule un intranet, Internet et un réseau domestique et illustre la fonctionnalité d’accès à distance dans différents scénarios de connexion Internet.  
  
> [!IMPORTANT]  
> Pour appuyer cette démonstration, il utilise un nombre minimum d’ordinateurs. La configuration détaillée dans ce guide est fournie à des fins de tests en laboratoire seulement et ne doit pas être utilisée dans un environnement de production.  
  


