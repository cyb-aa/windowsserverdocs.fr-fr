---
title: Commutateur virtuel Hyper-V
description: Cette rubrique fournit une vue d’ensemble du commutateur virtuel Hyper-V dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: article
ms.assetid: 398440ac-5988-41ce-b91e-eab343a255d3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 17962b9bbb90a5ea1b6a4a303c64d72f74be37b5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860920"
---
# <a name="hyper-v-virtual-switch"></a>Commutateur virtuel Hyper-V

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique fournit une vue d’ensemble du commutateur virtuel Hyper-V, qui vous offre la possibilité de connecter des machines virtuelles \(machines virtuelles\) à des réseaux externes à l’Hyper\-hôte V, notamment intranet de votre organisation et Internet. 

Vous pouvez également vous connecter à des réseaux virtuels sur le serveur qui exécute Hyper\-V lorsque vous déployez Sdn \(SDN\).

> [!NOTE]  
> Outre cette rubrique, la documentation suivante sur le commutateur virtuel Hyper-V est disponible.  
>   
> - [Gérer le commutateur virtuel Hyper-V](Manage-Hyper-V-Virtual-Switch.md) 
> - [Accès Direct à la mémoire à distance (RDMA) et le commutateur incorporé association (SET)](RDMA-and-Switch-Embedded-Teaming.md)
> - [Applets de commande Team de commutateur réseau dans Windows PowerShell](https://technet.microsoft.com/library/jj553812.aspx)
> - [Quelles sont les nouveautés dans VMM 2016](https://docs.microsoft.com/system-center/vmm/whats-new#networking)
> - [Configurer la mise en réseau de fabric VMM](https://docs.microsoft.com/system-center/vmm/manage-networks)
> - [Créer des réseaux avec VMM 2012](https://social.technet.microsoft.com/wiki/contents/articles/3140.create-networks-with-vmm-2012.aspx)  
> - [Hyper-V : Configurer des réseaux locaux virtuels et le balisage VLAN](https://social.technet.microsoft.com/wiki/contents/articles/1306.hyper-v-configure-vlans-and-vlan-tagging.aspx)  
> - [Hyper-V : L’extension de commutateur virtuel WFP doit être activée si elle est requise par les extensions tierces](https://social.technet.microsoft.com/wiki/contents/articles/13071.hyper-v-the-wfp-virtual-switch-extension-should-be-enabled-if-it-is-required-by-third-party-extensions.aspx)
>
> Pour plus d’informations sur les autres technologies de mise en réseau, consultez [mise en réseau dans Windows Server 2016](https://docs.microsoft.com/windows-server/networking/networking).
  
Hyper\-V Virtual Switch est un logiciel basé Ethernet réseau commutateur de couche 2 qui est disponible dans Hyper\-V Manager lorsque vous installez Hyper\-rôle serveur V.

Commutateur virtuel Hyper-V inclut des fonctionnalités gérées par programme et extensibles pour connecter des machines virtuelles à des réseaux virtuels et le réseau physique. Qui plus est, le commutateur virtuel Hyper-V assure l’application de la stratégie de sécurité et d’isolement, ainsi que des niveaux de service.  
  
> [!NOTE]  
> Le commutateur virtuel Hyper-V ne prend en charge qu’Ethernet et pas les autres technologies de réseau local (LAN) câblé, comme Infiniband et Fibre Channel.  
  
Commutateur virtuel Hyper-V inclut des fonctionnalités d’isolation de locataire, mise en forme du trafic, protection contre les ordinateurs virtuels malveillants et la résolution simplifiée. 

Prise en charge intégrée pour Network Device Interface Specification \(NDIS\) filtrer les pilotes et la plateforme de filtrage Windows \(WFP\) pilotes d’appel, le commutateur virtuel Hyper-V permet de logiciels indépendants fournisseurs \(les éditeurs de logiciels\) pour créer des plug-ins extensibles, appelés Extensions de commutateur virtuel, ce qui peut fournir des capacités améliorées de mise en réseau et de sécurité. Les extensions de commutateur virtuel que vous ajoutez au commutateur virtuel Hyper-V sont répertoriées dans la fonctionnalité Gestionnaire de commutateur virtuel du Gestionnaire Hyper-V.
  
Dans l’illustration suivante, une machine virtuelle a une carte réseau virtuelle connectée au commutateur virtuel Hyper-V via un port de commutateur.  
  
![Connexions de commutateur virtuelles](../media/Hyper-V-Virtual-Switch/Vswitch_01.jpg)  
  
Fonctionnalités du commutateur virtuel Hyper-V vous fournissent plus d’options pour mettre en œuvre l’isolation des locataires, trafic réseau façonnage et le contrôle, et utilisant la protection mesures de protection contre les ordinateurs virtuels malveillants.

>[!NOTE]
> Dans Windows Server 2016, une machine virtuelle avec une carte réseau virtuelle affiche précisément le débit maximal pour la carte réseau virtuelle. Pour afficher la vitesse de la carte réseau virtuelle dans **connexions réseau**, cliquez sur l’icône de carte réseau virtuel souhaité, puis sur **état**. La carte réseau virtuelle **état** boîte de dialogue s’ouvre. Dans **connexion**, la valeur de **vitesse** correspond à la vitesse de la carte réseau physique installée sur le serveur.
  
## <a name="bkmk_apps"></a>Utilisations de commutateur virtuel Hyper-V

Voici quelques scénarios d’utilisation pour le commutateur virtuel Hyper-V.

**Affichage des statistiques**: un développeur chez un fournisseur de solutions cloud hébergées implémente un package de gestion qui affiche l’état du commutateur virtuel Hyper-V. Le package de gestion peut demander les statistiques sur les fonctionnalités actuelles, les paramètres de configuration et des ports spécifiques sur tout le commutateur à l’aide de WMI. L’état du commutateur est ensuite affiché pour offrir aux administrateurs une vue d’ensemble.  
  
**Suivi des ressources**: un fournisseur d’hébergement propose des services facturés en fonction de l’abonnement souscrit. Les différents niveaux d’abonnement offrent plusieurs niveaux de performance réseau. L’administrateur attribue des ressources pour respecter les contrats SLA de sorte que la disponibilité du réseau soit équilibrée. L’administrateur effectue le suivi par programme des informations telles que l’utilisation actuelle de la bande passante affectée et le nombre de machine virtuelle (VM) affecté de la file d’attente de machines virtuelles (VMQ) ou les canaux IOV. Le même programme enregistre en outre périodiquement les ressources utilisées en plus de celles assignées par ordinateur virtuel pour le suivi à double entrée ou pour les ressources.  
  
**Gestion de l’ordre des extensions du commutateur**: une entreprise a installé des extensions sur leur hôte Hyper-V pour contrôler le trafic et signaler la détection d’intrusions. Lors de la maintenance, des extensions peuvent être mises à jour ce qui est susceptible de modifier l’ordre des extensions. Un simple script est exécuté pour rétablir l’ordre des extensions après leur mise à jour.  
  
**Extension de transfert gérant l’ID du VLAN**: une société importante spécialisée dans la commutation met au point une extension de transfert qui applique toutes les stratégies de mise en réseau. Un des éléments gérés correspond aux ID de réseau local virtuel. Le commutateur virtuel cède le contrôle du réseau local virtuel à l’extension de transfert. Installation de l’entreprise de commutation appelle par programme une Windows Management Instrumentation (WMI) application interface de programmation (API) qui active la transparence, indiquant ainsi au commutateur virtuel Hyper-V de passer et de ne rien faire avec les étiquettes VLAN.  
  
## <a name="bkmk_func"></a>Fonctionnalité de commutateur virtuel Hyper-V
 
Voici certaines des fonctionnalités principales incluses dans le commutateur virtuel Hyper-V :  
  
-   **Protection ARP/ND empoisonnement (usurpation d’identité)**: protège contre toute machine virtuelle malveillante susceptible d’usurper une identité par empoisonnement du cache ARP (Address Resolution Protocol) dans le but de subtiliser les adresses IP d’autres machines virtuelles. Il fournit une protection contre les attaques pouvant être lancées contre IPv6 par usurpation ND (Neighbor Discovery).  
  
-   **Protection de la protection DHCP**: protège contre toute machine virtuelle malveillante se présentant comme un serveur DHCP (Dynamic Host Configuration Protocol) pour lancer des attaques de type intercepteur.  
  
-   **ACL de port**: permet de filtrer le trafic en fonction d’adresses/plages MAC (Media Access Control) ou IP (Internet Protocol), ce qui permet de configurer l’isolement du réseau virtuel.  
  
-   **Mode trunk à une machine virtuelle**: permet aux administrateurs de configurer une machine virtuelle spécifique sous la forme d’un appareil virtuel, puis de diriger le trafic des différents réseaux locaux virtuels vers la machine virtuelle en question.  
  
-   **Analyse du trafic réseau**: permet aux administrateurs de surveiller le trafic qui transite par le commutateur réseau.  
  
-   **Réseau local virtuel (privé) isolé**: permet aux administrateurs de scinder le trafic sur plusieurs réseaux locaux virtuels afin d’établir plus facilement des communautés isolées de locataires.  
  
La liste suivante récapitule les fonctionnalités qui améliorent l’utilisation du commutateur virtuel Hyper-V :  
  
-   **Limite de bande passante et en rafale prennent en charge**: le minimum de bande passante assure une bande passante réservée. La bande passante maximale limite son exploitation par une machine virtuelle.  
  
-   **Notification ECN (Explicit Congestion) en charge du marquage**:  ECN marquage, également appelé données nom (DCTCP), permet le commutateur physique et le système d’exploitation de réguler le flux de trafic que les ressources de mémoire tampon du commutateur ne soient pas saturées, ce qui aboutit à un débit supérieur du trafic.  
  
-   **Diagnostics**: les diagnostics permettent un suivi simple des événements et des paquets qui transitent par le commutateur virtuel.
