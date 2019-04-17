---
title: "Migrer une batterie SQL serveur AD FS 2.0 de fédération"
description: "Fournit des informations sur la migration d’une batterie SQL server 2.0 AD FS vers Windows Server 2012"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8433219850793aa34b646a3bf14cba42d3de4988
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>Migrer une batterie AD FS 2.0 WID  
Ce document fournit des informations détaillées sur la migration d’une batterie AD FS 2.0 SQL vers Windows Server 2012.


## <a name="migrate-a-sql-server-farm"></a>Migration d’une batterie SQL Server  
 Pour migrer une batterie SQL Server vers Windows Server 2012, effectuez la procédure suivante:  
  
1.  Pour chaque serveur de votre batterie SQL Server, consultez et effectuez les procédures de [migrer une batterie SQL Server](prepare-to-migrate-a-sql-server-farm.md).  
  
2.  Supprimer tous les serveurs de votre batterie SQL Server à partir de l’équilibrage de charge.  
  
3.  Mettre à niveau le système d’exploitation sur ce serveur de votre batterie SQL Server à partir de Windows Server 2008 R2 ou Windows Server 2008 vers Windows Server 2012. Pour plus d’informations, voir [l’installation de Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Le résultat de la mise à niveau du système d’exploitation, la configuration AD FS sur ce serveur est perdue et le rôle de serveur 2.0 AD FS est supprimé. Le rôle de serveur AD FS Windows Server 2012 est installé à la place, mais il n’est pas configuré. Vous devez manuellement créer la configuration AD FS d’origine et restaurer les paramètres AD FS restants pour terminer la migration de serveur de fédération.  
  
4.  Créez la configuration AD FS d’origine sur ce serveur de votre batterie SQL Server à l’aide des applets de commande Windows PowerShell AD FS pour ajouter un serveur à une batterie existante.  
  
> [!IMPORTANT]
>  Vous devez utiliser Windows PowerShell pour créer la configuration AD FS d’origine si vous utilisez SQL Server pour stocker votre base de données de configuration AD FS.  

  - Ouvrez Windows PowerShell et exécutez la commande suivante: `$fscredential = Get-Credential`.  
  - Entrez le nom et le mot de passe du compte de service que vous avez enregistrées pendant la préparation de votre batterie SQL Server pour la migration.  
  - Exécutez la commande suivante: `Add-AdfsFarmNode -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<Data Source>;Integrated Security=True"`, où `Data Source` est la valeur de source de données dans la valeur de chaîne de connexion de magasin de stratégie dans le fichier suivant: `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`.  
  
5.  Ajoutez le serveur simplement mis à niveau vers Windows Server 2012 à l’équilibrage de charge.  
  
6.  Répétez les étapes 2 à 6 pour les nœuds restants de votre batterie SQL Server.  
  
7.  Lorsque tous les serveurs de votre batterie SQL Server sont mis à niveau vers Windows Server 2012, restaurez toutes les personnalisations AD FS restantes, telles que les magasins d’attributs personnalisés.  

## <a name="next-steps"></a>Étapes suivantes
 [Préparer la migration du serveur AD FS 2.0 de fédération](prepare-to-migrate-ad-fs-fed-server.md)   
 [Préparer la migration AD FS 2.0 serveur Proxy de fédération](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrer le serveur AD FS 2.0 de fédération](migrate-the-ad-fs-fed-server.md)   
 [Migrer le serveur Proxy AD FS 2.0 de fédération](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrer les Agents de 1.1 Web AD FS](migrate-the-ad-fs-web-agent.md)



