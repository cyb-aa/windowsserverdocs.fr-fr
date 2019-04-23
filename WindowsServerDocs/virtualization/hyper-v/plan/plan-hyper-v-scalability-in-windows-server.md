---
title: Plan pour l’extensibilité d’Hyper-V dans Windows Server 2016
description: Répertorie le nombre maximal pris en charge le nombre pour les composants que vous pouvez ajouter à ou supprimer à partir d’Hyper-V et les machines virtuelles, telles que la quantité de mémoire et le nombre de processeurs virtuel.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: KBDAzure
ms.author: kathydav
ms.date: 09/28/2016
ms.openlocfilehash: 03a464269c8aea29c226dee776f0dfacfe48743a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850260"
---
# <a name="plan-for-hyper-v-scalability-in-windows-server-2016"></a>Plan pour l’extensibilité d’Hyper-V dans Windows Server 2016

> S'applique à : Windows Server 2016
  
Cet article vous donne plus d’informations sur la configuration maximale pour les composants que vous pouvez ajouter et supprimer sur un hôte Hyper-V ou de ses ordinateurs virtuels, telles que les processeurs virtuels ou des points de contrôle. Lorsque vous planifiez votre déploiement, envisagez les valeurs maximales qui s’appliquent à chaque machine virtuelle, ainsi que celles qui s’appliquent à l’hôte Hyper-V. 

Valeurs maximales de mémoire et de processeurs logiques sont la plus importante augmente à partir de Windows Server 2012, en réponse aux demandes pour prendre en charge des scénarios plus récents tels que l’analytique de données et apprentissage machine. Le blog de Windows Server a récemment publié les résultats des performances d’une machine virtuelle avec 5.5 téraoctets de mémoire et 128 processeurs virtuels en cours d’exécution de base de données en mémoire de 4 To. Performances étaient supérieure à 95 % des performances d’un serveur physique. Pour plus d’informations, consultez [performances des machines virtuelles à grande échelle Windows Server 2016 Hyper-V pour le traitement de transactions en mémoire](https://blogs.technet.microsoft.com/windowsserver/2016/09/28/windows-server-2016-hyper-v-large-scale-vm-performance-for-in-memory-transaction-processing/). Autres numéros sont semblables à celles qui s’appliquent à Windows Server 2012. \(Valeurs maximales pour Windows Server 2012 R2 sont les mêmes que Windows Server 2012.\) 
  
> [!NOTE]  
> Pour plus d'informations sur System Center Virtual Machine Manager (VMM), voir [Virtual Machine Manager](https://technet.microsoft.com/system-center-docs/vmm/vmm). VMM est un produit Microsoft vendu séparément qui permet de gérer un centre de données virtualisé.  
  
## <a name="maximums-for-virtual-machines"></a>Valeurs maximales pour les machines virtuelles  
Ces valeurs maximales s’appliquent à chaque machine virtuelle. Pas tous les composants sont disponibles dans les deux générations d’ordinateurs virtuels. Pour obtenir une comparaison des générations, consultez [dois-je créer une machine virtuelle de génération 1 ou 2 dans Hyper-V ?](should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md) 
  
|Component|Durée maximum|Notes|  
|-------------|-----------|---------|  
|Points de contrôle|50|Le nombre réel peut être inférieur, selon le stockage disponible. Chaque point de contrôle est stockée sous forme de fichier .avhd qui utilise le stockage physique.|  
|Mémoire|12 To pour la génération 2 ; <br>1 To pour la génération 1|Vérifiez la configuration requise du système d’exploitation spécifique pour déterminer les capacités minimales et recommandées.|  
|Ports série (COM)|2|Aucun.|  
|Taille des disques physiques connectés directement à un ordinateur virtuel|Varie|La taille maximale est fonction du système d’exploitation invité.|  
|Carte Fibre Channel virtuelle|4|En guise de meilleure pratique, nous vous recommandons de connecter chaque carte Fibre Channel virtuelle à un SAN virtuel différent.|  
|Lecteurs de disquettes virtuels|1 lecteur de disquettes virtuel|Aucun.|
|Capacité du disque dur virtuel|64 To pour le format VHDX ;<br>2 040 Go pour le format de disque dur virtuel|Chaque disque dur virtuel est stocké sur un média physique en tant que fichier .vhdx ou .vhd, selon le format utilisé par le disque dur virtuel.|  
|Disques virtuels IDE|4|Le disque de démarrage (parfois appelé le disque de démarrage) doit être attaché à un des périphériques IDE. Il peut s’agir d’un disque dur virtuel ou d’un disque physique connecté directement à un ordinateur virtuel.|  
|Processeurs virtuels|240 pour la génération 2 ;<br>64 pour la génération 1 ;<br>320 disponible à l’hôte du système d’exploitation (partition racine)|Le nombre de processeurs virtuels pris en charge par un système d’exploitation invité pourrait être inférieur. Pour plus d’informations, consultez les informations publiées pour le système d’exploitation spécifique.|
|Contrôleurs SCSI virtuels|4|Utilisation de périphériques SCSI virtuels nécessite des services d’intégration sont disponibles pour les systèmes d’exploitation invités pris en charge. Pour plus d’informations sur lequel les systèmes d’exploitation sont pris en charge, consultez [machines virtuelles prises en charge de Linux et FreeBSD](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md) et [Windows pris en charge les systèmes d’exploitation invités](../supported-windows-guest-operating-systems-for-hyper-v-on-windows.md).|  
|Disques SCSI virtuels|256|Chaque contrôleur SCSI prend en charge jusqu’à 64 disques. En d’autres termes, chaque ordinateur virtuel peut être configuré avec un maximum de 256 disques SCSI virtuels (4 contrôleurs x 64 disques par contrôleur). (4 contrôleurs x 64 disques par contrôleur).|  
|Cartes réseau virtuel|12 au total :<br> -Les adaptateurs de réseau Hyper-V 8<br>-4 cartes réseau héritées|La carte réseau spécifique de Hyper-V offre de meilleures performances et requiert un pilote inclus dans les services d’intégration. Pour plus d’informations, consultez [Plan de mise en réseau Hyper-V dans Windows Server](plan-hyper-v-networking-in-windows-server.md).|  
  
## <a name="maximums-for-hyper-v-hosts"></a>Valeurs maximales pour les ordinateurs hôtes Hyper-V  
Ces valeurs maximales s’appliquent à chaque hôte Hyper-V.  
  
|Component|Durée maximum|Notes|  
|-------------|-----------|---------|  
|Processeurs logiques|512|Ces deux éléments doivent être activés dans le microprogramme :<br /><br />Virtualisation à assistance matérielle<br />-Prévention de l’exécution matérielle des données (DEP)<br /><br />Le système d’exploitation (partition racine) de l’hôte ne voient que maximales 320 processeurs logiques|  
|Mémoire|24 To|Aucun.|  
|Association de cartes réseau|Aucune limite imposée par Hyper-V.|Pour plus d’informations, consultez [association de cartes réseau](../../../networking/technologies/nic-teaming/NIC-Teaming.md).|  
|Cartes réseau physiques|Aucune limite imposée par Hyper-V.|Aucun.|  
|Ordinateurs virtuels en cours d'exécution par serveur|1024|Aucun.|  
|Stockage|Limité par la capacité est prise en charge par le système d’exploitation hôte. Aucune limite imposée par Hyper-V.|**Remarque :** Microsoft prend en charge le stockage connecté au réseau (NAS) lors de l’utilisation de SMB 3.0. Le stockage NFS n'est pas pris en charge.|
|Ports de commutateurs de réseau virtuel par serveur|Variable ; aucune limite imposée par Hyper-V.|La limite pratique varie en fonction des ressources informatiques disponibles.|  
|Processeurs virtuels par processeur logique|Aucun ratio imposé par Hyper-V.|Aucun.|  
|Processeurs virtuels par serveur|2 048|Aucun.|  
|Réseaux de zone de stockage virtuels|Aucune limite imposée par Hyper-V.|Aucun.|  
|Commutateurs virtuels|Variable ; aucune limite imposée par Hyper-V.|La limite pratique varie en fonction des ressources informatiques disponibles.|  
 
## <a name="failover-clusters-and-hyper-v"></a>Clusters de basculement et Hyper-V  
Ce tableau répertorie les valeurs maximales qui s’appliquent lors de l’utilisation de Hyper-V et clustering avec basculement. Il est important de faire pour vous assurer qu’il y aura suffisamment de ressources matérielles pour exécuter toutes les machines virtuelles dans un environnement en cluster de planification de capacité.  

Pour en savoir plus sur les mises à jour pour le Clustering de basculement, y compris les nouvelles fonctionnalités pour les machines virtuelles, consultez [Nouveautés du clustering avec basculement dans Windows Server 2016](../../../failover-clustering/whats-new-in-failover-clustering.md).

|Component|Durée maximum|Notes|  
|-------------|-----------|---------|  
|Nœuds par cluster|64|Tenez compte des nœuds à réserver pour le basculement, ainsi que des tâches de maintenance telles que l’application de mises à jour. Nous vous recommandons de planifier suffisamment de ressources pour prévoir la réservation d’un nœud pour le basculement, ce qui signifie qu’il reste inactif jusqu’à ce qu’un autre nœud y bascule. (Ce type de nœud est parfois qualifié de nœud passif.) Vous pouvez augmenter ce nombre si vous souhaitez réserver des nœuds supplémentaires. Aucun ratio recommandée ou d’un multiplicateur de nœuds réservés pour les nœuds actifs ; la seule exigence est que le nombre total de nœuds dans un cluster ne peut pas dépasser le nombre maximal de 64.|  
|Ordinateurs virtuels exécutés simultanément par nœud|8 000 par cluster|Plusieurs facteurs peuvent affecter le nombre réel d’ordinateurs virtuels que vous pouvez exécuter simultanément sur un nœud, tel que :<br />-Quantité de mémoire physique utilisée par chaque ordinateur virtuel.<br />-Mise en réseau et la bande passante de stockage.<br />-Nombre de piles de disques, ce qui affecte les performances d’e/s disque.|  
  

