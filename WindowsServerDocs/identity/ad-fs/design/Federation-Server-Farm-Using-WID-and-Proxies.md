---
ms.assetid: f0464182-56a2-4bfa-a8c8-7e39c1bd62d3
title: "Batterie de serveurs de fédération avec WID et proxys"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 92d3da7cf7ab8596d6e2866543701bf530753dae
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: fr-FR
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>Batterie de serveurs de fédération avec WID et proxys

>S’applique à: Windows Server2016, Windows Server2012R2

Cette topologie de déploiement pour ActiveDirectory Federation Services \(ADFS\) est identique à la batterie de serveurs de fédération avec topologie de base de données interne Windows \(WID\), mais il ajoute des ordinateurs de proxy pour le réseau de périmètre pour prendre en charge des utilisateurs externes. Ces proxys rediriger les demandes d’authentification client provenant de l’extérieur de votre réseau d’entreprise à la batterie de serveurs de fédération. Dans les versions antérieures d’ADFS, ces serveurs proxy de fédération ont été appelés serveurs proxy de fédération.  
  
> [!IMPORTANT]  
> Dans ActiveDirectory Federation Services \(ADFS\) dans Windows Server2012R2, le rôle d’un serveur proxy de fédération est géré par un nouveau service de rôle accès à distance appelé Proxy d’Application Web. Pour activer ADFS pour l’accessibilité depuis l’extérieur du réseau d’entreprise, qui était l’objectif du déploiement d’un serveur proxy de fédération dans les versions héritées de ADFS, telles que les services ADFS 2.0 et ADFS dans Windows Server2012, vous pouvez déployer un ou plusieurs proxys d’application web pour ADFS dans Windows Server2012R2.  
>   
> Dans le contexte d’ADFS, le Proxy d’Application Web fonctionne comme un serveur proxy de fédération ADFS. En outre, le Proxy d’Application Web fournit la fonctionnalité de proxy inverse pour les applications web à l’intérieur de votre réseau d’entreprise permettre aux utilisateurs sur n’importe quel appareil d’y accéder depuis l’extérieur du réseau d’entreprise. Pour plus d’informations sur le service de rôle Proxy d’Application Web, voir vue d’ensemble du Proxy d’Application Web.  
>   
> Pour planifier le déploiement de proxy d’Application Web, vous pouvez passer en revue les informations contenues dans les rubriques suivantes:  
>   
> -   [Planifier l’Infrastructure du Proxy d’Application Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx)  
> -   [Planifier le serveur de Proxy d’Application Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
## <a name="deployment-considerations"></a>Considérations relatives au déploiement  
Cette section décrit des considérations sur le public, les avantages et les limites qui sont associés à cette topologie de déploiement.  
  
### <a name="who-should-use-this-topology"></a>Qui doit utiliser cette topologie?  
  
-   Les organisations avec moins de 100relations d’approbation configurée qui doivent fournir leurs utilisateurs internes et les utilisateurs externes \ (qui sont connectés à ordinateurs qui se trouvent physiquement en dehors de l’entreprise Updatable) avec une connexion \(SSO\) accès unique à des applications ou services fédérés  
  
-   Organisations qui doivent fournir des utilisateurs internes et externes aux utilisateurs avec accès par authentification UNIQUE MicrosoftOffice 365  
  
-   Organisations de petite taille qui ont des utilisateurs externes et requièrent des services redondants et évolutives  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quels sont les avantages de l’utilisation de cette topologie?  
  
-   Les mêmes avantages répertoriés pour le [Federation Server batterie de serveurs à l’aide de WID](Federation-Server-Farm-Using-WID.md) topologie, ainsi que l’avantage de fournir un accès supplémentaire pour les utilisateurs externes  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quelles sont les limites de l’utilisation de cette topologie?  
  
-   Les mêmes restrictions que répertoriés pour le [Federation Server batterie de serveurs à l’aide de WID](Federation-Server-Farm-Using-WID.md) topologie  

||1 \-100 RP approbations|Plus de 100 RP approbations 
| ----- |-----| ------ |
|1 \-30 ADFS nœuds|WID pris en charge|Non pris en charge à l’aide de WID \-SQL requis 
|Plus de 30 ADFS nœuds|Non pris en charge à l’aide de WID \-SQL requis|Non pris en charge à l’aide de WID \-SQL requis  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recommandations de mise en réseau et la sélection élective serveur  
Pour déployer cette topologie, en plus d’ajouter deux proxys d’application web, il se peut que vous devez vous assurer que votre réseau de périmètre permet également d’accéder à un serveur de système de nom de domaine \(DNS\) et à un deuxième hôte \(NLB\) équilibrage de charge réseau. Le second hôte NLB doit être configuré avec un cluster d’équilibrage de charge réseau qui utilise une adresse IP de cluster accessible via Internet, et il doit utiliser le même paramètre de nom DNS de cluster en tant que le précédent cluster NLB que vous avez configuré sur le réseau d’entreprise \(fs.fabrikam.com\). Les proxys d’application web doivent également être configurés avec des adresses IP Internet accessibles.  
  
L’illustration suivante montre la batterie de serveurs de fédération existante avec topologie WID décrite précédemment et comment la société fictive Fabrikam, Inc., fournit un accès à un serveur DNS de périmètre, ajoute un deuxième hôte NLB avec le même nom \(fs.fabrikam.com\) cluster DNS, et ajoute deux proxys d’application web \(wap1 and wap2\) au réseau de périmètre.  
  
![Proxys et WID la batterie de serveurs](media/WIDFarmADFSBlue.gif)  
  
Pour plus d’informations sur la configuration de votre environnement réseau pour une utilisation avec les serveurs de fédération ou proxys d’application web, consultez «Conditions requises pour la résolution de nom» section [configuration ADFS requise](AD-FS-Requirements.md) et [planifier l’Infrastructure Web Application Proxy (WAP)](https://technet.microsoft.com/library/dn383648.aspx).  
  
## <a name="see-also"></a>Voir aussi  
[Planifier votre topologie de déploiement ADFS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guide de conception ADFS dans Windows Server2012R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

