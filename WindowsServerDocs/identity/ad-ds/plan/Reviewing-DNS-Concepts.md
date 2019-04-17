---
ms.assetid: 133474ee-316d-4b1c-acc6-ad5434a064d5
title: Examen des Concepts DNS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 29c4c3d1d785d1b3b1fa1926eca8298aab2b3ee3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="reviewing-dns-concepts"></a>Examen des Concepts DNS

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Système DNS (Domain Name) est une base de données distribuée qui représente un espace de noms. L’espace de noms contient toutes les informations nécessaires pour les clients rechercher n’importe quel nom. Tout serveur DNS peut répondre aux requêtes sur n’importe quel nom au sein de son espace de noms. Un serveur DNS répond aux requêtes de l’une des manières suivantes:  
  
-   Si la réponse est dans sa mémoire cache, il répond à la requête à partir du cache.  
  
-   Si la réponse est dans une zone hébergée par le serveur DNS, il répond à la requête à partir de sa zone. Une zone est une partie de l’arborescence DNS stocké sur un serveur DNS. Lorsqu’un serveur DNS héberge une zone, il ne fait autorité pour les noms de la zone (autrement dit, le serveur DNS peut répondre aux requêtes de n’importe quel nom dans la zone). Par exemple, un serveur qui héberge la zone contoso.com peut répondre aux requêtes pour n’importe quel nom dans contoso.com.  
  
-   Si le serveur ne peut pas répondre à la requête à partir de son cache ou aux zones, il interroge d’autres serveurs de la réponse.  
  
Il est important de comprendre les fonctionnalités de base de DNS, telles que la délégation, la résolution de noms récursive et zones DNS intégrées à ActiveDirectory, car ils ont un impact direct sur votre conception de la structure logique ActiveDirectory.  
  
Pour plus d’informations sur DNS et les Services de domaine ActiveDirectory (ADDS), consultez [DNS et les services ADDS](../../ad-ds/plan/DNS-and-AD-DS.md).  
  
## <a name="delegation"></a>Délégation  
Pour un serveur DNS pour répondre aux requêtes sur n’importe quel nom, il doit disposer d’un chemin d’accès direct ou indirect à chaque zone dans l’espace de noms. Ces chemins d’accès sont créés au moyen de délégation. Une délégation est un enregistrement dans une zone parent qui répertorie un serveur de noms faisant autorité pour la zone dans le niveau de la hiérarchie. Délégations rendent possible pour les serveurs dans une zone pour faire référence les clients aux serveurs dans d’autres zones. L’illustration suivante montre un exemple de délégation.  
  
![Concepts DNS](../../media/Reviewing-DNS-Concepts/0c24b576-d41a-4e5d-ad3d-6be81e095835.gif)  
  
Le serveur DNS racine héberge la zone racine représentée par un point (. ). La zone racine contient une délégation à une zone dans le niveau de la hiérarchie, la zone com. La délégation dans la zone racine indique le serveur DNS racine que, pour rechercher la zone de com, il doit contacter le serveur Com. De même, la délégation dans la zone com indique le serveur Com qui, pour rechercher la zone contoso.com, il doit contacter le serveur de Contoso.  
  
> [!NOTE]  
> Une délégation utilise deux types d’enregistrements. L’enregistrement de ressource de nom du serveur de (noms NS) fournit le nom d’un serveur faisant autorité. Hôte (A) et les enregistrements de ressource hôte (AAAA) fournissent IP version4 (IPv4) et IP version6 (IPv6) adresses d’un serveur faisant autorité.  
  
Ce système de zones et les délégations crée une arborescence hiérarchique qui représente l’espace de noms DNS. Chaque zone représente une couche de la hiérarchie, et chaque délégation représente une branche de l’arborescence.  
  
À l’aide de la hiérarchie des zones et les délégations, un serveur DNS racine permettre trouver n’importe quel nom dans l’espace de noms DNS. La zone racine inclut des délégations conduire directement ou indirectement à toutes les autres zones dans la hiérarchie. N’importe quel serveur qui peut interroger le serveur DNS racine peut utiliser les informations dans les délégations pour trouver n’importe quel nom dans l’espace de noms.  
  
## <a name="recursive-name-resolution"></a>Résolution de noms récursive  
Résolution de noms récursive est le processus par lequel un serveur DNS utilise la hiérarchie des zones et les délégations pour répondre aux requêtes pour lesquels il n’est pas faisant autorité.  
  
Dans certaines configurations, les serveurs DNS incluent les indications de racine (autrement dit, une liste de noms et adresses IP) leur permettant d’interroger des serveurs DNS racine. Dans d’autres configurations, les serveurs de transférer toutes les requêtes qu’ils ne peuvent pas vers un autre serveur. Les indications de racine et de transfert sont les deux méthodes serveurs DNS peuvent utiliser pour résoudre les requêtes pour lesquels ils ne sont pas faisant autorités.  
  
### <a name="resolving-names-by-using-root-hints"></a>Résolution de noms à l’aide des indications de racine  
Les indications de racine permettent à un serveur DNS localiser les serveurs DNS racine. Une fois un serveur DNS localise le serveur DNS racine, il peut résoudre une requête pour cet espace de noms. L’illustration suivante décrit comment DNS résout un nom à l’aide des indications de racine.  
  
![Concepts DNS](../../media/Reviewing-DNS-Concepts/1c044845-b104-4262-a7af-474ba3558a85.gif)  
  
Dans cet exemple, les événements suivants se produisent:  
  
1.  Un client envoie une requête récursive à un serveur DNS pour demander l’adresse IP qui correspond au nom ftp.contoso.com. Une requête récursive indique que le client souhaite une réponse définitive à sa requête. La réponse à la requête récursive doit être une adresse valide ou un message indiquant que l’adresse est introuvable.  
  
2.  Étant donné que le serveur DNS ne fait pas autorité pour le nom et n’a pas la réponse dans sa mémoire cache, le serveur DNS utilise des indications de racine pour trouver l’adresse IP du serveur DNS racine.  
  
3.  Le serveur DNS utilise une requête itérative pour demander le serveur DNS racine pour résoudre le nom ftp.contoso.com. Une requête itérative indique que le serveur accepte une référence à un autre serveur à la place d’une réponse définitive à la requête. Étant donné que le nom ftp.contoso.com se termine avec l’étiquette de com, le serveur DNS racine renvoie une référence au serveur Com qui héberge la zone com.  
  
4.  Le serveur DNS utilise une requête itérative pour demander au serveur Com pour résoudre le nom ftp.contoso.com. Étant donné que le nom ftp.contoso.com se termine par le nom contoso.com, le serveur Com renvoie une référence au serveur de Contoso qui héberge le contoso.com la zone.  
  
5.  Le serveur DNS utilise une requête itérative pour demander au serveur Contoso pour résoudre le nom ftp.contoso.com. Le serveur Contoso recherche la réponse dans ses données de zone, puis renvoie la réponse au serveur.  
  
6.  Le serveur renvoie ensuite le résultat au client.  
  
### <a name="resolving-names-by-using-forwarding"></a>Résolution de noms à l’aide du transfert  
Transfert permet la résolution de noms itinéraire via des serveurs spécifiques au lieu d’utiliser les indications de racine. L’illustration suivante décrit comment DNS résout un nom à l’aide de transfert.  
  
![Concepts DNS](../../media/Reviewing-DNS-Concepts/05bc2eb0-1033-4e53-ae30-244fa247d000.gif)  
  
Dans cet exemple, les événements suivants se produisent:  
  
1.  Un client interroge un serveur DNS pour le nom ftp.contoso.com.  
  
2.  Le serveur DNS transfère la requête vers un autre serveur DNS, appelé un redirecteur.  
  
3.  Étant donné que le redirecteur ne fait pas autorité pour le nom et n’a pas la réponse dans sa mémoire cache, il utilise les indications de racine pour trouver l’adresse IP du serveur DNS racine.  
  
4.  Le redirecteur utilise une requête itérative pour demander le serveur DNS racine pour résoudre le nom ftp.contoso.com. Étant donné que le nom ftp.contoso.com se termine par le nom de com, le serveur DNS racine renvoie une référence au serveur Com qui héberge la zone com.  
  
5.  Le redirecteur utilise une requête itérative pour demander au serveur Com pour résoudre le nom ftp.contoso.com. Étant donné que le nom ftp.contoso.com se termine par le nom contoso.com, le serveur Com renvoie une référence au serveur de Contoso qui héberge le contoso.com la zone.  
  
6.  Le redirecteur utilise une requête itérative pour demander au serveur Contoso pour résoudre le nom ftp.contoso.com. Le serveur Contoso recherche la réponse dans ses fichiers de zone et puis renvoie la réponse du serveur.  
  
7.  Le redirecteur renvoie ensuite le résultat vers le serveur DNS d’origine.  
  
8.  Le serveur DNS d’origine puis renvoie le résultat au client.  
  


