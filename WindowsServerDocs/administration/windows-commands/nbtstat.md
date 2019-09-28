---
title: nbtstat
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1d2ea99e-72f1-471f-9525-d2c49bf3be82
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd9148c922ac97e83b3b21bcb8b6585775505bf2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373317"
---
# <a name="nbtstat"></a>nbtstat

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Affiche les statistiques de protocole NetBIOS sur TCP/IP (NetBT), les tables de noms NetBIOS pour l’ordinateur local et les ordinateurs distants, ainsi que le cache de noms NetBIOS. **nbtstat** permet d’actualiser le cache de noms NetBIOS et les noms inscrits auprès du service WINS (Windows Internet Name Service). Utilisé sans paramètres, **nbtstat** affiche l’aide. 

## <a name="syntax"></a>Syntaxe

```
nbtstat [/a <remoteName>] [/A <IPaddress>] [/c] [/n] [/r] [/R] [/RR] [/s] [/S] [<Interval>]
```

### <a name="parameters"></a>Paramètres

|    Paramètre    |                                                                                                                         Description                                                                                                                         |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /a <remoteName> |    Affiche la table de noms NetBIOS d’un ordinateur distant, où *nom_distant* est le nom NetBIOS de l’ordinateur distant. La table de noms NetBIOS est la liste des noms NetBIOS qui correspond aux applications NetBIOS s’exécutant sur cet ordinateur.     |
| /A <IPaddress>  |                                                           Affiche la table de noms NetBIOS d’un ordinateur distant, spécifiée par l’adresse IP (en notation décimale séparée par des points) de l’ordinateur distant.                                                            |
|       /c        |                                                                        Affiche le contenu du cache de noms NetBIOS, la table des noms NetBIOS et leurs adresses IP résolues.                                                                         |
|       /n        |                                            Affiche la table de noms NetBIOS de l’ordinateur local. L’état **inscrit** indique que le nom est inscrit par diffusion ou avec un serveur WINS.                                             |
|       /r        |      Affiche les statistiques de résolution de noms NetBIOS. Sur un ordinateur exécutant Windows XP ou Windows Server 2003 configuré pour utiliser WINS, ce paramètre retourne le nombre de noms qui ont été résolus et inscrits à l’aide de la diffusion et du service WINS.       |
|       /R        |                                                                      Vide le contenu du cache de noms NetBIOS, puis recharge les entrées avec balises #PRE à partir du fichier **Lmhosts** .                                                                      |
|       /RR       |                                                                           Libère, puis actualise les noms NetBIOS de l’ordinateur local qui est inscrit auprès des serveurs WINS.                                                                            |
|       /s        |                                                                          Affiche les sessions clientes et serveur NetBIOS, en tentant de convertir l’adresse IP de destination en un nom.                                                                           |
|       COMMUTATEUR        |                                                                          Affiche les sessions client et serveur NetBIOS, en répertoriant les ordinateurs distants par adresse IP de destination uniquement.                                                                          |
|   <Interval>    | Réaffiche les statistiques sélectionnées, en interrompant le nombre de secondes spécifié dans *intervalle* entre chaque affichage. Appuyez sur CTRL + C pour arrêter l’affichage des statistiques. Si ce paramètre est omis, **nbtstat** n’imprime les informations de configuration actuelles qu’une seule fois. |
|       /?        |                                                                                                            Affiche l'aide à l'invite de commandes.                                                                                                             |

## <a name="remarks"></a>Notes

-   les paramètres de ligne de commande **nbtstat** respectent la casse.

-   Le tableau suivant décrit les en-têtes de colonnes générés par **nbtstat**:

    |Orientation|Description|
    |------|--------|
    |Entrée|Nombre d’octets reçus.|
    |Sortie|Nombre d’octets envoyés.|
    |Entrée/Sortie|Indique si la connexion provient de l’ordinateur (sortant) ou d’un autre ordinateur vers l’ordinateur local (entrant).|
    |Assurance|Durée restante pendant laquelle une entrée de cache de table de noms est conservée avant d’être purgée.|
    |Nom local|Nom NetBIOS local associé à la connexion.|
    |Hôte distant|Nom ou adresse IP associé à l’ordinateur distant.|
    |< 03 >|Dernier octet d’un nom NetBIOS converti en hexadécimal. Chaque nom NetBIOS a une longueur de 16 caractères. Ce dernier octet a souvent une importance particulière, car le même nom peut être présent plusieurs fois sur un ordinateur, ce qui diffère uniquement par le dernier octet. Par exemple, < 20 > est un espace en texte ASCII.|
    |type|Type de nom. Un nom peut être un nom unique ou un nom de groupe.|
    |Statut|Si le service NetBIOS sur l’ordinateur distant est en cours d’exécution (inscrit) ou si un nom d’ordinateur dupliqué a inscrit le même service (conflit).|
    |État|État des connexions NetBIOS.|

-   Le tableau suivant décrit les États de connexion NetBIOS possibles :

    |État|Description|
    |-----|--------|
    |Connecté|Une session a été établie.|
    |associé à|Un point de terminaison de connexion a été créé et associé à une adresse IP.|
    |Listen|Ce point de terminaison est disponible pour une connexion entrante.|
    |Idle|Ce point de terminaison a été ouvert, mais ne peut pas recevoir de connexions.|
    |Correspondance|Une session se trouve dans la phase de connexion et le mappage de nom à adresse IP de la destination est en cours de résolution.|
    |Refuser|Une session entrante est actuellement acceptée et sera connectée bientôt.|
    |La reconnexion|Une session tente de se reconnecter (elle n’a pas réussi à se connecter à la première tentative).|
    |Sortant|Une session se trouve dans la phase de connexion et la connexion TCP est en cours de création.|
    |Entrant|Une session entrante est dans la phase de connexion.|
    |Déconnexion|Une session est en cours de déconnexion.|
    |Disconnected|L’ordinateur local a émis une déconnexion et attend la confirmation du système distant.|

-   Cette commande est disponible uniquement si le protocole TCP/IP (Internet Protocol) est installé en tant que composant dans les propriétés d’une carte réseau dans connexions réseau.

## <a name="BKMK_Examples"></a>Illustre
Pour afficher la table de noms NetBIOS de l’ordinateur distant avec le nom d’ordinateur NetBIOS CORP07, tapez :

```
nbtstat /a CORP07
```

Pour afficher la table de noms NetBIOS de l’ordinateur distant affecté à l’adresse IP de 10.0.0.99, tapez :

```
nbtstat /A 10.0.0.99
```

Pour afficher la table de noms NetBIOS de l’ordinateur local, tapez :

```
nbtstat /n
```

Pour afficher le contenu du cache de noms NetBIOS de l’ordinateur local, tapez :

```
nbtstat /c
```

Pour vider le cache de noms NetBIOS et recharger les entrées avec balises #PRE dans le fichier LMHOSTS local, tapez :

```
nbtstat /R
```

Pour libérer les noms NetBIOS inscrits auprès du serveur WINS et les ré-inscrire, tapez :

```
nbtstat /RR
```

Pour afficher les statistiques de session NetBIOS par adresse IP toutes les cinq secondes, tapez :

```
nbtstat /S 5
```

## <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)


