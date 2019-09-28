---
ms.assetid: f775cbda-a75d-439d-9aa7-82f3bc8dc932
title: Batterie de serveurs de fédération utilisant la base de données interne Windows
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b0a84940018a0e71aaa1b47c7af3aba5966fe0ae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408059"
---
# <a name="federation-server-farm-using-wid"></a>Batterie de serveurs de fédération utilisant la base de données interne Windows

La topologie par défaut pour Services ADFS \(AD FS @ no__t-1 est une batterie de serveurs de Fédération, à l’aide de la base de données interne Windows \(WID @ no__t-3. Dans cette topologie, AD FS utilise WID comme magasin pour la base de données de configuration AD FS pour tous les serveurs de Fédération joints à cette batterie. La batterie réplique et maintient les données du service de fédération de la base de données de configuration à travers chaque serveur de la batterie. AD FS dans Windows Server 2012 R2 permet aux organisations avec 100 ou moins d’approbations de partie de confiance de configurer des batteries de serveurs de Fédération à l’aide de WID avec un maximum de 30 serveurs.  
  
La création du premier serveur de fédération d’une batterie consiste aussi à créer un nouveau service de fédération. Lorsque vous utilisez WID pour la base de données de configuration AD FS, le premier serveur de Fédération que vous créez dans la batterie est appelé *serveur de Fédération principal*. Cela signifie que cet ordinateur est configuré avec une copie\/en lecture/écriture de la base de données de configuration AD FS.  
  
Tous les autres serveurs de Fédération configurés pour cette batterie sont désignés sous le terme de *serveurs de Fédération secondaires* , car ils doivent répliquer les modifications apportées sur le\-serveur de Fédération principal vers les copies en lecture seule du AD FS base de données de configuration qu’ils stockent localement.  
  
> [!IMPORTANT]  
> Nous vous recommandons d’utiliser au moins deux serveurs de Fédération dans une\-configuration à charge équilibrée.  
  
## <a name="deployment-considerations"></a>Considérations relatives au déploiement  
Cette section décrit les différentes considérations à prendre en compte concernant le public concerné, les avantages et les limitations associés à cette topologie de déploiement.  
  
### <a name="who-should-use-this-topology"></a>Qui doit utiliser cette topologie ?  
  
-   Organisations avec 100 ou moins de relations d’approbation configurées qui doivent fournir à \(leurs utilisateurs internes une connexion à des ordinateurs connectés physiquement au réseau\) d’entreprise à\-l’aide de l’authentification \(uniqueAccès\) SSO à des applications ou services fédérés  
  
-   Les organisations qui souhaitent fournir à leurs utilisateurs internes un accès SSO à Microsoft Online Services ou Microsoft Office 365  
  
-   Organisations plus petites qui requièrent des services redondants et évolutifs  
  
> [!NOTE]  
> Les organisations avec des bases de données plus volumineuses doivent envisager d’utiliser la [batterie de serveurs de Fédération à l’aide](Federation-Server-Farm-Using-SQL-Server.md) de la topologie de déploiement SQL Server. Les organisations avec des utilisateurs qui se connectent depuis l’extérieur du réseau doivent envisager d’utiliser soit la [batterie de serveurs de Fédération à l’aide de la topologie wid et proxys,](Federation-Server-Farm-Using-WID-and-Proxies.md) soit la [batterie de serveurs de Fédération à l’aide](Federation-Server-Farm-Using-SQL-Server.md) de la topologie de SQL Server.  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quels sont les avantages de l’utilisation de cette topologie ?  
  
-   Fournit un accès SSO aux utilisateurs internes  
  
-   Redondance des données et des \(service FS (Federation Service) chaque serveur de Fédération réplique les modifications sur les autres serveurs de Fédération de la même batterie\)  
  
-   WID est inclus dans Windows. par conséquent, il n’est pas nécessaire d’acheter SQL Server  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quelles sont les limitations de l’utilisation de cette topologie ?  
  
-   Une batterie de serveurs WID a une limite de 30 serveurs de Fédération si vous avez 100 ou moins d’approbations de partie de confiance.  
  
-   Une batterie de serveurs wid ne prend pas en charge la détection de \(relecture de jetons ou\) la\)résolution d’artefacts du protocole SAML Security Assertion Markup Language \(.  
  
Le tableau suivant fournit un résumé de l’utilisation d’une batterie de serveurs WID.  Utilisez-le pour planifier votre implémentation.  
  
|| 1 \- 100 confiances RP | Plus de 100 confiances RP |
| --- | --- | --- |
|1 \- 30 nœuds de AD FS|WID pris en charge|Non pris en charge avec WID-SQL requis 
|Plus de 30 nœuds de AD FS|Non pris en charge avec WID-SQL requis|Non pris en charge avec WID-SQL requis  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recommandations relatives à l’emplacement du serveur et à la disposition du réseau  
Lorsque vous êtes prêt à commencer le déploiement de cette topologie sur votre réseau, vous devez prévoir de placer tous les serveurs de Fédération de votre réseau d’entreprise derrière un hôte NLB \(\) d’équilibrage de charge réseau qui peut être configuré pour un cluster NLB. avec un nom DNS \(\) et une adresse IP de cluster du système de noms de domaine de cluster dédiés.  
  
> [!NOTE]  
> Ce nom DNS de cluster doit correspondre au nom de service FS (Federation Service), par exemple fs.fabrikam.com.  
  
L’hôte NLB peut utiliser les paramètres définis dans ce cluster NLB pour allouer des demandes du client aux serveurs de Fédération individuels. L’illustration suivante montre comment la société fictive Fabrikam, Inc., définit la première phase de son déploiement à l’aide d’une\-batterie \(de serveurs de Fédération d’ordinateurs\) FS1 et de FS2 avec wid et le positionnement d’un serveur DNS et un seul hôte NLB connecté au réseau d’entreprise.  
  
![batterie de serveurs utilisant WID](media/FarmWID.gif)  
  
> [!NOTE]  
> En cas de défaillance de cet hôte NLB unique, les utilisateurs ne pourront pas accéder aux applications ou services fédérés. Ajoutez de nouveaux hôtes NLB si vos contraintes d’entreprise ne vous autorisent pas à avoir un seul point de défaillance.  
  
Pour plus d’informations sur la configuration de votre environnement de mise en réseau pour une utilisation avec des serveurs de Fédération, consultez la section Configuration requise pour la résolution de noms dans [AD FS configuration requise](AD-FS-Requirements.md).  
  
## <a name="see-also"></a>Voir aussi  
[Planifier votre topologie de déploiement d’AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guide de conception AD FS dans Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

