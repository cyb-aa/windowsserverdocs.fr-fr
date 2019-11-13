---
title: Étape 2 Préparez les serveurs de cluster
description: Cette rubrique fait partie du guide déployer l’accès à distance dans un cluster dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 35d68abb-6914-42e0-91e8-803933cf785e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 87983076ee8a7d5546a5ac491ed4ca88153798f0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367411"
---
# <a name="step-2-prepare-cluster-servers"></a>Étape 2 Préparez les serveurs de cluster

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Avant de pouvoir configurer un déploiement de cluster, vous devez préparer des serveurs supplémentaires à ajouter au cluster.  
  
|Tâche|Description|  
|----|--------|  
|[2,1 configurer l’infrastructure d’accès à distance](#BKMK_config)|Sur chaque serveur que vous souhaitez ajouter au cluster, configurez la topologie du serveur, l’adressage IP, le routage et le transfert. Si vous configurez un cluster à charge équilibrée de machines virtuelles, vous devez configurer les machines virtuelles de façon à ce qu’elles utilisent l’usurpation d’adresses MAC.<br /><br />En outre, joignez chaque serveur au même domaine et connectez tous les serveurs au même sous-réseau.|  
|[2,2 installer le rôle accès à distance](#BKMK_Install)|Sur chaque serveur supplémentaire que vous souhaitez ajouter au cluster, installez le rôle accès à distance.|  
|[2,3 installer NLB](#BKMK_NLB)|Sur le serveur d’accès à distance déployé et sur chaque serveur supplémentaire que vous souhaitez ajouter au cluster, installez la fonctionnalité NLB. Notez que cette étape n’est pas requise lors de l’utilisation d’un Load Balancer externe.|  
  
## <a name="BKMK_config"></a>2,1 configurer l’infrastructure d’accès à distance  
Pour configurer un cluster d’accès à distance, vous devez configurer la topologie du serveur, l’adressage IP, le routage et le transfert sur chaque serveur qui fera partie du cluster.  
  
### <a name="to-configure-the-remote-access-infrastructure"></a>Pour configurer l’infrastructure d’accès à distance  
  
1.  Configurez chacun des serveurs qui seront dans le cluster avec la même topologie que le premier serveur d’accès à distance.  
  
2.  Configurez chacun des serveurs qui se trouveront dans le cluster avec l’adressage IP, le routage et le transfert appropriés en fonction de la configuration du premier serveur d’accès à distance. Notez que tous les serveurs du cluster doivent être connectés au même sous-réseau.  
  
3.  Joignez chacun des serveurs qui seront dans le cluster au même domaine que le premier serveur d’accès à distance.  
  
## <a name="BKMK_Install"></a>2,2 installer le rôle accès à distance  
Pour configurer un cluster d’accès à distance, vous devez installer le rôle accès à distance sur chaque serveur qui fera partie du cluster.  
  
### <a name="to-install-the-remote-access-role-on-always-on-vpn-servers"></a>Pour installer le rôle accès à distance sur des serveurs Always On VPN  
  
1.  Sur le serveur DirectAccess, dans la console Gestionnaire de serveur, dans le **tableau de bord**, cliquez sur **Ajouter des rôles et des fonctionnalités**.  
  
2.  Cliquez sur **Suivant** trois fois pour accéder à l’écran de sélection du rôle de serveur.  
  
3.  Dans la boîte de dialogue **Sélectionner des rôles de serveurs** , sélectionnez **accès à distance**, puis cliquez sur **suivant**.  
  
4.  Cliquez sur **suivant** trois fois.  
  
5.  Dans la boîte de dialogue **Sélectionner des services de rôle** , sélectionnez **DirectAccess et VPN (RAS)** , puis cliquez sur **Ajouter des fonctionnalités**.  
  
6.  Sélectionnez **routage**, sélectionnez **proxy d’application Web**, cliquez sur **Ajouter des fonctionnalités**, puis cliquez sur **suivant**.  
  
7. Cliquez sur **Suivant**, puis cliquez sur **Installer**.  
  
8.  Dans la boîte de dialogue **Progression de l’installation** , vérifiez que l’installation s’est correctement déroulée et cliquez sur **Fermer**.  
  
9.  Répétez cette procédure sur tous les serveurs qui doivent être membres du cluster.  
  
## <a name="BKMK_NLB"></a>2,3 installer NLB  
Pour configurer un cluster d’accès à distance, vous devez installer la fonctionnalité d’équilibrage de charge réseau sur chaque serveur qui fera partie du cluster.  
  
> [!NOTE]  
> Cette étape n’est pas requise si un équilibreur de charge externe est utilisé.  
  
#### <a name="to-install-the-nlb-role"></a>Pour installer le rôle NLB  
  
1.  Sur le serveur DirectAccess, dans la console Gestionnaire de serveur, dans le **tableau de bord**, cliquez sur **Ajouter des rôles et des fonctionnalités**.  
  
2.  Cliquez sur **suivant** quatre fois pour accéder à l’écran de sélection des fonctionnalités du serveur.  
  
3.  Dans la boîte de dialogue **Sélectionner des fonctionnalités** , sélectionnez équilibrage de la **charge réseau**, cliquez sur **Ajouter des fonctionnalités**, cliquez sur **suivant**, puis sur **installer**.  
  
4.  Dans la boîte de dialogue **Progression de l’installation** , vérifiez que l’installation s’est correctement déroulée et cliquez sur **Fermer**.  
  
5.  Répétez cette procédure sur tous les serveurs qui doivent être membres du cluster.  
  
## <a name="BKMK_Links"></a>Voir aussi  
  
-   [Étape 3 : configurer un cluster à charge équilibrée](Step-3-Configure-a-Load-Balanced-Cluster.md)  
  


