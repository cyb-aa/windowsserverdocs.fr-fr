---
title: Fonctionnalités et technologies intégrées logicielles et matérielles (SH)
description: 'Ces fonctionnalités ont des composants logiciels et matériels. Le logiciel est étroitement lié à des capacités matérielles qui sont requises pour la fonctionnalité soit opérationnelle. Par exemple : VMMQ VMQ, déchargement de somme de contrôle côté envoi IPv4 et RSS.'
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/12/2018
ms.openlocfilehash: 98857a5a6d665728c1aab2a6a2df64997d4166b0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446226"
---
# <a name="software-and-hardware-sh-integrated-features-and-technologies"></a>Fonctionnalités et technologies intégrées logicielles et matérielles (SH)

Ces fonctionnalités ont des composants logiciels et matériels. Le logiciel est étroitement lié à des capacités matérielles qui sont requises pour la fonctionnalité soit opérationnelle. Par exemple : VMMQ VMQ, déchargement de somme de contrôle côté envoi IPv4 et RSS.

>[!TIP]
>Fonctionnalités SH et HO sont disponibles si la carte réseau installée le prend en charge. Les descriptions de fonctionnalité ci-dessous décrit comment savoir si votre carte réseau prend en charge la fonctionnalité.

## <a name="converged-nic"></a>Carte réseau convergé 

Carte réseau convergé est une technologie qui permet des cartes réseau virtuelles dans l’hôte Hyper-V pour exposer des services RDMA pour les processus hôtes. Windows Server 2016 n’a plus besoin de cartes réseau distinctes pour RDMA. La fonctionnalité de carte d’interface réseau convergé permet les cartes réseau virtuelles dans la partition hôte (VNIC) pour exposer RDMA à la partition hôte et de partager la bande passante des cartes réseau entre le trafic RDMA et la machine virtuelle et le reste du trafic TCP/UDP de manière équitable et facile à gérer.

![Carte de réseau convergé avec SDN](../../media/Converged-NIC/conv-nic-sdn.png)

Vous pouvez gérer l’opération de carte réseau convergence via VMM ou Windows PowerShell. Les applets de commande PowerShell sont les mêmes applets de commande utilisé pour RDMA (voir ci-dessous).

Pour utiliser la fonctionnalité de carte réseau convergence :

1.  Veillez à définir l’hôte pour DCB.

2.  Veillez à activer RDMA sur la carte réseau, ou dans le cas d’une équipe de jeu, les cartes réseau sont liées au commutateur Hyper-V. 

3.  Veillez à activer RDMA sur les cartes réseau virtuelles désignées pour RDMA dans l’hôte. 

Pour plus d’informations sur RDMA et SET, consultez [accès de mémoire Direct à distance (RDMA) et l’association de cartes SET (Switch Embedded)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming).

## <a name="data-center-bridging-dcb"></a>DCB (Data Center Bridging) 

DCB est une suite de normes Institute of Electrical and Electronics Engineers (IEEE) qui active Converged Fabrics dans les centres de données. DCB assure la gestion de bande passante basée sur la file d’attente de matériel dans un hôte avec le commutateur adjacent coopération. Tout le trafic pour le stockage, les données de mise en réseau, clusters Communication entre processus (IPC), de gestion et partagent la même infrastructure de réseau Ethernet. Dans Windows Server 2016, DCB peut être appliqué individuellement à une carte réseau et aux cartes réseau liés à Hyper-V basculer.

Pour DCB, Windows Server utilise basé sur la priorité de flux de contrôle (PFC), standardisé dans IEEE 802.1qbb. Le PFC crée une structure réseau (presque) sans perte en empêchant le dépassement de capacité dans les classes de trafic. Windows Server utilise également amélioré Transmission sélection (ETS), standardisé dans IEEE 802.1Qaz. ETS permet la division de la bande passante en parties réservées pour les classes jusqu'à huit du trafic. Chaque classe de trafic a son propre transmission de file d’attente et, en utilisant le PFC, peut démarrer et arrêter la transmission au sein d’une classe.

Pour plus d’informations, consultez [Data Center Bridging (DCB)](https://docs.microsoft.com/windows-server/networking/technologies/dcb/dcb-top).

## <a name="hyper-v-network-virtualization"></a>Virtualisation de réseau Hyper-V

|                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **v1 (HNVv1)**       |                     Introduite dans Windows Server 2012, la virtualisation de réseau Hyper-V (HNV) permet la virtualisation des réseaux des clients sur une infrastructure réseau physique partagée. Moyennant des changements minimaux nécessaires sur l’infrastructure réseau physique, HNV fournit des fournisseurs de services l’agilité nécessaire pour déployer et migrer des charges de travail clientes n’importe où sur les trois clouds : le service fournisseur cloud, le cloud privé ou le cloud public Microsoft Azure.                     |
| **v2 NVGRE (HNVv2 NVGRE)** | Dans Windows Server 2016 et System Center Virtual Machine Manager, Microsoft fournit une solution de virtualisation de réseau de bout en bout qui inclut la passerelle RAS, l’équilibrage de charge logiciel, contrôleur de réseau et bien plus encore. Pour plus d’informations, consultez [vue d’ensemble de la virtualisation de réseau Hyper-V dans Windows Server 2016](https://technet.microsoft.com/windows-server-docs/networking/sdn/technologies/hyper-v-network-virtualization/hyperv-network-virtualization-overview-windows-server). |
| **v2 VxLAN HNVv2 VxLAN)** |                                                                                                                                                                                        Dans Windows Server 2016, fait partie de l’extension de SDN, que vous gérez dans le contrôleur de réseau.                                                                                                                                                                                        |

---

## <a name="ipsec-task-offload-ipsecto"></a>Déchargement de tâches IPsec (IPsecTO) 

Déchargement de tâches IPsec est une fonctionnalité de carte réseau qui permet au système d’exploitation à utiliser le processeur sur la carte réseau pour le travail de chiffrement IPsec.

>[!IMPORTANT] 
>Déchargement de tâche IPsec est une technologie héritée qui n’est pas pris en charge par la plupart des cartes réseau, et où il n’existe pas, il est désactivé par défaut.

## <a name="private-virtual-local-area-network-pvlan"></a>Privé virtuel réseau local (PVLAN). 

Pvlan autorise la communication uniquement entre les machines virtuelles sur le même serveur de virtualisation. Un réseau privé virtuel n’est pas lié à une carte réseau physique. Un réseau virtuel privé est isolé de tout le trafic réseau externe sur le serveur de virtualisation, ainsi que tout le trafic réseau entre le système d’exploitation de gestion et le réseau externe. Ce type de réseau est utile lorsque vous avez besoin de créer un environnement réseau isolé, comme un domaine de test isolé. L’Hyper-V et les piles SDN prend en charge le mode PVLAN isolé Port uniquement.

Pour plus d’informations sur l’isolement de PVLAN, consultez [System Center : Blog d’ingénierie de Virtual Machine Manager](https://blogs.technet.microsoft.com/scvmm/2013/06/04/logical-networks-part-iv-pvlan-isolation/).

## <a name="remote-direct-memory-access-rdma"></a>RDMA (Remote Direct Memory Access) 

Il s’agit d’une technologie de réseau qui fournit la communication à débit élevé et à faible latence qui réduit l’utilisation du processeur. RDMA prend en charge la mise en réseau de la copie de zéro en activant la carte réseau transférer des données directement vers ou à partir de la mémoire de l’application. Prenant en charge RDMA signifie que la carte réseau (physique ou virtuelle) est capable d’exposer RDMA à un client RDMA. Compatible RDMA, en revanche, signifie qu'une carte réseau prenant en charge RDMA expose l’interface RDMA haut de la pile.

Pour plus d’informations sur RDMA, consultez [accès de mémoire Direct à distance (RDMA) et l’association de cartes SET (Switch Embedded)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming).

## <a name="receive-side-scaling-rss"></a>Receive Side Scaling (RSS) 

RSS est une fonctionnalité de carte réseau qui sépare différents ensembles de flux de données et les remet à des processeurs différents pour le traitement. RSS parallélise la mise en réseau de traitement, l’activation d’un ordinateur hôte à l’échelle à des débits très élevés. 

Pour plus d’informations, consultez [mise à l’échelle côté réception (RSS)](https://docs.microsoft.com/windows-hardware/drivers/network/introduction-to-receive-side-scaling).

## <a name="single-root-input-output-virtualization-sr-iov"></a>Virtualisation d’entrée-sortie de racine unique (SR-IOV) 

SR-IOV autorise le trafic de machine virtuelle déplacer directement à partir de la carte réseau à la machine virtuelle sans passer par l’hôte Hyper-V. SR-IOV est une incroyable amélioration des performances pour une machine virtuelle, mais ne dispose pas de la possibilité pour l’hôte pour gérer ce canal. Utiliser SR-IOV lorsque la charge de travail est valide, approuvé et généralement la seule machine virtuelle dans l’hôte.

Le trafic qui utilise SR-IOV contourne le commutateur Hyper-V, ce qui signifie que toutes les stratégies, par exemple, ACL, ou la gestion de bande passante n’est pas appliquée. Le trafic SR-IOV ne peuvent pas être passé via toute fonctionnalité de virtualisation de réseau, afin de l’encapsulation NV-GRE ou VxLAN ne peut pas être appliquée. Utiliser uniquement SR-IOV pour des charges de travail bien approuvées dans des situations spécifiques. En outre, vous ne pouvez pas utiliser les stratégies d’hôte, de gestion de bande passante et de technologies de virtualisation.

À l’avenir, les deux technologies permettrait SR-IOV : Flux de Tables (GFT) et non génériques matériel décharger de la QoS (gestion de la bande passante sur la carte réseau) : une fois que les cartes réseau de notre écosystème de les prendre en charge. La combinaison de ces deux technologies serait utile pour toutes les machines virtuelles, SR-IOV permettrait stratégies, la virtualisation et règles de gestion de la bande passante à appliquer et pourrait résultat dans un grand bond en avant dans générales application de SR-IOV.

Pour plus d’informations, consultez [vue d’ensemble des e/s virtualisation une racine unique (SR-IOV)](https://docs.microsoft.com/windows-hardware/drivers/network/overview-of-single-root-i-o-virtualization--sr-iov-).

## <a name="tcp-chimney-offload"></a>Déchargement TCP Chimney

Le déchargement TCP Chimney, également appelé TCP moteur de déchargement (TOE), est une technologie qui permet à l’hôte décharger tout le traitement TCP pour la carte réseau. Étant donné que la pile TCP de Windows Server est presque toujours plus efficace que le moteur TOE, à l’aide de déchargement TCP Chimney est déconseillé.

>[!IMPORTANT]
>Le déchargement TCP Chimney est une technologie obsolète. Nous vous recommandons de que ne pas utiliser le déchargement TCP Chimney que Microsoft peut cesser de prise en charge à l’avenir.

## <a name="virtual-local-area-network-vlan"></a>Réseau local virtuel (VLAN) 

Réseau local virtuel est une extension de l’en-tête de trame Ethernet pour activer le partitionnement d’un réseau local dans plusieurs réseaux locaux virtuels, chacun utilisant son propre espace d’adressage. Dans Windows Server 2016, les réseaux locaux virtuels sont définies sur les ports du commutateur Hyper-V ou en définissant des interfaces de l’équipe sur les équipes d’association de cartes réseau. Pour plus d’informations, consultez [association de cartes réseau et réseaux locaux virtuels (VLAN)](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nict-and-vlans).

## <a name="virtual-machine-queue-vmq"></a>File d’attente d’ordinateurs virtuels (VMQ, Virtual Machine Queue) 

Vmq est une fonctionnalité de carte réseau qui alloue une file d’attente pour chaque machine virtuelle. Chaque fois que vous avez Hyper-V est activé ; Vous devez également activer des ordinateurs virtuels. Dans Windows Server 2016, Vmq permet vPorts de commutateur de la carte réseau avec une file d’attente unique affecté à la vPort fournissent les mêmes fonctionnalités. Pour plus d’informations, consultez [virtuel l’échelle côté réception (vRSS)](https://docs.microsoft.com/windows-server/networking/technologies/vrss/vrss-top) et [association de cartes réseau](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

## <a name="virtual-machine-multi-queue-vmmq"></a>Virtual Machine File d’attente (VMMQ) 

VMMQ est une fonctionnalité de carte réseau qui autorise le trafic pour une machine virtuelle afin de répartir sur plusieurs files d’attente, chacune traité par un processeur physique différent. Le trafic est ensuite transmis à plusieurs LPs dans la machine virtuelle comme il le serait dans vRSS, ce qui permet de distribution de la bande passante réseau substantielle la machine virtuelle.

---