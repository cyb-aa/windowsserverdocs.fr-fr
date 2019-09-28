---
title: Détails techniques sur la virtualisation de réseau Hyper-V dans Windows Server 2016
description: Cette rubrique fournit des informations techniques sur la virtualisation de réseau Hyper-V dans Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.service: virtual-network
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9efe0231-94c1-4de7-be8e-becc2af84e69
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e692384e9416e21e00556af6ada9af8df1713a03
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405858"
---
# <a name="hyper-v-network-virtualization-technical-details-in-windows-server-2016"></a>Détails techniques sur la virtualisation de réseau Hyper-V dans Windows Server 2016

>S’applique à :  Windows Server 2016

La virtualisation de serveur permet à plusieurs instances de serveur de s’exécuter simultanément sur un seul hôte physique. Pourtant, les instances de serveur sont isolées les unes par rapport aux autres. En substance, chaque ordinateur virtuel fonctionne comme s’il s’agissait du seul serveur à s’exécuter sur l’ordinateur physique.  

La virtualisation de réseau offre une fonctionnalité similaire, dans laquelle plusieurs réseaux virtuels (potentiellement avec des adresses IP qui se chevauchent) s’exécutent sur la même infrastructure de réseau physique et chaque réseau virtuel fonctionne comme s’il s’agissait du seul réseau virtuel exécuté sur l’infrastructure réseau partagée. La figure 1 illustre cette relation.  

![virtualisation de serveur comparée à la virtualisation de réseau](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF1.gif)  

Figure 1 : virtualisation de serveur comparée à la virtualisation de réseau  

## <a name="hyper-v-network-virtualization-concepts"></a>Principes de la virtualisation de réseau Hyper-V  
Dans la virtualisation de réseau Hyper-V (HNV), un client ou un locataire est défini comme le « propriétaire » d’un ensemble de sous-réseaux IP qui sont déployés dans une entreprise ou un centre de donnes. Un client peut être une société ou une entreprise avec plusieurs services ou divisions dans un centre de données privé qui nécessite l’isolement réseau, ou un locataire dans un centre de données public qui est hébergé par un fournisseur de services. Chaque client peut disposer d’un ou plusieurs [réseaux virtuels](#VirtualNetworks) dans le centre de donnes, et chaque réseau virtuel se compose d’un ou de plusieurs [sous-réseaux virtuels](#VirtualSubnets).  

Il existe deux implémentations de HNV qui seront disponibles dans Windows Server 2016 : HNVv1 et HNVv2.  

-   **HNVv1**  

    HNVv1 est compatible avec Windows Server 2012 R2 et System Center 2012 R2 Virtual Machine Manager (VMM). La configuration de HNVv1 s’appuie sur la gestion WMI et les applets de commande Windows PowerShell (facilitées par le biais de System Center VMM) pour définir les paramètres d’isolation et l’adresse du client (CA)-mappages et routage de réseau virtuel vers adresse physique (PA). Aucune fonctionnalité supplémentaire n’a été ajoutée à HNVv1 dans Windows Server 2016 et aucune nouvelle fonctionnalité n’est planifiée.  

    • SET Teaming et HNV v1 ne sont pas compatibles par la plateforme.

    o pour utiliser des passerelles NVGRE HA, les utilisateurs doivent utiliser l’équipe LBFO ou aucune équipe. Ou

    o utilise des passerelles déployées par le contrôleur de réseau avec SET associationd Switch.


-   **HNVv2**  

    Un nombre important de nouvelles fonctionnalités sont incluses dans HNVv2, qui est implémenté à l’aide de l’extension de transfert de la plateforme de filtrage virtuel Azure (VFP) dans le commutateur Hyper-V. HNVv2 est entièrement intégré à Microsoft Azure Stack qui comprend le nouveau contrôleur de réseau dans la pile SDN (Software Defined Networking).  La stratégie de réseau virtuel est définie via le [contrôleur de réseau](../../../sdn/technologies/network-controller/Network-Controller.md) Microsoft à l’aide d’une API RESTful Northbound (NB) et montée sur un agent hôte via plusieurs Southbound INTEFACES (SBI), y compris OVSDB. La stratégie des programmes de l’agent hôte dans l’extension VFP du commutateur Hyper-V où elle est appliquée.  

    > [!IMPORTANT]  
    > Cette rubrique se concentre sur HNVv2.  

### <a name="VirtualNetworks"></a>Réseau virtuel  

-   Chaque réseau virtuel se compose d’un ou de plusieurs sous-réseaux virtuels. Un réseau virtuel constitue une limite d’isolation dans laquelle les machines virtuelles d’un réseau virtuel peuvent communiquer entre elles. Traditionnellement, cette isolation a été appliquée à l’aide de réseaux locaux virtuels avec une plage d’adresses IP séparée et une balise 802.1 q ou un ID de réseau local virtuel. Toutefois, avec HNV, l’isolation est appliquée à l’aide de l’encapsulation NVGRE ou VXLAN pour créer des réseaux de superposition avec la possibilité de chevaucher des sous-réseaux IP entre les clients ou les locataires.  

-   Chaque réseau virtuel a un ID de domaine de routage unique (RDID) sur l’ordinateur hôte. Ce RDID est à peu près mappé à un ID de ressource pour identifier la ressource REST de réseau virtuel dans le contrôleur de réseau. La ressource REST de réseau virtuel est référencée à l’aide d’un espace de noms Uniform Resource Identifier (URI) avec l’ID de ressource ajouté.  

### <a name="VirtualSubnets"></a>Sous-réseaux virtuels  

-   Un sous-réseau virtuel implémente la sémantique de sous-réseau IP de couche 3 pour les ordinateurs virtuels qui font partie d’un même sous-réseau virtuel. Le sous-réseau virtuel forme un domaine de diffusion (semblable à un réseau local virtuel) et l’isolation est appliquée à l’aide du champ NVGRE TNI (ID réseau du locataire) ou VXLAN (VNI).  

-   Chaque sous-réseau virtuel appartient à un seul réseau virtuel (RDID) et un ID de sous-réseau virtuel unique est attribué à l’aide de la clé TNI ou VNI dans l’en-tête de paquet encapsulé. La valeur de l’identification de la page doit être unique dans le centre de centres et se situer dans la plage comprise entre 4096 et 2 ^ 24-2.  

L’un des principaux avantages du réseau virtuel et du domaine de routage est qu’il permet aux clients d’apporter leurs propres topologies de réseau (par exemple, des sous-réseaux IP) au Cloud. La figure 2 montre un exemple dans lequel Contoso Corp possède deux réseaux distincts : le réseau Recherche et développement (« R&D Net ») et le réseau commercial (« Sales Net »). Comme ces réseaux ont des ID de domaine de routage différents, ils ne peuvent pas interagir ensemble. Autrement dit, le réseau R&D Net Contoso est isolé du réseau Sales Net Contoso, alors même que les deux appartiennent à Contoso Corp. Contoso R&D Net contient trois sous-réseaux virtuels. Notez que les RDID et VSID sont uniques au sein d’un centre de données.  

![réseaux client et sous-réseaux virtuels](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF6.gif)  

Figure 2 : réseaux client et sous-réseaux virtuels  

**Transfert de couche 2**  

Dans la figure 2, les ordinateurs virtuels de l’ordinateur virtuel 2 5001 peuvent avoir leurs paquets transférés vers des ordinateurs virtuels qui se trouvent également dans le port de la machine virtuelle 5001 via le commutateur Hyper-V. Les paquets entrants à partir d’un ordinateur virtuel dans la version 5001 de la machine virtuelle sont envoyés à un VPort spécifique sur le commutateur Hyper-V. Les règles d’entrée (par exemple, encap) et les mappages (par exemple, l’en-tête d’encapsulation) sont appliqués par le commutateur Hyper-V pour ces paquets. Les paquets sont ensuite transférés vers un autre VPort sur le commutateur Hyper-V (si la machine virtuelle de destination est attachée au même hôte) ou à un commutateur Hyper-V différent sur un autre ordinateur hôte (si la machine virtuelle de destination se trouve sur un autre ordinateur hôte).  

**Routage de couche 3**  

De même, les machines virtuelles de la version de l’ordinateur virtuel 5001 peuvent avoir leurs paquets routés vers des machines virtuelles de la machine virtuelle 5002 ou de la version 2.0 5003 par le routeur distribué HNV qui est présent dans chaque VSwitch de l’hôte Hyper-V. Lors de la remise du paquet au commutateur Hyper-V, HNV met à jour la valeur de l’emplacement de la machine virtuelle du paquet entrant sur l’emplacement de la machine virtuelle de destination. Cela ne se produit que si les deux VSID se trouvent dans le même RDID.  Par conséquent, les cartes réseau virtuelles avec RDID1 ne peuvent pas envoyer de paquets aux cartes réseau virtuelles avec RDID2 sans traverser une passerelle.  

> [!NOTE]  
> Dans la description du flot de paquets ci-dessus, le terme « ordinateur virtuel » correspond en fait à la carte réseau virtuelle de la machine virtuelle. Il est fréquent qu’un ordinateur virtuel n’ait qu’une seule carte réseau virtuelle. Dans ce cas, les mots « ordinateur virtuel » et « carte réseau virtuelle » peuvent signifier la même chose.  

Chaque sous-réseau virtuel définit un sous-réseau IP de couche 3 et une limite de domaine de diffusion de couche 2 similaire à un VLAN. Lorsqu’un ordinateur virtuel diffuse un paquet, HNV utilise la réplication monodiffusion (UR) pour effectuer une copie du paquet d’origine et remplacer l’adresse IP de destination et le MAC par les adresses de chaque machine virtuelle qui se trouvent dans le même emplacement de la machine virtuelle.  

> [!NOTE]  
> Lorsque Windows Server 2016 est fourni, les multidiffusions de diffusion et de sous-réseau sont implémentées à l’aide de la réplication monodiffusion. Le routage multidiffusion entre sous-réseaux et IGMP ne sont pas pris en charge.  

En plus d’être un domaine de diffusion, le VSID assure une isolation. Une carte réseau virtuelle dans HNV est connectée à un port de commutateur Hyper-V qui aura des règles ACL appliquées directement au port (ressource REST virtualNetworkInterface) ou au sous-réseau virtuel dont il fait partie.  

Une règle de liste de contrôle d’accès doit être appliquée au port de commutateur Hyper-V. Cette liste de contrôle d’accès peut être autoriser tout, refuser tout ou être plus spécifique pour autoriser uniquement certains types de trafic basés sur des correspondances de 5 tuples (IP source, IP de destination, port source, port de destination, protocole).  

> [!NOTE]  
> Les extensions de commutateur Hyper-V ne fonctionnent pas avec HNVv2 dans la nouvelle pile de mise en réseau SDN (Software Defined Networking). HNVv2 est implémenté à l’aide de l’extension de commutateur VFP (Azure Virtual Filtering Platform) qui ne peut pas être utilisée conjointement avec une autre extension de commutateur tiers.  

## <a name="switching-and-routing-in-hyper-v-network-virtualization"></a>Basculement et routage dans la virtualisation de réseau Hyper-V  
HNVv2 implémente les sémantiques de routage de couche 2 (L2) et de couche 3 (L3) appropriées pour fonctionner comme un commutateur physique ou un routeur. Lorsqu’une machine virtuelle connectée à un réseau virtuel HNV tente d’établir une connexion avec une autre machine virtuelle dans le même sous-réseau virtuel, il doit d’abord connaître l’adresse MAC de l’autorité de certification de la machine virtuelle distante. S’il existe une entrée ARP pour l’adresse IP de l’ordinateur virtuel de destination dans la table ARP de l’ordinateur virtuel source, l’adresse MAC de cette entrée est utilisée. Si une entrée n’existe pas, la machine virtuelle source envoie une diffusion ARP avec une demande pour l’adresse MAC correspondant à l’adresse IP de l’ordinateur virtuel de destination à retourner. Le commutateur Hyper-V intercepte cette demande et l’envoie à l’agent hôte. L’agent hôte recherche dans sa base de données locale une adresse MAC correspondante pour l’adresse IP de la machine virtuelle de destination demandée.  

> [!NOTE]  
> L’agent hôte, agissant en tant que serveur OVSDB, utilise une variante du schéma VTEP pour stocker les mappages CA-PA, la table MAC, etc.  

Si une adresse MAC est disponible, l’agent hôte injecte une réponse ARP et la renvoie à la machine virtuelle. Une fois que la pile de mise en réseau de la machine virtuelle dispose de toutes les informations d’en-tête L2 requises, le frame est envoyé au port Hyper-V correspondant sur le commutateur V. En interne, le commutateur Hyper-V teste ce frame par rapport aux règles de correspondance à N tuples assignées au V-port et applique certaines transformations au frame en fonction de ces règles. Plus important encore, un ensemble de transformations d’encapsulation est appliqué pour construire l’en-tête d’encapsulation à l’aide de NVGRE ou VXLAN, selon la stratégie définie au niveau du contrôleur de réseau. En fonction de la stratégie programmée par l’agent hôte, un mappage CA-PA est utilisé pour déterminer l’adresse IP de l’hôte Hyper-V sur lequel réside l’ordinateur virtuel de destination. Le commutateur Hyper-V garantit que les règles de routage et les balises VLAN correctes sont appliquées au paquet externe pour qu’il atteigne l’adresse PA distante.  

Si une machine virtuelle connectée à un réseau virtuel HNV souhaite créer une connexion avec une machine virtuelle dans un sous-réseau virtuel différent, le paquet doit être acheminé en conséquence. HNV suppose une topologie en étoile où il n’existe qu’une seule adresse IP dans l’espace de l’autorité de certification utilisée comme tronçon suivant pour atteindre tous les préfixes IP (c’est-à-dire un itinéraire/passerelle par défaut). Actuellement, cela applique une limitation à un seul itinéraire par défaut et les itinéraires autres que ceux par défaut ne sont pas pris en charge.  

### <a name="routing-between-virtual-subnets"></a>Routage entre des sous-réseaux virtuels  
Dans un réseau physique, un sous-réseau IP est un domaine de couche 2 (L2) dans lequel les ordinateurs (virtuels et physiques) peuvent communiquer directement entre eux. Le domaine L2 est un domaine de diffusion dans lequel les entrées ARP (IP : mappage d’adresses MAC) sont apprises par le biais de requêtes ARP qui sont diffusées sur toutes les interfaces et les réponses ARP sont renvoyées à l’hôte demandeur. L’ordinateur utilise les informations MAC apprises à partir de la réponse ARP pour construire complètement le frame L2, y compris les en-têtes Ethernet. Toutefois, si une adresse IP se trouve dans un sous-réseau L3 différent, la requête ARP ne franchit pas cette limite L3. Au lieu de cela, une interface de routeur L3 (tronçon suivant ou passerelle par défaut) avec une adresse IP dans le sous-réseau source doit répondre à ces demandes ARP avec sa propre adresse MAC.  

Dans le réseau Windows standard, un administrateur peut créer des itinéraires statiques et les assigner à une interface réseau. En outre, une « passerelle par défaut » est généralement configurée pour être l’adresse IP de saut suivant sur une interface où les paquets destinés à l’itinéraire par défaut (0.0.0.0/0) sont envoyés. Les paquets sont envoyés à cette passerelle par défaut s’il n’existe pas d’itinéraire spécifique. Il s’agit en général du routeur pour votre réseau physique.  HNV utilise un routeur intégré qui fait partie de chaque ordinateur hôte et possède une interface dans chaque ordinateur virtuel pour créer un routeur distribué pour le ou les réseaux virtuels.  

Étant donné que HNV suppose une topologie en étoile, le routeur distribué HNV agit comme une passerelle par défaut unique pour tout le trafic qui se passe entre les sous-réseaux virtuels qui font partie du même réseau d’une même pièce. L’adresse utilisée comme passerelle par défaut correspond par défaut à l’adresse IP la plus basse dans la valeur de la valeur par défaut, et elle est affectée au routeur distribué HNV. Ce routeur distribué permet à un moyen très efficace de router correctement tout le trafic à l’intérieur d’un réseau d’une pièce d’hébergement réseau, car chaque hôte peut acheminer directement le trafic vers l’hôte approprié sans avoir besoin d’un intermédiaire.  Cela est particulièrement vrai lorsque deux ordinateurs virtuels du même réseau d’ordinateurs virtuels, mais de différents sous-réseaux virtuels, se trouvent sur le même hôte physique.  Comme vous le verrez plus loin dans cette section, le paquet ne doit jamais quitter l’hôte physique.  

### <a name="routing-between-pa-subnets"></a>Routage entre les sous-réseaux PA  
Contrairement à HNVv1, qui a alloué une adresse IP PA pour chaque sous-réseau virtuel (HNVv2), utilise désormais une adresse IP PA par membre de l’équipe de cartes réseau Switch-Embedded Teaming (SET). Le déploiement par défaut suppose une association de deux cartes réseau et affecte deux adresses IP PA par hôte. Un seul hôte a des adresses IP d’adresse IP attribuées à partir du même sous-réseau logique de fournisseur (PA) sur le même réseau local virtuel. Deux machines virtuelles clientes dans le même sous-réseau virtuel peuvent être situées sur deux hôtes différents qui sont connectés à deux sous-réseaux logiques de fournisseurs différents. HNV crée les en-têtes d’adresse IP externe pour le paquet encapsulé en fonction du mappage CA-PA. Toutefois, il s’appuie sur la pile TCP/IP de l’hôte vers le protocole ARP pour la passerelle PA par défaut, puis génère les en-têtes Ethernet externes en fonction de la réponse ARP. En règle générale, cette réponse ARP provient de l’interface SVI sur le commutateur physique ou le routeur L3 sur lequel l’hôte est connecté. HNV s’appuie donc sur le routeur L3 pour acheminer les paquets encapsulés entre les sous-réseaux logiques du fournisseur et les réseaux locaux virtuels.  

### <a name="routing-outside-a-virtual-network"></a>Routage hors d’un réseau virtuel  
La plupart des déploiements clients nécessitent une communication entre l’environnement HNV et les ressources qui ne font pas partie de ce même environnement. Les passerelles de virtualisation de réseau sont nécessaires pour permettre la communication entre les deux environnements. Les infrastructures nécessitant une passerelle HNV incluent le cloud privé et le Cloud hybride. Fondamentalement, les passerelles HNV sont nécessaires pour le routage de couche 3 entre les réseaux internes et externes (y compris NAT) ou entre différents sites et/ou Clouds (privés ou publics) qui utilisent un tunnel VPN ou GRE IPSec.  

Les passerelles peuvent présenter différents facteurs de forme physique. Elles peuvent être générées sur Windows Server 2016, incorporées dans un commutateur Top of rack (TDR) agissant comme une passerelle VXLAN, accessible via une adresse IP virtuelle (VIP) publiée par un équilibreur de charge, placées dans d’autres Appliances réseau existantes, ou peuvent être un nouveau dispositif réseau autonome. .  

Pour plus d’informations sur les options de la passerelle RAS Windows, consultez [passerelle RAS](../../../../remote/remote-access/ras-gateway/RAS-Gateway.md).  

## <a name="packet-encapsulation"></a>Encapsulation de paquet  
Dans HNV, chaque carte réseau virtuelle est associée à deux adresses IP :  

-   **Adresse client** (AC) adresse IP attribuée par le client, en fonction de son infrastructure intranet. Cette adresse permet au client d’échanger du trafic réseau avec l’ordinateur virtuel comme s’il n’avait pas été déplacé vers un cloud public ou privé. L’adresse client est visible de l’ordinateur virtuel et accessible au client.  

-   **Adresse du fournisseur** (PA) adresse IP assignée par le fournisseur d’hébergement ou les administrateurs du centre de l’infrastructure en fonction de leur infrastructure réseau physique. L’adresse fournisseur figure dans les paquets du réseau qui sont échangés avec le serveur exécutant Hyper-V qui héberge l’ordinateur virtuel. Cette adresse est visible sur le réseau physique, mais pas sur l’ordinateur virtuel.  

Les adresses client préservent la topologie réseau du client, qui est virtualisée et dissociée des adresses et de la topologie du réseau physique sous-jacent, conformément à l’implémentation des adresses fournisseur. Le diagramme suivant illustre la relation conceptuelle entre les adresses client des ordinateurs virtuels et les adresses fournisseur d’une infrastructure réseau à la suite d’une virtualisation de réseau.  

![diagramme conceptuel d’une virtualisation de réseau sur une infrastructure physique](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF2.gif)  

Figure 6 : diagramme conceptuel d’une virtualisation de réseau sur une infrastructure physique  

Dans le diagramme, les machines virtuelles clientes envoient des paquets de données dans l’espace de l’autorité de certification, qui parcourent l’infrastructure réseau physique par le biais de leurs propres réseaux virtuels, ou « tunnels ». Dans l’exemple ci-dessus, les tunnels peuvent être considérés comme des « enveloppes » autour des paquets de données Contoso et Fabrikam avec des étiquettes d’expédition vertes (adresses PA) à remettre de l’hôte source de gauche à l’hôte de destination à droite. La clé est la manière dont les hôtes déterminent les « adresses d’expédition » (PA) correspondant aux adresses de l’autorité de certification contoso et fabrikam, comment l’enveloppe est placée autour des paquets et comment les hôtes de destination peuvent désencapsuler les paquets et les remettre au contoso et à fabrikam ordinateurs virtuels de destination correctement.  

Cette analogie simple a mis en évidence les principaux aspects de la virtualisation de réseau :  

-   Chaque adresse client d’ordinateur virtuel est mappée à une adresse fournisseur d’hôte physique. Plusieurs adresses client peuvent être associées à la même adresse fournisseur.  

-   Les machines virtuelles envoient des paquets de données dans les espaces de l’autorité de certification, qui sont placés dans une « enveloppe » avec une paire de source et de destination PA basée sur le mappage.  

-   Les mappages entre adresses client et fournisseur doivent permettre aux hôtes de distinguer les paquets à destination des différents ordinateurs virtuels client.  

Par conséquent, la virtualisation d’un réseau consiste à virtualiser les adresses réseau utilisées par les ordinateurs virtuels. Le contrôleur de réseau est responsable du mappage d’adresses et l’agent hôte gère la base de données de mappage à l’aide du schéma MS_VTEP. La section suivante décrit les mécanismes réels de la virtualisation d’adresses.  

## <a name="network-virtualization-through-address-virtualization"></a>La virtualisation de réseau via la virtualisation d’adresses  
HNV implémente les réseaux de locataires superposés à l’aide de l’encapsulation générique de routage (NVGRE) de la virtualisation de réseau ou du réseau local eXtensible virtuel (VXLAN).  VXLAN est la valeur par défaut.  

### <a name="virtual-extensible-local-area-network-vxlan"></a>Réseau local eXtensible (VXLAN)  
Le protocole VXLAN (Virtual eXtensible Local Area Network) ([RFC 7348](https://www.rfc-editor.org/info/rfc7348)) a été largement adopté sur le marché, avec la prise en charge de fournisseurs comme Cisco, Brocade, Arista, Dell, HP et d’autres. Le protocole VXLAN utilise le protocole UDP comme transport. Le port de destination UDP affecté à IANA pour VXLAN est 4789 et le port source UDP doit être un hachage d’informations du paquet interne à utiliser pour la diffusion ECMP. Après l’en-tête UDP, un en-tête VXLAN est ajouté au paquet qui comprend un champ réservé de 4 octets suivi d’un champ de 3 octets pour l’identificateur de réseau VXLAN (VNI)-3-, suivi d’un autre champ réservé de 1 octet. Après l’en-tête VXLAN, la trame d’origine de l’autorité de certification L2 (sans la trame Ethernet de l’autorité de certification) est ajoutée.  

![En-tête de paquet VXLAN](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VXLAN-packet-header.png)  

### <a name="generic-routing-encapsulation-nvgre"></a>Encapsulation générique de routage (NVGRE)  
Ce mécanisme de virtualisation de réseau utilise l’encapsulation générique de routage (NVGRE) dans l’en-tête de tunnel. Dans NVGRE, le paquet de l’ordinateur virtuel est encapsulé dans un autre paquet. L’en-tête de ce nouveau paquet contient les adresses IP fournisseur sources et de destination appropriées, outre l’ID de sous-réseau virtuel, qui est stocké dans le champ de clé de l’en-tête GRE, comme le montre la figure 7.  

![Encapsulation NVGRE](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF3.gif)  

Figure 7 : virtualisation de réseau - encapsulation NVGRE  

L’ID de sous-réseau virtuel permet aux hôtes d’identifier l’ordinateur virtuel client pour un paquet donné, même si les PA et les autorités de certification sur les paquets peuvent se chevaucher. De ce fait, tous les ordinateurs virtuels d’un même hôte peuvent partager une adresse fournisseur unique, comme l’illustre la figure 7.  

La partage de l’adresse fournisseur a des conséquences importantes sur l’extensibilité réseau. Le nombre d’adresses IP et MAC que l’infrastructure réseau doit mémoriser s’en trouve considérablement réduit. Par exemple, si chaque hôte final compte en moyenne 30 ordinateurs virtuels, le nombre d’adresses IP et MAC que doit mémoriser l’infrastructure réseau est réduit selon un facteur de 30. Les ID de sous-réseau virtuel incorporés dans les paquets permettent en outre une corrélation facile entre les paquets et les clients effectifs.  

Le schéma de partage d’attrib pour Windows Server 2012 R2 est un PA par ordinateur hôte. Pour Windows Server 2016, le schéma est un PA par membre de l’équipe de cartes réseau.  

Avec Windows Server 2016 et versions ultérieures, HNV prend entièrement en charge NVGRE et VXLAN. elle ne nécessite pas de mise à niveau ou d’achat de nouveau matériel réseau, tels que des cartes réseau, des commutateurs ou des routeurs. Cela est dû au fait que ces paquets sur le câble sont des paquets IP ordinaires dans l’espace d’adressage local, qui est compatible avec l’infrastructure réseau d’aujourd’hui.  Toutefois, pour obtenir des performances optimales, utilisez les cartes réseau prises en charge avec les pilotes les plus récents qui prennent en charge les déchargements de tâches.  

## <a name="multi-tenant-deployment-example"></a>exemple de déploiement mutualisé  
Le diagramme suivant montre un exemple de déploiement de deux clients situés dans un centre de code Cloud avec la relation CA-PA définie par les stratégies réseau.  

![exemple de déploiement mutualisé](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF5.png)  

Figure 8 : exemple de déploiement mutualisé  

Examinez l’exemple illustré dans la figure 8. Avant d’utiliser le service IaaS partagé du fournisseur d’hébergement :  

-   Contoso Corp a exécuté un serveur SQL Server (nommé **SQL**) à l’adresse IP 10.1.1.11 et un serveur Web (nommé **Web**) à l’adresse IP 10.1.1.12, qui utilise son serveur SQL Server pour les transactions de base de données.  

-   Fabrikam Corp a exécuté un serveur SQL Server, également nommé **SQL** et a assigné l’adresse IP 10.1.1.11, ainsi qu’un serveur Web, également nommé **Web** avec la même adresse IP 10.1.1.12, qui utilise son serveur SQL Server pour les transactions de base de données.  

Nous partons du principe que le fournisseur de services d’hébergement a créé précédemment le réseau logique de fournisseur (PA) via le contrôleur de réseau pour correspondre à sa topologie de réseau physique. Le contrôleur de réseau alloue deux adresses IP PA à partir du préfixe IP du sous-réseau logique où les hôtes sont connectés. Le contrôleur de réseau indique également la balise VLAN appropriée pour appliquer les adresses IP.  

À l’aide du contrôleur de réseau, contoso Corp et Fabrikam Corp créent ensuite leur réseau virtuel et les sous-réseaux qui sont sauvegardés par le réseau logique de fournisseur (PA) spécifié par le fournisseur de services d’hébergement. Contoso Corp et Fabrikam Corp transfèrent leurs serveurs SQL Server et Web respectifs sur le service IaaS partagé du même fournisseur d’hébergement où, par coïncidence, elles exécutent les ordinateurs virtuels **SQL** sur l’hôte 1 Hyper-V et les ordinateurs virtuels **Web** (IIS7) sur l’hôte 2 Hyper-V. Tous les ordinateurs virtuels conservent leurs adresses IP (client) intranet d’origine.  

Les deux sociétés se voient attribuer l’ID de sous-réseau virtuel suivant par le contrôleur de réseau, comme indiqué ci-dessous.  L’agent hôte sur chacun des hôtes Hyper-V reçoit les adresses IP PA allouées à partir du contrôleur de réseau et crée deux cartes réseau virtuelles d’ordinateur hôte PA dans un compartiment réseau non défini par défaut. Une interface réseau est affectée à chacun de ces cartes réseau virtuelles d’ordinateur hôte où l’adresse IP PA est affectée comme indiqué ci-dessous :  

-   Les machines virtuelles de contoso Corp et le pas : Le fournisseur d’adresses Web est 5001, l’adresse **SQL** **est 192.168.1.10** , le **PA Web** est 192.168.2.20  

-   Les machines virtuelles de Fabrikam Corp et le pas : Le fournisseur d’adresses Web est 6001, l’adresse **SQL** **est 192.168.1.10** , le **PA Web** est 192.168.2.20  

Le contrôleur de réseau raccorde toutes les stratégies réseau (y compris le mappage CA-PA) à l’agent hôte SDN qui conservera la stratégie dans un magasin persistant (dans les tables de base de données OVSDB).  

Lorsque l’ordinateur virtuel Web de contoso Corp (10.1.1.12) sur l’hôte 2 Hyper-V crée une connexion TCP à la SQL Server sur 10.1.1.11, voici ce qui se produit :  

-   ARP d’ordinateur virtuel pour l’adresse MAC de destination de 10.1.1.11  

-   L’extension VFP du vSwitch intercepte ce paquet et l’envoie à l’agent hôte SDN  

-   L’agent hôte SDN recherche l’adresse MAC pour 10.1.1.11 dans le magasin de stratégies.  

-   Si un MAC est trouvé, l’agent hôte injecte une réponse ARP à la machine virtuelle  

-   Si aucun MAC n’est trouvé, aucune réponse n’est envoyée et l’entrée ARP de la machine virtuelle pour 10.1.1.11 est marquée comme inaccessible.  

-   La machine virtuelle construit maintenant un paquet TCP avec les en-têtes Ethernet et IP de l’autorité de certification et l’envoie au vSwitch.  

-   L’extension de transfert VFP dans le vSwitch traite ce paquet via les couches VFP (décrites ci-dessous) affectées au port vSwitch source sur lequel le paquet a été reçu et crée une nouvelle entrée de circulation dans la table de workflows unifiés VFP.  

-   Le moteur VFP effectue une recherche de correspondance de règle ou de table de fluides pour chaque couche (par exemple, une couche de réseau virtuel) en fonction des en-têtes IP et Ethernet.  

-   La règle correspondante de la couche réseau virtuelle fait référence à un espace de mappage CA-PA et effectue l’encapsulation.  

-   Le type d’encapsulation (VXLAN ou NVGRE) est spécifié dans la couche de réseau virtuel avec la valeur de l’une.  

-   Dans le cas de l’encapsulation VXLAN, un en-tête UDP externe est construit avec la largeur de l’an 5001 dans l’en-tête VXLAN.  
    Un en-tête IP externe est construit avec les adresses de source et de destination attribuées aux ordinateurs hôtes Hyper-V 2 (192.168.2.20) et Hyper-V hôte 1 (192.168.1.10), respectivement basés sur le magasin de stratégies de l’agent hôte SDN.  

-   Ce paquet est ensuite transmis à la couche de routage PA dans VFP.  

-   La couche de routage PA dans VFP fait référence au compartiment réseau utilisé pour le trafic de l’espace PA et un ID de réseau local virtuel et utilise la pile TCP/IP de l’ordinateur hôte pour transférer correctement le paquet PA vers l’hôte 1 Hyper-V.  

-   Lors de la réception du paquet encapsulé, l’hôte 1 Hyper-V reçoit le paquet dans le compartiment réseau PA et le transfère au vSwitch.  

-   Le VFP traite le paquet par le biais de ses couches VFP et crée une nouvelle entrée de circulation dans la table de fluides unifiés VFP.  

-   Le moteur VFP met en correspondance les règles Ingres dans la couche réseau virtuelle et supprime les en-têtes Ethernet, IP et VXLAN du paquet encapsulé externe.  

-   Le moteur VFP transfère ensuite le paquet au port vSwitch auquel la machine virtuelle de destination est connectée.  

Un processus similaire pour le trafic entre les machines virtuelles Fabrikam Corp **Web** et **SQL** utilise les paramètres de stratégie HNV pour Fabrikam Corp. Par conséquent, avec HNV, les machines virtuelles Fabrikam Corp et Contoso Corp interagissent comme si elles étaient sur leurs intranets d’origine. Ils ne peuvent jamais interagir entre eux, même s’ils utilisent les mêmes adresses IP.  

Les adresses distinctes (ca et pas), les paramètres de stratégie des hôtes Hyper-V et la traduction d’adresses entre l’autorité de certification et l’administrateur système pour le trafic entrant et sortant des ordinateurs virtuels isolent ces ensembles de serveurs à l’aide de la clé NVGRE ou VLXAN VNID. De plus, les mappages et la transformation de la virtualisation dissocient l’architecture de réseau virtuel de l’infrastructure de réseau physique. Bien que les serveurs **SQL** et **Web** Contoso et les serveurs **SQL** et **Web** Fabrikam résident dans leurs propres sous-réseaux IP d’adresses client (10.1.1/24), leur déploiement physique se produit sur deux hôtes situés dans des sous-réseaux d’adresses fournisseur différents, 192.168.1/24 et 192.168.2/24, respectivement. Cela implique que l’approvisionnement et la migration dynamique des ordinateurs virtuels entre sous-réseaux devient possible avec HNV.  

## <a name="hyper-v-network-virtualization-architecture"></a>Architecture de la virtualisation de réseau Hyper-V  
Dans Windows Server 2016, HNVv2 est implémenté à l’aide de la plateforme de filtrage virtuel Azure (VFP) qui est une extension de filtrage NDIS au sein du commutateur Hyper-V. Le concept clé de VFP est celui d’un moteur de Flow correspondance-action avec une API interne exposée à l’agent hôte SDN pour la programmation de la stratégie réseau. L’agent hôte SDN lui-même reçoit la stratégie réseau du contrôleur de réseau sur les canaux de communication OVSDB et WCF SouthBound. Non seulement la stratégie de réseau virtuel (par exemple, le mappage CA-PA) est programmée à l’aide de VFP, mais une stratégie supplémentaire telle que les ACL, la qualité de service, etc.  

La hiérarchie d’objets pour le vSwitch et l’extension de transfert VFP est la suivante :  

-   Commutateur virtuel  

    -   Gestion des cartes réseau externes  

    -   Déchargements de matériel de carte réseau  

    -   Règles de transfert global  

    -   Port  

        -   Couche de transfert de sortie pour épinglage  

        -   Listes d’espaces pour les mappages et les pools NAT  

        -   Table de Flow unifiée  

        -   Couche VFP  

            -   Table de Flow  

            -   Regrouper  

            -   Règle  

                -   Les règles peuvent référencer des espaces  

Dans le VFP, une couche est créée par type de stratégie (par exemple, réseau virtuel) et est un ensemble générique de tables de règles/de flows. Elle n’a pas de fonctionnalité intrinsèque tant que des règles spécifiques n’ont pas été affectées à cette couche pour implémenter cette fonctionnalité. Une priorité et des couches sont affectées à chaque couche par ordre croissant de priorité. Les règles sont organisées en groupes en fonction principalement de la direction et de la famille d’adresses IP. Les groupes sont également affectés d’une priorité et, au maximum, une règle d’un groupe peut correspondre à un fluide donné.  

La logique de transfert pour le vSwitch avec l’extension VFP est la suivante :  

-   Traitement des entrées (entrée du point de vue du paquet entrant dans un port)  

-   Transfert  

-   Traitement de sortie (sortie du point de vue de la sortie du paquet d’un port)  

VFP prend en charge le transfert de MAC interne pour les types d’encapsulation NVGRE et VXLAN, ainsi que le transfert basé sur un réseau local virtuel MAC externe.  

L’extension VFP possède un chemin d’accès lent et un chemin d’accès rapide pour la traversée de paquets. Le premier paquet d’un Flow doit traverser tous les groupes de règles de chaque couche et effectuer une recherche de règle qui est une opération coûteuse. Toutefois, une fois qu’un workflow est inscrit dans la table de Flow unifiée avec une liste d’actions (en fonction des règles correspondantes), tous les paquets suivants sont traités en fonction des entrées de la table de Flow unifiée.  

La stratégie HNV est programmée par l’agent hôte. Chaque carte réseau d’ordinateur virtuel est configurée avec une adresse IPv4. Il s’agit des adresses client dont se serviront les ordinateurs virtuels pour communiquer entre eux et qui sont transportées dans les paquets IP en provenance des ordinateurs virtuels. HNV encapsule le cadre de l’autorité de certification dans un frame PA en fonction des stratégies de virtualisation de réseau stockées dans la base de données de l’agent hôte.  

![Architecture HNV](../../../media/hyper-v-network-virtualization-technical-details-in-windows-server/VNetF7.png)  

Figure 9 : Architecture HNV  

## <a name="summary"></a>Récapitulatif  
Les centres de données en nuage peuvent apporter de nombreux avantages, notamment une extensibilité améliorée et une meilleure utilisation des ressources. Pour profiter de ces avantages potentiels, il est nécessite de faire appel à une technologie à même de résoudre fondamentalement les problèmes de l’extensibilité mutualisée dans un environnement dynamique. La virtualisation HNV a été conçue pour traiter ces problèmes et également pour améliorer l’efficacité des opérations du centre de données en dissociant la topologie du réseau virtuel de la topologie du réseau physique. Reposant sur une norme existante, HNV s’exécute dans le centre de développement actuel et fonctionne avec votre infrastructure VXLAN existante. Les clients disposant de HNV peuvent désormais consolider leurs centres de développement dans un Cloud privé ou étendre de manière transparente leurs centres de développement dans un environnement de fournisseur de serveurs d’hébergement avec un Cloud hybride.  

## <a name="BKMK_LINKS"></a>Voir aussi  
Pour en savoir plus sur HNVv2, consultez les liens suivants :  


|       Type de contenu       |                                                                                                                                              Références                                                                                                                                              |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Ressources de la communauté**  |                                                                Blog sur l'[architecture de cloud privé](https://blogs.technet.com/b/privatecloud) -   <br />-Poser des questions : [cloudnetfb@microsoft.com](mailto:%20cloudnetfb@microsoft.com)                                                                |
|         **APPELÉE**          |                                                                   -   [NVGRE Draft RFC](https://www.ietf.org/id/draft-sridharan-virtualization-nvgre-07.txt)<br />-   [VXLAN-RFC 7348](https://www.rfc-editor.org/info/rfc7348)                                                                    |
| **Technologies connexes** | -Pour plus d’informations techniques sur la virtualisation de réseau Hyper-V dans Windows Server 2012 R2, consultez [Détails techniques sur la virtualisation de réseau Hyper-v](https://technet.microsoft.com/library/jj134174.aspx)<br />[contrôleur de réseau](../../../sdn/technologies/network-controller/Network-Controller.md) -    |

