---
title: Résolution des problèmes de configuration de cartes réseau convergées
description: Cette rubrique fait partie du Guide de configuration de la carte réseau convergée pour Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0bc6746f-2adb-43d8-a503-52f473833164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 297044397088bfb64b51e1553d3f69d5b933e81b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405913"
---
# <a name="troubleshooting-converged-nic-configurations"></a>Résolution des problèmes de configuration de cartes réseau convergées

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser le script suivant pour vérifier si la configuration RDMA est correcte sur l’hôte Hyper-V.

- [Télécharger le script Test-Rdma. ps1](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

Vous pouvez également utiliser les commandes Windows PowerShell suivantes pour dépanner et vérifier la configuration de vos cartes réseau convergées.

## <a name="get-netadapterrdma"></a>NetAdapterRdma

Pour vérifier la configuration RDMA de votre carte réseau, exécutez la commande Windows PowerShell suivante sur le serveur Hyper-V.

    
    Get-NetAdapterRdma | fl *
    

Vous pouvez utiliser les résultats attendus et inattendus suivants pour identifier et résoudre les problèmes après avoir exécuté cette commande sur l’hôte Hyper-V.

### <a name="get-netadapterrdma-expected-results"></a>Résultats attendus de NetAdapterRdma

Les carte réseau virtuelle hôtes et la carte réseau physique affichent des fonctionnalités RDMA non nulles.

![Résultats attendus pour Windows PowerShell](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-01.jpg)

### <a name="get-netadapterrdma-unexpected-results"></a>Obtenir des résultats inattendus NetAdapterRdma

Si vous obtenez des résultats inattendus lorsque vous exécutez la commande **NetAdapterRdma,** procédez comme suit.

1. Assurez-vous que le miniport Mlnx et les pilotes de bus Mlnx sont les derniers. Pour le Mellanox, utilisez au moins Drop 42. 
2. Vérifiez que le miniport Mlnx et les pilotes de bus correspondent en vérifiant la version du pilote par le biais de Device Manager. Le pilote de bus se trouve dans périphériques système. Le nom doit commencer par Mellanox Connect-X 3 PRO VPI, comme illustré dans la capture d’écran suivante des propriétés de la carte réseau.

![Propriétés des cartes réseau](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-02.jpg)

4. Assurez-vous que Network direct (RDMA) est activé sur la carte réseau physique et les carte réseau virtuelle hôtes.
5. Assurez-vous que vSwitch est créé sur la carte physique appropriée en vérifiant ses capacités RDMA.
6. Vérifiez le journal système observateur et filtrez par source « Hyper-V-VmSwitch ».

--- 

## <a name="get-smbclientnetworkinterface"></a>SmbClientNetworkInterface

En guise d’étape supplémentaire pour vérifier votre configuration RDMA, exécutez la commande Windows PowerShell suivante sur le serveur Hyper-V.


    Get-SmbClientNetworkInterface

### <a name="get-smbclientnetworkinterface-expected-results"></a>Résultats attendus de SmbClientNetworkInterface

Le carte réseau virtuelle d’hôte doit apparaître comme étant en mesure d’atteindre RDMA du point de vue de SMB.

![Propriétés des cartes réseau](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-03.jpg)


### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Obtenir des résultats inattendus SmbClientNetworkInterface

1. Assurez-vous que le miniport Mlnx et les pilotes de bus Mlnx sont les derniers. Pour le Mellanox, utilisez au moins Drop 42. 
2. Vérifiez que le miniport Mlnx et les pilotes de bus correspondent en vérifiant la version du pilote par le biais de Device Manager. Le pilote de bus se trouve dans périphériques système. Le nom doit commencer par Mellanox Connect-X 3 PRO VPI, comme illustré dans la capture d’écran suivante des propriétés de la carte réseau.
3. Assurez-vous que Network direct (RDMA) est activé sur la carte réseau physique et les carte réseau virtuelle hôtes.
4. Assurez-vous que le commutateur virtuel Hyper-V est créé sur la carte physique appropriée en vérifiant ses capacités RDMA.
5. Vérifier les journaux observateur pour « client SMB » dans l' **application et les services | Microsoft | Windows**.

--- 

## <a name="get-netadapterqos"></a>NetAdapterQos

Vous pouvez afficher la configuration QoS \(\) de la qualité de service de la carte réseau en exécutant la commande Windows PowerShell suivante.

    Get-NetAdapterQos

### <a name="get-netadapterqos-expected-results"></a>Résultats attendus de NetAdapterQos

Les priorités et les classes de trafic doivent être affichées selon la première étape de configuration que vous avez effectuée à l’aide de ce guide.

![Qualité des priorités et des classes de service](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-04.jpg)

### <a name="get-netadapterqos-unexpected-results"></a>Obtenir des résultats inattendus NetAdapterQos

Si vos résultats sont inattendus, procédez comme suit.

1. S’assurer que la carte réseau physique prend en charge \(le\) Data Center Bridging DCB et QoS
2. Vérifiez que les pilotes de carte réseau sont à jour.

--- 

## <a name="get-smbmultichannelconnection"></a>SmbMultiChannelConnection

Vous pouvez utiliser la commande Windows PowerShell suivante pour vérifier que l’adresse IP du nœud distant est capable\-d’utiliser RDMA.

    Get-SmbMultiChannelConnection


### <a name="get-smbmultichannelconnection-expected-results"></a>Résultats attendus de SmbMultiChannelConnection

L’adresse IP du nœud distant est indiquée comme étant en mesure d’atteindre RDMA.

![Adresse IP du nœud distant acceptant RDMA](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-05.jpg)

### <a name="get-smbmultichannelconnection-unexpected-results"></a>Obtenir des résultats inattendus SmbMultiChannelConnection

Si vos résultats sont inattendus, procédez comme suit.

1. Assurez-vous que le ping fonctionne de deux manières.
2. Assurez-vous que le pare-feu ne bloque pas le lancement de la connexion SMB. En particulier, activez la règle de pare-feu pour le port SMB direct 5445 pour iWARP et 445 pour ROCE.

--- 

## <a name="get-smbclientnetworkinterface"></a>SmbClientNetworkInterface

Vous pouvez utiliser la commande suivante pour vérifier que la carte réseau virtuelle que vous avez activée pour RDMA est\-indiquée comme étant compatible RDMA par SMB.

    Get-SmbClientNetworkInterface


### <a name="get-smbclientnetworkinterface-expected-results"></a>Résultats attendus de SmbClientNetworkInterface

La carte réseau virtuelle qui a été activée pour RDMA doit être considérée comme compatible RDMA par SMB.

![Rapports SMB prenant en charge RDMA par les cartes réseau](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-06.jpg)

### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Obtenir des résultats inattendus SmbClientNetworkInterface

Si vos résultats sont inattendus, procédez comme suit.

1. Assurez-vous que le ping fonctionne de deux manières.
2. Assurez-vous que le pare-feu ne bloque pas le lancement de la connexion SMB.

--- 

## <a name="vstat-mellanox-specific"></a>vstat \(-spécifique à Mellanox\)

Si vous utilisez des cartes réseau Mellanox, vous pouvez utiliser la commande **vstat** pour vérifier la version RDMA sur la RoCE \(\) Ethernet convergée sur les nœuds Hyper-V.

### <a name="vstat-expected-results"></a>résultats attendus vstat

La version de RoCE sur les deux nœuds doit être la même. C’est également un bon moyen de vérifier que la version du microprogramme sur les deux nœuds est la dernière.

![Exemples de résultats de la vérification de version RoCE](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-07.jpg)

### <a name="vstat-unexpected-results"></a>vstat des résultats inattendus

Si vos résultats sont inattendus, procédez comme suit.

1. Définir la version correcte de RoCE à l’aide de Set-MlnxDriverCoreSetting
2. Installez le microprogramme le plus récent à partir du site Web Mellanox.

--- 

## <a name="perfmon-counters"></a>Compteurs Perfmon

Vous pouvez consulter les compteurs de l’analyseur de performances pour vérifier l’activité RDMA de votre configuration.

![Exemples de résultats de l’analyseur de performances](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-08.jpg)

--- 

## <a name="related-topics"></a>Rubriques connexes

- [Configuration de carte réseau convergée avec une seule carte réseau](cnic-single.md)
- [Configuration de carte réseau associée à une carte réseau convergée](cnic-datacenter.md)
- [Configuration du commutateur physique pour la carte réseau convergée](cnic-app-switch-config.md)

---
