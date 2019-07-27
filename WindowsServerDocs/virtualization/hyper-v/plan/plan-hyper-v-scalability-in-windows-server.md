---
title: Planifier l’extensibilité d’Hyper-V dans Windows Server 2016
description: Répertorie le nombre maximal pris en charge pour les composants que vous pouvez ajouter ou supprimer dans Hyper-V et les machines virtuelles, comme la quantité de mémoire et le nombre de processeurs virtuels.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: KBDAzure
ms.author: kathydav
ms.date: 09/28/2016
ms.openlocfilehash: 534de49e50d7b415c9d64c32927418a4395f6f4f
ms.sourcegitcommit: 6f968368c12b9dd699c197afb3a3d13c2211f85b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68544751"
---
# <a name="plan-for-hyper-v-scalability-in-windows-server-2016"></a>Planifier l’extensibilité d’Hyper-V dans Windows Server 2016

> S'applique à : Windows Server 2016, Windows Server 2019
  
Cet article fournit des détails sur la configuration maximale des composants que vous pouvez ajouter et supprimer sur un ordinateur hôte Hyper-V ou sur ses ordinateurs virtuels, tels que les processeurs virtuels ou les points de contrôle. Lorsque vous planifiez votre déploiement, prenez en compte les valeurs maximales qui s’appliquent à chaque ordinateur virtuel, ainsi que celles qui s’appliquent à l’hôte Hyper-V. 

Les valeurs maximales pour la mémoire et les processeurs logiques représentent les plus grandes augmentations de Windows Server 2012, en réponse aux demandes de prise en charge de scénarios plus récents tels que les Machine Learning et l’analyse de données. Le blog de Windows Server a récemment publié les résultats de performances d’un ordinateur virtuel avec 5,5 téraoctets de mémoire et 128 processeurs virtuels exécutant une base de données en mémoire de 4 to. Les performances étaient supérieures à 95% des performances d’un serveur physique. Pour plus d’informations, consultez [performances des machines virtuelles Hyper-V Windows Server 2016 Hyper-V pour le traitement des transactions en mémoire](https://blogs.technet.microsoft.com/windowsserver/2016/09/28/windows-server-2016-hyper-v-large-scale-vm-performance-for-in-memory-transaction-processing/). Les autres nombres sont similaires à ceux qui s’appliquent à Windows Server 2012. \(Les valeurs maximales pour Windows Server 2012 R2 étaient les mêmes que celles de Windows Server 2012.\) 
  
> [!NOTE]  
> Pour plus d'informations sur System Center Virtual Machine Manager (VMM), voir [Virtual Machine Manager](https://technet.microsoft.com/system-center-docs/vmm/vmm). VMM est un produit Microsoft vendu séparément qui permet de gérer un centre de données virtualisé.  
  
## <a name="maximums-for-virtual-machines"></a>Valeurs maximales pour les machines virtuelles  
Ces valeurs maximales s’appliquent à chaque ordinateur virtuel. Tous les composants ne sont pas disponibles dans les deux générations de machines virtuelles. Pour une comparaison des générations, consultez [dois-je créer une machine virtuelle de génération 1 ou 2 dans Hyper-V?](should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md) 
  
|Composant|Maximale|Notes|  
|-------------|-----------|---------|  
|Points de contrôle|50|Le nombre réel peut être inférieur, selon le stockage disponible. Chaque point de contrôle est stocké sous la forme d’un fichier. avhd qui utilise le stockage physique.|  
|Mémoire|12 to pour la génération 2; <br>1 to pour la génération 1|Vérifiez la configuration requise du système d’exploitation spécifique pour déterminer les capacités minimales et recommandées.|  
|Ports série (COM)|2|Aucune.|  
|Taille des disques physiques connectés directement à un ordinateur virtuel|Varie|La taille maximale est fonction du système d’exploitation invité.|  
|Carte Fibre Channel virtuelle|4|En guise de meilleure pratique, nous vous recommandons de connecter chaque carte Fibre Channel virtuelle à un SAN virtuel différent.|  
|Lecteurs de disquettes virtuels|1 lecteur de disquettes virtuel|Aucune.|
|Capacité du disque dur virtuel|64 to pour le format VHDX;<br>2040 Go pour le format VHD|Chaque disque dur virtuel est stocké sur un média physique en tant que fichier .vhdx ou .vhd, selon le format utilisé par le disque dur virtuel.|  
|Disques virtuels IDE|4|Le disque de démarrage (parfois appelé «disque de démarrage») doit être attaché à l’un des périphériques IDE. Il peut s’agir d’un disque dur virtuel ou d’un disque physique connecté directement à un ordinateur virtuel.|  
|Processeurs virtuels|240 pour la génération 2;<br>64 pour la génération 1;<br>320 disponible pour le système d’exploitation hôte (partition racine)|Le nombre de processeurs virtuels pris en charge par un système d’exploitation invité pourrait être inférieur. Pour plus d’informations, consultez les informations publiées pour le système d’exploitation spécifique.|
|Contrôleurs SCSI virtuels|4|L’utilisation de périphériques SCSI virtuels nécessite des services d’intégration, qui sont disponibles pour les systèmes d’exploitation invités pris en charge. Pour plus d’informations sur les systèmes d’exploitation pris en charge, consultez [machines virtuelles Linux et FreeBSD prises en](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md) charge et [systèmes d’exploitation invités Windows pris en charge](../supported-windows-guest-operating-systems-for-hyper-v-on-windows.md).|  
|Disques SCSI virtuels|256|Chaque contrôleur SCSI prend en charge jusqu’à 64 disques. En d’autres termes, chaque ordinateur virtuel peut être configuré avec un maximum de 256 disques SCSI virtuels (4 contrôleurs x 64 disques par contrôleur). (4 contrôleurs x 64 disques par contrôleur).|  
|Cartes réseau virtuel|Windows Server 2016 prend en charge 12 au total:<br> -8 cartes réseau spécifiques à Hyper-V<br>-4 cartes réseau héritées <br> Windows Server 2019 prend en charge 72 au total: <br> -cartes réseau spécifiques à Hyper-V 64<br>-4 cartes réseau héritées  |La carte réseau spécifique à Hyper-V offre de meilleures performances et requiert un pilote inclus dans Integration Services. Pour plus d’informations, consultez [planifier la mise en réseau Hyper-V dans Windows Server](plan-hyper-v-networking-in-windows-server.md).|  
  
## <a name="maximums-for-hyper-v-hosts"></a>Valeurs maximales pour les hôtes Hyper-V  
Ces valeurs maximales s’appliquent à chaque hôte Hyper-V.  
  
|Composant|Maximale|Notes|  
|-------------|-----------|---------|  
|Processeurs logiques|512|Les deux doivent être activés dans le microprogramme:<br /><br />-Virtualisation assistée par matériel<br />-Prévention de l’exécution des données (DEP) appliquée par le matériel<br /><br />Le système d’exploitation hôte (partition racine) ne voit que les processeurs logiques 320 maximum|  
|Mémoire|24 To|Aucune.|  
|Association de cartes réseau|Aucune limite imposée par Hyper-V.|Pour plus d’informations, consultez [Association de cartes réseau](../../../networking/technologies/nic-teaming/NIC-Teaming.md).|  
|Cartes réseau physiques|Aucune limite imposée par Hyper-V.|Aucune.|  
|Ordinateurs virtuels en cours d'exécution par serveur|1 024|Aucune.|  
|Stockage|Limité par ce qui est pris en charge par le système d’exploitation hôte. Aucune limite imposée par Hyper-V.|**Remarque :** Microsoft prend en charge le stockage NAS (Network-Attached Storage) lors de l’utilisation de SMB 3,0. Le stockage NFS n'est pas pris en charge.|
|Ports de commutateurs de réseau virtuel par serveur|Variable ; aucune limite imposée par Hyper-V.|La limite pratique varie en fonction des ressources informatiques disponibles.|  
|Processeurs virtuels par processeur logique|Aucun ratio imposé par Hyper-V.|Aucune.|  
|Processeurs virtuels par serveur|2 048|Aucune.|  
|Réseaux de zone de stockage virtuels|Aucune limite imposée par Hyper-V.|Aucune.|  
|Commutateurs virtuels|Variable ; aucune limite imposée par Hyper-V.|La limite pratique varie en fonction des ressources informatiques disponibles.|  
 
## <a name="failover-clusters-and-hyper-v"></a>Clusters de basculement et Hyper-V  
Ce tableau répertorie les valeurs maximales qui s’appliquent lors de l’utilisation d’Hyper-V et du clustering de basculement. Il est important d’effectuer une planification de capacité pour vous assurer qu’il y aura suffisamment de ressources matérielles pour exécuter toutes les machines virtuelles dans un environnement en cluster.  

Pour en savoir plus sur les mises à jour du clustering de basculement, notamment les nouvelles fonctionnalités des machines virtuelles, consultez Nouveautés du clustering de [basculement dans Windows Server 2016](../../../failover-clustering/whats-new-in-failover-clustering.md).

|Composant|Maximale|Notes|  
|-------------|-----------|---------|  
|Nœuds par cluster|64|Tenez compte des nœuds à réserver pour le basculement, ainsi que des tâches de maintenance telles que l’application de mises à jour. Nous vous recommandons de planifier suffisamment de ressources pour prévoir la réservation d’un nœud pour le basculement, ce qui signifie qu’il reste inactif jusqu’à ce qu’un autre nœud y bascule. (Ce type de nœud est parfois qualifié de nœud passif.) Vous pouvez augmenter ce nombre si vous souhaitez réserver des nœuds supplémentaires. Il n’existe aucun quotient ou multiplicateur recommandé de nœuds réservés pour les nœuds actifs. la seule exigence est que le nombre total de nœuds dans un cluster ne peut pas dépasser la valeur maximale de 64.|  
|Ordinateurs virtuels exécutés simultanément par nœud|8 000 par cluster|Plusieurs facteurs peuvent affecter le nombre réel de machines virtuelles que vous pouvez exécuter en même temps sur un nœud, par exemple:<br />-Quantité de mémoire physique utilisée par chaque ordinateur virtuel.<br />-Bande passante de stockage et de mise en réseau.<br />-Nombre de piles de disques, ce qui affecte les performances d’e/s disque.|  
  

