---
title: pathping
description: Découvrez comment obtenir la latence du réseau et les informations de perte à l’aide de la commande pathping.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ec430125-b1dc-4aad-a7c9-b70f486d9e3c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: db3a0f5cd3c711f7df0a13627969dc7b74b3d605
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837240"
---
# <a name="pathping"></a>pathping

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Fournit des informations sur la latence du réseau et de perte de connexion réseau à des tronçons intermédiaires entre une source et de destination. **PathPing** envoie des messages de demande echo plusieurs pour chaque routeur entre une source et de destination sur une période de temps et calcule ensuite les résultats en fonction des paquets renvoyés par chaque routeur. Étant donné que **pathping** affiche le degré de perte de paquets sur n’importe quel routeur ou un lien, vous pouvez déterminer quels routeurs ou sous-réseaux peut-être des problèmes de réseau. 

**PathPing** effectue l’équivalent de la **tracert** commande en identifiant les routeurs qui se trouvent sur le chemin d’accès. Ensuite, il envoie régulièrement des commandes ping à tous les routeurs sur une période de temps et calcule des statistiques basées sur le nombre retourné à partir de chacun. Utilisée sans paramètres, **pathping** affiche l’aide. 

## <a name="syntax"></a>Syntaxe
```
pathping [/n] [/h] [/g <Hostlist>] [/p <Period>] [/q <NumQueries> [/w <timeout>] [/i <IPaddress>] [/4 <IPv4>] [/6 <IPv6>][<TargetName>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/n|Empêche **pathping** de tenter de résoudre les adresses IP des routeurs intermédiaires à leurs noms. Cela peut accélérer l’affichage de **pathping** résultats.|
|/h \<MaximumHops>|Spécifie le nombre maximal de sauts dans le chemin d’accès pour rechercher la cible (destination). La valeur par défaut est 30 tronçons.|
|/g \<paramètre ListeHôtes >|Spécifie que l’utilisation de messages de demande de l’itinéraire Source libre de l’écho option dans l’en-tête IP avec l’ensemble de destinations intermédiaires spécifiées dans *paramètre ListeHôtes*. Avec le routage de source libre, les destinations intermédiaires successives peuvent être séparées par un ou plusieurs routeurs. Le nombre maximal d’adresses ou des noms dans la liste d’hôtes est 9. Le *paramètre ListeHôtes* est une série d’adresses IP (en notation décimale) séparée par des espaces.|
|/p \<période >|Spécifie le nombre de millisecondes à attendre entre les pings consécutifs. La valeur par défaut est 250 millisecondes (1/4 seconde).|
|/q \<NumQueries>|Spécifie le nombre d’écho demander les messages envoyés à chaque routeur situé dans le chemin d’accès. La valeur par défaut est 100 requêtes.|
|/w \<délai d’attente >|Spécifie le nombre de millisecondes à attendre pour chaque réponse. La valeur par défaut est 3 000 millisecondes (3 secondes).|
|/i \<IPaddress>|Spécifie l’adresse source.|
|/4 \<IPv4>|Indique que pathping utilise IPv4 uniquement.|
|/6 \<IPv6>|Indique que pathping utilise IPv6 uniquement.|
|\<TargetName>|Spécifie la destination, qui est identifié par nom d’hôte ou adresse IP.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
-   **PathPing** paramètres respectent la casse.
-   Pour éviter la congestion du réseau, les tests ping doit être envoyées à un rythme assez lent.
-   Pour minimiser les effets des pertes de rafale, n’envoyez pas pings trop fréquemment.
-   Lorsque vous utilisez le **/p** paramètre, commandes ping est envoyées individuellement à chaque tronçon intermédiaire. Pour cette raison, l’intervalle entre deux commandes ping envoyées au même tronçon est *période* multiplié par le nombre de sauts.
-   Lorsque vous utilisez le **/w** paramètre, plusieurs commandes ping peut être envoyées en parallèle. Pour cette raison, la quantité de temps spécifiée dans le *délai d’attente* paramètre n’est pas limité par la quantité de temps spécifiée dans le *période* paramètre de délai d’attente entre les tests ping.
-   Cette commande est disponible uniquement si le protocole TCP/IP (Internet Protocol) est installé en tant que composant dans les propriétés d’une carte réseau dans Connexions réseau.

## <a name="BKMK_Examples"></a>Exemples

L’exemple suivant **pathping** sortie de la commande :

```
D:\>pathping /n corp1
Tracing route to corp1 [10.54.1.196]
over a maximum of 30 hops:
  0  172.16.87.35
  1  172.16.87.218
  2  192.168.52.1
  3  192.168.80.1
  4  10.54.247.14
  5  10.54.1.196
computing statistics for 125 seconds...
            Source to Here   This Node/Link
Hop  RTT    Lost/Sent = Pct  Lost/Sent = Pct  address
  0                                           172.16.87.35
                                0/ 100 =  0%   |
  1   41ms     0/ 100 =  0%     0/ 100 =  0%  172.16.87.218
                               13/ 100 = 13%   |
  2   22ms    16/ 100 = 16%     3/ 100 =  3%  192.168.52.1
                                0/ 100 =  0%   |
  3   24ms    13/ 100 = 13%     0/ 100 =  0%  192.168.80.1
                                0/ 100 =  0%   |
  4   21ms    14/ 100 = 14%     1/ 100 =  1%  10.54.247.14
                                0/ 100 =  0%   |
  5   24ms    13/ 100 = 13%     0/ 100 =  0%  10.54.1.196
Trace complete.
```
Lorsque **pathping** est exécuté, la première liste de résultats le chemin d’accès. Il s’agit du même chemin d’accès qui est repris en utilisant le **tracert** commande. Ensuite, un message d’état occupé s’affiche pendant environ 90 secondes (la durée varie par nombre de sauts). Pendant ce temps, les informations sont collectées à partir de tous les routeurs répertoriés précédemment et des liens entre eux. à la fin de cette période, les résultats des tests sont affichés.

Dans l’exemple de rapport ci-dessus, le **ce nœud/lien**, **perdus ou Sent = Pct** et **adresse** colonnes montrent que la liaison entre 172.16.87.218 et 192.168.52.1 perd 13 pourcentage des paquets. Les routeurs au niveau des tronçons 2 et 4 sont perdent également des paquets qui leur sont adressés, mais cette perte n’affecte pas leur capacité à transférer le trafic qui n’est pas adressé à eux.

Les taux de perte affichés pour les liaisons, identifiés comme une barre verticale (**|**) dans le **adresse** colonne, indiquer une congestion du lien qui provoque la perte de paquets sont transférés sur le chemin d’accès. Les taux de perte affichés pour les routeurs (identifiés par leur adresse IP) indiquent que ces routeurs est peut-être surchargés.

## <a name="additional-references"></a>Références supplémentaires
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)