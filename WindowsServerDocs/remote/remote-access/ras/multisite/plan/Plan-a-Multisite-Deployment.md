---
title: Planifier un déploiement multisite
description: Cette rubrique fait partie du guide déployer plusieurs serveurs d’accès à distance dans un déploiement multisite dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8387eabe-7363-4367-b5b1-03c67baa2933
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d5c189969c31ef9627dd7daee0e09162ea9aec39
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313875"
---
# <a name="plan-a-multisite-deployment"></a>Planifier un déploiement multisite

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

 Windows Server 2016, Windows Server 2012 combinez DirectAccess et le VPN RRAS (Routing and Remote Access Service) en un seul rôle d’accès à distance. Cette vue d’ensemble fournit une introduction aux étapes de planification requises pour déployer l’accès à distance Windows Server 2016 ou Windows Server 2012 dans une configuration multisite.  
  
1.  [Déployez un serveur DirectAccess unique avec des paramètres avancés](https://technet.microsoft.com/library/hh831436(v=ws.11).aspx). Cette étape comprend la planification de l’infrastructure requise pour déployer un serveur unique. Il comprend la planification des paramètres réseau et serveur, les certificats requis, les paramètres DNS, le déploiement du serveur emplacement réseau, les serveurs d’administration DirectAccess, les paramètres de Active Directory et les objets stratégie de groupe (GPO).  
  
2.  [Étape 2 planifier l’infrastructure multisite](Step-2-Plan-the-Multisite-Infrastructure.md). Cette étape comprend la planification des Active Directory et des objets de stratégie de groupe et la configuration DNS.  
  
3.  [Étape 3 : planifier le déploiement multisite](Step-3-Plan-the-Multisite-Deployment.md). Cette étape comprend la planification des paramètres de certificat, la configuration du serveur d’emplacement réseau, les paramètres de point d’entrée du client, les paramètres de préfixe IPv6 et éventuellement les paramètres d’équilibrage de charge globaux.  
  
> [!NOTE]  
> Consignez vos décisions de planification pour le déploiement avancé de l’accès à distance. Cet enregistrement peut servir d'aide pour toute personne impliquée dans les procédures de déploiement.  
  
Une fois ces étapes de planification terminées, consultez [configuration d’un déploiement multisite](../configure/Configure-a-Multisite-Deployment.md).  
  


