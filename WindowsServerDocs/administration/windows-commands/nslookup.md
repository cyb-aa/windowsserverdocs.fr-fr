---
title: nslookup
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 41516932-7833-434a-aa92-b4cf0f9a7ef7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 84e3e9ee920f458ca775dd7b76d892f10ba2f992
ms.sourcegitcommit: ee8e0b217be6f6b2532ee7265fb4be00c106e124
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70878116"
---
# <a name="nslookup"></a>nslookup

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Affiche des informations que vous pouvez utiliser pour diagnostiquer l’infrastructure DNS (Domain Name System). Avant d’utiliser cet outil, vous devez vous familiariser avec le fonctionnement du service DNS. L’outil de ligne de commande nslookup est disponible uniquement si vous avez installé le protocole TCP/IP.
## <a name="syntax"></a>Syntaxe

```
nslookup [<-SubCommand ...>] [{<computerTofind> | -<Server>}]
nslookup /exit
nslookup /finger [<UserName>] [{[>] <FileName>|[>>] <FileName>}]
nslookup /{help | ?}
nslookup /ls [<Option>] <DNSDomain> [{[>] <FileName>|[>>] <FileName>}]
nslookup /lserver <DNSDomain> 
nslookup /root 
nslookup /server <DNSDomain>
nslookup /set <KeyWord>[=<Value>]
nslookup /set all 
nslookup /set class=<Class>
nslookup /set [no]d2
nslookup /set [no]debug
nslookup /set [no]defname
nslookup /set domain=<DomainName>
nslookup /set [no]ignore
nslookup /set port=<Port>
nslookup /set querytype=<ResourceRecordtype>
nslookup /set [no]recurse
nslookup /set retry=<Number>
nslookup /set root=<RootServer>
nslookup /set [no]search
nslookup /set srchlist=<DomainName>[/...]
nslookup /set timeout=<Number>
nslookup /set type=<ResourceRecordtype>
nslookup /set [no]vc
nslookup /view <FileName>
```

## <a name="parameters"></a>Paramètres

|                       Paramètre                       |                                                                                                         Description                                                                                                         |
|-------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   [nslookup exit Command](nslookup-exit-command.md)   |                                                                                                     Quitte **nslookup**.                                                                                                     |
| [nslookup finger Command](nslookup-finger-command.md) |                                                                                  Établit une connexion avec le serveur Finger sur l’ordinateur actuel.                                                                                   |
|           [nslookup help](nslookup-help.md)           |                                                                                    Affiche un bref résumé des sous-commandes **nslookup** .                                                                                    |
|             [nslookup ls](nslookup-ls.md)             |                                                                                             Répertorie des informations pour un domaine DNS.                                                                                             |
|        [nslookup lserver](nslookup-lserver.md)        |                                                                                   Remplace le serveur par défaut par le domaine DNS spécifié.                                                                                   |
|           [nslookup root](nslookup-root.md)           |                                                                     Remplace le serveur par défaut par le serveur pour la racine de l’espace de noms de domaine DNS.                                                                     |
|         [nslookup server](nslookup-server.md)         |                                                                                   Remplace le serveur par défaut par le domaine DNS spécifié.                                                                                   |
|            [nslookup set](nslookup-set.md)            |                                                                              Modifie les paramètres de configuration qui affectent le fonctionnement des recherches.                                                                               |
|        [nslookup set all](nslookup-set-all.md)        |                                                                                  Imprime les valeurs actuelles des paramètres de configuration.                                                                                   |
|      [nslookup set class](nslookup-set-class.md)      |                                                                     Modifie la classe de requête. La classe spécifie le groupe de protocoles des informations.                                                                     |
|         [nslookup set d2](nslookup-set-d2.md)         |                                                                     Active ou désactive le mode de débogage exhaustif. Tous les champs de chaque paquet sont imprimés.                                                                      |
|      [nslookup set debug](nslookup-set-debug.md)      |                                                                                               Active ou désactive le mode de débogage.                                                                                               |
|                 nslookup/Set defname                 |                                            Ajoute le nom de domaine DNS par défaut à une demande de recherche de composant unique. Un composant unique est un composant qui ne contient aucune période.                                            |
|     [nslookup set domain](nslookup-set-domain.md)     |                                                                                 Remplace le nom de domaine DNS par défaut par le nom spécifié.                                                                                  |
|                 nslookup/Set ignore                  |                                                                                              Ignore les erreurs de troncation de paquets.                                                                                              |
|       [nslookup set port](nslookup-set-port.md)       |                                                                          Remplace le port du serveur de noms DNS TCP/UDP par défaut par la valeur spécifiée.                                                                           |
|  [nslookup set querytype](nslookup-set-querytype.md)  |                                                                                       Modifie le type d’enregistrement de ressource pour la requête.                                                                                       |
|    [nslookup set recurse](nslookup-set-recurse.md)    |                                                                    Indique au serveur de noms DNS d’interroger d’autres serveurs s’il ne contient pas les informations.                                                                    |
|      [nslookup set retry](nslookup-set-retry.md)      |                                                                                                 Définit le nombre de nouvelles tentatives.                                                                                                 |
|       [nslookup set root](nslookup-set-root.md)       |                                                                                    Modifie le nom du serveur racine utilisé pour les requêtes.                                                                                    |
|     [nslookup set search](nslookup-set-search.md)     | Ajoute les noms de domaine DNS dans la liste de recherche de domaine DNS à la demande jusqu’à ce qu’une réponse soit reçue. Cela s’applique lorsque le jeu et la demande de recherche contiennent au moins un point, mais ne se terminent pas par un point final. |
|   [nslookup set srchlist](nslookup-set-srchlist.md)   |                                                                                    Modifie le nom de domaine DNS par défaut et la liste de recherche.                                                                                     |
|    [nslookup set timeout](nslookup-set-timeout.md)    |                                                                           Modifie le nombre initial de secondes d’attente d’une réponse à une demande.                                                                           |
|       [nslookup set type](nslookup-set-type.md)       |                                                                                       Modifie le type d’enregistrement de ressource pour la requête.                                                                                       |
|         [nslookup set vc](nslookup-set-vc.md)         |                                                                     Spécifie d’utiliser ou non un circuit virtuel lors de l’envoi de requêtes au serveur.                                                                      |
|           [nslookup view](nslookup-view.md)           |                                                                          Trie et répertorie la sortie de la sous-commande ou des commandes **ls** précédente.                                                                          |

## <a name="remarks"></a>Notes
- Si *computerTofind* est une adresse IP et que la requête est pour un type d’enregistrement de ressource a ou PTR, le nom de l’ordinateur est retourné. Si *computerTofind* est un nom et n’a pas de période de fin, le nom de domaine DNS par défaut est ajouté au nom. Ce comportement dépend de l’état des sous- **commandes Set** suivantes : **Domain**, **srchlist**, **defname**et **Search**.
- Si vous tapez un trait d’Union (-) à la place de *computerTofind*, l’invite de commandes passe en mode interactif **nslookup** .
- La longueur de la ligne de commande doit être inférieure à 256 caractères.
- **nslookup** a deux modes : interactif et non interactif.
  Si vous n’avez besoin de rechercher qu’un seul élément de données, utilisez le mode non interactif. Pour le premier paramètre, tapez le nom ou l’adresse IP de l’ordinateur que vous souhaitez rechercher. Pour le deuxième paramètre, tapez le nom ou l’adresse IP d’un serveur de noms DNS. Si vous omettez le deuxième argument, **nslookup** utilise le serveur de noms DNS par défaut.
  Si vous avez besoin de Rechercher plusieurs éléments de données, vous pouvez utiliser le mode interactif. tapez un trait d’Union (-) pour le premier paramètre et le nom ou l’adresse IP d’un serveur de noms DNS pour le deuxième paramètre. Ou omettez les deux paramètres et **nslookup** utilise le serveur de noms DNS par défaut. Voici quelques conseils sur l’utilisation du mode interactif :
  -   Pour interrompre les commandes interactives à tout moment, appuyez sur CTRL + B.
  -   Pour quitter, tapez **Exit**.
  -   Pour traiter une commande intégrée comme un nom d’ordinateur, faites-le précéder du caractère d'\\échappement ().
  -   Une commande non reconnue est interprétée comme un nom d’ordinateur.
- Si la demande de recherche échoue, **nslookup** imprime un message d’erreur. Le tableau suivant répertorie les messages d’erreur possibles.
  |**Message d’erreur**|**Description**|
  |-----------|----------|
  |`timed out`|Le serveur n’a pas répondu à une demande après un certain laps de temps et un certain nombre de tentatives. Vous pouvez définir le délai d’attente à l’aide de la sous-commande **Set Timeout** . Vous pouvez définir le nombre de nouvelles tentatives avec la sous-commande **Set Retry** .|
  |`No response from server`|Aucun serveur de noms DNS n’est en cours d’exécution sur l’ordinateur serveur.|
  |`No records`|Le serveur de noms DNS n’a pas d’enregistrements de ressource du type de requête actuel pour l’ordinateur, bien que le nom de l’ordinateur soit valide. Le type de requête est spécifié à l’aide de la commande **Set QueryType** .|
  |`Nonexistent domain`|L’ordinateur ou le nom de domaine DNS n’existe pas.|
  |`Connection refused`<br /><br />ou<br /><br />`Network is unreachable`|La connexion au serveur de noms DNS ou au serveur Finger n’a pas pu être établie. Cette erreur se produit généralement avec les requêtes **ls** et **Finger** .|
  |`Server failure`|Le serveur de noms DNS a trouvé une incohérence interne dans sa base de données et n’a pas pu retourner une réponse valide.|
  |`Refused`|Le serveur de noms DNS a refusé de traiter la demande.|
  |`format error`|Le serveur de noms DNS a détecté que le format du paquet de demande n’était pas correct. Elle peut indiquer une erreur dans **nslookup**.|
- Pour plus d’informations sur la commande **nslookup** et DNS, consultez les ressources suivantes :
  - Lee, T., Davies, J. 2000. Informations *techniques de référence sur les protocoles et services TCP/IP de Microsoft Windows 2000*. Redmond, Washington : Microsoft Press.
  - Albitz, P., Loukides, M. et C. Liu. 2001. *DNS et bind, quatrième édition*. Sebastopol, Californie : O’Reilly et Associates, Inc.
  - Larson, M. et C. Liu. 2001. *DNS sur Windows 2000*. Sebastopol, Californie : O’Reilly et Associates, Inc.
    #### <a name="examples"></a>Exemples
    Chaque option de ligne de commande se compose d’un trait d’Union (-) suivi immédiatement du nom de commande et, dans certains cas, d’un signe égal (=), puis d’une valeur. Par exemple, pour modifier le type de requête par défaut en informations sur l’hôte (ordinateur) et le délai d’expiration initial à 10 secondes, tapez : **nslookup-QueryType = HINFO-Timeout = 10**
    ## <a name="see-also"></a>Voir aussi
    [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
