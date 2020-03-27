---
title: Planifier l’accès à distance avec l’authentification par mot de passe à usage unique
description: Cette rubrique fait partie du guide déployer l’accès à distance avec l’authentification par mot de passe à usage unique dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 762bc463-eead-46ac-8b90-32355743c27c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1f0b030667b0b10a22b4e90d1ddff87086c04aa0
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313607"
---
# <a name="plan-remote-access-with-otp-authentication"></a>Planifier l’accès à distance avec l’authentification par mot de passe à usage unique

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

 Windows Server 2016 et Windows Server 2012 combinent DirectAccess et le réseau privé virtuel (RRAS) du service de routage et d’accès à distance (RRAS) en un seul rôle d’accès à distance. Cette vue d’ensemble fournit une introduction aux étapes de configuration requises pour déployer un déploiement multisite de l’accès à distance Windows Server 2016 ou Windows Server 2012 unique.  
  
  
-  Étape 1 : [déployer un serveur DirectAccess unique avec des paramètres avancés](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings). Cette étape comprend la planification de l’infrastructure requise pour déployer un serveur unique. Il comprend la planification des paramètres réseau et serveur, les certificats requis, les paramètres DNS, le déploiement du serveur emplacement réseau, les serveurs d’administration DirectAccess, les paramètres de Active Directory et les objets stratégie de groupe (GPO).  
  
-   [Étape 2 : planifier le déploiement du serveur RADIUS](Step-2-Plan-the-RADIUS-Server-Deployment.md)  
  
-   [Étape 3 : planifier le déploiement de certificats avec mot de passe à usage unique](Step-3-Plan-OTP-Certificate-Deployment.md)  
  
-   [Étape 4 : planifier le mot de passe à usage unique sur le serveur d’accès à distance](Step-4-Plan-for-OTP-on-the-Remote-Access-Server.md)  
  
Une fois ces étapes de planification terminées, consultez [configurer l’accès à distance avec l’authentification par mot de passe à usage unique](https://technet.microsoft.com/windows-server-docs/networking/remote-access/ras/otp/configure/configure-ra-with-otp-authentication). Pour plus d’informations sur la configuration d’un déploiement multisite comme preuve de concept dans un environnement Lab, consultez [Guide de laboratoire de test : démonstration de DirectAccess avec l’authentification par mot de passe à usage unique et RSA SecurID](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/tlg-otp-securid/test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid).  
  


