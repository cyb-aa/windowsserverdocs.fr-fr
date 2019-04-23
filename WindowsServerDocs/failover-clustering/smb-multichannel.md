---
ms.assetid: a6343f1c-e9dd-4a02-91ad-39bd519d66cd
title: Réseaux de clusters à plusieurs cartes réseau et SMB Multichannel simplifiés
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: RobHindman
ms.author: robhind
ms.date: 09/15/2016
ms.openlocfilehash: 45d8364adf9d3db24a8e6d8f7bc91178ce7d1551
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881130"
---
# <a name="simplified-smb-multichannel-and-multi-nic-cluster-networks"></a>Réseaux de clusters à plusieurs cartes réseau et SMB Multichannel simplifiés

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Simplifié SMB Multichannel et Multi -<abbr title="carte d’Interface réseau">Carte réseau</abbr> réseaux de Cluster est une nouvelle fonctionnalité dans Windows Server 2016 qui permet l’utilisation de plusieurs cartes réseau sur le même sous-réseau de réseau de cluster et active automatiquement SMB Multichannel.  

Les réseaux de Cluster de plusieurs cartes réseau et SMB Multichannel simplifiés offre les avantages suivants :  
- Le Clustering de basculement reconnaît automatiquement toutes les cartes réseau sur les nœuds qui sont à l’aide de la même commutateur / même sous-réseau - aucune configuration supplémentaire requise.  
- SMB Multichannel est activé automatiquement.  
- Les réseaux qui ont uniquement des ressources d’adresses IP locales IPv6 lien (fe80) sont reconnues sur les réseaux de cluster uniquement (privés).  
- Une seule ressource d’adresse IP est configurée sur chaque nom de réseau de Cluster de Point d’accès (NN) par défaut.  
- Validation du cluster n’est plus émet des messages d’avertissement lorsque plusieurs cartes réseau se trouvent sur le même sous-réseau.  

## <a name="requirements"></a>Configuration requise  
-   Plusieurs cartes réseau par serveur, à l’aide du même commutateur / sous-réseau.  

## <a name="how-to-take-advantage-of-multi-nic-clusters-networks-and-simplified-smb-multichannel"></a>Tirer parti de plusieurs cartes réseau de clusters réseaux et SMB multichannel simplifiés  
Cette section décrit comment tirer parti des nouveaux réseaux de clusters de plusieurs cartes réseau et des fonctionnalités multicanaux SMB simplifiées dans Windows Server 2016.  

### <a name="use-at-least-two-networks-for-failover-clustering"></a>Utilisez au moins deux réseaux pour le Clustering de basculement   
Bien que cela soit rare, commutateurs réseau peuvent échouer, il est toujours recommandé d’utiliser au moins deux réseaux pour le Clustering de basculement. Tous les réseaux qui sont trouvent sont utilisés pour les pulsations de cluster. Évitez d’utiliser un réseau unique pour votre Cluster de basculement afin d’éviter un point de défaillance unique. Dans l’idéal, il doit y être plusieurs chemins de communication physique entre les nœuds du cluster et aucun point de défaillance unique.  

![Illustration de deux réseaux pour le Clustering de basculement](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig1.png)  
**Figure 1 : Utilisez au moins deux réseaux pour le Clustering de basculement**  

### <a name="use-multiple-nics-across-clusters"></a>Utiliser plusieurs cartes réseau sur des clusters  

Meilleur parti de la SMB multichannel simplifiés est réalisée lorsque plusieurs cartes réseau est utilisés sur des clusters - dans le stockage et de clusters de charge de travail de stockage. Ainsi, les clusters de la charge de travail (Hyper-V, Instance de Cluster de basculement SQL Server, réplica de stockage, etc.) à utiliser SMB multichannel et les résultats dans une utilisation plus efficace du réseau. Dans convergé (également appelé désagrégé) configuration où un cluster de serveur de fichiers avec montée en puissance est utilisé pour stocker les données de la charge de travail pour Hyper-V ou cluster de l’Instance de Cluster de basculement SQL Server, ce réseau est souvent appelé « sous-réseau le nord-sud » du cluster / réseau . De nombreux clients maximiser le débit de ce réseau en investissant dans des commutateurs et les cartes réseau compatibles RDMA.  

![Illustration d’un sous-réseau nord-sud SMB](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig2.png)  
**Figure 2 : Pour obtenir un débit réseau maximale, utilisez plusieurs cartes réseau sur le cluster de serveur de fichiers avec montée en puissance et le cluster Hyper-V ou Instance de Cluster de basculement SQL Server, qui partagent le sous-réseau nord-sud**  

![Capture d’écran de deux clusters à l’aide de plusieurs cartes réseau dans le même sous-réseau pour tirer parti de SMB multichannel](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig3.png)  
**Figure 3 : Deux clusters (Scale-out File Server pour le stockage, SQL Server <abbr title="Instance de Clustering de basculement">FCI</abbr> pour la charge de travail) les deux utilisent plusieurs cartes réseau dans le même sous-réseau pour tirer parti de SMB Multichannel et obtenir une meilleure réseau débit.** 

## <a name="automatic-recognition-of-ipv6-link-local-private-networks"></a>Reconnaissance automatique des réseaux privés IPv6 lien Local  
Lorsque des réseaux privés (cluster uniquement) avec plusieurs cartes réseau sont détectées, le cluster reconnaît automatiquement les adresses IP IPv6 lien Local (fe80) pour chaque carte réseau sur chaque sous-réseau. Cet administrateurs gagner du temps, car ils n’ont plus à configurer manuellement les ressources de l’adresse IP locale de lien IPv6 (fe80).  

Lorsque vous utilisez plusieurs réseaux privés (cluster uniquement), vérifiez la configuration de routage IPv6 pour vous assurer que le routage n’est pas configuré pour traverser des sous-réseaux, dans la mesure où cela réduira les performances du réseau.  

![Capture d’écran de configuration automatique de réseau dans le Gestionnaire du Cluster de basculement UI](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig4.png)  
**Figure 4 : Configuration de la ressource adresse IPv6 du lien Local (fe80) automatique**  

## <a name="throughput-and-fault-tolerance"></a>Débit et une tolérance de panne  
Windows Server 2016 automatiquement détecte les fonctionnalités de la carte réseau et tentera d’utiliser chaque carte réseau dans la configuration possible le plus rapide. Cartes réseau qui est associées, cartes réseau à l’aide de RSS et cartes réseau avec la fonction RDMA peut être utilisé. Le tableau ci-dessous récapitule les compromis lors de l’utilisation de ces technologies. Débit maximal est atteint lors de l’utilisation de plusieurs cartes de réseau compatibles RDMA. Pour plus d’informations, consultez [les principes fondamentaux de SMB Mutlichannel](https://blogs.technet.microsoft.com/josebda/2012/06/28/the-basics-of-smb-multichannel-a-feature-of-windows-server-2012-and-smb-3-0/).

![Obtenir une illustration de débit et tolérance de panne pour différentes configurations de carte réseau](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig5.png)  
**Figure 5 : Débit et tolérance de panne pour divers conifigurations de carte réseau**   

## <a name="frequently-asked-questions"></a>Forum Aux Questions  
**Toutes les cartes réseau dans un réseau de plusieurs cartes réseau sont utilisés pour les pulsations de cluster ?**  
    Oui.  

**Un réseau de multi-NIC utilisable pour la communication de cluster uniquement ? Ou bien, il est utilisable uniquement pour la communication client et le cluster ?**  
    Soit configuration fonctionnera - tous les rôles de réseau de cluster fonctionnera sur un réseau de plusieurs cartes réseau.  

**SMB Multichannel est également utilisé pour le trafic CSV et du cluster ?**  
    Oui, par défaut tous les clusters et le trafic CSV utilisera les réseaux disponibles de multi-NIC. Les administrateurs peuvent utiliser les applets de commande PowerShell de Clustering de basculement ou l’interface utilisateur du Gestionnaire de Cluster de basculement pour modifier le rôle du réseau.  

**Comment puis-je voir les paramètres SMB Multichannel ?**  
    Utilisez le **Get-SMBServerConfiguration** applet de commande, recherchez la valeur de la propriété EnableMultiChannel.  

**Est la propriété commune de cluster que plumballcrosssubnetroutes respectée sur un réseau de plusieurs cartes réseau ?**  
     Oui.  

## <a name="see-also"></a>Voir aussi  
- [Nouveautés du Clustering de basculement dans Windows Server](whats-new-in-failover-clustering.md)  
