---
ms.assetid: f775cbda-a75d-439d-9aa7-82f3bc8dc932
title: Batterie de serveurs de fédération utilisant la base de données interne Windows
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 926848f9d39a4e00cb30a6bbde5aecee0ef31043
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853082"
---
# <a name="federation-server-farm-using-wid"></a>Batterie de serveurs de fédération utilisant la base de données interne Windows

La topologie par défaut pour Services ADFS \(AD FS\) est une batterie de serveurs de Fédération, à l’aide de la \(de base de données interne Windows\)WID. Dans cette topologie, AD FS utilise WID comme magasin pour la base de données de configuration AD FS pour tous les serveurs de Fédération joints à cette batterie. La batterie réplique et maintient les données du service de fédération de la base de données de configuration à travers chaque serveur de la batterie. AD FS dans Windows Server 2012 R2 permet aux organisations avec 100 ou moins d’approbations de partie de confiance de configurer des batteries de serveurs de Fédération à l’aide de WID avec un maximum de 30 serveurs.  
  
La création du premier serveur de fédération d’une batterie consiste aussi à créer un nouveau service de fédération. Lorsque vous utilisez WID pour la base de données de configuration AD FS, le premier serveur de Fédération que vous créez dans la batterie est appelé *serveur de Fédération principal*. Cela signifie que cet ordinateur est configuré avec une copie en lecture\/en écriture de la base de données de configuration AD FS.  
  
Tous les autres serveurs de Fédération configurés pour cette batterie sont désignés comme *serveurs de Fédération secondaires* , car ils doivent répliquer toutes les modifications apportées sur le serveur de Fédération principal à la lecture\-uniquement les copies de la base de données de configuration AD FS qu’elles stockent localement.  
  
> [!IMPORTANT]  
> Nous vous recommandons d’utiliser au moins deux serveurs de Fédération dans une configuration de charge\-équilibrée.  
  
## <a name="deployment-considerations"></a>Considérations relatives au déploiement  
Cette section décrit les différentes considérations à prendre en compte concernant le public concerné, les avantages et les limitations associés à cette topologie de déploiement.  
  
### <a name="who-should-use-this-topology"></a>Qui doit utiliser cette topologie ?  
  
-   Les organisations disposant de 100 ou moins de relations d’approbation configurées qui doivent fournir à leurs utilisateurs internes des \(connectés aux ordinateurs qui sont physiquement connectés au réseau d’entreprise\) avec une authentification unique\-sur \(SSO\) accès aux applications ou services fédérés  
  
-   Les organisations qui souhaitent fournir à leurs utilisateurs internes un accès SSO à Microsoft Online Services ou Microsoft Office 365  
  
-   Organisations plus petites qui requièrent des services redondants et évolutifs  
  
> [!NOTE]  
> Les organisations avec des bases de données plus volumineuses doivent envisager d’utiliser la [batterie de serveurs de Fédération à l’aide](Federation-Server-Farm-Using-SQL-Server.md) de la topologie de déploiement SQL Server. Les organisations avec des utilisateurs qui se connectent depuis l’extérieur du réseau doivent envisager d’utiliser soit la [batterie de serveurs de Fédération à l’aide de la topologie wid et proxys,](Federation-Server-Farm-Using-WID-and-Proxies.md) soit la [batterie de serveurs de Fédération à l’aide](Federation-Server-Farm-Using-SQL-Server.md) de la topologie de SQL Server.  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quels sont les avantages de l’utilisation de cette topologie ?  
  
-   Fournit un accès SSO aux utilisateurs internes  
  
-   La redondance des données et des service FS (Federation Service) \(chaque serveur de Fédération réplique les modifications sur les autres serveurs de Fédération de la même batterie\)  
  
-   WID est inclus dans Windows. par conséquent, il n’est pas nécessaire d’acheter SQL Server  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quelles sont les limitations de l’utilisation de cette topologie ?  
  
-   Une batterie de serveurs WID a une limite de 30 serveurs de Fédération si vous avez 100 ou moins d’approbations de partie de confiance.  
  
-   Une batterie de serveurs WID ne prend pas en charge la détection de relecture de jetons ou la résolution d’artefacts \(partie du Security Assertion Markup Language \(protocole\) SAML\).  
  
Le tableau suivant fournit un résumé de l’utilisation d’une batterie de serveurs WID.  Utilisez-le pour planifier votre implémentation.  
  
|| 1 \- les approbations de RP 100 | Plus de 100 confiances RP |
| --- | --- | --- |
|1 \- 30 nœuds AD FS|WID pris en charge|Non pris en charge avec WID-SQL requis 
|Plus de 30 nœuds de AD FS|Non pris en charge avec WID-SQL requis|Non pris en charge avec WID-SQL requis  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recommandations relatives à l’emplacement du serveur et à la disposition du réseau  
Lorsque vous êtes prêt à commencer le déploiement de cette topologie sur votre réseau, vous devez prévoir de placer tous les serveurs de Fédération de votre réseau d’entreprise derrière un équilibrage de charge réseau \(NLB\) ordinateur hôte qui peut être configuré pour un cluster NLB avec un système de noms de domaine de cluster dédié \(nom d'\) DNS et une adresse IP de cluster.  
  
> [!NOTE]  
> Ce nom DNS de cluster doit correspondre au nom de service FS (Federation Service), par exemple fs.fabrikam.com.  
  
L’hôte NLB peut utiliser les paramètres définis dans ce cluster NLB pour allouer des demandes du client aux serveurs de Fédération individuels. L’illustration suivante montre comment la société fictive Fabrikam, Inc., définit la première phase de son déploiement à l’aide\-d’une batterie de serveurs de Fédération d’ordinateurs \(FS1 et FS2\) avec WID et le positionnement d’un serveur DNS et d’un seul hôte NLB connecté au réseau d’entreprise.  
  
![batterie de serveurs utilisant WID](media/FarmWID.gif)  
  
> [!NOTE]  
> En cas de défaillance de cet hôte NLB unique, les utilisateurs ne pourront pas accéder aux applications ou services fédérés. Ajoutez de nouveaux hôtes NLB si vos contraintes d’entreprise ne vous autorisent pas à avoir un seul point de défaillance.  
  
Pour plus d’informations sur la configuration de votre environnement de mise en réseau pour une utilisation avec des serveurs de Fédération, consultez la section Configuration requise pour la résolution de noms dans [AD FS configuration requise](AD-FS-Requirements.md).  
  
## <a name="see-also"></a>Voir aussi  
[Planifier votre topologie de déploiement d’AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guide de conception AD FS dans Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

