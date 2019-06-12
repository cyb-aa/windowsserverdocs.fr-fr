---
title: nslookup
description: 'Rubrique de commandes de Windows pour ***- '
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
ms.openlocfilehash: 194cb96846e42b175978a2f6fc7268a93875d315
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811094"
---
# <a name="nslookup"></a>nslookup

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Affiche des informations que vous pouvez utiliser pour diagnostiquer l’infrastructure du système DNS (Domain Name). Avant d’utiliser cet outil, vous devez connaître le fonctionnement du DNS. L’outil de ligne de commande nslookup est disponible uniquement si vous avez installé le protocole TCP/IP.
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
| [nslookup finger Command](nslookup-finger-command.md) |                                                                                  Se connecte avec le serveur du doigt sur l’ordinateur actuel.                                                                                   |
|           [nslookup help](nslookup-help.md)           |                                                                                    Affiche un résumé de **nslookup** sous-commandes.                                                                                    |
|             [nslookup ls](nslookup-ls.md)             |                                                                                             Répertorie des informations pour un domaine DNS.                                                                                             |
|        [nslookup lserver](nslookup-lserver.md)        |                                                                                   modifie le serveur par défaut pour le domaine DNS spécifié.                                                                                   |
|           [nslookup root](nslookup-root.md)           |                                                                     changements du serveur par défaut sur le serveur pour la racine de l’espace de noms de domaine DNS.                                                                     |
|         [nslookup server](nslookup-server.md)         |                                                                                   modifie le serveur par défaut pour le domaine DNS spécifié.                                                                                   |
|            [nslookup set](nslookup-set.md)            |                                                                              Modifie les paramètres de configuration qui affectent la fonction de recherches.                                                                               |
|        [nslookup set all](nslookup-set-all.md)        |                                                                                  Imprime les valeurs actuelles des paramètres de configuration.                                                                                   |
|      [nslookup set class](nslookup-set-class.md)      |                                                                     Modifie la classe de requête. La classe spécifie le groupe de protocole des informations.                                                                     |
|         [nslookup set d2](nslookup-set-d2.md)         |                                                                     Active ou désactive la mode de débogage exhaustif. Tous les champs de chaque paquet sont imprimés.                                                                      |
|      [nslookup set debug](nslookup-set-debug.md)      |                                                                                               Active ou désactive la mode de débogage.                                                                                               |
|                 nslookup /set defname                 |                                            Ajoute le nom de domaine DNS par défaut à une demande de recherche de composant unique. Un composant unique est un composant contenant aucune période.                                            |
|     [nslookup set domain](nslookup-set-domain.md)     |                                                                                 modifie le nom de domaine DNS par défaut au nom spécifié.                                                                                  |
|                 nslookup /set ignore                  |                                                                                              Ignore les erreurs de troncation de paquet.                                                                                              |
|       [nslookup set port](nslookup-set-port.md)       |                                                                          modifie le port de serveur de nom DNS de TCP/UDP par défaut à la valeur spécifiée.                                                                           |
|  [nslookup set querytype](nslookup-set-querytype.md)  |                                                                                       modifie le type d’enregistrement de ressource pour la requête.                                                                                       |
|    [nslookup set recurse](nslookup-set-recurse.md)    |                                                                    Indique au serveur de nom DNS à interroger d’autres serveurs s’il ne possède pas les informations.                                                                    |
|      [nslookup set retry](nslookup-set-retry.md)      |                                                                                                 Définit le nombre de tentatives.                                                                                                 |
|       [nslookup set root](nslookup-set-root.md)       |                                                                                    modifie le nom du serveur racine utilisé pour les requêtes.                                                                                    |
|     [nslookup set search](nslookup-set-search.md)     | Ajoute les noms de domaine DNS dans la liste de recherche du domaine DNS à la demande jusqu'à ce qu’une réponse est reçue. Cela s’applique lorsque le jeu et la recherche demandent contenir au moins un point, mais ne se terminent pas par un point final. |
|   [nslookup set srchlist](nslookup-set-srchlist.md)   |                                                                                    Modifie la liste recherche et le nom des domaines DNS par défaut.                                                                                     |
|    [nslookup set timeout](nslookup-set-timeout.md)    |                                                                           modifie le nombre initial de secondes à attendre une réponse à une demande.                                                                           |
|       [nslookup set type](nslookup-set-type.md)       |                                                                                       modifie le type d’enregistrement de ressource pour la requête.                                                                                       |
|         [nslookup set vc](nslookup-set-vc.md)         |                                                                     Spécifie d’utiliser ou n’utilisez pas un circuit virtuel lors de l’envoi des demandes vers le serveur.                                                                      |
|           [nslookup view](nslookup-view.md)           |                                                                          trie et affiche la sortie de la précédente **ls** sous-commande ou des commandes.                                                                          |

## <a name="remarks"></a>Notes
- Si *paramètre Ordinateur_à_rechercher* est une adresse IP et de la requête est pour un A ou de type liée à un enregistrement de ressource PTR, le nom de l’ordinateur est retournée. Si *paramètre Ordinateur_à_rechercher* est un nom et n’a pas de fin de période, la valeur par défaut de nom de domaine DNS est ajouté au nom. Ce comportement dépend de l’état des opérations suivantes **définir** sous-commandes : **domaine**, **srchlist**, **defname**, et  **recherche**.
- Si vous tapez un trait d’union (-) au lieu de *paramètre Ordinateur_à_rechercher*, l’invite de commandes change pour **nslookup** mode interactif.
- La longueur de ligne de commande doit être inférieure à 256 caractères.
- **nslookup** comporte deux modes : interactives et.
  Si vous avez besoin rechercher uniquement un seul élément de données, utilisez le mode non interactif. Pour le premier paramètre, tapez le nom ou l’adresse IP de l’ordinateur que vous souhaitez rechercher. Pour le deuxième paramètre, tapez le nom ou l’adresse IP d’un serveur de nom DNS. Si vous omettez le deuxième argument, **nslookup** utilise le serveur de noms DNS par défaut.
  Si vous avez besoin rechercher plus d’un élément de données, vous pouvez utiliser le mode interactif. Tapez un trait d’union (-) pour le premier paramètre et le nom ou l’adresse IP d’un serveur de nom DNS pour le deuxième paramètre. Ou, omettez les deux paramètres et **nslookup** utilise le serveur de noms DNS par défaut. Voici quelques conseils sur l’utilisation en mode interactif :
  -   Pour interrompre des commandes interactives à tout moment, appuyez sur CTRL + B.
  -   Pour quitter, tapez **quitter**.
  -   Pour traiter une commande intégrée comme un nom d’ordinateur, faites-le précéder du caractère d’échappement (\\).
  -   Une commande non reconnue est interprétée comme un nom d’ordinateur.
- Si la demande de recherche échoue, **nslookup** imprime un message d’erreur. Le tableau suivant répertorie les messages d’erreur possibles.
  |**message d’erreur**|**Description**|
  |-----------|----------|
  |`timed out`|Le serveur n’a pas répondu à une demande après un certain laps de temps et un certain nombre de nouvelles tentatives. Vous pouvez définir le délai d’attente avec le **définir le délai d’attente** sous-commande. Vous pouvez définir le nombre de nouvelles tentatives avec le **nouvelle tentative ensemble** sous-commande.|
  |`No response from server`|Aucun serveur de nom DNS n’est en cours d’exécution sur l’ordinateur serveur.|
  |`No records`|Le serveur de noms DNS n’a pas d’enregistrements de ressource du type de requête actuel pour l’ordinateur, bien que le nom d’ordinateur est valide. Le type de requête est spécifié avec la **définir querytype** commande.|
  |`Nonexistent domain`|L’ordinateur ou le nom de domaine DNS n’existe pas.|
  |`Connection refused`<br /><br />- ou -<br /><br />`Network is unreachable`|La connexion pour le serveur de nom DNS ou un doigt n’a pas pu être établie. Cette erreur se produit couramment avec **ls** et **doigt** demandes.|
  |`Server failure`|Le serveur de noms DNS a trouvé une incohérence dans sa base de données et ne peut pas renvoyer une réponse valide.|
  |`Refused`|Le serveur de noms DNS a refusé de traiter la demande.|
  |`format error`|Le serveur de noms DNS trouvé que le paquet de demande n’était pas dans le format approprié. Cela peut indiquer une erreur dans **nslookup**.|
- Pour plus d’informations sur la **nslookup** commande et DNS, consultez les ressources suivantes :
  - Lee, T., Davies, J. 2000. *Référence technique de Microsoft Windows 2000 TCP/IP Protocols and Services*. Redmond, Washington : Microsoft Press.
  - Albitz, M. de P., Loukides et C. Liu. 2001. *DNS et BIND, quatrième édition*. Sebastopol, Californie : O ' Reilly and associates, Inc.
  - Larson, M. et C. Liu. 2001. *DNS sur Windows 2000*. Sebastopol, Californie : O ' Reilly and associates, Inc.
    #### <a name="examples"></a>Exemples
    Chaque option de ligne de commande se compose d’un trait d’union (-) immédiatement suivi par le nom de commande et, dans certains cas, un signe égal (=) et une valeur. Par exemple, pour modifier le type de requête par défaut pour les informations sur l’hôte (ordinateur) et le délai d’attente initiale de 10 secondes, tapez : **nslookup - querytype = hinfo - timeout = 10**
    ## <a name="see-also"></a>Voir aussi
    [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
