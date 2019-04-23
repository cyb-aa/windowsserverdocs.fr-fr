---
title: Configurer un déploiement multisite
description: Cette rubrique fait partie du guide de déploiement de plusieurs serveurs d’accès distant dans un déploiement Multisite dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cb84920e-7cf5-4266-b071-d09e3d5e1f10
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b602855db271348ac48ee0a5691424a7321c7370
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849750"
---
# <a name="configure-a-multisite-deployment"></a>Configurer un déploiement multisite

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

 Windows Server 2016 combine DirectAccess et accès distant (RAS) VPN dans un seul rôle accès à distance. Cette vue d’ensemble présente les étapes de configuration requises pour déployer un déploiement multisite unique Windows Server 2016 ou d’accès à distance de Windows Server 2012.  
  
-   Étape 1 : [Déployer un serveur DirectAccess unique avec des paramètres avancés](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings). Installer et configurer un serveur d’accès à distance unique. Le déploiement multisite vous oblige à installer un serveur unique avant de configurer un déploiement multisite.  
  
-   [Étape 2 : Configurer l’infrastructure multisite](Step-2-Configure-the-Multisite-Infrastructure.md). Pour un déploiement multisite, vous devez configurer des sites Active Directory supplémentaires et des contrôleurs de domaine. Groupes de sécurité supplémentaires et des objets de stratégie de groupe (GPO) sont également requis si vous n’utilisez pas de stratégie de groupe configurés automatiquement.  
  
-   [Étape 3 : Configurer le déploiement multisite](Step-3-Configure-the-Multisite-Deployment.md)-installer le rôle accès à distance sur d’autres serveurs d’accès à distance, activez le déploiement multisite et configurez les serveurs supplémentaires en tant que points d’entrée pour le déploiement.  
  
-   [Étape 4 : Vérifier le déploiement multisite](Step-4-Verify-the-Multisite-Deployment.md) 
  


