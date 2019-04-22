---
ms.assetid: 02580b2f-a339-4470-947c-d700b2d55f3f
title: Création d’une batterie de serveurs de fédération
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2e7c991cf87bc0e6914e158f0878bcadbede3c22
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816230"
---
# <a name="when-to-create-a-federation-server-farm"></a>Création d’une batterie de serveurs de fédération

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envisagez de créer une batterie de serveurs de fédération dans Active Directory Federation Services \(AD FS\) lorsque vous avez un déploiement AD FS plus volumineux et que vous souhaitez fournir une tolérance de panne, la charge\-équilibrage ou l’évolutivité pour votre Service de fédération de l’organisation. Le fait de créer deux ou plusieurs serveurs de fédération dans le même réseau, configurer chacun d’eux à utiliser le même Service de fédération et ajout de jeton de la clé publique de chaque serveur\-certificats de signature pour le composant logiciel enfichable Gestion AD FS\-dans Crée une batterie de serveurs de fédération.  
  
Vous pouvez créer une batterie de serveurs de fédération ou installer des serveurs de fédération supplémentaires à une batterie existante à l’aide de l’Assistant Configuration du serveur de fédération AD FS. Pour plus d'informations, voir [When to Create a Federation Server](When-to-Create-a-Federation-Server.md).  
  
> [!NOTE]  
> Lorsque vous choisissez l’option permettant de créer un **nouvelle batterie de serveurs de fédération** à l’aide de l’Assistant Configuration du serveur de fédération AD FS, l’Assistant va essayer de créer un objet conteneur \(pour le partage de certificats\) dans Active Directory. Par conséquent, il est important de commencer par se connecter à l’ordinateur lors de la configuration du rôle de serveur de fédération à l’aide d’un compte disposant des autorisations suffisantes dans Active Directory pour créer cet objet conteneur.  
  
Avant que les serveurs de fédération peuvent être regroupées comme une batterie de serveurs, ils doivent tout d’abord être mis en cluster afin que le nom de domaine complet de demandes qui arrivent à un seul entièrement \(FQDN\) sont acheminés vers les différents serveurs de fédération dans la batterie de serveurs. Vous pouvez créer le cluster de serveurs en déployant l’équilibrage de charge réseau \(NLB\) au sein du réseau d’entreprise. Ce guide part du principe que NLB a été correctement configuré pour chacun des serveurs de fédération de la batterie de serveurs du cluster.  
  
Pour plus d’informations sur la façon de configurer un nom de domaine complet du cluster à l’aide de la technologie Microsoft NLB, consultez [en spécifiant les paramètres de Cluster](https://go.microsoft.com/fwlink/?LinkID=74651).  
  
## <a name="best-practices-for-deploying-a-federation-server-farm"></a>Meilleures pratiques pour le déploiement d’une batterie de serveurs de fédération  
Nous vous recommandons les meilleures pratiques suivantes pour le déploiement d’un serveur de fédération dans un environnement de production :  
  
-   Si vous déployez plusieurs serveurs de fédération en même temps ou si vous savez que vous allez ajouter davantage de serveurs à la batterie de serveurs au fil du temps, envisagez de créer une image de serveur d’un serveur de fédération existant dans la batterie de serveurs et de l’installation à partir de cette image lorsque vous avez besoin pour cr les serveurs de fédération supplémentaires Cré rapidement.  
  
    > [!NOTE]  
    > Si vous décidez d’utiliser la méthode d’image de serveur pour le déploiement de serveurs de fédération supplémentaires, il est inutile accomplir les tâches dans [liste de vérification : Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md) chaque fois que vous souhaitez ajouter un nouveau serveur à la batterie de serveurs.  
  
-   Utiliser NLB ou une autre forme de clustering pour allouer une adresse IP unique pour de nombreux ordinateurs de serveur de fédération.  
  
-   Réserver une adresse IP statique pour chaque serveur de fédération dans la batterie de serveurs et, selon votre système de nom de domaine \(DNS\) configuration, l’insertion d’adresses une exclusion pour chaque adresse IP dans Dynamic Host Configuration Protocol \(DHCP\). La technologie d’équilibrage de la charge réseau Microsoft nécessite que chaque serveur participant au cluster NLB soit affecté une adresse IP statique.  
  
-   Si la base de données de configuration AD FS est stockée dans une base de données SQL, évitez de modifier la base de données SQL à partir de plusieurs serveurs de fédération en même temps.  
  
## <a name="configuring-federation-servers-for-a-farm"></a>Configuration de serveurs de fédération d’une batterie de serveurs  
Le tableau suivant décrit les tâches qui doivent être effectuées afin que chaque serveur de fédération puisse intégrer dans un environnement de batterie de serveurs.  
  
|Tâche|Description|  
|--------|---------------|  
|Si vous utilisez SQL Server pour stocker la base de données de configuration AD FS|Une batterie de serveurs de fédération se compose de deux ou plusieurs serveurs de fédération qui partagent la même base de données de configuration AD FS et le jeton\-certificats de signature. La base de données de configuration peut être stockée dans une base de données interne Windows ou une base de données SQL Server. Si vous envisagez de stocker la base de données de configuration dans une base de données SQL, assurez-vous que la base de données de configuration est accessible afin qu’il est accessible par tous les nouveaux serveurs de fédération qui participent à la batterie de serveurs. **Remarque :** Pour les scénarios de batterie de serveurs, il est important que la base de données de configuration se trouvent sur un ordinateur qui ne participe pas comme un serveur de fédération dans cette batterie de serveurs. L’équilibrage de la charge réseau Microsoft n’autorise pas les ordinateurs participant à une batterie de serveurs à communiquer entre eux. **Remarque :** Vérifiez que l’identité de l’AD FS le pool d’applications dans Internet Information Services \(IIS\) \) sur chaque fédération server qui participe à la batterie de serveurs a accès en lecture à la base de données de configuration.|  
|Obtenir et partager des certificats|Vous pouvez obtenir un seul serveur certificat d’authentification auprès d’une autorité de certification publique \(autorité de certification\)— par exemple, VeriSign. Vous pouvez ensuite configurer le certificat afin que tous les serveurs de fédération partagent la même partie de clé privée du certificat. Pour plus d’informations sur la façon de partager le même certificat, consultez [liste de vérification : Configuration d’un serveur de fédération](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md). **Remarque :** Le composant logiciel enfichable Gestion AD FS\-dans fait référence aux certificats d’authentification serveur pour les serveurs de fédération en tant que certificats de communication de service.<br /><br />Pour plus d'informations, voir [Certificate Requirements for Federation Servers](Certificate-Requirements-for-Federation-Servers.md).|  
|Pointer vers la même instance de SQL Server|Si la base de données de configuration AD FS est stockée dans une base de données SQL, le nouveau serveur de fédération doit pointer vers la même instance de SQL Server qui est utilisée par d’autres serveurs de fédération dans la batterie de serveurs afin que le nouveau serveur puisse intégrer dans la batterie de serveurs.|  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
