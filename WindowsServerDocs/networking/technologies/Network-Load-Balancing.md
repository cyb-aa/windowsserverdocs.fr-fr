---
title: Équilibrage de charge réseau
description: Cette rubrique fournit une vue d’ensemble de la fonctionnalité d’équilibrage de charge réseau (NLB) dans Windows Server 2016 et inclut des liens vers des informations supplémentaires sur la création, la configuration et la gestion des clusters d’équilibrage de charge réseau.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-nlb
ms.topic: article
ms.assetid: 244a4b48-06e5-4796-8750-a50e4f88ac72
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b9fd39381316a8bcd06328e7aa75492ed99bc7f1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="network-load-balancing"></a>Équilibrage de charge réseau

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique fournit une vue d’ensemble de la fonctionnalité \(NLB\) équilibrage de charge réseau dans Windows Server 2016 et inclut des liens vers des informations supplémentaires sur la création, la configuration et la gestion des clusters d’équilibrage de charge réseau.

Vous pouvez utiliser l’équilibrage de charge réseau pour gérer les deux ou plusieurs serveurs en tant qu’un cluster virtuel unique. Équilibrage de charge réseau améliore la disponibilité et l’extensibilité des applications de serveur Internet tels que ceux utilisés sur le web, FTP, de pare-feu, proxy, réseau privé virtuel \(VPN\) et autres serveurs stratégiques mission\ réseau.   

>[!NOTE]
>Windows Server 2016 inclut une nouvelle inspirée du langage Azure équilibreur de charge logicielle \(SLB\) en tant que composant de l’infrastructure \(SDN\) logiciel défini de mise en réseau. Utilisation SLB au lieu de l’équilibrage de charge réseau si vous utilisez SDN, sont à l’aide de charges de travail autres que Windows, le besoin de traduction d’adresses réseau sortant \(NAT\), ou besoin de couche 3 \(L3\) ou l’équilibrage de charge non TCP. Vous pouvez continuer à utiliser l’équilibrage de charge réseau avec Windows Server 2016 pour les déploiements non SDN. Pour plus d’informations sur les différentes, voir [l’équilibrage de charge logiciels (SLB) pour SDN](../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).

La fonctionnalité \(NLB\) équilibrage de charge réseau répartit le trafic sur plusieurs serveurs à l’aide du protocole réseau TCP\/IP. En combinant les deux ou plusieurs ordinateurs qui exécutent des applications dans un cluster virtuel unique, équilibrage de charge réseau fournit la fiabilité et performances des serveurs web et autres serveurs stratégiques mission\.  
  
Les serveurs dans un cluster d’équilibrage de charge réseau sont appelés *hôtes*, et chaque hôte exécute une copie distincte des applications serveur. Équilibrage de charge réseau répartit les demandes clientes entrantes sur les ordinateurs hôtes du cluster. Vous pouvez configurer la charge doit être gérée par chaque hôte. Vous pouvez également ajouter des hôtes de façon dynamique le cluster pour traiter une charge accrue. Équilibrage de charge réseau peut également diriger tout le trafic vers un seul hôte désigné, appelé le *hôte par défaut*.  
  
Équilibrage de charge réseau permet à tous les ordinateurs dans le cluster à l’aide du même ensemble d’adresses IP et maintient un ensemble d’adresses IP dédiées uniques pour chaque ordinateur hôte. Pour les applications load\ équilibrée, lorsqu’un ordinateur hôte échoue ou est mis hors connexion, la charge est automatiquement redistribuée entre les ordinateurs qui fonctionnent toujours. Lorsqu’il est prêt, l’ordinateur hors connexion peut rejoindre le cluster de manière transparente et récupérer sa part de la charge de travail, ce qui permet aux autres ordinateurs du cluster de traiter moins de trafic.  
  
## <a name="BKMK_APP"></a>Applications pratiques  
Équilibrage de charge réseau est utile pour s’assurer que les applications sans état, tels que les serveurs web exécutant Internet Information Services \(IIS\), sont disponibles avec un temps d’arrêt minimal et qu’elles sont extensibles \ (en ajoutant des serveurs supplémentaires en tant que la charge increases\). Les sections suivantes décrivent comment équilibrage de charge réseau prend en charge la haute disponibilité, d’extensibilité et la facilité de gestion des serveurs en cluster qui exécutent ces applications.  
  
### <a name="high-availability"></a>Haute disponibilité  
Un système à haute disponibilité offre de manière fiable un niveau de service avec un temps d’arrêt minimal acceptable. Pour fournir une haute disponibilité, équilibrage de charge réseau inclut les fonctionnalités d’intégrée qui peuvent automatiquement:  
  
-   Détecter un hôte de cluster en panne ou hors connexion, puis récupérer.  
  
-   Équilibrer la charge réseau lorsque les ordinateurs hôtes sont ajoutés ou supprimés.  
  
-   Récupérer et redistribuer la charge de travail dans les dix secondes.  
  
### <a name="scalability"></a>Extensibilité  
Extensibilité est la mesure de la capacité d’un ordinateur, un service ou une application à répondre aux demandes croissantes des performances. Pour les clusters d’équilibrage de charge réseau, l’évolutivité est la possibilité d’ajouter un ou plusieurs systèmes à un cluster existant lorsque la charge totale du cluster dépasse ses capacités. Pour prendre en charge l’évolutivité, vous pouvez effectuer les opérations Listées ci-après:  
  
-   Équilibrer les demandes de charge dans le cluster NLB pour les services TCP\/IP individuels.  
  
-   Prend en charge jusqu'à 32 ordinateurs dans un seul cluster.  
  
-   Équilibrer plusieurs demandes de charge de serveur \ (à partir du même client ou à partir de plusieurs clients\) sur plusieurs ordinateurs hôtes du cluster.  
  
-   Ajouter des hôtes au cluster NLB en tant que la charge augmente, sans entraîner l’échec du cluster.  
  
-   Supprimez les ordinateurs hôtes du cluster lorsque la charge diminue.  
  
-   Activer des performances élevées et faible temps système via une mise en œuvre entièrement en pipeline. Traitement en pipeline permet les demandes d’envoi vers le cluster d’équilibrage de charge réseau sans attendre une réponse à une demande précédente.  
  
### <a name="manageability"></a>Facilité de gestion  
Pour prendre en charge de la facilité de gestion, vous pouvez effectuer les opérations Listées ci-après:  
  
-   Gérer et configurer plusieurs clusters d’équilibrage de charge réseau et les hôtes du cluster à partir d’un seul ordinateur à l’aide du Gestionnaire NLB ou le [applets de commande de l’équilibrage de charge réseau (NLB) dans Windows PowerShell](https://technet.microsoft.com/library/hh801274.aspx).
  
-   Spécifiez l’équilibrage de comportement pour un seul port IP ou un groupe de ports à l’aide de règles de gestion de port de charge.  
  
-   Définir les règles de port différent pour chaque site Web. Si vous utilisez le même ensemble de serveurs load\ équilibrée pour plusieurs applications ou sites Web, les règles de port sont basées sur l’adresse IP virtuelle de destination \(using virtual clusters\).  

-   Diriger toutes les demandes clientes vers un seul hôte à l’aide de facultatif, les règles de single\-host. Équilibrage de charge réseau route les demandes clientes vers un hôte particulier qui exécute des applications spécifiques.  

-   Bloquer l’accès réseau à certains ports IP.  

-   Prise en charge Internet Group Management Protocol \(IGMP\) sur les hôtes du cluster aux ports commutés \ (où les paquets réseau entrants sont envoyés à tous les ports sur le switch\) lorsqu’il fonctionne en mode de multidiffusion.  

-   Démarrer, arrêter et contrôler les actions de l’équilibrage de charge réseau à distance à l’aide des commandes Windows PowerShell ou des scripts.  

-   Afficher le journal des événements Windows pour vérifier les événements d’équilibrage de charge réseau. Équilibrage de charge réseau consigne toutes les actions et modifications de cluster dans le journal des événements.  

## <a name="important-functionality"></a>Fonctionnalités importantes  
 
Équilibrage de charge réseau est installé comme un composant du pilote réseau Windows Server standard. Ses opérations sont transparentes pour la pile de mise en réseau TCP\/IP. La figure suivante illustre la relation entre l’équilibrage de charge réseau et autres composants logiciels dans une configuration standard.  
  
![Équilibrage de charge réseau et autres composants logiciels](../media/NLB/nlb.jpg)  
  
Voici les principales fonctionnalités de l’équilibrage de charge réseau.  
  
- Ne nécessite aucune modification matérielle à exécuter.  
  
- Fournit des outils d’équilibrage de la charge réseau pour configurer et gérer plusieurs clusters et tous les ordinateurs hôtes à partir d’un seul ordinateur local ou distant.  
  
- Permet aux clients d’accéder au cluster à l’aide d’un seul nom Internet logique et l’adresse IP virtuelle, appelée adresse IP du cluster \ (elle conserve des noms individuels pour chaque computer\). Équilibrage de charge réseau autorise plusieurs adresses IP virtuelles pour les serveurs multirésidents.  
  
> [!NOTE]  
> Lorsque vous déployez des ordinateurs virtuels en tant que clusters virtuels, équilibrage de charge réseau ne nécessite pas de serveurs multirésidents avec plusieurs adresses IP virtuelles.  
  
- Permet de NLB pour être lié à plusieurs cartes réseau, qui vous permet de configurer plusieurs clusters indépendants sur chaque hôte. Prise en charge de plusieurs cartes réseau diffère des clusters virtuels dans des clusters virtuels vous autorise à configurer plusieurs clusters sur une seule carte réseau.  
  
- Ne requiert aucune modification pour les applications serveur afin qu’ils puissent exécuter dans un cluster NLB.  
  
- Peut être configuré pour ajouter automatiquement un ordinateur hôte au cluster si cet hôte du cluster échoue et est ensuite remis en ligne. L’hôte ajouté peut commencer à gérer les nouvelles demandes de serveur à partir de clients.  
  
-   Vous permet de mettre des ordinateurs hors connexion pour la maintenance préventive sans perturber les opérations de cluster sur les autres ordinateurs hôtes.  
  
## <a name="BKMK_HARD"></a>Configuration matérielle requise  
Voici la configuration matérielle requise pour l’exécution d’un cluster d’équilibrage de charge réseau.  
  
-   Tous les ordinateurs hôtes du cluster doivent résider sur le même sous-réseau.  
  
-   Il n’existe aucune restriction sur le nombre de cartes réseau sur chaque ordinateur hôte, et des hôtes différents peuvent avoir un nombre différent de cartes.  
  
-   Dans chaque cluster, toutes les cartes réseau doivent être soit multidiffusion, soit monodiffusion. Équilibrage de charge réseau ne prend pas en charge un environnement mixte de multidiffusion et de monodiffusion au sein d’un seul cluster.  
  
-   Si vous utilisez le mode de monodiffusion, la carte réseau qui est utilisée pour gérer le trafic client-cluster celle-ci doit prendre en charge la modification de son adresse media access control \(MAC\).  
  
## <a name="BKMK_SOFT"></a>Configuration logicielle requise  
Voici la configuration logicielle requise pour l’exécution d’un cluster d’équilibrage de charge réseau.  
  
-   TCP\/IP uniquement peut être utilisé sur la carte pour lesquels l’équilibrage de charge réseau est activé sur chaque ordinateur hôte. Ne pas ajouter d’autres protocoles \ (par exemple, IPX\) pour cette carte.  
  
-   Les adresses IP des serveurs du cluster doivent être statiques.  
  
> [!NOTE]  
> Équilibrage de charge réseau ne prend pas en charge \(DHCP\) Dynamic Host Configuration Protocol. Équilibrage de charge réseau désactive le protocole DHCP sur chaque interface configurée.  
  
## <a name="BKMK_INSTALL"></a>Informations d’installation  
Vous pouvez installer l’équilibrage de charge réseau à l’aide du Gestionnaire de serveur ou les commandes Windows PowerShell pour l’équilibrage de charge réseau.

Si vous le souhaitez, vous pouvez installer les outils l’équilibrage de charge réseau pour gérer un cluster d’équilibrage de charge réseau local ou distant. Les outils incluent le Gestionnaire d’équilibrage de charge réseau et les commandes de l’équilibrage de charge réseau Windows PowerShell.

### <a name="installation-with-server-manager"></a>Installation avec le Gestionnaire de serveur

Dans le Gestionnaire de serveur, vous pouvez utiliser l’ajout de rôles et fonctionnalités Assistant pour ajouter le **équilibrage de charge réseau** fonctionnalité. Lorsque vous terminez l’Assistant, équilibrage de charge réseau est installé, et vous n’avez pas besoin de redémarrer l’ordinateur.


### <a name="installation-with-windows-powershell"></a>Installation avec Windows PowerShell  

Pour installer l’équilibrage de charge réseau à l’aide de Windows PowerShell, exécutez la commande suivante à une invite de commandes Windows PowerShell avec élévation de privilèges sur l’ordinateur sur lequel vous souhaitez installer l’équilibrage de charge réseau.

    
    Install-WindowsFeature NLB -IncludeManagementTools
    
Une fois l’installation terminée, aucun redémarrage de l’ordinateur n’est nécessaire.

Pour plus d’informations, voir [Install-WindowsFeature](https://technet.microsoft.com/library/jj205467.aspx).

### <a name="network-load-balancing-manager"></a>Gestionnaire d’équilibrage de charge réseau
Pour ouvrir le Gestionnaire d’équilibrage de charge réseau dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **Gestionnaire d’équilibrage de charge réseau**.
  
## <a name="BKMK_LINKS"></a>Ressources supplémentaires  
Le tableau suivant fournit des liens vers des informations supplémentaires sur la fonctionnalité d’équilibrage de charge réseau.  
  
|Type de contenu|Références|  
|----------------|--------------|  
|Déploiement|[Guide de déploiement d’équilibrage de charge du réseau](https://technet.microsoft.com/library/cc754833(WS.10).aspx) & #124; [La configuration réseau équilibrage de charge avec les Services Terminal Server](https://technet.microsoft.com/library/cc771300(v=WS.10).aspx)|  
|Opérations|[La gestion des Clusters d’équilibrage de charge réseau](https://technet.microsoft.com/library/cc753954(WS.10).aspx) & #124; [Définition des paramètres de l’équilibrage de charge réseau](https://technet.microsoft.com/library/cc731619(WS.10).aspx) & #124; [Contrôle des hôtes sur les Clusters d’équilibrage de charge réseau](https://technet.microsoft.com/library/cc770870(WS.10).aspx)|  
|Résolution des problèmes|[Résolution des problèmes de Clusters d’équilibrage de charge réseau](https://technet.microsoft.com/library/cc732592(WS.10).aspx) & #124; [Erreurs et événements de Cluster d’équilibrage de charge réseau](https://technet.microsoft.com/library/cc731678(WS.10).aspx)|
|Outils et paramètres|[Applets de commande PowerShell de Windows d’équilibrage de la charge réseau](https://go.microsoft.com/fwlink/p/?LinkId=238123)|
|Ressources de la Communauté|[Forum sur la haute disponibilité \(Clustering\)](https://go.microsoft.com/fwlink/p/?LinkId=230641)