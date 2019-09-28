---
title: pathping
description: Découvrez comment obtenir des informations sur la latence et la perte du réseau à l’aide de la commande pathping.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 3232aaac979aa4e410d31db810abdd940d1c24bf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372401"
---
# <a name="pathping"></a>pathping

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Fournit des informations sur la latence du réseau et la perte réseau au niveau des sauts intermédiaires entre une source et une destination. **pathping** envoie plusieurs messages de demande d’écho à chaque routeur entre une source et une destination sur une période donnée, puis calcule les résultats en fonction des paquets renvoyés par chaque routeur. Comme **pathping** affiche le degré de perte de paquets au niveau d’un routeur ou d’un lien donné, vous pouvez identifier les routeurs ou sous-réseaux qui rencontrent des problèmes réseau. 

**pathping** exécute l’équivalent de la commande **tracert** en identifiant les routeurs qui se trouvent sur le chemin d’accès. Il envoie ensuite régulièrement des pings à tous les routeurs pendant une période donnée et calcule les statistiques en fonction du nombre retourné par chaque. Utilisé sans paramètres, **pathping** affiche l’aide. 

## <a name="syntax"></a>Syntaxe
```
pathping [/n] [/h] [/g <Hostlist>] [/p <Period>] [/q <NumQueries> [/w <timeout>] [/i <IPaddress>] [/4 <IPv4>] [/6 <IPv6>][<TargetName>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/n|Empêche **pathping** de tenter de résoudre les adresses IP des routeurs intermédiaires en leurs noms. Cela peut accélérer l’affichage des résultats de **pathping** .|
|/h @no__t 0MaximumHops >|Spécifie le nombre maximal de sauts dans le chemin d’accès pour rechercher la cible (destination). La valeur par défaut est 30 tronçons.|
|/g @no__t 0Hostlist >|Spécifie que les messages de demande d’écho utilisent l’option de route de source libre dans l’en-tête IP avec l’ensemble de destinations intermédiaires spécifié dans *hostlist*. Avec un routage source libre, les destinations intermédiaires successives peuvent être séparées par un ou plusieurs routeurs. Le nombre maximal d’adresses ou de noms dans la liste d’ordinateurs hôtes est de 9. Le *hostlist* est une série d’adresses IP (en notation décimale séparée par des points), séparées par des espaces.|
|/p @no__t 0Period >|Spécifie le nombre de millisecondes d’attente entre les pings successifs. La valeur par défaut est 250 millisecondes (1/4 seconde).|
|/q \<NumQueries >|Spécifie le nombre de messages de demande d’écho envoyés à chaque routeur dans le chemin d’accès. La valeur par défaut est 100 requêtes.|
|/w @no__t 0timeout >|Spécifie le nombre de millisecondes à attendre pour chaque réponse. La valeur par défaut est 3000 millisecondes (3 secondes).|
|/i \<IPaddress >|Spécifie l’adresse source.|
|/4 @no__t 0IPv4 >|Spécifie que pathping utilise uniquement IPv4.|
|/6 @no__t 0IPv6 >|Spécifie que pathping utilise uniquement IPv6.|
|@no__t 0TargetName >|Spécifie la destination, qui est identifiée par une adresse IP ou un nom d’hôte.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
-   les paramètres **pathping** respectent la casse.
-   Pour éviter toute congestion du réseau, les pings doivent être envoyés à un rythme suffisamment lent.
-   Pour réduire les effets des pertes de rafales, n’envoyez pas de pings trop fréquemment.
-   Lors de l’utilisation du paramètre **/p** , les pings sont envoyés individuellement à chaque tronçon intermédiaire. Pour cette raison, l’intervalle entre deux pings envoyés au même tronçon est la *période* multiplié par le nombre de tronçons.
-   Quand vous utilisez le paramètre **/w** , plusieurs pings peuvent être envoyés en parallèle. Pour cette raison, la durée spécifiée dans le paramètre de *délai d’attente* n’est pas limitée par la durée spécifiée dans le paramètre *period* pour l’attente entre les pings.
-   Cette commande est disponible uniquement si le protocole TCP/IP (Internet Protocol) est installé en tant que composant dans les propriétés d’une carte réseau dans connexions réseau.

## <a name="BKMK_Examples"></a>Illustre

L’exemple suivant illustre la sortie de la commande **pathping** :

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
Lorsque **pathping** est exécuté, les premiers résultats répertorient le chemin d’accès. Il s’agit du même chemin d’accès que celui qui est affiché à l’aide de la commande **tracert** . Ensuite, un message occupé s’affiche pendant environ 90 secondes (le temps varie selon le nombre de tronçons). Pendant ce temps, les informations sont collectées à partir de tous les routeurs précédemment listés et des liens entre eux. à la fin de cette période, les résultats des tests sont affichés.

Dans l’exemple de rapport ci-dessus, les colonnes **nœud/lien**, **perdu/envoyé = PCT** et **adresse** indiquent que le lien entre 172.16.87.218 et 192.168.52.1 abandonne 13% des paquets. Les routeurs situés aux tronçons 2 et 4 suppriment également les paquets qui leur sont adressés, mais cette perte n’affecte pas leur capacité à transférer le trafic qui ne leur est pas adressé.

Les taux de perte affichés pour les liens, identifiés sous la forme d’une barre verticale ( **|** ) dans la colonne **adresse** , indiquent la congestion de lien qui provoque la perte de paquets transférés sur le chemin d’accès. Les taux de perte affichés pour les routeurs (identifiés par leurs adresses IP) indiquent que ces routeurs peuvent être surchargés.

## <a name="additional-references"></a>Références supplémentaires
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)