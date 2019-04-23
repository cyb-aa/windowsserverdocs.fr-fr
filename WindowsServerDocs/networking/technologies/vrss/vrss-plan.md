---
title: Planifiez l’utilisation de vRSS
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850440"
---
# <a name="plan-the-use-of-vrss"></a>Planifiez l’utilisation de vRSS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans Windows Server 2016, vRSS est activé par défaut, mais vous devez préparer votre environnement pour permettre la vRSS fonctionner correctement dans une machine virtuelle \(machine virtuelle\) ou sur une carte virtuelle hôte \(carte réseau virtuelle\). Dans Windows Server 2012 R2, vRSS a été désactivée par défaut.

Lorsque vous planifiez et préparez l’utilisation de vRSS, vérifiez que :

- La carte réseau physique est compatible avec la file d’attente de la Machine virtuelle \(VMQ\) et a une vitesse de liaison de 10 Gbits/s ou plus.
- VMQ est activé sur la carte réseau physique et sur l’Hyper\-port de commutateur virtuel V
- Il n’existe aucune entrée de racine unique\-Output Virtualization \(SR\-IOV\) configuré pour la machine virtuelle.
- Association de cartes réseau est configurée correctement.
- La machine virtuelle a plusieurs processeurs logiques \(LPs\).

>[!NOTE]
>vRSS est également activée par défaut pour les cartes réseau virtuelles hôtes qui ont RSS est activé.

Voici des informations supplémentaires, vous devez suivre ces étapes de préparation.
  
1. **Capacité de la carte réseau**. Vérifiez que la carte réseau est compatible avec la file d’attente de la Machine virtuelle \(VMQ\) et a une vitesse de liaison de 10 Gbits/s ou plus. Si la vitesse de liaison est inférieure à 10 Gbits/s, le Hyper\-désactive le commutateur virtuel V VMQ par défaut, même si elle s’affiche toujours VMQ comme activé dans les résultats de la commande Windows PowerShell **Get-NetAdapterVmq**. Une méthode, vous pouvez utiliser pour vérifier que les ordinateurs virtuels est activé ou désactivé consiste à utiliser la commande **Get-NetAdapterVmqQueue**.  Si les ordinateurs virtuels est désactivée, les résultats de cette commande indiquent qu’il n’existe aucun QueueID affecté à la carte réseau virtuelle hôte ou de la machine virtuelle. 
  
2. **Activer VMQ**. Vérifiez que la file d’attente d’ordinateurs virtuels est activée sur l’ordinateur hôte. vRSS ne fonctionne pas si l’hôte ne prend pas en charge les ordinateurs virtuels. Vous pouvez vérifier que les ordinateurs virtuels est activée en exécutant **Get-VMSwitch** et recherchez la carte qui utilise le commutateur virtuel. Exécutez ensuite **Get-NetAdapterVmq** et vérifiez que la carte s’affiche dans les résultats avec la file d’attente d’ordinateurs virtuels activée.
  
3. **Absence de SR\-IOV**. Vérifiez qu’une seule racine d’entrée\-Output Virtualization \(SR\-IOV\) fonction virtuelle \(VF\) pilote n’est pas attaché à l’interface réseau de machine virtuelle. Vous pouvez le vérifier à l’aide de la **Get-NetAdapterSriov** commande. Si un pilote de fonction virtuelle est chargé, RSS utilise les paramètres de mise à l’échelle à partir de ce pilote au lieu de ceux configurés par vRSS. Si le pilote de fonction virtuelle ne prend pas en charge RSS, vRSS est désactivé.
  
4. **Configuration d’association de cartes réseau**. Si vous utilisez l’association de cartes réseau, il est important de configurer correctement les ordinateurs virtuels pour travailler avec les paramètres d’association de cartes réseau. Pour obtenir des informations détaillées sur l’association de cartes réseau de déploiement et la gestion, consultez [association de cartes réseau](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

5. **Nombre de LPs**. Vérifiez que la machine virtuelle possède plusieurs processeurs logiques \(LP\). vRSS s’appuie sur RSS dans la machine virtuelle ou sur l’hôte Hyper-V pour équilibrer reçu le trafic vers plusieurs LPs pour un traitement parallèle. Vous pouvez observer combien LPs a de votre machine virtuelle en exécutant la commande Windows PowerShell **Get-VMProcessor** dans l’hôte. Après avoir exécuté la commande, vous pouvez observer l’entrée de la colonne nombre pour le nombre de LPs.

La carte réseau virtuelle hôte a toujours accès à tous les processeurs physiques ; Pour configurer la carte réseau virtuelle hôte pour utiliser un certain nombre de processeurs, utilisez les paramètres **- BaseProcessorNumber** et **- MaxProcessors** lorsque vous exécutez le **Set-NetAdapterRss** Commande de Windows PowerShell.

---