---
title: Commutateur virtuel Hyper-V
description: Cette rubrique fournit une vue d’ensemble du commutateur virtuel Hyper-V dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: article
ms.assetid: 398440ac-5988-41ce-b91e-eab343a255d3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c508005af67e9dd5b0c9a22693aca25eb19e8e48
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366829"
---
# <a name="hyper-v-virtual-switch"></a>Commutateur virtuel Hyper-V

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique fournit une vue d’ensemble du commutateur virtuel Hyper-V, qui vous permet de connecter des ordinateurs virtuels \(des machines virtuelles\) à des réseaux externes à l’ordinateur hôte Hyper\-V, y compris l’intranet de votre organisation et Internet. 

Vous pouvez également vous connecter à des réseaux virtuels sur le serveur qui exécute hyper\-V quand vous déployez un réseau à définition logicielle \(SDN\).

> [!NOTE]  
> En plus de cette rubrique, la documentation suivante du commutateur virtuel Hyper-V est disponible.  
>   
> - [Gérer le commutateur virtuel Hyper-V](Manage-Hyper-V-Virtual-Switch.md) 
> - [RDMA (Remote Direct Memory Access) et SET (Switch Embedded Teaming)](RDMA-and-Switch-Embedded-Teaming.md)
> - [Applets de commande d’équipe du commutateur réseau dans Windows PowerShell](https://technet.microsoft.com/library/jj553812.aspx)
> - [Nouveautés de VMM 2016](https://docs.microsoft.com/system-center/vmm/whats-new#networking)
> - [Configurer l’infrastructure de mise en réseau VMM](https://docs.microsoft.com/system-center/vmm/manage-networks)
> - [Créer des réseaux avec VMM 2012](https://social.technet.microsoft.com/wiki/contents/articles/3140.create-networks-with-vmm-2012.aspx)  
> - [Hyper-V : configurer des réseaux locaux virtuels et le balisage VLAN](https://social.technet.microsoft.com/wiki/contents/articles/1306.hyper-v-configure-vlans-and-vlan-tagging.aspx)  
> - [Hyper-V : l’extension de commutateur virtuel WFP doit être activée si elle est requise par les extensions tierces](https://social.technet.microsoft.com/wiki/contents/articles/13071.hyper-v-the-wfp-virtual-switch-extension-should-be-enabled-if-it-is-required-by-third-party-extensions.aspx)
>
> Pour plus d’informations sur les autres technologies de mise en réseau, voir [mise en réseau dans Windows Server 2016](https://docs.microsoft.com/windows-server/networking/networking).
  
Le commutateur virtuel Hyper\-V est un commutateur réseau Ethernet de couche 2, qui est disponible dans le Gestionnaire Hyper\-V lorsque vous installez le rôle serveur Hyper\-V.

Le commutateur virtuel Hyper-V comprend des fonctionnalités gérées par programme et extensibles pour connecter des machines virtuelles aux réseaux virtuels et au réseau physique. Qui plus est, le commutateur virtuel Hyper-V assure l’application de la stratégie de sécurité et d’isolement, ainsi que des niveaux de service.  
  
> [!NOTE]  
> Le commutateur virtuel Hyper-V ne prend en charge qu’Ethernet et pas les autres technologies de réseau local (LAN) câblé, comme Infiniband et Fibre Channel.  
  
Le commutateur virtuel Hyper-V comprend des fonctionnalités d’isolation du client, la mise en forme du trafic, la protection contre les machines virtuelles malveillantes et une résolution simplifiée des problèmes. 

Avec la prise en charge intégrée de la spécification de l’interface de périphérique réseau \(NDIS\) filtrage des pilotes et de la plateforme de filtrage Windows \(des pilotes de\) WFP, le commutateur virtuel Hyper-V permet aux éditeurs de logiciels indépendants \(ISV\) de créer des plug-ins extensibles, appelés extensions de commutateur virtuel, qui offrent des fonctionnalités de sécurité et de mise en réseau améliorées. Les extensions de commutateur virtuel que vous ajoutez au commutateur virtuel Hyper-V sont répertoriées dans la fonctionnalité Gestionnaire de commutateur virtuel du Gestionnaire Hyper-V.
  
Dans l’illustration suivante, une machine virtuelle dispose d’une carte réseau virtuelle connectée au commutateur virtuel Hyper-V via un port commuté.  
  
![Connexions de commutateur virtuel](../media/Hyper-V-Virtual-Switch/Vswitch_01.jpg)  
  
Les fonctionnalités du commutateur virtuel Hyper-V vous offrent davantage d’options pour l’application de l’isolation des locataires, la mise en forme et le contrôle du trafic réseau et l’utilisation de mesures de protection contre les machines virtuelles malveillantes.

>[!NOTE]
> Dans Windows Server 2016, une machine virtuelle avec une carte réseau virtuelle affiche avec précision le débit maximal pour la carte réseau virtuelle. Pour afficher la vitesse de la carte réseau virtuelle dans **connexions réseau**, cliquez avec le bouton droit sur l’icône de carte réseau virtuelle souhaitée, puis cliquez sur **État**. La boîte de dialogue **État** de la carte d’interface réseau virtuelle s’ouvre. Dans le cadre de la **connexion**, la valeur **Vitesse** correspond à la vitesse de la carte réseau physique installée sur le serveur.
  
## <a name="bkmk_apps"></a>Utilisations du commutateur virtuel Hyper-V

Voici quelques scénarios de cas d’usage pour le commutateur virtuel Hyper-V.

**Affichage des statistiques**: un développeur chez un fournisseur de Cloud hébergé implémente un package de gestion qui affiche l’état actuel du commutateur virtuel Hyper-V. Le package de gestion peut demander les statistiques sur les fonctionnalités actuelles, les paramètres de configuration et des ports spécifiques sur tout le commutateur à l’aide de WMI. L’état du commutateur est ensuite affiché pour offrir aux administrateurs une vue d’ensemble.  
  
**Suivi des ressources**: une société d’hébergement vend les services d’hébergement facturés en fonction du niveau d’adhésion. Les différents niveaux d’abonnement offrent plusieurs niveaux de performance réseau. L’administrateur attribue des ressources pour respecter les contrats SLA de sorte que la disponibilité du réseau soit équilibrée. L’administrateur effectue le suivi par programme des informations, telles que l’utilisation actuelle de la bande passante affectée et le nombre de files d’attente d’ordinateurs virtuels (ordinateurs virtuels) affectés ou de canaux IOV. Le même programme enregistre en outre périodiquement les ressources utilisées en plus de celles assignées par ordinateur virtuel pour le suivi à double entrée ou pour les ressources.  
  
**Gestion de l’ordre des extensions de commutateur**: une entreprise a installé des extensions sur leur hôte Hyper-V pour surveiller le trafic et signaler la détection d’intrusion. Lors de la maintenance, des extensions peuvent être mises à jour ce qui est susceptible de modifier l’ordre des extensions. Un simple script est exécuté pour rétablir l’ordre des extensions après leur mise à jour.  
  
L' **extension de transfert gère l’ID de réseau local virtuel**: une société de commutation principale crée une extension de transfert qui applique toutes les stratégies de mise en réseau. Un des éléments gérés correspond aux ID de réseau local virtuel. Le commutateur virtuel cède le contrôle du réseau local virtuel à l’extension de transfert. L’installation de l’entreprise de commutation appelle par programme une interface de programmation d’applications (API) Windows Management Instrumentation (WMI) qui active la transparence, indiquant au commutateur virtuel Hyper-V de passer et de ne pas effectuer d’action sur les étiquettes VLAN.  
  
## <a name="bkmk_func"></a>Fonctionnalité du commutateur virtuel Hyper-V
 
Voici certaines des fonctionnalités principales incluses dans le commutateur virtuel Hyper-V :  
  
-   **Protection contre l’empoisonnement (usurpation d’identité)** : offre une protection contre les machines virtuelles malveillantes à l’aide de l’usurpation ARP (Address Resolution Protocol) pour voler des adresses IP à partir d’autres machines virtuelles. Il fournit une protection contre les attaques pouvant être lancées contre IPv6 par usurpation ND (Neighbor Discovery).  
  
-   **Protection contre la protection DHCP**: protège contre une machine virtuelle malveillante qui se présente comme un serveur DHCP (Dynamic Host Configuration Protocol) pour les attaques de l’intercepteur.  
  
-   **Listes**de contrôle d’accès des ports : fournit un filtrage du trafic basé sur les adresses/plages de Access Control de média (Mac) ou IP (Internet Protocol), ce qui vous permet de configurer l’isolement du réseau virtuel.  
  
-   **Mode Trunk vers une machine virtuelle**: permet aux administrateurs de configurer une machine virtuelle spécifique en tant qu’appliance virtuelle, puis de diriger le trafic de différents réseaux locaux virtuels vers cette machine virtuelle.  
  
-   **Surveillance du trafic réseau**: permet aux administrateurs d’examiner le trafic qui traverse le commutateur réseau.  
  
-   **Réseau local virtuel isolé (privé)** : permet aux administrateurs de séparer le trafic sur plusieurs réseaux locaux virtuels, afin d’établir plus facilement des communautés isolées de locataires.  
  
La liste suivante récapitule les fonctionnalités qui améliorent l’utilisation du commutateur virtuel Hyper-V :  
  
-   **Limite de bande passante et prise en charge Burst**: la bande passante minimale garantit une quantité de bande passante réservée. La bande passante maximale limite son exploitation par une machine virtuelle.  
  
-   **Prise en charge du marquage ECN (Explicit congestion notification)** : le marquage ECN, également connu sous le nom de Data nom (DCTCP), permet au commutateur physique et au système d’exploitation de réguler le flux de trafic afin que les ressources de mémoire tampon du commutateur ne soient pas submergées, ce qui entraîne une augmentation du débit du trafic.  
  
-   **Diagnostics**: les diagnostics permettent de suivre et de surveiller facilement les événements et les paquets via le commutateur virtuel.
