---
title: Configurer des réseaux locaux virtuels pour Hyper-V
description: Fournit des instructions sur la configuration d’un réseau local virtuel (VLAN) pour une utilisation par les ordinateurs virtuels sur un ordinateur hôte Hyper-V.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8510a709-001c-4eee-b6d6-c451e8a8a836
author: KBDAzure
ms.author: kathydav
ms.date: 10/11/2016
ms.openlocfilehash: f2c240a3ad9f9783e509efb288cc6c6410339685
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364279"
---
# <a name="configure-virtual-local-area-networks-for-hyper-v"></a>Configurer des réseaux locaux virtuels pour Hyper-V
Les réseaux locaux virtuels \(les réseaux locaux virtuels\) offrent un moyen d’isoler le trafic réseau. Les réseaux locaux virtuels sont configurés dans des commutateurs et des routeurs qui prennent en charge 802.1 q. Si vous configurez plusieurs réseaux locaux virtuels et que vous souhaitez que la communication se produise entre eux, vous devez configurer les périphériques réseau pour l’autoriser. 

Pour configurer des réseaux locaux virtuels, vous aurez besoin des éléments suivants :  
  
-   Une carte réseau physique et un pilote qui prend en charge l’identification du réseau local virtuel 802.1 q.  
-   Un commutateur réseau physique qui prend en charge le marquage VLAN 802.1 q.  
  
Sur l’ordinateur hôte, vous configurez le commutateur virtuel pour autoriser le trafic réseau sur le port commuté physique. Il s’agit des ID de réseau local virtuel que vous souhaitez utiliser en interne avec les ordinateurs virtuels. Ensuite, vous configurez l’ordinateur virtuel pour spécifier le VLAN que l’ordinateur virtuel utilisera pour toutes les communications réseau.  
  
#### <a name="to-allow-a-virtual-switch-to-use-a-vlan"></a>Pour autoriser un commutateur virtuel à utiliser un réseau local virtuel  
  
1.  Ouvrez le Gestionnaire Hyper\-V.  
  
2.  Dans le menu actions, cliquez sur **Gestionnaire de commutateur virtuel**.  
  
3.  Sous **commutateurs virtuels**, sélectionnez un commutateur virtuel connecté à une carte réseau physique qui prend en charge les réseaux locaux virtuels. 

4. Dans le volet droit, sous ID de réseau local virtuel, sélectionnez **activer l’identification de réseau local virtuel** , puis tapez un nombre pour l’ID de réseau local virtuel.  
  
    Tout le trafic qui transite par la carte réseau physique connectée au commutateur virtuel est identifié par l’ID de réseau local virtuel que vous avez défini.  
  
#### <a name="to-allow-a-virtual-machine-to-use-a-vlan"></a>Pour autoriser un ordinateur virtuel à utiliser un réseau local virtuel  
  
1.  Ouvrez le Gestionnaire Hyper\-V.  
  
2.  Dans le volet de résultats, sous **ordinateurs virtuels**, sélectionnez l’ordinateur virtuel approprié, puis cliquez avec le bouton droit sur **paramètres**.  

3.  Sous **matériel**, sélectionnez un commutateur virtuel configuré avec un réseau local virtuel.
  
4.  Dans le volet droit, sélectionnez **activer l’identification du réseau local virtuel**, puis tapez le même ID de réseau local virtuel que celui que vous avez spécifié pour le commutateur virtuel. 

Si la machine virtuelle doit utiliser davantage de réseaux locaux virtuels, effectuez l’une des opérations suivantes :  
  
-   Connectez plus de cartes réseau virtuelles aux commutateurs virtuels appropriés et attribuez les ID de réseau local virtuel. Veillez à configurer correctement les adresses IP et que le trafic que vous souhaitez acheminer via le réseau local virtuel utilise également l’adresse IP correcte.  
  
-   Configurez l’adaptateur de mot de réseau virtuel en mode Trunk à l’aide de l' [ensemble\-VMNetworkAdapterVlan](https://technet.microsoft.com/library/hh848475.aspx) applet.
  
## <a name="see-also"></a>Voir aussi  
 
[Commutateur virtuel Hyper\-V](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/hyper-v-virtual-switch)