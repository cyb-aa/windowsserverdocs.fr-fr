---
title: Étape 4 vérifier le cluster
description: Cette rubrique fait partie du guide déployer l’accès à distance dans un cluster dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: f22dcf10-b453-4664-a9ef-e40e95c72f63
ms.author: lizross
author: eross-msft
ms.openlocfilehash: c31db158856eb3c3881a36c29e40d1227116201f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855262"
---
# <a name="step-4-verify-the-cluster"></a>Étape 4 vérifier le cluster

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit comment vérifier que vous avez correctement configuré votre déploiement de cluster DirectAccess.  
  
### <a name="to-verify-access-to-internal-resources-through-the-cluster"></a>Pour vérifier l’accès aux ressources internes par le biais du cluster  
  
1.  Connectez un ordinateur client DirectAccess au réseau d’entreprise et obtenez la stratégie de groupe.  
  
2.  Connectez l’ordinateur client au réseau externe et tentez d’accéder aux ressources internes.  
  
    Vous devriez pouvoir accéder à toutes les ressources d’entreprise.  
  
3.  Testez la connectivité via chaque serveur du cluster en désactivant ou en déconnectant le réseau externe, à l’exception de l’un des serveurs de cluster. Sur l’ordinateur client, tentez d’accéder aux ressources de l’entreprise. Répétez le test sur un autre serveur de cluster.  
  
    Vous devez être en mesure d’accéder à toutes les ressources d’entreprise par le biais de chaque serveur de cluster.  
  


