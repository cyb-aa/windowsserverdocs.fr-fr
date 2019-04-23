---
ms.assetid: 935ea7c2-4678-4033-b50f-2036a0359c5d
title: Où placer un serveur de fédération
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 376cec7f3a4fb1f988ac5d458b05220c7b9de970
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857690"
---
# <a name="where-to-place-a-federation-server"></a>Où placer un serveur de fédération

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Comme les raisons de sécurité, place les Services de fédération Active Directory \(AD FS\)devant un pare-feu, les serveurs de fédération et les connecter à votre réseau d’entreprise pour empêcher l’exposition à partir d’Internet. Ceci est important, car les serveurs de fédération sont autorisés à octroyer des jetons de sécurité. Par conséquent, ils doivent avoir le même niveau de protection qu’un contrôleur de domaine. Si un serveur de fédération est compromis, un utilisateur malveillant a la possibilité d’émettre des jetons d’accès complet à toutes les applications Web et aux serveurs de fédération qui sont protégés par Active Directory Federation Services \(AD FS\) dans toutes les ressources organisations partenaires.  
  
> [!NOTE]  
> En tant qu’une sécurité optimale, évitez d’avoir vos serveurs de fédération directement accessibles sur Internet. Donnez vos serveurs de fédération un accès Internet direct uniquement lors de la configuration d’un environnement de laboratoire de test ou lorsque votre organisation ne dispose pas d’un réseau de périmètre.  
  
Pour les réseaux d’entreprise, un intranet\-accessible sur le pare-feu est établie entre le réseau d’entreprise et le réseau de périmètre et un Internet\-accessible sur le pare-feu est souvent établi entre le réseau de périmètre et le Internet. Dans ce cas, le serveur de fédération se trouve à l’intérieur du réseau d’entreprise, et il n’est pas directement accessible par les clients Internet.  
  
> [!NOTE]  
> Les ordinateurs clients qui sont connectés au réseau d’entreprise peuvent communiquer directement avec le serveur de fédération via l’authentification intégrée Windows.  
  
Un serveur proxy de fédération doit être placé dans le réseau de périmètre avant de configurer vos serveurs pare-feu pour une utilisation avec AD FS. Pour plus d'informations, voir [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md).  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server"></a>Configuration de vos serveurs pare-feu pour un serveur de fédération  
Afin que les serveurs de fédération peuvent communiquer directement avec les serveurs proxy de fédération, le serveur pare-feu d’intranet doit être configuré pour autoriser Secure Hypertext Transfer Protocol \(HTTPS\) le trafic en provenance du serveur proxy de fédération pour le serveur de fédération. Il s’agit d’une exigence, car le serveur pare-feu d’intranet doit publier le serveur de fédération à l’aide du port 443 afin que le serveur proxy de fédération dans le réseau de périmètre peut accéder au serveur de fédération.  
  
En outre, l’intranet\-accessible sur le serveur pare-feu, tel qu’un serveur exécutant Internet Security and Acceleration \(ISA\) serveur, utilise un processus appelé publication de serveur pour distribuer des demandes des clients Internet vers le serveurs de fédération d’entreprise approprié. Cela signifie que vous devez créer manuellement une règle de publication de serveur sur le serveur intranet exécutant ISA Server qui publie l’URL du serveur de fédération en cluster, par exemple, http :\/\/fs.fabrikam.com.  
  
Pour plus d'informations sur la configuration de la publication de serveur dans un réseau de périmètre, consultez [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md). Pour plus d’informations sur la configuration d’ISA Server pour publier un serveur, consultez [créer une règle de publication Web sécurisée](https://go.microsoft.com/fwlink/?LinkId=75182).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
