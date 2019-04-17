---
ms.assetid: 3acaa977-ed63-4e38-ac81-229908c47208
title: La gestion des Cookies du serveur LDAP
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 89369cb1e52a315520062ca5ecc96b66ac3e2bfc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="how-ldap-server-cookies-are-handled"></a>La gestion des Cookies du serveur LDAP

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Dans LDAP, certaines requêtes génèrent un résultat de grand valeur. Ces requêtes présentent des défis pour le serveur Windows.  
  
Collecte et la création de ces jeux de résultats volumineux sont un travail considérable. De nombreux attributs doivent être convertis d’une représentation interne pour la représentation de transfert LDAP. Pour beaucoup d’attributs, une conversion d’un format interne, souvent binaire, doit se produire dans un format UTF-8 textuel dans le cadre de la réponse LDAP.  
  
Un autre défi est que les jeux de résultats avec des dizaines de milliers d’objets deviennent encombrants, facilement plusieurs centaines-mégaoctets. Ils requièrent alors un virtuelle et l’espace d’adressage et également le transfert sur réseau présente des problèmes car l’effort entier est perdu lorsque la session TCP se décompose en transit.  
  
Ces capacités et les aspects logistiques ont conduit les développeurs Microsoft LDAP à créer une extension LDAP appelée «Requête paginée». Elle implémente un contrôle LDAP pour séparer une seule requête volumineuse en segments plus petits de jeux de résultats. Il est devenu une norme RFC [RFC 2696](http://www.ietf.org/rfc/rfc2696).  
  
## <a name="cookie-handling-on-client"></a>Gestion des cookies sur le Client  
La méthode de requête paginée utilise la taille de page définie par le client ou via un [stratégie LDAP](https://support.microsoft.com/kb/315071/en-us) («MaxPageSize»). Le client doit toujours activer la pagination en envoyant un contrôle LDAP.  

  
Lorsque vous travaillez sur une requête avec un grand nombre de résultats, à un moment donné, le nombre maximal d’objets autorisés est atteint. Le serveur LDAP empaquette le message de réponse et ajoute un cookie qui contient les informations nécessaires pour continuer ultérieurement la recherche.  
  
L’application cliente doit traiter le cookie comme un objet blob opaque. Il peut récupérer le nombre d’objets dans la réponse et que vous pouvez continuer la recherche en fonction de la présence du cookie. Le client continue la recherche en envoyant la requête au serveur LDAP avec les mêmes paramètres tels que l’objet de base et le filtre et inclut la valeur du cookie qui a été retournée dans la réponse précédente.  
  
Si le nombre d’objets ne remplit pas une page, la requête LDAP est terminée et la réponse ne contient aucun cookie de page. Si aucun cookie n’est retourné par le serveur, le client doit considérer que la recherche paginée est terminée avec succès.  
  
Si une erreur est renvoyée par le serveur, le client doit considérer que la recherche paginée est un échec. Une nouvelle tentative de la recherche provoquera le redémarrage de la recherche à partir de la première page.  
  
## <a name="server-side-cookie-handling"></a>Gestion des cookies côté serveur  
Le serveur Windows renvoie le cookie au client et, parfois, stocke les informations relatives au cookie sur le serveur. Ces informations sont stockées sur le serveur dans un cache et sont soumis à certaines limites.  
  
Dans ce cas, le cookie envoyé au client par le serveur est également utilisé par le serveur pour rechercher les informations à partir du cache sur le serveur. Lorsque le client continue la recherche paginée, Windows Server utilise le cookie du client, ainsi que toutes les informations connexes à partir du cache de cookie du serveur pour continuer la recherche. Si le serveur ne peut pas trouver les informations connexes sur le cookie à partir du cache du serveur en raison d’une raison quelconque, la recherche est arrêtée et l’erreur est renvoyée au client.  
  
## <a name="how-the-cookie-pool-is-managed"></a>La gestion du pool de cookies  
Bien évidemment, le serveur LDAP sert plusieurs clients à la fois, et également plusieurs clients à la fois peuvent lancer des requêtes qui nécessitent l’utilisation du cache de cookie du serveur. Par conséquent, il l’implémentation de Windows Server est un suivi de l’utilisation du pool de cookies et limites sont mis en place pour le pool de cookies ne prend pas trop de ressources. Les limites peuvent être définies par l’administrateur utilisant les paramètres suivants dans la stratégie LDAP. Les valeurs par défaut et les explications sont:  
  
**MinResultSets: 4**  
  
Le serveur LDAP ne recherche pas la taille maximale du pool décrit ci-dessous, s’il y a moins de MinResultSets entrées dans le cache de cookie du serveur.  
  
**MaxResultSetSize: 262 144 octets**  
  
La taille du cache de cookie total sur le serveur ne doit pas dépasser le nombre maximal de MaxResultSetSize en octets. Si tel est le cas, les cookies à partir du plus ancien sont supprimés jusqu'à ce que le pool est inférieur à MaxResultSetSize octets ou à MinResultSets cookies dans le pool. Cela signifie qu’à l’aide des paramètres par défaut, le serveur LDAP considère qu’un pool de 450 Ko est OK, s’il existe seulement 3 cookies stockés.  
  
**MaxResultSetsPerConn: 10**  
  
Le serveur LDAP n’autorise pas plus de MaxResultSetsPerConn cookies par connexion LDAP dans le pool.  
  
## <a name="handling-deleted-cookies"></a>Gestion des Cookies supprimés  
La suppression des informations de cookie du cache du serveur LDAP n’entraîne pas une erreur immédiate pour les applications dans tous les cas. Les applications peuvent redémarrer la recherche paginée à partir du début et la terminer sur une autre tentative. Certaines applications possèdent ce genre de mécanisme de nouvelle tentative pour ajouter de la robustesse.  
  
Certaines applications peuvent accéder par le biais d’une recherche paginée et ne jamais la terminer. Cela peut laisser des entrées dans le serveur LDAP cache de cookie, qui est géré via le mécanisme de la section 4. Il est essentiel de libérer de la mémoire sur le serveur pour les recherches LDAP actives.  
  
Que se passe-t-il lorsqu’un cookie est supprimé sur le serveur et le client continue la recherche avec ce descripteur de cookie? Le serveur LDAP ne recherche le cookie dans le serveur de cache de cookie et renvoie une erreur pour la requête, la réponse d’erreur étant semblable à:  
  
```  
00000057: LdapErr: DSID-xxxxxxxx, comment: Error processing control, data 0, v1db1  
```  
  
> [!NOTE]  
> La valeur hexadécimale «DSID» varie en fonction de la version des fichiers binaires serveur LDAP.  
  
## <a name="reporting-on-the-cookie-pool"></a>Création de rapports sur le pool de cookies  
Le serveur LDAP a la possibilité de consigner les événements dans la catégorie «16 Ldap Interface» le [clé des diagnostics NTDS](https://support.microsoft.com/kb/314980/en-us). Si vous définissez cette catégorie sur «2», vous pouvez obtenir les événements suivants:  
  
```  
Log Name:      Directory Service  
Source:        Microsoft-Windows-ActiveDirectory_DomainService  
Event ID:      2898  
Task Category: LDAP Interface  
Level:         Information  
Description:  
Internal event: The LDAP server has reached the limit of the number of Result Sets it will maintain for a single connection.  A stored Result Set will be discarded.  This will result in a client being unable to continue a paged LDAP search.  
Maximum number of Result Sets allowed per LDAP connection:  
10  
Current number of Result Sets for this LDAP connection:  
11  
  
User Action  
The client should consider a more efficient search filter.  The limit for Maximum Result Sets per Connection may also be increased.  
  
```  
  
```  
Log Name:      Directory Service  
Source:        Microsoft-Windows-ActiveDirectory_DomainService  
Event ID:      2899  
Task Category: LDAP Interface  
Level:         Information  
Description:  
Internal event: The LDAP server has exceeded the limit of the LDAP Maximum Result Set Size. A stored Result Set will be discarded.  This will result in a client being unable to continue a paged LDAP search.   
  
Number of result sets currently stored:   
4   
Current Result Set Size:   
263504   
Maximum Result Set Size:   
262144   
Size of single Result Set being discarded:   
40876   
User Action   
The client should consider a more efficient search filter.  The limit for Maximum Result Set Size may also be increased.  
  
```  
  
Les événements signalent qu’un cookie stocké a été supprimé. Cela ne signifie pas qu’un client a constaté l’erreur LDAP, mais seulement que le serveur LDAP a atteint les limites d’administration pour le cache.  Dans certains cas, un client LDAP peut avoir abandonné la recherche paginée et peut ne jamais voir l’erreur.  
  
## <a name="monitoring-the-cookie-pool"></a>Analyse le pool de cookies  
Si vous rencontrez jamais d’erreurs de recherche LDAP dans votre domaine, vous devrez peut-être jamais à surveiller le pool de cookies de recherche LDAP server page. Au cas où vous voyez page LDAP de rechercher des erreurs connexes dans votre environnement, vous pouvez avoir un problème avec les limites d’administrateur cookie pool.  
  
Les événements 2898 et 2899 sont les seuls moyens de savoir que le serveur LDAP a atteint les limites d’administrateur. Lorsque vous rencontrez cette erreur requêtes LDAP en raison de l’erreur ci-dessus de traitement de contrôle, vous devez envisager l’augmentation des limites sur un ou plusieurs des paramètres de stratégie LDAP mentionnés à la section 4, en fonction de l’événement que vous obtenez.  
  
Si vous voyez l’événement 2898 sur votre serveur de contrôleur de domaine/LDAP, nous vous recommandons de que définir MaxResultSetsPerConn sur 25. Plus de 25 recherches paginées parallèles sur une seule connexion LDAP n’est pas habituel. Si vous continuez à voir l’événement 2898, analysez votre application de client LDAP qui rencontre l’erreur. La suspicion serait qu’une récupération des résultats paginés supplémentaires, laisse le cookie en attente et redémarre une nouvelle requête. Pour voir si l’application à un moment aurait suffisamment de cookies pour ses besoins, vous pouvez également augmenter la valeur de MaxResultSetsPerConn au-delà de 25 lorsque vous consultez les événements 2899 consignés sur vos contrôleurs de domaine, le plan est différent. Si votre serveur de contrôleur de domaine/LDAP s’exécute sur un ordinateur avec suffisamment de mémoire (plusieurs gigaoctets de mémoire disponible), nous vous recommandons de définir MaxResultsetSize sur le serveur LDAP pour > = 250 Mo. Cette limite est suffisamment grande pour contenir des volumes importants de recherches de paginées LDAP même sur des répertoires très volumineux.  
  
Si vous ne voyez toujours les événements 2899 avec un pool de 250 Mo ou plus, vous avez probablement de nombreux clients avec un très grand nombre d’objets retournés, interrogés de manière très fréquente. Les données que vous pouvez recueillir avec le [ensemble de collecteurs données Active Directory](http://blogs.technet.com/b/askds/archive/2010/06/08/son-of-spa-ad-data-collector-sets-in-win2008-and-beyond.aspx) peut vous aider à trouver des requêtes paginées répétitives qui occupent vos serveurs LDAP occupé. Ces requêtes s’affichent avec un nombre de «Entrées retournées» qui correspond à la taille de la page utilisée.  
  
Si possible, vous devez passer en revue la conception de l’application et implémenter une approche différente avec une fréquence inférieure, volume de données ou moins d’instances client interrogeant ces données. Dans le cas d’applications pour lequel vous avez accès du code source, ce guide pour [création d’Applications AD-Enabled efficaces](https://msdn.microsoft.com/en-us/library/ms808539.aspx) peut vous aider à comprendre la façon optimale pour les applications accèdent à Active Directory.  
  
Si le comportement de requête ne peut pas être modifié, une approche consiste à ajouter plus les instances répliquées des contextes d’appellation nécessités et pour redistribuer les clients et finalement à réduire la charge sur les serveurs LDAP.  
  


