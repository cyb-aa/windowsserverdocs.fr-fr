---
title: Étape 2 planifier les serveurs de Cluster
description: Cette rubrique fait partie du guide de déploiement des accès à distance dans un Cluster dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 673c5bfb-b590-4932-8e54-ca0a466d90cc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0dcb14a03f02f931d59743f1b1b8b24b84ba8351
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282860"
---
# <a name="step-2-plan-cluster-servers"></a>Étape 2 planifier les serveurs de Cluster

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Après avoir déployé un seul serveur d’accès à distance, envisagez d’ajouter des serveurs supplémentaires au cluster.  
  
|Tâche|Description|  
|----|--------|  
|[2.1 l’installation des rôles et fonctionnalités](#BKMK_Install).|Pour chaque serveur qui sera ajouté au cluster, planifier l’installation du rôle accès à distance et la fonctionnalité NLB de Windows (si nécessaire), planifiez la topologie, l’adressage IP, le routage et le transfert.|  
|[2.2 configurer les paramètres du serveur](#BKMK_Config)|Configurer les paramètres pour chaque serveur qui sera ajouté au cluster. Notez que vous pouvez configurer un cluster à charge équilibrée de la charge de serveurs à l’aide de machines virtuelles. Dans l’ordre de routage et de connectivité fonctionne correctement, vous devez configurer les machines virtuelles pour utiliser l’usurpation des adresses MAC.|  
  
## <a name="BKMK_Install"></a>2.1 l’installation des rôles et fonctionnalités  
Pour chaque serveur que vous souhaitez joindre au cluster, envisagez d’installer le rôle accès à distance. En outre prévoyez d’installer la fonctionnalité d’équilibrage de charge réseau (NLB) si vous souhaitez répartir le trafic au cluster avec équilibrage de charge réseau Windows. Pour plus d’informations, consultez [équilibrage de charge réseau](https://technet.microsoft.com/windows-server-docs/networking/technologies/network-load-balancing).  
  
## <a name="BKMK_Config"></a>2.2 configurer les paramètres du serveur  
Pour chaque serveur qui sera ajouté au cluster, planifiez les paramètres adresse IP et domaine. Notez les points suivants :  
  
1.  Serveurs du cluster doivent tous appartenir au même domaine.  
  
2.  Serveurs du cluster doivent se trouver sur le même sous-réseau.  
  
3.  Chaque serveur dans le cluster doit avoir le même nombre de cartes réseau en cours d’utilisation pour le déploiement de DirectAccess.  
  
Lorsque vous chargez équilibrer le cluster à l’aide de Windows NLB les paramètres NLB de Windows suivantes sont appliquées :  
  
1.  Opération en mode monodiffusion. Cela peut être remplacé par multidiffusion à l’aide du Gestionnaire NLB. Ce paramètre ne peut pas être modifié dans la console de gestion de l’accès à distance.  
  
2.  Poids facteur défini comme égal, où tous les serveurs de cluster ont égale de la charge de la charge.  
  
3.  Le trafic du mode de filtrage sera être réparties sur plusieurs hôtes.  
  
4.  Affinité de l’affinité unique est définie.  
  
5.  Les deux protocoles  

