---
title: Résolution des problèmes convergé des Configurations de carte réseau
description: Cette rubrique fait partie de la convergé NIC Configuration Guide pour Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0bc6746f-2adb-43d8-a503-52f473833164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3004ff9d6fe874410c24d174755a6d26f99f8f2a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876480"
---
# <a name="troubleshooting-converged-nic-configurations"></a>Résolution des problèmes convergé des Configurations de carte réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser le script suivant pour vérifier si le RDMA est correctement configuré sur l’ordinateur hôte Hyper-V.

- [Télécharger le script Test-Rdma.ps1](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

Vous pouvez également utiliser les commandes Windows PowerShell suivantes pour dépanner et vérifier la configuration de vos cartes réseau convergé.

## <a name="get-netadapterrdma"></a>Get-NetAdapterRdma

Pour vérifier votre configuration RDMA de carte réseau, exécutez la commande Windows PowerShell suivante sur le serveur Hyper-V.

    
    Get-NetAdapterRdma | fl *
    

Pour identifier et résoudre les problèmes après avoir exécuté cette commande sur l’ordinateur hôte Hyper-V, vous pouvez utiliser suivantes attendu et des résultats inattendus.

### <a name="get-netadapterrdma-expected-results"></a>Résultats de Get-NetAdapterRdma attendu

Carte réseau virtuelle hôte et la carte réseau physique affichent les fonctionnalités RDMA différente de zéro.

![Résultats de PowerShell Windows attendu](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-01.jpg)

### <a name="get-netadapterrdma-unexpected-results"></a>Get-NetAdapterRdma résultats inattendus

Procédez comme suit si vous recevez des résultats inattendus lorsque vous exécutez le **Get-NetAdapterRdma** commande.

1. Assurez-vous que le miniport Mlnx et les pilotes de bus Mlnx sont plus récentes. Pour Mellanox, utilisez drop au moins 42. 
2. Vérifiez que les pilotes miniport et bus Mlnx correspondent en vérifiant la version du pilote par le biais du Gestionnaire de périphériques. Vous trouverez le pilote de bus périphériques système. Le nom doit commencer par Mellanox Connect-X 3 PRO VPI, comme illustré dans la capture d’écran suivante de propriétés de la carte réseau.

![Propriétés des cartes réseau](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-02.jpg)

4. Assurez-vous que le réseau Direct (RDMA) est activé sur la carte réseau physique et la carte réseau virtuelle hôte.
5. Assurez-vous que le commutateur virtuel est créé sur la carte physique à droite en vérifiant ses fonctionnalités RDMA.
6. Vérifiez le journal système de l’Observateur d’événements et les filtrer par source « Hyper-V-VmSwitch ».

--- 

## <a name="get-smbclientnetworkinterface"></a>Get-SmbClientNetworkInterface

Comme une étape supplémentaire pour vérifier votre configuration RDMA, exécutez la commande Windows PowerShell suivante sur le serveur Hyper-V.


    Get-SmbClientNetworkInterface

### <a name="get-smbclientnetworkinterface-expected-results"></a>Résultats de Get-SmbClientNetworkInterface attendu

La carte réseau virtuelle hôte doit apparaître comme prenant en charge RDMA de SMB point de vue également.

![Propriétés des cartes réseau](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-03.jpg)


### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Get-SmbClientNetworkInterface résultats inattendus

1. Assurez-vous que le miniport Mlnx et les pilotes de bus Mlnx sont plus récentes. Pour Mellanox, utilisez drop au moins 42. 
2. Vérifiez que les pilotes miniport et bus Mlnx correspondent en vérifiant la version du pilote par le biais du Gestionnaire de périphériques. Vous trouverez le pilote de bus périphériques système. Le nom doit commencer par Mellanox Connect-X 3 PRO VPI, comme illustré dans la capture d’écran suivante de propriétés de la carte réseau.
3. Assurez-vous que le réseau Direct (RDMA) est activé sur la carte réseau physique et la carte réseau virtuelle hôte.
4. Assurez-vous que le commutateur virtuel Hyper-V est créé sur la carte physique à droite en vérifiant ses fonctionnalités RDMA.
5. Recherchez « Client SMB » les journaux de l’Observateur d’événements **Services et applications | Microsoft | Windows**.

--- 

## <a name="get-netadapterqos"></a>Get-NetAdapterQos

Vous pouvez afficher la qualité de la carte réseau du service \(QoS\) configuration en exécutant la commande Windows PowerShell suivante.

    Get-NetAdapterQos

### <a name="get-netadapterqos-expected-results"></a>Résultats de Get-NetAdapterQos attendu

Priorités et classes de trafic doivent être affichées en fonction de la première étape de configuration que vous avez effectuées à l’aide de ce guide.

![Qualité des priorités de service et des classes](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-04.jpg)

### <a name="get-netadapterqos-unexpected-results"></a>Get-NetAdapterQos résultats inattendus

Si vos résultats sont inattendus, procédez comme suit.

1. Assurez-vous que la carte réseau physique prend en charge Data Center Bridging \(DCB\) et qualité de service
2. Assurez-vous que les pilotes de carte réseau sont à jour.

--- 

## <a name="get-smbmultichannelconnection"></a>Get-SmbMultiChannelConnection

Vous pouvez utiliser la commande Windows PowerShell suivante pour vérifier que les adresse IP du nœud distant est RDMA\-capable.

    Get-SmbMultiChannelConnection


### <a name="get-smbmultichannelconnection-expected-results"></a>Résultats de Get-SmbMultiChannelConnection attendu

Adresse IP du nœud distant est affiché comme RDMA capable.

![Adresse IP de nœud distant compatible RDMA](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-05.jpg)

### <a name="get-smbmultichannelconnection-unexpected-results"></a>Get-SmbMultiChannelConnection résultats inattendus

Si vos résultats sont inattendus, procédez comme suit.

1. Assurez-vous que ping fonctionne les deux sens.
2. Assurez-vous que le pare-feu ne bloque pas l’initialisation de la connexion SMB. Plus précisément, activer la règle de pare-feu pour le port SMB Direct 5445 pour iWARP et 445 pour le ROCE.

--- 

## <a name="get-smbclientnetworkinterface"></a>Get-SmbClientNetworkInterface

Vous pouvez utiliser la commande suivante pour vérifier que la carte réseau virtuelle que vous avez activé pour RDMA est signalée comme RDMA\-capable de protocole SMB.

    Get-SmbClientNetworkInterface


### <a name="get-smbclientnetworkinterface-expected-results"></a>Résultats de Get-SmbClientNetworkInterface attendu

Carte réseau virtuelle qui a été activée pour RDMA doit être considéré comme compatible RDMA par SMB.

![SMB signale que les cartes réseau est prenant en charge RDMA](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-06.jpg)

### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Get-SmbClientNetworkInterface résultats inattendus

Si vos résultats sont inattendus, procédez comme suit.

1. Assurez-vous que ping fonctionne les deux sens.
2. Assurez-vous que le pare-feu ne bloque pas l’initialisation de la connexion SMB.

--- 

## <a name="vstat-mellanox-specific"></a>vstat \(Mellanox spécifiques\)

Si vous utilisez des cartes réseau Mellanox, vous pouvez utiliser la **vstat** commande pour vérifier que le RDMA over Converged Ethernet \(RoCE\) version sur les nœuds Hyper-V.

### <a name="vstat-expected-results"></a>résultats vstat attendu

La version RoCE sur les deux nœuds doit être le même. C’est également un bon moyen pour vérifier que la version du microprogramme sur les deux nœuds est la plus récente.

![Exemples de résultat de vérifications de version RoCE](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-07.jpg)

### <a name="vstat-unexpected-results"></a>vstat résultats inattendus

Si vos résultats sont inattendus, procédez comme suit.

1. Définir la version RoCE correcte à l’aide de Set-MlnxDriverCoreSetting
2. Installez la dernière version du microprogramme à partir du site Web Mellanox.

--- 

## <a name="perfmon-counters"></a>Compteurs PerfMon

Vous pouvez consulter les compteurs dans l’Analyseur de performances pour vérifier l’activité RDMA de votre configuration.

![Exemples de résultat de moniteur de performances](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-08.jpg)

--- 

## <a name="related-topics"></a>Rubriques connexes

- [Configuration de carte réseau convergé avec une seule carte réseau](cnic-single.md)
- [Configuration des cartes réseau de carte réseau convergé](cnic-datacenter.md)
- [Configuration de commutateur physique pour la carte réseau convergé](cnic-app-switch-config.md)

---
