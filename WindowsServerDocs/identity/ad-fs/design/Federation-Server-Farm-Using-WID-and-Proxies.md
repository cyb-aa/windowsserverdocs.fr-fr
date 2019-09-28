---
ms.assetid: f0464182-56a2-4bfa-a8c8-7e39c1bd62d3
title: Batterie de serveurs de fédération utilisant la base de données interne Windows et des proxys
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6a123afaebba002b8ee4fb98d5cee5aded286a96
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359134"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>Batterie de serveurs de fédération utilisant la base de données interne Windows et des proxys

Cette topologie de déploiement de Services ADFS \(AD FS @ no__t-1 est identique à la batterie de serveurs de Fédération avec la base de données interne Windows \(WID @ no__t-3, mais elle ajoute des ordinateurs proxy au réseau de périmètre pour prendre en charge utilisateurs externes. Ces proxies redirigent les demandes d’authentification du client qui proviennent de l’extérieur de votre réseau d’entreprise vers la batterie de serveurs de Fédération. Dans les versions précédentes de AD FS, ces proxies étaient appelés serveurs proxy de Fédération.  
  
> [!IMPORTANT]  
> Dans Services ADFS \(AD FS @ no__t-1 dans Windows Server 2012 R2, le rôle de serveur proxy de Fédération est géré par un nouveau service de rôle accès à distance appelé proxy d’application Web. Pour activer votre AD FS pour l’accessibilité depuis l’extérieur du réseau d’entreprise, ce qui était l’objectif du déploiement d’un serveur proxy de Fédération dans les versions héritées de AD FS, comme AD FS 2,0 et AD FS dans Windows Server 2012, vous pouvez déployer un ou plusieurs proxys d’application Web pour un D FS dans Windows Server 2012 R2.  
>   
> Dans le contexte de AD FS, le proxy d’application Web fonctionne comme un proxy de serveur de fédération AD FS. De plus, il fournit une fonctionnalité de proxy inverse aux applications web de votre réseau d’entreprise, permettant aux utilisateurs de n’importe quel appareil d’y accéder depuis l’extérieur du réseau d’entreprise. Pour plus d’informations sur le proxy d’application web, consultez Vue d’ensemble du proxy d’application web.  
>   
> Pour planifier le déploiement de proxy d'Application Web, vous pouvez consulter les informations contenues dans les rubriques suivantes :  
>   
> -   [Planifier l’infrastructure du proxy d’application Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx)  
> -   [Planifier le serveur proxy d’application Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
## <a name="deployment-considerations"></a>Considérations relatives au déploiement  
Cette section décrit les différentes considérations à prendre en compte concernant le public concerné, les avantages et les limitations associés à cette topologie de déploiement.  
  
### <a name="who-should-use-this-topology"></a>Qui doit utiliser cette topologie ?  
  
-   Les organisations avec 100 ou moins de relations d’approbation configurées qui doivent fournir à la fois leurs utilisateurs internes et les utilisateurs externes \(who sont connectées aux ordinateurs qui se trouvent physiquement en dehors du réseau d’entreprise @ no__t-1 avec l’authentification unique @ no__t-2On @no__t 3SSO @ no__t-4 accès aux applications ou services fédérés  
  
-   Les organisations qui doivent fournir à leurs utilisateurs internes et externes un accès SSO à Microsoft Office 365  
  
-   Organisations plus petites qui possèdent des utilisateurs externes et nécessitent des services évolutifs et évolutifs  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quels sont les avantages de l’utilisation de cette topologie ?  
  
-   Les mêmes avantages que ceux qui sont listés pour la [batterie de serveurs de Fédération à l’aide](Federation-Server-Farm-Using-WID.md) de la topologie wid, ainsi que l’avantage de fournir un accès supplémentaire pour les utilisateurs externes  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quelles sont les limitations de l’utilisation de cette topologie ?  
  
-   Les mêmes limitations que celles indiquées pour la [batterie de serveurs de Fédération à l’aide](Federation-Server-Farm-Using-WID.md) de la topologie wid  

||1 \- 100 confiances RP|Plus de 100 confiances RP 
| ----- |-----| ------ |
|1 \- 30 nœuds de AD FS|WID pris en charge|Non pris en charge \- avec SQL wid requis 
|Plus de 30 nœuds de AD FS|Non pris en charge \- avec SQL wid requis|Non pris en charge \- avec SQL wid requis  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recommandations relatives à l’emplacement du serveur et à la disposition du réseau  
Pour déployer cette topologie, en plus d’ajouter deux proxys d’application Web, vous devez vous assurer que votre réseau de périmètre peut également fournir l’accès à un serveur Domain Name System \(DNS @ no__t-1 et à un second hôte d’équilibrage de charge réseau \(NLB @ no__t-3. Le second hôte NLB doit être configuré avec un cluster NLB qui utilise une adresse IP de cluster Internet @ no__t-0accessible, et il doit utiliser le même paramètre de nom DNS de cluster que le cluster NLB précédent que vous avez configuré sur le réseau d’entreprise @no__t -1FS. fabrikam. com @ no__t-2. Les proxys d’application Web doivent également être configurés avec des adresses IP Internet @ no__t-0accessible.  
  
L’illustration suivante montre la batterie de serveurs de fédération existante avec la topologie WID décrite précédemment et comment la société fictive Fabrikam, Inc., la société fournit un accès à un serveur DNS de périmètre, ajoute un deuxième hôte NLB avec le même nom DNS de cluster @no__t -0fs. fabrikam. com @ no__t-1, et ajoute deux proxys d’application Web \(wap1 et WAP2 @ no__t-3 au réseau de périmètre.  
  
![Batterie de serveurs et proxys WID](media/WIDFarmADFSBlue.gif)  
  
Pour plus d’informations sur la configuration de votre environnement de mise en réseau pour une utilisation avec des serveurs de Fédération ou des proxys d’application Web, consultez la section « exigences relatives à la résolution de noms » dans [AD FS configuration requise](AD-FS-Requirements.md) et [planifier l’infrastructure du proxy d’application Web. (WAP)](https://technet.microsoft.com/library/dn383648.aspx).  
  
## <a name="see-also"></a>Voir aussi  
[Planifier votre topologie de déploiement d’AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guide de conception AD FS dans Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

