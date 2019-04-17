---
ms.assetid: 8890ccc9-068d-4da2-bd51-8a2964173ff1
title: "Batterie de serveurs de fédération avec WID et proxys"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4bd815daccdd72a8c612b9b728ce12378c1926e7
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>Batterie de serveurs de fédération avec WID et proxys

>S’applique à: Windows Server2012

Cette topologie de déploiement pour ActiveDirectory Federation Services \(ADFS\) est identique à la batterie de serveurs de fédération avec topologie de base de données interne Windows \(WID\), mais il ajoute des serveurs proxy de fédération au réseau de périmètre pour prendre en charge des utilisateurs externes. Serveurs proxy de fédération rediriger les demandes d’authentification client provenant de l’extérieur de votre réseau d’entreprise à la batterie de serveurs de fédération.  
  
## <a name="deployment-considerations"></a>Considérations relatives au déploiement  
Cette section décrit des considérations sur le public, les avantages et les limites qui sont associés à cette topologie de déploiement.  
  
### <a name="who-should-use-this-topology"></a>Qui doit utiliser cette topologie?  
  
-   Les organisations avec moins de 100relations d’approbation configurée qui doivent fournir leurs utilisateurs internes et les utilisateurs externes \ (qui sont connectés à ordinateurs qui se trouvent physiquement en dehors de l’entreprise Updatable) avec une connexion \(SSO\) accès unique à des applications ou services fédérés  
  
-   Organisations qui doivent fournir des utilisateurs internes et externes aux utilisateurs avec accès par authentification unique MicrosoftOffice 365  
  
-   Organisations de petite taille qui ont des utilisateurs externes et requièrent des services redondants et évolutives  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quels sont les avantages de l’utilisation de cette topologie?  
  
-   Les mêmes avantages répertoriés pour le [Federation Server batterie de serveurs à l’aide de WID](Federation-Server-Farm-Using-WID-2012.md) topologie, ainsi que l’avantage de fournir un accès supplémentaire pour les utilisateurs externes  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quelles sont les limites de l’utilisation de cette topologie?  
  
-   Les mêmes restrictions que répertoriés pour le [Federation Server batterie de serveurs à l’aide de WID](Federation-Server-Farm-Using-WID-2012.md) topologie  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recommandations de mise en réseau et la sélection élective serveur  
Pour déployer cette topologie, en plus d’ajouter deux serveurs proxys de fédération, il se peut que vous devez vous assurer que votre réseau de périmètre permet également d’accéder à un serveur de système de nom de domaine \(DNS\) et à un deuxième hôte \(NLB\) équilibrage de charge réseau. Le second hôte NLB doit être configuré avec un cluster d’équilibrage de charge réseau qui utilise une adresse IP de cluster accessible via Internet, et il doit utiliser le même paramètre de nom DNS de cluster en tant que le précédent cluster NLB que vous avez configuré sur le réseau d’entreprise \(fs.fabrikam.com\). Serveurs proxy de fédération doivent également être configurés avec des adresses IP Internet accessibles.  
  
L’illustration suivante montre la batterie de serveurs de fédération existante avec topologie WID décrite précédemment et comment la société fictive Fabrikam, Inc., fournit un accès à un serveur DNS de périmètre, ajoute un deuxième hôte NLB avec le même nom \(fs.fabrikam.com\) cluster DNS, et ajoute deux serveurs proxys de fédération \(fsp1 and fsp2\) au réseau de périmètre.  
  
![Batterie de serveurs à l’aide de WID](media/FarmWIDProxies.gif)  
  
Pour plus d’informations sur la configuration de votre environnement réseau pour une utilisation avec les serveurs de fédération ou serveurs proxy de fédération, voir [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md) ou [Name Resolution Requirements for Federation Server Proxies](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
