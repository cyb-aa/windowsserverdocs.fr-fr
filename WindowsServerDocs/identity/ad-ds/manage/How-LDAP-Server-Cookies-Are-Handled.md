---
ms.assetid: 3acaa977-ed63-4e38-ac81-229908c47208
title: Gestion des cookies du serveur LDAP
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f90f53763e7a31ffed1fd820061910742e5cf98a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823232"
---
# <a name="how-ldap-server-cookies-are-handled"></a>Gestion des cookies du serveur LDAP

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dans LDAP, certaines requêtes génèrent un jeu de résultats de grande taille. Ces requêtes présentent des défis pour Windows Server.  
  
La collecte et la création de ces jeux de résultats volumineux représentent un travail considérable. De nombreux attributs doivent être convertis d'une représentation interne vers la représentation de transfert LDAP. Pour beaucoup d'attributs, une conversion d'un format interne, souvent binaire, doit se produire dans un format UTF-8 textuel dans le cadre de la réponse LDAP.  
  
Un autre défi est que les jeux de résultats avec des dizaines de milliers d'objets deviennent encombrants, facilement plusieurs centaines de méga-octets. Ils requièrent alors un espace d'adressage virtuel volumineux. En outre, le transfert sur réseau présente des problèmes car l'effort entier est perdu lorsque la session TCP se décompose en transit.  
  
Ces problèmes de capacité et de logistique ont conduit les développeurs LDAP Microsoft à créer une extension LDAP appelée « requête paginée ». Elle implémente un contrôle LDAP pour séparer une seule requête volumineuse en segments plus petits de jeux de résultats. Il est devenu la norme RFC [2696](http://www.ietf.org/rfc/rfc2696).  
  
## <a name="cookie-handling-on-client"></a>Gestion des cookies sur le client  
La méthode de requête paginée utilise la taille de page définie par le client ou par le biais d’une [stratégie LDAP](https://support.microsoft.com/kb/315071/en-us) (« MaxPageSize »). Le client doit systématiquement activer la pagination en envoyant un contrôle LDAP.  

  
Lorsque vous travaillez sur une requête avec un grand nombre de résultats, à un moment donné, le nombre maximal d'objets autorisés est atteint. Le serveur LDAP empaquette le message de réponse et ajoute un cookie qui contient les informations nécessaires pour continuer ultérieurement la recherche.  
  
L'application cliente doit traiter le cookie comme un objet blob opaque. Elle peut récupérer le nombre d'objets dans la réponse et continuer la recherche en fonction de la présence du cookie. Le client continue la recherche en envoyant à nouveau la requête au serveur LDAP avec les mêmes paramètres, tels que l'objet de base et le filtre, et inclut la valeur du cookie qui a été retournée dans la réponse précédente.  
  
Si le nombre d’objets ne remplit pas une page, la requête LDAP est terminée et la réponse ne contient aucun cookie de page. Si aucun cookie n'est retourné par le serveur, le client doit considérer que la recherche paginée est terminée avec succès.  
  
Si une erreur est renvoyée par le serveur, le client doit considérer que la recherche paginée est un échec. Une nouvelle tentative de recherche provoquera le redémarrage de la recherche à partir de la première page.  
  
## <a name="server-side-cookie-handling"></a>Gestion des cookies côté serveur  
Le serveur Windows renvoie le cookie au client et, parfois, stocke les informations relatives au cookie sur le serveur. Ces informations sont stockées sur le serveur dans un cache et sont soumises à certaines limites.  
  
Dans ce cas, le cookie envoyé au client par le serveur est également utilisé par le serveur pour rechercher les informations à partir du cache sur le serveur. Lorsque le client continue la recherche paginée, Windows Server utilise le cookie du client ainsi que toutes les informations connexes à partir du cache de cookie du serveur afin de continuer la recherche. Si le serveur ne peut pas trouver les informations connexes sur le cookie à partir du cache du serveur pour une raison quelconque, la recherche est arrêtée et l'erreur est retournée au client.  
  
## <a name="how-the-cookie-pool-is-managed"></a>Gestion du pool de cookies  
Évidemment, le serveur LDAP sert plusieurs clients à la fois, et plusieurs clients à la fois peuvent lancer des requêtes qui requièrent l'utilisation du cache de cookie du serveur. Par conséquent, l'implémentation de Windows Server comporte un suivi de l'utilisation et des limites du pool de cookies afin que le pool n'utilise pas trop de ressources. Les limites peuvent être définies par l'administrateur en utilisant les paramètres suivants dans la stratégie LDAP. Les valeurs par défaut et les explications sont les suivantes :  
  
**MinResultSets : 4**  
  
Le serveur LDAP ne recherche pas la taille maximale du pool indiquée ci-dessous s'il y a moins de MinResultSets entrées dans le cache de cookie du serveur.  
  
**MaxResultSetSize : 262 144 octets**  
  
La taille totale du cache de cookie sur le serveur ne doit pas dépasser le nombre maximal de MaxResultSetSize en octets. Le cas échéant, les cookies, à partir du plus ancien, sont supprimés jusqu'à ce que le pool soit inférieur à MaxResultSetSize octets ou à MinResultSets cookies. Cela signifie que, en utilisant les paramètres par défaut, le serveur LDAP considère qu'un pool de 450 Ko est correct s'il y a seulement 3 cookies stockés.  
  
**MaxResultSetsPerConn : 10**  
  
Le serveur LDAP n'autorise pas plus de MaxResultSetsPerConn cookies par connexion LDAP dans le pool.  
  
## <a name="handling-deleted-cookies"></a>Gestion des cookies supprimés  
La suppression des informations de cookie du cache du serveur LDAP n'entraîne pas une erreur immédiate pour les applications dans tous les cas. Les applications peuvent redémarrer la recherche paginée à partir du début et la terminer sur une autre tentative. Certaines applications possèdent ce genre de mécanisme de nouvelle tentative pour ajouter de la robustesse.  
  
Certaines applications peuvent effectuer une recherche paginée et ne jamais la terminer. Cela peut laisser des entrées dans le cache de cookie du serveur LDAP, ce qui est géré par le mécanisme décrit à la section 4. Il est essentiel de libérer de la mémoire sur le serveur pour les recherches LDAP actives.  
  
Que se passe-t-il lorsqu'un cookie est supprimé du serveur et que le client continue la recherche avec ce descripteur de cookie ? Le serveur LDAP ne trouve pas le cookie dans le cache de cookie du serveur et renvoie une erreur pour la requête, la réponse d'erreur étant semblable à :  
  
```  
00000057: LdapErr: DSID-xxxxxxxx, comment: Error processing control, data 0, v1db1  
```  
  
> [!NOTE]  
> La valeur hexadécimale en arrière-plan de « DSID » varie en fonction de la version de build des fichiers binaires du serveur LDAP.  
  
## <a name="reporting-on-the-cookie-pool"></a>Création de rapports sur le pool de cookies  
Le serveur LDAP peut consigner des événements via la catégorie « 16 interface LDAP » dans la [clé de diagnostics NTDS](https://support.microsoft.com/kb/314980/en-us). Si vous définissez cette catégorie sur « 2 », vous pouvez vous procurer les événements suivants :  
  
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
  
Les événements signalent qu'un cookie stocké a été supprimé. Cela NE signifie PAS qu'un client a constaté l'erreur LDAP, mais seulement que le serveur LDAP a atteint les limites d'administration pour le cache.  Dans certains cas, un client LDAP peut avoir abandonné la recherche paginée et peut ne jamais voir l'erreur.  
  
## <a name="monitoring-the-cookie-pool"></a>Surveillance du pool de cookies  
Si vous ne rencontrez jamais d'erreurs de recherche LDAP dans votre domaine, vous n'aurez peut-être jamais à surveiller le pool de cookies de recherche paginée du serveur LDAP. Dans le cas où vous voyez des erreurs liées à la recherche paginée LDAP dans votre environnement, vous pouvez avoir un problème avec les limites d'administrateur du pool de cookies.  
  
Les événements 2898 et 2899 sont les seuls moyens de savoir si le serveur LDAP a atteint les limites d'administrateur. Lorsque vous rencontrez cette erreur de requêtes LDAP en raison de l'erreur de traitement de contrôle ci-dessus, vous devez envisager l'augmentation des limites de l'un ou de plusieurs des paramètres de stratégie LDAP mentionnés à la section 4, en fonction de l'événement que vous obtenez.  
  
Si vous voyez l'événement 2898 sur votre serveur de contrôleur de domaine/LDAP, nous vous recommandons de définir MaxResultSetsPerConn sur 25. Plus de 25 recherches paginées parallèles sur une seule connexion LDAP n'est pas habituel. Si vous continuez à voir l'événement 2898, analysez votre application cliente LDAP qui rencontre l'erreur. Il est probable que, d'une certaine manière, l'application reste bloquée lors de la récupération des résultats paginés supplémentaires, laisse le cookie en attente et redémarre une nouvelle requête. Déterminez donc si l'application, à un moment donné, a suffisamment de cookies pour ses besoins. Vous pouvez également augmenter la valeur de MaxResultSetsPerConn au-delà de 25 Lorsque vous voyez des événements 2899 consignés sur vos contrôleurs de domaine, l'approche est différente. Si votre serveur de contrôleur de domaine/LDAP s'exécute sur un ordinateur avec suffisamment de mémoire (plusieurs gigaoctets de mémoire disponible), nous vous recommandons de définir MaxResultsetSize sur le serveur LDAP sur une valeur supérieure ou égale à 250 Mo. Cette limite est assez grande pour contenir des volumes importants de recherches paginées LDAP même sur des répertoires très volumineux.  
  
Si vous voyez toujours les événements 2899 avec un pool de 250 Mo ou plus, vous avez probablement de nombreux clients avec un très grand nombre d'objets retournés, interrogés de manière très fréquente. Les données que vous pouvez collecter avec l' [ensemble de collecteurs de données Active Directory](https://blogs.technet.com/b/askds/archive/2010/06/08/son-of-spa-ad-data-collector-sets-in-win2008-and-beyond.aspx) peuvent vous aider à trouver des requêtes paginées répétitives qui maintiennent vos serveurs LDAP occupés. Ces requêtes s’affichent avec un nombre d’entrées retournées qui correspond à la taille de la page utilisée.  
  
Si possible, vous devez examiner la conception de l’application et implémenter une approche différente avec une fréquence inférieure, le volume de données et/ou moins d’instances de client qui interrogent ces données. Dans le cas des applications pour lesquelles vous disposez d’un accès au code source, ce guide de [création d’applications ad efficaces](https://msdn.microsoft.com/library/ms808539.aspx) peut vous aider à comprendre la façon optimale pour les applications d’accéder à Active Directory.  
  
Si le comportement de la requête ne peut pas être modifié, une approche consiste à ajouter plus d’instances répliquées des contextes d’attribution de noms nécessaires et à redistribuer les clients, puis à réduire la charge sur les serveurs LDAP individuels.  
  


