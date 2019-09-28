---
ms.assetid: 935ea7c2-4678-4033-b50f-2036a0359c5d
title: Où placer un serveur de fédération
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: c64b28f0e62839ff771cd50c0f09b534861cf9e2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358777"
---
# <a name="where-to-place-a-federation-server"></a>Où placer un serveur de fédération

En guise de meilleure pratique de sécurité, placez Services ADFS serveurs \(AD FS @ no__t-1federation devant un pare-feu et connectez-les à votre réseau d’entreprise pour empêcher l’exposition à Internet. Cela est important, car les serveurs de Fédération ont une autorisation complète pour accorder des jetons de sécurité. Par conséquent, ils doivent avoir le même niveau de protection qu’un contrôleur de domaine. Si un serveur de Fédération est compromis, un utilisateur malveillant a la possibilité d’émettre des jetons d’accès complets à toutes les applications Web et à des serveurs de Fédération protégés par Services ADFS \(AD FS @ no__t-1 dans tous les partenaires de ressources lucratif.  
  
> [!NOTE]  
> Pour des raisons de sécurité, évitez d’accéder directement à vos serveurs de Fédération sur Internet. Envisagez de donner à vos serveurs de Fédération un accès direct à Internet uniquement lorsque vous configurez un environnement de laboratoire de test ou que votre organisation ne dispose pas d’un réseau de périmètre.  
  
Pour les réseaux d’entreprise types, un pare-feu intranet @ no__t-0facing est établi entre le réseau d’entreprise et le réseau de périmètre, et un pare-feu Internet @ no__t-1facing est souvent établi entre le réseau de périmètre et Internet. Dans ce cas, le serveur de Fédération se trouve dans le réseau d’entreprise et n’est pas directement accessible par les clients Internet.  
  
> [!NOTE]  
> Les ordinateurs clients qui sont connectés au réseau d’entreprise peuvent communiquer directement avec le serveur de Fédération par le biais de l’authentification intégrée de Windows.  
  
Un proxy de serveur de Fédération doit être placé dans le réseau de périmètre avant de configurer vos serveurs pare-feu pour une utilisation avec AD FS. Pour plus d'informations, voir [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md).  
  
## <a name="configuring-your-firewall-servers-for-a-federation-server"></a>Configuration de vos serveurs pare-feu pour un serveur de fédération  
Pour que les serveurs de Fédération puissent communiquer directement avec les serveurs proxys de Fédération, le serveur pare-feu d’intranet doit être configuré pour @no__t autoriser le trafic 0HTTPS @ no__t-1 à partir du serveur proxy de Fédération vers le serveur de Fédération. Il s’agit d’une condition requise car le serveur pare-feu d’intranet doit publier le serveur de Fédération à l’aide du port 443 afin que le serveur proxy de Fédération dans le réseau de périmètre puisse accéder au serveur de Fédération.  
  
En outre, le serveur de pare-feu intranet @ no__t-0facing, tel qu’un serveur exécutant Internet Security and Acceleration \(ISA @ no__t-2 Server, utilise un processus appelé publication de serveur pour distribuer les demandes des clients Internet à l’entreprise appropriée. serveurs de Fédération. Cela signifie que vous devez créer manuellement une règle de publication de serveur sur le serveur intranet exécutant ISA Server, qui publie l’URL du serveur de Fédération en cluster, par exemple, http : @no__t -0\/fs.fabrikam.com.  
  
Pour plus d'informations sur la configuration de la publication de serveur dans un réseau de périmètre, consultez [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md). Pour plus d’informations sur la configuration d’ISA Server pour publier un serveur, consultez [créer une règle de publication Web sécurisée](https://go.microsoft.com/fwlink/?LinkId=75182).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
