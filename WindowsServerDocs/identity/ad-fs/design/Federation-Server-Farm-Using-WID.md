---
ms.assetid: f775cbda-a75d-439d-9aa7-82f3bc8dc932
title: Batterie de serveurs de fédération utilisant la base de données interne Windows
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 067461b90ed5ce03d9470a450917dcbb93cf653a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191312"
---
# <a name="federation-server-farm-using-wid"></a>Batterie de serveurs de fédération utilisant la base de données interne Windows

La topologie par défaut pour les Services de fédération Active Directory \(AD FS\) est une batterie de serveurs de fédération, à l’aide de la base de données interne Windows \(WID\). Dans cette topologie, AD FS utilise WID comme magasin pour la base de données de configuration AD FS pour tous les serveurs de fédération qui sont joints à la batterie. La batterie réplique et maintient les données du service de fédération de la base de données de configuration à travers chaque serveur de la batterie. AD FS dans Windows Server 2012 R2 permet aux entreprises avec 100 ou moins de confiance pour configurer les batteries de serveurs de fédération avec WID jusqu'à 30 serveurs.  
  
La création du premier serveur de fédération d’une batterie consiste aussi à créer un nouveau service de fédération. Lorsque vous utilisez WID pour la base de données de configuration AD FS, le premier serveur de fédération que vous créez dans la batterie de serveurs est appelé le *serveur de fédération principal*. Cela signifie que cet ordinateur est configuré avec une lecture\/écrire la copie de la base de données de configuration AD FS.  
  
Tous les autres serveurs de fédération que vous configurez pour cette batterie de serveurs sont appelés *serveurs de fédération secondaires* , car ils doivent répliquer toutes les modifications sont apportées sur le serveur de fédération principal pour la lecture\-uniquement copie de la base de données de configuration AD FS qu’ils stockent localement.  
  
> [!IMPORTANT]  
> Nous vous recommandons d’utiliser au moins deux serveurs de fédération dans une charge\-configuration à charge équilibrée.  
  
## <a name="deployment-considerations"></a>Considérations relatives au déploiement  
Cette section décrit divers points de vue sur le public visé, les avantages et les limites qui sont associés à cette topologie de déploiement.  
  
### <a name="who-should-use-this-topology"></a>Qui doit utiliser cette topologie ?  
  
-   Les organisations avec des relations d’approbation configurées 100 ou moins devant fournir à leurs utilisateurs internes \(connecté une session sur des ordinateurs qui sont physiquement connectés au réseau d’entreprise\) avec l’authentification unique\-sur \(SSO\) accès aux applications fédérées ou de services  
  
-   Organisations qui souhaitent fournir à leurs utilisateurs internes avec un accès de l’authentification unique à Microsoft Office 365 ou Microsoft Online Services  
  
-   Petites organisations qui requièrent des services redondants, évolutif  
  
> [!NOTE]  
> Les organisations avec des bases de données volumineuses devraient envisager d’utiliser le [batterie de serveur de fédération à l’aide de SQL Server](Federation-Server-Farm-Using-SQL-Server.md) topologie de déploiement. Les organisations avec des utilisateurs qui se connectent à partir de l’extérieur du réseau doivent prendre en compte à l’aide la [Federation Server batterie de serveurs à l’aide de WID et proxys](Federation-Server-Farm-Using-WID-and-Proxies.md) topologie ou [batterie de serveur de fédération à l’aide de SQL Server](Federation-Server-Farm-Using-SQL-Server.md) topologie.  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quels sont les avantages de l’utilisation de cette topologie ?  
  
-   Fournit l’accès de l’authentification unique aux utilisateurs internes  
  
-   Redondance des données et le Service de fédération \(chaque serveur de fédération réplique les modifications aux autres serveurs de fédération dans la même batterie de serveurs\)  
  
-   WID est inclus avec Windows ; Par conséquent, sans devoir acheter SQL Server  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quelles sont les limitations de l’utilisation de cette topologie ?  
  
-   Une batterie de serveurs WID a une limite de 30 serveurs de fédération si vous avez 100 ou moins de confiance.  
  
-   Une batterie de serveurs WID ne prend pas en charge la résolution de détection ou d’artefact de relecture de jetons \(dans le cadre de la Security Assertion Markup Language \(SAML\) protocole\).  
  
Le tableau suivant fournit un résumé de l’aide d’une batterie de serveurs WID.  Il permet de planifier votre implémentation.  
  
|| 1 \- 100 approbations de partie de confiance | Plus de 100 approbations de partie de confiance |
| --- | --- | --- |
|1 \- 30 AD FS nœuds|WID pris en charge|Non pris en charge à l’aide de WID - SQL nécessaire 
|Nœuds de plus de 30 AD FS|Non pris en charge à l’aide de WID - SQL nécessaire|Non pris en charge à l’aide de WID - SQL nécessaire  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recommandations de mise en page de positionnement et de réseau serveur  
Lorsque vous êtes prêt à déployer cette topologie de votre réseau, vous devez prévoir de placer tous les serveurs de fédération dans votre réseau d’entreprise derrière un équilibrage de charge réseau \(NLB\) hôte qui peut être configurée pour un cluster NLB avec un cluster dédié Domain Name System \(DNS\) adresse IP nom et le cluster.  
  
> [!NOTE]  
> Ce nom DNS de cluster doit correspondre au nom de Service de fédération, par exemple, fs.fabrikam.com.  
  
L’hôte NLB peut utiliser les paramètres qui sont définis dans ce cluster NLB pour allouer les demandes des clients vers les serveurs de fédération individuels. L’illustration suivante montre comment la société fictive Fabrikam, Inc., configure la première phase de son déploiement à l’aide de deux\-batterie de serveurs de fédération ordinateur \(fs1 et fs2\) avec WID et le positionnement d’un serveur DNS et un seul hôte NLB qui est associé au réseau d’entreprise.  
  
![batterie de serveurs à l’aide de WID](media/FarmWID.gif)  
  
> [!NOTE]  
> S’il existe un échec sur ce seul hôte NLB, les utilisateurs ne sera pas en mesure d’accéder aux applications ou services fédérés. Ajoutez de nouveaux hôtes NLB si vos contraintes d’entreprise ne vous autorisent pas à avoir un seul point de défaillance.  
  
Pour plus d’informations sur comment configurer votre environnement réseau pour une utilisation avec les serveurs de fédération, consultez la section Name Resolution Requirements dans [configuration AD FS requise](AD-FS-Requirements.md).  
  
## <a name="see-also"></a>Voir aussi  
[Planifier votre topologie de déploiement d’AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guide de conception AD FS dans Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

