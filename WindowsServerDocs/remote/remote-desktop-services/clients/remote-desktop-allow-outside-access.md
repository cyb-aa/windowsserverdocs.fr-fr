---
title: Bureau à distance - Accorder un accès à votre PC à partir de l’extérieur du réseau
description: En savoir plus sur les options disponibles pour accéder à distance à votre PC depuis l’extérieur du réseau
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: haley-rowland
manager: dongill
ms.author: elizapo
ms.date: 04/04/2018
ms.localizationpriority: medium
ms.openlocfilehash: 9e90a2faa14b65bc766c7d7ec47d5e815658c06e
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66805059"
---
# <a name="remote-desktop---allow-access-to-your-pc-from-outside-your-pcs-network"></a>Bureau à distance - Accorder un accès à votre PC à partir de l’extérieur du réseau de votre PC

>S’applique à : Windows 10, Windows Server 2016

Lorsque vous vous connectez à votre PC à l’aide d’un client Bureau à distance, vous créez une connexion peer-to-peer. Cela signifie que vous avez besoin d’un accès direct au PC (parfois appelé « l’hôte »). Si vous devez vous connecter à votre PC depuis l’extérieur du réseau et que votre ordinateur est allumé, vous devez activer cet accès. Vous avez deux possibilités : utiliser le réacheminement de port ou configurer un VPN.

## <a name="enable-port-forwarding-on-your-router"></a>Activer le réacheminement de port sur votre routeur

Le réacheminement de port mappe simplement le port sur l’adresse IP de votre routeur (votre adresse IP publique) sur le port et l’adresse IP du PC auquel vous souhaitez accéder. 

Les étapes d’activation du réacheminement de port dépendent du routeur que vous utilisez, vous devez donc rechercher en ligne les instructions pour votre routeur. Pour obtenir une description générale des étapes, consultez [wikiHow pour configurer le réacheminement de port sur un routeur](https://www.wikihow.com/Set-Up-Port-Forwarding-on-a-Router).

Avant de mapper le port, vous aurez besoin des éléments suivants :

- Adresse IP interne du PC : Regardez dans **Paramètres > Réseau et Internet > État > Afficher les propriétés de votre réseau**. Recherchez la configuration réseau avec l’état « Opérationnel », et récupérez l’**adresse IPv4**.

   ![Configuration du fonctionnement réseau](../media/rdclient-operational-network.png)

- Votre adresse IP publique (l’adresse IP du routeur). Il existe plusieurs façons de trouver cette information : vous pouvez rechercher (dans Bing ou Google) « mon adresse IP » ou consulter les [propriétés du réseau Wi-Fi](https://binged.it/2Gwob34) (pour Windows 10).
- Le numéro de port mappé. Dans la plupart des cas il s’agit du port 3389, qui est le port utilisé par défaut pour les connexions Bureau à distance.
- Un accès administrateur à votre routeur.  

   >[!WARNING]
   > Vous ouvrez votre PC aux accès Internet : assurez-vous que vous avez défini un mot de passe fort sur votre PC.

Une fois que vous avez mappé le port, vous pourrez vous connecter à votre PC hôte depuis l’extérieur du réseau local en se connectant à l’adresse IP publique de votre routeur (la deuxième entrée de liste à puces ci-dessus).

L’adresse du routeur IP peut changer : votre fournisseur d’accès Internet peut affecter une nouvelle adresse IP à tout moment. Pour éviter de rencontrer ce problème, envisagez d’utiliser un DNS dynamique : cela vous permet de vous connecter au PC à l’aide d’un nom de domaine facile à retenir, au lieu de l’adresse IP. Votre routeur met automatiquement à jour le service DDNS avec votre nouvelle adresse IP, si elle change.

Avec la plupart des routeurs, vous pouvez définir quelle adresse IP source ou quel réseau source peut utiliser le mappage de port. Par conséquent, si vous savez que vous voulez uniquement vous connecter à partir du bureau, vous pouvez ajouter l’adresse IP de votre réseau professionnel, ce qui vous permet d’éviter l’ouverture du port à Internet. Si l’hôte que vous utilisez pour vous connecter utilise des adresses IP dynamiques, définissez la restriction de source pour autoriser l’accès à partir de la plage de ce fournisseur d’accès Internet.

Vous pouvez également envisager la configuration d’une [adresse IP statique](/windows-hardware/customize/mobile/mcsf/enable-static-ip) sur votre PC afin que l’adresse IP interne ne change pas. Si vous le faites, le réacheminement de port du routeur pointera toujours vers la bonne adresse IP.


## <a name="use-a-vpn"></a>Utiliser un VPN

Si vous vous connectez à votre réseau local à l’aide d’un réseau privé virtuel (VPN), vous n’êtes pas obligé d’ouvrir votre PC à l’Internet public. Au lieu de cela, lorsque vous vous connectez au VPN, votre client Bureau à distance se comporte comme s’il faisait partie du même réseau et s’il était en mesure d’accéder à votre PC. Il existe de nombreux services VPN : vous pouvez utiliser celui qui vous convient le mieux.