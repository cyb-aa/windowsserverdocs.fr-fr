---
title: "Migrer une batterie WID serveur AD FS 2.0 de fédération"
description: "Fournit des informations sur la migration d’une batterie de serveurs AD FS 2.0 server WID vers Windows Server 2012"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: dbef7d07041a1fd32656c95947d5202b566c068a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>Migrer une batterie AD FS 2.0 WID  
Ce document fournit des informations détaillées sur la migration d’une annonce sur la batterie de serveurs ADFS 2.0 Windows de base de données interne de Windows Server 2012.

## <a name="migrate-an-ad-fs-wid-farm"></a>Migrer une batterie AD FS WID
Pour migrer une batterie WID vers Windows Server 2012, effectuez la procédure suivante:  
  
1.  Pour chaque nœud (serveur) de la batterie WID, consultez et effectuez les procédures de [préparer la migration d’une batterie WID](prepare-to-migrate-a-wid-farm.md).  
  
2.  Supprimez tous les nœuds secondaires à partir de l’équilibrage de charge.  
  
3.  Mettre à niveau du système d’exploitation sur ce serveur à partir de Windows Server 2008 R2 ou Windows Server 2008 vers Windows Server 2012. Pour plus d’informations, voir [l’installation de Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Le résultat de la mise à niveau du système d’exploitation, la configuration AD FS sur ce serveur est perdue et le rôle de serveur 2.0 AD FS est supprimé. Le rôle de serveur AD FS Windows Server 2012 est installé à la place, mais il n’est pas configuré. Vous devez créer la configuration AD FS d’origine et restaurer les paramètres AD FS restants pour terminer la migration de serveur de fédération.  
  
4.  Créez la configuration AD FS d’origine sur ce serveur.  
  
Vous pouvez créer la configuration AD FS d’origine à l’aide de la **Assistant Configuration du serveur de fédération AD FS** pour ajouter un serveur de fédération à une batterie WID. Pour plus d’informations, voir [ajouter un serveur de fédération à une batterie de serveurs de fédération](add-a-federation-server-to-a-federation-server-farm.md).  
  
> [!NOTE]
> Lorsque vous atteignez le **spécifier le serveur de fédération principal et un compte de Service** page dans le **Assistant Configuration du serveur de fédération AD FS**, entrez le nom du serveur de fédération principal de la batterie WID et veillez à entrer les informations de compte de service que vous avez enregistrées pendant la préparation de la migration AD FS. Pour plus d’informations, voir [préparer la migration AD FS 2.0 de serveur de fédération](prepare-to-migrate-a-wid-farm.md). 
>  
> Lorsque vous atteignez le **spécifier le nom de Service de fédération** page, veillez à sélectionner le certificat SSL que vous avez enregistrée à le «préparer la migration d’une batterie WID» dans [préparer la migration AD FS 2.0 de serveur de fédération](prepare-to-migrate-a-wid-farm.md).  
  
5.  Mettre à jour vos pages Web AD FS sur ce serveur. Si vous avez sauvegardé vos pages Web AD FS personnalisées pendant la préparation de la migration, vous devez utiliser vos données de sauvegarde pour remplacer les pages Web AD FS qui ont été créés par défaut dans le **%systemdrive%\inetpub\adfs\ls** répertoire suite à la configuration AD FS dans Windows Server 2012.  
  
6.  Ajoutez le serveur simplement mis à niveau vers Windows Server 2012 à l’équilibrage de charge.  
  
7.  Répétez les étapes 1 à 6 pour les serveurs secondaires restants de votre batterie WID.  
  
8.  Promouvoir l’un des serveurs secondaires mis à niveau vers le serveur principal de votre batterie WID. Pour ce faire, ouvrez Windows PowerShell et exécutez la commande suivante: `PSH:> Set-AdfsSyncProperties –Role PrimaryComputer`.  
  
9. Supprimez le serveur principal d’origine de votre batterie WID l’équilibrage de charge.  
  
10. Rétrograder le serveur principal d’origine de votre batterie WID soit un serveur secondaire à l’aide de Windows PowerShell. Ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande AD FS à votre session Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Puis exécutez la commande suivante pour rétrograder le serveur principal d’origine pour être un serveur secondaire: `PSH:> Set-AdfsSyncProperties – Role SecondaryComputer –PrimaryComputerName <FQDN of the Primary Federation Server>`.  
  
11. Mettre à niveau du système d’exploitation sur ce dernier nœud (serveur) de votre batterie WID à partir de Windows Server 2008 R2 ou Windows Server 2008 vers Windows Server 2012. Pour plus d’informations, voir [l’installation de Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Le résultat de la mise à niveau le système d’exploitation, la configuration AD FS sur ce serveur est perdue et le rôle de serveur 2.0 AD FS est supprimé. Le rôle de serveur AD FS Windows Server 2012 est installé à la place, mais il n’est pas configuré. Vous devez manuellement créer la configuration AD FS d’origine et restaurer les paramètres AD FS restants pour terminer la migration de serveur de fédération.  
  
12. Créer la configuration AD FS d’origine sur ce dernier nœud (serveur) de votre batterie WID.  
  
Vous pouvez créer la configuration AD FS d’origine à l’aide de la **Assistant Configuration du serveur de fédération AD FS** pour ajouter un serveur de fédération à une batterie WID. Pour plus d’informations, voir [ajouter un serveur de fédération à une batterie de serveurs de fédération](add-a-federation-server-to-a-federation-server-farm.md).  
  
> [!NOTE]
> Lorsque vous atteignez le **spécifier le serveur de fédération principal et un compte de Service** page dans le **Assistant Configuration du serveur de fédération AD FS**, entrez les informations de compte de service que vous avez enregistrées pendant la préparation de la migration AD FS. Pour plus d’informations, voir [préparer la migration AD FS 2.0 de serveur de fédération](prepare-to-migrate-a-wid-farm.md). 
>  
> Lorsque vous atteignez le **spécifier le nom de Service de fédération** page, veillez à sélectionner le certificat SSL que vous avez enregistrée à [préparer la migration AD FS 2.0 de serveur de fédération](prepare-to-migrate-a-wid-farm.md).  
  
13. Mettre à jour vos pages Web AD FS sur ce dernier serveur dans votre batterie WID. Si vous avez sauvegardé vos pages Web AD FS personnalisées pendant la préparation de la migration, utiliser vos données de sauvegarde pour remplacer les pages Web AD FS qui ont été créés par défaut dans le **%systemdrive%\inetpub\adfs\ls** répertoire suite à la configuration AD FS dans Windows Server 2012.  
  
14. Ajoutez ce dernier serveur de votre batterie WID simplement mis à niveau vers Windows Server 2012 à l’équilibrage de charge.  
  
15. Restaurez toutes les personnalisations AD FS restantes, telles que les magasins d’attributs personnalisés.  
  
## <a name="next-steps"></a>Étapes suivantes
 [Préparer la migration du serveur AD FS 2.0 de fédération](prepare-to-migrate-ad-fs-fed-server.md)   
 [Préparer la migration AD FS 2.0 serveur Proxy de fédération](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrer le serveur AD FS 2.0 de fédération](migrate-the-ad-fs-fed-server.md)   
 [Migrer le serveur Proxy AD FS 2.0 de fédération](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrer les Agents de 1.1 Web AD FS](migrate-the-ad-fs-web-agent.md)