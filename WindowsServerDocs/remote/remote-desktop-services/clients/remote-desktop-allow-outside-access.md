---
title: Bureau à distance - autoriser l’accès à votre PC à partir de votre réseau
description: En savoir plus sur les options disponibles pour accéder à distance à votre PC hors réseau de PC
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
ms.openlocfilehash: d77c362d9d06b70ad0747002ed8853a39e05b7ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888150"
---
# <a name="remote-desktop---allow-access-to-your-pc-from-outside-your-pcs-network"></a>Bureau à distance - autoriser l’accès à votre PC à partir de réseau de votre PC

>S'applique à : Windows 10, Windows Server 2016

Lorsque vous vous connectez à votre PC à l’aide d’un client Bureau à distance, vous créez une connexion peer-to-peer. Cela signifie que vous avez besoin d’un accès direct à l’ordinateur (parfois appelé « l’hôte »). Si vous avez besoin pour vous connecter à votre PC à partir en dehors du réseau de que votre ordinateur est en cours d’exécution, vous devez activer cet accès. Vous disposez de deux options : utiliser le réacheminement de port ou configurez un VPN.

## <a name="enable-port-forwarding-on-your-router"></a>Activer le transfert de port sur votre routeur

Réacheminement de port mappe simplement le port sur l’adresse IP de votre routeur (votre adresse IP publique) pour le port et l’adresse IP de l’ordinateur que vous souhaitez accéder. 

Les étapes spécifiques pour l’activation de réacheminement de port dépendent du routeur que vous utilisez, vous devrez rechercher en ligne pour obtenir des instructions de votre routeur. Pour obtenir une description générale des étapes, consultez [wikiHow pour définir du transfert de Port sur un routeur](https://www.wikihow.com/Set-Up-Port-Forwarding-on-a-Router).

Avant de mapper le port vous aurez besoin des éléments suivants :

- Adresse IP interne du PC : Regarder dans **Paramètres > réseau & Internet > état > Afficher les propriétés de votre réseau**. Trouver la configuration de réseau avec un état « Opérationnel », puis d’obtenir le **adresse IPv4**.

   ![Configuration de réseau opérationnel](../media/rdclient-operational-network.png)

- Votre adresse IP publique (du routeur IP). Il existe plusieurs façons de trouver : vous pouvez rechercher (dans Bing ou Google) pour « mon adresse IP » ou d’afficher le [propriétés du réseau Wi-Fi](https://binged.it/2Gwob34) (pour Windows 10).
- Numéro de port qui est mappé. Dans la plupart des cas il s’agit 3389 - qui est le port par défaut utilisé par les connexions Bureau à distance.
- Accès administrateur à votre routeur.  

   >[!WARNING]
   > Vous ouvrez votre PC jusqu'à internet : Assurez-vous que vous avez un mot de passe fort défini pour votre PC.

Une fois que vous mappez le port, vous serez en mesure de se connecter à votre ordinateur hôte à partir de l’extérieur du réseau local en vous connectant à l’adresse IP publique de votre routeur (la deuxième liste à puces ci-dessus).

L’adresse du routeur IP permettre changer, votre fournisseur de services internet (ISP) peut affecter une nouvelle adresse IP à tout moment. Pour éviter de rencontrer ce problème, envisagez d’utiliser un DNS dynamique : cela vous permet de vous connecter à l’ordinateur à l’aide d’un nom facile à retenir domaine, au lieu de l’adresse IP. Votre routeur met automatiquement à jour le service DDNS avec votre nouvelle adresse IP, il change.

Avec la plupart des routeurs, vous pouvez définir quelle adresse IP source ou le réseau source permettre utiliser le mappage de port. Par conséquent, si vous savez que vous voulez uniquement vous connecter à partir de travail, vous pouvez ajouter l’adresse IP pour votre réseau professionnel - qui vous permet d’éviter l’ouverture du port à l’internet public entière. Si l’hôte que vous utilisez pour vous connecter utilise des adresses IP dynamiques, définissez la restriction de la source pour autoriser l’accès à partir de l’ensemble de ce fournisseur de services Internet particulier.

Vous pouvez également envisager la configuration d’un [adresse IP statique](/windows-hardware/customize/mobile/mcsf/enable-static-ip) sur votre PC afin de l’adresse IP interne ne change pas. Si vous le faites qui, puis le routeur réacheminement de port sera toujours pointer vers l’adresse IP correcte.


## <a name="use-a-vpn"></a>Utiliser un VPN

Si vous vous connectez à votre réseau local à l’aide d’un réseau privé virtuel (VPN), vous n’êtes pas obligé d’ouvrir votre PC à l’internet public. Au lieu de cela, lorsque vous vous connectez au VPN, votre client Bureau à distance se comporte comme il fait partie du même réseau et être en mesure d’accéder à votre PC. Il existe un nombre de services VPN : vous pouvez trouver et utiliser selon ce qui convient le mieux.