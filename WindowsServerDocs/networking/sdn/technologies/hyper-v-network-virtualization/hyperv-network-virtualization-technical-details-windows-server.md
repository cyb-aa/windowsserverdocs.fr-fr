---
title: Détails techniques de la virtualisation réseau Hyper-V dans Windows Server 2016
description: Cette rubrique fournit des informations techniques sur la virtualisation de réseau Hyper-V dans Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.service: virtual-network
ms.suite: na
ms.technology:
- networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9efe0231-94c1-4de7-be8e-becc2af84e69
ms.author: pashort
author: shortpatti
ms.openlocfilehash: af2b2a0b151601124bb473c465e7d5a97f2b9150
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="hyper-v-network-virtualization-technical-details-in-windows-server-2016"></a>Détails techniques de la virtualisation réseau Hyper-V dans Windows Server 2016

>S’applique à: Windows Server 2016

Virtualisation de serveur permet à plusieurs instances de serveur pour s’exécuter simultanément sur un seul hôte physique. encore des instances de serveur sont isolés les uns des autres. Chaque ordinateur virtuel fonctionne essentiellement comme s’il s’agit du seul serveur en cours d’exécution sur l’ordinateur physique.  
  
La virtualisation de réseau fournit une fonctionnalité similaire, dans les plusieurs réseaux virtuels (avec éventuellement des chevauchements d’adresses IP) s’exécuter sur la même infrastructure réseau physique et chaque réseau virtuel fonctionne comme s’il s’agit du seul réseau virtuel en cours d’exécution sur l’infrastructure réseau partagé. La figure 1 illustre cette relation.  
  
![Virtualisation de serveur comparée à la virtualisation de réseau](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF1.gif)  
  
Figure 1: La virtualisation de serveur comparée à la virtualisation de réseau  
  
## <a name="hyper-v-network-virtualization-concepts"></a>Concepts de la virtualisation réseau Hyper-V  
Dans la virtualisation de réseau Hyper-V (HNV), un client ou un client est défini comme le «propriétaire» d’un ensemble de sous-réseaux IP qui sont déployés dans une entreprise ou d’un centre de données. Un client peut être une société ou entreprise avec plusieurs départements ou des unités commerciales dans un centre de données privé qui nécessitent l’isolation du réseau ou un locataire dans un centre de données publique qui est hébergé par un fournisseur de services. Chaque client peut disposer d’un ou plusieurs [réseaux virtuels](#VirtualNetworks) dans le centre de données et chaque virtuel réseau se compose d’un ou plusieurs [sous-réseaux virtuels](#VirtualSubnets).  
  
Il existe deux implémentations de HNV qui seront disponibles dans Windows Server 2016: HNVv1 et HNVv2.  
  
-   **HNVv1**  
  
    HNVv1 est compatible avec Windows Server 2012 R2 et System Center 2012 R2 Virtual Machine Manager (VMM). Configuration pour HNVv1 s’appuie sur la gestion WMI et les applets de commande Windows PowerShell (facilitée par le biais de System Center VMM) pour définir les paramètres d’isolation et d’adresse de client (CA) - réseau virtuel - à des mappages d’adresse physique (PA) et le routage. Aucuns fonctionnalités supplémentaires n’ont été ajoutés à HNVv1 dans Windows Server 2016 et aucuns nouvelles fonctionnalités ne sont planifiées.  
    
    • CONFIGURER l’association de cartes réseau et V1 HNV ne sont pas compatibles par plateforme.
    
    O à utiliser des utilisateurs de passerelles HA NVGRE devez soit utiliser LBFO équipe ou aucune équipe. Ou
    
    Passerelles de contrôleur de réseau utilisez déployés o avec ensemble associées commutateur.

  
-   **HNVv2**  
  
    Un nombre important de nouvelles fonctionnalités est inclus dans HNVv2 qui est implémentée à l’aide d’Azure Virtual filtrage plateforme (VFP) extension dans le commutateur Hyper-V de transfert. HNVv2 est entièrement intégré avec Microsoft Azure Stack qui inclut le nouveau contrôleur de réseau dans la pile de mise en réseau défini par logiciel (SDN).  Stratégie de réseau virtuel est définie par le biais de Microsoft [contrôleur de réseau](../../../sdn/technologies/network-controller/Network-Controller.md) à l’aide d’un RESTful NorthBound (NB) API et raccordés à un Agent hôte via plusieurs SouthBound aux interfaces (essai), y compris OVSDB. La stratégie contre les programmes Agent hôte dans l’extension VFP du commutateur Hyper-V sur lequel elle est appliquée.  
  
    > [!IMPORTANT]  
    > Cette rubrique se concentre sur HNVv2.  
  
### <a name="VirtualNetworks"></a>Réseau virtuel  
  
-   Chaque réseau virtuel se compose d’un ou plusieurs sous-réseaux virtuels. Un réseau virtuel constitue une limite d’isolement où les ordinateurs virtuels au sein d’un réseau virtuel peut communiquer uniquement entre eux. En règle générale, cette isolation a été appliquée à l’aide de réseaux locaux virtuels avec une plage d’adresses IP séparée et 802. 1 q balise ou ID de VLAN. Mais avec HNV, l’isolation est appliquée à l’aide de l’encapsulation NVGRE ou VXLAN pour créer des réseaux de superposition avec la possibilité de chevauchement de sous-réseaux IP entre les clients ou locataires.  
  
-   Chaque réseau virtuel possède un ID de domaine routage (RDID) unique sur l’ordinateur hôte. Cette RDID correspond à peu près à un ID de ressource pour identifier le réseau virtuel ressource reste dans le contrôleur de réseau. La ressource reste du réseau virtuel est référencé à l’aide d’un espace de noms d’identificateur de ressource uniforme (URI) avec l’ID de ressource ajouté.  
  
### <a name="VirtualSubnets"></a>Sous-réseaux virtuels  
  
-   Un sous-réseau virtuel implémente la sémantique de sous-réseau IP de couche 3 pour les ordinateurs virtuels dans le même sous-réseau virtuel. Le sous-réseau virtuel constitue un domaine de diffusion (similaire à un réseau local virtuel) et l’isolation est appliquée à l’aide du champ de l’ID de réseau client NVGRE (TNI) ou un identificateur de réseau VXLAN (VNI).  
  
-   Chaque sous-réseau virtuel appartient à un réseau virtuel unique (RDID), et il est attribué à un unique sous-réseau ID virtuel (VSID) à l’aide de la TNI VNI touche ou dans l’en-tête de paquet encapsulé. Le VSID doit être unique au sein du centre de données et se trouve dans la plage de 4096 à 2 ^ 24-2.  
  
Des principaux avantages du réseau virtuel et du domaine de routage sont qu’il permet aux clients de porter leurs topologies réseau (par exemple, les sous-réseaux IP) vers le cloud. La figure 2 illustre un exemple dans lequel Contoso Corp possède deux réseaux distincts: R & D Net et du réseau Sales Net. Ces réseaux ayant un autre domaine de routage ID, ils ne peuvent pas interagir avec eux. Autrement dit, R & D Net Contoso est isolé du réseau Sales Net Contoso même si les deux appartiennent à Contoso Corp Contoso R & D Net contient trois sous-réseaux virtuels. Notez que les RDID et VSID sont uniques au sein d’un centre de données.  
  
![Réseaux client et sous-réseaux virtuels](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF6.gif)  
  
Figure 2: Réseaux client et sous-réseaux virtuels  
  
**Transfert de couche 2**  
  
Dans la Figure 2, les ordinateurs virtuels dans le VSID est 5001 peuvent voir leurs paquets transférés vers les ordinateurs virtuels qui sont également dans le VSID est 5001 via le commutateur Hyper-V. Les paquets entrants à partir d’un ordinateur virtuel dans le VSID est 5001 sont envoyées à un VPort spécifique sur le commutateur Hyper-V. Règles d’entrée (par exemple, encap) et les mappages (par exemple, en-tête d’encapsulation) sont appliquées par le commutateur Hyper-V pour ces paquets. Les paquets sont ensuite transmis à un autre VPort sur le commutateur Hyper-V (si l’ordinateur virtuel de destination est attaché sur le même hôte) ou à un autre commutateur Hyper-V sur un autre hôte (si l’ordinateur virtuel de destination se trouve sur un autre hôte).  
  
**Couche 3 routage**  
  
De même, les ordinateurs virtuels dans le VSID est 5001 peuvent voir leurs paquets routés vers les ordinateurs virtuels dans le VSID 5002 ou 5003 par le routeur distribué HNV qui n’est présent dans le commutateur virtuel Hyper-V de chaque ordinateur hôte. Lors de remettre le paquet Hyper-V commutateur, HNV met à jour le VSID du paquet entrant par rapport au vsid de l’ordinateur virtuel de destination. Cela se produit uniquement si les deux vsid se trouvent dans le même RDID.  Par conséquent, les cartes réseau virtuelles associés à RDID1 ne peuvent pas envoyer des paquets pour les cartes réseau virtuelles associées à RDID2 sans traverser une passerelle.  
  
> [!NOTE]  
> Dans la description de flux paquet ci-dessus, le terme «ordinateur virtuel» signifie en réalité la carte réseau virtuelle sur l’ordinateur virtuel. Il est fréquent qu’un ordinateur virtuel n’a qu’une carte réseau virtuelle unique. Dans ce cas, les mots «ordinateur virtuel» et «carte réseau virtuelle» peuvent conceptuellement la même signification.  
  
Chaque sous-réseau virtuel définit un sous-réseau IP de couche 3 et une limite de domaine de diffusion de couche 2 (L2) similaire à un réseau local virtuel. Lorsqu’un ordinateur virtuel diffuse un paquet, HNV utilise la réplication de monodiffusion (UR) pour effectuer une copie du paquet d’origine et remplacez l’adresse IP de destination et MAC avec les adresses de chaque ordinateur virtuel qui se trouvent dans le même VSID.  
  
> [!NOTE]  
> Lors de la livraison de Windows Server 2016, multidiffuser diffusion et le sous-réseau sera implémentée à l’aide de la réplication de monodiffusion. Le routage entre sous-réseaux multidiffusion et IGMP ne sont pas pris en charge.  
  
En plus d’être un domaine de diffusion, le VSID assure une isolation. Dans HNV, une carte réseau virtuelle connectée à un port de commutateur Hyper-V ayant des règles ACL appliquées directement au port (ressource de reste virtualNetworkInterface) ou pour le sous-réseau virtuel (VSID) dont il fait partie.  
  
Le port de commutateur Hyper-V doit avoir une règle ACL appliquée. Cette liste ACL peut être autoriser tous les, tout refuser, ou plus spécifiques pour autoriser uniquement certains types de trafic en fonction de mise en correspondance 5-tuple (IP Source, adresse IP de Destination, Port Source, le Port de Destination, protocole).  
  
> [!NOTE]  
> Les Extensions de commutateur Hyper-V ne fonctionnera pas avec HNVv2 dans la nouvelle pile logicielle définie de mise en réseau (SDN). HNVv2 est implémentée à l’aide de l’extension de commutateur de plateforme de filtrage virtuel Azure (VFP) qui ne peuvent pas être utilisée conjointement avec n’importe quel autre extension de commutateur 3ème partie.  
  
## <a name="switching-and-routing-in-hyper-v-network-virtualization"></a>Basculement et le routage dans la virtualisation de réseau Hyper-V  
HNVv2 implémente la sémantique de routage de couche 3 (L3) fonctionne comme un commutateur physique et de commutation de couche 2 (L2) correcte ou routeur fonctionne. Lorsqu’un ordinateur virtuel connecté à un réseau virtuel HNV tente d’établir une connexion avec un autre ordinateur virtuel dans le même sous-réseau virtuel (VSID) il devez d’abord en savoir plus de l’adresse MAC de l’autorité de certification de l’ordinateur virtuel à distance. S’il existe une entrée ARP pour l’adresse IP de l’ordinateur virtuel de destination dans la table ARP de l’ordinateur virtuel source, l’adresse MAC à partir de cette entrée est utilisée. Si une entrée n’existe pas, l’ordinateur virtuel source envoie qu'un ARP de diffusion avec une demande de l’adresse MAC correspondant à l’adresse IP de l’ordinateur virtuel de destination doit être retourné. Le commutateur Hyper-V intercepte cette demande et l’envoyer à l’Agent de l’ordinateur hôte. L’Agent hôte recherche dans sa base de données local pour une adresse MAC correspondante pour l’adresse IP de l’ordinateur virtuel de destination demandée.  
  
> [!NOTE]  
> L’Agent hôte, joue le rôle du serveur OVSDB, utilise une variante du schéma VTEP pour stocker les mappages de l’autorité de certification-PA, tableau MAC et ainsi de suite.  
  
Si une adresse MAC est disponible, l’Agent hôte injecte une réponse ARP et renvoie à l’ordinateur virtuel. Une fois que la pile de mise en réseau de l’ordinateur virtuel a toutes les informations d’en-tête L2 requises, la trame est envoyée au Port Hyper-V correspondant sur le commutateur-V. En interne, le commutateur Hyper-V teste cette image par rapport aux règles de correspondance N-tuple affecté au Port-V et s’applique certaines transformations à l’image basée sur ces règles. Plus important encore, un ensemble de transformations de l’encapsulation est appliqué pour construire l’en-tête d’encapsulation à l’aide de NVGRE ou VXLAN, en fonction de la stratégie définie au niveau du contrôleur de réseau. Selon la stratégie programmée par l’Agent de l’ordinateur hôte, un mappage de l’autorité de certification-PA est utilisé pour déterminer l’adresse IP de l’hôte Hyper-V dans lequel réside l’ordinateur virtuel de destination. Le commutateur Hyper-V garantit les règles de routage corrects et balises VLAN sont appliqués au paquet externe afin qu’il atteint l’adresse PA à distance.  
  
Si un ordinateur virtuel connecté à un réseau virtuel HNV souhaite créer une connexion avec un ordinateur virtuel dans un autre sous-réseau virtuel (VSID), le paquet doit être routé en conséquence. HNV suppose une étoile-topologie où il est uniquement une adresse IP dans l’espace de l’autorité de certification utilisée en tant que tronçon suivant pour atteindre tous les préfixes d’IP (sens une passerelle par défaut itinéraire /). Actuellement, il met en œuvre une limitation à un seul itinéraire par défaut et les itinéraires par défaut ne sont pas pris en charge.  
  
### <a name="routing-between-virtual-subnets"></a>Routage entre sous-réseaux virtuels  
Dans un réseau physique, un sous-réseau IP est un domaine de couche 2 (L2) où les ordinateurs (virtuels et physiques) peuvent communiquer directement avec eux. Le domaine de niveau 2 est un domaine de diffusion où entrées ARP (mappage d’adresse IP:MAC) apprises par le biais de requêtes ARP qui sont diffusés sur toutes les interfaces et les réponses ARP sont envoyées vers l’hôte demande. L’ordinateur utilise les informations de MAC apprises à partir de la réponse ARP pour construire complètement la trame L2, y compris les en-têtes Ethernet. Toutefois, si une adresse IP est dans un autre sous-réseau L3, la demande ARP ne dépasse pas cette limite L3. Au lieu de cela, une interface de routeur (passerelle par défaut ou de tronçon suivant) L3 avec une adresse IP du sous-réseau source doit répondre à ces requêtes ARP avec sa propre adresse MAC.  
  
Dans la mise en réseau Windows standard, un administrateur peut créer des itinéraires statiques et affectez-les à une interface réseau. En outre, une «passerelle par défaut» est généralement configurée pour l’adresse IP de tronçon suivant sur une interface où les paquets destinés à l’itinéraire par défaut (0.0.0.0/0) sont envoyés. Paquets sont envoyés à cette passerelle par défaut, si aucun itinéraire spécifique n’existe. Il s’agit généralement du routeur pour votre réseau physique.  HNV utilise un routeur intégré qui fait partie de chaque ordinateur hôte et possède une interface dans chaque VSID pour créer un routeur distribué pour les réseaux virtuels.  
  
Dans la mesure où HNV suppose une topologie en étoile, le routeur distribué HNV agit comme une passerelle par défaut unique pour tout le trafic qui transite entre les sous-réseaux virtuels qui font partie du même réseau VSID. L’adresse utilisée comme les valeurs par défaut de la passerelle par défaut à l’adresse IP plus bas dans le VSID et est attribué au routeur distribué HNV. Ce routeur distribué permet un moyen très efficace pour tout le trafic à l’intérieur d’un réseau VSID être correctement routé, car chaque hôte peut acheminer directement le trafic vers l’hôte approprié sans intermédiaire.  Cela est particulièrement vrai lorsque deux ordinateurs virtuels dans le même réseau d’ordinateurs virtuels, mais différents sous-réseaux virtuels se trouvent sur le même hôte physique.  Comme vous verrez plus loin dans cette section, le paquet n’a jamais laisser l’ordinateur hôte physique.  
  
### <a name="routing-between-pa-subnets"></a>Routage entre sous-réseaux PA  
Contrairement à HNVv1 qui allouée à une seule adresse IP fournisseur pour chaque sous-réseau virtuel (VSID), HNVv2 utilise désormais une adresse IP de l’adresse fournisseur par membre de l’équipe NIC Switch-Embedded SET (Teaming). Le déploiement par défaut suppose une association de deux et affecte les deux adresses IP fournisseur par hôte. Un seul ordinateur hôte a PA des adresses IP affectées à partir du même sous-réseau logique de fournisseur (PA) sur le même réseau local virtuel. Deux ordinateurs virtuels de clients dans le même sous-réseau virtuel peuvent en effet se trouver sur deux hôtes différents qui sont connectés à deux sous-réseaux logiques autre fournisseur. HNV va créer les en-têtes IP externes du paquet encapsulé en fonction du mappage de l’autorité de certification-PA. Toutefois, il s’appuie sur la pile TCP/IP hôte à ARP la passerelle par défaut PA et génère ensuite les en-têtes Ethernet externes en fonction de la réponse ARP. En règle générale, cette réponse ARP provient de l’interface SVI sur le commutateur physique ou un routeur L3 où l’ordinateur hôte est connecté. HNV donc repose sur le routeur L3 pour le routage les paquets encapsulés entre sous-réseaux logiques fournisseur / VLAN.  
  
### <a name="routing-outside-a-virtual-network"></a>Routage hors d’un réseau virtuel  
La plupart des déploiements client nécessitent une communication à partir de l’environnement HNV et ressources qui ne font pas partie de l’environnement HNV. Passerelles de virtualisation de réseau sont nécessaires pour permettre la communication entre les deux environnements. Nuage privé et Cloud hybride parmi des infrastructures qui exigent une passerelle HNV. En fait, les passerelles HNV sont requis pour le routage de couche 3 entre les réseaux internes et externes (physiques) (y compris NAT) ou entre différents sites et/ou clouds (privées ou publiques) qui utilisent un tunnel IPSec VPN ou GRE.  
  
Les passerelles peuvent présenter des facteurs de forme physique différent. Elles peuvent reposer sur Windows Server 2016, intégrée à un commutateur TOR Top of Rack () agissant comme une passerelle VXLAN, accessible via un IP virtuelle (VIP) publiés par un équilibreur de charge, à d’autres dispositifs réseau existants, ou consister en un nouveau dispositif réseau autonome.  
  
Pour plus d’informations sur les options de la passerelle Windows, voir [passerelle](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md).  
  
## <a name="packet-encapsulation"></a>Encapsulation de paquet  
Dans HNV, chaque carte réseau virtuelle est associée à deux adresses IP:  
  
-   **Adresse client** (CA) l’adresse IP assignée par le client, en fonction de son infrastructure intranet. Cette adresse permet au client d’échanger du trafic réseau avec l’ordinateur virtuel comme s’il n’avait pas été déplacé vers un cloud public ou privé. L’autorité de certification est visible à l’ordinateur virtuel et accessible par le client.  
  
-   **Adresse fournisseur** (PA) l’adresse IP assignée par le fournisseur d’hébergement ou les administrateurs du centre de données basées sur l’infrastructure réseau physique. L’adresse fournisseur figure dans les paquets sur le réseau qui sont échangés avec le serveur exécutant Hyper-V qui héberge l’ordinateur virtuel. Cette adresse est visible sur le réseau physique, mais pas sur l’ordinateur virtuel.  
  
Les autorités de certification mettre à jour la topologie de réseau du client, qui est virtualisée et dissociée de la topologie du réseau physique sous-jacent réel et les adresses, tel qu’implémenté par le client. Le diagramme suivant illustre la relation conceptuelle entre l’ordinateur virtuel autorités de certification et d’infrastructure réseau PAs à la suite de la virtualisation de réseau.  
  
![Diagramme conceptuel de la virtualisation de réseau sur une infrastructure physique](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF2.gif)  
  
Figure 6: Le schéma conceptuel de la virtualisation de réseau sur une infrastructure physique  
  
Dans le diagramme, virtuels client envoient des paquets de données dans l’espace de l’autorité de certification, qui transitent par l’infrastructure réseau physique par le biais de leurs propres réseaux virtuels, ou «tunnels». Dans l’exemple ci-dessus, les tunnels peuvent être considérés de comme des «enveloppes» les paquets de données Contoso et Fabrikam avec verts étiquettes d’expédition (adresses fournisseur) pour être remises par l’ordinateur hôte source de gauche à l’hôte de destination de droite. La clé est de savoir comment les hôtes déterminent les «adresses d’expédition» (fournisseur) correspondant à Contoso et l’autorité de certification Fabrikam, comment le «enveloppe» englobe les paquets et comment les hôtes de destination peuvent déballer les paquets et les remettre correctement pour les machines virtuelles destination Contoso et Fabrikam.  
  
Cette analogie simple mis en évidence les principaux aspects de la virtualisation de réseau:  
  
-   Chaque autorité de certification d’ordinateur virtuel est mappé à un hôte physique PA. Il peut y avoir plusieurs autorités de certification associé à la même adresse fournisseur.  
  
-   Les ordinateurs virtuels envoyer des paquets de données dans les espaces de l’autorité de certification, qui sont placés dans une «enveloppe» avec une paire de source et de destination PA en fonction du mappage.  
  
-   Les mappages de l’autorité de certification-PA doivent autoriser les ordinateurs hôtes différencier les paquets pour les ordinateurs virtuels client différents.  
  
Par conséquent, le mécanisme pour virtualiser le réseau consiste à virtualiser les adresses réseau utilisées par les ordinateurs virtuels. Le contrôleur de réseau est chargé pour le mappage d’adresse, et l’agent hôte gère la base de données de mappage à l’aide du schéma MS_VTEP. La section suivante décrit les mécanismes réels de la virtualisation d’adresses.  
  
## <a name="network-virtualization-through-address-virtualization"></a>Virtualisation de réseau par le biais de virtualisation d’adresses  
HNV implémente superposer des réseaux des clients à l’aide de réseau virtualisation générique Encapsulation routage (NVGRE) ou le Virtual eXtensible LAN VXLAN ().  VXLAN est la valeur par défaut.  
  
### <a name="virtual-extensible-local-area-network-vxlan"></a>Virtual eXtensible LAN VXLAN)  
Le réseau Local eXtensible virtuel (VXLAN) ([RFC 7348](http://www.rfc-editor.org/info/rfc7348)) protocole a été largement adopté sur le marché, avec prise en charge des fournisseurs tels que Cisco, Brocade, Arista, Dell, HP et d’autres personnes. Le protocole VXLAN utilise UDP comme le transport. Le port de destination UDP affectée IANA pour VXLAN est 4789 et le port UDP source doit être un hachage des informations à partir du paquet interne à utiliser pour le routage ECMP propagation. Après l’en-tête UDP, un en-tête VXLAN est ajouté au paquet qui inclut un champ réservé de 4 octets suivi d’un champ de 3 octets pour le VXLAN réseau identificateur (VNI) - VSID - suivie d’un autre champ réservé de 1 octet. Après l’en-tête VXLAN, la trame L2 de l’autorité de certification d’origine (sans la trame Ethernet de l’autorité de certification FCS) est ajoutée.  
  
![En-tête de paquet VXLAN](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VXLAN-packet-header.png)  
  
### <a name="generic-routing-encapsulation-nvgre"></a>Encapsulation générique de routage (NVGRE)  
Ce mécanisme de virtualisation de réseau utilise l’Encapsulation de routage générique (NVGRE) dans l’en-tête de tunnel. Dans NVGRE, le paquet de l’ordinateur virtuel est encapsulé dans un autre paquet. L’en-tête de ce nouveau paquet a source approprié et adresses IP fournisseur de destination en plus de l’ID de sous-réseau virtuel, qui est stocké dans le champ de clé de l’en-tête GRE, comme illustré dans la Figure 7.  
  
![Encapsulation NVGRE](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF3.gif)  
  
Figure 7: Virtualisation de réseau - encapsulation NVGRE  
  
L’ID de sous-réseau virtuel permet aux hôtes d’identifier l’ordinateur virtuel client pour un paquet donné, même si les adresses fournisseur et de l’autorité de certification de le des paquets peuvent se chevaucher. Ainsi, tous les ordinateurs virtuels sur le même hôte de partager une adresse fournisseur unique, comme illustré dans la Figure 7.  
  
Partage de l’adresse fournisseur a un impact important sur l’extensibilité réseau. Le nombre d’adresses IP et MAC qui doivent être appris par l’infrastructure du réseau peut être considérablement réduit. Par exemple, si chaque hôte final a en moyenne 30 ordinateurs virtuels, le nombre d’IP et les adresses MAC qui doivent être appris par l’infrastructure réseau est réduit par un facteur de 30. les ID de sous-réseau virtuel incorporés dans les paquets également activer une corrélation facile entre les paquets et les clients effectifs.  
  
Adresse fournisseur du schéma de partage pour Windows Server 2012 R2 est une adresse fournisseur par VSID par hôte. Pour Windows Server 2016, le schéma est une seule adresse fournisseur par les membres de l’équipe de cartes réseau.  
  
Avec Windows Server 2016 et versions ultérieures, HNV prend entièrement en charge NVGRE et VXLAN l’emploi; Il ne nécessite pas la mise à niveau ou acquisition de nouveau matériel réseau tels que les cartes réseau, commutateurs ou routeurs. Il s’agit, car ces paquets sur le réseau sont paquet IP normal dans l’espace d’adresse fournisseur, qui est compatible avec l’infrastructure de réseau d’aujourd'hui.  Toutefois, pour obtenir le meilleur parti de performances pris en charge les cartes réseau avec les derniers pilotes qui prennent en charge des déchargements de tâche.  
  
## <a name="multi-tenant-deployment-example"></a>Exemple de déploiement mutualisé  
Le diagramme suivant montre un exemple de déploiement de deux clients situés dans un centre de données cloud avec la relation d’autorité de certification-PA définie par les stratégies de réseau.  
  
![Exemple de déploiement mutualisé](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF5.png)  
  
Figure 8: Exemple de déploiement mutualisé  
  
Prenons l’exemple dans la Figure 8. Service IaaS partagé avant la déplacer vers le fournisseur d’hébergement:  
  
-   Contoso Corp a exécuté un serveur SQL Server (nommé **SQL**) à l’adresse IP 10.1.1.11 et un serveur web (nommé **Web**) à l’adresse IP 10.1.1.12, qui utilise son serveur SQL Server pour les transactions de base de données.  
  
-   Fabrikam Corp a exécuté un serveur SQL Server, également nommé **SQL** et affecté l’adresse IP 10.1.1.11 et un serveur web, également nommé **Web** à l’adresse IP 10.1.1.12, qui utilise son serveur SQL Server pour les transactions de base de données.  
  
Nous supposons que le fournisseur de services a créé précédemment le réseau logique du fournisseur (PA) via le contrôleur de réseau pour correspondre à la topologie de réseau physique. Le contrôleur de réseau alloue deux adresses IP fournisseur à partir de préfixe d’IP du sous-réseau logique dans lequel les ordinateurs hôtes sont connectés. Le contrôleur de réseau indique également la balise VLAN appropriée pour appliquer les adresses IP.  
  
Le réseau de contrôleur, Contoso Corp et Fabrikam Corp puis de créer leur réseau virtuel et les sous-réseaux qui sont sauvegardées par le réseau logique du fournisseur (PA) spécifié par le fournisseur de services. Contoso Corp et Fabrikam Corp transfèrent leurs serveurs SQL respectifs et web même fournisseur d’hébergement de service IaaS partagé où, par coïncidence, elles exécutent le **SQL** les ordinateurs virtuels sur l’hôte 1 Hyper-V et le **Web** virtuels (IIS7) sur l’hôte 2 Hyper-V. Tous les ordinateurs virtuels conservent leurs adresses IP de l’intranet d’origine (client).  
  
Les deux sociétés sont affectées de l’ID de sous-réseau virtuel suivant (VSID) par le contrôleur de réseau comme indiqué ci-dessous.  L’Agent hôte sur chacun des ordinateurs hôtes Hyper-V reçoit les adresses IP fournisseur alloués à partir du contrôleur de réseau et crée deux cartes réseau virtuelles de hôtes PA dans un compartiment réseau par défaut. Une interface réseau est affectée à chacune de ces cartes réseau virtuelles hôtes où l’adresse IP PA est affectée, comme illustré ci-dessous:  
  
-   Les ordinateurs virtuels Contoso Corp VSID et les adresses fournisseur: **VSID** est 5001, **PA SQL** est 192.168.1.10 et l’adresse **PA Web** est 192.168.2.20  
  
-   Virtuels de Fabrikam Corp VSID et les adresses fournisseur: **VSID** est 6001, **PA SQL** est 192.168.1.10 et l’adresse **PA Web** est 192.168.2.20  
  
Le contrôleur de réseau il ajoute toutes les stratégies de réseau (y compris le mappage de l’autorité de certification-PA) à l’Agent de hôte SDN qui gère la stratégie dans un magasin persistant (dans les tables de base de données OVSDB).  
  
Lorsque l’ordinateur virtuel de Web de Contoso Corp (10.1.1.12) sur l’hôte 2 Hyper-V crée une connexion TCP pour le serveur SQL Server à l’adresse 10.1.1.11, l’événement suivant se produit:  
  
-   ARP de machine virtuelle pour l’adresse MAC de destination de 10.1.1.11  
  
-   L’extension VFP dans le commutateur virtuel intercepte ce paquet et l’envoie à l’Agent d’ordinateur hôte SDN  
  
-   L’Agent hôte SDN ressemble dans son magasin de stratégies pour l’adresse MAC de 10.1.1.11  
  
-   Si un MAC est trouvé, l’Agent hôte injecte une réponse ARP à l’ordinateur virtuel  
  
-   Si un MAC n’est trouvé, aucune réponse est envoyée et l’entrée ARP de l’ordinateur virtuel pour 10.1.1.11 est marquée inaccessible.  
  
-   L’ordinateur virtuel maintenant construit un paquet TCP avec les en-têtes IP et Ethernet de l’autorité de certification appropriés et l’envoie au vSwitch  
  
-   L’extension dans le commutateur virtuel de transfert de VFP traite ce paquet à travers les couches VFP (décrite ci-dessous) affecté au port de commutateur virtuel source sur lequel le paquet a été reçu et crée une nouvelle entrée de flux dans le tableau de flux unifiée VFP  
  
-   Le moteur VFP examine la règle correspondant ou flux de table pour chaque couche (par exemple, couche de réseau virtuel) basée sur les en-têtes IP et Ethernet.  
  
-   La règle de correspondance de la couche réseau virtuel fait référence à un espace de mappage d’adresse fournisseur de l’autorité de certification et effectue l’encapsulation.  
  
-   Le type d’encapsulation (VXLAN ou NVGRE) est spécifié dans la couche réseau virtuel, ainsi que le VSID.  
  
-   Dans le cas d’encapsulation VXLAN, un en-tête UDP externe est construit avec le VSID du 5001 dans l’en-tête VXLAN.  
    Un en-tête IP externe est construit avec l’adresse PA source et de destination affectée à l’hôte Hyper-V, 2 (192.168.2.20) et l’hôte Hyper-V 1 (192.168.1.10) respectivement basées sur le magasin de stratégies de l’Agent hôte SDN.  
  
-   Il passe ensuite ce paquet sur la couche de routage PA dans VFP.  
  
-   La couche de routage PA VFP référencera le compartiment réseau utilisé pour le trafic de l’espace d’adressage fournisseur et un ID de réseau local virtuel et permet de transférer le paquet PA vers l’hôte Hyper-V 1 correctement la pile TCP/IP de l’ordinateur hôte.  
  
-   Lors de la réception du paquet encapsulé, hôte 1 Hyper-V reçoit le paquet dans le compartiment de réseau de fournisseur et le transmet au vSwitch.  
  
-   VFP traite le paquet par ses couches VFP et créer une nouvelle entrée de flux dans le tableau de flux unifiée VFP.  
  
-   Le moteur VFP correspond aux règles ingres dans la couche réseau virtuel et supprime les en-têtes Ethernet, IP et VXLAN du paquet encapsulé externe.  
  
-   Le moteur VFP transfère ensuite le paquet sur le port de commutateur virtuel auquel est connectée l’ordinateur virtuel de destination.  
  
Un processus similaire pour le trafic entre le Fabrikam Corp **Web** et **SQL** virtuels utilise les paramètres de stratégie HNV pour Fabrikam corp. Par conséquent, avec des machines virtuelles HNV, Fabrikam Corp et Contoso Corp interagissent comme si elles étaient sur leurs intranets d’origine. Ils ne peuvent jamais interagir entre eux, même s’ils utilisent les mêmes adresses IP.  
  
Les adresses distinctes (autorités de certification et les adresses fournisseur), les paramètres de stratégie des ordinateurs hôtes Hyper-V et la traduction d’adresses entre l’autorité de certification et de fournisseur pour le trafic entrant et sortant virtuels isolent ces ensembles de serveurs à l’aide de la clé NVGRE ou le VNID VLXAN. En outre, les mappages de virtualisation et de la transformation dissocie l’architecture de réseau virtuel à partir de l’infrastructure réseau physique. Bien que Contoso **SQL** et **Web** et Fabrikam **SQL** et **Web** résident dans leurs propres sous-réseaux IP de l’autorité de certification (10.1.1/24), leur déploiement physique se produit sur deux hôtes situés dans différents sous-réseaux, 192.168.1/24 et 192.168.2/24, respectivement. Il en résulte que la migration dynamique et mise en service des ordinateurs virtuels entre sous-réseaux devient possibles avec HNV.  
  
## <a name="hyper-v-network-virtualization-architecture"></a>Architecture de virtualisation de réseau Hyper-V  
Dans Windows Server 2016, HNVv2 est implémentée à l’aide d’Azure Virtual filtrage plateforme (VFP) qui est une extension de filtre NDIS au sein du commutateur Hyper-V. Le concept de VFP clé est celle d’un moteur de flux d’Action de correspondance avec une API interne exposée à l’Agent d’ordinateur hôte SDN pour la programmation de stratégie réseau. L’Agent hôte SDN lui-même reçoit la stratégie de réseau à partir du contrôleur réseau via les canaux de communication OVSDB et WCF SouthBound. Stratégie de réseau virtuel n’est pas uniquement (par exemple, mappage de l’autorité de certification-PA) programmée à l’aide de VFP mais stratégie supplémentaires tels que les ACL, la qualité de service et ainsi de suite.  
  
La hiérarchie d’objets pour le commutateur virtuel et l’extension de transfert de VFP est le suivant:  
  
-   commutateur virtuel  
  
    -   Gestion de la carte réseau externe  
  
    -   Déchargements de matériel de carte réseau  
  
    -   Règles de transfert globales  
  
    -   Port  
  
        -   Sortie de transfert de couche pour épingler des cheveux  
  
        -   Listes d’espace pour les mappages et les pools NAT  
  
        -   Table de flux unifiée  
  
        -   Couche VFP  
  
            -   Table de flux  
  
            -   Groupe  
  
            -   Règle  
  
                -   Les règles peuvent référencer des espaces  
  
Dans le VFP, une couche est créée par type de stratégie (par exemple, un réseau virtuel) et est un ensemble de tables de la règle/flux générique. Il ne dispose pas des fonctionnalités intrinsèques jusqu'à ce que des règles spécifiques sont affectés à cette couche pour implémenter ces fonctionnalités. Une priorité est attribuée à chaque couche et des couches sont attribués à un port par ordre croissant de priorité. Les règles sont organisés en groupes principalement direction et famille d’adresses IP. Les groupes sont également de priorité et au plus, une règle d’un groupe peut correspondre à un flux donné.  
  
La logique de transfert pour le commutateur virtuel avec l’extension VFP est la suivante:  
  
-   Traitement d’entrée (entrée à partir du point de vue du paquet entrant en un port)  
  
-   Transfert  
  
-   Traitement de la sortie (sortant à partir du point de vue du paquet quitte un port)  
  
VFP prend en charge le transfert de MAC interne pour les types d’encapsulation NVGRE et VXLAN ainsi qu’externe transfert réseau local virtuel MAC en fonction.  
  
L’extension VFP a un ralentissement du chemin d’accès et le chemin d’accès rapide pour le parcours de paquet. Le premier paquet dans un flux doit parcourir tous les groupes de règles dans chaque couche et faire une règle de recherche qui est une opération coûteuse. Toutefois, une fois qu’un flux est enregistré dans la table de flux unifiée avec une liste d’actions (basé sur les règles de mise en correspondance) tous les paquets suivants seront traités basés sur les entrées de table de flux unifiée.  
  
Stratégie HNV est programmée par l’agent de l’ordinateur hôte. Chaque carte réseau d’ordinateur virtuel est configuré avec une adresse IPv4. Ceux-ci sont les autorités de certification qui servira par les ordinateurs virtuels à communiquer entre eux, et sont transportées dans les paquets IP à partir d’ordinateurs virtuels. HNV encapsule la trame de l’autorité de certification dans un cadre PA basé sur des stratégies de virtualisation de réseau stockés dans la base de données de l’agent de l’ordinateur hôte.  
  
![Architecture HNV](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF7.png)  
  
Figure 9: Architecture HNV  
  
## <a name="summary"></a>Résumé  
Les centres de données en nuage peuvent fournir de nombreux avantages, notamment une extensibilité améliorée et une meilleure utilisation des ressources. Pour bénéficier de ces avantages potentiels nécessite une technologie fondamentalement les problèmes d’extensibilité mutualisée dans un environnement dynamique. HNV a été conçue pour résoudre ces problèmes et également pour améliorer l’efficacité opérationnelle du centre de données en dissociant la topologie réseau virtuelle pour la topologie réseau physique. S’appuyant sur une norme existante, HNV s’exécute dans le centre de données d’aujourd'hui et fonctionne avec votre infrastructure VXLAN existante. Les clients avec HNV peuvent désormais consolider leurs centres de données dans un cloud privé ou étendre de manière transparente leurs centres de données à l’environnement d’un fournisseur serveur d’hébergement d’un nuage hybride.  
  
## <a name="BKMK_LINKS"></a>Voir aussi  
Pour en savoir plus sur HNVv2, voir les liens suivants:  
  
|Type de contenu|Références|  
|----------------|--------------|  
|**Ressources de la Communauté**|-   [Blog de l’Architecture de Cloud privé](http://blogs.technet.com/b/privatecloud)<br />-Posez des questions:[cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)|  
|**RFC**|-   [NVGRE document RFC préliminaire sur](http://www.ietf.org/id/draft-sridharan-virtualization-nvgre-07.txt)<br />-   [VXLAN - RFC 7348](http://www.rfc-editor.org/info/rfc7348)|  
|**Technologies connexes**|-Pour plus d’informations techniques de virtualisation de réseau Hyper-V dans Windows Server 2012 R2, voir [détails techniques sur la virtualisation de réseau Hyper-V](https://technet.microsoft.com/library/jj134174.aspx)<br />-   [Contrôleur de réseau](../../../sdn/technologies/network-controller/Network-Controller.md)|  
  


