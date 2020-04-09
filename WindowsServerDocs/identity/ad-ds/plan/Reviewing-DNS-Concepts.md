---
ms.assetid: 133474ee-316d-4b1c-acc6-ad5434a064d5
title: Examen des concepts DNS
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 37c33ca181394c66ef149715c3f1477774061660
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822062"
---
# <a name="reviewing-dns-concepts"></a>Examen des concepts DNS

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

DNS (Domain Name System) est une base de données distribuée qui représente un espace de noms. L’espace de noms contient toutes les informations nécessaires à n’importe quel client pour rechercher n’importe quel nom. Tout serveur DNS peut répondre à des requêtes concernant n’importe quel nom dans son espace de noms. Un serveur DNS répond aux requêtes de l’une des manières suivantes :  
  
- Si la réponse se trouve dans son cache, elle répond à la requête à partir du cache.  
- Si la réponse se trouve dans une zone hébergée par le serveur DNS, elle répond à la requête à partir de sa zone. Une zone est une partie de l’arborescence DNS stockée sur un serveur DNS. Lorsqu’un serveur DNS héberge une zone, il fait autorité pour les noms de cette zone (autrement dit, le serveur DNS peut répondre aux requêtes pour n’importe quel nom de la zone). Par exemple, un serveur hébergeant la zone contoso.com peut répondre à des requêtes pour n’importe quel nom dans contoso.com.  
- Si le serveur ne peut pas répondre à la requête à partir de son cache ou de ses zones, il interroge d’autres serveurs pour la réponse.  

Il est important de comprendre les fonctionnalités de base de DNS, telles que la délégation, la résolution de noms récursive et les zones DNS intégrées à Active Directory, car elles ont un impact direct sur votre conception de structure logique Active Directory.  
  
Pour plus d’informations sur DNS et Active Directory Domain Services (AD DS), consultez [DNS et AD DS](../../ad-ds/plan/DNS-and-AD-DS.md).  
  
## <a name="delegation"></a>Délégation

Pour qu’un serveur DNS réponde aux requêtes relatives à n’importe quel nom, il doit disposer d’un chemin d’accès direct ou indirect à chaque zone de l’espace de noms. Ces chemins d’accès sont créés au moyen de la délégation. Une délégation est un enregistrement dans une zone parente qui répertorie un serveur de noms faisant autorité pour la zone au niveau suivant de la hiérarchie. Les délégations permettent aux serveurs dans une zone de faire référence aux serveurs dans d’autres zones. L’illustration suivante montre un exemple de délégation.  
  
![Concepts DNS](../../media/Reviewing-DNS-Concepts/0c24b576-d41a-4e5d-ad3d-6be81e095835.gif)  
  
Le serveur racine DNS héberge la zone racine représentée par un point (. ). La zone racine contient une délégation à une zone au niveau suivant de la hiérarchie, la zone com. La délégation dans la zone racine indique au serveur racine DNS que, pour trouver la zone com, il doit contacter le serveur com. De même, la délégation dans la zone com indique au serveur com que, pour trouver la zone contoso.com, il doit contacter le serveur contoso.  
  
> [!NOTE]  
> Une délégation utilise deux types d’enregistrements. L’enregistrement de ressource de serveur de noms (NS) fournit le nom d’un serveur faisant autorité. Les enregistrements de ressource d’hôte (A) et d’hôte (AAAA) fournissent des adresses IP version 4 (IPv4) et IP version 6 (IPv6) d’un serveur faisant autorité.  
  
Ce système de zones et de délégations crée une arborescence hiérarchique qui représente l’espace de noms DNS. Chaque zone représente une couche dans la hiérarchie, et chaque délégation représente une branche de l’arborescence.  
  
En utilisant la hiérarchie des zones et des délégations, un serveur racine DNS peut trouver n’importe quel nom dans l’espace de noms DNS. La zone racine comprend des délégations qui mènent directement ou indirectement à toutes les autres zones de la hiérarchie. Tout serveur pouvant interroger le serveur racine DNS peut utiliser les informations contenues dans les délégations pour rechercher n’importe quel nom dans l’espace de noms.  
  
## <a name="recursive-name-resolution"></a>Résolution de noms récursive

La résolution de noms récursive est le processus par lequel un serveur DNS utilise la hiérarchie de zones et de délégations pour répondre aux requêtes pour lesquelles il ne fait pas autorité.  
  
Dans certaines configurations, les serveurs DNS incluent des indications de racine (autrement dit, une liste de noms et d’adresses IP) qui leur permettent d’interroger les serveurs racines DNS. Dans d’autres configurations, les serveurs transfèrent toutes les requêtes qu’ils ne peuvent pas répondre à un autre serveur. Le transfert et les indications de racine sont les deux méthodes que les serveurs DNS peuvent utiliser pour résoudre les requêtes pour lesquelles ils ne font pas autorité.  
  
### <a name="resolving-names-by-using-root-hints"></a>Résolution de noms à l’aide d’indications de racine

Les indications de racine permettent à n’importe quel serveur DNS de localiser les serveurs racines DNS. Une fois qu’un serveur DNS a localisé le serveur racine DNS, il peut résoudre n’importe quelle requête pour cet espace de noms. L’illustration suivante décrit comment DNS résout un nom à l’aide d’indications de racine.  
  
![Concepts DNS](../../media/Reviewing-DNS-Concepts/1c044845-b104-4262-a7af-474ba3558a85.gif)  
  
Dans cet exemple, les événements suivants se produisent :  
  
1. Un client envoie une requête récursive à un serveur DNS pour demander l’adresse IP qui correspond au nom ftp.contoso.com. Une requête récursive indique que le client souhaite une réponse définitive à sa requête. La réponse à la requête récursive doit être une adresse valide ou un message indiquant que l’adresse est introuvable.  
2. Étant donné que le serveur DNS ne fait pas autorité pour le nom et qu’il n’a pas de réponse dans son cache, le serveur DNS utilise des indications de racine pour trouver l’adresse IP du serveur racine DNS.  
3. Le serveur DNS utilise une requête itérative pour demander au serveur racine DNS de résoudre le nom ftp.contoso.com. Une requête itérative indique que le serveur va accepter une référence à un autre serveur à la place d’une réponse définitive à la requête. Étant donné que le nom ftp.contoso.com se termine par l’étiquette com, le serveur racine DNS retourne une référence au serveur com qui héberge la zone com.  
4. Le serveur DNS utilise une requête itérative pour demander au serveur com de résoudre le nom ftp.contoso.com. Étant donné que le nom ftp.contoso.com se termine par le nom contoso.com, le serveur COM retourne une référence au serveur Contoso qui héberge la zone contoso.com.  
5. Le serveur DNS utilise une requête itérative pour demander au serveur Contoso de résoudre le nom ftp.contoso.com. Le serveur Contoso recherche la réponse dans ses données de zone, puis renvoie la réponse au serveur.  
6. Le serveur retourne ensuite le résultat au client.  
  
### <a name="resolving-names-by-using-forwarding"></a>Résolution de noms à l’aide du transfert

Le transfert vous permet d’acheminer la résolution de noms via des serveurs spécifiques au lieu d’utiliser des indications de racine. L’illustration suivante décrit comment DNS résout un nom à l’aide du transfert.  
  
![Concepts DNS](../../media/Reviewing-DNS-Concepts/05bc2eb0-1033-4e53-ae30-244fa247d000.gif)  
  
Dans cet exemple, les événements suivants se produisent :  
  
1. Un client interroge un serveur DNS pour le nom ftp.contoso.com.  
2. Le serveur DNS transfère la requête à un autre serveur DNS, appelé redirecteur.  
3. Étant donné que le redirecteur ne fait pas autorité pour le nom et qu’il n’a pas de réponse dans son cache, il utilise des indications de racine pour trouver l’adresse IP du serveur racine DNS.  
4. Le redirecteur utilise une requête itérative pour demander au serveur racine DNS de résoudre le nom ftp.contoso.com. Étant donné que le nom ftp.contoso.com se termine par le nom com, le serveur racine DNS retourne une référence au serveur com qui héberge la zone com.  
5. Le redirecteur utilise une requête itérative pour demander au serveur com de résoudre le nom ftp.contoso.com. Étant donné que le nom ftp.contoso.com se termine par le nom contoso.com, le serveur COM retourne une référence au serveur Contoso qui héberge la zone contoso.com.  
6. Le redirecteur utilise une requête itérative pour demander au serveur Contoso de résoudre le nom ftp.contoso.com. Le serveur Contoso recherche la réponse dans ses fichiers de zone, puis renvoie la réponse au serveur.  
7. Le redirecteur retourne ensuite le résultat au serveur DNS d’origine.  
8. Le serveur DNS d’origine renvoie alors le résultat au client.  
