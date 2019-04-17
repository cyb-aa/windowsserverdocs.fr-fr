---
title: "Migrer l’agent web AD FS"
description: "Fournit des informations sur l’agent web AD FS vers Windows Server 2012."
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 945a5f4cf0e6c491479b095671ff5e77416c6fa3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-the-ad-fs-web-agent"></a>Migrer l’agent web AD FS

Pour migrer les services AD FS 1.1 agent basé sur les jetons Windows ou AD FS 1.1 prenant en charge les revendications agent qui est installé avec Windows Server 2008 R2 ou Windows Server 2008 vers Windows Server 2012, effectuer une mise à niveau sur place du système d’exploitation de l’ordinateur qui héberge l’agent pour Windows Server 2012. Pour plus d’informations, voir [l’installation de Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx). Aucune configuration supplémentaire n’est requise.  
  
> [!IMPORTANT]
>  Migré AD FS 1.1 Windows basée sur le jeton d’agent fonctionne uniquement avec un service de fédération 1.1 AD FS qui est installé avec Windows Server 2008 R2 ou Windows Server 2008. Pour plus d’informations, voir [il interagit avec AD FS 1.x](Interoperating-with-AD-FS-1.x.md).  
>   
>  L’objet d’une migration AD FS 1.1 prenant en charge les revendications agent web fonctionne avec les éléments suivants:  
>   
>  -   Service de fédération AD FS 1.1 installé avec Windows Server 2008 R2 ou Windows Server 2008  
> -   Service de fédération AD FS 2.0 installé sur Windows Server 2008 R2 ou Windows Server 2008  
> -   Service de fédération AD FS installé avec Windows Server 2012  
>   
>  Pour plus d’informations, voir [il interagit avec AD FS 1.x](Interoperating-with-AD-FS-1.x.md).  
  
  
## <a name="next-steps"></a>Étapes suivantes
 [Préparer la migration du serveur AD FS 2.0 de fédération](prepare-to-migrate-ad-fs-fed-server.md)   
 [Préparer la migration AD FS 2.0 serveur Proxy de fédération](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrer le serveur AD FS 2.0 de fédération](migrate-the-ad-fs-fed-server.md)   
 [Migrer le serveur Proxy AD FS 2.0 de fédération](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrer les Agents de 1.1 Web AD FS](migrate-the-ad-fs-web-agent.md)