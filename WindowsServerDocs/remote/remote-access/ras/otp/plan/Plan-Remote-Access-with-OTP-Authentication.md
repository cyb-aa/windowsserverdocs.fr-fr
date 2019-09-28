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
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 69127ef86e1e14620f8cbb29322e930c6d921702
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404355"
---
# <a name="plan-remote-access-with-otp-authentication"></a>Planifier l’accès à distance avec l’authentification par mot de passe à usage unique

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

 Windows Server 2016 et Windows Server 2012 combinent DirectAccess et le réseau privé virtuel (RRAS) du service de routage et d’accès à distance (RRAS) en un seul rôle d’accès à distance. Cette vue d’ensemble fournit une introduction aux étapes de configuration requises pour déployer un déploiement multisite de l’accès à distance Windows Server 2016 ou Windows Server 2012 unique.  
  
  
-  Étape 1 : [Déployez un serveur DirectAccess unique avec des paramètres avancés](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings). Cette étape comprend la planification de l’infrastructure requise pour déployer un serveur unique. Il comprend la planification des paramètres réseau et serveur, les certificats requis, les paramètres DNS, le déploiement du serveur emplacement réseau, les serveurs d’administration DirectAccess, les paramètres de Active Directory et les objets stratégie de groupe (GPO).  
  
-   [Étape 2 : Planifier le déploiement du serveur RADIUS @ no__t-0  
  
-   [Étape 3 : Planifier le déploiement des certificats avec mot de passe à usage unique @ no__t-0  
  
-   [Étape 4 : Planifier le mot de passe à usage unique sur le serveur d’accès à distance @ no__t-0  
  
Une fois ces étapes de planification terminées, consultez [configurer l’accès à distance avec l’authentification par mot de passe à usage unique](https://technet.microsoft.com/windows-server-docs/networking/remote-access/ras/otp/configure/configure-ra-with-otp-authentication). Pour plus d’informations sur la configuration d’un déploiement multisite en tant que preuve de concept dans un environnement Lab, consultez le Guide de laboratoire [Test : Montrez DirectAccess avec l’authentification par mot de passe à usage unique et RSA SecurID @ no__t-0.  
  


