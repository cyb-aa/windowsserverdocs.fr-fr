---
title: Étape 2 préparer les serveurs de Cluster
description: Cette rubrique fait partie du guide de déploiement des accès à distance dans un Cluster dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 35d68abb-6914-42e0-91e8-803933cf785e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 10a40b30acbf022ed34f454d753884cb8c5c97d4
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281208"
---
# <a name="step-2-prepare-cluster-servers"></a>Étape 2 préparer les serveurs de Cluster

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Avant de pouvoir configurer un déploiement de cluster, vous préparez les serveurs supplémentaires à ajouter au cluster.  
  
|Tâche|Description|  
|----|--------|  
|[2.1 configurer l’infrastructure d’accès à distance](#BKMK_config)|Sur chaque serveur que vous souhaitez ajouter au cluster, configurer la topologie du serveur, l’adressage IP, le routage et transfert. Si vous configurez un cluster à charge équilibrée de la charge de machines virtuelles, vous devez configurer les machines virtuelles pour utiliser l’usurpation des adresses MAC.<br /><br />En outre, joindre chaque serveur au même domaine et vous connecter tous les serveurs au même sous-réseau.|  
|[2.2 installer le rôle accès à distance](#BKMK_Install)|Sur chaque serveur supplémentaire que vous souhaitez ajouter au cluster, installez le rôle accès à distance|  
|[2.3 installation de NLB](#BKMK_NLB)|Sur le serveur d’accès à distance déployé et sur chaque serveur supplémentaire que vous souhaitez ajouter au cluster, installez la fonctionnalité d’équilibrage de charge réseau. Notez que cette étape n’est pas requise lors de l’utilisation d’un équilibreur de charge externe.|  
  
## <a name="BKMK_config"></a>2.1 configurer l’infrastructure d’accès à distance  
Pour configurer un cluster de l’accès à distance, vous devez configurer la topologie du serveur, l’adressage IP, le routage et transfert sur chaque serveur qui fera partie du cluster.  
  
### <a name="to-configure-the-remote-access-infrastructure"></a>Pour configurer l’infrastructure d’accès à distance  
  
1.  Configurez chacun des serveurs qui seront dans le cluster avec la même topologie en tant que le premier serveur d’accès à distance.  
  
2.  Configurez chacun des serveurs qui seront dans le cluster avec approprié IP addressing, le routage et le transfert en fonction de la configuration du premier serveur d’accès à distance. Notez que tous les serveurs du cluster doivent être connectés au même sous-réseau.  
  
3.  Joignez tous les serveurs qui seront dans le cluster pour le même domaine que le premier serveur d’accès à distance.  
  
## <a name="BKMK_Install"></a>2.2 installer le rôle accès à distance  
Pour configurer un cluster de l’accès à distance, vous devez installer le rôle accès à distance sur chaque serveur qui forme une partie du cluster.  
  
### <a name="to-install-the-remote-access-role-on-always-on-vpn-servers"></a>Pour installer le rôle accès à distance sur les serveurs VPN Always On  
  
1.  Sur le serveur DirectAccess, dans la console Gestionnaire de serveur, dans le **tableau de bord**, cliquez sur **ajouter des rôles et fonctionnalités**.  
  
2.  Cliquez sur **Suivant** trois fois pour accéder à l’écran de sélection du rôle de serveur.  
  
3.  Sur le **sélectionner des rôles de serveur** boîte de dialogue, sélectionnez **accès à distance**, puis cliquez sur **suivant**.  
  
4.  Cliquez sur **suivant** trois fois.  
  
5.  Sur le **sélectionnez services de rôle** boîte de dialogue, sélectionnez **DirectAccess et VPN (RAS)** puis cliquez sur **ajouter des fonctionnalités**.  
  
6.  Sélectionnez **routage**, sélectionnez **Proxy d’Application Web**, cliquez sur **ajouter des fonctionnalités**, puis cliquez sur **suivant**.  
  
7. Cliquez sur **Suivant**, puis cliquez sur **Installer**.  
  
8.  Dans la boîte de dialogue **Progression de l’installation** , vérifiez que l’installation s’est correctement déroulée et cliquez sur **Fermer**.  
  
9.  Répétez cette procédure sur tous les serveurs que vous souhaitez être membres du cluster.  
  
## <a name="BKMK_NLB"></a>2.3 installation de NLB  
Pour configurer un cluster de l’accès à distance, vous devez installer la fonctionnalité d’équilibrage de charge réseau sur chaque serveur qui forme une partie du cluster.  
  
> [!NOTE]  
> Cette étape n’est pas requise si un équilibreur de charge externe est utilisé.  
  
#### <a name="to-install-the-nlb-role"></a>Pour installer le rôle de l’équilibrage de charge réseau  
  
1.  Sur le serveur DirectAccess, dans la console Gestionnaire de serveur, dans le **tableau de bord**, cliquez sur **ajouter des rôles et fonctionnalités**.  
  
2.  Cliquez sur **suivant** quatre fois pour accéder à l’écran de sélection de fonctionnalité de serveur.  
  
3.  Sur le **sélectionner des fonctionnalités** boîte de dialogue, sélectionnez **équilibrage de charge réseau**, cliquez sur **ajouter des fonctionnalités**, cliquez sur **suivant**, puis cliquez sur **Installer**.  
  
4.  Dans la boîte de dialogue **Progression de l’installation** , vérifiez que l’installation s’est correctement déroulée et cliquez sur **Fermer**.  
  
5.  Répétez cette procédure sur tous les serveurs que vous souhaitez être membres du cluster.  
  
## <a name="BKMK_Links"></a>Voir aussi  
  
-   [Étape 3 : Configurer un cluster d’équilibrage de charge](Step-3-Configure-a-Load-Balanced-Cluster.md)  
  


