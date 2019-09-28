---
title: Configurer un déploiement multisite
description: Cette rubrique fait partie du guide déployer plusieurs serveurs d’accès à distance dans un déploiement multisite dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cb84920e-7cf5-4266-b071-d09e3d5e1f10
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 25c0ce5d62268f64113ebc39345b2d50867bebf7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367121"
---
# <a name="configure-a-multisite-deployment"></a>Configurer un déploiement multisite

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

 Windows Server 2016 associe DirectAccess et le VPN RAS (Remote Access Service) à un rôle d’accès à distance unique. Cette vue d’ensemble fournit une introduction aux étapes de configuration requises pour déployer un déploiement multisite de l’accès à distance Windows Server 2016 ou Windows Server 2012 unique.  
  
-   Étape 1 : [Déployez un serveur DirectAccess unique avec des paramètres avancés](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings). Installez et configurez un serveur d’accès à distance unique. Le déploiement multisite nécessite l’installation d’un serveur unique avant la configuration d’un déploiement multisite.  
  
-   [Étape 2 : Configurez l’infrastructure multisite @ no__t-0. Dans le cas d’un déploiement multisite, vous devez configurer des sites Active Directory supplémentaires et des contrôleurs de domaine. Des groupes de sécurité et des objets de stratégie de groupe supplémentaires sont également requis si vous n’utilisez pas d’objets de stratégie de groupe configurés automatiquement.  
  
-   [Étape 3 : Configurer le déploiement multisite @ no__t-0-installer le rôle accès à distance sur des serveurs d’accès à distance supplémentaires, activer le déploiement multisite et configurer les serveurs supplémentaires comme points d’entrée pour le déploiement.  
  
-   [Étape 4 : Vérifier le déploiement multisite @ no__t-0 
  


