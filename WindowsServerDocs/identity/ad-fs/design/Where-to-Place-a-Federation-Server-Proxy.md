---
ms.assetid: ec26705c-4446-4226-b9b4-b775b642f0f4
title: "Où placer un serveur Proxy de fédération"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4bde30f694c6490962edaa0c3fe1543e74ba7fd7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="where-to-place-a-federation-server-proxy"></a>Où placer un serveur Proxy de fédération

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Vous pouvez placer des serveurs proxy de fédération Active Directory Federation Services \(AD FS\) dans un réseau de périmètre pour fournir une couche de protection contre les utilisateurs malveillants pouvant provenir d’Internet. Serveurs proxy de fédération est idéales pour l’environnement réseau de périmètre, car ils n’ont pas accès aux clés privées utilisées pour créer des jetons. Toutefois, serveurs proxy de fédération permettre acheminer de manière efficace les demandes entrantes aux serveurs de fédération qui sont autorisés à produire ces jetons.  
  
Il n’est pas nécessaire de placer un serveur proxy de fédération à l’intérieur du réseau d’entreprise pour le partenaire de compte ou le partenaire de ressource dans la mesure où les ordinateurs clients qui sont connectés au réseau d’entreprise peuvent communiquer directement avec le serveur de fédération. Dans ce scénario, le serveur de fédération fournit également des fonctionnalités de proxy de serveur de fédération pour les ordinateurs clients qui seront bientôt disponibles à partir du réseau d’entreprise.  
  
Comme cela est courant avec les réseaux de périmètre, un pare-feu intranet\ est établi entre le réseau de périmètre et le réseau d’entreprise, et un pare-feu Internet est souvent établi entre le réseau de périmètre et Internet. Dans ce scénario, le serveur proxy de fédération se situe entre ces deux pare-feu sur le réseau de périmètre.  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server-proxy"></a>Configuration de vos serveurs de pare-feu pour un serveur proxy de fédération  
Pour le processus federation server proxy redirection aboutisse, tous les serveurs de pare-feu doivent être configurés pour autoriser le trafic \(HTTPS\) Secure Hypertext Transfer Protocol. L’utilisation du protocole HTTPS est requis car les serveurs de pare-feu doivent publier le serveur proxy de fédération, via le port 443, afin que le serveur proxy de fédération dans le réseau de périmètre peut accéder au serveur de fédération du réseau d’entreprise.  
  
> [!NOTE]  
> Toutes les communications vers et depuis les ordinateurs clients se produisent également via HTTPS.  
  
En outre, du pare-feu Internet côté serveur, tel qu’un ordinateur exécutant Microsoft Internet Security and Acceleration \(ISA\) serveur, utilise un processus appelé publication de serveur pour distribuer les demandes des clients Internet vers les serveurs du réseau d’entreprise, telles que les serveurs proxy de fédération ou de serveurs de fédération et de périmètre appropriés.  
  
Les règles de publication de serveur déterminent le fonctionne de la publication de serveur: filtrage essentiellement, toutes les demandes entrantes et sortantes via l’ordinateur ISA Server. Règles de publication serveur mappent les demandes de client pour les serveurs appropriés derrière l’ordinateur ISA Server. Pour plus d’informations sur comment configurer ISA Server pour publier un serveur, consultez [créer une règle de publication Web sécurisée](https://go.microsoft.com/fwlink/?LinkId=75182).  
  
Dans le monde fédéré des services ADFS, les demandes client sont faites à une URL spécifique, par exemple, une fédération identificateur URL de serveur tels que http://fs.fabrikam.com. Étant donné que les demandes client proviennent d’à partir d’Internet, le serveur de pare-feu côté Internet doit être configuré pour publier l’identificateur URL du serveur de fédération pour chaque serveur proxy de fédération qui est déployé dans le réseau de périmètre.  
  
### <a name="configuring-isa-server-to-allow-ssl"></a>Configuration d’ISA Server pour autoriser SSL  
Pour faciliter la sécurisation des communications AD FS, vous devez configurer ISA Server pour autoriser les communications Secure Sockets Layer \(SSL\) entre les éléments suivants:  
  
-   **Serveurs de fédération et les serveurs proxy de fédération.** Un canal SSL est requis pour toutes les communications entre les serveurs de fédération et les serveurs proxy de fédération. Par conséquent, vous devez configurer ISA Server pour autoriser une connexion SSL entre le réseau d’entreprise et le réseau de périmètre.  
  
-   **Les ordinateurs clients, les serveurs de fédération et les serveurs proxy de fédération.** Afin que les communications peut se produire entre les ordinateurs clients et serveurs de fédération ou entre les ordinateurs clients et serveurs proxy de fédération, vous pouvez placer un ordinateur exécutant ISA Server devant le serveur de fédération ou un serveur proxy de fédération.  
  
    Si votre organisation effectue une authentification de client SSL sur le serveur de fédération ou un serveur proxy de fédération, lorsque vous placez un ordinateur exécutant ISA Server devant le serveur de fédération ou un serveur proxy de fédération, le serveur doit être configuré pour pass\-par le biais de la connexion SSL, car la connexion SSL doit se terminer au serveur de fédération ou serveur proxy de fédération.  
  
    Si votre organisation n’effectue pas d’authentification de client SSL sur le serveur de fédération ou un serveur proxy de fédération, une autre option consiste à mettre fin à la connexion SSL sur l’ordinateur exécutant ISA Server, puis re\-établir une connexion SSL avec le serveur de fédération ou un serveur proxy de fédération.  
  
> [!NOTE]  
> Le serveur de fédération ou un serveur proxy de fédération nécessite que la connexion sécurisée par SSL pour protéger le contenu du jeton de sécurité.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
