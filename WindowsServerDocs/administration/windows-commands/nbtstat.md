---
title: nbtstat
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0193152674dc934aa4f2d3be4dec54afc3066951
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867700"
---
# <a name="nbtstat"></a>nbtstat

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cache de noms NetBIOS affiche les tables de nom NetBIOS pour l’ordinateur local et les ordinateurs distants sur les statistiques du protocole TCP/IP (NetBT) et NetBIOS. **nbtstat** permet une actualisation du cache de noms NetBIOS et les noms enregistrés avec les services Internet (WINS, Windows Internet Name Service). Utilisée sans paramètres, **nbtstat** affiche l’aide. 

## <a name="syntax"></a>Syntaxe

```
nbtstat [/a <remoteName>] [/A <IPaddress>] [/c] [/n] [/r] [/R] [/RR] [/s] [/S] [<Interval>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|/a <remoteName>|Affiche la table de noms NetBIOS d’un ordinateur distant, où *remoteName* est le nom NetBIOS de l’ordinateur distant. La table de noms NetBIOS est la liste des noms NetBIOS correspondant aux applications NetBIOS en cours d’exécution sur cet ordinateur.|
|/A <IPaddress>|Affiche la table de noms NetBIOS d’un ordinateur distant, spécifié par l’adresse IP (en notation décimale) de l’ordinateur distant.|
|/c|Affiche le contenu de NetBIOS nom du cache, la table de noms NetBIOS et les adresses IP correspondantes.|
|/n|Affiche la table de noms NetBIOS de l’ordinateur local. L’état de **inscrit** indique que le nom est enregistré par diffusion ou avec un serveur WINS.|
|/r|Affiche les statistiques de résolution de nom NetBIOS. Sur un ordinateur exécutant Windows XP ou Windows Server 2003 qui est configuré pour utiliser WINS, ce paramètre retourne le nombre de noms qui ont été résolus et inscrits par diffusion et WINS.|
|/R|Purge le contenu du cache de noms NetBIOS et le recharge le #tagged entrées à partir de la **Lmhosts** fichier.|
|/RR|Libère, puis actualise les noms NetBIOS pour l’ordinateur local qui est inscrit auprès des serveurs WINS.|
|/s|Affiche les sessions NetBIOS client et serveur, essayez de convertir l’adresse IP de destination à un nom.|
|/S|Affiche les sessions NetBIOS client et serveur, répertoriant les ordinateurs distants par adresse IP de destination uniquement.|
|<Interval>|Actualise les statistiques sélectionnées, suspension le nombre de secondes spécifié dans *intervalle* entre chaque affichage. Appuyez sur CTRL + C pour arrêter l’affichage des statistiques. Si ce paramètre est omis, **nbtstat** imprime les informations de configuration une seule fois.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes

-   **nbtstat** des paramètres de ligne de commande respectent la casse.

-   Le tableau suivant décrit les en-têtes de colonnes qui sont générés par **nbtstat**:

    |Orientation|Description|
    |------|--------|
    |Entrée|Le nombre d’octets reçus.|
    |Sortie|Le nombre d’octets envoyés.|
    |Entrée/Sortie|Si la connexion est à partir de l’ordinateur (sortant) ou d’un autre ordinateur pour l’ordinateur local (entrant).|
    |Durée de vie|Le temps restant une entrée de cache de table de nom se trouvera avant, il est purgé.|
    |Nom local|Le nom NetBIOS local associé à la connexion.|
    |Hôte distant|Nom ou adresse IP associée à l’ordinateur distant.|
    |<03>|Le dernier octet d’un nom NetBIOS convertis au format hexadécimal. Chaque nom NetBIOS est de 16 caractères. Ce dernier octet a souvent une importance spéciale car le même nom peut être présent plusieurs fois sur un ordinateur, qui se différencie uniquement par le dernier octet. Par exemple, < 20 > est un espace en texte ASCII.|
    |type|Le type de nom. Un nom peut être un nom unique ou un nom de groupe.|
    |État|Si le service NetBIOS sur l’ordinateur distant est en cours d’exécution (inscrit) ou un nom d’ordinateur dupliqué a inscrit le même service (conflit).|
    |État|L’état des connexions NetBIOS.|

-   Le tableau suivant décrit les États de connexion NetBIOS possibles :

    |État|Description|
    |-----|--------|
    |Connecté|Une session a été établie.|
    |Associés|Un point de terminaison de connexion a été créé et associé à une adresse IP.|
    |à l’écoute|Ce point de terminaison est disponible pour une connexion entrante.|
    |Idle|Ce point de terminaison a été ouvert, mais ne peut pas recevoir des connexions.|
    |Connexion|Une session est en phase de connexion et le mappage de nom de l’adresse de la destination est résolu.|
    |Accepter|Une session entrante est en cours de validation et est connectée sous peu.|
    |La reconnexion|Une session tente de se reconnecter (il n’a pas pu se connecter à la première tentative).|
    |Sortant|Une session est en phase de connexion et la connexion TCP est en cours de création.|
    |Entrant|Une session entrante est en phase de connexion.|
    |Déconnexion|Une session est en cours de déconnexion.|
    |Disconnected|L’ordinateur local a émis une déconnexion et il est en attente de confirmation du système distant.|

-   Cette commande est disponible uniquement si le protocole TCP/IP (Internet Protocol) est installé en tant que composant dans les propriétés d’une carte réseau dans Connexions réseau.

## <a name="BKMK_Examples"></a>Exemples
Pour afficher la table de noms NetBIOS de l’ordinateur distant avec le nom d’ordinateur NetBIOS CORP07, tapez :

```
nbtstat /a CORP07
```

Pour afficher la table de noms NetBIOS de l’ordinateur distant est affectée l’adresse IP 10.0.0.99, tapez :

```
nbtstat /A 10.0.0.99
```

Pour afficher la table de noms NetBIOS de l’ordinateur local, tapez :

```
nbtstat /n
```

Pour afficher le contenu de l’ordinateur local de cache de noms NetBIOS, tapez :

```
nbtstat /c
```

Pour vider le cache de noms NetBIOS et recharger le # préalable tagged entrées dans le fichier Lmhosts local, tapez :

```
nbtstat /R
```

Pour libérer les noms NetBIOS inscrits auprès du serveur WINS et les inscrire à nouveau, tapez :

```
nbtstat /RR
```

Pour afficher les statistiques de session NetBIOS en adresse IP toutes les cinq secondes, tapez :

```
nbtstat /S 5
```

## <a name="additional-references"></a>Références supplémentaires

-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)


