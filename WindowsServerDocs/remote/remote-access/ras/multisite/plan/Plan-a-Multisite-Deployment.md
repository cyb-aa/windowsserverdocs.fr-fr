---
title: Planifier un déploiement multisite
description: Cette rubrique fait partie du guide de déploiement de plusieurs serveurs d’accès distant dans un déploiement Multisite dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8387eabe-7363-4367-b5b1-03c67baa2933
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ba813fc5da53e9635e9ef1363f1a559a29419312
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282600"
---
# <a name="plan-a-multisite-deployment"></a>Planifier un déploiement multisite

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

 Permet de combiner DirectAccess, routage et Service d’accès distant (RRAS) VPN dans un seul rôle accès à distance Windows Server 2016, Windows Server 2012. Cette vue d’ensemble présente les étapes de planification requises pour déployer Windows Server 2016 ou accès à distance de Windows Server 2012 dans une configuration multisite.  
  
1.  [Déployer un serveur DirectAccess unique avec des paramètres avancés](https://technet.microsoft.com/library/hh831436(v=ws.11).aspx). Cette étape comprend la planification de l’infrastructure requise pour déployer un serveur unique. Il comprend la planification de réseau et les paramètres de serveur, configuration requise des certificats, les paramètres DNS, déploiement du serveur emplacement réseau, serveurs d’administration DirectAccess, paramètres Active Directory et objets de stratégie de groupe (GPO).  
  
2.  [Étape 2 planifier l’infrastructure multisite](Step-2-Plan-the-Multisite-Infrastructure.md). Cette étape inclut Active Directory et la planification de la stratégie de groupe et la configuration DNS.  
  
3.  [Étape 3 planifier le déploiement Multisite](Step-3-Plan-the-Multisite-Deployment.md). Cette étape comprend la planification des paramètres de certificat, configuration du serveur emplacement réseau, paramètres de point d’entrée client, les paramètres de préfixe IPv6 et paramètres d’équilibrage de charge globale si vous le souhaitez.  
  
> [!NOTE]  
> Enregistrez vos décisions de planification pour l’accès à distance avancé de déploiement. Cet enregistrement peut servir d'aide pour toute personne impliquée dans les procédures de déploiement.  
  
Une fois que vous avez terminé ces étapes de planification, consultez [configuration d’un déploiement multisite](../configure/Configure-a-Multisite-Deployment.md).  
  


