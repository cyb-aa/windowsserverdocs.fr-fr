---
title: Configurer un déploiement multisite
description: Cette rubrique fait partie du guide déployer plusieurs serveurs d’accès à distance dans un déploiement multisite dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: cb84920e-7cf5-4266-b071-d09e3d5e1f10
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 2c2d044c02673d74d7aa8ec076aeac7e2f1aebdb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858382"
---
# <a name="configure-a-multisite-deployment"></a>Configurer un déploiement multisite

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

 Windows Server 2016 associe DirectAccess et le VPN RAS (Remote Access Service) à un rôle d’accès à distance unique. Cette vue d’ensemble fournit une introduction aux étapes de configuration requises pour déployer un déploiement multisite de l’accès à distance Windows Server 2016 ou Windows Server 2012 unique.  
  
-   Étape 1 : [déployer un serveur DirectAccess unique avec des paramètres avancés](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings). Installez et configurez un serveur d’accès à distance unique. Le déploiement multisite nécessite l’installation d’un serveur unique avant la configuration d’un déploiement multisite.  
  
-   [Étape 2 : configurer l’infrastructure multisite](Step-2-Configure-the-Multisite-Infrastructure.md). Dans le cas d’un déploiement multisite, vous devez configurer des sites Active Directory supplémentaires et des contrôleurs de domaine. Des groupes de sécurité et des objets de stratégie de groupe supplémentaires sont également requis si vous n’utilisez pas d’objets de stratégie de groupe configurés automatiquement.  
  
-   [Étape 3 : configurer le déploiement multisite](Step-3-Configure-the-Multisite-Deployment.md)-installez le rôle accès à distance sur des serveurs d’accès à distance supplémentaires, activez le déploiement multisite et configurez les serveurs supplémentaires comme points d’entrée pour le déploiement.  
  
-   [Étape 4 : vérifier le déploiement multisite](Step-4-Verify-the-Multisite-Deployment.md) 
  


