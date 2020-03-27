---
title: Configuration du commutateur physique pour la carte réseau convergée
description: Dans cette rubrique, nous vous fournissons des instructions pour la configuration de vos commutateurs physiques.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 6d53c797-fb67-4b9e-9066-1c9a8b76d2aa
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/14/2018
ms.openlocfilehash: 57fc944461254e78635913ac298bacc26a0789f2
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309616"
---
# <a name="physical-switch-configuration-for-converged-nic"></a>Configuration du commutateur physique pour la carte réseau convergée

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette rubrique, nous vous fournissons des instructions pour la configuration de vos commutateurs physiques. 


Il s’agit uniquement de commandes et de leurs utilisations ; vous devez déterminer les ports auxquels les cartes réseau sont connectées dans votre environnement. 

>[!IMPORTANT]
>Assurez-vous que la stratégie de réseau local virtuel et de non-déplacement est définie pour la priorité sur laquelle SMB est configuré.

## <a name="arista-switch-dcs-7050s-64-eos-4137m"></a>Commutateur Arista \(DC\-7050s\-64, EOS\-4.13.7 M\)

1.  en \(accéder au mode administrateur, demande généralement un mot de passe\)
2.  configuration \(pour passer en mode de configuration\)
3.  afficher l' \(d’exécution affiche la configuration en cours d’exécution\)
4.  Identifiez les ports de commutateur auxquels vos cartes réseau sont connectées. Dans cet exemple, il s’agit de 14/1, 15/1, 16/1, 17/1.
5.  int ETH 14/1, 15/1, 16/1, 17/1 \(entrent en mode de configuration pour ces ports\)
6.  IEEE en mode dcbx
7.  priorité : mode de contrôle de flow sur
8.  VLAN natif switchport trunk 225
9.  réseau local virtuel switchport trunk autorisé 100-225
10. Trunk en mode switchport
11. priorité de priorité-Flow-Control Priority 3 non-Drop
12. Co-approbation QoS
13. afficher l’exécution \(vérifier que la configuration est correctement configurée sur les ports\)
14. WR \(pour que les paramètres persistent au redémarrage du commutateur\)

### <a name="tips"></a>Conseils :
1.  No #command # nie une commande
2.  Ajout d’un nouveau réseau local virtuel : int VLAN 100 \(si le réseau de stockage se trouve sur VLAN 100\)
3.  Comment vérifier les réseaux locaux virtuels existants : afficher le réseau local virtuel
4.  Pour plus d’informations sur la configuration du commutateur Arista, recherchez en ligne : Arista EOS Manual
5.  Utilisez cette commande pour vérifier les paramètres PFC : afficher les détails des compteurs de priorité-Flow-Control

--- 

## <a name="dell-switch-s4810-ftos-99-00"></a>Commutateur Dell \(S4810, FTOS 9,9 \(0,0\)\)

    
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

## <a name="cisco-switch-nexus-3132-version-602u61"></a>Commutateur Cisco \(la version 3132, version 6,0\(2\)U6\(1\)\)

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
    

### <a name="port-specific"></a>Spécifique au port

    
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

- [Configuration de carte réseau convergée avec une seule carte réseau](cnic-single.md)
- [Configuration de carte réseau associée à une carte réseau convergée](cnic-datacenter.md)
- [Résolution des problèmes de configuration de cartes réseau convergées](cnic-app-troubleshoot.md)

--- 