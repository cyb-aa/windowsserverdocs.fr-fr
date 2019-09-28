---
ms.assetid: a6343f1c-e9dd-4a02-91ad-39bd519d66cd
title: Réseaux de clusters à plusieurs cartes réseau et SMB Multichannel simplifiés
ms.prod: windows-server
ms.technology: storage-failover-clustering
ms.topic: article
author: RobHindman
ms.author: robhind
ms.date: 09/15/2016
ms.openlocfilehash: 7816016daae1d06568cd6149791a9a368d8602f8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361114"
---
# <a name="simplified-smb-multichannel-and-multi-nic-cluster-networks"></a>Réseaux de clusters à plusieurs cartes réseau et SMB Multichannel simplifiés

> S’applique à : Windows Server 2019, Windows Server 2016

SMB Multichannel simplifié et multi-<abbr title="Carte d’interface réseau">INTERFACE</abbr> Les réseaux de clusters sont une fonctionnalité qui permet d’utiliser plusieurs cartes réseau sur le même sous-réseau de réseau de cluster et active automatiquement SMB Multichannel.

Les réseaux de clusters à plusieurs cartes réseau et SMB Multichannel simplifiés présentent les avantages suivants :  
- Le clustering de basculement reconnaît automatiquement toutes les cartes réseau sur les nœuds qui utilisent le même commutateur/même sous-réseau, aucune configuration supplémentaire n’est nécessaire.  
- SMB Multichannel est activé automatiquement.  
- Les réseaux qui ont uniquement des ressources d’adresses IP de lien local IPv6 (FE80) sont reconnus sur les réseaux (privés) en cluster uniquement.  
- Une seule ressource d’adresse IP est configurée sur chaque point d’accès au cluster (NN) par défaut.  
- La validation de cluster ne émet plus de messages d’avertissement lorsque plusieurs cartes réseau sont détectées sur le même sous-réseau.  

## <a name="requirements"></a>Configuration requise  
-   Plusieurs cartes réseau par serveur, utilisant le même commutateur/sous-réseau.  

## <a name="how-to-take-advantage-of-multi-nic-clusters-networks-and-simplified-smb-multichannel"></a>Comment tirer parti des réseaux de clusters à plusieurs cartes réseau et de SMB Multichannel simplifié  
Cette section décrit comment tirer parti des nouveaux réseaux de clusters à plusieurs cartes réseau et des fonctionnalités SMB Multichannel simplifiées.  

### <a name="use-at-least-two-networks-for-failover-clustering"></a>Utiliser au moins deux réseaux pour le clustering de basculement   
Bien qu’il soit rare, les commutateurs réseau peuvent échouer. il est toujours recommandé d’utiliser au moins deux réseaux pour le clustering de basculement. Tous les réseaux trouvés sont utilisés pour les pulsations de cluster. Évitez d’utiliser un seul réseau pour votre cluster de basculement afin d’éviter un point de défaillance unique. Dans l’idéal, il doit y avoir plusieurs chemins de communication physique entre les nœuds du cluster et aucun point de défaillance unique.  

@no__t 0Illustration de deux réseaux pour le clustering de basculement @ no__t-1  
**Figure 1 : Utiliser au moins deux réseaux pour le clustering de basculement @ no__t-0  

### <a name="use-multiple-nics-across-clusters"></a>Utiliser plusieurs cartes réseau sur plusieurs clusters  

L’avantage maximal du protocole SMB Multichannel simplifié est atteint lorsque plusieurs cartes réseau sont utilisées sur des clusters, à la fois dans les clusters de charge de travail de stockage et de stockage. Cela permet aux clusters de charge de travail (Hyper-V, SQL Server instance de cluster de basculement, réplica de stockage, etc.) d’utiliser SMB Multichannel et d’utiliser plus efficacement le réseau. Dans une configuration de cluster convergé (ou désagrégée) où un cluster de serveurs de fichiers avec montée en puissance parallèle est utilisé pour stocker les données de charge de travail d’un cluster d’instance de cluster de basculement Hyper-V ou SQL Server, ce réseau est souvent appelé « sous-réseau nord-sud »/réseau . De nombreux clients optimisent le débit de ce réseau en investissant dans des commutateurs et des cartes NIC capables d’atteindre RDMA.  

![Illustration d’un sous-réseau SMB Nord-Sud @ no__t-1  
**Figure 2: Pour obtenir un débit réseau maximal, utilisez plusieurs cartes réseau sur le cluster de serveurs de fichiers avec montée en puissance parallèle et le cluster d’instances de cluster de basculement Hyper-V ou SQL Server, qui partagent le sous-réseau nord-sud @ no__t-0  

![Screencap de deux clusters utilisant plusieurs cartes d’interface réseau dans le même sous-réseau pour tirer parti de SMB Multichannel @ no__t-1  
**Figure 3: Deux clusters (serveur de fichiers avec montée en puissance parallèle pour le stockage, SQL Server instance de clustering <abbr title="Failover @ no__t-1FCI @ no__t-2 pour la charge de travail) utilisent tous deux des cartes réseau dans le même sous-réseau pour tirer parti de SMB Multichannel et améliorer le débit du réseau.** 

## <a name="automatic-recognition-of-ipv6-link-local-private-networks"></a>Reconnaissance automatique des réseaux privés locaux de liaison IPv6  
Lorsque des réseaux privés (cluster uniquement) avec plusieurs cartes réseau sont détectés, le cluster reconnaît automatiquement les adresses IP de lien IPv6 (FE80) pour chaque carte réseau sur chaque sous-réseau. Cela permet aux administrateurs de gagner du temps, car ils n’ont plus à configurer manuellement les ressources d’adresse IP de lien local (FE80) IPv6.  

Lorsque vous utilisez plusieurs réseaux privés (cluster uniquement), vérifiez la configuration du routage IPv6 pour vous assurer que le routage n’est pas configuré pour les sous-réseaux, car cela réduira les performances du réseau.  

@no__t 0Screencap de la configuration automatique du réseau dans l’interface utilisateur Gestionnaire du cluster de basculement @ no__t-1  
**Figure 4 : Configuration automatique de la ressource d’adresse locale de liaison IPv6 (FE80) @ no__t-0  

## <a name="throughput-and-fault-tolerance"></a>Débit et tolérance de panne  
Windows Server 2019 et Windows Server 2016 détectent automatiquement les fonctionnalités de la carte réseau et tentent d’utiliser chaque carte réseau dans la configuration la plus rapide possible. Toutes les cartes réseau associées, les cartes réseau utilisant RSS et les cartes réseau avec fonction RDMA peuvent toutes être utilisées. Le tableau ci-dessous résume les compromis lors de l’utilisation de ces technologies. Le débit maximal est atteint lors de l’utilisation de plusieurs cartes réseau avec compatibilité RDMA. Pour plus d’informations, consultez [les principes de base de SMB Mutlichannel](https://blogs.technet.microsoft.com/josebda/2012/06/28/the-basics-of-smb-multichannel-a-feature-of-windows-server-2012-and-smb-3-0/).

![An illustration du débit et de la tolérance de panne pour différentes configurations de carte réseau @ no__t-1  
@no__t 0Figure 5 : Débit et tolérance de panne pour différentes cartes réseau conifigurations @ no__t-0   

## <a name="frequently-asked-questions"></a>Forum Aux Questions  
**Toutes les cartes réseau d’un réseau multi-NIC sont-elles utilisées pour le battage du cœur du cluster ?**  
    Oui.  

**Can un réseau à plusieurs cartes réseau doit-il être utilisé uniquement pour les communications de cluster ? Ou peut-il être utilisé uniquement pour la communication entre le client et le cluster ?**  
    L’une ou l’autre des configurations fonctionne-tous les rôles réseau de cluster fonctionnent sur un réseau à plusieurs cartes réseau.  

**SMB Multichannel est-il également utilisé pour le trafic de cluster et CSV ?**  
    Oui, par défaut, tous les trafics de cluster et CSV utilisent des réseaux multi-NIC disponibles. Les administrateurs peuvent utiliser les applets de commande PowerShell de clustering de basculement ou l’interface utilisateur Gestionnaire du cluster de basculement pour modifier le rôle réseau.  

**Comment puis-je voir les paramètres SMB Multichannel ?**  
    Utilisez l’applet de commande **obtenir-SMBServerConfiguration** , recherchez la valeur de la propriété EnableMultiChannel.  

**La propriété commune PlumbAllCrossSubnetRoutes du cluster est-elle respectée sur un réseau à plusieurs cartes réseau ?**  
     Oui.  

## <a name="see-also"></a>Voir aussi  
- [Nouveautés du clustering de basculement dans Windows Server](whats-new-in-failover-clustering.md)  
