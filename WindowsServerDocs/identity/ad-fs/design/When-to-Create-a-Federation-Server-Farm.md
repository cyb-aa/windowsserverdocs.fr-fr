---
ms.assetid: 02580b2f-a339-4470-947c-d700b2d55f3f
title: Création d’une batterie de serveurs de fédération
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: fcfc7d640d3688bf0e23557af9bd56082418ef37
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358941"
---
# <a name="when-to-create-a-federation-server-farm"></a>Création d’une batterie de serveurs de fédération

Envisagez de créer une batterie de serveurs de Fédération dans Services ADFS \(AD FS\) lorsque vous avez un déploiement de AD FS plus important et que vous souhaitez fournir la tolérance de panne, la charge\-l’équilibrage ou l’évolutivité au service FS (Federation Service) de votre organisation. Le fait de créer deux serveurs de Fédération ou plus dans le même réseau, de configurer chacun d’eux pour qu’ils utilisent le même service FS (Federation Service), et d’ajouter la clé publique du jeton de chaque serveur\-les certificats de signature au\-du composant logiciel enfichable de gestion AD FS dans crée une batterie de serveurs de Fédération.  
  
Vous pouvez créer une batterie de serveurs de Fédération ou installer des serveurs de Fédération supplémentaires sur une batterie existante à l’aide de l’Assistant Configuration du serveur de fédération AD FS. Pour plus d'informations, voir [When to Create a Federation Server](When-to-Create-a-Federation-Server.md).  
  
> [!NOTE]  
> Lorsque vous choisissez l’option de création d’une **batterie de serveurs de Fédération** à l’aide de l’Assistant Configuration du serveur de fédération AD FS, l’Assistant tente de créer un objet conteneur \(pour le partage de certificats\) dans Active Directory. Par conséquent, il est important de commencer par se connecter à l’ordinateur lors de la configuration du rôle de serveur de fédération à l’aide d’un compte disposant des autorisations suffisantes dans Active Directory pour créer cet objet conteneur.  
  
Avant de pouvoir regrouper les serveurs de Fédération en tant que batterie de serveurs, ils doivent d’abord être mis en cluster afin que les demandes arrivant à un nom de domaine complet unique \(nom de domaine complet\) soient acheminées vers les différents serveurs de Fédération de la batterie de serveurs. Vous pouvez créer le cluster de serveurs en déployant l’équilibrage de la charge réseau \(NLB\) au sein du réseau d’entreprise. Ce guide suppose que l’équilibrage de la charge réseau a été configuré de manière appropriée pour le cluster de chacun des serveurs de Fédération de la batterie.  
  
Pour plus d’informations sur la configuration d’un nom de domaine complet de cluster à l’aide de la technologie Microsoft NLB, consultez [spécification des paramètres de cluster](https://go.microsoft.com/fwlink/?LinkID=74651).  
  
## <a name="best-practices-for-deploying-a-federation-server-farm"></a>Meilleures pratiques pour le déploiement d’une batterie de serveurs de fédération  
Nous vous recommandons les meilleures pratiques suivantes pour le déploiement d’un serveur de Fédération dans un environnement de production :  
  
-   Si vous envisagez de déployer plusieurs serveurs de Fédération en même temps ou si vous savez que vous allez ajouter des serveurs à la batterie de serveurs au fil du temps, envisagez de créer une image serveur d’un serveur de Fédération existant dans la batterie de serveurs, puis de l’installer à partir de cette image lorsque vous avez besoin de CR cré des serveurs de Fédération supplémentaires rapidement.  
  
    > [!NOTE]  
    > Si vous décidez d’utiliser la méthode d’image serveur pour déployer des serveurs de Fédération supplémentaires, il n’est pas nécessaire d’effectuer les tâches décrites dans [liste de vérification : configuration d’un serveur de Fédération](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md) chaque fois que vous souhaitez ajouter un nouveau serveur à la batterie de serveurs.  
  
-   Utilisez l’équilibrage de la charge réseau ou une autre forme de clustering pour allouer une adresse IP unique à de nombreux serveurs de Fédération.  
  
-   Réservez une adresse IP statique pour chaque serveur de Fédération de la batterie de serveurs et, selon votre système de nom de domaine \(configuration de\) DNS, insérez une exclusion pour chaque adresse IP dans le protocole de configuration d’hôte dynamique \(le\)DHCP. La technologie d’équilibrage de la charge réseau Microsoft nécessite que chaque serveur participant au cluster NLB soit affecté une adresse IP statique.  
  
-   Si la base de données de configuration AD FS est stockée dans une base de données SQL, évitez de modifier la base de données SQL à partir de plusieurs serveurs de Fédération en même temps.  
  
## <a name="configuring-federation-servers-for-a-farm"></a>Configuration de serveurs de fédération d’une batterie de serveurs  
Le tableau suivant décrit les tâches qui doivent être effectuées pour que chaque serveur de Fédération puisse participer à un environnement de batterie de serveurs.  
  
|Tâche|Description|  
|--------|---------------|  
|Si vous utilisez SQL Server pour stocker la base de données de configuration AD FS|Une batterie de serveurs de Fédération se compose de deux serveurs de Fédération ou plus qui partagent les mêmes AD FS base de données de configuration et de jeton\-les certificats de signature. La base de données de configuration peut être stockée dans une base de données interne Windows ou une base de données SQL Server. Si vous envisagez de stocker la base de données de configuration dans une base de données SQL, assurez-vous que la base de données de configuration est accessible à tous les nouveaux serveurs de Fédération qui participent à la batterie de serveurs. **Remarque :** Pour les scénarios de batterie de serveurs, il est important que la base de données de configuration se trouve sur un ordinateur qui ne participe pas également en tant que serveur de Fédération de cette batterie. L’équilibrage de la charge réseau Microsoft n’autorise pas les ordinateurs participant à une batterie de serveurs à communiquer entre eux. **Remarque :** Assurez-vous que l’identité du pool d’AD FS dans Internet Information Services \(IIS\)\) sur chaque serveur de Fédération qui participe à la batterie de serveurs dispose d’un accès en lecture à la base de données de configuration.|  
|Obtenir et partager des certificats|Vous pouvez obtenir un certificat d’authentification de serveur unique auprès d’une autorité de certification publique \(\)de l’autorité de certification, par exemple, VeriSign. Vous pouvez ensuite configurer le certificat afin que tous les serveurs de fédération partagent la même partie de clé privée du certificat. Pour plus d’informations sur le partage du même certificat, voir [Checklist: Setting Up a Federation Server](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md). **Remarque :** Le\-du composant logiciel enfichable Gestion de AD FS dans fait référence aux certificats d’authentification serveur pour les serveurs de Fédération en tant que certificats de communication de service.<br /><br />Pour plus d'informations, voir [Certificate Requirements for Federation Servers](Certificate-Requirements-for-Federation-Servers.md).|  
|Pointer vers la même instance de SQL Server|Si la base de données de configuration AD FS est stockée dans une base de données SQL, le nouveau serveur de Fédération doit pointer vers la même instance de SQL Server utilisée par les autres serveurs de Fédération de la batterie afin que le nouveau serveur puisse participer à la batterie.|  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
