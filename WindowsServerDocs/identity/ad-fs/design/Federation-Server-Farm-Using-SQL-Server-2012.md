---
ms.assetid: 6618b3ce-0e94-4009-b887-d8e05453358b
title: Batterie de serveurs de fédération utilisant SQL Server
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 66c8bae2fbccca2bf618e46ffd3ccc05cb52f911
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191504"
---
# <a name="federation-server-farm-using-sql-server"></a>Batterie de serveurs de fédération utilisant SQL Server

Cette topologie pour Active Directory Federation Services \(AD FS\) diffère de la batterie de serveurs de fédération à l’aide de la base de données interne Windows \(WID\) topologie de déploiement qui elle ne réplique pas les données à chaque serveur de fédération dans la batterie de serveurs. Au lieu de cela, tous les serveurs de fédération dans la batterie de serveurs peuvent lire et écrire des données dans une base de données commune qui est stocké sur un serveur exécutant Microsoft SQL Server qui se trouve dans le réseau d’entreprise.  
  
## <a name="deployment-considerations"></a>Considérations relatives au déploiement  
Cette section décrit divers points de vue sur le public visé, les avantages et les limites qui sont associés à cette topologie de déploiement.  
  
### <a name="who-should-use-this-topology"></a>Qui doit utiliser cette topologie ?  
  
-   Grandes organisations avec plus de 100 relations d’approbation qui doivent fournir leurs utilisateurs internes et les utilisateurs externes avec l’authentification unique\-sur \(SSO\) accès aux applications fédérées ou de services  
  
-   Les organisations ayant déjà utilisent SQL Server et souhaitent tirer parti de leurs outils existants et de savoir-faire  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quels sont les avantages de l’utilisation de cette topologie ?  
  
-   Prise en charge pour un plus grand nombre de relations d’approbation \(plus de 100\)  
  
-   Prise en charge pour la détection de relecture de jetons \(une fonctionnalité de sécurité\) et résolution d’artefacts \(dans le cadre de la Security Assertion Markup Language \(SAML\) protocole 2.0\)  
  
-   Outils de support pour tous les avantages de SQL Server, telles que la mise en miroir de base de données, clustering de basculement, reporting et gestion  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quelles sont les limitations de l’utilisation de cette topologie ?  
  
-   Cette topologie ne fournit pas de redondance de base de données par défaut. Bien qu’une batterie de serveurs de fédération avec topologie WID réplique automatiquement la base de données WID sur chaque serveur de fédération dans la batterie de serveurs, la batterie de serveurs de fédération avec topologie SQL Server contient une seule copie de la base de données  
  
> [!NOTE]  
> SQL Server prend en charge plusieurs données différentes et les options de redondance d’application, y compris le clustering de basculement, la mise en miroir de base de données et les différents types de réplication SQL Server.  
  
Le Microsoft Information Technology \(informatique\) service utilise la mise en miroir de base de données SQL Server dans haute\-sécurité \(synchrone\) mode et le clustering de basculement pour fournir une élevée\- prise en charge de la disponibilité de l’instance de SQL Server. SQL Server transactionnelle \(homologue\-à\-homologue\) et la réplication de fusion n’ont pas été testés par l’équipe de produit AD FS chez Microsoft. Pour plus d’informations sur SQL Server, consultez [vue d’ensemble des Solutions haute disponibilité](https://go.microsoft.com/fwlink/?LinkId=179853) ou [en sélectionnant le Type de réplication appropriée](https://go.microsoft.com/fwlink/?LinkId=214648).  
  
### <a name="supported-sql-server-versions"></a>Versions prises en charge de SQL Server  
Les versions suivantes de SQL server sont prises en charge avec AD FS installé avec Windows Server 2012 :  
  
-   SQL Server 2008 \/ R2  
  
-   SQL Server 2012  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recommandations de mise en page de positionnement et de réseau serveur  
Comme pour la batterie de serveurs de fédération avec topologie WID, tous les serveurs de fédération dans la batterie de serveurs sont configurés pour utiliser un seul cluster Domain Name System \(DNS\) nom \(qui représente le nom du Service de fédération\)et adresse IP du cluster d’un cadre de l’équilibrage de charge réseau \(NLB\) configuration du cluster. Cela permet à l’hôte NLB d’allouer les demandes des clients vers les serveurs de fédération individuels. Serveurs proxy de fédération permet aux demandes des clients de proxy pour la batterie de serveurs de fédération.  
  
L’illustration suivante montre comment la société fictive Contoso Pharmaceuticals a déployé sa batterie de serveurs de fédération avec topologie SQL Server dans le réseau d’entreprise. Il montre également comment cette entreprise a configuré le réseau de périmètre avec un accès à un serveur DNS, un hôte NLB supplémentaire qui utilise le même nom DNS de cluster \(fs.contoso.com\) qui est utilisé sur le cluster d’équilibrage de charge réseau de réseau d’entreprise et avec deux serveurs proxy de fédération \(fsp1 and fsp2\).  
  
![batterie de serveurs à l’aide de SQL](media/FarmSQLProxies.gif)  
  
Pour plus d’informations sur la configuration de votre environnement réseau pour une utilisation avec les serveurs de fédération ou de serveurs proxy de fédération, consultez [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md) ou [nom Configuration de la résolution pour les serveurs proxy de fédération](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
