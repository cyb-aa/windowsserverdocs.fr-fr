---
title: Configurer des réseaux locaux virtuels pour Hyper-V
description: Fournit des instructions pour la configuration d’un réseau local virtuel (VLAN) pour une utilisation par les ordinateurs virtuels sur un ordinateur hôte Hyper-V.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8510a709-001c-4eee-b6d6-c451e8a8a836
author: KBDAzure
ms.author: kathydav
ms.date: 10/11/2016
ms.openlocfilehash: 5b5eaf175e7c09124aaa3f7a33523e8b87a9ae84
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848460"
---
# <a name="configure-virtual-local-area-networks-for-hyper-v"></a>Configurer des réseaux locaux virtuels pour Hyper-V
Réseaux locaux virtuels \(VLAN\) offrent un moyen pour isoler le trafic réseau. Réseaux locaux virtuels sont configurés dans les commutateurs et les routeurs qui prennent en charge 802. 1 q. Si vous configurez plusieurs réseaux locaux virtuels et que vous souhaitez que les communications entre les, vous devez configurer les périphériques réseau pour autoriser que. 

Vous avez besoin des éléments à configurer des réseaux locaux virtuels :  
  
-   Une carte réseau physique et le pilote qui prend en charge 802. 1 q un marquage VLAN.  
-   Un commutateur réseau physique qui prend en charge 802. 1 q un marquage VLAN.  
  
Sur l’ordinateur hôte, vous devez configurer le commutateur virtuel pour autoriser le trafic réseau sur le port de commutateur physique. Il s’agit de l’ID de VLAN que vous souhaitez utiliser en interne avec des machines virtuelles. Ensuite, vous configurez la machine virtuelle pour spécifier le réseau local virtuel que la machine virtuelle utilisera pour toutes les communications réseau.  
  
#### <a name="to-allow-a-virtual-switch-to-use-a-vlan"></a>Pour autoriser un commutateur virtuel pour utiliser un réseau local virtuel  
  
1.  Ouvrez Hyper\-V Manager.  
  
2.  Dans le menu Actions, cliquez sur **Gestionnaire de commutateur virtuel**.  
  
3.  Sous **commutateurs virtuels**, sélectionnez un commutateur virtuel connecté à une carte réseau physique qui prend en charge des réseaux locaux virtuels. 

4. Dans le volet droit, sous l’ID de réseau local virtuel, sélectionnez **activer l’identification LAN virtuelle** puis tapez un nombre pour l’ID. VLAN  
  
    Tout le trafic qui passe par la carte réseau physique connectée au commutateur virtuel est marqué avec l’ID de VLAN que vous définissez.  
  
#### <a name="to-allow-a-virtual-machine-to-use-a-vlan"></a>Pour autoriser une machine virtuelle à utiliser un réseau local virtuel  
  
1.  Ouvrez Hyper\-V Manager.  
  
2.  Dans le volet de résultats, sous **Machines virtuelles**puis avec le bouton droit et sélectionnez la machine virtuelle appropriée **paramètres**.  

3.  Sous **matériel**, sélectionnez un commutateur virtuel est configuré avec un réseau local virtuel.
  
4.  Dans le volet droit, sélectionnez **activer l’identification LAN virtuelle**, puis tapez le même ID de VLAN que celui que vous avez spécifié pour le commutateur virtuel. 

Si la machine virtuelle doit utiliser plusieurs réseaux locaux virtuels, effectuez l’une des opérations suivantes :  
  
-   Se connecter plusieurs cartes réseau virtuelles pour appropriées des commutateurs virtuels et affecter l’ID de VLAN. Veillez à configurer les adresses IP correctement et que le trafic que vous souhaitez acheminer via le réseau local virtuel également utilise l’adresse IP correcte.  
  
-   Configurer la carte réseau virtuelle word en mode trunk à l’aide du [définir\-VMNetworkAdapterVlan](https://technet.microsoft.com/library/hh848475.aspx) applet de commande.
  
## <a name="see-also"></a>Voir aussi  
 
[Hyper\-V Virtual Switch](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/hyper-v-virtual-switch)