---
title: Équilibrage de la charge réseau
description: Dans cette rubrique, nous vous proposons une vue d’ensemble de la fonctionnalité d’équilibrage de charge réseau \(NLB @ no__t-1 dans Windows Server 2016. Vous pouvez utiliser NLB pour gérer deux serveurs ou plus en tant que cluster virtuel unique. L’équilibrage de la charge réseau améliore la disponibilité et l’extensibilité des applications de serveur Internet, telles que celles utilisées sur les serveurs Web, FTP, de pare-feu, proxy, de réseau privé virtuel \(VPN @ no__t-1 et d’autres serveurs mission @ no__t-2critical.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-nlb
ms.topic: article
ms.assetid: 244a4b48-06e5-4796-8750-a50e4f88ac72
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: 4d79b6f29fbe64633bf04604ad586aff3dd86edf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405847"
---
# <a name="network-load-balancing"></a>Équilibrage de la charge réseau

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Dans cette rubrique, nous vous proposons une vue d’ensemble de la fonctionnalité d’équilibrage de charge réseau \(NLB @ no__t-1 dans Windows Server 2016. Vous pouvez utiliser NLB pour gérer deux serveurs ou plus en tant que cluster virtuel unique. L’équilibrage de la charge réseau améliore la disponibilité et l’extensibilité des applications de serveur Internet, telles que celles utilisées sur les serveurs Web, FTP, de pare-feu, proxy, de réseau privé virtuel \(VPN @ no__t-1 et d’autres serveurs mission @ no__t-2critical.  

> [!NOTE]
> Windows Server 2016 comprend un nouveau logiciel inspiré d’Azure Load Balancer \(SLB @ no__t-1 comme composant de la mise en réseau définie par le logiciel \(SDN @ no__t-3. Utilisez SLB au lieu de NLB si vous utilisez SDN, que vous utilisez des charges de travail non-Windows, que vous avez besoin de la traduction d’adresses réseau sortantes \(NAT @ no__t-1, ou que vous avez besoin de la couche 3 \(L3 @ no__t-3 ou d’un équilibrage de charge basé sur non TCP. Vous pouvez continuer à utiliser NLB avec Windows Server 2016 pour les déploiements non SDN. Pour plus d’informations sur SLB, voir la rubrique relative [à l’équilibrage de charge logiciel (SLB) pour SDN](../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).

La fonctionnalité d’équilibrage de la charge réseau \(NLB @ no__t-1 distribue le trafic sur plusieurs serveurs à l’aide du protocole réseau TCP @ no__t-2IP. En combinant plusieurs ordinateurs qui exécutent des applications dans un seul cluster virtuel, l’équilibrage de la charge réseau assure la fiabilité et les performances des serveurs Web et d’autres serveurs mission @ no__t-0critical.  
  
Les serveurs d’un cluster d’équilibrage de la charge réseau sont appelés *hôtes* et chacun d’eux exécute une copie distincte des applications de serveurs. L’équilibrage de la charge réseau répartit les demandes de client entrantes entre les hôtes du cluster. Vous pouvez configurer la charge qui doit être gérée par chaque hôte. Vous pouvez également ajouter des hôtes de façon dynamique au cluster afin de traiter une charge accrue. L’équilibrage de la charge réseau dirige également tout le trafic vers un seul hôte désigné, appelé *hôte par défaut*.  
  
L’équilibrage de la charge réseau permet de traiter tous les ordinateurs du cluster à l’aide du même ensemble d’adresses IP. Il permet également de conserver un ensemble d’adresses IP dédiées uniques pour chaque hôte. Pour les applications Load @ no__t-0balanced, lorsqu’un ordinateur hôte échoue ou se met hors connexion, la charge est automatiquement redistribuée entre les ordinateurs qui fonctionnent toujours. Lorsqu’il est prêt, l’ordinateur hors connexion peut rejoindre de manière transparente le cluster et récupérer sa part de la charge, ce qui permet aux autres ordinateurs du cluster de traiter moins de trafic.  
  
## <a name="practical-applications"></a>Cas pratiques  
L’équilibrage de la charge réseau est utile pour s’assurer que les applications sans État, telles que les serveurs Web exécutant Internet Information Services \(IIS @ no__t-1, sont disponibles avec un temps d’arrêt minimal et qu’elles sont évolutives \(by ajoutant des serveurs supplémentaires en tant que charge. augmente @ no__t-3. Les sections suivantes expliquent comment l’équilibrage de la charge réseau prend en charge la haute disponibilité, l’extensibilité et la simplicité de gestion des serveurs en cluster qui exécutent ces applications.  
  
### <a name="high-availability"></a>Haute disponibilité  
Un système à haute disponibilité offre de manière fiable un niveau de service acceptable avec un temps d’immobilisation très faible. Pour offrir une haute disponibilité, l’équilibrage de la charge réseau comprend des fonctionnalités intégrées à @ no__t-0in qui peuvent automatiquement :  
  
-   détecter un hôte de cluster en panne ou hors connexion, puis le récupérer ;  
  
-   équilibrer la charge réseau lors de l’ajout ou de la suppression d’hôtes ;  
  
-   récupérer et redistribuer la charge dans un délai de dix secondes.  
  
### <a name="scalability"></a>Extensibilité  
L’extensibilité est la mesure de la capacité d’un ordinateur, d’un service ou d’une application à répondre à des besoins croissants de performances. Pour les clusters d’équilibrage de la charge réseau, l’extensibilité correspond à la possibilité d’ajouter un ou plusieurs systèmes à un cluster existant lorsque la charge totale du cluster dépasse ses capacités. Pour assurer cette extensibilité, l’équilibrage de la charge réseau vous permet d’effectuer les opérations listées ci-après :  
  
-   Équilibrer les demandes de charge dans le cluster NLB pour les services TCP @ no__t-0IP individuels.  
  
-   Prendre en charge jusqu’à 32 ordinateurs dans un seul cluster.  
  
-   Équilibrer plusieurs demandes de chargement de serveur \(from le même client ou de plusieurs clients @ no__t-1 sur plusieurs hôtes du cluster.  
  
-   Ajouter des hôtes au cluster d’équilibrage de la charge réseau au fur et à mesure de l’augmentation de la charge, sans entraîner un échec du cluster.  
  
-   Supprimer des hôtes du cluster lorsque la charge diminue.  
  
-   Permettre des performances élevées et un faible temps système via une mise en œuvre entièrement en pipeline. Le traitement en pipeline permet d’envoyer les demandes au cluster d’équilibrage de la charge réseau sans attendre la réponse à une demande précédente.  
  
### <a name="manageability"></a>Facilité de gestion  
Pour assurer cette simplicité de gestion, l’équilibrage de la charge réseau vous permet d’effectuer les opérations listées ci-après :  
  
-   Gérer et configurer plusieurs clusters NLB et les hôtes du cluster à partir d’un seul ordinateur à l’aide du gestionnaire NLB ou des [applets de commande d’équilibrage de la charge réseau (NLB) dans Windows PowerShell](https://technet.microsoft.com/library/hh801274.aspx).
  
-   Spécifier le comportement d’équilibrage de la charge pour un seul port IP ou un groupe de ports à l’aide de règles de gestion des ports.  
  
-   Définir des règles de port différentes pour chaque site Web. Si vous utilisez le même ensemble de serveurs de charge @ no__t-0balanced pour plusieurs applications ou sites Web, les règles de port se basent sur l’adresse IP virtuelle de destination @no__t les clusters virtuels 1using @ no__t-2.  

-   Diriger toutes les demandes du client vers un seul hôte à l’aide de règles facultatives @ no__t-0host. L’équilibrage de la charge réseau route les demandes clientes vers un hôte particulier qui exécute des applications spécifiques.  

-   Bloquer l’accès réseau non souhaité à certains ports IP.  

-   Activez la prise en charge d’Internet Group Management Protocol \(IGMP @ no__t-1 sur les hôtes du cluster pour contrôler la saturation des ports commutés \(where les paquets réseau entrants sont envoyés à tous les ports sur le commutateur @ no__t-3 en mode de multidiffusion.  

-   Démarrer, arrêter et contrôler les actions d’équilibrage de la charge réseau à distance, à l’aide de commandes ou de scripts Windows PowerShell.  

-   Afficher le journal des événements Windows pour vérifier les événements d’équilibrage de la charge réseau. L’équilibrage de la charge réseau consigne toutes les actions et modifications du cluster dans le journal des événements.  

## <a name="important-functionality"></a>Fonctionnalités importantes  
 
L’équilibrage de la charge réseau est installé en tant que composant de pilote réseau Windows Server standard. Ses opérations sont transparentes pour la pile de mise en réseau TCP @ no__t-0IP. L’illustration suivante montre la relation entre l’équilibrage de la charge réseau et d’autres composants logiciels dans une configuration classique.  
  
![Équilibrage de la charge réseau et autres composants logiciels](../media/NLB/nlb.jpg)  
  
Voici les principales fonctionnalités de l’équilibrage de la charge réseau.  
  
- Elle ne nécessite aucune modification du matériel pour pouvoir s’exécuter.  
  
- Elle fournit des outils d’équilibrage de la charge réseau pour configurer et gérer plusieurs clusters, ainsi que tous les hôtes de cluster, à partir d’un seul ordinateur distant ou local.  
  
- Permet aux clients d’accéder au cluster à l’aide d’un seul nom Internet logique et d’une seule adresse IP virtuelle, appelée adresse IP du cluster \(IT conserve des noms individuels pour chaque ordinateur @ no__t-1. L’équilibrage de la charge réseau autorise plusieurs adresses IP virtuelles pour les serveurs multirésidents.  
  
> [!NOTE]  
> Lorsque vous déployez des machines virtuelles en tant que clusters virtuels, l’équilibrage de la charge réseau ne nécessite pas que les serveurs soient multirésidents pour avoir plusieurs adresses IP virtuelles.  
  
- L’équilibrage de la charge réseau peut être lié à plusieurs cartes réseau, ce qui vous permet de configurer plusieurs clusters indépendants sur chaque hôte. La prise en charge de plusieurs cartes réseau diffère des clusters virtuels en ce sens que les clusters virtuels vous permettent de configurer plusieurs clusters sur une seule carte réseau.  
  
- Elle ne nécessite aucune modification des applications serveur pour que ces dernières puissent s’exécuter dans un cluster d’équilibrage de la charge réseau.  
  
- Elle peut être configurée pour ajouter automatiquement un hôte au cluster si cet hôte de cluster tombe en panne et s’il est ensuite remis en ligne. L’hôte ajouté peut démarrer le traitement des nouvelles demandes adressées au serveur par les clients.  
  
-   Elle vous permet de mettre des ordinateurs hors connexion à des fins de maintenance préventive sans perturber les opérations de cluster sur les autres hôtes.  
  
## <a name="hardware-requirements"></a>Configuration matérielle requise  
Voici la configuration matérielle requise pour exécuter un cluster NLB.  
  
-   Tous les hôtes du cluster doivent résider sur le même sous-réseau.  
  
-   Il n’existe aucune restriction sur le nombre de cartes réseau de chaque hôte. Par ailleurs, les différents hôtes peuvent avoir un nombre distinct de cartes.  
  
-   Au sein de chaque cluster, toutes les cartes réseau doivent être soit multidiffusion, soit monodiffusion. L’équilibrage de la charge réseau ne prend pas en charge un environnement mixte de multidiffusion et de monodiffusion au sein d’un seul cluster.  
  
-   Si vous utilisez le mode de monodiffusion, la carte réseau utilisée pour gérer le trafic client @ no__t-0to @ no__t-1cluster doit prendre en charge la modification de l’adresse de contrôle d’accès au média \(MAC @ no__t-3.  
  
## <a name="software-requirements"></a>Configuration logicielle requise  
Voici la configuration logicielle requise pour exécuter un cluster NLB.  
  
-   Seul TCP @ no__t-0IP peut être utilisé sur l’adaptateur pour lequel NLB est activé sur chaque hôte. N’ajoutez pas d’autres protocoles \(for, IPX @ no__t-1 à cet adaptateur.  
  
-   Les adresses IP des serveurs du cluster doivent être statiques.  
  
> [!NOTE]  
> NLB ne prend pas en charge le protocole de configuration d’hôte dynamique @no__t 0DHCP @ no__t-1. L’équilibrage de la charge réseau désactive le protocole DHCP sur chaque interface configurée.  
  
## <a name="installation-information"></a>Informations sur l’installation  
Vous pouvez installer NLB à l’aide de Gestionnaire de serveur ou des commandes Windows PowerShell pour l’équilibrage de la charge réseau.

Vous pouvez éventuellement installer les outils d’équilibrage de la charge réseau pour gérer un cluster d’équilibrage de la charge réseau local ou distant. Les outils incluent le gestionnaire d’équilibrage de la charge réseau et les commandes Windows PowerShell NLB.

### <a name="installation-with-server-manager"></a>Installation avec Gestionnaire de serveur

Dans Gestionnaire de serveur, vous pouvez utiliser l’Assistant Ajout de rôles et de fonctionnalités pour ajouter la fonctionnalité d’équilibrage de la **charge réseau** . Une fois l’exécution de l’Assistant terminée, l’équilibrage de la charge réseau est installé et vous n’avez pas besoin de redémarrer l’ordinateur.


### <a name="installation-with-windows-powershell"></a>Installation avec Windows PowerShell  

Pour installer NLB à l’aide de Windows PowerShell, exécutez la commande suivante dans une invite Windows PowerShell avec élévation de privilèges sur l’ordinateur sur lequel vous souhaitez installer NLB.

    
    Install-WindowsFeature NLB -IncludeManagementTools
    
Une fois l’installation terminée, aucun redémarrage de l’ordinateur n’est nécessaire.

Pour plus d’informations, voir [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/install-windowsfeature?view=win10-ps).

### <a name="network-load-balancing-manager"></a>Gestionnaire d’équilibrage de la charge réseau
Pour ouvrir le Gestionnaire d’équilibrage de la charge réseau dans le Gestionnaire de serveur, cliquez sur **Outils**, puis sur **Gestionnaire d’équilibrage de la charge réseau**.
  
## <a name="additional-resources"></a>Ressources supplémentaires  
Le tableau suivant fournit des liens vers des informations supplémentaires sur la fonctionnalité NLB.  
  
|Type de contenu|Références|  
|----------------|--------------|  
|Déploiement|[Guide](https://technet.microsoft.com/library/cc754833(WS.10).aspx) &#124; de déploiement de l’équilibrage de charge réseau [configuration de l’équilibrage de charge réseau avec les services Terminal Server](https://technet.microsoft.com/library/cc771300(v=WS.10).aspx)|  
|Opérations|[Gestion des clusters](https://technet.microsoft.com/library/cc753954(WS.10).aspx) &#124; d’équilibrage de charge réseau [définition des paramètres](https://technet.microsoft.com/library/cc731619(WS.10).aspx) &#124; d’équilibrage de charge réseau [contrôle des ordinateurs hôtes sur les clusters d’équilibrage de charge réseau](https://technet.microsoft.com/library/cc770870(WS.10).aspx)|  
|Résolution des problèmes|[Résolution des problèmes](https://technet.microsoft.com/library/cc732592(WS.10).aspx) &#124; liés aux [événements et erreurs de cluster](https://technet.microsoft.com/library/cc731678(WS.10).aspx) d’équilibrage de charge réseau|
|Outils et paramètres|[Applets de commande Windows PowerShell d’équilibrage de la charge réseau](https://go.microsoft.com/fwlink/p/?LinkId=238123)|
|Ressources de la communauté|[Forum haute disponibilité \(Clustering @ no__t-2](https://go.microsoft.com/fwlink/p/?LinkId=230641)
