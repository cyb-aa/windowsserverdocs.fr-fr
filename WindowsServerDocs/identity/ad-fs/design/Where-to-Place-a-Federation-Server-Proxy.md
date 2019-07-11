---
ms.assetid: ec26705c-4446-4226-b9b4-b775b642f0f4
title: Où installer un serveur proxy de fédération
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 03792d7ae5fec3c209cf1abfaa7af3fdfdb75f08
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67792273"
---
# <a name="where-to-place-a-federation-server-proxy"></a>Où installer un serveur proxy de fédération

Vous pouvez placer des Services de fédération Active Directory \(AD FS\)proxys serveur de fédération dans un réseau de périmètre pour fournir une couche de protection contre les utilisateurs malveillants pouvant provenir d’Internet. Les serveurs proxy de fédération sont parfaits pour l’environnement de réseau de périmètre, car ils n’ont pas accès aux clés privées utilisées pour créer des jetons. Toutefois, serveurs proxy de fédération peut router efficacement des demandes entrantes vers les serveurs de fédération qui sont autorisés à produire ces jetons.  
  
Il n’est pas nécessaire de placer un serveur proxy de fédération à l’intérieur du réseau d’entreprise pour le partenaire de compte ou le partenaire de ressource, car les ordinateurs clients qui sont connectés au réseau d’entreprise peuvent communiquer directement avec le serveur de fédération. Dans ce scénario, le serveur de fédération fournit également des fonctionnalités de proxy de serveur de fédération pour les ordinateurs clients qui seront disponibles à partir du réseau d’entreprise.  
  
Comme c’est généralement avec les réseaux de périmètre, un intranet\-accessible sur le pare-feu est établie entre le réseau de périmètre et le réseau d’entreprise et un Internet\-accessible sur le pare-feu est souvent établi entre le réseau de périmètre et le Internet. Dans ce scénario, le serveur proxy de fédération se situe entre les deux de ces pare-feux sur le réseau de périmètre.  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server-proxy"></a>Configuration de vos serveurs de pare-feu pour un serveur proxy de fédération  
Pour le processus federation server proxy redirection réussisse, tous les serveurs de pare-feu doivent être configurés pour autoriser Secure Hypertext Transfer Protocol \(HTTPS\) le trafic. L’utilisation du protocole HTTPS est requise, car les serveurs de pare-feu doivent publier le serveur proxy de fédération, via le port 443, afin que le serveur proxy de fédération dans le réseau de périmètre peut accéder au serveur de fédération dans le réseau d’entreprise.  
  
> [!NOTE]  
> Toutes les communications vers et à partir des ordinateurs clients se produisent également via HTTPS.  
  
En outre, Internet\-accessible sur le serveur pare-feu, comme un ordinateur exécutant Microsoft Internet Security and Acceleration \(ISA\) serveur, utilise un processus appelé publication de serveur pour distribuer le client Internet requêtes pour les serveurs du réseau d’entreprise, telles que les serveurs proxy de fédération ou de serveurs de fédération et de périmètre appropriés.  
  
Les règles de publication de serveur déterminent le fonctionnement de la publication de serveur, qui consiste essentiellement dans le filtrage de toutes les demandes entrantes et sortantes via l’ordinateur ISA Server. Les règles de publication serveur mettent en correspondance les demandes clients avec les serveurs appropriés derrière l’ordinateur ISA Server. Pour plus d’informations sur la configuration d’ISA Server pour publier un serveur, consultez [créer une règle de publication Web sécurisée](https://go.microsoft.com/fwlink/?LinkId=75182).  
  
Dans le monde fédéré des services AD FS, les demandes client sont envoyées à une URL spécifique, par exemple, une identificateur URL du serveur de fédération tels que http :\//fs.fabrikam.com. Étant donné que les demandes client proviennent d’à partir d’Internet, Internet\-accessible sur le serveur pare-feu doit être configuré pour publier l’identificateur URL du serveur de fédération pour chaque serveur proxy de fédération qui est déployé dans le réseau de périmètre.  
  
### <a name="configuring-isa-server-to-allow-ssl"></a>Configuration d’ISA Server pour autoriser SSL  
Pour faciliter des communications sécurisées AD FS, vous devez configurer ISA Server pour autoriser le protocole SSL \(SSL\) communications entre les éléments suivants :  
  
-   **Serveurs de fédération et les serveurs proxy de fédération.** Un canal SSL est requis pour toutes les communications entre les serveurs de fédération et les serveurs proxy de fédération. Par conséquent, vous devez configurer ISA Server de manière à autoriser une connexion SSL entre le réseau d’entreprise et le réseau de périmètre.  
  
-   **Les ordinateurs clients, les serveurs de fédération et les serveurs proxy de fédération.** Afin que les communications peut se produire entre les ordinateurs clients et serveurs de fédération ou entre les ordinateurs clients et serveurs proxy de fédération, vous pouvez placer un ordinateur exécutant ISA Server devant le serveur de fédération ou le serveur proxy de fédération.  
  
    Si votre organisation exécute l’authentification client SSL sur le serveur de fédération ou le serveur proxy de fédération, lorsque vous placez un ordinateur exécutant ISA Server devant le serveur de fédération ou le serveur proxy de fédération, le serveur doit être configuré pour pass\-par le biais de la connexion SSL, car la connexion SSL doit se terminer au serveur de fédération ou serveur proxy de fédération.  
  
    Si votre organisation n’effectue pas l’authentification du client SSL sur le serveur de fédération ou le serveur proxy de fédération, une autre option consiste à mettre fin à la connexion SSL sur l’ordinateur exécutant ISA Server, puis réinstallés\-établir une connexion SSL au le serveur de fédération ou un serveur proxy de fédération.  
  
> [!NOTE]  
> Le serveur de fédération ou le serveur proxy de fédération requiert que la connexion sécurisée par SSL pour protéger le contenu du jeton de sécurité.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
