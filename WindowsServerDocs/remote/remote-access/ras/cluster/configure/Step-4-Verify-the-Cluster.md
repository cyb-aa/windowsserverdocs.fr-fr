---
title: Étape 4 vérifier le Cluster
description: Cette rubrique fait partie du guide de déploiement des accès à distance dans un Cluster dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f22dcf10-b453-4664-a9ef-e40e95c72f63
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 424c4f881c168ea691dd51cd2d86a4a234c41075
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282990"
---
# <a name="step-4-verify-the-cluster"></a>Étape 4 vérifier le Cluster

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit comment vérifier que vous avez correctement configuré votre déploiement de cluster DirectAccess.  
  
### <a name="to-verify-access-to-internal-resources-through-the-cluster"></a>Pour vérifier l’accès aux ressources internes via le cluster  
  
1.  Connectez un ordinateur client DirectAccess au réseau d’entreprise et obtenez la stratégie de groupe.  
  
2.  Connectez l’ordinateur client au réseau externe et tentez d’accéder aux ressources internes.  
  
    Vous devriez pouvoir accéder à toutes les ressources d’entreprise.  
  
3.  Tester la connectivité via chaque serveur du cluster par la désactivation ou la déconnexion du réseau externe, une seule des serveurs en cluster. Sur l’ordinateur client, tentez d’accéder aux ressources d’entreprise. Répéter le test sur un serveur de cluster différent.  
  
    Doit pouvoir accéder à toutes les ressources d’entreprise via chaque serveur en cluster.  
  


