---
title: Migrer l’agent Web AD FS
description: Fournit des informations sur l’agent Web AD FS à Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: ddba9668b54e325dae6dfc0cf67d50d3ae5d90be
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408194"
---
# <a name="migrate-the-ad-fs-web-agent"></a>Migrer l’agent Web AD FS

Pour migrer l’agent basé sur un jeton Windows AD FS 1,1 ou l’agent prenant en charge les revendications AD FS 1,1 qui est installé avec Windows Server 2008 R2 ou Windows Server 2008 vers Windows Server 2012, effectuez une mise à niveau sur place du système d’exploitation de l’ordinateur qui héberge l’un ou l’autre agent vers Windows Server 2012. Pour plus d’informations, voir [Installation de Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx). Aucune autre configuration n’est demandée.  
  
> [!IMPORTANT]
>  L’agent basé sur les jetons Windows AD FS 1.1 ayant fait l’objet de la migration fonctionne uniquement avec un service de fédération AD FS 1.1 installé avec Windows Server 2008 R2 ou Windows Server 2008. Pour plus d'informations, voir [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md).  
> 
>  L’agent web prenant en charge les revendications AD FS 1.1 ayant fait l’objet de la migration fonctionne avec les services suivants :  
> 
> - Service de fédération AD FS 1.1 installé avec Windows Server 2008 R2 ou Windows Server 2008  
>   -   Service de fédération AD FS 2.0 installé avec Windows Server 2008 R2 ou Windows Server 2008  
>   -   Service de fédération AD FS installé avec Windows Server 2012  
> 
>   Pour plus d'informations, voir [Interoperating with AD FS 1.x](Interoperating-with-AD-FS-1.x.md).  
  
  
## <a name="next-steps"></a>Étapes suivantes
 [Préparer la migration du serveur de fédération AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Préparer la migration du serveur proxy de fédération AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrer le serveur de fédération AD FS 2,0](migrate-the-ad-fs-fed-server.md)   
 [Migrer le serveur proxy de fédération AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrer les agents web AD FS 1.1](migrate-the-ad-fs-web-agent.md)