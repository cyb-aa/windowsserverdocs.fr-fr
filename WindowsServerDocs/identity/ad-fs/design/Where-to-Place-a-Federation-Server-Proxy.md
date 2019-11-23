---
ms.assetid: ec26705c-4446-4226-b9b4-b775b642f0f4
title: Où installer un serveur proxy de fédération
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 73e68d03e4f2f76dbaf4a497da551640476d0438
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407824"
---
# <a name="where-to-place-a-federation-server-proxy"></a>Où installer un serveur proxy de fédération

Vous pouvez placer Services ADFS \(AD FS\)serveurs proxy de Fédération dans un réseau de périmètre pour fournir une couche de protection contre les utilisateurs malveillants qui peuvent provenir d’Internet. Les serveurs proxy de fédération sont parfaits pour l’environnement de réseau de périmètre, car ils n’ont pas accès aux clés privées utilisées pour créer des jetons. Toutefois, les proxies de serveur de Fédération peuvent acheminer efficacement les demandes entrantes vers les serveurs de Fédération autorisés à produire ces jetons.  
  
Il n’est pas nécessaire de placer un serveur proxy de Fédération dans le réseau d’entreprise pour le partenaire de compte ou le partenaire de ressource, car les ordinateurs clients qui sont connectés au réseau d’entreprise peuvent communiquer directement avec le serveur de Fédération. Dans ce scénario, le serveur de Fédération fournit également la fonctionnalité de serveur proxy de Fédération pour les ordinateurs clients qui proviennent du réseau d’entreprise.  
  
Comme c’est généralement le cas avec les réseaux de périmètre, un pare-feu\-intranet est établi entre le réseau de périmètre et le réseau d’entreprise, et un pare-feu Internet\-est souvent établi entre le réseau de périmètre et Internet. Dans ce scénario, le serveur proxy de Fédération se situe entre ces deux pare-feu sur le réseau de périmètre.  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server-proxy"></a>Configuration de vos serveurs de pare-feu pour un serveur proxy de fédération  
Pour que le processus de redirection du serveur proxy de Fédération aboutisse, tous les serveurs de pare-feu doivent être configurés de façon à autoriser le trafic HTTPs (Hypertext Transfer Protocol) \(HTTPs\). L’utilisation du protocole HTTPs est nécessaire, car les serveurs de pare-feu doivent publier le serveur proxy de Fédération à l’aide du port 443, afin que le serveur proxy de Fédération dans le réseau de périmètre puisse accéder au serveur de Fédération du réseau d’entreprise.  
  
> [!NOTE]  
> Toutes les communications vers et à partir des ordinateurs clients se produisent également via HTTPS.  
  
En outre, le serveur de pare-feu Internet\-, tel qu’un ordinateur exécutant Microsoft Internet Security and Acceleration \(ISA\) Server, utilise un processus appelé publication de serveur pour distribuer les demandes des clients Internet au périmètre et aux serveurs réseau d’entreprise appropriés, tels que les serveurs proxys de Fédération ou les serveurs de Fédération.  
  
Les règles de publication de serveur déterminent le fonctionnement de la publication de serveur, qui consiste essentiellement dans le filtrage de toutes les demandes entrantes et sortantes via l’ordinateur ISA Server. Les règles de publication serveur mettent en correspondance les demandes clients avec les serveurs appropriés derrière l’ordinateur ISA Server. Pour plus d’informations sur la configuration d’ISA Server pour publier un serveur, consultez [créer une règle de publication Web sécurisée](https://go.microsoft.com/fwlink/?LinkId=75182).  
  
Dans le monde fédéré de AD FS, ces requêtes client sont généralement effectuées sur une URL spécifique, par exemple, une URL d’identificateur de serveur de Fédération telle que http :\//fs.fabrikam.com. Étant donné que ces demandes client proviennent d’Internet, le serveur pare-feu Internet\-doit être configuré pour publier l’URL de l’identificateur du serveur de Fédération pour chaque serveur proxy de Fédération déployé dans le réseau de périmètre.  
  
### <a name="configuring-isa-server-to-allow-ssl"></a>Configuration d’ISA Server pour autoriser SSL  
Pour faciliter la sécurisation des communications AD FS, vous devez configurer ISA Server de façon à autoriser les communications protocole SSL \(SSL\) entre les éléments suivants :  
  
-   **Serveurs de Fédération et proxys de serveur de Fédération.** Un canal SSL est requis pour toutes les communications entre les serveurs de Fédération et les serveurs proxys de Fédération. Par conséquent, vous devez configurer ISA Server de manière à autoriser une connexion SSL entre le réseau d’entreprise et le réseau de périmètre.  
  
-   **Les ordinateurs clients, les serveurs de Fédération et les serveurs proxys de Fédération.** Pour que les communications puissent se produire entre des ordinateurs clients et des serveurs de Fédération ou entre des ordinateurs clients et des serveurs proxys de Fédération, vous pouvez placer un ordinateur exécutant ISA Server devant le serveur de Fédération ou le serveur proxy de Fédération.  
  
    Si votre organisation exécute l’authentification client SSL sur le serveur de Fédération ou le serveur proxy de Fédération, lorsque vous placez un ordinateur exécutant ISA Server devant le serveur de Fédération ou le serveur proxy de Fédération, le serveur doit être configuré pour passer\-via la connexion SSL, car la connexion SSL doit se terminer au niveau du serveur de Fédération ou du serveur proxy de Fédération.  
  
    Si votre organisation n’effectue pas l’authentification du client SSL sur le serveur de Fédération ou le serveur proxy de Fédération, une autre option consiste à mettre fin à la connexion SSL sur l’ordinateur exécutant ISA Server, puis à\-établir une connexion SSL au serveur de Fédération ou au serveur proxy de Fédération.  
  
> [!NOTE]  
> Le serveur de Fédération ou le serveur proxy de Fédération exige que la connexion soit sécurisée par SSL pour protéger le contenu du jeton de sécurité.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
