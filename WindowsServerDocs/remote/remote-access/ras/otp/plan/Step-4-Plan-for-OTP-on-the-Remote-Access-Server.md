---
title: Plan de l’étape 4 pour OTP sur le serveur d’accès à distance
description: Cette rubrique fait partie du guide de déploiement des accès à distance avec authentification OTP dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b97b2fd-767a-45c1-a64e-5b3edd0c8a47
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a22d58e8e775ae341691cab1f5b40f8121ea533f
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282384"
---
# <a name="step-4-plan-for-otp-on-the-remote-access-server"></a>Plan de l’étape 4 pour OTP sur le serveur d’accès à distance

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Après la planification pour les paramètres de certificat et le mot de passe à usage unique serveur RADIUS de (OTP), la dernière étape de planification d’un déploiement OTP d’accès à distance consiste à planifier les paramètres d’OTP client sur le serveur d’accès à distance.  
  
|Tâche|Description|  
|----|--------|  
|[4.1 planifier les exemptions d’OTP client](#bkmk_4_1_Exemptions)|Planifier les exemptions pour les utilisateurs qui n’est pas nécessaire pour l’authentification à l’aide de secret à usage unique.|  
|[4.2 planifier pour les clients Windows 7](#bkmk_4_2_Win7)|Envisagez de déployer la connectivité Assistant DirectAccess (DCA) 2.0 sur les ordinateurs clients Windows 7.|  
|[4.3 plan des cartes à puce](#BKMK_smartcard)|Planifier l’utilisation de cartes à puce pour une autorisation supplémentaire.|  
  
## <a name="bkmk_4_1_Exemptions"></a>4.1 planifier les exemptions d’OTP client  
Lorsque l’authentification unique est activée, par défaut tous les utilisateurs doivent s’authentifier à l’aide d’une combinaison nom d’utilisateur et mot de passe et les informations d’identification de secret à usage unique. Toutefois, vous pouvez autoriser les utilisateurs sélectionnés pour s’authentifier à l’aide d’un nom d’utilisateur et le mot de passe uniquement, sans le secret à usage unique. Pour ce faire, créez un groupe de sécurité et ajoutez tous les utilisateurs que vous souhaitez à exclure de l’authentification unique.  
  
> [!NOTE]  
> Seuls les ordinateurs clients à partir d’une seule forêt peuvent être exemptés due à ce groupe de sécurité qu’une seule peut être sélectionné pour les exemptions de client.  
  
## <a name="bkmk_4_2_Win7"></a>4.2 planifier pour les clients Windows 7  
Par défaut, les ordinateurs clients Windows 7 ne peut pas s’authentifier à l’aide de secret à usage unique.  Les ordinateurs clients Windows 7 requièrent DCA 2.0 pour s’authentifier à l’aide de secret à usage unique dans un déploiement de l’accès à distance de Windows Server 2012. Pour plus d’informations sur l’Assistant DCA 2.0, consultez [2.0 de l’Assistant connectivité DirectAccess](https://go.microsoft.com/fwlink/?LinkId=253699) sur du Microsoft Download Center.  
  
## <a name="BKMK_smartcard"></a>4.3 plan des cartes à puce  
Lorsque l’authentification unique est activée, l’option pour activer l’utilisation de cartes à puce pour une autorisation supplémentaire est disponible. Créer un groupe de sécurité pour autoriser l’accès temporaire dans le cas où une carte à puce n’est pas fonctionnel.  
  
## <a name="BKMK_Links"></a>Voir aussi  
  
-   [Configurer DirectAccess avec l’authentification OTP](https://technet.microsoft.com/windows-server-docs/networking/remote-access/ras/otp/deploy-ra-otp)  
  


