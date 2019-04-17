---
ms.assetid: 935ea7c2-4678-4033-b50f-2036a0359c5d
title: "Où placer un serveur de fédération"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 376cec7f3a4fb1f988ac5d458b05220c7b9de970
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="where-to-place-a-federation-server"></a>Où placer un serveur de fédération

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Sécurité des meilleures pratiques, placer des serveurs de fédération Active Directory Federation Services \(AD FS\) devant un pare-feu et les connecter à votre réseau d’entreprise pour éviter l’exposition à partir d’Internet. Cela est important, car les serveurs de fédération pour octroyer des jetons de sécurité. Par conséquent, ils doivent avoir le même niveau de protection comme contrôleur de domaine. Si un serveur de fédération est compromis, un utilisateur malveillant a la possibilité d’émettre des jetons d’accès complet à toutes les applications Web et aux serveurs de fédération qui sont protégées par \(AD FS\) Active Directory Federation Services dans toutes les organisations partenaires de ressource.  
  
> [!NOTE]  
> Une sécurité optimale, évitez d’avoir vos serveurs de fédération directement accessibles sur Internet. Autorisez vos serveurs de fédération accès direct à Internet uniquement lors de la configuration d’un environnement de laboratoire de test ou lorsque votre organisation ne dispose pas d’un réseau de périmètre.  
  
Pour les réseaux d’entreprise, un pare-feu intranet\ est établi entre le réseau d’entreprise et le réseau de périmètre, et un pare-feu Internet est souvent établi entre le réseau de périmètre et Internet. Dans ce cas, le serveur de fédération qui se trouve à l’intérieur du réseau d’entreprise, et il n’est pas directement accessible par les clients Internet.  
  
> [!NOTE]  
> Les ordinateurs clients qui sont connectés au réseau d’entreprise peuvent communiquer directement avec le serveur de fédération via l’authentification Windows intégrée.  
  
Un serveur proxy de fédération doit être placé dans le réseau de périmètre avant de configurer vos serveurs pare-feu pour une utilisation avec AD FS. Pour plus d’informations, voir [où placer un serveur Proxy de fédération](Where-to-Place-a-Federation-Server-Proxy.md).  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server"></a>Configuration de vos serveurs pare-feu pour un serveur de fédération  
Afin que les serveurs de fédération peuvent communiquer directement avec les serveurs proxy de fédération, le serveur pare-feu d’intranet doit être configuré pour autoriser le trafic de Secure Hypertext Transfer Protocol \(HTTPS\) le serveur proxy de fédération vers le serveur de fédération. Cela est obligatoire, car le serveur pare-feu d’intranet doit publier le serveur de fédération à l’aide du port 443 afin que le serveur proxy de fédération dans le réseau de périmètre peut accéder au serveur de fédération.  
  
En outre, le pare-feu intranet\ côté serveur, tel qu’un serveur exécutant Internet Security and Acceleration \(ISA\) serveur, utilise un processus appelé publication de serveur pour distribuer les demandes des clients Internet pour les serveurs de fédération d’entreprise approprié. Cela signifie que vous devez créer manuellement une règle de publication de serveur sur le serveur intranet exécutant ISA Server qui publie l’URL du serveur de fédération en cluster, par exemple, http:///\/fs.fabrikam.com.  
  
Pour plus d’informations sur la façon de configurer la publication de serveur dans un réseau de périmètre, voir [où placer un serveur Proxy de fédération](Where-to-Place-a-Federation-Server-Proxy.md). Pour plus d’informations sur comment configurer ISA Server pour publier un serveur, consultez [créer une règle de publication Web sécurisée](https://go.microsoft.com/fwlink/?LinkId=75182).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
