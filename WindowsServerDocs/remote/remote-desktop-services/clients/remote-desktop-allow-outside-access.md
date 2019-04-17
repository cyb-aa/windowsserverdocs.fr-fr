---
title: Bureau à distance - autoriser l’accès à votre ordinateur à partir de l’extérieur de votre réseau
description: En savoir plus sur les options d’accès à distance à votre ordinateur à partir de l’extérieur du réseau de l’ordinateur
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
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "1708657"
---
# <a name="remote-desktop---allow-access-to-your-pc-from-outside-your-pcs-network"></a>Bureau à distance - autoriser l’accès à votre ordinateur à partir de l’extérieur du réseau de votre ordinateur

>S’applique à: Windows 10, Windows Server 2016

Lorsque vous vous connectez à votre ordinateur à l’aide d’un client de bureau à distance, vous créez une connexion d’égal à égal. Cela signifie que vous avez besoin d’un accès direct à l’ordinateur de bureau (parfois appelée «l’hôte»). Si vous avez besoin pour se connecter à votre ordinateur à partir d’en dehors du réseau de que votre ordinateur est en cours d’exécution, vous devez activer cet accès. Vous disposez de deux options: utiliser le transfert de port ou de configurer un VPN.

## <a name="enable-port-forwarding-on-your-router"></a>Activer le transfert de port sur votre routeur.

Réacheminement de port mappe simplement le port sur l’adresse IP de votre routeur (votre adresse IP publique) sur le port et l’adresse IP de l’ordinateur que vous souhaitez accéder. 

Les étapes spécifiques permettant de réacheminement de port dépendent le routeur que vous utilisez, il vous faudra donc rechercher en ligne pour obtenir des instructions de votre routeur. Pour une présentation générale des étapes, consultez la rubrique [wikiHow pour définir du transfert de Port sur un routeur](https://www.wikihow.com/Set-Up-Port-Forwarding-on-a-Router).

Avant de mapper le port, vous devez les éléments suivants:

- Adresse IP interne de PC: regardez dans **Paramètres > réseau et Internet > état > Afficher les propriétés de votre réseau**. Trouvez la configuration réseau dont le statut est «Opérationnel», puis obtenir l' **adresse IPv4**.

   ![Configuration réseau opérationnel](../media/rdclient-operational-network.png)

- Votre adresse IP publique (adresse IP du routeur). Il existe plusieurs façons pour rechercher ce: vous pouvez rechercher (dans Bing ou Google) pour «mon adresse IP» ou afficher les [Propriétés de réseau Wi-Fi](https://binged.it/2Gwob34) (pour Windows 10).
- Numéro de port qui est mappé. Dans la plupart des cas, il s’agit 3389 - qui est le port par défaut utilisé par les connexions de bureau à distance.
- Accès d’administration sur votre routeur.  

   >[!WARNING]
   > Vous ouvrez votre ordinateur à internet, vérifiez que vous disposez d’un mot de passe fort définies pour votre PC.

Après avoir défini le port, vous serez en mesure de se connecter à votre ordinateur hôte à partir de l’extérieur du réseau local en se connectant à l’adresse IP publique de votre routeur (la deuxième puce ci-dessus).

L’adresse du routeur IP peut modifier - votre fournisseur de services internet (fournisseur de services Internet) pouvez attribuer une nouvelle adresse IP à tout moment. Pour éviter de rencontrer ce problème, envisagez d’utiliser le système DNS dynamique: cela vous permet de vous connecter à l’ordinateur à l’aide d’un nom de domaine, au lieu de l’adresse IP facilement. Votre routeur met à jour automatiquement le service DDNS avec la nouvelle adresse IP, il change.

Avec la plupart des routeurs, vous pouvez définir les adresse IP source ou le réseau source peut utiliser le mappage de port. Par conséquent, si vous savez que vous allez uniquement se connecter à partir de travail, vous pouvez ajouter l’adresse IP pour votre réseau de travail - qui vous permet d’éviter les ouvrir le port à l’internet public entière. Si l’hôte que vous utilisez pour vous connecter utilise des adresses IP dynamiques, définissez la restriction de source pour autoriser l’accès à partir de l’ensemble de ce fournisseur de services Internet particulier.

Vous pouvez également envisager la configuration d’une [adresse IP statique](/windows-hardware/customize/mobile/mcsf/enable-static-ip) sur votre ordinateur afin de ne modifie pas l’adresse IP interne. Si vous procédez ainsi que, puis du routeur réacheminement de port sera toujours pointer vers l’adresse IP correcte.


## <a name="use-a-vpn"></a>Utilisez un réseau privé virtuel

Si vous vous connectez à votre réseau local à l’aide d’un réseau privé virtuel (VPN), vous n’êtes pas obligé d’ouvrir votre PC à l’internet public. Au lieu de cela, lorsque vous vous connectez au réseau VPN, votre client de bureau à distance se comporte comme il fait partie du même réseau et être en mesure d’accéder à votre ordinateur. Il existe un nombre de services VPN: vous pouvez trouver et utiliser selon la situation qui convient le mieux.