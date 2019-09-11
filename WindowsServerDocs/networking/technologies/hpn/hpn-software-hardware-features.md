---
title: Fonctionnalités et technologies intégrées logicielles et matérielles (SH)
description: Ces fonctionnalités comportent des composants logiciels et matériels. Le logiciel est étroitement lié aux capacités matérielles requises pour que la fonctionnalité fonctionne. Citons par exemple VMMQ, les ordinateurs virtuels, le déchargement de la somme de contrôle IPv4 côté envoi et RSS.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/12/2018
ms.openlocfilehash: 2f6bb2190dbaa432d20f445c90a560843d6cf4e5
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871942"
---
# <a name="software-and-hardware-sh-integrated-features-and-technologies"></a>Fonctionnalités et technologies intégrées logicielles et matérielles (SH)

Ces fonctionnalités comportent des composants logiciels et matériels. Le logiciel est étroitement lié aux capacités matérielles requises pour que la fonctionnalité fonctionne. Citons par exemple VMMQ, les ordinateurs virtuels, le déchargement de la somme de contrôle IPv4 côté envoi et RSS.

>[!TIP]
>Les fonctionnalités SH et HO sont disponibles si la carte réseau installée la prend en charge. Les descriptions de fonctionnalités ci-dessous expliquent comment savoir si votre carte réseau prend en charge la fonctionnalité.

## <a name="converged-nic"></a>Carte réseau convergée 

Une carte réseau convergée est une technologie qui permet aux cartes réseau virtuelles de l’hôte Hyper-V d’exposer des services RDMA pour héberger des processus. Windows Server 2016 ne nécessite plus de cartes réseau distinctes pour RDMA. La fonctionnalité de carte réseau convergée permet aux cartes réseau virtuelles de la partition hôte (cartes réseau virtuelles) d’exposer RDMA à la partition hôte et de partager la bande passante des cartes réseau entre le trafic RDMA et la machine virtuelle et le trafic TCP/UDP de manière équitable et gérable.

![Carte réseau convergée avec SDN](../../media/Converged-NIC/conv-nic-sdn.png)

Vous pouvez gérer l’opération de carte réseau convergée via VMM ou Windows PowerShell. Les applets de commande PowerShell sont les mêmes applets de commande utilisées pour RDMA (voir ci-dessous).

Pour utiliser la fonctionnalité de carte réseau convergée :

1.  Veillez à configurer l’ordinateur hôte pour DCB.

2.  Veillez à activer RDMA sur la carte réseau ou, dans le cas d’une équipe de jeu, les cartes réseau sont liées au commutateur Hyper-V. 

3.  Veillez à activer RDMA sur le cartes réseau virtuelles désigné pour RDMA dans l’hôte. 

Pour plus d’informations sur RDMA et sur SET, consultez [accès direct à la mémoire à distance (RDMA) et Switch Embedded Teaming (Set)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming).

## <a name="data-center-bridging-dcb"></a>DCB (Data Center Bridging) 

DCB est une suite de normes IEEE (Institute of Electrical and Electronics Engineers) qui permet d’établir des structures convergées dans des centres de données. DCB fournit une gestion de la bande passante basée sur la file d’attente matérielle dans un hôte avec la collaboration du commutateur adjacent. Tout le trafic pour le stockage, la mise en réseau des données, la communication entre processus en cluster (IPC) et la gestion partagent la même infrastructure réseau Ethernet. Dans Windows Server 2016, DCB peut être appliqué à n’importe quelle carte d’interface réseau et aux cartes réseau liées au commutateur Hyper-V.

Pour DCB, Windows Server utilise le contrôle de distribution basé sur les priorités (PFC), standardisé dans IEEE 802.1 qbb. PFC crée une infrastructure réseau sans perte (presque) en empêchant le dépassement des classes de trafic. Windows Server utilise également la sélection de transmission améliorée (ETS), standardisée dans IEEE 802.1 Qaz. ETS permet de diviser la bande passante en portions réservées pour un maximum de huit classes de trafic. Chaque classe de trafic a sa propre file d’attente de transmission et, grâce à l’utilisation de PFC, peut démarrer et arrêter la transmission au sein d’une classe.

Pour plus d’informations, consultez [Data Center Bridging (DCB)](https://docs.microsoft.com/windows-server/networking/technologies/dcb/dcb-top).

## <a name="hyper-v-network-virtualization"></a>Virtualisation de réseau Hyper-V

|                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **v1 (HNVv1)**       |                     Introduite dans Windows Server 2012, la virtualisation de réseau Hyper-V (HNV) permet la virtualisation de réseaux clients sur une infrastructure réseau physique partagée. Avec des modifications minimales nécessaires sur l’infrastructure réseau physique, HNV offre aux fournisseurs de services l’agilité de déployer et de migrer les charges de travail des locataires n’importe où sur les trois Clouds : le Cloud du fournisseur de services, le cloud privé ou le Microsoft Azure cloud public.                     |
| **v2 NVGRE (HNVv2 NVGRE)** | Dans Windows Server 2016 et System Center Virtual Machine Manager, Microsoft fournit une solution de virtualisation de réseau de bout en bout qui inclut la passerelle RAS, l’équilibrage de la charge logicielle, le contrôleur de réseau, et bien plus encore. Pour plus d’informations, voir [vue d’ensemble de la virtualisation de réseau Hyper-V dans Windows Server 2016](https://technet.microsoft.com/windows-server-docs/networking/sdn/technologies/hyper-v-network-virtualization/hyperv-network-virtualization-overview-windows-server). |
| **v2 VxLAN (HNVv2 VxLAN)** |                                                                                                                                                                                        Dans Windows Server 2016, fait partie de l’extension SDN, que vous gérez via le contrôleur de réseau.                                                                                                                                                                                        |

---

## <a name="ipsec-task-offload-ipsecto"></a>Déchargement de tâche IPsec (IPsecTO) 

Le déchargement de tâches IPsec est une fonctionnalité de carte réseau qui permet au système d’exploitation d’utiliser le processeur sur la carte réseau pour le travail de chiffrement IPsec.

>[!IMPORTANT] 
>Le déchargement de tâches IPsec est une technologie héritée qui n’est pas prise en charge par la plupart des cartes réseau, et où elle existe, elle est désactivée par défaut.

## <a name="private-virtual-local-area-network-pvlan"></a>Réseau local virtuel privé (PVLAN). 

Les PVLAN autorisent la communication uniquement entre les ordinateurs virtuels sur le même serveur de virtualisation. Un réseau virtuel privé n’est pas lié à une carte réseau physique. Un réseau virtuel privé est isolé de tout le trafic réseau externe sur le serveur de virtualisation, ainsi que tout trafic réseau entre le système d’exploitation de gestion et le réseau externe. Ce type de réseau est utile lorsque vous avez besoin de créer un environnement réseau isolé, comme un domaine de test isolé. Les piles Hyper-V et SDN prennent uniquement en charge le mode de port isolé PVLAN.

Pour plus d’informations sur l’isolation [PVLAN, consultez System Center : Virtual Machine Manager blog](https://blogs.technet.microsoft.com/scvmm/2013/06/04/logical-networks-part-iv-pvlan-isolation/)d’ingénierie.

## <a name="remote-direct-memory-access-rdma"></a>RDMA (Remote Direct Memory Access) 

RDMA est une technologie de mise en réseau qui fournit une communication à débit élevé et à faible latence qui réduit l’utilisation du processeur. RDMA prend en charge la mise en réseau de copie zéro en permettant à la carte réseau de transférer des données directement vers ou depuis la mémoire de l’application. Le mode de compatibilité RDMA signifie que la carte réseau (physique ou virtuelle) peut exposer RDMA à un client RDMA. En revanche, compatible RDMA signifie qu’une carte réseau prenant en charge RDMA expose l’interface RDMA en haut de la pile.

Pour plus d’informations sur RDMA, consultez [accès direct à la mémoire à distance (RDMA) et Switch Embedded Teaming (Set)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming).

## <a name="receive-side-scaling-rss"></a>Receive Side Scaling (RSS) 

RSS est une fonctionnalité de carte réseau qui sépare différents jeux de flux et les remet à différents processeurs à des fins de traitement. RSS parallélise le traitement réseau, ce qui permet à un hôte de s’adapter à des débits de données très élevés. 

Pour plus d’informations, consultez la page relative à la [mise à l’échelle côté réception (RSS)](https://docs.microsoft.com/windows-hardware/drivers/network/introduction-to-receive-side-scaling).

## <a name="single-root-input-output-virtualization-sr-iov"></a>Virtualisation d’entrée-sortie à racine unique (SR-IOV) 

SR-IOV permet au trafic des machines virtuelles de passer directement de la carte d’interface réseau à la machine virtuelle sans passer par l’hôte Hyper-V. SR-IOV est une amélioration incroyable des performances d’une machine virtuelle, mais ne permet pas à l’hôte de gérer ce canal. Utilisez uniquement SR-IOV lorsque la charge de travail est correcte, approuvée et généralement l’unique machine virtuelle de l’hôte.

Le trafic qui utilise SR-IOV contourne le commutateur Hyper-V, ce qui signifie que toutes les stratégies, par exemple, les listes de contrôle d’accès ou la gestion de la bande passante ne seront pas appliquées. Le trafic SR-IOV ne peut pas non plus être transmis via une fonctionnalité de virtualisation de réseau. par conséquent, l’encapsulation NV-GRE ou VxLAN ne peut pas être appliquée. Utilisez SR-IOV uniquement pour les charges de travail fiables dans des situations spécifiques. En outre, vous ne pouvez pas utiliser les stratégies d’hôte, la gestion de la bande passante et les technologies de virtualisation.

À l’avenir, deux technologies autorisent SR-IOV : Les tables de Flow génériques (GFT) et le déchargement de la QoS matérielle (gestion de la bande passante dans la carte réseau) : une fois que les cartes réseau de notre écosystème les prennent en charge. La combinaison de ces deux technologies rendrait SR-IOV utile pour toutes les machines virtuelles, autoriserait l’application de stratégies, de virtualisation et de règles de gestion de la bande passante, et pourrait entraîner des bonds dans l’application générale de SR-IOV.

Pour plus d’informations, consultez [vue d’ensemble de la virtualisation d’e/s d’une racine unique (SR-IOV)](https://docs.microsoft.com/windows-hardware/drivers/network/overview-of-single-root-i-o-virtualization--sr-iov-).

## <a name="tcp-chimney-offload"></a>Déchargement TCP Chimney

Le déchargement TCP Chimney, également appelé TCP Engine Offload (TOE), est une technologie qui permet à l’hôte de décharger tout le traitement TCP sur la carte réseau. Étant donné que la pile TCP de Windows Server est presque toujours plus efficace que le moteur de TOE, l’utilisation du déchargement TCP Chimney n’est pas recommandée.

>[!IMPORTANT]
>Le déchargement TCP Chimney est une technologie déconseillée. Nous vous recommandons de ne pas utiliser le déchargement TCP Chimney, car Microsoft peut cesser de le prendre en charge à l’avenir.

## <a name="virtual-local-area-network-vlan"></a>Réseau local virtuel (VLAN) 

Le réseau local virtuel est une extension de l’en-tête de trame Ethernet pour permettre le partitionnement d’un réseau local en plusieurs réseaux locaux virtuels, chacun utilisant son propre espace d’adressage. Dans Windows Server 2016, les réseaux locaux virtuels sont définis sur les ports du commutateur Hyper-V ou en définissant des interfaces d’équipe sur les équipes d’association de cartes réseau. Pour plus d’informations, consultez [Association de cartes réseau et réseaux locaux virtuels (VLAN)](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nict-and-vlans).

## <a name="virtual-machine-queue-vmq"></a>File d’attente d’ordinateurs virtuels (VMQ, Virtual Machine Queue) 

VMQ est une fonctionnalité de carte réseau qui alloue une file d’attente pour chaque machine virtuelle. À chaque fois que Hyper-V est activé ; vous devez également activer l’ordinateurs virtuels. Dans Windows Server 2016, VMQ utilise le commutateur de carte réseau vPorts avec une file d’attente unique assignée au vPort pour fournir les mêmes fonctionnalités. Pour plus d’informations, voir la rubrique relative à [la mise à l’échelle côté réception virtuelle et l'](https://docs.microsoft.com/windows-server/networking/technologies/vrss/vrss-top) [Association de cartes réseau](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

## <a name="virtual-machine-multi-queue-vmmq"></a>Plusieurs files d’attente d’ordinateurs virtuels (VMMQ) 

VMMQ est une fonctionnalité de carte réseau qui permet de répartir le trafic d’une machine virtuelle sur plusieurs files d’attente, chacune étant traitée par un processeur physique différent. Le trafic est ensuite transmis à plusieurs processeurs de la machine virtuelle tels qu’ils seraient dans vRSS, ce qui permet de transmettre une bande passante réseau importante à la machine virtuelle.

---