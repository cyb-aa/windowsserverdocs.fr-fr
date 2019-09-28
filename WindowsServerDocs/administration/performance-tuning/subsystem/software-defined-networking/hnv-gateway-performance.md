---
title: Réglage des performances de la passerelle HNV dans les réseaux à définition logicielle
description: Instructions de réglage des performances de la passerelle HNV sur les réseaux à définition logicielle
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; AnPaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 907b160b143af18a8ede3a9a7975fa8b22753118
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383498"
---
# <a name="hnv-gateway-performance-tuning-in-software-defined-networks"></a>Réglage des performances de la passerelle HNV dans les réseaux à définition logicielle

Cette rubrique fournit des spécifications matérielles et des recommandations de configuration pour les serveurs qui exécutent Hyper-V et hébergeant des machines virtuelles de passerelle Windows Server, en plus des paramètres de configuration pour les machines virtuelles de la passerelle Windows Server . Pour obtenir des performances optimales des machines virtuelles de la passerelle Windows Server, ces instructions doivent être suivies.
Les sections suivantes présentent le matériel et la configuration requis quand vous déployez une passerelle Windows Server.
1. Recommandations en matière de matériel Hyper-V
2. Configuration hôte Hyper-V
3. Configuration de la machine virtuelle de la passerelle Windows Server

## <a name="hyper-v-hardware-recommendations"></a>Recommandations en matière de matériel Hyper-V

Voici la configuration matérielle minimale recommandée pour chaque serveur qui exécute Windows Server 2016 et Hyper-V.

| Composant serveur               | Spécification                                                                                                                                                                                                                                                                   |
|--------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Unité centrale (CPU)  | Nœuds NUMA (non-Uniform Memory Architecture) : 2 <br> S’il existe plusieurs machines virtuelles de passerelle Windows Server sur l’ordinateur hôte, pour des performances optimales, chaque machine virtuelle de passerelle doit avoir un accès complet à un nœud NUMA. Et il doit être différent du nœud NUMA utilisé par la carte physique de l’ordinateur hôte. |
| Cœurs par nœud NUMA            | 2                                                                                                                                                                                                                                                                               |
| Hyper-Threading                | Désactivé. Hyper-Threading n’améliore pas les performances d’une passerelle Windows Server.                                                                                                                                                                                           |
| Mémoire vive (RAM)     | 48 Go                                                                                                                                                                                                                                                                           |
| Cartes d’interface réseau (NIC) | 2 10 Go de cartes réseau, les performances de la passerelle dépendent de la vitesse de ligne. Si le taux de lignes est inférieur à 10Gbps, les numéros de débit du tunnel de la passerelle sont également déplacés par le même facteur.                                                                                          |

Vérifiez que le nombre de processeurs virtuels assignés à un ordinateur virtuel Passerelle Windows Server ne dépasse pas le nombre de processeurs sur le nœud NUMA. Par exemple, si un nœud NUMA a 8 cœurs, le nombre de processeurs virtuels doit être inférieur ou égal à 8. Pour des performances optimales, il doit s’agir de 8. Pour connaître le nombre de nœuds NUMA et le nombre de cœurs par nœud NUMA, exécutez le script Windows PowerShell suivant sur chaque hôte Hyper-V :

```PowerShell
$nodes = [object[]] $(gwmi –Namespace root\virtualization\v2 -Class MSVM_NumaNode)
$cores = ($nodes | Measure-Object NumberOfProcessorCores -sum).Sum
$lps = ($nodes | Measure-Object NumberOfLogicalProcessors -sum).Sum


Write-Host "Number of NUMA Nodes: ", $nodes.count
Write-Host ("Total Number of Cores: ", $cores)
Write-Host ("Total Number of Logical Processors: ", $lps)
```

>[!Important]
> L’allocation de processeurs virtuels sur les nœuds NUMA peut avoir un impact négatif sur les performances de passerelle Windows Server. L’exécution de plusieurs ordinateurs virtuels, avec des processeurs virtuels à partir d’un nœud NUMA, fournit de meilleures performances qu’un seul ordinateur virtuel auquel tous les processeurs virtuels sont assignés.

Une seule machine virtuelle de passerelle avec huit processeurs virtuels et au moins 8 Go de RAM est recommandée lors de la sélection du nombre de machines virtuelles de passerelle à installer sur chaque hôte Hyper-V lorsque chaque nœud NUMA a huit cœurs. Dans ce cas, un nœud NUMA est dédié à l’ordinateur hôte.

## <a name="hyper-v-host-configuration"></a>Configuration de l’hôte Hyper-V

Voici la configuration recommandée pour chaque serveur qui exécute Windows Server 2016 et Hyper-V et dont la charge de travail consiste à exécuter des machines virtuelles de passerelle Windows Server. Ces instructions de configuration comprennent l’utilisation d’exemples de commande Windows PowerShell. Ces exemples contiennent des espaces réservés pour les valeurs réelles que vous devez fournir quand vous exécutez ces commandes dans votre environnement. Par exemple, les espaces réservés de nom de carte réseau sont « NIC1 » et « NIC2 ». Quand vous exécutez des commandes qui utilisent ces espaces réservés, remplacez-les par les noms réels des cartes réseau de vos serveurs, dans le cas contraire les commandes échoueront.

>[!Note]
> Pour exécuter les commandes Windows PowerShell suivantes, vous devez être membre du groupe Administrateurs.

| Élément de configuration                          | Configuration de Windows PowerShell                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SET (Switch Embedded Teaming)                     | Lorsque vous créez un vswitch avec plusieurs cartes réseau, l’Association incorporée de commutateur est automatiquement activée pour ces cartes. <br> ```New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2"``` <br> L’Association traditionnelle via LBFO n’est pas prise en charge avec SDN dans Windows Server 2016. Switch Embedded Teaming vous permet d’utiliser le même ensemble de cartes réseau pour votre trafic virtuel et le trafic RDMA. Cela n’était pas pris en charge avec l’Association de cartes réseau basée sur LBFO.                                                        |
| Modération de l’interruption sur les cartes réseau physiques       | Utilisez les paramètres par défaut. Pour vérifier la configuration, vous pouvez utiliser la commande Windows PowerShell suivante : ```Get-NetAdapterAdvancedProperty```                                                                                                                                                                                                                                                                                                                                                                    |
| Taille des tampons de réception sur les cartes réseau physiques       | Vous pouvez vérifier si les cartes réseau physiques prennent en charge la configuration de ce paramètre en exécutant la commande ```Get-NetAdapterAdvancedProperty```. S’ils ne prennent pas en charge ce paramètre, la sortie de la commande n’inclut pas la propriété « recevoir les tampons ». Si les cartes réseau ne prennent pas en charge ce paramètre, vous pouvez utiliser la commande Windows PowerShell suivante pour définir la taille des tampons de réception : <br>```Set-NetAdapterAdvancedProperty "NIC1" –DisplayName "Receive Buffers" –DisplayValue 3000``` <br>                          |
| Taille des tampons d’envoi sur les cartes réseau physiques          | Vous pouvez vérifier si les cartes réseau physiques prennent en charge la configuration de ce paramètre en exécutant la commande ```Get-NetAdapterAdvancedProperty```. Si les cartes réseau ne prennent pas en charge ce paramètre, la sortie de la commande n’inclut pas la propriété « Send buffers ». Si les cartes réseau ne prennent pas en charge ce paramètre, vous pouvez utiliser la commande Windows PowerShell suivante pour définir la taille des tampons d’envoi : <br> ```Set-NetAdapterAdvancedProperty "NIC1" –DisplayName "Transmit Buffers" –DisplayValue 3000``` <br>                           |
| Partage du trafic entrant (RSS) sur les cartes réseau physiques | Vous pouvez vérifier si RSS est activé sur vos cartes réseau physiques en exécutant la commande Windows PowerShell obtenir-NetAdapterRss. Vous pouvez utiliser les commandes Windows PowerShell suivantes pour activer et configurer RSS sur vos cartes réseau : <br> ```Enable-NetAdapterRss "NIC1","NIC2"```<br> ```Set-NetAdapterRss "NIC1","NIC2" –NumberOfReceiveQueues 16 -MaxProcessors``` <br> REMARQUE : Si VMMQ ou l’ordinateurs virtuels sont activés, il n’est pas nécessaire d’activer RSS sur les cartes réseau physiques. Vous pouvez l’activer sur les cartes réseau virtuelles de l’ordinateur hôte |
| VMMQ                                        | Pour activer VMMQ pour une machine virtuelle, exécutez la commande suivante : <br> ```Set-VmNetworkAdapter -VMName <gateway vm name>,-VrssEnabled $true -VmmqEnabled $true``` <br> REMARQUE : Toutes les cartes réseau ne prennent pas en charge VMMQ. Actuellement, il est pris en charge sur les séries Chelsio T5 et T6, Mellanox CX-3 et CX-4 et QLogic 45xxx                                                                                                                                                                                                                                      |
| File d’attente d’ordinateurs virtuels (VMQ) sur l’association de cartes réseau | Vous pouvez activer l’espace de travail à partir de l’ensemble de votre équipe à l’aide de la commande Windows PowerShell suivante : <br>```Enable-NetAdapterVmq``` <br> REMARQUE : Cette option doit être activée uniquement si le matériel ne prend pas en charge VMMQ. S’il est pris en charge, VMMQ doit être activé pour obtenir de meilleures performances.                                                                                                                                                                                                                                                               |
>[!Note]
> Les ordinateurs virtuels et vRSS entrent en image uniquement lorsque la charge sur la machine virtuelle est élevée et que le processeur est utilisé au maximum. Le nombre maximal de cœurs de processeurs a été dépassé. Les ordinateurs virtuels et les ordinateurs virtuels seront ensuite bénéfiques pour aider à répartir la charge de traitement entre plusieurs cœurs. Cela ne s’applique pas au trafic IPsec, car le trafic IPsec est limité à un seul cœur.

## <a name="windows-server-gateway-vm-configuration"></a>Configuration des ordinateurs virtuels Passerelle Windows Server

Sur les deux hôtes Hyper-V, vous pouvez configurer plusieurs machines virtuelles configurées en tant que passerelles avec la passerelle Windows Server. Vous pouvez utiliser le Gestionnaire de commutateur virtuel (Virtual Switch Manager) pour créer un commutateur virtuel Hyper-V qui est lié à l’association de cartes réseau sur l’hôte Hyper-V. Notez que, pour des performances optimales, vous devez déployer une seule machine virtuelle de passerelle sur un ordinateur hôte Hyper-V.
Vous trouverez ci-dessous la configuration recommandée pour chaque ordinateur virtuel Passerelle Windows Server.

| Élément de configuration                 | Configuration de Windows PowerShell                                                                                                                                                                                                                                                                                                                                                               |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Mémoire                             | 8 Go                                                                                                                                                                                                                                                                                                                                                                                           |
| Nombre de cartes réseau virtuelles | 3 cartes réseau avec les utilisations spécifiques suivantes : 1 pour la gestion qui est utilisée par le système d’exploitation de gestion, 1 externe qui fournit l’accès aux réseaux externes, 1 qui est interne qui fournit l’accès aux réseaux internes uniquement.                                                                                                                                                            |
| Receive Side Scaling (RSS)         | Vous pouvez conserver les paramètres RSS par défaut pour la carte réseau de gestion. L’exemple de configuration suivant s’applique à un ordinateur virtuel avec 8 processeurs virtuels. Pour les cartes réseau externes et internes, vous pouvez activer RSS avec BaseProcNumber défini sur 0 et MaxRssProcessors défini sur 8 à l’aide de la commande Windows PowerShell suivante : <br> ```Set-NetAdapterRss "Internal","External" –BaseProcNumber 0 –MaxProcessorNumber 8``` <br> |
| Mémoire tampon côté envoi                   | Vous pouvez conserver les paramètres de mémoire tampon côté envoi par défaut pour la carte réseau de gestion. Pour les cartes réseau interne et externe, vous pouvez configurer la mémoire tampon côté envoi avec 32 Mo de RAM en utilisant la commande Windows PowerShell suivante : <br> ```Set-NetAdapterAdvancedProperty "Internal","External" –DisplayName "Send Buffer Size" –DisplayValue "32MB"``` <br>                                                       |
| Mémoire tampon côté réception                | Vous pouvez conserver les paramètres par défaut de la mémoire tampon côté réception pour la carte réseau de gestion. Pour les cartes réseau interne et externe, vous pouvez configurer la mémoire tampon de réception avec 16 Mo de RAM en utilisant la commande Windows PowerShell suivante : <br> ```Set-NetAdapterAdvancedProperty "Internal","External" –DisplayName "Receive Buffer Size" –DisplayValue "16MB"``` <br>                                            |
| Forward Optimization               | Vous pouvez conserver les paramètres d’optimisation par défaut pour la carte réseau de gestion. Pour les cartes réseau interne et externe, vous pouvez activer l’optimisation directe à l’aide de la commande Windows PowerShell suivante : <br> ```Set-NetAdapterAdvancedProperty "Internal","External" –DisplayName "Forward Optimization" –DisplayValue "1"``` <br>                                                                      |
