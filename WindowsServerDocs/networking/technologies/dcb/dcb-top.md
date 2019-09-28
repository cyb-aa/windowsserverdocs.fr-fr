---
title: DCB (Data Center Bridging)
description: Vous pouvez utiliser cette rubrique pour obtenir des informations de présentation sur Data Center Bridging dans Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: da58f312-bd3b-4bb6-98ca-6177869dd6ad
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4c4948789be642f7a190a3cb51d7ee05c4a9822b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405738"
---
# <a name="data-center-bridging-dcb"></a>Data Center Bridging \(DCB\)

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour obtenir des informations de présentation sur \(Data\)Center Bridging DCB.

DCB est une suite de normes IEEE \(\) de Institute of Electrical and Electronics Engineers qui permet de converger des fabrics dans le centre de données, où le stockage,\-la mise en réseau des données, la communication \(entreprocessusduclusterIPC\)et le trafic de gestion partagent tous la même infrastructure réseau Ethernet.

>[!NOTE]
>En plus de cette rubrique, la documentation DCB suivante est disponible.
>
>- [Installer DCB dans Windows Server 2016 ou Windows 10](dcb-install.md)
>- [Gérer Data Center Bridging (DCB)](dcb-manage.md)

DCB fournit une\-allocation de bande passante basée sur le matériel à un type spécifique de trafic réseau et améliore la fiabilité du transport\-Ethernet avec l’utilisation d’un contrôle de flux basé sur la priorité.

L'\-allocation de bande passante basée sur le matériel est essentielle si le trafic contourne le système d’exploitation et est déchargé sur une carte réseau convergée \(,\)qui peut prendre en charge iSCSI (Internet Small Computer System Interface). Accès \(direct à la mémoire\) à distance RDMA sur Ethernet ou Fiber Channel \(sur\)Ethernet FCoE.

Le\-contrôle de workflow basé sur les priorités est essentiel si le protocole de couche supérieure, tel que Fiber Channel, suppose un transport sous-jacent sans perte.

## <a name="dcb-protocols-and-management-options"></a>Protocoles et options de gestion DCB

DCB est constitué de l’ensemble de protocoles suivant. 

- Service \(de transmission avancé\) ETS – IEEE 802.1 Qaz, qui s’appuie sur les normes 802.1 p et 802.1 q
- Contrôle \(de workflow de\)priorité PFS, IEEE 802.1 qbb 
- Le protocole \(d’échange\)DCB DCBX, IEEE 802.1 AB, tel qu’il est étendu dans la norme 802.1 Qaz.

Le protocole DCBX vous permet de configurer DCB sur un commutateur, qui peut ensuite configurer automatiquement un appareil final, tel qu’un ordinateur exécutant Windows Server 2016.

Si vous préférez gérer DCB à partir d’un commutateur, vous n’avez pas besoin d’installer DCB en tant que fonctionnalité dans Windows Server 2016, mais cette approche présente certaines limitations.

Étant donné que DCBX peut uniquement informer la carte réseau de l’ordinateur hôte des tailles de classe ETS et de l’activation de PFC, toutefois, les hôtes Windows Server requièrent généralement que DCB soit installé afin que le trafic soit mappé aux classes ETS.

Les applications Windows ne sont généralement pas conçues pour participer à des échanges DCBX. Pour cette raison, l’hôte doit être configuré séparément des commutateurs réseau, mais avec des paramètres identiques.

Si vous choisissez de gérer DCB à partir d’un commutateur, vous pouvez toujours afficher les configurations dans Windows Server 2016 à l’aide des commandes Windows PowerShell.

##  <a name="important-dcb-functionality"></a>Fonctionnalité DCB importante

Voici une liste qui récapitule les fonctionnalités de DCB.

1. Assure l’interopérabilité entre\-les cartes réseau avec DCB et\-les commutateurs pris en charge par DCB.

2. Fournit un transport Ethernet sans perte entre un ordinateur exécutant Windows Server 2016 et son commutateur voisin en activant le\-contrôle de transit basé sur la priorité sur la carte réseau.

3. Permet d’allouer de la bande passante à \(un\) contrôle de trafic TC par pourcentage, où le TC peut se composer d’une ou plusieurs classes de trafic qui sont différenciées \(par\) la priorité de la classe de trafic 802.1 p. Note.

4. Permet aux administrateurs de serveur ou réseau d’affecter une application à une classe de trafic particulière ou à des protocoles connus fondés sur les priorités, à un port TCP/UDP réputé ou à un port NetworkDirect utilisé par cette application.

5. Fournit une gestion DCB par le biais de \(Windows\) Server 2016 Windows Management Instrumentation WMI et Windows PowerShell. Pour plus d’informations, consultez la section [commandes Windows PowerShell pour DCB](#bkmk_wps) plus loin dans cette rubrique, en plus des rubriques suivantes.
    - [Composants DCB fournis par le système](https://msdn.microsoft.com/windows/hardware/drivers/network/system-provided-dcb-components)
    - [NDIS configuration requise pour la QoS pour Data Center Bridging](https://msdn.microsoft.com/windows/hardware/drivers/network/ndis-qos-requirements-for-data-center-bridging)

6. Fournit une gestion DCB par le biais de Windows Server 2016 stratégie de groupe.

7. Prend en charge la coexistence des solutions QoS \(\) de qualité de service de Windows Server 2016.

>[!NOTE]
>Avant d’utiliser une version RDMA sur RoCE \(\) Ethernet convergée de RDMA, vous devez activer DCB. Bien qu’il ne soit pas nécessaire pour les \(réseaux\) iWARP du protocole RDMA Internet, le test\-a déterminé que toutes les technologies RDMA basées sur Ethernet fonctionnent mieux avec DCB. Pour cette raison, vous devez envisager d’utiliser DCB pour les déploiements RDMA iWARP. Pour plus d’informations, consultez [accès direct à la mémoire à distance (RDMA) et Switch Embedded Teaming (Set)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

##  <a name="practical-applications-of-dcb"></a>Applications pratiques de DCB

De nombreuses organisations disposent d’installations \(San\) \) de réseau \(de zone de stockage FC de grande couche Fibre Channel pour le service de stockage. Les installations SAN FC nécessitent des cartes réseau spéciales sur les serveurs et des commutateurs FC dans le réseau. En général, ces organisations utilisent également des cartes réseau Ethernet et des commutateurs.

Dans la plupart des cas, le matériel FC est beaucoup plus onéreux à déployer que le matériel Ethernet, ce qui entraîne des dépenses d’investissement importantes. En outre, la nécessité de disposer d’une carte réseau SAN Ethernet et FC distincte et d’un commutateur matériel requiert davantage d’espace, de puissance et de refroidissement dans un centre de donnes, ce qui entraîne des dépenses opérationnelles supplémentaires en cours.

Du point de vue des coûts, il est avantageux pour de nombreuses organisations de fusionner leur\-technologie FC avec leur solution matérielle Ethernet pour fournir des services de stockage et de mise en réseau des données.

### <a name="using-dcb-for-an-ethernet-based-converged-fabric-for-storage-and-data-networking"></a>Utilisation de DCB pour une\-infrastructure convergée Ethernet pour le stockage et la mise en réseau des données

Pour les organisations qui disposent déjà d’un grand réseau SAN FC, mais qui souhaitent migrer des investissements supplémentaires vers la technologie FC, DCB vous permet de créer une infrastructure convergée Ethernet pour le stockage et la mise en réseau des données. Une infrastructure convergée DCB peut réduire le coût total de possession \(\) futur et simplifier la gestion.

Pour les hébergeurs qui ont déjà adopté ou qui prévoient d’adopter iSCSI comme solution de stockage, DCB peut fournir\-une réservation de bande passante assistée par le matériel pour le trafic iSCSI afin de garantir l’isolation des performances. Et contrairement à d’autres solutions propriétaires, DCB\-est basé sur des normes et, par conséquent, relativement facile à déployer et à gérer dans un environnement réseau hétérogène.

Une implémentation de DCB\-basée sur Windows Server 2016 atténue un grand nombre des problèmes qui peuvent se produire lorsque des solutions de structure convergées sont fournies par \(plusieurs\)fabricants OEM de fabricants de matériel d’origine.

Les implémentations de solutions propriétaires fournies par plusieurs fabricants d’ordinateurs OEM peuvent ne pas interagir les unes avec les autres, peuvent être difficiles à gérer et auront généralement des planifications de maintenance logicielle différentes. 

En revanche, Windows Server 2016 DCB est basé\-sur des normes et vous pouvez déployer et gérer DCB dans un réseau hétérogène.

## <a name="bkmk_wps"></a>Commandes Windows PowerShell pour DCB

Il existe des commandes Windows PowerShell DCB pour Windows Server 2016 et Windows Server 2012 R2. Vous pouvez utiliser toutes les commandes pour Windows Server 2012 R2 dans Windows Server 2016.

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Commandes Windows PowerShell pour DCB Windows Server 2016

La rubrique suivante pour Windows Server 2016 fournit des descriptions et la syntaxe des applets de commande Windows \(PowerShell pour l’ensemble \(des\)applets de commande spécifiques à la\) qualité de service (QoS\-) de Data Center Bridg. Elle répertorie les applets de commande par ordre alphabétique en fonction du verbe situé au début de l’applet de commande.

- [Module DcbQoS](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Commandes Windows PowerShell pour DCB Windows Server 2012 R2

La rubrique suivante pour Windows Server 2012 R2 fournit des descriptions et la syntaxe des applets de commande Windows \(PowerShell pour l’ensemble \(des\)applets de commande spécifiques à la\) qualité de service (QoS\-) de Data Center Bridg. Elle répertorie les applets de commande par ordre alphabétique en fonction du verbe situé au début de l’applet de commande.

- [Applets de commande Quality of service (QoS) Data Center Bridging (QoS) dans Windows PowerShell](https://technet.microsoft.com/library/hh967440.aspx)
