---
ms.assetid: a6343f1c-e9dd-4a02-91ad-39bd519d66cd
title: "Les réseaux de Cluster de plusieurs cartes réseau et SMB Multichannel simplifiés"
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: RobHindman
ms.author: robhind
ms.date: 09/15/2016
ms.openlocfilehash: 45d8364adf9d3db24a8e6d8f7bc91178ce7d1551
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="simplified-smb-multichannel-and-multi-nic-cluster-networks"></a>Les réseaux de Cluster de plusieurs cartes réseau et SMB Multichannel simplifiés

> S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Simplifié SMB Multichannel et Multi -<abbr title="carte d’Interface réseau">Carte réseau</abbr> réseaux de Cluster est une nouvelle fonctionnalité dans Windows Server2016 qui permet d’utiliser plusieurs cartes réseau sur le même sous-réseau de réseau de cluster et active automatiquement SMB Multichannel.  

Les réseaux de Cluster de plusieurs cartes réseau et SMB Multichannel offre les avantages suivants:  
- Le Clustering de basculement reconnaît automatiquement toutes les cartes réseau sur les nœuds qui sont à l’aide de la même commutateur / même sous-réseau - aucune configuration supplémentaire requise.  
- SMB Multichannel est activé automatiquement.  
- Réseaux uniquement à des ressources d’adresses IP locales IPv6 lien (fe80) sont reconnus sur les réseaux de cluster uniquement (privés).  
- Une ressource d’adresse IP unique est configurée sur chaque nom de réseau de Cluster de Point d’accès (NN) par défaut.  
- Validation du cluster n’est plus émet des messages d’avertissement lorsque plusieurs cartes réseau se trouvent sur le même sous-réseau.  

## <a name="requirements"></a>Configuration requise  
-   Plusieurs cartes réseau par serveur, à l’aide de la même commutateur / sous-réseau.  

## <a name="how-to-take-advantage-of-multi-nic-clusters-networks-and-simplified-smb-multichannel"></a>Tirer parti de plusieurs cartes réseau de clusters réseaux et SMB multichannel simplifiés  
Cette section décrit comment tirer parti des fonctionnalités de multicanaux SMB simplifiées et nouveaux réseaux de clusters de plusieurs cartes réseau dans Windows Server2016.  

### <a name="use-at-least-two-networks-for-failover-clustering"></a>Utiliser au moins deux réseaux pour le Clustering de basculement   
Bien qu’il soit rare, commutateurs réseau peuvent échouer, il est toujours recommandé d’utiliser au moins deux réseaux pour le Clustering de basculement. Tous les réseaux qui sont trouvés sont utilisés pour les pulsations du cluster. Évitez d’utiliser un réseau unique pour votre Cluster de basculement afin d’éviter un point de défaillance unique. Dans l’idéal, il doit y être plusieurs chemins d’accès physiques de communication entre les nœuds du cluster et aucun point de défaillance unique.  

![Illustration de deux réseaux pour le Clustering de basculement](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig1.png)  
**Figure1: Utiliser au moins deux réseaux pour le Clustering de basculement**  

### <a name="use-multiple-nics-across-clusters"></a>Utilisez plusieurs cartes réseau entre des clusters  

Meilleur parti de la SMB multichannel simplifiés est atteint lorsque plusieurs cartes réseau sont utilisées au sein de clusters - stockage et de clusters de la charge de travail de stockage. Ainsi, les clusters de la charge de travail (Hyper-V, Instance de Cluster de basculement SQLServer, le réplica de stockage, etc.) pour utiliser SMB multichannel et les résultats dans une utilisation plus efficace du réseau. Dans un convergé (également appelé désagrégé) où un cluster de serveur de fichiers avec montée en puissance parallèle est utilisé pour stocker les données de la charge de travail pour Hyper-V ou cluster de l’Instance de Cluster de basculement SQLServer, ce réseau est souvent appelé «le sous-réseau nord sud» de configuration du cluster et du réseau. De nombreux clients optimiser le débit de ce réseau à investir dans les commutateurs et les cartes réseau compatibles RDMA.  

![Illustration d’un sous-réseau nord sud SMB](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig2.png)  
**Figure2: Pour obtenir le débit maximal du réseau, utilisez plusieurs cartes réseau sur le cluster de serveur de fichiers avec montée en puissance parallèle et le cluster Hyper-V ou Instance de Cluster de basculement SQLServer - qui partagent le sous-réseau nord sud**  

![ScreenCap de deux clusters à l’aide de plusieurs cartes réseau dans le même sous-réseau pour tirer parti de SMB multichannel](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig3.png)  
**Figure3: Deux clusters (serveur de fichiers avec montée en puissance parallèle pour le stockage, SQLServer <abbr title="Instance de Clustering de basculement">l’ICF</abbr> pour la charge de travail) utilisent plusieurs cartes réseau dans le même sous-réseau pour tirer parti de SMB Multichannel et obtenir le meilleur débit du réseau.** 

## <a name="automatic-recognition-of-ipv6-link-local-private-networks"></a>Reconnaissance automatique de réseaux privés IPv6 lien Local  
Lorsque des réseaux privés (cluster uniquement) avec plusieurs cartes réseau sont détectées, le cluster reconnaître automatiquement des adresses IP locales IPv6 lien (fe80) pour chaque carte réseau sur chaque sous-réseau. Cet administrateurs gagner du temps dans la mesure où ils n’ont plus à configurer manuellement des ressources de l’adresse IP locale IPv6 lien (fe80).  

Lorsque vous utilisez plusieurs réseaux privés (cluster uniquement), vérifiez la configuration de routage IPv6 pour vous assurer que le routage n’est pas configuré pour franchir les sous-réseaux, dans la mesure où cela permettra de réduire les performances du réseau.  

![ScreenCap de configuration automatique de réseau dans le Gestionnaire du Cluster de basculement UI](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig4.png)  
**Figure4: Configuration des ressources automatique des adresses IPv6 lien Local (fe80)**  

## <a name="throughput-and-fault-tolerance"></a>Débit et la tolérance de pannes  
Windows Server2016 automatiquement détecte les fonctionnalités des cartes réseau et essaiera d’utiliser chaque carte réseau dans la configuration possible plus rapide. Cartes réseau qui sont associées, les cartes réseau à l’aide de RSS et les cartes réseau avec la fonction RDMA peuvent être utilisés. Le tableau ci-dessous récapitule les compromis lors de l’utilisation de ces technologies. Débit maximal est obtenu lors de l’utilisation de plusieurs cartes réseau compatibles de RDMA. Pour plus d’informations, voir [les principes fondamentaux de SMB Mutlichannel](https://blogs.technet.microsoft.com/josebda/2012/06/28/the-basics-of-smb-multichannel-a-feature-of-windows-server-2012-and-smb-3-0/).

![Obtenir une illustration de débit et tolérance de panne pour différentes configurations de cartes réseau](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig5.png)  
**Figure5: Débit et une tolérance de différentes cartes réseau conifigurations**   

## <a name="frequently-asked-questions"></a>Forum aux questions  
**Toutes les cartes réseau dans un réseau de plusieurs cartes réseau sont utilisés pour les pulsations de cluster?**  
    Oui.  

**Un réseau de plusieurs cartes réseau sont utilisables pour les communications de cluster uniquement? Ou peut uniquement être utilisé pour la communication client et le cluster?**  
    Soit configuration fonctionnera - fonctionnent tous les rôles de réseau de cluster sur un réseau de plusieurs cartes réseau.  

**SMB Multichannel est également utilisé pour le trafic CSV et du cluster?**  
    Oui, par défaut tous les clusters et le trafic CSV utilise les réseaux disponibles plusieurs cartes réseau. Les administrateurs peuvent utiliser les applets de commande PowerShell de Clustering de basculement ou le Gestionnaire du Cluster de basculement l’interface utilisateur pour modifier le rôle du réseau.  

**Comment puis-je voir les paramètres SMB Multichannel?**  
    Utilisez le **Get-SMBServerConfiguration** applet de commande, recherchez la valeur de la propriété EnableMultiChannel.  

**Est la propriété commune de cluster que plumballcrosssubnetroutes respectée sur un réseau de plusieurs cartes réseau?**  
     Oui.  

## <a name="see-also"></a>Voir aussi  
- [Nouveautés du Clustering de basculement dans Windows Server](whats-new-in-failover-clustering.md)  
