---
ms.assetid: 133474ee-316d-4b1c-acc6-ad5434a064d5
title: Examen des concepts DNS
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d077ecc30caca9b8f3b382af624121fec729afae
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890500"
---
# <a name="reviewing-dns-concepts"></a>Examen des concepts DNS

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Système DNS (Domain Name) est une base de données distribuée qui représente un espace de noms. L’espace de noms contient toutes les informations nécessaires pour n’importe quel client à rechercher n’importe quel nom. N’importe quel serveur DNS peut répondre aux requêtes sur n’importe quel nom au sein de son espace de noms. Un serveur DNS répond aux requêtes dans une des manières suivantes :  
  
- Si la réponse est dans son cache, il répond à la requête à partir du cache.  
- Si la réponse est dans une zone hébergée par le serveur DNS, il répond à la requête à partir de sa zone. Une zone est une partie de l’arborescence DNS stocké sur un serveur DNS. Lorsqu’un serveur DNS héberge une zone, il fait autorité pour les noms dans cette zone (autrement dit, le serveur DNS peut répondre aux requêtes pour n’importe quel nom dans la zone). Par exemple, un serveur qui héberge la zone contoso.com peut répondre aux requêtes pour n’importe quel nom de contoso.com.  
- Si le serveur ne peut pas répondre à la requête à partir de son cache ou des zones, il interroge les autres serveurs pour la réponse.  

Il est important de comprendre les principales fonctionnalités de DNS, telles que la délégation, résolution de noms récursive et zones DNS intégrées à Active Directory, car elles ont un impact direct sur votre conception de la structure logique Active Directory.  
  
Pour plus d’informations sur DNS et des Services de domaine Active Directory (AD DS), consultez [DNS et AD DS](../../ad-ds/plan/DNS-and-AD-DS.md).  
  
## <a name="delegation"></a>Délégation

Pour un serveur DNS pour répondre aux requêtes sur n’importe quel nom, il doit avoir un chemin d’accès direct ou indirect à chaque zone dans l’espace de noms. Ces chemins d’accès sont créés au moyen de délégation. Une délégation est un enregistrement dans une zone parente qui répertorie un serveur de noms faisant autorité pour la zone dans le niveau suivant de la hiérarchie. Délégations rendent possible pour les serveurs dans une zone pour faire référence les clients aux serveurs dans d’autres zones. L’illustration suivante montre un exemple de délégation.  
  
![Concepts DNS](../../media/Reviewing-DNS-Concepts/0c24b576-d41a-4e5d-ad3d-6be81e095835.gif)  
  
Le serveur DNS racine héberge la zone racine représentée sous la forme d’un point (. ). La zone racine contient une délégation à une zone dans le niveau de la hiérarchie, la zone com. La délégation dans la zone racine indique le serveur DNS racine que, pour trouver la zone com, il doit contacter le serveur Com. De même, la délégation dans la zone com indique le serveur Com qui, pour trouver la zone contoso.com, il doit contacter le serveur de Contoso.  
  
> [!NOTE]  
> Une délégation utilise deux types d’enregistrements. L’enregistrement de ressource de nom du serveur de (noms NS) fournit le nom d’un serveur faisant autorité. Hôte (A) et les enregistrements de ressource hôte (AAAA) fournissent des IP version 4 (IPv4) et IP version 6 (IPv6) adresses d’un serveur faisant autorité.  
  
Ce système de zones et les délégations crée une arborescence hiérarchique qui représente l’espace de noms DNS. Chaque zone représente une couche dans la hiérarchie, et chaque délégation représente une branche de l’arborescence.  
  
À l’aide de la hiérarchie des zones et des délégations, un serveur DNS racine peut trouver n’importe quel nom dans l’espace de noms DNS. La zone racine inclut des délégations qui mènent directement ou indirectement à toutes les autres zones dans la hiérarchie. N’importe quel serveur qui peut interroger le serveur DNS racine peut utiliser les informations dans les délégations pour rechercher n’importe quel nom dans l’espace de noms.  
  
## <a name="recursive-name-resolution"></a>Résolution de noms récursive

Résolution de noms récursive est le processus par lequel un serveur DNS utilise la hiérarchie des zones et des délégations de répondre aux requêtes pour lesquelles il ne fait pas autorité.  
  
Dans certaines configurations, les serveurs DNS incluent des indications de racine (autrement dit, une liste de noms et adresses IP) leur permettant d’interroger des serveurs DNS racine. Dans d’autres configurations, les serveurs transférer toutes les requêtes qu’ils ne peut pas répondre à un autre serveur. Les indications de racine et de transfert sont les deux méthodes utilisables par les serveurs DNS pour résoudre les requêtes pour lesquelles ils ne font pas autorités.  
  
### <a name="resolving-names-by-using-root-hints"></a>Résolution de noms à l’aide des indications de racine

Indications de racine permettent à un serveur DNS localiser les serveurs DNS racine. Une fois un serveur DNS localise le serveur DNS racine, il peut résoudre n’importe quelle requête pour cet espace de noms. L’illustration suivante décrit comment le DNS résout un nom à l’aide des indications de racine.  
  
![Concepts DNS](../../media/Reviewing-DNS-Concepts/1c044845-b104-4262-a7af-474ba3558a85.gif)  
  
Dans cet exemple, les événements suivants se produisent :  
  
1. Un client envoie une requête récursive à un serveur DNS pour demander l’adresse IP qui correspond à la ftp.contoso.com de nom. Une requête récursive indique que le client veut une réponse définitive à sa requête. La réponse à la requête récursive doit être une adresse valide ou un message indiquant que l’adresse est introuvable.  
2. Étant donné que le serveur DNS ne fait pas autorité pour le nom et n’a pas la réponse dans son cache, le serveur DNS utilise les indications de racine pour rechercher l’adresse IP du serveur DNS racine.  
3. Le serveur DNS utilise une requête itérative pour demander au serveur de racine DNS pour résoudre le nom ftp.contoso.com. Une requête itérative indique que le serveur accepte une référence à un autre serveur à la place d’une réponse définitive à la requête. Étant donné que le ftp.contoso.com nom se termine par l’étiquette com, le serveur DNS racine renvoie une référence au serveur Com qui héberge la zone com.  
4. Le serveur DNS utilise une requête itérative pour demander au serveur Com pour résoudre le nom ftp.contoso.com. Étant donné que le ftp.contoso.com nom se termine par le nom de contoso.com, le serveur Com retourne une référence au serveur de Contoso qui héberge la zone contoso.com.  
5. Le serveur DNS utilise une requête itérative pour demander au serveur de Contoso pour résoudre le nom ftp.contoso.com. Le serveur Contoso recherche la réponse dans ses données de zone, puis renvoie la réponse au serveur.  
6. Le serveur renvoie ensuite le résultat au client.  
  
### <a name="resolving-names-by-using-forwarding"></a>Résolution de noms à l’aide de transfert

Transfert vous permet de résolution de nom d’itinéraire via des serveurs spécifiques au lieu d’utiliser des indications de racine. L’illustration suivante décrit comment le DNS résout un nom à l’aide de transfert.  
  
![Concepts DNS](../../media/Reviewing-DNS-Concepts/05bc2eb0-1033-4e53-ae30-244fa247d000.gif)  
  
Dans cet exemple, les événements suivants se produisent :  
  
1. Un client interroge un serveur DNS pour le nom ftp.contoso.com.  
2. Le serveur DNS transfère la requête vers un autre serveur DNS, appelé un redirecteur.  
3. Étant donné que le redirecteur ne fait pas autorité pour le nom et n’a pas la réponse dans son cache, il utilise des indications de racine pour trouver l’adresse IP du serveur DNS racine.  
4. Le redirecteur utilise une requête itérative pour demander au serveur de racine DNS pour résoudre le nom ftp.contoso.com. Étant donné que le ftp.contoso.com nom se termine par le nom com, le serveur DNS racine renvoie une référence au serveur Com qui héberge la zone com.  
5. Le redirecteur utilise une requête itérative pour demander au serveur Com pour résoudre le nom ftp.contoso.com. Étant donné que le ftp.contoso.com nom se termine par le nom de contoso.com, le serveur Com retourne une référence au serveur de Contoso qui héberge la zone contoso.com.  
6. Le redirecteur utilise une requête itérative pour demander au serveur de Contoso pour résoudre le nom ftp.contoso.com. Le serveur Contoso recherche la réponse dans ses fichiers de zone, puis renvoie la réponse au serveur.  
7. Le redirecteur retourne ensuite le résultat vers le serveur DNS d’origine.  
8. Le serveur DNS d’origine retourne ensuite le résultat au client.  
