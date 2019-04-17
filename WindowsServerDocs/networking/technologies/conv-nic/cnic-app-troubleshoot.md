---
title: Résolution des problèmes convergent des Configurations de cartes réseau
description: Cette rubrique fait partie de la convergé NIC Configuration Guide pour Windows Server2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0bc6746f-2adb-43d8-a503-52f473833164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 373ecd9b9fff62aaabd8caa176ff091ec98ad81c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="troubleshooting-converged-nic-configurations"></a>Résolution des problèmes convergent des Configurations de cartes réseau

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser le script suivant pour vérifier si la configuration RDMA est correcte sur l’ordinateur hôte Hyper-V.

- [Télécharger le script Test-Rdma .ps1](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

Vous pouvez également utiliser les commandes Windows PowerShell suivantes pour dépanner et vérifier la configuration de vos cartes réseau convergé.

## <a name="get-netadapterrdma"></a>Get-NetAdapterRdma

Pour vérifier votre configuration RDMA des cartes réseau, exécutez la commande Windows PowerShell suivante sur le serveur Hyper-V.

    
    Get-NetAdapterRdma | fl *
    

Pour identifier et résoudre les problèmes après l’exécution de cette commande sur l’ordinateur hôte Hyper-V, vous pouvez utiliser suivantes prévu et des résultats inattendus.

### <a name="get-netadapterrdma-expected-results"></a>Get-NetAdapterRdma les résultats attendus

Carte réseau virtuelle de l’ordinateur hôte et de la carte réseau physique affichent les fonctions RDMA différente de zéro.

![Résultats de Windows PowerShell prévu](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-01.jpg)

### <a name="get-netadapterrdma-unexpected-results"></a>Get-NetAdapterRdma des résultats inattendus

Procédez comme suit si vous recevez des résultats inattendus lorsque vous exécutez le **Get-NetAdapterRdma** commande.

1. Assurez-vous que les pilotes de bus Mlnx Mlnx miniport sont plus récentes. Pour Mellanox, utilisez drop au moins 42. 
2. Vérifiez que les pilotes de miniport et bus Mlnx correspondent en vérifiant la version du pilote par le biais du Gestionnaire de périphériques. Vous trouverez le pilote de bus périphériques système. Le nom doit commencer par Mellanox Connect-X 3 PRO VPI, comme illustré dans la capture d’écran suivante de propriétés de la carte réseau.

![Propriétés de la carte réseau](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-02.jpg)

4. Assurez-vous que le réseau Direct (RDMA) est activé sur la carte réseau physique et les cartes réseau virtuelles hôtes.
5. Assurez-vous que le commutateur virtuel est créé sur la carte physique droite en vérifiant ses fonctions RDMA.
6. Vérifiez le journal système EventViewer et filtrer par source «Hyper-V-VmSwitch».

## <a name="get-smbclientnetworkinterface"></a>Obtenir SmbClientNetworkInterface

Comme une étape supplémentaire pour vérifier votre configuration RDMA, exécutez la commande Windows PowerShell suivante sur le serveur Hyper-V.


    Get SmbClientNetworkInterface

### <a name="get-smbclientnetworkinterface-expected-results"></a>Obtenir des résultats SmbClientNetworkInterface prévu

La carte réseau virtuelle hôte doit apparaître comme compatible RDMA à partir du point de vue de SMB également.

![Propriétés de la carte réseau](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-03.jpg)


### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Obtenir SmbClientNetworkInterface des résultats inattendus

1. Assurez-vous que les pilotes de bus Mlnx Mlnx miniport sont plus récentes. Pour Mellanox, utilisez drop au moins 42. 
2. Vérifiez que les pilotes de miniport et bus Mlnx correspondent en vérifiant la version du pilote par le biais du Gestionnaire de périphériques. Vous trouverez le pilote de bus périphériques système. Le nom doit commencer par Mellanox Connect-X 3 PRO VPI, comme illustré dans la capture d’écran suivante de propriétés de la carte réseau.
3. Assurez-vous que le réseau Direct (RDMA) est activé sur la carte réseau physique et les cartes réseau virtuelles hôtes.
4. Assurez-vous que le commutateur virtuel Hyper-V est créé sur la carte physique droite en vérifiant ses fonctions RDMA.
5. Recherchez «Client SMB» les journaux EventViewer **Services et applications | Microsoft | Windows**.

## <a name="get-netadapterqos"></a>Get-NetAdapterQos

Vous pouvez afficher la qualité de la carte réseau de configuration du service \(QoS\) en exécutant la commande Windows PowerShell suivante.

    Get-NetAdapterQos

### <a name="get-netadapterqos-expected-results"></a>Get-NetAdapterQos les résultats attendus

Priorités et des classes de trafic doivent s’afficher en fonction de la première étape de configuration que vous avez effectuées à l’aide de ce guide.

![Qualité des priorités de service et des classes](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-04.jpg)

### <a name="get-netadapterqos-unexpected-results"></a>Get-NetAdapterQos des résultats inattendus

Si vos résultats sont inattendus, procédez comme suit.

1. Vérifiez que la carte réseau physique prend en charge \(DCB\) Data Center Bridging et qualité de service
2. Assurez-vous que les pilotes de carte réseau sont à jour.


## <a name="get-smbmultichannelconnection"></a>Get-SmbMultiChannelConnection

Vous pouvez utiliser la commande Windows PowerShell suivante pour vérifier que l’adresse du nœud distant est compatible RDMA\.

    Get-SmbMultiChannelConnection


### <a name="get-smbmultichannelconnection-expected-results"></a>Get-SmbMultiChannelConnection les résultats attendus

Adresse IP du nœud distant s’affiche comme RDMA compatible.

![Adresse IP de nœud distant compatibles RDMA](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-05.jpg)

### <a name="get-smbmultichannelconnection-unexpected-results"></a>Get-SmbMultiChannelConnection des résultats inattendus

Si vos résultats sont inattendus, procédez comme suit.

1. Assurez-vous que la commande ping fonctionne de deux façons.
2. Assurez-vous que le pare-feu ne bloque pas l’initialisation de la connexion SMB. Plus spécifiquement, activer la règle de pare-feu pour le port SMB Direct5445 pour iWARP et 445 pour le ROCE.

## <a name="get-smbclientnetworkinterface"></a>Get-SmbClientNetworkInterface

Vous pouvez utiliser la commande suivante pour vérifier que la carte réseau virtuelle que vous avez activé pour RDMA est signalée comme étant capable de RDMA\ par SMB.

    Get-SmbClientNetworkInterface


### <a name="get-smbclientnetworkinterface-expected-results"></a>Get-SmbClientNetworkInterface les résultats attendus

Carte réseau virtuelle qui a été activé pour RDMA doit être considérée comme compatible RDMA par SMB.

![SMB signale que les cartes réseau sont compatible RDMA](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-06.jpg)

### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Get-SmbClientNetworkInterface des résultats inattendus

Si vos résultats sont inattendus, procédez comme suit.

1. Assurez-vous que la commande ping fonctionne de deux façons.
2. Assurez-vous que le pare-feu ne bloque pas l’initialisation de la connexion SMB.

## <a name="vstat-mellanox-specific"></a>Vstat \(Mellanox specific\)

Si vous utilisez des cartes réseau Mellanox, vous pouvez utiliser le **vstat** pour vérifier le RDMA sur la version de \(RoCE\) Converged Ethernet sur les nœuds Hyper-V.

### <a name="vstat-expected-results"></a>Résultats vstat prévu

La version RoCE sur les deux nœuds doit être le même. Il s’agit également d’un moyen efficace pour vérifier que la version du microprogramme sur les deux nœuds est plus récente.

![Exemples de résultat de vérification de version RoCE](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-07.jpg)

### <a name="vstat-unexpected-results"></a>Vstat des résultats inattendus

Si vos résultats sont inattendus, procédez comme suit.

1. Définir la version RoCE correcte à l’aide de Set-MlnxDriverCoreSetting
2. Installer le microprogramme le plus récent à partir du site Web Mellanox.


## <a name="perfmon-counters"></a>Compteurs de performance

Vous pouvez consulter les compteurs dans l’Analyseur de performances pour vérifier l’activité RDMA de votre configuration.

![Exemples de résultat de moniteur de performances](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-08.jpg)

## <a name="all-topics-in-this-guide"></a>Toutes les rubriques de ce guide

Ce guide contient les rubriques suivantes.

- [Configuration des cartes réseau convergé avec une seule carte réseau](cnic-single.md)
- [Configuration des cartes réseau associées de cartes réseau convergé](cnic-datacenter.md)
- [Configuration du commutateur physique pour les cartes réseau convergé](cnic-app-switch-config.md)
- [Résolution des problèmes convergent des Configurations de cartes réseau](cnic-app-troubleshoot.md)
