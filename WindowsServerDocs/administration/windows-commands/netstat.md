---
title: netstat
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 60e2718f-93cc-4ceb-bf0e-58a6a6e4fc8b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c06684eb73639e7480b5bad39d4d679739682800
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437148"
---
# <a name="netstat"></a>netstat

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche les connexions TCP actives, les ports sur lesquels l’ordinateur est à l’écoute, statistiques Ethernet, la table de routage IP, les statistiques IPv4 (pour les protocoles IP, ICMP, TCP et UDP) et statistiques IPv6 (pour IPv6, ICMPv6, TCP sur IPv6 et UDP sur les protocoles IPv6). Utilisée sans paramètres, **netstat** affiche les connexions TCP actives. 

## <a name="syntax"></a>Syntaxe
```
netstat [-a] [-e] [-n] [-o] [-p <Protocol>] [-r] [-s] [<Interval>]
```

### <a name="parameters"></a>Paramètres

|   Paramètre   |                                                                                                                                              Description                                                                                                                                              |
|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      -a       |                                                                                                   Affiche toutes les connexions TCP actives et les ports TCP et UDP écoutés par l’ordinateur.                                                                                                   |
|      -e       |                                                                                 Affiche les statistiques Ethernet, tels que le nombre d’octets et les paquets envoyés et reçus. Ce paramètre peut être combiné avec **-s**.                                                                                  |
|      -n       |                                                                               Affiche les connexions TCP actives, toutefois, adresses et numéros de port sont au format numérique et aucune tentative est effectuée pour déterminer les noms.                                                                               |
|      -o       |                          Affiche les connexions TCP actives et inclut l’ID de processus (PID) pour chaque connexion. Vous pouvez trouver l’application selon le PID sur l’onglet processus du Gestionnaire des tâches Windows. Ce paramètre peut être combiné avec **-** , **- n**, et **-p**.                           |
| -p <Protocol> |               Affiche les connexions pour le protocole spécifié par *protocole*. Dans ce cas, le *protocole* peut être tcp, udp, tcpv6 ou udpv6. Si ce paramètre est utilisé avec **-s** pour afficher des statistiques par protocole, *protocole* peut être tcp, udp, icmp, ip, tcpv6, udpv6, icmpv6 ou ipv6.                |
|      -s       | Affiche les statistiques par protocole. Par défaut, les statistiques sont affichées pour les protocoles TCP, UDP, ICMP et IP. Si le protocole IPv6 est installé, les statistiques sont affichées pour le protocole TCP sur IPv6, UDP sur IPv6, ICMPv6 et IPv6 protocoles. Le **-p** paramètre peut être utilisé pour spécifier un ensemble de protocoles. |
|      -r       |                                                                                                     Affiche le contenu de la table de routage IP. Cela équivaut à la commande route print.                                                                                                     |
|  <Interval>   |                                                        Actualise les informations sélectionnées chaque *intervalle* secondes. Appuyez sur CTRL + C pour arrêter le réaffichage. Si ce paramètre est omis, **netstat** imprime les informations sélectionnées dans une seule fois.                                                         |
|      /?       |                                                                                                                                 Affiche l'aide à l'invite de commandes.                                                                                                                                  |

## <a name="remarks"></a>Notes
-   Paramètres utilisés avec cette commande doivent être précédés d’un trait d’union ( **-** ) au lieu d’une barre oblique ( **/** ).
-   **netstat** fournit des statistiques pour les éléments suivants :
    -   Proto le nom du protocole (TCP ou UDP).
    -   Adresse locale de l’adresse IP de l’ordinateur local et le numéro de port utilisé. Le nom de l’ordinateur local qui correspond à l’adresse IP et le nom du port sont indiqués, sauf si le **- n** est précisé. Si le port n’est pas encore établi, le numéro de port est indiqué par un astérisque (*).
    -   étrangère adresse IP à l’adresse et numéro de port de l’ordinateur distant auquel le socket est connecté. Les noms qui correspond à l’adresse IP et le port sont affichés, sauf si le **- n** est précisé. Si le port n’est pas encore établi, le numéro de port est indiqué par un astérisque (*).
    -   (état) Indique l’état d’une connexion TCP. Les états possibles sont les suivantes : CLOSE_WAIT fermé établie FIN_WAIT_1 attente finale 2 LAST_ACK écoute Sync reçue SYN_SEND timeD_WAIT pour plus d’informations sur les États d’une connexion TCP, consultez la Rfc 793.
-   Cette commande est disponible uniquement si le protocole TCP/IP (Internet Protocol) est installé en tant que composant dans les propriétés d’une carte réseau dans Connexions réseau.

## <a name="BKMK_Examples"></a>Exemples
Pour afficher les statistiques Ethernet et les statistiques pour tous les protocoles, tapez :
```
netstat -e -s
```
Pour afficher les statistiques pour uniquement les protocoles TCP et UDP, tapez :
```
netstat -s -p tcp udp
```
Pour afficher les connexions TCP actives et les ID de processus toutes les 5 secondes, tapez :
```
netstat -o 5
```
Pour afficher le TCP active les connexions et le processus ID à l’aide de la forme numérique, tapez :
```
netstat -n -o
```

## <a name="additional-references"></a>Références supplémentaires
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
