---
title: netstat
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 54bd21b7e96275d329e45e825971d9236488c793
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373264"
---
# <a name="netstat"></a>netstat

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Affiche les connexions TCP actives, les ports sur lesquels l’ordinateur écoute, les statistiques Ethernet, la table de routage IP, les statistiques IPv4 (pour les protocoles IP, ICMP, TCP et UDP) et les statistiques IPv6 (pour les protocoles IPv6, ICMPv6, TCP sur IPv6 et UDP sur IPv6). Utilisé sans paramètres, **netstat** affiche les connexions TCP actives. 

## <a name="syntax"></a>Syntaxe
```
netstat [-a] [-e] [-n] [-o] [-p <Protocol>] [-r] [-s] [<Interval>]
```

### <a name="parameters"></a>Paramètres

|   Paramètre   |                                                                                                                                              Description                                                                                                                                              |
|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      -a       |                                                                                                   Affiche toutes les connexions TCP actives et les ports TCP et UDP sur lesquels l’ordinateur est à l’écoute.                                                                                                   |
|      -e       |                                                                                 Affiche des statistiques Ethernet, telles que le nombre d’octets et de paquets envoyés et reçus. Ce paramètre peut être combiné avec **-s**.                                                                                  |
|      -n       |                                                                               Affiche les connexions TCP actives, mais les adresses et les numéros de port sont exprimés numériquement et aucune tentative n’est faite pour déterminer les noms.                                                                               |
|      -o       |                          Affiche les connexions TCP actives et comprend l’ID de processus (PID) de chaque connexion. Vous pouvez trouver l’application en fonction du PID sous l’onglet processus du gestionnaire des tâches de Windows. Ce paramètre peut être combiné avec **-a**, **-n**et **-p**.                           |
| -p <Protocol> |               Affiche les connexions pour le protocole spécifié par le *protocole*. Dans ce cas, le *protocole* peut être TCP, UDP, TCPv6 ou UDPv6. Si ce paramètre est utilisé avec **-s** pour afficher les statistiques par protocole, le *protocole* peut être TCP, UDP, ICMP, IP, TCPv6, UDPv6, ICMPv6 ou IPv6.                |
|      -s       | Affiche les statistiques par protocole. Par défaut, les statistiques sont affichées pour les protocoles TCP, UDP, ICMP et IP. Si le protocole IPv6 est installé, les statistiques sont affichées pour les protocoles TCP sur IPv6, UDP sur IPv6, ICMPv6 et IPv6. Le paramètre **-p** peut être utilisé pour spécifier un ensemble de protocoles. |
|      -r       |                                                                                                     Affiche le contenu de la table de routage IP. Cela équivaut à la commande route print.                                                                                                     |
|  <Interval>   |                                                        Affiche à nouveau les informations sélectionnées chaque *intervalle* en secondes. Appuyez sur CTRL + C pour arrêter le réaffichage. Si ce paramètre est omis, **netstat** n’imprime les informations sélectionnées qu’une seule fois.                                                         |
|      /?       |                                                                                                                                 Affiche l'aide à l'invite de commandes.                                                                                                                                  |

## <a name="remarks"></a>Notes
-   Les paramètres utilisés avec cette commande doivent être précédés d’un trait d’Union ( **-** ) et non d’une barre oblique ( **/** ).
-   **netstat** fournit des statistiques pour les éléments suivants :
    -   Proto nom du protocole (TCP ou UDP).
    -   Adresse locale adresse IP de l’ordinateur local et numéro de port utilisé. Le nom de l’ordinateur local qui correspond à l’adresse IP et le nom du port s’affichent sauf si le paramètre **-n** est spécifié. Si le port n’est pas encore établi, le numéro de port est affiché sous la forme d’un astérisque (*).
    -   adresse étrangère adresse IP et numéro de port de l’ordinateur distant auquel le socket est connecté. Les noms qui correspondent à l’adresse IP et au port s’affichent, sauf si le paramètre **-n** est spécifié. Si le port n’est pas encore établi, le numéro de port est affiché sous la forme d’un astérisque (*).
    -   Département Indique l’état d’une connexion TCP. Les États possibles sont les suivants : CLOSE_WAIT CLOSEd FIN_WAIT_1 FIN_WAIT_2 LAST_ACK listEN SYN_RECEIVED SYN_SEND timeD_WAIT pour plus d’informations sur les États d’une connexion TCP, consultez la RFC 793.
-   Cette commande est disponible uniquement si le protocole TCP/IP (Internet Protocol) est installé en tant que composant dans les propriétés d’une carte réseau dans connexions réseau.

## <a name="BKMK_Examples"></a>Illustre
Pour afficher à la fois les statistiques Ethernet et les statistiques de tous les protocoles, tapez :
```
netstat -e -s
```
Pour afficher les statistiques uniquement pour les protocoles TCP et UDP, tapez :
```
netstat -s -p tcp udp
```
Pour afficher les connexions TCP actives et les ID de processus toutes les 5 secondes, tapez :
```
netstat -o 5
```
Pour afficher les connexions TCP actives et les ID de processus à l’aide d’une forme numérique, tapez :
```
netstat -n -o
```

## <a name="additional-references"></a>Références supplémentaires
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
