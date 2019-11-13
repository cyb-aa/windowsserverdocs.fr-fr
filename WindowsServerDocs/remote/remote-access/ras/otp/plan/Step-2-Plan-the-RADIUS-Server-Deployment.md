---
title: Étape 2 planifier le déploiement du serveur RADIUS
description: Cette rubrique fait partie du guide déployer l’accès à distance avec l’authentification par mot de passe à usage unique dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d6ad863-02a5-49b0-9aff-d189e78b2b80
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a991b312a0938a3809acd2b94c00aa678f5b41da
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404394"
---
# <a name="step-2-plan-the-radius-server-deployment"></a>Étape 2 planifier le déploiement du serveur RADIUS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Après le déploiement d’un serveur d’accès à distance unique, planifiez le serveur d’authentification par mot de passe à usage unique.  
  
|Tâche|Description|  
|----|--------|  
|2,1 planifier le serveur RADIUS|Pour le serveur d’authentification par mot de passe à usage unique, l’accès à distance dans Windows Server 2016 et Windows Server 2012 prend en charge tous les serveurs avec mot de passe à usage unique RADIUS prenant en charge le protocole PAP (Password Authentication Protocol).|  
  
## <a name="BKMK_1.1"></a>2,1 planifier le serveur RADIUS  
Notez les points suivants lors de la planification d’un serveur RADIUS pour l’authentification par mot de passe à usage unique :  
  
-   Pour la plupart des types de déploiement à mot de passe à usage unique, vous devez configurer le serveur d’accès à distance en tant qu’agent RADIUS. Pour plus d’informations, consultez la documentation du fournisseur de mot de passe à usage unique.  
  
-   Pour tous les déploiements de mot de passe à usage unique, vous devez synchroniser vos utilisateurs Active Directory avec votre serveur RADIUS.  
  
-   Il n’est pas nécessaire que le serveur RADIUS soit membre d’un domaine.  
  
-   Lorsque vous déployez le serveur RADIUS, vous configurez un secret partagé et le numéro de port pour le trafic RADIUS. Prenez note de ces détails. ils sont requis lorsque vous configurez le serveur d’accès à distance.  
  
Vous pouvez consulter un exemple de guide de laboratoire de test qui définit l’authentification par mot de passe à usage unique avec un serveur RSA SecurID dans le [Guide de laboratoire de test : démonstration de DirectAccess avec l’authentification par mot de passe à usage unique et RSA SecurID](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/tlg-otp-securid/test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid)  
  
  
  


