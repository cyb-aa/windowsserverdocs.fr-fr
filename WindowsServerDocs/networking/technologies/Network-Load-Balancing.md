---
title: Équilibrage de la charge réseau
description: Dans cette rubrique, nous vous fournissons une vue d’ensemble de l’équilibrage de charge réseau \(NLB\) fonctionnalité dans Windows Server 2016. Vous pouvez utiliser l’équilibrage de charge réseau pour gérer deux ou plusieurs serveurs en tant qu’un seul cluster virtuel. Équilibrage de charge réseau améliore la disponibilité et l’évolutivité des applications de serveur Internet tels que ceux utilisés sur le web, FTP, de pare-feu, proxy, réseau privé virtuel \(VPN\)et autres mission\-serveurs critiques.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-nlb
ms.topic: article
ms.assetid: 244a4b48-06e5-4796-8750-a50e4f88ac72
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: d0cf1e1d6b1681a0f18908b08cd17572159e0462
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881750"
---
# <a name="network-load-balancing"></a>Équilibrage de la charge réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette rubrique, nous vous fournissons une vue d’ensemble de l’équilibrage de charge réseau \(NLB\) fonctionnalité dans Windows Server 2016. Vous pouvez utiliser l’équilibrage de charge réseau pour gérer deux ou plusieurs serveurs en tant qu’un seul cluster virtuel. Équilibrage de charge réseau améliore la disponibilité et l’évolutivité des applications de serveur Internet tels que ceux utilisés sur le web, FTP, de pare-feu, proxy, réseau privé virtuel \(VPN\)et autres mission\-serveurs critiques.  

>[!NOTE]
>Windows Server 2016 inclut un nouvel équilibrage de charge logiciel d’inspirée d’Azure \(SLB\) en tant que composant de la Software-Defined Networking \(SDN\) infrastructure. Utilisez SLB au lieu de l’équilibrage de charge réseau si vous utilisez SDN, sont à l’aide de charges de travail non Windows, le besoin de traduction d’adresses réseau sortantes \(NAT\), ou vous avez besoin de couche 3 \(L3\) ou l’équilibrage de charge non TCP. Vous pouvez continuer à utiliser l’équilibrage de charge réseau avec Windows Server 2016 pour les déploiements non SDN. Pour plus d’informations sur SLB, consultez [d’équilibrage de charge logiciel (SLB) pour SDN](../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).

L’équilibrage de charge réseau \(NLB\) fonctionnalité répartit le trafic sur plusieurs serveurs à l’aide de TCP\/protocole réseau IP. En combinant deux ou plusieurs ordinateurs qui exécutent des applications dans un cluster virtuel unique, équilibrage de charge réseau assure la fiabilité et les performances des serveurs web et autres mission\-serveurs critiques.  
  
Les serveurs d’un cluster d’équilibrage de la charge réseau sont appelés *hôtes* et chacun d’eux exécute une copie distincte des applications de serveurs. L’équilibrage de la charge réseau répartit les demandes de client entrantes entre les hôtes du cluster. Vous pouvez configurer la charge qui doit être gérée par chaque hôte. Vous pouvez également ajouter des hôtes de façon dynamique au cluster afin de traiter une charge accrue. L’équilibrage de la charge réseau dirige également tout le trafic vers un seul hôte désigné, appelé *hôte par défaut*.  
  
L’équilibrage de la charge réseau permet de traiter tous les ordinateurs du cluster à l’aide du même ensemble d’adresses IP. Il permet également de conserver un ensemble d’adresses IP dédiées uniques pour chaque hôte. Pour la charge\-à charge équilibrée des applications, lorsqu’un ordinateur hôte échoue ou est hors connexion, la charge est automatiquement redistribuée entre les ordinateurs qui fonctionnent toujours. Lorsqu’il est prêt, l’ordinateur hors connexion peut rejoindre de manière transparente le cluster et récupérer sa part de la charge, ce qui permet aux autres ordinateurs du cluster de traiter moins de trafic.  
  
## <a name="practical-applications"></a>Cas pratiques  
NLB est utile pour s’assurer que sans état applications, telles que des serveurs web exécutant Internet Information Services \(IIS\), sont disponibles avec un temps mort minimal, et qu’elles sont extensibles \(en ajoutant des serveurs en tant que la charge augmente\). Les sections suivantes expliquent comment l’équilibrage de la charge réseau prend en charge la haute disponibilité, l’extensibilité et la simplicité de gestion des serveurs en cluster qui exécutent ces applications.  
  
### <a name="high-availability"></a>Haute disponibilité  
Un système à haute disponibilité offre de manière fiable un niveau de service acceptable avec un temps d’immobilisation très faible. Pour fournir une haute disponibilité, NLB inclut généré\-qui peut automatiquement des fonctionnalités :  
  
-   détecter un hôte de cluster en panne ou hors connexion, puis le récupérer ;  
  
-   équilibrer la charge réseau lors de l’ajout ou de la suppression d’hôtes ;  
  
-   récupérer et redistribuer la charge dans un délai de dix secondes.  
  
### <a name="scalability"></a>Extensibilité  
L’extensibilité est la mesure de la capacité d’un ordinateur, d’un service ou d’une application à répondre à des besoins croissants de performances. Pour les clusters d’équilibrage de la charge réseau, l’extensibilité correspond à la possibilité d’ajouter un ou plusieurs systèmes à un cluster existant lorsque la charge totale du cluster dépasse ses capacités. Pour assurer cette extensibilité, l’équilibrage de la charge réseau vous permet d’effectuer les opérations listées ci-après :  
  
-   Équilibrer les demandes de charge au sein du cluster NLB pour TCP individuel\/services IP.  
  
-   Prendre en charge jusqu’à 32 ordinateurs dans un seul cluster.  
  
-   Équilibrer plusieurs demandes de charge de serveur \(à partir du même client ou de plusieurs clients\) sur plusieurs ordinateurs hôtes du cluster.  
  
-   Ajouter des hôtes au cluster d’équilibrage de la charge réseau au fur et à mesure de l’augmentation de la charge, sans entraîner un échec du cluster.  
  
-   Supprimer des hôtes du cluster lorsque la charge diminue.  
  
-   Permettre des performances élevées et un faible temps système via une mise en œuvre entièrement en pipeline. Le traitement en pipeline permet d’envoyer les demandes au cluster d’équilibrage de la charge réseau sans attendre la réponse à une demande précédente.  
  
### <a name="manageability"></a>Facilité de gestion  
Pour assurer cette simplicité de gestion, l’équilibrage de la charge réseau vous permet d’effectuer les opérations listées ci-après :  
  
-   Gérer et configurer plusieurs clusters d’équilibrage de charge réseau et les hôtes du cluster à partir d’un seul ordinateur à l’aide du Gestionnaire NLB ou le [applets de commande d’équilibrage de charge réseau (NLB) dans Windows PowerShell](https://technet.microsoft.com/library/hh801274.aspx).
  
-   Spécifier le comportement d’équilibrage de la charge pour un seul port IP ou un groupe de ports à l’aide de règles de gestion des ports.  
  
-   Définir des règles de port différentes pour chaque site Web. Si vous utilisez le même ensemble de la charge\-des serveurs à charge équilibrée pour plusieurs applications ou sites Web, les règles de port sont basés sur l’adresse IP virtuelle de destination \(à l’aide de clusters virtuels\).  

-   Diriger toutes les demandes de client vers un seul hôte à l’aide de facultatif unique\-héberger les règles. L’équilibrage de la charge réseau route les demandes clientes vers un hôte particulier qui exécute des applications spécifiques.  

-   Bloquer l’accès réseau non souhaité à certains ports IP.  

-   Activer Internet Group Management Protocol \(IGMP\) prise en charge sur les hôtes du cluster pour contrôler les ports commutés \(où les paquets réseau entrants sont envoyés à tous les ports sur le commutateur\) lors du fonctionnement dans mode de multidiffusion.  

-   Démarrer, arrêter et contrôler les actions d’équilibrage de la charge réseau à distance, à l’aide de commandes ou de scripts Windows PowerShell.  

-   Afficher le journal des événements Windows pour vérifier les événements d’équilibrage de la charge réseau. L’équilibrage de la charge réseau consigne toutes les actions et modifications du cluster dans le journal des événements.  

## <a name="important-functionality"></a>Fonctionnalités importantes  
 
NLB est installé comme un composant de pilote de mise en réseau Windows Server standard. Ses opérations sont transparentes pour le protocole TCP\/pile réseau IP. La figure suivante montre la relation entre NLB et les autres composants logiciels dans une configuration standard.  
  
![Équilibrage de charge réseau et d’autres composants logiciels](../media/NLB/nlb.jpg)  
  
Voici les principales fonctionnalités d’équilibrage de charge réseau.  
  
- Elle ne nécessite aucune modification du matériel pour pouvoir s’exécuter.  
  
- Elle fournit des outils d’équilibrage de la charge réseau pour configurer et gérer plusieurs clusters, ainsi que tous les hôtes de cluster, à partir d’un seul ordinateur distant ou local.  
  
- Permet aux clients d’accéder au cluster à l’aide d’un seul nom Internet logique et l’adresse IP virtuelle, qui est appelé à l’adresse IP de cluster \(il conserve des noms individuels pour chaque ordinateur\). L’équilibrage de la charge réseau autorise plusieurs adresses IP virtuelles pour les serveurs multirésidents.  
  
> [!NOTE]  
> Lorsque vous déployez des machines virtuelles en tant que clusters virtuels, NLB ne nécessite pas les serveurs multirésidents avec plusieurs adresses IP virtuelles.  
  
- L’équilibrage de la charge réseau peut être lié à plusieurs cartes réseau, ce qui vous permet de configurer plusieurs clusters indépendants sur chaque hôte. La prise en charge de plusieurs cartes réseau diffère des clusters virtuels en ce sens que les clusters virtuels vous permettent de configurer plusieurs clusters sur une seule carte réseau.  
  
- Elle ne nécessite aucune modification des applications serveur pour que ces dernières puissent s’exécuter dans un cluster d’équilibrage de la charge réseau.  
  
- Elle peut être configurée pour ajouter automatiquement un hôte au cluster si cet hôte de cluster tombe en panne et s’il est ensuite remis en ligne. L’hôte ajouté peut démarrer le traitement des nouvelles demandes adressées au serveur par les clients.  
  
-   Elle vous permet de mettre des ordinateurs hors connexion à des fins de maintenance préventive sans perturber les opérations de cluster sur les autres hôtes.  
  
## <a name="hardware-requirements"></a>Configuration matérielle requise  
Voici la configuration matérielle requise pour exécuter un cluster NLB.  
  
-   Tous les hôtes du cluster doivent résider sur le même sous-réseau.  
  
-   Il n’existe aucune restriction sur le nombre de cartes réseau de chaque hôte. Par ailleurs, les différents hôtes peuvent avoir un nombre distinct de cartes.  
  
-   Au sein de chaque cluster, toutes les cartes réseau doivent être soit multidiffusion, soit monodiffusion. L’équilibrage de la charge réseau ne prend pas en charge un environnement mixte de multidiffusion et de monodiffusion au sein d’un seul cluster.  
  
-   Si vous utilisez le mode de monodiffusion, la carte réseau qui est utilisé pour gérer le client\-à\-le trafic de cluster doit prendre en charge la modification de son contrôle d’accès media \(MAC\) adresse.  
  
## <a name="software-requirements"></a>Configuration logicielle requise  
Voici la configuration logicielle requise pour exécuter un cluster NLB.  
  
-   Seul TCP\/IP peut être utilisé sur la carte pour lequel NLB est activé sur chaque hôte. N’ajoutez pas d’autres protocoles \(, par exemple, IPX\) à cet adaptateur.  
  
-   Les adresses IP des serveurs du cluster doivent être statiques.  
  
> [!NOTE]  
> NLB ne prend pas en charge Dynamic Host Configuration Protocol \(DHCP\). L’équilibrage de la charge réseau désactive le protocole DHCP sur chaque interface configurée.  
  
## <a name="installation-information"></a>Informations sur l’installation  
Vous pouvez installer l’équilibrage de charge réseau à l’aide du Gestionnaire de serveur ou les commandes Windows PowerShell pour l’équilibrage de charge réseau.

Vous pouvez éventuellement installer les outils d’équilibrage de la charge réseau pour gérer un cluster d’équilibrage de la charge réseau local ou distant. Les outils incluent le Gestionnaire d’équilibrage de charge réseau et les commandes de l’équilibrage de charge réseau Windows PowerShell.

### <a name="installation-with-server-manager"></a>Installation avec le Gestionnaire de serveur

Dans le Gestionnaire de serveur, vous pouvez utiliser l’ajout de rôles et fonctionnalités Assistant pour ajouter le **équilibrage de charge réseau** fonctionnalité. Lorsque vous terminez l’Assistant, NLB est installé, et vous n’avez pas besoin de redémarrer l’ordinateur.


### <a name="installation-with-windows-powershell"></a>Installation avec Windows PowerShell  

Pour installer l’équilibrage de charge réseau à l’aide de Windows PowerShell, exécutez la commande suivante à une invite de commandes Windows PowerShell avec élévation de privilèges sur l’ordinateur où vous souhaitez installer NLB.

    
    Install-WindowsFeature NLB -IncludeManagementTools
    
Une fois l’installation terminée, aucun redémarrage de l’ordinateur n’est nécessaire.

Pour plus d’informations, voir [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/install-windowsfeature?view=win10-ps).

### <a name="network-load-balancing-manager"></a>Gestionnaire d’équilibrage de charge réseau
Pour ouvrir le Gestionnaire d’équilibrage de la charge réseau dans le Gestionnaire de serveur, cliquez sur **Outils**, puis sur **Gestionnaire d’équilibrage de la charge réseau**.
  
## <a name="additional-resources"></a>Ressources supplémentaires  
Le tableau suivant fournit des liens vers des informations supplémentaires sur la fonctionnalité d’équilibrage de charge réseau.  
  
|Type de contenu|Références|  
|----------------|--------------|  
|Déploiement|[Guide de déploiement d’équilibrage de charge réseau](https://technet.microsoft.com/library/cc754833(WS.10).aspx) &#124; [configuration réseau équilibrage de charge avec les Services Terminal Server](https://technet.microsoft.com/library/cc771300(v=WS.10).aspx)|  
|Opérations|[La gestion des Clusters d’équilibrage de la charge réseau](https://technet.microsoft.com/library/cc753954(WS.10).aspx) &#124; [définissant les paramètres d’équilibrage de charge réseau](https://technet.microsoft.com/library/cc731619(WS.10).aspx) &#124; [contrôle des hôtes sur les Clusters d’équilibrage de la charge réseau](https://technet.microsoft.com/library/cc770870(WS.10).aspx)|  
|Résolution des problèmes|[Dépannage des Clusters d’équilibrage de la charge réseau](https://technet.microsoft.com/library/cc732592(WS.10).aspx) &#124; [erreurs et événements de Cluster d’équilibrage de charge réseau](https://technet.microsoft.com/library/cc731678(WS.10).aspx)|
|Outils et paramètres|[Applets de commande PowerShell de Windows de l’équilibrage de charge réseau](https://go.microsoft.com/fwlink/p/?LinkId=238123)|
|Ressources de la communauté|[Haute disponibilité \(Clustering\) Forum](https://go.microsoft.com/fwlink/p/?LinkId=230641)
