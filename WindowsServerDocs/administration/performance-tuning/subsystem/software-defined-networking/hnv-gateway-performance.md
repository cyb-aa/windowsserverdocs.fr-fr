---
title: Performances de la passerelle HNV de paramétrage dans le logiciel de réseaux définis
description: Performances de la passerelle HNV instructions sur les réseaux à définition logicielle de réglage
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; AnPaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 217428b84a00b2e2231a15cb3878d0abcec1d9ad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827320"
---
# <a name="hnv-gateway-performance-tuning-in-software-defined-networks"></a>Performances de la passerelle HNV de paramétrage dans le logiciel de réseaux définis

Cette rubrique fournit des spécifications matérielles et les recommandations de configuration pour les serveurs exécutant Hyper-V et héberge des ordinateurs virtuels passerelle Windows Server, en plus des paramètres de configuration pour les machines virtuelles de passerelle Windows Server (machines virtuelles) . Pour extraire les meilleures performances de machines virtuelles de passerelle Windows Server, il est probable que ces instructions seront suivies.
Les sections suivantes présentent le matériel et la configuration requis quand vous déployez une passerelle Windows Server.
1. Recommandations en matière de matériel Hyper-V
2. Configuration hôte Hyper-V
3. Configuration de machine virtuelle de passerelle Windows Server

## <a name="hyper-v-hardware-recommendations"></a>Recommandations en matière de matériel Hyper-V

Voici la configuration matérielle minimale recommandée pour chaque serveur qui exécute Windows Server 2016 et Hyper-V.

| Composant serveur               | Spécification                                                                                                                                                                                                                                                                   |
|--------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Unité centrale (CPU)  | Nœuds de l’Architecture de mémoire non uniforme (NUMA) : 2 <br> S’il existe plusieurs Windows Server machines virtuelles de passerelle sur l’ordinateur hôte, pour de meilleures performances, chaque machine virtuelle de passerelle doit avoir un accès complet à un seul nœud NUMA. Et il doit être différent à partir du nœud NUMA utilisé par la carte physique hôte. |
| Cœurs par nœud NUMA            | 2                                                                                                                                                                                                                                                                               |
| Hyper-Threading                | Désactivé. Hyper-Threading n’améliore pas les performances d’une passerelle Windows Server.                                                                                                                                                                                           |
| Mémoire vive (RAM)     | 48 Go                                                                                                                                                                                                                                                                           |
| Cartes d’interface réseau (NIC) | Deux cartes réseau de 10 Go, les performances de la passerelle varie selon la vitesse de ligne. Si le taux de ligne est inférieure à 10 Gbits/s, les numéros de débit de tunnel de passerelle seront arrêtent également par le même facteur.                                                                                          |

Vérifiez que le nombre de processeurs virtuels assignés à un ordinateur virtuel Passerelle Windows Server ne dépasse pas le nombre de processeurs sur le nœud NUMA. Par exemple, si un nœud NUMA a 8 cœurs, le nombre de processeurs virtuels doit être inférieur ou égal à 8. Pour de meilleures performances, il doit être de 8. Pour connaître le nombre de nœuds NUMA et le nombre de cœurs par nœud NUMA, exécutez le script Windows PowerShell suivant sur chaque hôte Hyper-V :

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

Une machine virtuelle de passerelle avec huit processeurs virtuels et au moins 8 Go de RAM est recommandé lorsque vous sélectionnez le nombre de machines virtuelles de passerelle à installer sur chaque hôte Hyper-V quand chaque nœud NUMA a huit cœurs. Dans ce cas, un seul nœud NUMA est dédié à la machine hôte.

## <a name="hyper-v-host-configuration"></a>Configuration de l’hôte Hyper-V

Voici la configuration recommandée pour chaque serveur qui exécute Windows Server 2016 et Hyper-V et dont la charge de travail consiste à exécuter des machines virtuelles de passerelle Windows Server. Ces instructions de configuration comprennent l’utilisation d’exemples de commande Windows PowerShell. Ces exemples contiennent des espaces réservés pour les valeurs réelles que vous devez fournir quand vous exécutez ces commandes dans votre environnement. Par exemple, des espaces réservés de nom de carte réseau sont « NIC1 ? et « NIC2. ? Quand vous exécutez des commandes qui utilisent ces espaces réservés, remplacez-les par les noms réels des cartes réseau de vos serveurs, dans le cas contraire les commandes échoueront.

>[!Note]
> Pour exécuter les commandes Windows PowerShell suivantes, vous devez être membre du groupe Administrateurs.

| Élément de configuration                          | Configuration de Windows Powershell                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SET (Switch Embedded Teaming)                     | Lorsque vous créez un commutateur virtuel avec plusieurs cartes réseau, il activé automatiquement commutateur incorporé l’association pour ces cartes. <br> ```New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2"``` <br> Association de cartes traditionnel via LBFO n’est pas pris en charge avec SDN dans Windows Server 2016. Switch Embedded Teaming vous permet d’utiliser le même jeu de cartes réseau pour votre trafic virtuel et RDMA. Cela n’a été pas pris en charge avec l’association de cartes réseau selon LBFO.                                                        |
| Modération de l’interruption sur les cartes réseau physiques       | Utilisez les paramètres par défaut. Pour vérifier la configuration, vous pouvez utiliser la commande Windows PowerShell suivante : ```Get-NetAdapterAdvancedProperty```                                                                                                                                                                                                                                                                                                                                                                    |
| Taille des tampons de réception sur les cartes réseau physiques       | Vous pouvez vérifier si les cartes réseau physiques prennent en charge la configuration de ce paramètre en exécutant la commande ```Get-NetAdapterAdvancedProperty```. Si elles ne gèrent pas ce paramètre, la sortie de la commande n’inclut pas la propriété « tampons de réception. ? Si les cartes réseau ne prennent pas en charge ce paramètre, vous pouvez utiliser la commande Windows PowerShell suivante pour définir la taille des tampons de réception : <br>```Set-NetAdapterAdvancedProperty “NIC1�? –DisplayName “Receive Buffers�? –DisplayValue 3000``` <br>                          |
| Taille des tampons d’envoi sur les cartes réseau physiques          | Vous pouvez vérifier si les cartes réseau physiques prennent en charge la configuration de ce paramètre en exécutant la commande ```Get-NetAdapterAdvancedProperty```. Si les cartes réseau ne prennent pas en charge ce paramètre, la sortie de la commande n’inclut pas la propriété « mémoires tampons d’envoi. ? Si les cartes réseau ne prennent pas en charge ce paramètre, vous pouvez utiliser la commande Windows PowerShell suivante pour définir la taille des tampons d’envoi : <br> ```Set-NetAdapterAdvancedProperty “NIC1�? –DisplayName “Transmit Buffers�? –DisplayValue 3000``` <br>                           |
| Partage du trafic entrant (RSS) sur les cartes réseau physiques | Vous pouvez vérifier si vos cartes réseau physiques ont RSS activée en exécutant la commande Windows PowerShell Get-NetAdapterRss. Vous pouvez utiliser les commandes Windows PowerShell suivantes pour activer et configurer RSS sur vos cartes réseau : <br> ```Enable-NetAdapterRss “NIC1�?,�?NIC2�?```<br> ```Set-NetAdapterRss “NIC1�?,�?NIC2�? –NumberOfReceiveQueues 16 -MaxProcessors``` <br> REMARQUE : Si VMMQ ou VMQ est activé, RSS n’a pas à être activé sur les cartes réseau physiques. Vous pouvez l’activer sur les cartes réseau virtuelles hôtes |
| VMMQ                                        | Pour activer VMMQ pour une machine virtuelle, exécutez la commande suivante : <br> ```Set-VmNetworkAdapter -VMName <gateway vm name>,-VrssEnabled $true -VmmqEnabled $true``` <br> REMARQUE : Pas toutes les cartes réseau prennent en charge VMMQ. Actuellement, il est pris en charge sur la série 45xxx Chelsio T5 et T6 Mellanox CX-3 et 4 de CX et QLogic                                                                                                                                                                                                                                      |
| File d’attente d’ordinateurs virtuels (VMQ) sur l’association de cartes réseau | Vous pouvez activer VMQ sur votre équipe de jeu à l’aide de la commande Windows PowerShell suivante : <br>```Enable-NetAdapterVmq``` <br> REMARQUE : Cette option doit être activée uniquement si le matériel ne prend pas en charge VMMQ. Si la prise en charge, VMMQ doit être activé pour de meilleures performances.                                                                                                                                                                                                                                                               |
>[!Note]
> VMQ et vRSS entrent en image uniquement lorsque la charge sur la machine virtuelle est élevée et que le processeur est utilisé à la valeur maximale. Alors seulement sera au moins un processeur de base de max out. VMQ et vRSS puis sera très utile aider à répartir la charge de traitement sur plusieurs cœurs. Cela n’est pas applicable pour le trafic IPsec comme le trafic IPsec est limité à un seul cœur.

## <a name="windows-server-gateway-vm-configuration"></a>Configuration des ordinateurs virtuels Passerelle Windows Server

Sur les deux hôtes Hyper-V, vous pouvez configurer plusieurs machines virtuelles qui sont configurés en tant que passerelles avec passerelle Windows Server. Vous pouvez utiliser le Gestionnaire de commutateur virtuel (Virtual Switch Manager) pour créer un commutateur virtuel Hyper-V qui est lié à l’association de cartes réseau sur l’hôte Hyper-V. Notez que pour de meilleures performances, vous devez déployer une seule machine virtuelle de passerelle sur un ordinateur hôte Hyper-V.
Vous trouverez ci-dessous la configuration recommandée pour chaque ordinateur virtuel Passerelle Windows Server.

| Élément de configuration                 | Configuration de Windows Powershell                                                                                                                                                                                                                                                                                                                                                               |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Mémoire                             | 8 Go                                                                                                                                                                                                                                                                                                                                                                                           |
| Nombre de cartes réseau virtuelles | Utilise de 3 NIC avec le spécifiques suivantes : 1 pour la gestion qui est utilisée par le système d’exploitation de gestion, 1 externe qui fournit l’accès à des réseaux externes 1 interne qui fournit l’accès aux réseaux internes uniquement.                                                                                                                                                            |
| Receive Side Scaling (RSS)         | Vous pouvez conserver la valeur par défaut des paramètres de RSS pour la carte de gestion. L’exemple de configuration suivant s’applique à un ordinateur virtuel avec 8 processeurs virtuels. Pour les cartes réseau interne et externe, vous pouvez activer RSS avec BaseProcNumber égal à 0 et MaxRssProcessors égal à 8 à l’aide de la commande Windows PowerShell suivante : <br> ```Set-NetAdapterRss “Internal�?,�?External�? –BaseProcNumber 0 –MaxProcessorNumber 8``` <br> |
| Mémoire tampon du côté d’envoi                   | Vous pouvez conserver la valeur par défaut les paramètres Send Side Buffer pour la carte de gestion. Pour interne et externe de cartes réseau, vous pouvez configurer Send Side Buffer à 32 Mo de RAM à l’aide de la commande Windows PowerShell suivante : <br> ```Set-NetAdapterAdvancedProperty “Internal�?,�?External�? –DisplayName “Send Buffer Size�? –DisplayValue “32MB�?``` <br>                                                       |
| Mémoire tampon du côté de réception                | Vous pouvez conserver la valeur par défaut Receive Side Buffer paramètres pour la carte de gestion. Pour les interne et les cartes réseau externes, vous pouvez configurer Receive Side Buffer à 16 Mo de RAM à l’aide de la commande Windows PowerShell suivante : <br> ```Set-NetAdapterAdvancedProperty “Internal�?,�?External�? –DisplayName “Receive Buffer Size�? –DisplayValue “16MB�?``` <br>                                            |
| Forward Optimization               | Vous pouvez conserver la valeur par défaut les paramètres Forward Optimization pour la carte de gestion. Pour les interne et les cartes réseau externes, vous pouvez activer Forward Optimization en utilisant la commande Windows PowerShell suivante : <br> ```Set-NetAdapterAdvancedProperty “Internal�?,�?External�? –DisplayName “Forward Optimization�? –DisplayValue “1�?``` <br>                                                                      |
