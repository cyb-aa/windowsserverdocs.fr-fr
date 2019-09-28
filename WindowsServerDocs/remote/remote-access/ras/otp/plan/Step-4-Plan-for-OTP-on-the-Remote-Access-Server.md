---
title: Étape 4 planifier le mot de passe à usage unique sur le serveur d’accès à distance
description: Cette rubrique fait partie du guide déployer l’accès à distance avec l’authentification par mot de passe à usage unique dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b97b2fd-767a-45c1-a64e-5b3edd0c8a47
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cc833ea2ae5d24754a445d6c1252f21a59cc6f13
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404382"
---
# <a name="step-4-plan-for-otp-on-the-remote-access-server"></a>Étape 4 planifier le mot de passe à usage unique sur le serveur d’accès à distance

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Après la planification des paramètres du serveur RADIUS et du certificat de mot de passe à usage unique, la dernière étape de la planification d’un déploiement OTP d’accès à distance consiste à planifier les paramètres de mot de passe à usage unique du client sur le serveur d’accès à distance.  
  
|Tâche|Description|  
|----|--------|  
|[4,1 planifier des exemptions de client avec mot de passe à usage unique](#bkmk_4_1_Exemptions)|Planifiez les exemptions pour les utilisateurs dont vous n’avez pas besoin pour l’authentification à l’aide du mot de passe à usage unique.|  
|[4,2 planifier des clients Windows 7](#bkmk_4_2_Win7)|Planifiez le déploiement de l’Assistant de connectivité DirectAccess (DCA) 2,0 sur les ordinateurs clients Windows 7.|  
|[4,3 planifier des cartes à puce](#BKMK_smartcard)|Planifiez l’utilisation de cartes à puce pour une autorisation supplémentaire.|  
  
## <a name="bkmk_4_1_Exemptions"></a>4,1 planifier des exemptions de client avec mot de passe à usage unique  
Lorsque l’authentification par mot de passe à usage unique est activée, par défaut, tous les utilisateurs doivent s’authentifier à l’aide d’une combinaison de nom d’utilisateur et mot de passe, ainsi que des informations d’identification OTP Toutefois, vous pouvez autoriser les utilisateurs sélectionnés à s’authentifier en utilisant un nom d’utilisateur et un mot de passe uniquement, sans mot de passe à usage unique. Pour ce faire, créez un groupe de sécurité et ajoutez tous les utilisateurs que vous souhaitez exempter de l’authentification par mot de passe à usage unique.  
  
> [!NOTE]  
> Seuls les ordinateurs clients d’une seule forêt peuvent être exemptés en raison du fait qu’un seul groupe de sécurité peut être sélectionné pour les exemptions du client.  
  
## <a name="bkmk_4_2_Win7"></a>4,2 planifier des clients Windows 7  
Par défaut, les ordinateurs clients Windows 7 ne peuvent pas s’authentifier avec un mot de passe à usage unique.  Les ordinateurs clients Windows 7 nécessitent DCA 2,0 pour s’authentifier à l’aide du mot de passe à usage unique dans un déploiement de l’accès à distance Windows Server 2012. Pour plus d’informations sur DCA 2,0, voir [Assistant de connectivité DirectAccess 2,0](https://go.microsoft.com/fwlink/?LinkId=253699) dans le centre de téléchargement Microsoft.  
  
## <a name="BKMK_smartcard"></a>4,3 planifier des cartes à puce  
Lorsque l’authentification par mot de passe à usage unique est activée, l’option permettant d’activer l’utilisation de cartes à puce pour une autorisation supplémentaire est disponible. Créez un groupe de sécurité pour autoriser l’accès temporaire dans le cas où la carte à puce d’un utilisateur ne fonctionnera pas.  
  
## <a name="BKMK_Links"></a>Voir aussi  
  
-   [Configurer DirectAccess avec l’authentification par mot de passe à usage unique](https://technet.microsoft.com/windows-server-docs/networking/remote-access/ras/otp/deploy-ra-otp)  
  


