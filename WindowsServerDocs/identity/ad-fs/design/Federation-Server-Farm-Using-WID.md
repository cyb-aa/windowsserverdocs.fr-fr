---
ms.assetid: f775cbda-a75d-439d-9aa7-82f3bc8dc932
title: "Batterie de serveurs de fédération avec WID"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 41c2179cbd8bf2c6032f233335099b512c02f880
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="federation-server-farm-using-wid"></a>Batterie de serveurs de fédération avec WID

>S’applique à: Windows Server2016, Windows Server2012R2

La topologie par défaut pour les Services de fédération ActiveDirectory \(ADFS\) est une batterie de serveurs de fédération à l’aide de la base de données interne Windows \(WID\). Dans cette topologie, ADFS utilise WID que le Windows store pour la base de données de configuration ADFS pour tous les serveurs de fédération qui sont joints à la batterie. La batterie réplique et maintient les données du Service de fédération dans la base de données de configuration sur chaque serveur de la batterie de serveurs. ADFS dans Windows Server2012R2 permet aux organisations avec moins de 100approbations de confiance pour configurer des batteries de serveurs de fédération avec WID jusqu'à 30serveurs.  
  
Le fait de créer le premier serveur de fédération dans une batterie de serveurs crée également un Service de fédération. Lorsque vous utilisez WID pour la base de données de configuration ADFS, le premier serveur de fédération que vous créez dans la batterie de serveurs est appelé le *serveur de fédération principal*. Cela signifie que cet ordinateur est configuré avec une copie en lecture/écriture de la base de données de configuration ADFS.  
  
Tous les autres serveurs de fédération que vous configurez pour cette batterie de serveurs sont appelés *serveurs de fédération secondaires*, car ils doivent répliquer les modifications apportées sur le serveur de fédération principal pour les copies en lecture seule de la base de données de configuration ADFS qu’ils stockent localement.  
  
> [!IMPORTANT]  
> Nous recommandons l’utilisation d’au moins deux serveurs de fédération dans une configuration load\ équilibrée.  
  
## <a name="deployment-considerations"></a>Considérations relatives au déploiement  
Cette section décrit des considérations sur le public, les avantages et les limites qui sont associés à cette topologie de déploiement.  
  
### <a name="who-should-use-this-topology"></a>Qui doit utiliser cette topologie?  
  
-   Les organisations avec moins de 100relations d’approbation configurée qui doivent fournir leurs utilisateurs internes \ (connecté une session sur les ordinateurs qui sont connectés physiquement à l’entreprise Updatable) avec une connexion \(SSO\) accès unique à des applications ou services fédérés  
  
-   Organisations qui souhaitent proposer leurs utilisateurs internes avec accès par authentification unique à MicrosoftOnline Services ou MicrosoftOffice 365  
  
-   Organisations de petite taille qui nécessitent des services redondants et évolutives  
  
> [!NOTE]  
> Les organisations avec des bases de données volumineuses envisagez d’utiliser le [batterie de serveur de fédération à l’aide de SQLServer](Federation-Server-Farm-Using-SQL-Server.md) topologie de déploiement. Organisations avec des utilisateurs qui se connectent à partir de l’extérieur du réseau doivent prendre en compte à l’aide du [Federation Server batterie de serveurs à l’aide de WID et proxys](Federation-Server-Farm-Using-WID-and-Proxies.md) topologie ou le [batterie de serveur de fédération à l’aide de SQLServer](Federation-Server-Farm-Using-SQL-Server.md) topologie.  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quels sont les avantages de l’utilisation de cette topologie?  
  
-   Fournit un accès par authentification unique aux utilisateurs internes  
  
-   Redondance des données et le Service de fédération \ (chaque serveur de fédération réplique les modifications sur les autres serveurs de fédération dans le même farm\)  
  
-   WID est inclus avec Windows; par conséquent, sans devoir acheter de SQLServer  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quelles sont les limites de l’utilisation de cette topologie?  
  
-   Une batterie WID dispose d’un nombre maximal de serveurs de fédération 30 si vous avez moins de 100 approbations de partie confiance.  
  
-   Une batterie WID ne prend pas en charge résolution détection ou artefact de relecture de jetons \ (partie du \(SAML\) protocol\ Security Assertion Markup Language).  
  
Le tableau suivant fournit un résumé pour l’utilisation d’une batterie WID.  Il permet de planifier votre implémentation.  
  
|| 1 \-100 RP approbations | Plus de 100 RP approbations |
| --- | --- | --- |
|1 \-30 AD FS nœuds|WID pris en charge|Non pris en charge à l’aide de WID - SQL requis 
|Plus de 30 AD FS nœuds|Non pris en charge à l’aide de WID - SQL requis|Non pris en charge à l’aide de WID - SQL requis  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recommandations de mise en réseau et la sélection élective serveur  
Lorsque vous êtes prêt à commencer à déployer cette topologie de votre réseau, vous devez prévoir de placer tous les serveurs de fédération dans votre réseau d’entreprise derrière un hôte \(NLB\) équilibrage de charge réseau qui peut être configuré pour un cluster d’équilibrage de charge réseau avec un nom du système de nom de domaine \(DNS\) cluster dédié et l’adresse IP du cluster.  
  
> [!NOTE]  
> Ce nom DNS de cluster doit correspondre au nom de Service de fédération, par exemple, fs.fabrikam.com.  
  
L’hôte NLB peut utiliser les paramètres qui sont définies dans ce cluster d’équilibrage de charge réseau pour allouer les demandes des clients vers les serveurs de fédération individuels. L’illustration suivante montre comment la société fictive Fabrikam, Inc., configure la première phase de son déploiement à l’aide d’une batterie de serveurs de fédération contribuent à ordinateur \(fs1 and fs2\) avec WID et le positionnement d’un serveur DNS et un seul hôte NLB qui est connecté au réseau d’entreprise.  
  
![Batterie de serveurs à l’aide de WID](media/FarmWID.gif)  
  
> [!NOTE]  
> S’il existe une défaillance sur ce seul hôte NLB, les utilisateurs ne seront pas en mesure d’accéder aux applications ou services fédérés. Ajouter de nouveaux hôtes NLB si vos exigences professionnelles ne permettent pas d’avoir un seul point de défaillance.  
  
Pour plus d’informations sur la configuration de votre environnement réseau pour une utilisation avec les serveurs de fédération, consultez la section Configuration requise pour la résolution de nom dans [configuration ADFS requise](AD-FS-Requirements.md).  
  
## <a name="see-also"></a>Voir aussi  
[Planifier votre topologie de déploiement ADFS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guide de conception ADFS dans Windows Server2012R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

