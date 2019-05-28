---
ms.assetid: 8890ccc9-068d-4da2-bd51-8a2964173ff1
title: Batterie de serveurs de fédération utilisant la base de données interne Windows et des proxys
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 19e73e43a863ec60fbc9da09b24173220bb331ed
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191358"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>Batterie de serveurs de fédération utilisant la base de données interne Windows et des proxys

Cette topologie de déploiement d’Active Directory Federation Services \(AD FS\) est identique à la batterie de serveurs de fédération avec base de données interne Windows \(WID\) topologie, mais il ajoute des serveurs proxy de fédération pour le réseau de périmètre pour prendre en charge des utilisateurs externes. Les serveurs proxy de fédération rediriger les demandes d’authentification client provenant de l’extérieur de votre réseau d’entreprise à la batterie de serveurs de fédération.  
  
## <a name="deployment-considerations"></a>Considérations relatives au déploiement  
Cette section décrit divers points de vue sur le public visé, les avantages et les limites qui sont associés à cette topologie de déploiement.  
  
### <a name="who-should-use-this-topology"></a>Qui doit utiliser cette topologie ?  
  
-   Les organisations avec des relations d’approbation configurées 100 ou moins qui doivent fournir leurs utilisateurs internes et les utilisateurs externes \(qui sont connectés aux ordinateurs qui sont physiquement situés en dehors du réseau d’entreprise\) avec unique connexion\-sur \(SSO\) accès aux applications fédérées ou de services  
  
-   Organisations qui doivent fournir leurs utilisateurs internes et les utilisateurs externes avec accès par authentification unique à Microsoft Office 365  
  
-   Petites organisations qui ont des utilisateurs externes et nécessitent des services redondants, évolutif  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quels sont les avantages de l’utilisation de cette topologie ?  
  
-   Les mêmes avantages qu’apparaissent dans le [Federation Server batterie de serveurs à l’aide de WID](Federation-Server-Farm-Using-WID-2012.md) topologie, ainsi que l’avantage de fournir un accès supplémentaire pour les utilisateurs externes  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quelles sont les limitations de l’utilisation de cette topologie ?  
  
-   Les mêmes limitations que répertoriées pour le [Federation Server batterie de serveurs à l’aide de WID](Federation-Server-Farm-Using-WID-2012.md) topologie  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recommandations de mise en page de positionnement et de réseau serveur  
Pour déployer cette topologie, outre l’ajout de deux serveurs proxys de fédération, vous devez vérifier que votre réseau de périmètre permet également d’accéder à un système de nom de domaine \(DNS\) serveur et à un deuxième équilibrage de charge réseau \(NLB\) hôte. Le second hôte NLB doit être configuré avec un cluster NLB qui utilise un Internet\-adresse IP du cluster accessible et il doivent utiliser le même paramètre de nom DNS de cluster en tant que le précédent cluster NLB que vous avez configuré sur le réseau d’entreprise \( FS.fabrikam.com\). Les serveurs proxy de fédération doivent également être configurés avec Internet\-des adresses IP accessibles.  
  
L’illustration suivante montre la batterie de serveurs de fédération existante avec topologie WID qui a été décrit précédemment et comment la société fictive Fabrikam, Inc., fournit l’accès à un serveur DNS de périmètre, ajoute un deuxième hôte NLB avec le nom DNS du même cluster \(fs.fabrikam.com\), et ajoute deux serveurs proxys de fédération \(fsp1 and fsp2\) au réseau de périmètre.  
  
![batterie de serveurs à l’aide de WID](media/FarmWIDProxies.gif)  
  
Pour plus d’informations sur la configuration de votre environnement réseau pour une utilisation avec les serveurs de fédération ou de serveurs proxy de fédération, consultez [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md) ou [nom Configuration de la résolution pour les serveurs proxy de fédération](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
