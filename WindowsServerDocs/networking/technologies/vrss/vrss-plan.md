---
title: Planifier l’utilisation de vRSS
description: Vous pouvez utiliser cette rubrique pour préparer votre machine virtuelle et l’hôte Hyper-V à l’aide de vRSS dans Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 695e6192-5e84-4ab4-b33e-8ebf6b8f5cbb
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/04/2018
ms.openlocfilehash: e6558b00e87721d8ab81c84946a14745c4faa812
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133385"
---
# Planifier l’utilisation de vRSS

>S’applique à: Windows Server (canal semi-annuel), WindowsServer2016

Dans Windows Server 2016, vRSS est activé par défaut, mais vous devez préparer votre environnement pour permettre à vRSS fonctionner correctement dans une machine virtuelle \(VM\) ou sur un hôte adaptateur virtuel \(vNIC\). Dans Windows Server 2012 R2, vRSS a été désactivé par défaut.

Lorsque vous planifiez et préparez l’utilisation de vRSS, vérifiez que:

- La carte réseau physique est compatible avec la file d’attente de la Machine virtuelle \(VMQ\) et a au moins une vitesse de liaison de 10 Gbits/s.
- VMQ est activé sur la carte réseau physique et sur le port de commutateur virtuel Hyper\-V
- Il n’existe aucune \(SR\-IOV\) la virtualisation de sortie unique Input\ racine configuré pour l’ordinateur virtuel.
- Association de cartes réseau est correctement configurée.
- L’ordinateur virtuel a plusieurs processeurs logiques \(LPs\).

>[!NOTE]
>vRSS est également activée par défaut pour les cartes réseau virtuelles hôtes qui ont RSS activé.

Voici des informations supplémentaires, vous devez effectuer ces étapes de préparation.
  
1. **Capacité de l’adaptateur réseau**. Vérifiez que la carte réseau est compatible avec la file d’attente de la Machine virtuelle \(VMQ\) et a une vitesse de liaison de 10 Gbits/s ou plus. Si la vitesse de liaison est inférieure à 10 Gbits/s, le commutateur virtuel Hyper\-V désactive VMQ par défaut, même si elle s’affiche toujours VMQ est activée dans les résultats de la commande Windows PowerShell **Get-NetAdapterVmq**. Une méthode, que vous pouvez utiliser pour vérifier que VMQ est activé ou désactivé consiste à utiliser la commande **Get-NetAdapterVmqQueue**.  Si VMQ est désactivée, les résultats de cette commande montrent qu’il n’existe aucune QueueID affecté à la carte réseau virtuelle hôte ou d’ordinateur virtuel. 
  
2. **Activer VMQ**. Vérifiez que VMQ est activé sur l’ordinateur hôte. vRSS ne fonctionne pas si l’hôte ne prend pas en charge VMQ. Vous pouvez vérifier que VMQ est activée par **Get-VMSwitch** en cours d’exécution et la recherche de la carte réseau qui utilise le commutateur virtuel. Ensuite, exécutez **Get-NetAdapterVmq** et vous assurer que la carte s’affiche dans les résultats et a VMQ activé.
  
3. **Absence de SR\-IOV**. Vérifiez qu’un \(SR\-IOV\) la virtualisation de sortie unique racine Input\ pilote de fonction virtuelle \(VF\) n’est pas associé à l’interface réseau de machine virtuelle. Vous pouvez le vérifier à l’aide de la commande **Get-NetAdapterSriov** . Si un pilote de facteur est chargé, RSS utilise les paramètres de mise à l’échelle à partir de ce pilote au lieu de ceux qui sont configurés par vRSS. Si le pilote de facteur ne prend pas en charge RSS, vRSS est désactivée.
  
4. **Association de Configuration**. Si vous utilisez l’association de cartes réseau, il est important de configurer correctement VMQ pour fonctionner avec les paramètres d’association de cartes. Pour obtenir des informations détaillées sur l’association de cartes de déploiement et de gestion, consultez [l’Association de cartes](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

5. **Nombre de PL**. Vérifiez que l’ordinateur virtuel a plusieurs processeurs logiques \(LP\). vRSS s’appuie sur RSS dans la machine virtuelle ou sur l’hôte Hyper-V pour équilibrer reçu le trafic sur plusieurs PL pour le traitement en parallèle. Vous pouvez observer PL combien votre machine virtuelle a en exécutant la commande Windows PowerShell **Get-VMProcessor** dans l’hôte. Une fois que vous exécutez la commande, vous pouvez observer l’entrée de la colonne nombre pour le nombre de pl.

La carte réseau virtuelle hôte dispose toujours d’un accès à tous les processeurs physiques; Pour configurer la carte réseau virtuelle hôte pour utiliser un certain nombre de processeurs, utilisez les paramètres **- BaseProcessorNumber** et **-MaxProcessors** lorsque vous exécutez la commande Windows PowerShell **Set-NetAdapterRss** .

---