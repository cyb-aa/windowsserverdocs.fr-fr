---
ms.assetid: f0464182-56a2-4bfa-a8c8-7e39c1bd62d3
title: Batterie de serveurs de fédération utilisant la base de données interne Windows et des proxys
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d49ae34d83d4a0b912bd92dbb9de16e18cc5b7ff
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191340"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>Batterie de serveurs de fédération utilisant la base de données interne Windows et des proxys

Cette topologie de déploiement d’Active Directory Federation Services \(AD FS\) est identique à la batterie de serveurs de fédération avec base de données interne Windows \(WID\) topologie, mais il ajoute des ordinateurs de proxy pour le réseau de périmètre pour prendre en charge des utilisateurs externes. Ces proxies rediriger les demandes d’authentification client provenant de l’extérieur de votre réseau d’entreprise à la batterie de serveurs de fédération. Dans les versions précédentes d’AD FS, ces proxys ont été appelées serveurs proxy de fédération.  
  
> [!IMPORTANT]  
> Dans Active Directory Federation Services \(AD FS\) dans Windows Server 2012 R2, le rôle d’un serveur proxy de fédération est géré par un nouveau service de rôle accès à distance appelé Proxy d’Application Web. Pour activer AD FS pour l’accessibilité depuis l’extérieur du réseau d’entreprise, ce qui était l’objectif du déploiement d’un serveur proxy de fédération dans les versions héritées d’AD FS, telles que AD FS 2.0 et AD FS dans Windows Server 2012, vous pouvez déployer un ou plusieurs proxys d’application web pour A FS D dans Windows Server 2012 R2.  
>   
> Dans le contexte des services AD FS, le Proxy d’Application Web fonctionne comme un serveur proxy de fédération AD FS. De plus, il fournit une fonctionnalité de proxy inverse aux applications web de votre réseau d’entreprise, permettant aux utilisateurs de n’importe quel appareil d’y accéder depuis l’extérieur du réseau d’entreprise. Pour plus d’informations sur le proxy d’application web, consultez Vue d’ensemble du proxy d’application web.  
>   
> Pour planifier le déploiement de proxy d'Application Web, vous pouvez consulter les informations contenues dans les rubriques suivantes :  
>   
> -   [Planifier l’Infrastructure de Proxy d’Application Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx)  
> -   [Planifier le serveur de Proxy d’Application Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
## <a name="deployment-considerations"></a>Considérations relatives au déploiement  
Cette section décrit divers points de vue sur le public visé, les avantages et les limites qui sont associés à cette topologie de déploiement.  
  
### <a name="who-should-use-this-topology"></a>Qui doit utiliser cette topologie ?  
  
-   Les organisations avec des relations d’approbation configurées 100 ou moins qui doivent fournir leurs utilisateurs internes et les utilisateurs externes \(qui sont connectés aux ordinateurs qui sont physiquement situés en dehors du réseau d’entreprise\) avec unique connexion\-sur \(SSO\) accès aux applications fédérées ou de services  
  
-   Organisations qui doivent fournir leurs utilisateurs internes et les utilisateurs externes avec accès par authentification unique à Microsoft Office 365  
  
-   Petites organisations qui ont des utilisateurs externes et nécessitent des services redondants, évolutif  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quels sont les avantages de l’utilisation de cette topologie ?  
  
-   Les mêmes avantages qu’apparaissent dans le [Federation Server batterie de serveurs à l’aide de WID](Federation-Server-Farm-Using-WID.md) topologie, ainsi que l’avantage de fournir un accès supplémentaire pour les utilisateurs externes  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quelles sont les limitations de l’utilisation de cette topologie ?  
  
-   Les mêmes limitations que répertoriées pour le [Federation Server batterie de serveurs à l’aide de WID](Federation-Server-Farm-Using-WID.md) topologie  

||1 \- 100 approbations de partie de confiance|Plus de 100 approbations de partie de confiance 
| ----- |-----| ------ |
|1 \- 30 AD FS nœuds|WID pris en charge|Non pris en charge à l’aide de WID \- SQL requis 
|Nœuds de plus de 30 AD FS|Non pris en charge à l’aide de WID \- SQL requis|Non pris en charge à l’aide de WID \- SQL requis  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recommandations de mise en page de positionnement et de réseau serveur  
Pour déployer cette topologie, outre l’ajout de deux web proxys d’application, vous devez vous assurer que votre réseau de périmètre permet également d’accéder à un système de nom de domaine \(DNS\) serveur et à un équilibrage de charge réseau deuxième \( NLB\) hôte. Le second hôte NLB doit être configuré avec un cluster NLB qui utilise un Internet\-adresse IP du cluster accessible et il doivent utiliser le même paramètre de nom DNS de cluster en tant que le précédent cluster NLB que vous avez configuré sur le réseau d’entreprise \( FS.fabrikam.com\). Les proxys d’application web doivent également être configurés avec Internet\-des adresses IP accessibles.  
  
L’illustration suivante montre la batterie de serveurs de fédération existante avec topologie WID qui a été décrit précédemment et comment la société fictive Fabrikam, Inc., fournit l’accès à un serveur DNS de périmètre, ajoute un deuxième hôte NLB avec le nom DNS du même cluster \(fs.fabrikam.com\)et ajoute deux web proxys d’application \(wap1 et wap2\) au réseau de périmètre.  
  
![Proxys et WID de batterie de serveurs](media/WIDFarmADFSBlue.gif)  
  
Pour plus d’informations sur la configuration de votre environnement réseau pour une utilisation avec les serveurs de fédération ou de proxys d’application web, consultez « Exigences de résolution de nom » section [configuration AD FS requise](AD-FS-Requirements.md) et [planifier le Web Infrastructure du Proxy d’application (WAP)](https://technet.microsoft.com/library/dn383648.aspx).  
  
## <a name="see-also"></a>Voir aussi  
[Planifier votre topologie de déploiement d’AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guide de conception AD FS dans Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

