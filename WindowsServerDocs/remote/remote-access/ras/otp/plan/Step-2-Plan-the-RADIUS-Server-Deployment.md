---
title: Étape 2 planifier le déploiement de serveur RADIUS
description: Cette rubrique fait partie du guide de déploiement des accès à distance avec authentification OTP dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d6ad863-02a5-49b0-9aff-d189e78b2b80
ms.author: pashort
author: shortpatti
ms.openlocfilehash: faf3f0b7c691edfb2c41e7b568e0791a3cad76b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859210"
---
# <a name="step-2-plan-the-radius-server-deployment"></a>Étape 2 planifier le déploiement de serveur RADIUS

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Après avoir déployé un seul serveur d’accès à distance, planifiez le serveur d’authentification de mot de passe à usage unique (OTP).  
  
|Tâche|Description|  
|----|--------|  
|2.1 planifier le serveur RADIUS|Pour le serveur d’authentification OTP, accès à distance dans Windows Server 2016 et Windows Server 2012 prend en charge n’importe quel serveur de secret à usage unique prenant en charge RADIUS qui prend en charge le protocole d’authentification de mot de passe (PAP).|  
  
## <a name="BKMK_1.1"></a>2.1 planifier le serveur RADIUS  
Notez les points suivants lors de la planification d’un serveur RADIUS pour l’authentification unique :  
  
-   Pour la plupart des types de déploiements de secret à usage unique, vous devez configurer le serveur d’accès à distance comme un agent de RADIUS. Pour plus d’informations, consultez la documentation du fournisseur secret à usage unique.  
  
-   Pour tous les déploiements de secret à usage unique, vous devez synchroniser vos utilisateurs Active Directory avec votre serveur RADIUS.  
  
-   Le serveur RADIUS n’a pas besoin d’être membre du domaine.  
  
-   Lorsque vous déployez le serveur RADIUS, vous configurez un secret partagé et le numéro de port pour le trafic RADIUS. Prenez note de ces détails ; ils sont nécessaires lorsque vous configurez le serveur d’accès à distance.  
  
Vous pouvez afficher un guide de laboratoire de test exemple configure l’authentification unique avec un serveur RSA SecurID dans [Guide de laboratoire de Test : Illustrer DirectAccess avec l’authentification OTP et RSA SecurID](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/tlg-otp-securid/test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid).  
  
  
  


