---
ms.assetid: 8890ccc9-068d-4da2-bd51-8a2964173ff1
title: Batterie de serveurs de fédération utilisant la base de données interne Windows et des proxys
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 60072037aea4ecd81376e1334f3a89b7bb2ff851
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408083"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>Batterie de serveurs de fédération utilisant la base de données interne Windows et des proxys

Cette topologie de déploiement pour Services ADFS \(AD FS @ no__t-1 est identique à la batterie de serveurs de Fédération avec la base de données interne Windows \(WID @ no__t-3, mais elle ajoute des serveurs proxys de Fédération au réseau de périmètre pour prendre en charge les utilisateurs externes. Les serveurs proxys de Fédération redirigent les demandes d’authentification du client qui proviennent de l’extérieur de votre réseau d’entreprise vers la batterie de serveurs de Fédération.  
  
## <a name="deployment-considerations"></a>Considérations relatives au déploiement  
Cette section décrit les différentes considérations à prendre en compte concernant le public concerné, les avantages et les limitations associés à cette topologie de déploiement.  
  
### <a name="who-should-use-this-topology"></a>Qui doit utiliser cette topologie ?  
  
-   Les organisations avec 100 ou moins de relations d’approbation configurées qui doivent fournir à la fois leurs utilisateurs internes et les utilisateurs externes \(who sont connectées aux ordinateurs qui se trouvent physiquement en dehors du réseau d’entreprise @ no__t-1 avec l’authentification unique @ no__t-2On @no__t 3SSO @ no__t-4 accès aux applications ou services fédérés  
  
-   Les organisations qui doivent fournir à leurs utilisateurs internes et externes un accès SSO à Microsoft Office 365  
  
-   Organisations plus petites qui possèdent des utilisateurs externes et nécessitent des services évolutifs et évolutifs  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quels sont les avantages de l’utilisation de cette topologie ?  
  
-   Les mêmes avantages que ceux qui sont listés pour la [batterie de serveurs de Fédération à l’aide](Federation-Server-Farm-Using-WID-2012.md) de la topologie wid, ainsi que l’avantage de fournir un accès supplémentaire pour les utilisateurs externes  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quelles sont les limitations de l’utilisation de cette topologie ?  
  
-   Les mêmes limitations que celles indiquées pour la [batterie de serveurs de Fédération à l’aide](Federation-Server-Farm-Using-WID-2012.md) de la topologie wid  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recommandations relatives à l’emplacement du serveur et à la disposition du réseau  
Pour déployer cette topologie, en plus d’ajouter deux serveurs proxys de Fédération, vous devez vous assurer que votre réseau de périmètre peut également fournir l’accès à un serveur Domain Name System \(DNS @ no__t-1 et à un second hôte de charge réseau \(NLB @ no__t-3. Le second hôte NLB doit être configuré avec un cluster NLB qui utilise une adresse IP de cluster Internet @ no__t-0accessible, et il doit utiliser le même paramètre de nom DNS de cluster que le cluster NLB précédent que vous avez configuré sur le réseau d’entreprise @no__t -1FS. fabrikam. com @ no__t-2. Les serveurs proxys de Fédération doivent également être configurés avec des adresses IP Internet @ no__t-0accessible.  
  
L’illustration suivante montre la batterie de serveurs de fédération existante avec la topologie WID décrite précédemment et comment la société fictive Fabrikam, Inc., la société fournit un accès à un serveur DNS de périmètre, ajoute un deuxième hôte NLB avec le même nom DNS de cluster @no__t -0fs. fabrikam. com @ no__t-1, et ajoute deux serveurs proxy de Fédération \(fsp1 et fsp2 @ no__t-3 au réseau de périmètre.  
  
![batterie de serveurs utilisant WID](media/FarmWIDProxies.gif)  
  
Pour plus d’informations sur la configuration de votre environnement de mise en réseau pour une utilisation avec des serveurs de Fédération ou des serveurs proxys de Fédération, consultez [Configuration requise pour la résolution de noms pour les serveurs de Fédération](Name-Resolution-Requirements-for-Federation-Servers.md) ou [exigences de résolution de noms pour la Fédération Serveurs proxy](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
