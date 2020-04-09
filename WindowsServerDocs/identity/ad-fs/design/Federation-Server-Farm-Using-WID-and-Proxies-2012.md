---
ms.assetid: 8890ccc9-068d-4da2-bd51-8a2964173ff1
title: Batterie de serveurs de fédération utilisant la base de données interne Windows et des proxys
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9c6dba880b80de43bb713d1b4495f0e03d56a695
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853092"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>Batterie de serveurs de fédération utilisant la base de données interne Windows et des proxys

Cette topologie de déploiement pour Services ADFS \(AD FS\) est identique à celle de la batterie de serveurs de Fédération avec la topologie \(de la base de données interne Windows\), mais elle ajoute des serveurs proxys de Fédération au réseau de périmètre pour prendre en charge les utilisateurs externes. Les serveurs proxys de Fédération redirigent les demandes d’authentification du client qui proviennent de l’extérieur de votre réseau d’entreprise vers la batterie de serveurs de Fédération.  
  
## <a name="deployment-considerations"></a>Considérations relatives au déploiement  
Cette section décrit les différentes considérations à prendre en compte concernant le public concerné, les avantages et les limitations associés à cette topologie de déploiement.  
  
### <a name="who-should-use-this-topology"></a>Qui doit utiliser cette topologie ?  
  
-   Les organisations disposant de 100 ou moins de relations d’approbation configurées qui doivent fournir à la fois leurs utilisateurs internes et les utilisateurs externes \(qui sont connectés aux ordinateurs qui se trouvent physiquement en dehors du réseau d’entreprise\) avec une\-authentification unique sur \(SSO\) accès aux applications ou services fédérés  
  
-   Les organisations qui doivent fournir à leurs utilisateurs internes et externes un accès SSO à Microsoft Office 365  
  
-   Organisations plus petites qui possèdent des utilisateurs externes et nécessitent des services évolutifs et évolutifs  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quels sont les avantages de l’utilisation de cette topologie ?  
  
-   Les mêmes avantages que ceux qui sont listés pour la [batterie de serveurs de Fédération à l’aide](Federation-Server-Farm-Using-WID-2012.md) de la topologie wid, ainsi que l’avantage de fournir un accès supplémentaire pour les utilisateurs externes  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quelles sont les limitations de l’utilisation de cette topologie ?  
  
-   Les mêmes limitations que celles indiquées pour la [batterie de serveurs de Fédération à l’aide](Federation-Server-Farm-Using-WID-2012.md) de la topologie wid  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recommandations relatives à l’emplacement du serveur et à la disposition du réseau  
Pour déployer cette topologie, en plus d’ajouter deux serveurs proxys de Fédération, vous devez vous assurer que votre réseau de périmètre peut également fournir l’accès à un système de nom de domaine \(serveur DNS\) et à un second équilibrage de la charge réseau \(ordinateur hôte\) NLB. Le second hôte NLB doit être configuré avec un cluster NLB qui utilise une adresse IP de cluster accessible par Internet\-, et il doit utiliser le même paramètre de nom DNS de cluster que le cluster NLB précédent que vous avez configuré sur le réseau d’entreprise \(fs.fabrikam.com\). Les serveurs proxys de Fédération doivent également être configurés avec des adresses IP accessibles par Internet\-.  
  
L’illustration suivante montre la batterie de serveurs de fédération existante avec la topologie WID décrite précédemment et la manière dont la société fictive Fabrikam, Inc., qui donne accès à un serveur DNS de périmètre, ajoute un deuxième hôte NLB avec le même nom DNS de cluster \(fs.fabrikam.com\)et ajoute deux serveurs proxy de Fédération \(fsp1 et fsp2\) au réseau de périmètre.  
  
![batterie de serveurs utilisant WID](media/FarmWIDProxies.gif)  
  
Pour plus d’informations sur la configuration de votre environnement de mise en réseau pour une utilisation avec des serveurs de Fédération ou des serveurs proxys de Fédération, consultez [Configuration requise pour la résolution de noms pour les serveurs de Fédération](Name-Resolution-Requirements-for-Federation-Servers.md) ou [exigences de résolution de noms pour les serveurs proxys de Fédération](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
