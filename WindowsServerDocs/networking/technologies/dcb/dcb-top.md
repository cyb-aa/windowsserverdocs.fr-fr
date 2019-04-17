---
title: Data Center Bridging (DCB)
description: Vous pouvez utiliser cette rubrique pour obtenir des informations à propos de Data Center Bridging dans Windows Server2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: da58f312-bd3b-4bb6-98ca-6177869dd6ad
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3d465e855adc387d7136919ac11fbab56c792c34
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="data-center-bridging-dcb"></a>Data Center Bridging \(DCB\)

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour obtenir des informations sur les \(DCB\) Data Center Bridging.

DCB est une suite de normes \(IEEE\) Institute of Electrical and Electronics Engineers qui active Converged Fabrics dans le centre de données où le stockage, mise en réseau des données, le cluster \(IPC\) Inter\-Process Communication et le trafic de gestion partagent l’infrastructure du réseau Ethernet.

>[!NOTE]
>Outre cette rubrique, la documentation de DCB suivante est disponible
>
>- [Installer DCB dans Windows Server2016 ou Windows10](dcb-install.md)
>- [Gérer Data Center Bridging (DCB)](dcb-manage.md)

DCB permet l’allocation de bande passante basée sur hardware\ à un type spécifique de trafic réseau et améliore la fiabilité du transport Ethernet avec l’utilisation du contrôle de flux basé sur priority\.

L’allocation de bande passante basée sur Hardware\ est essentielle si le trafic contourne le système d’exploitation et être déchargé sur une carte réseau convergé, ce qui peut prendre en charge Internet Small Computer System Interface \(iSCSI\), Remote Direct Memory Access \(RDMA\) sur Ethernet ou Fibre Channel sur Ethernet \(FCoE\).

Contrôle de flux basé sur Priority\ est essentiel si le protocole de couche supérieure, tel que Fiber Channel, considère un transport sous-jacent sans perte.

## <a name="dcb-protocols-and-management-options"></a>Protocoles DCB et les Options de gestion

DCB se compose de l’ensemble des protocoles. 

- Améliorée Transmission Service \(ETS\) – IEEE 802.1Qaz, qui repose sur la 802 .1p et de 802. 1 q normes
- Contrôler les flux de priorité \(PFS\), IEEE 802.1qbb 
- DCB Exchange Protocol \(DCBX\), IEEE 802.1AB, comme l’étendue dans le 802.1Qaz standard.

Le protocole DCBX vous autorise à configurer DCB sur un commutateur peut ensuite les configurer automatiquement un périphérique de fin, comme un ordinateur exécutant Windows Server2016.

Si vous préférez gérer DCB à partir d’un commutateur, vous n’avez pas besoin d’installer DCB en tant que fonctionnalité dans Windows Server2016, mais cette approche inclut certaines limitations.

Étant donné que DCBX peut indiquer seulement la carte réseau hôte des tailles de classe ETS et l’activation PFC, toutefois, les ordinateurs hôtes Windows Server nécessitent généralement que que DCB est installé afin que le trafic est mappé aux classes ETS.

Applications Windows ne sont généralement pas conçues pour participer aux échanges DCBX. Pour cette raison, l’hôte doit être configuré séparément à partir de commutateurs de réseau, mais avec des paramètres identiques.

Si vous choisissez Gérer DCB à partir d’un commutateur, vous pouvez toujours afficher les configurations dans Windows Server2016 à l’aide de commandes Windows PowerShell.

##  <a name="important-dcb-functionality"></a>Fonctionnalités importantes de DCB

Voici une liste qui récapitule les fonctionnalités de DCB.

1. Fournit l’interopérabilité entre les cartes réseau compatibles DCB\ et des commutateurs compatibles DCB\.

2. Fournit un transport Ethernet sans perte entre un ordinateur exécutant Windows Server2016 et son commutateur voisin en activant le contrôle de flux basé sur priority\ sur la carte réseau.

3. Fournit la possibilité d’allouer de la bande passante à un \(TC\) contrôler le trafic en pourcentage, où le traffic d’une ou plusieurs classes de trafic qui se distinguent par les indicateurs de \(priority\) de classe de trafic 802.1p.

4. Permet aux administrateurs de serveur ou réseau affecter une application d’une classe de trafic particulière ou une priorité en fonction des protocoles connus, port TCP/UDP connu ou port NetworkDirect utilisé par l’application.

5. Fournit une gestion DCB par le biais de Windows Server2016WindowsManagementInstrumentation \(WMI\) et Windows PowerShell. Pour plus d’informations, consultez la section [les commandes Windows PowerShell pour DCB](#bkmk_wps) plus loin dans cette rubrique, outre les rubriques suivantes.
    - [Composants DCB fourni par le système](https://msdn.microsoft.com/windows/hardware/drivers/network/system-provided-dcb-components)
    - [Exigences de QoS NDIS pour le pontage de centre de données](https://msdn.microsoft.com/windows/hardware/drivers/network/ndis-qos-requirements-for-data-center-bridging)

6. Fournit une gestion DCB par le biais de stratégie de groupe Windows Server2016.

7. Prend en charge la coexistence de solutions \(QoS\) de Windows Server2016 qualité de Service.

>[!NOTE]
>Avant d’utiliser n’importe quel RDMA sur la version de \(RoCE\) Converged Ethernet de RDMA, vous devez activer DCB. Alors que non requis pour les réseaux \(iWARP\) protocole Internet large zone RDMA, test a déterminé que toutes les technologies RDMA basée sur Ethernet\ fonctionnent mieux avec DCB. Pour cette raison, envisagez d’utiliser DCB pour les déploiements de RDMA iWARP. Pour plus d’informations, voir [accès à la mémoire Direct à distance (RDMA) et le commutateur SET (Embedded Teaming)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

##  <a name="practical-applications-of-dcb"></a>Applications pratiques de DCB

De nombreuses organisations possèdent un grand Fiber Channel \(FC\) zone réseau \(SAN\) installations de stockage pour le service de stockage. SAN FC nécessitent des cartes réseau spéciales sur les serveurs et des commutateurs FC dans le réseau. Ces organisations utilisent généralement également commutateurs et cartes réseau Ethernet.

Dans la plupart des cas, le matériel FC est nettement plus onéreux à déployer que le matériel Ethernet, ce qui se traduit les dépenses d’investissement volumineux. En outre, la nécessité de distinct Ethernet et SAN FC carte et le commutateur matériel réseau requiert l’espace supplémentaire, la puissance et la capacité de refroidissement dans un centre de données, ce qui entraîne des dépenses opérationnelles supplémentaires, en cours.

À partir d’un point de vue du coût, il est avantageux pour de nombreuses organisations de fusionner leur technologie FC et leur solution matérielle Ethernet\ pour fournir le stockage et les services réseau de données.

### <a name="using-dcb-for-an-ethernet-based-converged-fabric-for-storage-and-data-networking"></a>À l’aide de DCB pour une structure de convergé Ethernet\ pour le stockage et de mise en réseau de données

Pour les organisations qui ont un grand SAN FC déjà, mais souhaitent investissement supplémentaire dans la technologie FC, DCB vous permet de générer un basé sur Ethernet convergé fabric pour le stockage et de mise en réseau des données. Un DCB convergé structure peut réduire le coût total de futur de possession \(TCO\) et simplifier la gestion.

Pour les hébergeurs qui ont déjà adopté ou prévoyant d’adopter, en tant que solution de stockage, DCB peut fournir une réservation de bande passante assistée par le hardware\ pour le trafic iSCSI afin de garantir une isolation des performances. Et contrairement à d’autres propriétaires de solutions, DCB est basée sur les standards\ - et ainsi relativement facile à déployer et gérer dans un environnement réseau hétérogène.

Une implémentation Windows Server2016\ de DCB allège un grand nombre des problèmes qui peuvent se produire lorsque convergé solutions de tissu sont fournies par plusieurs fabricants \(OEMs\).

Les implémentations de solutions propriétaires fournies par les fabricants OEM plusieurs ne peut pas interagir avec eux, peut être difficiles à gérer et afficheront généralement calendriers de maintenance logicielle différents. 

En revanche, Windows Server2016 DCB repose sur standards\, et vous pouvez déployer et gérer DCB dans un réseau hétérogène.

## <a name="bkmk_wps"></a>Commandes Windows PowerShell pour DCB

Il existe des commandes DCB Windows PowerShell pour Windows Server2016 et Windows Server2012R2. Vous pouvez utiliser toutes les commandes pour Windows Server2012R2 dans Windows Server2016.

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Windows Server2016les commandes Windows PowerShell pour DCB

La rubrique suivante pour Windows Server2016 fournit une applet de commande Windows PowerShell et leur syntaxe pour tous les \(DCB\) Data Center Bridging qualité de Service \ applets de commande \-specific (QoS\). Elle répertorie les applets de commande dans l’ordre alphabétique en fonction du verbe au début de l’applet de commande.

- [Module DcbQoS](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Windows Server2012R2 les commandes Windows PowerShell pour DCB

La rubrique suivante pour Windows Server2012R2 fournit une applet de commande Windows PowerShell et leur syntaxe pour tous les \(DCB\) Data Center Bridging qualité de Service \ applets de commande \-specific (QoS\). Elle répertorie les applets de commande dans l’ordre alphabétique en fonction du verbe au début de l’applet de commande.

- [Data Center Bridging (DCB) de qualité de Service (QoS) des applets de commande dans Windows PowerShell](https://technet.microsoft.com/library/hh967440.aspx)
