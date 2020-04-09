---
title: Étape 2 planifier les serveurs de cluster
description: Cette rubrique fait partie du guide déployer l’accès à distance dans un cluster dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 673c5bfb-b590-4932-8e54-ca0a466d90cc
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 7ac00278e501115d81d80f55c1ceae33a379cc4a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855242"
---
# <a name="step-2-plan-cluster-servers"></a>Étape 2 planifier les serveurs de cluster

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Après le déploiement d’un serveur d’accès à distance unique, envisagez d’ajouter des serveurs supplémentaires au cluster.  
  
|Tâche|Description|  
|----|--------|  
|[2,1 installation de rôles et de fonctionnalités](#BKMK_Install).|Pour chaque serveur qui sera ajouté au cluster, planifiez l’installation du rôle accès à distance et la fonctionnalité NLB Windows (si nécessaire), planifiez la topologie, l’adressage IP, le routage et le transfert.|  
|[2,2 configurer les paramètres du serveur](#BKMK_Config)|Configurez les paramètres de chaque serveur qui sera ajouté au cluster. Notez que vous pouvez configurer un cluster à charge équilibrée de serveurs à l’aide de machines virtuelles. Pour que le routage et la connectivité fonctionnent correctement, vous devez configurer les machines virtuelles de manière à ce qu’elles utilisent l’usurpation d’adresses MAC.|  
  
## <a name="21-installing-roles-and-features"></a><a name="BKMK_Install"></a>2,1 installation de rôles et de fonctionnalités  
Pour chaque serveur que vous souhaitez joindre au cluster, envisagez d’installer le rôle accès à distance. En outre, planifiez l’installation de la fonctionnalité d’équilibrage de charge réseau (NLB) si vous souhaitez équilibrer la charge du trafic vers le cluster à l’aide de Windows NLB. Pour plus d’informations, consultez équilibrage de la [charge réseau](https://technet.microsoft.com/windows-server-docs/networking/technologies/network-load-balancing).  
  
## <a name="22-configure-server-settings"></a><a name="BKMK_Config"></a>2,2 configurer les paramètres du serveur  
Pour chaque serveur qui sera ajouté au cluster, planifiez les paramètres d’adresse IP et de domaine. Notez les points suivants :  
  
1.  Les serveurs du cluster doivent tous appartenir au même domaine.  
  
2.  Les serveurs du cluster doivent se trouver sur le même sous-réseau.  
  
3.  Chaque serveur du cluster doit avoir le même nombre de cartes réseau utilisées pour le déploiement de DirectAccess.  
  
Lorsque vous équilibrez la charge du cluster à l’aide de l’équilibrage de charge réseau Windows, les paramètres NLB Windows suivants sont appliqués :  
  
1.  Mode de fonctionnement-monodiffusion. Vous pouvez le remplacer par multidiffusion à l’aide du gestionnaire NLB. Ce paramètre ne peut pas être modifié dans la console de gestion de l’accès à distance.  
  
2.  Facteur de poids de charge-défini comme égal, où tous les serveurs de cluster ont une charge égale.  
  
3.  Mode de filtrage : le trafic est équilibré en charge sur plusieurs hôtes.  
  
4.  Affinity-l’affinité simple est définie.  
  
5.  Protocoles-les deux  

