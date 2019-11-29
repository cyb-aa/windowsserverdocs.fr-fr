---
title: Déployer Windows Server Update Services
description: Rubrique Windows Server Update Service (WSUS) - Vue d’ensemble du processus de déploiement avec des liens vers les quatre étapes à effectuer
ms.prod: windows-server
ms.reviewer: na
ms.technology: manage-wsus
ms.topic: get-started-article
ms.assetid: 2708f6b2-4252-4b8f-9b7e-84c9b4222075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e3e6bcd5f90d1a7df2a35dda45b4bf8951940815
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361680"
---
# <a name="deploy-windows-server-update-services"></a>Déployer Windows Server Update Services

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

WSUS (Windows Server Update Services) permet aux administrateurs informatiques de déployer les dernières mises à jour de produits Microsoft. WSUS est un rôle de serveur Windows Server qui peut être installé pour gérer et distribuer des mises à jour. Un serveur WSUS peut être la source des mises à jour pour les autres serveurs WSUS de l’organisation. Le serveur WSUS qui agit en tant que source des mises à jour est appelé serveur en amont.  

Dans une implémentation WSUS, au moins un serveur WSUS du réseau doit se connecter à Microsoft Update pour obtenir les informations sur les mises à jour disponibles. Vous pouvez déterminer, en fonction de la configuration et de la sécurité du réseau, le nombre d’autres serveurs se connectant directement à Microsoft Update.  

Ce guide fournit des informations conceptuelles pour la planification et le déploiement de WSUS.  

-   [Planifier votre déploiement WSUS](../plan/plan-your-wsus-deployment.md)  

-   [Étape 1 : installer le rôle serveur WSUS](1-install-the-wsus-server-role.md)  

-   [Étape 2 : configurer WSUS](2-configure-wsus.md)  

-   [Étape 3 : approuver et déployer des mises à jour dans WSUS](3-approve-and-deploy-updates-in-wsus.md)  

-   [Étape 4 : configurer les paramètres de stratégie de groupe pour les mises à jour automatiques](4-configure-group-policy-settings-for-automatic-updates.md)  
