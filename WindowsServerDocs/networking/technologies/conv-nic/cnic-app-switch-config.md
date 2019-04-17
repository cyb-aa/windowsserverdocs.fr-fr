---
title: Configuration du commutateur physique pour les cartes réseau convergé
description: Cette rubrique fait partie de la convergé NIC Configuration Guide pour Windows Server2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 6d53c797-fb67-4b9e-9066-1c9a8b76d2aa
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 98a2e249aea38bd4d07dc1bcbc9b1ca98b98b6d6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="physical-switch-configuration-for-converged-nic"></a>Configuration du commutateur physique pour les cartes réseau convergé

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser les sections suivantes en tant que les instructions pour configurer vos commutateurs physiques.

Il s’agit uniquement les commandes et leurs utilisations; vous devez déterminer les ports auxquels les cartes réseau sont connectées dans votre environnement. 

>[!IMPORTANT]
>Assurez-vous que la politique de non-déplacer et de réseau local virtuel est définie pour la priorité sur laquelle SMB est configuré.

## <a name="arista-switch-dcs-7050s-64-eos-4137m"></a>Commutateur arista \ (dcs\-7050s\-64, EOS\-4.13.7M\)

1.  En \ (passer en mode administrateur, vous demandera généralement un password\)
2.  Configuration \ (pour conclure mode\ configuration)
3.  Afficher exécution \ (affiche configuration\ en cours d’exécution en cours)
4.  Découvrez auquel vos cartes réseau est connectés à des ports de commutateur. Dans cet exemple, ils sont 14/1,15/1,16/1,17/1.
5.  Ed int 14/1,15/1,16/1,17/1 \ (permet d’entrer en mode de configuration pour ces ports\)
6.  Mode dcbx ieee
7.  Mode de contrôle de flux prioritaire sur
8.  Réseau local virtuel natif switchport trunk 225
9.  Switchport trunk autorisé vlan 100-225
10. Jonction de mode switchport
11. Contrôle de flux prioritaire priorité 3 non-déplacer
12. Qualité de service de confiance co
13. Afficher exécution \ (Vérifiez que la configuration est correctement configurée sur le ports\)
14. wR \ (pour rendre les paramètres persiste à travers reboot\ commutateur)

### <a name="tips"></a>Conseils:
1.  Aucune commande # inverse d’une commande
2.  Comment ajouter un nouveau réseau local virtuel: int vlan 100 \ (si le réseau de stockage est sur le réseau local virtuel 100\)
3.  Comment vérifier les réseaux locaux virtuels existants: afficher le réseau local virtuel
4.  Pour plus d’informations sur la configuration du commutateur Arista, recherchez en ligne: Arista fin du support manuel
5.  Cette commande permet de vérifier les paramètres de PFC: afficher les détails des compteurs de contrôle de flux de priorité

## <a name="dell-switch-s4810-ftos-99-00"></a>Commutateur Dell \ (S4810, \(0.0\)\) FTOS 9.9

    
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
    

## <a name="cisco-switch-nexus-3132-version-602u61"></a>Commutateur Cisco \ (Nexus3132, version6.0\(2\)U6\(1\)\)

### <a name="global"></a>Global
    
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
    

## <a name="all-topics-in-this-guide"></a>Toutes les rubriques de ce guide

Ce guide contient les rubriques suivantes.

- [Configuration des cartes réseau convergé avec une seule carte réseau](cnic-single.md)
- [Configuration des cartes réseau associées de cartes réseau convergé](cnic-datacenter.md)
- [Configuration du commutateur physique pour les cartes réseau convergé](cnic-app-switch-config.md)
- [Résolution des problèmes convergent des Configurations de cartes réseau](cnic-app-troubleshoot.md)
