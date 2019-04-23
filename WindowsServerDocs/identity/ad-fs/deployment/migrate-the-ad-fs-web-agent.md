---
title: Migrer l’agent web ADFS
description: Fournit des informations sur l’agent web AD FS vers Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 945a5f4cf0e6c491479b095671ff5e77416c6fa3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877590"
---
# <a name="migrate-the-ad-fs-web-agent"></a>Migrer l’agent web ADFS

Pour migrer les services AD FS 1.1 agent à l’aide de jeton Windows ou l’AD FS 1.1 prenant en charge les revendications agent qui est installé avec Windows Server 2008 R2 ou Windows Server 2008 vers Windows Server 2012, effectuer une mise à niveau sur place du système d’exploitation de l’ordinateur qui héberge l’agent vers Windows Server 2012. Pour plus d’informations, voir [Installation de Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx). Aucune autre configuration n’est demandée.  
  
> [!IMPORTANT]
>  L’agent basé sur les jetons Windows AD FS 1.1 ayant fait l’objet de la migration fonctionne uniquement avec un service de fédération AD FS 1.1 installé avec Windows Server 2008 R2 ou Windows Server 2008. Pour plus d'informations, voir [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md).  
>   
>  L’agent web prenant en charge les revendications AD FS 1.1 ayant fait l’objet de la migration fonctionne avec les services suivants :  
>   
>  -   Service de fédération AD FS 1.1 installé avec Windows Server 2008 R2 ou Windows Server 2008  
> -   Service de fédération AD FS 2.0 installé avec Windows Server 2008 R2 ou Windows Server 2008  
> -   Service de fédération AD FS installé avec Windows Server 2012  
>   
>  Pour plus d'informations, voir [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md).  
  
  
## <a name="next-steps"></a>Étapes suivantes
 [Préparer la migration du serveur AD FS 2.0 de fédération](prepare-to-migrate-ad-fs-fed-server.md)   
 [Préparer la migration du serveur Proxy pour AD FS 2.0 de fédération](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrer le serveur AD FS 2.0 de fédération](migrate-the-ad-fs-fed-server.md)   
 [Migrer le serveur Proxy AD FS 2.0 de fédération](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrer les Agents de 1.1 Web AD FS](migrate-the-ad-fs-web-agent.md)