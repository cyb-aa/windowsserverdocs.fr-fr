---
title: DCB (Data Center Bridging)
description: Vous pouvez utiliser cette rubrique pour obtenir des informations sur Data Center Bridging dans Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: da58f312-bd3b-4bb6-98ca-6177869dd6ad
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7cb488192a873743db27d9c1d09c5912bc810bb8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834180"
---
# <a name="data-center-bridging-dcb"></a>Pontage de centre de données \(DCB\)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour obtenir des informations sur Data Center Bridging \(DCB\).

DCB est une suite de l’Institute of Electrical and Electronics Engineers \(IEEE\) normes qui active Converged Fabrics dans le centre de données, où stockage, mise en réseau, des données de cluster Inter\-Communication de processus \(IPC\), et tout le trafic de gestion partagent l’infrastructure du réseau Ethernet.

>[!NOTE]
>Outre cette rubrique, la documentation de DCB suivante est disponible
>
>- [Installer DCB dans Windows Server 2016 ou Windows 10](dcb-install.md)
>- [Gérer Data Center Bridging (DCB)](dcb-manage.md)

DCB fournit le matériel\-en fonction d’allocation de bande passante à un type spécifique de trafic réseau et améliore la fiabilité du transport Ethernet avec l’utilisation de priorité\-en fonction de contrôle de flux.

Matériel\-allocation de bande passante en est essentielle si le trafic contourne le système d’exploitation et être déchargé sur une carte réseau convergé, qui peut prendre en charge les Internet Small Computer System Interface \(iSCSI\), Remote Direct Memory Access \(RDMA\) sur Ethernet ou Fiber Channel over Ethernet \(FCoE\).

Priorité\-contrôle de flux en est essentiel si le protocole de couche supérieure, tel que Fiber Channel, considère un transport sous-jacent sans perte.

## <a name="dcb-protocols-and-management-options"></a>Protocoles DCB et les Options de gestion

DCB se compose de l’ensemble suivant de protocoles. 

- Amélioré le Service de Transmission \(ETS\) – IEEE 802.1Qaz, qui repose sur le 802 .1p et de 802. 1 q normes
- Contrôle de flux prioritaire \(PFS\), IEEE 802.1qbb 
- Protocole d’échange de DCB \(DCBX\), IEEE 802.1AB, en tant qu’étendue dans le 802.1Qaz standard.

Le protocole DCBX vous permet de configurer DCB sur un commutateur peut ensuite les configurer automatiquement un périphérique, tel qu’un ordinateur exécutant Windows Server 2016.

Si vous préférez gérer DCB à partir d’un commutateur, vous n’avez pas besoin d’installer DCB en tant que fonctionnalité dans Windows Server 2016, mais cette approche inclut certaines limitations.

Car DCBX peut informer seulement la carte réseau hôte de tailles de classe ETS et activation de PFC, toutefois, les hôtes Windows Server généralement requièrent que DCB est installé afin que le trafic est mappé aux classes ETS.

Les applications Windows ne sont généralement pas conçues pour participer aux échanges DCBX. Pour cette raison, l’hôte doit être configuré séparément à partir des commutateurs de réseau, mais avec des paramètres identiques.

Si vous choisissez de ne gérer DCB à partir d’un commutateur, vous pouvez toujours afficher les configurations dans Windows Server 2016 à l’aide de commandes Windows PowerShell.

##  <a name="important-dcb-functionality"></a>Fonctionnalités DCB importantes

Voici une liste qui récapitule les fonctionnalités de DCB.

1. Fournit une interopérabilité entre DCB\-cartes réseau compatibles et DCB\-commutateurs compatibles.

2. Fournit un transport Ethernet sans perte entre un ordinateur exécutant Windows Server 2016 et son commutateur voisin en activant la priorité\-en fonction de contrôle de flux sur la carte réseau.

3. Offre la possibilité d’allouer la bande passante à un contrôle du trafic \(TC\) par pourcentage, où le TC peut comporter une ou plusieurs classes de trafic sont différenciées par la classe de trafic 802.1p \(priorité\) indicateurs.

4. Permet aux administrateurs de serveur ou réseau d’affecter une application à une classe de trafic particulière ou à des protocoles connus fondés sur les priorités, à un port TCP/UDP réputé ou à un port NetworkDirect utilisé par cette application.

5. Fournit une gestion DCB via Windows Server 2016 Windows Management Instrumentation \(WMI\) et Windows PowerShell. Pour plus d’informations, consultez la section [commandes PowerShell de Windows pour DCB](#bkmk_wps) plus loin dans cette rubrique, outre les rubriques suivantes.
    - [Composants DCB fournie par le système](https://msdn.microsoft.com/windows/hardware/drivers/network/system-provided-dcb-components)
    - [Configuration requise de QoS NDIS pour le pontage de centre de données](https://msdn.microsoft.com/windows/hardware/drivers/network/ndis-qos-requirements-for-data-center-bridging)

6. Fournit une gestion DCB via la stratégie de groupe Windows Server 2016.

7. Prend en charge la coexistence de Windows Server 2016 qualité de Service \(QoS\) solutions.

>[!NOTE]
>Avant d’utiliser n’importe quel RDMA over Converged Ethernet \(RoCE\) version de RDMA, vous devez activer DCB. Bien que non requis pour le protocole Internet large zone RDMA \(iWARP\) réseaux, de test a déterminé que Ethernet tous les\-en fonction de RDMA technologies fonctionnent mieux avec DCB. Pour cette raison, vous devez envisager d’utiliser DCB pour les déploiements de RDMA iWARP. Pour plus d’informations, consultez [accès de mémoire Direct à distance (RDMA) et l’association de cartes SET (Switch Embedded)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

##  <a name="practical-applications-of-dcb"></a>Applications pratiques de DCB

De nombreuses organisations disposent de grandes Fiber Channel \(FC\) réseau de stockage \(SAN\) installations pour service de stockage. Les installations SAN FC nécessitent des cartes réseau spéciales sur les serveurs et des commutateurs FC dans le réseau. Ces entreprises utilisent généralement également les commutateurs et cartes réseau Ethernet.

Dans la plupart des cas, le matériel FC est nettement plus onéreux à déployer que le matériel Ethernet, ce qui entraîne des dépenses d’investissement volumineux. En outre, la configuration requise pour la carte Ethernet et SAN FC réseau adaptateur et le commutateur matériel distinct nécessite un espace supplémentaire, la puissance et la capacité de refroidissement dans un centre de données, entraînant ainsi les dépenses opérationnelles supplémentaires, en cours.

Du point de vue du coût, il est avantageux pour de nombreuses organisations à fusionner leur technologie FC et leur Ethernet\-solution de matériel pour assurer le stockage et mise en réseau des services de données.

### <a name="using-dcb-for-an-ethernet-based-converged-fabric-for-storage-and-data-networking"></a>À l’aide de DCB pour Ethernet\-en fonction de tissu convergé pour le stockage et de mise en réseau des données

Pour les organisations qui déjà un SAN FC de grande taille, mais souhaitant migrer investissement supplémentaire dans la technologie FC, DCB vous permet de générer un basé sur Ethernet convergé fabric pour le stockage et de mise en réseau des données. Un DCB convergé fabric peut réduire le coût total futures de la propriété \(coût total de possession\) et simplifier la gestion.

Pour les hébergeurs qui ont déjà adopté ou qui prévoient d’adopter, iSCSI comme solution de stockage, DCB peut fournir le matériel\-assisté de réservation de bande passante pour le trafic iSCSI afin de garantir l’isolation des performances. Et contrairement à d’autres propriétaires de solutions, DCB est normes\-en - et ainsi relativement facile à déployer et gérer dans un environnement réseau hétérogène.

Un serveur Windows Server 2016\-implémentation de DCB élimine une grande partie des problèmes qui peuvent se produire lorsque convergé solutions de tissu sont fournies par plusieurs fabricants \(OEM\).

Les implémentations de solutions propriétaires fournies par plusieurs fabricants OEM ne peut pas interagir avec eux, peuvent être difficiles à gérer et sera généralement les planifications de maintenance de logiciels différents. 

En revanche, Windows Server 2016 DCB est normes\-en, et vous pouvez déployer et gérer DCB dans un réseau hétérogène.

## <a name="bkmk_wps"></a>Commandes PowerShell de Windows pour DCB

Il existe des commandes de DCB Windows PowerShell pour Windows Server 2016 et Windows Server 2012 R2. Vous pouvez utiliser toutes les commandes pour Windows Server 2012 R2 dans Windows Server 2016.

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Commandes de Windows Server 2016 et Windows PowerShell pour DCB

La rubrique suivante pour Windows Server 2016 fournit les descriptions d’applet de commande Windows PowerShell et la syntaxe pour tous les Data Center Bridging \(DCB\) qualité de Service \(QoS\)\-applets de commande spécifique. Elle répertorie les applets de commande par ordre alphabétique en fonction du verbe situé au début de l’applet de commande.

- [DcbQoS Module](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Commandes Windows Server 2012 R2 Windows PowerShell pour DCB

La rubrique suivante pour Windows Server 2012 R2 fournit des descriptions d’applet de commande Windows PowerShell et la syntaxe pour tous les Data Center Bridging \(DCB\) qualité de Service \(QoS\)\-applets de commande spécifique. Elle répertorie les applets de commande par ordre alphabétique en fonction du verbe situé au début de l’applet de commande.

- [Data Center Bridging (DCB) de qualité de Service (QoS) des applets de commande dans Windows PowerShell](https://technet.microsoft.com/library/hh967440.aspx)
