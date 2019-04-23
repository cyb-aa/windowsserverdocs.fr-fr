---
title: Détails techniques de virtualisation réseau Hyper-V dans Windows Server 2016
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
ms.openlocfilehash: 8db47479d0c6e6014a7df6cecd59b1e87920b76a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887550"
---
# <a name="hyper-v-network-virtualization-technical-details-in-windows-server-2016"></a>Détails techniques de virtualisation réseau Hyper-V dans Windows Server 2016

>S’applique à :  Windows Server 2016

La virtualisation de serveur permet à plusieurs instances de serveur de s’exécuter simultanément sur un seul hôte physique. Pourtant, les instances de serveur sont isolées les unes par rapport aux autres. En substance, chaque ordinateur virtuel fonctionne comme s’il s’agissait du seul serveur à s’exécuter sur l’ordinateur physique.  
  
Virtualisation de réseau fournit une fonctionnalité similaire, dans quel plusieurs réseaux virtuels (avec éventuellement des adresses IP qui se chevauchent) s’exécuter sur la même infrastructure réseau physique et chaque réseau virtuel fonctionne comme s’il est le seul réseau virtuel en cours d’exécution l’infrastructure de réseau partagé. La figure 1 illustre cette relation.  
  
![virtualisation de serveur comparée à la virtualisation de réseau](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF1.gif)  
  
Figure 1 : virtualisation de serveur comparée à la virtualisation de réseau  
  
## <a name="hyper-v-network-virtualization-concepts"></a>Principes de la virtualisation de réseau Hyper-V  
Dans la virtualisation de réseau Hyper-V (HNV), un client ou un client est défini comme le « propriétaire » d’un ensemble de sous-réseaux IP qui sont déployés dans une entreprise ou d’un centre de données. Un client peut être une société ou entreprise avec plusieurs services ou divisions dans un centre de données privé qui nécessitent l’isolement réseau, ou un locataire dans un centre de données public qui est hébergé par un fournisseur de services. Chaque client peut avoir une ou plusieurs [réseaux virtuels](#VirtualNetworks) dans le centre de données et chaque virtuel réseau se compose d’un ou plusieurs [sous-réseaux virtuel](#VirtualSubnets).  
  
Il existe deux implémentations de HNV qui seront disponibles dans Windows Server 2016 : HNVv1 et HNVv2.  
  
-   **HNVv1**  
  
    HNVv1 est compatible avec Windows Server 2012 R2 et System Center 2012 R2 Virtual Machine Manager (VMM). Configuration pour HNVv1 s’appuie sur la gestion de WMI et des applets de commande Windows PowerShell (facilité via System Center VMM) pour définir les paramètres d’isolation et adresse de client (CA) - réseau virtuel - à des mappages d’adresse physique (PA) et le routage. Aucune des fonctionnalités supplémentaires n’ont été ajoutées à HNVv1 dans Windows Server 2016 et aucuns nouvelles fonctionnalités ne sont planifiées.  
    
    • Définissez l’association de cartes et HNV V1 ne sont pas compatibles par plateforme.
    
    o à utiliser les utilisateurs de passerelles à haute disponibilité NVGRE doive soit utiliser équipe LBFO ou aucune équipe. Ou
    
    passerelles d’utilisation : contrôleur de réseau déployé o avec ensemble associées commutateur.

  
-   **HNVv2**  
  
    Un nombre important de nouvelles fonctionnalités est inclus dans HNVv2 qui est implémentée à l’aide d’Azure virtuel filtrage plateforme (VFP) extension dans le commutateur Hyper-V de transfert. HNVv2 est entièrement intégré à Microsoft Azure Stack qui inclut le nouveau contrôleur de réseau dans la pile de mise en réseau SDN (Software Defined).  Stratégie de réseau virtuel est définie via le Microsoft [contrôleur de réseau](../../../sdn/technologies/network-controller/Network-Controller.md) à l’aide d’un RESTful NorthBound (NB) API et raccordés à un Agent hôte via plusieurs SouthBound aux interfaces (essai), y compris OVSDB. La stratégie de programmes de l’Agent hôte dans l’extension VFP du commutateur Hyper-V où elle est appliquée.  
  
    > [!IMPORTANT]  
    > Cette rubrique se concentre sur HNVv2.  
  
### <a name="VirtualNetworks"></a>Réseau virtuel  
  
-   Chaque réseau virtuel est constitué d’un ou plusieurs sous-réseaux virtuels. Un réseau virtuel constitue une limite d’isolation dans lequel les machines virtuelles au sein d’un réseau virtuel peuvent uniquement communiquer entre eux. En règle générale, cette isolation a été appliquée à l’aide de réseaux locaux virtuels avec une plage d’adresses IP séparée et 802. 1 q balise ou ID de VLAN. Mais, avec HNV, isolation est appliquée à l’aide de l’encapsulation NVGRE ou VXLAN pour créer des réseaux de superposition avec la possibilité de chevauchement des sous-réseaux IP entre les clients ou locataires.  
  
-   Chaque réseau virtuel a un ID de domaine routage (RDID) unique sur l’ordinateur hôte. Cette RDID mappe à peu près à un ID de ressource pour identifier la ressource REST dans le contrôleur de réseau du réseau virtuel. La ressource REST de réseau virtuel est référencé à l’aide d’un espace de noms d’identificateur de ressource uniforme (URI) avec l’ID de ressource ajoutée.  
  
### <a name="VirtualSubnets"></a>Sous-réseaux virtuels  
  
-   Un sous-réseau virtuel implémente la sémantique de sous-réseau IP de couche 3 pour les ordinateurs virtuels qui font partie d’un même sous-réseau virtuel. Le sous-réseau virtuel constitue un domaine de diffusion (semblable à un réseau local virtuel) et l’isolation est appliquée à l’aide du champ de l’ID de réseau client NVGRE (TNI) ou un identificateur de réseau VXLAN (VNI).  
  
-   Chaque sous-réseau virtuel appartient à un réseau virtuel unique (RDID), et il est assigné un unique sous-réseau virtuel ID (VSID) à l’aide de la TNI VNI touche ou dans l’en-tête de paquet encapsulé. Le VSID doit être unique dans le centre de données et est dans la plage de 4096 à 2 ^ 24-2.  
  
Des principaux avantages du réseau virtuel et du domaine de routage sont qu’il permet aux clients de porter leurs topologies réseau (par exemple, les sous-réseaux IP) vers le cloud. La figure 2 montre un exemple dans lequel Contoso Corp possède deux réseaux distincts : le réseau Recherche et développement (« R&D Net ») et le réseau commercial (« Sales Net »). Comme ces réseaux ont des ID de domaine de routage différents, ils ne peuvent pas interagir ensemble. Autrement dit, le réseau R&D Net Contoso est isolé du réseau Sales Net Contoso, alors même que les deux appartiennent à Contoso Corp. Contoso R&D Net contient trois sous-réseaux virtuels. Notez que les RDID et VSID sont uniques au sein d’un centre de données.  
  
![réseaux client et sous-réseaux virtuels](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF6.gif)  
  
Figure 2 : réseaux client et sous-réseaux virtuels  
  
**Transfert de couche 2**  
  
Dans la Figure 2, les machines virtuelles dans le VSID est 5001 peuvent voir leurs paquets transférés les ordinateurs virtuels qui figurent également dans le VSID est 5001 à travers le commutateur Hyper-V. Les paquets entrants à partir d’une machine virtuelle dans le VSID est 5001 sont envoyées à un VPort spécifique sur le commutateur Hyper-V. Règles d’entrée (par exemple, encap) et les mappages (par exemple, en-tête de l’encapsulation) sont appliquées par le commutateur Hyper-V pour ces paquets. Les paquets sont ensuite transmis à un autre VPort sur le commutateur Hyper-V (si cet ordinateur virtuel est attaché au même hôte) ou à un autre commutateur Hyper-V sur un autre hôte (si l’ordinateur virtuel de destination se trouve sur un autre hôte).  
  
**3 de routage de couche**  
  
De même, les machines virtuelles dans le VSID est 5001 peuvent voir leurs paquets routés aux machines virtuelles dans le VSID 5002 ou 5003 par le routeur distribué HNV qui est présent dans le commutateur virtuel de chaque hôte Hyper-V. Lors de remettre le paquet à Hyper-V commutateur, HNV met à jour le VSID du paquet entrant par rapport au VSID de l’ordinateur virtuel de destination. Cela ne se produit que si les deux VSID se trouvent dans le même RDID.  Par conséquent, les cartes réseau virtuelles associés à RDID1 ne peut pas envoyer des paquets aux cartes réseau virtuelles associées à RDID2 sans traverser une passerelle.  
  
> [!NOTE]  
> Dans la description du flux de paquet ci-dessus, le terme « ordinateur virtuel » signifie réellement la carte réseau virtuelle sur la machine virtuelle. Il est fréquent qu’un ordinateur virtuel n’ait qu’une seule carte réseau virtuelle. Dans ce cas, les mots « ordinateur virtuel » et « carte réseau virtuelle » peuvent signifier sur le plan conceptuel de la même chose.  
  
Chaque sous-réseau virtuel définit un sous-réseau IP de couche 3 et une limite de domaine de diffusion de couche 2 similaire à un VLAN. Lorsqu’un ordinateur virtuel diffuse un paquet, HNV utilise la réplication de monodiffusion (UR) pour effectuer une copie du paquet d’origine et remplacez l’adresse IP de destination et d’un MAC avec les adresses de chaque machine virtuelle qui se trouvent dans le même VSID.  
  
> [!NOTE]  
> Lorsque Windows Server 2016 est fourni, diffusion et le sous-réseau multidiffusions seront implémentées à l’aide de la réplication de monodiffusion. Le routage entre sous-réseaux multidiffusion et IGMP ne sont pas pris en charge.  
  
En plus d’être un domaine de diffusion, le VSID assure une isolation. Dans HNV, une carte de réseau virtuel est connectée à un port de commutateur Hyper-V qui les règles ACL appliquées directement au port (ressource REST virtualNetworkInterface) ou au sous-réseau virtuel (VSID) dont il fait partie.  
  
Le port de commutateur Hyper-V doit avoir une règle ACL appliquée. Cette liste ACL pourrait être autoriser ALL, DENY ALL, ou être plus précis pour autoriser uniquement certains types de trafic basée sur la correspondance de 5 tuples (IP Source, adresse IP de Destination, Port Source, Port de Destination, protocole).  
  
> [!NOTE]  
> Extensions de commutateur Hyper-V ne fonctionne pas avec HNVv2 dans la nouvelle pile de mise en réseau SDN (Software Defined). HNVv2 est implémenté à l’aide de l’extension de commutateur de plateforme de filtrage virtuel Azure (VFP) ne peuvent pas être utilisés conjointement avec une autre extension de commutateur de 3 rd-party.  
  
## <a name="switching-and-routing-in-hyper-v-network-virtualization"></a>Basculement et le routage de la virtualisation de réseau Hyper-V  
HNVv2 implémente la sémantique de routage de couche 3 (L3) fonctionne comme un commutateur physique et de commutation de couche 2 (L2) correct ou routeur fonctionnerait. Lorsqu’un ordinateur virtuel est connecté à un réseau virtuel HNV tente d’établir une connexion avec une autre machine virtuelle dans le même sous-réseau virtuel (VSID) il devez d’abord connaître l’adresse MAC de l’autorité de certification de la machine virtuelle à distance. S’il existe une entrée ARP pour l’adresse IP de la machine virtuelle de destination dans la table ARP de l’ordinateur virtuel source, l’adresse MAC à partir de cette entrée est utilisé. Si une entrée n’existe pas, l’ordinateur virtuel source enverra qu'un ARP de diffusion avec une demande pour l’adresse MAC correspondant à l’adresse IP de la machine virtuelle de destination doit être retourné. Le commutateur Hyper-V intercepte cette demande et envoyer à l’Agent hôte. L’Agent hôte recherchera dans sa base de données local pour une adresse MAC correspondante pour l’adresse IP de l’ordinateur virtuel de destination demandée.  
  
> [!NOTE]  
> L’Agent hôte, en tant que le serveur OVSDB, utilise une variante du schéma VTEP pour stocker les mappages de CA-PA, MAC table et ainsi de suite.  
  
Si une adresse MAC est disponible, l’Agent hôte injecte une réponse ARP et renvoie à la machine virtuelle. Une fois que la pile de mise en réseau de la machine virtuelle a toutes les informations d’en-tête L2 requises, la trame est envoyée au Port Hyper-V correspondant sur le commutateur-V. En interne, le commutateur Hyper-V teste ce frame par rapport à des règles de correspondance de N-tuple affecté au Port-V et applique certaines transformations vers le frame selon ces règles. Plus important encore, un ensemble de transformations d’encapsulation est appliqué pour construire l’en-tête d’encapsulation à l’aide de NVGRE ou VXLAN, selon la stratégie définie au niveau du contrôleur de réseau. Selon la stratégie programmée par l’Agent hôte, un mappage de l’autorité de certification-PA est utilisé pour déterminer l’adresse IP de l’hôte Hyper-V où se trouve la machine virtuelle de destination. Le commutateur Hyper-V garantit que les règles de routage corrects et balises VLAN sont appliqués au paquet externe pour qu’il atteigne l’adresse PA à distance.  
  
Si un ordinateur virtuel connecté à un réseau virtuel HNV souhaite créer une connexion avec une machine virtuelle dans un autre sous-réseau virtuel (VSID), le paquet doit être routé en conséquence. HNV suppose une topologie en étoile où il existe une seule adresse IP dans l’espace d’adressage utilisé en tant que tronçon suivant pour atteindre tous les préfixes d’adresse IP (ce qui signifie une valeur par défaut/la passerelle de routage). Actuellement, cela permet d’appliquer une limitation à un seul itinéraire par défaut et les itinéraires par défaut ne sont pas pris en charge.  
  
### <a name="routing-between-virtual-subnets"></a>Routage entre des sous-réseaux virtuels  
Dans un réseau physique, un sous-réseau IP est un domaine de couche 2 (L2) où les ordinateurs (virtuels et physiques) peuvent communiquer directement entre eux. Le domaine de niveau 2 est un domaine de diffusion où entrées ARP (mappage d’adresses IP:MAC) sont apprises lors des requêtes ARP qui sont diffusés sur toutes les interfaces et les réponses ARP sont renvoyées à l’hôte demandeur. L’ordinateur utilise les informations de MAC apprises à partir de la réponse ARP pour construire complètement le frame de niveau 2, y compris les en-têtes Ethernet. Toutefois, si une adresse IP est dans un autre sous-réseau L3, la demande ARP ne franchit pas cette limite L3. Au lieu de cela, une interface L3 de routeur (passerelle par défaut ou de saut suivant) avec une adresse IP du sous-réseau source doit répondre à ces requêtes ARP avec sa propre adresse MAC.  
  
Dans un réseau Windows standard, un administrateur peut créer des itinéraires statiques et l’attribuer à une interface réseau. En outre, une « passerelle par défaut » est généralement configurée pour être l’adresse IP de saut suivant sur une interface où sont envoyés les paquets destinés à l’itinéraire par défaut (0.0.0.0/0). Paquets sont envoyés à cette passerelle par défaut si aucun itinéraire spécifique n’existe. Il s’agit en général du routeur pour votre réseau physique.  HNV utilise un routeur intégré qui fait partie de chaque hôte et possède une interface dans chaque VSID pour créer un routeur distribué pour l’ou les réseaux virtuel.  
  
Dans la mesure où HNV suppose une topologie en étoile, le routeur distribué HNV fait Office de passerelle par défaut unique pour tout le trafic qui transite entre les sous-réseaux virtuels qui font partie du même réseau VSID. L’adresse en tant que les valeurs par défaut de la passerelle par défaut à l’adresse IP la plus basse dans le VSID et est affectée au routeur distribué HNV. Ce routeur distribué permet d’acheminer correctement, car chaque hôte peut acheminer directement le trafic vers l’hôte approprié sans intermédiaire un moyen très efficace pour tout le trafic à l’intérieur d’un réseau de VSID.  Cela est particulièrement vrai lorsque deux ordinateurs virtuels du même réseau d’ordinateurs virtuels, mais de différents sous-réseaux virtuels, se trouvent sur le même hôte physique.  Comme vous le verrez plus loin dans cette section, le paquet ne doit jamais quitter l’hôte physique.  
  
### <a name="routing-between-pa-subnets"></a>Routage entre les sous-réseaux de PA  
Contrairement aux HNVv1 qui alloués à une adresse IP de PA pour chaque sous-réseau virtuel (VSID), HNVv2 utilise désormais une adresse IP de PA par membre de l’équipe carte réseau Switch-Embedded Teaming (SET). Le déploiement par défaut suppose une équipe de deux cartes d’interface réseau et affecte les deux adresses IP fournisseur par hôte. Un seul hôte a PA des adresses IP affectées à partir du même sous-réseau logique de fournisseur (PA) sur le même réseau local virtuel. Deux machines virtuelles du locataire dans le même sous-réseau virtuel peut en effet se trouver sur deux hôtes différents qui sont connectés à deux sous-réseaux logique autre fournisseur. HNV construira les en-têtes d’adresse IP externes du paquet encapsulé en fonction du mappage de l’autorité de certification-PA. Toutefois, il s’appuie sur la pile TCP/IP hôte à ARP passerelle par défaut PA et puis génère les en-têtes Ethernet externes en fonction de la réponse ARP. En règle générale, cette réponse ARP provient de l’interface SVI sur le commutateur physique ou le routeur de L3 où l’hôte est connecté. HNV s’appuie donc sur le routeur L3 pour le routage les paquets encapsulés entre les sous-réseaux de logique de fournisseur / VLAN.  
  
### <a name="routing-outside-a-virtual-network"></a>Routage hors d’un réseau virtuel  
La plupart des déploiements clients nécessitent une communication entre l’environnement HNV et les ressources qui ne font pas partie de ce même environnement. Les passerelles de virtualisation de réseau sont nécessaires pour permettre la communication entre les deux environnements. Les infrastructures qui nécessite une passerelle HNV incluent le nuage privé et Cloud hybride. En fait, les passerelles HNV sont requis pour le routage de couche 3 entre les réseaux internes et externes (physiques) (y compris les NAT) ou entre différents sites et/ou clouds (privés ou publics) qui utilisent un tunnel VPN IPSec ou GRE.  
  
Les passerelles peuvent présenter différents facteurs de forme physique. Elles peuvent reposer sur Windows Server 2016, intégrée à un commutateur TOR Top of Rack () agissant comme une passerelle VXLAN, accessible via une IP virtuelle (VIP) publiés par un équilibreur de charge put à d’autres dispositifs réseau existants ou peut être un nouveau dispositif réseau autonome .  
  
Pour plus d’informations sur les options de la passerelle RAS de Windows, consultez [passerelle RAS](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md).  
  
## <a name="packet-encapsulation"></a>Encapsulation de paquet  
Dans HNV, chaque carte réseau virtuelle est associée à deux adresses IP :  
  
-   **Adresse du client** (CA) l’adresse IP assignée par le client, en fonction de son infrastructure intranet. Cette adresse permet au client d’échanger le trafic réseau avec la machine virtuelle comme s’il n’avait pas été transféré vers un cloud public ou privé. L’adresse client est visible de l’ordinateur virtuel et accessible au client.  
  
-   **Adresse du fournisseur** (PA) l’adresse IP attribuée par le fournisseur d’hébergement ou les administrateurs du centre de données en fonction de leur infrastructure réseau physique. L’adresse fournisseur figure dans les paquets du réseau qui sont échangés avec le serveur exécutant Hyper-V qui héberge l’ordinateur virtuel. Cette adresse est visible sur le réseau physique, mais pas sur l’ordinateur virtuel.  
  
Les adresses client préservent la topologie réseau du client, qui est virtualisée et dissociée des adresses et de la topologie du réseau physique sous-jacent, conformément à l’implémentation des adresses fournisseur. Le diagramme suivant illustre la relation conceptuelle entre les adresses client des ordinateurs virtuels et les adresses fournisseur d’une infrastructure réseau à la suite d’une virtualisation de réseau.  
  
![diagramme conceptuel d’une virtualisation de réseau sur une infrastructure physique](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF2.gif)  
  
Figure 6 : diagramme conceptuel d’une virtualisation de réseau sur une infrastructure physique  
  
Dans le diagramme, les machines virtuelles clientes envoient des paquets de données dans l’espace de l’autorité de certification, qui transitent par l’infrastructure réseau physique via leurs propres réseaux virtuels, ou « tunnels ». Dans l’exemple ci-dessus, les tunnels peuvent être considérés comme des « enveloppes » pour les paquets de données Contoso et Fabrikam disposant vert étiquettes d’expédition (adresses fournisseur) doivent être remises par l’hôte source sur la gauche à l’hôte de destination sur la droite. La clé est la façon dont les hôtes déterminent les « adresses d’expédition » (PA) correspondant à Contoso et l’autorité de certification Fabrikam, comment il est placé le « enveloppe » englobe les paquets et comment les hôtes de destination peuvent déballer les paquets et fournir à Contoso et Fabrikam destination virtual machines correctement.  
  
Cette analogie simple a mis en évidence les principaux aspects de la virtualisation de réseau :  
  
-   Chaque adresse client d’ordinateur virtuel est mappée à une adresse fournisseur d’hôte physique. Plusieurs adresses client peuvent être associées à la même adresse fournisseur.  
  
-   Machines virtuelles envoyer des paquets de données dans les espaces de l’autorité de certification, qui sont placés dans une « enveloppe » avec une paire de PA source et de destination en fonction du mappage.  
  
-   Les mappages entre adresses client et fournisseur doivent permettre aux hôtes de distinguer les paquets à destination des différents ordinateurs virtuels client.  
  
Par conséquent, la virtualisation d’un réseau consiste à virtualiser les adresses réseau utilisées par les ordinateurs virtuels. Le contrôleur de réseau est chargé pour le mappage d’adresse et l’agent hôte gère la base de données de mappage en utilisant le schéma MS_VTEP. La section suivante décrit les mécanismes réels de la virtualisation d’adresses.  
  
## <a name="network-virtualization-through-address-virtualization"></a>La virtualisation de réseau via la virtualisation d’adresses  
HNV implémente superpose des réseaux de clients à l’aide de réseau virtualisation générique Encapsulation routage (NVGRE) ou le réseau local eXtensible virtuelle (VXLAN).  VXLAN est la valeur par défaut.  
  
### <a name="virtual-extensible-local-area-network-vxlan"></a>Virtual eXtensible LAN VXLAN)  
Le réseau local eXtensible virtuelle (VXLAN) ([RFC 7348](https://www.rfc-editor.org/info/rfc7348)) protocole a été largement adopté sur le marché, avec prise en charge des fournisseurs comme Cisco, Brocade, Arista, Dell, HP et autres. Le protocole VXLAN utilise le protocole UDP comme transport. Le port de destination UDP affectée par l’IANA pour VXLAN est 4789 et le port UDP source doit être un hachage des informations à partir du paquet interne à utiliser pour le routage ECMP propagation. Après l’en-tête UDP, un en-tête VXLAN est ajouté au paquet qui inclut un champ réservé de 4 octets suivi d’un champ de 3 octets pour le VXLAN réseau identificateur (VNI) - VSID - suivie d’un autre champ réservé de 1 octet. Après l’en-tête VXLAN, le frame de niveau 2 d’autorité de certification d’origine (sans la trame Ethernet de l’autorité de certification FCS) est ajouté.  
  
![En-tête de paquet VXLAN](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VXLAN-packet-header.png)  
  
### <a name="generic-routing-encapsulation-nvgre"></a>Encapsulation générique de routage (NVGRE)  
Ce mécanisme de virtualisation de réseau utilise l’Encapsulation de routage générique (NVGRE) dans le cadre de l’en-tête de tunnel. Dans NVGRE, les paquets de la machine virtuelle sont encapsulé dans un autre paquet. L’en-tête de ce nouveau paquet contient les adresses IP fournisseur sources et de destination appropriées, outre l’ID de sous-réseau virtuel, qui est stocké dans le champ de clé de l’en-tête GRE, comme le montre la figure 7.  
  
![Encapsulation NVGRE](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF3.gif)  
  
Figure 7 : virtualisation de réseau - encapsulation NVGRE  
  
L’ID de sous-réseau virtuel permet aux hôtes d’identifier l’ordinateur virtuel client pour un paquet donné, même si les adresses fournisseur et l’autorité de certification de paquets peuvent se chevaucher. De ce fait, tous les ordinateurs virtuels d’un même hôte peuvent partager une adresse fournisseur unique, comme l’illustre la figure 7.  
  
La partage de l’adresse fournisseur a des conséquences importantes sur l’extensibilité réseau. Le nombre d’adresses IP et MAC que l’infrastructure réseau doit mémoriser s’en trouve considérablement réduit. Par exemple, si chaque hôte final compte en moyenne 30 ordinateurs virtuels, le nombre d’adresses IP et MAC que doit mémoriser l’infrastructure réseau est réduit selon un facteur de 30. Les ID de sous-réseau virtuel incorporés dans les paquets permettent en outre une corrélation facile entre les paquets et les clients effectifs.  
  
Le schéma de partage pour Windows Server 2012 R2 de PA est un PA par VSID par hôte. Pour Windows Server 2016, le schéma est un PA par membre d’équipe de cartes réseau.  
  
Avec Windows Server 2016 et versions ultérieures, HNV prend entièrement en charge NVGRE et VXLAN prêts à l’emploi ; Il ne nécessite pas la mise à niveau ou acquisition de nouveau matériel réseau telles que les cartes réseau, commutateurs ou routeurs. Il s’agit, car ces paquets sur le câble sont paquet IP normal dans l’espace d’adressage fournisseur, qui est compatible avec l’infrastructure de réseau d’aujourd'hui.  Toutefois, pour obtenir le meilleur parti de performances pris en charge cartes d’interface réseau avec les derniers pilotes qui prennent en charge des déchargements de tâche.  
  
## <a name="multi-tenant-deployment-example"></a>exemple de déploiement mutualisé  
Le diagramme suivant montre un exemple de déploiement de deux clients situés dans un centre de données cloud avec l’autorité de certification et où la relation définie par les stratégies de réseau.  
  
![exemple de déploiement mutualisé](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF5.png)  
  
Figure 8 : exemple de déploiement mutualisé  
  
Examinez l’exemple illustré dans la figure 8. Avant d’utiliser le service IaaS partagé du fournisseur d’hébergement :  
  
-   Contoso Corp a exécuté un serveur SQL Server (nommé **SQL**) à l’adresse IP 10.1.1.11 et un serveur Web (nommé **Web**) à l’adresse IP 10.1.1.12, qui utilise son serveur SQL Server pour les transactions de base de données.  
  
-   Fabrikam Corp a exécuté un serveur SQL Server, également nommé **SQL** et a assigné l’adresse IP 10.1.1.11, ainsi qu’un serveur Web, également nommé **Web** avec la même adresse IP 10.1.1.12, qui utilise son serveur SQL Server pour les transactions de base de données.  
  
Nous allons supposer que le fournisseur de services a créé précédemment le réseau logique de fournisseur (PA) via le contrôleur de réseau pour qu’elles correspondent à leur topologie réseau physique. Le contrôleur de réseau alloue deux adresses IP fournisseur à partir de préfixe d’adresse IP du sous-réseau logique où les ordinateurs hôtes sont connectés. Le contrôleur de réseau indique également la balise VLAN appropriée pour appliquer les adresses IP.  
  
À l’aide du réseau de contrôleur, Contoso Corp et Fabrikam Corp puis créer leur réseau virtuel et les sous-réseaux qui reposent sur le réseau logique de fournisseur (PA) spécifié par le fournisseur de services. Contoso Corp et Fabrikam Corp transfèrent leurs serveurs SQL Server et Web respectifs sur le service IaaS partagé du même fournisseur d’hébergement où, par coïncidence, elles exécutent les ordinateurs virtuels **SQL** sur l’hôte 1 Hyper-V et les ordinateurs virtuels **Web** (IIS7) sur l’hôte 2 Hyper-V. Tous les ordinateurs virtuels conservent leurs adresses IP (client) intranet d’origine.  
  
Les deux sociétés se voient assigner à l’ID suivant de sous-réseau virtuel (VSID) par le contrôleur de réseau comme indiqué ci-dessous.  L’Agent hôte sur chacun des hôtes Hyper-V reçoit les adresses IP fournisseur affectées à partir du contrôleur de réseau et crée deux cartes réseau virtuelles de hôte PA dans un compartiment réseau non-par défaut. Une interface réseau est assignée à chacun de ces cartes réseau virtuelles hôtes où l’adresse IP de PA est affecté comme indiqué ci-dessous :  
  
-   Machines virtuelles de Contoso Corp VSID et les adresses fournisseur : **VSID** est 5001, **SQL PA** est 192.168.1.10 et l’adresse **Web PA** est 192.168.2.20  
  
-   Ordinateurs virtuels de Fabrikam Corp VSID et les adresses fournisseur : **VSID** est 6001, l’adresse **SQL PA** est 192.168.1.10 et l’adresse **Web PA** est 192.168.2.20  
  
Le contrôleur de réseau il ajoute toutes les stratégies de réseau (y compris le mappage de l’autorité de certification-PA) à l’Agent hôte de SDN qui gère la stratégie dans un magasin persistant (dans les tables de base de données OVSDB).  
  
Lorsque la machine virtuelle de Contoso Corp Web (10.1.1.12) sur l’hôte 2 Hyper-V crée une connexion TCP à SQL Server à l’adresse 10.1.1.11, procédez comme suit :  
  
-   ARP de machine virtuelle pour l’adresse MAC de 10.1.1.11 destination  
  
-   L’extension VFP dans le commutateur virtuel intercepte ce paquet et l’envoie à l’Agent hôte SDN  
  
-   Recherche de l’Agent hôte de SDN dans son magasin de stratégies pour l’adresse MAC pour 10.1.1.11  
  
-   Si un MAC est trouvé, l’Agent hôte injecte une réponse ARP à la machine virtuelle  
  
-   Si un MAC n’est trouvé, aucune réponse n’est envoyée et l’entrée ARP dans la machine virtuelle pour 10.1.1.11 est marquée inaccessible.  
  
-   La machine virtuelle est désormais construit un paquet TCP avec les en-têtes Ethernet d’autorité de certification et l’adresse IP correctes et envoie au commutateur virtuel  
  
-   L’extension de commutateur virtuel de transfert de VFP traite ce paquet à travers les couches VFP (décrits ci-dessous) assigné au port de commutateur virtuel source sur lequel le paquet a été reçu et crée une nouvelle entrée de flux dans la table de flux unifiée VFP  
  
-   Le moteur VFP effectue la recherche de correspondance ou de la table de flux de règle pour chaque couche (par exemple, couche de réseau virtuel) basée sur les en-têtes d’adresse IP et Ethernet.  
  
-   La règle de mise en correspondance dans la couche de réseau virtuel fait référence à un espace de mappage CA-PA et effectue l’encapsulation.  
  
-   Le type d’encapsulation (VXLAN ou NVGRE) est spécifié dans la couche de réseau virtuel, ainsi que le VSID.  
  
-   Dans le cas d’encapsulation VXLAN, un en-tête UDP externe est construit avec le VSID 5001 dans l’en-tête VXLAN.  
    Un en-tête d’adresse IP externe est construit avec l’adresse PA source et de destination affecté à l’hôte Hyper-V, 2 (192.168.2.20) et l’hôte Hyper-V 1 (192.168.1.10) respectivement en fonction de magasin de stratégies de l’Agent hôte SDN.  
  
-   Ce paquet passe ensuite à la couche de routage PA dans VFP.  
  
-   La couche de routage PA dans VFP référence le compartiment réseau utilisé pour le trafic de l’espace d’adressage fournisseur et un ID de VLAN et utiliser la pile TCP/IP de l’hôte pour transférer le paquet PA à l’hôte 1 Hyper-V correctement.  
  
-   Lors de la réception du paquet encapsulé, hôte 1 Hyper-V reçoit le paquet dans le compartiment réseau PA et le transférer au commutateur virtuel.  
  
-   L’architecture VFP traite le paquet via ses couches VFP et créez une nouvelle entrée de flux dans la table de flux unifiée VFP.  
  
-   Le moteur VFP correspond aux règles ingres dans la couche de réseau virtuel et supprime les en-têtes Ethernet, adresse IP et VXLAN du paquet encapsulé externe.  
  
-   Le moteur VFP transfère ensuite le paquet vers le port de commutateur virtuel auquel la machine virtuelle de destination est connecté.  
  
Un processus similaire pour le trafic entre les machines virtuelles Fabrikam Corp **Web** et **SQL** utilise les paramètres de stratégie HNV pour Fabrikam Corp. Par conséquent, avec HNV, les machines virtuelles Fabrikam Corp et Contoso Corp interagissent comme si elles étaient sur leurs intranets d’origine. Ils ne peuvent jamais interagir entre eux, même s’ils utilisent les mêmes adresses IP.  
  
Les adresses distinctes (autorités de certification et les adresses fournisseur), les paramètres de stratégie des hôtes Hyper-V et la traduction d’adresses entre l’autorité de certification et de fournisseur pour le trafic entrant et sortant machine virtuelle isolent ces ensembles de serveurs à l’aide de la clé NVGRE ou le VNID VLXAN. De plus, les mappages et la transformation de la virtualisation dissocient l’architecture de réseau virtuel de l’infrastructure de réseau physique. Bien que les serveurs **SQL** et **Web** Contoso et les serveurs **SQL** et **Web** Fabrikam résident dans leurs propres sous-réseaux IP d’adresses client (10.1.1/24), leur déploiement physique se produit sur deux hôtes situés dans des sous-réseaux d’adresses fournisseur différents, 192.168.1/24 et 192.168.2/24, respectivement. Cela implique que l’approvisionnement et la migration dynamique des ordinateurs virtuels entre sous-réseaux devient possible avec HNV.  
  
## <a name="hyper-v-network-virtualization-architecture"></a>Architecture de la virtualisation de réseau Hyper-V  
Dans Windows Server 2016, HNVv2 est implémentée à l’aide d’Azure virtuel filtrage plateforme (VFP) qui est une extension de filtrage NDIS au sein du commutateur Hyper-V. Le concept clé de VFP est celle d’un moteur de flux de correspondance-Action avec une API interne exposée à l’Agent hôte de SDN pour la programmation de stratégie réseau. L’Agent hôte SDN lui-même reçoit la stratégie de réseau à partir du contrôleur de réseau sur les canaux de communication OVSDB et WCF SouthBound. Est non seulement la stratégie de réseau virtuel (par exemple, mappage de l’autorité de certification-PA) programmée à l’aide de VFP mais des stratégies supplémentaires comme les ACL, QoS et ainsi de suite.  
  
La hiérarchie d’objets pour le commutateur virtuel et l’extension de transfert de VFP est la suivante :  
  
-   vSwitch  
  
    -   Gestion de la carte réseau externe  
  
    -   Déchargements de matériel de carte réseau  
  
    -   Règles de transfert globales  
  
    -   Port  
  
        -   Sortie de transfert de couche pour l’épinglage de cheveux  
  
        -   Listes d’espace pour les mappages et les pools NAT  
  
        -   Table de flux unifiée  
  
        -   Couche VFP  
  
            -   Table de flux  
  
            -   Regrouper  
  
            -   Règle  
  
                -   Les règles peuvent référencer des espaces  
  
Dans l’architecture VFP, une couche est créée par le type de stratégie (par exemple, le réseau virtuel) et est un ensemble générique de tables de la règle/flux. Il ne dispose pas de fonctionnalités intrinsèque jusqu'à ce que les règles spécifiques sont affectées à cette couche pour implémenter ces fonctionnalités. Une priorité est attribuée à chaque couche et couches sont affectées à un port par ordre croissant de priorité. Les règles sont organisées en groupes basées essentiellement sur la direction et la famille d’adresses IP. Les groupes sont également affectés une priorité et au maximum, une règle à partir d’un groupe peut correspondre à un flux donné.  
  
La logique de transfert pour le commutateur virtuel avec l’extension VFP est comme suit :  
  
-   Traitement d’entrée (entrée à partir du point de vue du paquet entrant dans un port)  
  
-   Transfert  
  
-   Traitement de sortie (en sortie à partir du point de vue de paquet quittant un port)  
  
L’architecture VFP prend en charge le transfert MAC interne pour les types d’encapsulation NVGRE et VXLAN, ainsi que le transfert réseau local virtuel MAC en externe.  
  
L’extension VFP a un chemin d’accès lent et le chemin d’accès rapide pour la traversée du paquet. Le premier paquet dans un flux doit parcourir tous les groupes de règles dans chaque couche et faire une règle de recherche qui est une opération coûteuse. Toutefois, une fois qu’un flux est inscrite dans la table de flux unifiée avec une liste d’actions (selon les règles de mise en correspondance) tous les paquets suivants seront traités selon les entrées de table de flux unifiée.  
  
Stratégie HNV est programmé par l’agent hôte. Chaque carte réseau d’ordinateur virtuel est configuré avec une adresse IPv4. Il s’agit des adresses client dont se serviront les ordinateurs virtuels pour communiquer entre eux et qui sont transportées dans les paquets IP en provenance des ordinateurs virtuels. HNV encapsule le frame de l’autorité de certification dans un frame PA basé sur les stratégies de virtualisation de réseau stockées dans la base de données de l’agent hôte.  
  
![Architecture HNV](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF7.png)  
  
Figure 9 : Architecture HNV  
  
## <a name="summary"></a>Récapitulatif  
Les centres de données en nuage peuvent apporter de nombreux avantages, notamment une extensibilité améliorée et une meilleure utilisation des ressources. Pour profiter de ces avantages potentiels, il est nécessite de faire appel à une technologie à même de résoudre fondamentalement les problèmes de l’extensibilité mutualisée dans un environnement dynamique. La virtualisation HNV a été conçue pour traiter ces problèmes et également pour améliorer l’efficacité des opérations du centre de données en dissociant la topologie du réseau virtuel de la topologie du réseau physique. S’appuyant sur une norme existante, HNV s’exécute dans Centre de données votre et fonctionne avec votre infrastructure VXLAN existante. Avec HNV, les clients peuvent désormais consolider leurs centres de données dans un cloud privé ou étendre de manière transparente leurs centres de données à l’environnement d’un fournisseur serveur d’hébergement avec un cloud hybride.  
  
## <a name="BKMK_LINKS"></a>Voir aussi  
Pour en savoir plus sur HNVv2, consultez les liens suivants :  
  
|Type de contenu|Références|  
|----------------|--------------|  
|**Ressources de la Communauté**|-   [Blog de l’Architecture de Cloud privé](https://blogs.technet.com/b/privatecloud)<br />-Poser des questions : [cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)|  
|**RFC**|-   [RFC préliminaire sur NVGRE](https://www.ietf.org/id/draft-sridharan-virtualization-nvgre-07.txt)<br />-   [VXLAN - RFC 7348](https://www.rfc-editor.org/info/rfc7348)|  
|**Technologies associées**|-Pour les détails techniques de virtualisation de réseau Hyper-V dans Windows Server 2012 R2, voir [détails techniques de virtualisation de réseau Hyper-V](https://technet.microsoft.com/library/jj134174.aspx)<br />-   [Contrôleur de réseau](../../../sdn/technologies/network-controller/Network-Controller.md)|  
  


