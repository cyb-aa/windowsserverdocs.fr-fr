---
title: Planifier l’utilisation de vRSS
description: Vous pouvez utiliser cette rubrique pour préparer votre ordinateur virtuel et votre hôte Hyper-V pour l’utilisation de vRSS dans Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 695e6192-5e84-4ab4-b33e-8ebf6b8f5cbb
ms.localizationpriority: medium
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/04/2018
ms.openlocfilehash: befd2839319571827b88a1c7b2777c55861e31fa
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315262"
---
# <a name="plan-the-use-of-vrss"></a>Planifier l’utilisation de vRSS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans Windows Server 2016, vRSS est activé par défaut. Toutefois, vous devez préparer votre environnement pour permettre à vRSS de fonctionner correctement dans une machine virtuelle \(ordinateur virtuel\) ou sur une carte virtuelle hôte \(carte réseau virtuelle\). Dans Windows Server 2012 R2, vRSS a été désactivé par défaut.

Lors de la planification et de la préparation de l’utilisation de vRSS, vérifiez les éléments suivants :

- La carte réseau physique est compatible avec File d’attente d’ordinateurs virtuels \(l'\) d’ordinateurs virtuels et a une vitesse de liaison de 10 Gbits/s ou plus.
- Les ordinateurs virtuels sont activés sur la carte réseau physique et sur le port de commutateur virtuel Hyper\-V
- Il n’existe aucune entrée racine unique\-la virtualisation de sortie \(SR\-IOV\) configurée pour la machine virtuelle.
- L’Association de cartes réseau est configurée correctement.
- La machine virtuelle est dotée de plusieurs processeurs logiques \(de\).

>[!NOTE]
>le paramètre vRSS est également activé par défaut pour tous les cartes réseau virtuelles hôtes pour lesquels RSS est activé.

Vous trouverez ci-dessous des informations supplémentaires dont vous avez besoin pour effectuer ces étapes de préparation.
  
1. **Capacité de la carte réseau**. Vérifiez que la carte réseau est compatible avec File d’attente d’ordinateurs virtuels \(d’ordinateurs virtuels\) et a une vitesse de liaison de 10 Gbits/s ou plus. Si la vitesse de la liaison est inférieure à 10 Gbits/s, le commutateur virtuel Hyper\-V désactive la fonction d’extension de l’espace de main par défaut, même si l’option d’espace de messagerie est toujours activée dans les résultats de la commande Windows PowerShell **-NetAdapterVmq**. Une méthode que vous pouvez utiliser pour vérifier que la gestion des ordinateurs virtuels est activée ou désactivée consiste à utiliser la commande **NetAdapterVmqQueue**.  Si la machine virtuelle est désactivée, les résultats de cette commande indiquent qu’aucun QueueID n’est affecté à la machine virtuelle ou à la carte réseau virtuelle hôte. 
  
2. **Activez**les ordinateurs virtuels. Vérifiez que la file d’attente d’ordinateurs virtuels est activée sur l’ordinateur hôte. vRSS ne fonctionne pas si l’ordinateur hôte ne prend pas en charge les ordinateurs virtuels. Vous pouvez vérifier que l’ordinateur virtuel est activé en exécutant l’option **obtenir-VMSwitch** et en recherchant la carte utilisée par le commutateur virtuel. Exécutez ensuite **Get-NetAdapterVmq** et vérifiez que la carte s’affiche dans les résultats avec la file d’attente d’ordinateurs virtuels activée.
  
3. **Absence de SR\-IOV**. Vérifiez qu’une entrée racine unique\-la virtualisation de sortie \(SR\-IOV\) fonction virtuelle \(VF\) pilote n’est pas attachée à l’interface réseau de la machine virtuelle. Vous pouvez le vérifier à l’aide de la commande **NetAdapterSriov** . Si un pilote VF est chargé, RSS utilise les paramètres de mise à l’échelle de ce pilote au lieu de ceux configurés par vRSS. Si le pilote VF ne prend pas en charge RSS, vRSS est désactivé.
  
4. **Configuration de l’Association de cartes réseau**. Si vous utilisez l’Association de cartes réseau, il est important de configurer correctement les ordinateurs virtuels de manière à ce qu’ils fonctionnent avec les paramètres d’association de cartes réseau. Pour plus d’informations sur le déploiement et la gestion de l’Association de cartes réseau, consultez [Association de cartes réseau](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

5. **Nombre de**. Vérifiez que la machine virtuelle possède plusieurs processeurs logiques \(\)LP. vRSS s’appuie sur RSS sur la machine virtuelle ou sur l’hôte Hyper-V pour équilibrer la charge du trafic reçu vers plusieurs processeurs virtuels en vue d’un traitement parallèle. Vous pouvez observer le nombre de bits de votre machine virtuelle en exécutant la commande Windows PowerShell **VMProcessor** dans l’hôte. Une fois que vous avez exécuté la commande, vous pouvez observer l’entrée de la colonne Count pour le nombre de.

L’hôte carte réseau virtuelle a toujours accès à tous les processeurs physiques. pour configurer l’ordinateur hôte carte réseau virtuelle pour qu’il utilise un nombre spécifique de processeurs, utilisez les paramètres **-BaseProcessorNumber** et **-MaxProcessors** lorsque vous exécutez la commande Windows PowerShell **Set-NetAdapterRss** .

---