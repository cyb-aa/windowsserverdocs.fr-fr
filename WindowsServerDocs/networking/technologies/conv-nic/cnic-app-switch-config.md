---
title: Configuration de commutateur physique pour la carte d’interface réseau convergé
description: Dans cette rubrique, nous fournissons contenant des instructions pour configurer vos commutateurs physiques.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 6d53c797-fb67-4b9e-9066-1c9a8b76d2aa
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/14/2018
ms.openlocfilehash: e31d7b83fee84d9055d938f77b49389205786244
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829400"
---
# <a name="physical-switch-configuration-for-converged-nic"></a>Configuration de commutateur physique pour la carte d’interface réseau convergé

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette rubrique, nous fournissons contenant des instructions pour configurer vos commutateurs physiques. 


Il s’agit uniquement les commandes et leurs utilisations ; Vous devez déterminer les ports auxquels les cartes réseau sont connectées dans votre environnement. 

>[!IMPORTANT]
>Assurez-vous que le réseaux locaux virtuels et la stratégie non-drop est définie pour la priorité sur lequel SMB est configuré.

## <a name="arista-switch-dcs-7050s-64-eos-4137m"></a>Commutateur de arista \(contrôleurs de domaine\-7050s\-EOS 64,\-4.13.7M\)

1.  fr \(accédez au mode d’administration, généralement vous demande un mot de passe\)
2.  config \(pour entrer en mode de configuration\)
3.  Afficher exécution \(montre la configuration en cours d’exécution actuelle\)
4.  Découvrez auquel vos cartes réseau est connectées à des ports de commutateur. Dans cet exemple, ils sont 14/1,15/1,16/1,17/1.
5.  int eth 14/1,15/1,16/1,17/1 \(entrer en mode de configuration pour ces ports\)
6.  mode de dcbx ieee
7.  mode de contrôle de flux prioritaire sur
8.  switchport trunk vlan natif 225
9.  switchport trunk autorisé vlan 100-225
10. en mode trunk du mode switchport
11. priorité de contrôle de flux de priorité 3 non-déplacer
12. qualité de service de confiance cos
13. Afficher exécution \(vérifier que la configuration est configurée correctement sur les ports\)
14. wR \(pour rendre les paramètres sont conservées dans le commutateur de redémarrage\)

### <a name="tips"></a>Astuces :
1.  Aucune commande # # annule une commande
2.  Comment ajouter un nouveau réseau local virtuel : int vlan 100 \(si le réseau de stockage se trouve sur un réseau local virtuel 100\)
3.  Comment vérifier les réseaux locaux virtuels existants : afficher les vlan
4.  Pour plus d’informations sur la configuration de commutateur Arista, recherchez en ligne : Arista EOS manuel
5.  Utilisez cette commande pour vérifier les paramètres PFC : afficher les détails des compteurs de contrôle de flux de priorité

--- 

## <a name="dell-switch-s4810-ftos-99-00"></a>Commutateur Dell \(S4810, FTOS 9.9 \(0.0\)\)

    
    !
    dcb enable
    ! put pfc control on qos class 3
    configure
    dcb-map dcb-smb
    priority group 0 bandwidth 90 pfc on
    priority group 1 bandwidth 10 pfc off
    priority-pgid 1 1 1 0 1 1 1 1
    exit
    ! apply map to ports 0-31
    configure
    interface range ten 0/0-31
    dcb-map dcb-smb
    exit
    
--- 

## <a name="cisco-switch-nexus-3132-version-602u61"></a>Commutateur Cisco \(Nexus 3132, version 6.0\(2\)U6\(1\)\)

### <a name="global"></a>Globale
    
    class-map type qos match-all RDMA
    match cos 3
    class-map type queuing RDMA
    match qos-group 3
    policy-map type qos QOS_MARKING
    class RDMA
    set qos-group 3
    class class-default
    policy-map type queuing QOS_QUEUEING
    class type queuing RDMA
    bandwidth percent 50
    class type queuing class-default
    bandwidth percent 50
    class-map type network-qos RDMA
    match qos-group 3
    policy-map type network-qos QOS_NETWORK
    class type network-qos RDMA
    mtu 2240
    pause no-drop
    class type network-qos class-default
    mtu 9216
    system qos
    service-policy type qos input QOS_MARKING
    service-policy type queuing output QOS_QUEUEING
    service-policy type network-qos QOS_NETWORK
    

### <a name="port-specific"></a>Port spécifique

    
    switchport mode trunk
    switchport trunk native vlan 99
    switchport trunk allowed vlan 99,2000,2050   çuse VLANs that already exists
    spanning-tree port type edge
    flowcontrol receive on (not supported with PFC in Cisco NX-OS)
    flowcontrol send on (not supported with PFC in Cisco NX-OS)
    no shutdown
    priority-flow-control mode on
    
--- 

## <a name="related-topics"></a>Rubriques connexes

- [Configuration de carte réseau convergé avec une seule carte réseau](cnic-single.md)
- [Configuration des cartes réseau de carte réseau convergé](cnic-datacenter.md)
- [Résolution des problèmes convergé des Configurations de carte réseau](cnic-app-troubleshoot.md)

--- 