---
title: Migrer une batterie de serveurs de fédération AD FS 2,0 SQL Server
description: Fournit des informations sur la migration d’une batterie de serveurs SQL Server AD FS 2,0 vers Windows Server 2012
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 3c43d26868f39896ec8632397dc0fce1dfe2dd2a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408282"
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>Migrer une batterie de serveurs AD FS 2,0 WID  
Ce document fournit des informations détaillées sur la migration d’une batterie de serveurs SQL AD FS 2,0 vers Windows Server 2012.


## <a name="migrate-a-sql-server-farm"></a>Migrer une batterie SQL Server  
 Pour migrer une batterie de SQL Server vers Windows Server 2012, procédez comme suit :  
  
1.  Pour chaque serveur de votre batterie de SQL Server, examinez et exécutez les procédures décrites dans [migrer une batterie de SQL Server](prepare-to-migrate-a-sql-server-farm.md).  
  
2.  Supprimez de l'équilibrage de charge tout serveur de votre batterie SQL Server.  
  
3.  Mettez à niveau le système d’exploitation de ce serveur dans votre batterie de serveurs SQL Server à partir de Windows Server 2008 R2 ou Windows Server 2008 vers Windows Server 2012. Pour plus d’informations, voir [Installation de Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Suite à la mise à niveau du système d’exploitation, la configuration AD FS sur ce serveur est perdue et le rôle serveur AD FS 2,0 est supprimé. Le rôle serveur AD FS Windows Server 2012 est installé à la place, mais il n’est pas configuré. Vous devez créer manuellement la configuration de AD FS d’origine et restaurer les paramètres de AD FS restants pour terminer la migration du serveur de Fédération.  
  
4. Créez la configuration de AD FS d’origine sur ce serveur dans votre batterie de serveurs SQL Server en utilisant AD FS applets de commande Windows PowerShell pour ajouter un serveur à une batterie de serveurs existante.  
  
> [!IMPORTANT]
>  Vous devez utiliser Windows PowerShell pour créer la configuration de AD FS d’origine si vous utilisez SQL Server pour stocker votre base de données de configuration de AD FS.  

  - Ouvrez Windows PowerShell et exécutez la commande suivante : `$fscredential = Get-Credential`.  
  - Entrez le nom et le mot de passe du compte de service que vous avez enregistré pendant la préparation de la migration de votre batterie SQL Server.  
  - Exécutez la commande suivante : `Add-AdfsFarmNode -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<Data Source>;Integrated Security=True"`, où `Data Source` est la valeur de la source de données dans la valeur de la chaîne de connexion du magasin de stratégies dans le fichier suivant : `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`.  
  
5. Ajoutez le serveur que vous venez de mettre à niveau vers Windows Server 2012 vers l’équilibreur de charge.  
  
6. Répétez les étapes 2 à 6 pour les nœuds restants de votre batterie de SQL Server.  
  
7. Lorsque tous les serveurs de votre batterie de SQL Server sont mis à niveau vers Windows Server 2012, restaurez les personnalisations de AD FS restantes, telles que les magasins d’attributs personnalisés.  

## <a name="next-steps"></a>Étapes suivantes
 [Préparer la migration du serveur de fédération AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Préparer la migration du serveur proxy de fédération AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrer le serveur de fédération AD FS 2,0](migrate-the-ad-fs-fed-server.md)   
 [Migrer le serveur proxy de fédération AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrer les agents web AD FS 1.1](migrate-the-ad-fs-web-agent.md)



