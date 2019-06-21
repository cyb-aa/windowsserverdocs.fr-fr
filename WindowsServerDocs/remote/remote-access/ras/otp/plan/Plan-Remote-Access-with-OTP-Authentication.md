---
title: Planifier l’accès à distance avec l’authentification par mot de passe à usage unique
description: Cette rubrique fait partie du guide de déploiement des accès à distance avec authentification OTP dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 762bc463-eead-46ac-8b90-32355743c27c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 328e092dff23495203ee21fccbace1f7f36918d2
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280812"
---
# <a name="plan-remote-access-with-otp-authentication"></a>Planifier l’accès à distance avec l’authentification par mot de passe à usage unique

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

 Permet de combiner DirectAccess, routage et Service d’accès distant (RRAS) VPN dans un seul rôle accès à distance Windows Server 2016 et Windows Server 2012. Cette vue d’ensemble présente les étapes de configuration requises pour déployer un déploiement multisite unique Windows Server 2016 ou d’accès à distance de Windows Server 2012.  
  
  
-  Étape 1 : [Déployer un serveur DirectAccess unique avec des paramètres avancés](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings). Cette étape comprend la planification de l’infrastructure requise pour déployer un serveur unique. Il comprend la planification de réseau et les paramètres de serveur, configuration requise des certificats, les paramètres DNS, déploiement du serveur emplacement réseau, serveurs d’administration DirectAccess, paramètres Active Directory et objets de stratégie de groupe (GPO).  
  
-   [Étape 2 : Planifier le déploiement de serveur RADIUS](Step-2-Plan-the-RADIUS-Server-Deployment.md)  
  
-   [Étape 3 : Planifier le déploiement de certificat OTP](Step-3-Plan-OTP-Certificate-Deployment.md)  
  
-   [Étape 4 : Planifiez d’OTP sur le serveur d’accès à distance](Step-4-Plan-for-OTP-on-the-Remote-Access-Server.md)  
  
Une fois que vous avez terminé ces étapes de planification, consultez [configurer l’accès à distance avec l’authentification OTP](https://technet.microsoft.com/windows-server-docs/networking/remote-access/ras/otp/configure/configure-ra-with-otp-authentication). Pour plus d’informations sur la configuration d’un déploiement multisite comme preuve de concept dans un environnement lab, consultez [Guide de laboratoire de Test : Décrire DirectAccess avec l’authentification OTP et RSA SecurID](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/tlg-otp-securid/test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid).  
  


