---
ms.assetid: 02580b2f-a339-4470-947c-d700b2d55f3f
title: "Création d’une batterie de serveurs de fédération"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2e7c991cf87bc0e6914e158f0878bcadbede3c22
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="when-to-create-a-federation-server-farm"></a>Création d’une batterie de serveurs de fédération

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Envisagez de créer une batterie de serveurs de fédération dans Active Directory Federation Services \(AD FS\) lorsque vous disposez d’un déploiement AD FS supérieure et que vous souhaitez fournir une tolérance de panne, équilibrage de charge load\ ou de l’extensibilité au Service de fédération de votre organisation. Le fait de créer deux ou plusieurs serveurs de fédération dans le même réseau, la configuration de chacun d’eux afin d’utiliser le même Service de fédération et d’ajouter la clé publique de certificats de signature de token\ de chaque serveur pour la gestion AD FS enfichable créent une batterie de serveurs de fédération.  
  
Vous pouvez créer une batterie de serveurs de fédération ou installer des serveurs de fédération supplémentaires à une batterie existante à l’aide de l’Assistant Configuration du serveur de fédération AD FS. Pour plus d’informations, voir [quand créer un serveur de fédération](When-to-Create-a-Federation-Server.md).  
  
> [!NOTE]  
> Lorsque vous choisissez l’option pour créer un **nouvelle batterie de serveurs de fédération** à l’aide de l’Assistant Configuration du serveur de fédération AD FS, l’Assistant tente de créer un objet conteneur \(for sharing certificates\) dans Active Directory. Par conséquent, il est important que vous ouvrez une session l’ordinateur sur lequel vous configurez le rôle de serveur de fédération, avec un compte qui dispose des autorisations suffisantes dans Active Directory pour créer cet objet conteneur.  
  
Avant de serveurs de fédération peuvent être regroupés comme une batterie de serveurs, ils doivent tout d’abord être mis en cluster afin que les demandes qui arrivent à un seul pleinement qualifié \(FQDN\) sont routées vers les différents serveurs de fédération dans la batterie de serveurs de nom de domaine. Vous pouvez créer le cluster de serveurs en déployant l’équilibrage de charge réseau \(NLB\) à l’intérieur du réseau d’entreprise. Ce guide suppose que l’équilibrage de charge réseau a été configuré correctement pour chacun des serveurs de fédération de la batterie de serveurs de cluster.  
  
Pour plus d’informations sur la façon de configurer un nom de domaine complet du cluster à l’aide de la technologie Microsoft NLB, consultez [spécifiant les paramètres de Cluster](https://go.microsoft.com/fwlink/?LinkID=74651).  
  
## <a name="best-practices-for-deploying-a-federation-server-farm"></a>Meilleures pratiques pour le déploiement d’une batterie de serveurs de fédération  
Nous recommandons les meilleures pratiques suivantes pour le déploiement d’un serveur de fédération dans un environnement de production:  
  
-   Si vous déployez plusieurs serveurs de fédération en même temps ou si vous savez que vous allez ajouter des serveurs à la batterie de serveurs au fil du temps, pensez à créer une image serveur d’un serveur de fédération existante dans la batterie de serveurs et de l’installation à partir de cette image lorsque vous avez besoin créer rapidement des serveurs de fédération supplémentaires.  
  
    > [!NOTE]  
    > Si vous décidez d’utiliser la méthode d’image serveur pour le déploiement de serveurs de fédération supplémentaires, vous n’êtes pas obligé d’effectuer les tâches dans [liste de vérification: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md) chaque fois que vous souhaitez ajouter un nouveau serveur à la batterie de serveurs.  
  
-   Utiliser NLB ou une autre forme de clustering pour allouer une adresse IP unique pour de nombreux ordinateurs serveur de fédération.  
  
-   Réserver une adresse IP statique pour chaque serveur de fédération dans la batterie de serveurs et, selon votre configuration de système de nom de domaine \(DNS\), insérez une exclusion pour chaque adresse IP dans Dynamic Host Configuration Protocol \(DHCP\). La technologie Microsoft NLB nécessite que chaque serveur qui fait partie du cluster NLB être affecté une adresse IP statique.  
  
-   Si la base de données de configuration AD FS est stockée dans une base de données SQL, évitez de modifier la base de données SQL à partir de plusieurs serveurs de fédération en même temps.  
  
## <a name="configuring-federation-servers-for-a-farm"></a>Configuration des serveurs de fédération pour une batterie de serveurs  
Le tableau suivant décrit les tâches qui doivent être effectuées afin que chaque serveur de fédération peut intégrer dans un environnement de batterie de serveurs.  
  
|Tâche|Description|  
|--------|---------------|  
|Si vous utilisez SQL Server pour stocker la base de données de configuration AD FS|Une batterie de serveurs de fédération se compose de deux ou plusieurs serveurs de fédération qui partagent la même configuration AD FS base de données et des certificats de signature de token\. La base de données de configuration peut être stockée dans la base de données interne Windows ou dans une base de données SQL Server. Si vous envisagez de stocker la base de données de configuration dans une base de données SQL, assurez-vous que la base de données de configuration est accessible, afin qu’il est accessible par tous les nouveaux serveurs de fédération qui participent à la batterie de serveurs. **Remarque:** pour les scénarios de batterie de serveurs, il est important que la base de données de configuration se trouvent sur un ordinateur qui ne participe pas comme un serveur de fédération de cette batterie de serveurs. Microsoft NLB n’autorise pas les ordinateurs qui participent à une batterie de serveurs à communiquer entre eux. **Remarque:** Assurez-vous que l’identité du pool d’applications AD FS dans \(IIS\)\) services Internet (IIS) sur chaque serveur de fédération qui participe à la batterie de serveurs dispose d’un accès en lecture à la base de données de configuration.|  
|Obtenir et partager des certificats|Vous pouvez obtenir un seul serveur de certificat d’authentification auprès d’une autorité de certification publique \ (CA\), par exemple, VeriSign. Vous pouvez ensuite configurer le certificat afin que tous les serveurs de fédération partagent la même partie de clé privée du certificat. Pour plus d’informations sur la façon de partager le même certificat, voir [liste de vérification: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md). **Remarque:** la gestion ADFS enfichable fait référence aux certificats d’authentification serveur pour les serveurs de fédération en tant que certificats de communication du service.<br /><br />Pour plus d’informations, voir [certificats requis pour les serveurs de fédération](Certificate-Requirements-for-Federation-Servers.md).|  
|Pointez sur la même instance de SQL Server|Si la base de données de configuration AD FS est stockée dans une base de données SQL, le nouveau serveur de fédération doit pointer vers la même instance de SQL Server qui est utilisée par d’autres serveurs de fédération dans la batterie de serveurs afin que le nouveau serveur puisse intégrer dans la batterie de serveurs.|  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception ADFS dans Windows Server2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
