---
title: Migrer une batterie de serveurs WID AD FS 2,0 Federation Server
description: Fournit des informations sur la migration d’une batterie de serveurs AD FS 2,0 Server WID vers Windows Server 2012
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 89da3de4bc626e12a1fc34752841f2de1afb5322
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408247"
---
# <a name="migrate-an-ad-fs-20-wid-farm"></a>Migrer une batterie de serveurs AD FS 2,0 WID  
Ce document fournit des informations détaillées sur la migration d’une batterie de serveurs de base de données interne Windows (WID) AD FS 2,0 vers Windows Server 2012.

## <a name="migrate-an-ad-fs-wid-farm"></a>Migrer une batterie de serveurs AD FS WID
Pour migrer une batterie de serveurs WID vers Windows Server 2012, procédez comme suit :  
  
1.  Pour chaque nœud (serveur) de la batterie de serveurs WID, examinez et exécutez les procédures décrites dans [préparer la migration d’une batterie de serveurs wid](prepare-to-migrate-a-wid-farm.md).  
  
2.  Supprimez tout nœud non principal de l'équilibrage de charge.  
  
3.  Mise à niveau du système d’exploitation sur ce serveur de Windows Server 2008 R2 ou Windows Server 2008 vers Windows Server 2012. Pour plus d’informations, voir [Installation de Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Suite à la mise à niveau du système d’exploitation, la configuration AD FS sur ce serveur est perdue et le rôle serveur AD FS 2,0 est supprimé. Le rôle serveur AD FS Windows Server 2012 est installé à la place, mais il n’est pas configuré. Vous devez créer la configuration de AD FS d’origine et restaurer les paramètres de AD FS restants pour terminer la migration du serveur de Fédération.  
  
4. Créez la configuration de AD FS d’origine sur ce serveur.  
  
Vous pouvez créer la configuration de AD FS d’origine à l’aide de l' **Assistant Configuration du serveur de fédération AD FS** pour ajouter un serveur de Fédération à une batterie de serveurs WID. Pour plus d'informations, consultez [Ajouter un serveur de fédération à une batterie de serveurs de fédération](add-a-federation-server-to-a-federation-server-farm.md).  
  
> [!NOTE]
> Quand vous atteignez la page **spécifier le serveur de Fédération principal et un compte de service** dans l' **Assistant Configuration du serveur de fédération AD FS**, entrez le nom du serveur de Fédération principal de la batterie de serveurs wid et veillez à entrer le compte de service. les informations que vous avez enregistrées lors de la préparation de la migration de AD FS. Pour plus d’informations, consultez [préparer la migration du serveur de fédération AD FS 2,0](prepare-to-migrate-a-wid-farm.md). 
>  
> Quand vous atteignez la page **spécifier le nom du service FS (Federation Service)** , veillez à sélectionner le même certificat SSL que celui que vous avez enregistré dans la section « préparer la migration d’une batterie de serveurs wid » dans [préparer la migration du serveur de fédération AD FS 2,0](prepare-to-migrate-a-wid-farm.md).  
  
5. Mettez à jour vos pages web AD FS sur ce serveur. Si vous avez sauvegardé vos pages Web AD FS personnalisées pendant la préparation de la migration, vous devez utiliser vos données de sauvegarde pour remplacer les pages Web par défaut AD FS qui ont été créées par défaut dans le répertoire **%systemdrive%\inetpub\adfs\ls** à la suite de l’AD FS configuration sur Windows Server 2012.  
  
6. Ajoutez le serveur que vous venez de mettre à niveau vers Windows Server 2012 vers l’équilibreur de charge.  
  
7. Répétez les étapes 1 à 6 pour les autres serveurs secondaires de votre batterie de serveurs WID.  
  
8. Promouvez l'un des serveurs secondaires mis à niveau au rang de serveur principal de votre batterie utilisant la base de données interne Windows. Pour ce faire, ouvrez Windows PowerShell et exécutez la commande suivante : `PSH:> Set-AdfsSyncProperties –Role PrimaryComputer`.  
  
9. Supprimez de l'équilibrage de charge le serveur principal d'origine de votre batterie utilisant la base de données interne Windows.  
  
10. À l'aide de Windows PowerShell, rétrogradez au rang de serveur secondaire le serveur principal d'origine de votre batterie utilisant la base de données interne Windows. Ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande AD FS à votre session Windows PowerShell : `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Exécutez ensuite la commande suivante pour rétrograder le serveur principal d’origine en serveur secondaire : `PSH:> Set-AdfsSyncProperties – Role SecondaryComputer –PrimaryComputerName <FQDN of the Primary Federation Server>`.  
  
11. Mise à niveau du système d’exploitation sur ce dernier nœud (serveur) de votre batterie de serveurs WID à partir de Windows Server 2008 R2 ou Windows Server 2008 vers Windows Server 2012. Pour plus d’informations, voir [Installation de Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Suite à la mise à niveau du système d’exploitation, la configuration AD FS sur ce serveur est perdue et le rôle serveur AD FS 2,0 est supprimé. Le rôle serveur AD FS Windows Server 2012 est installé à la place, mais il n’est pas configuré. Vous devez créer manuellement la configuration de AD FS d’origine et restaurer les paramètres de AD FS restants pour terminer la migration du serveur de Fédération.  
  
12. Créez la configuration de AD FS d’origine sur ce dernier nœud (serveur) de votre batterie de serveurs WID.  
  
Vous pouvez créer la configuration de AD FS d’origine à l’aide de l' **Assistant Configuration du serveur de fédération AD FS** pour ajouter un serveur de Fédération à une batterie de serveurs WID. Pour plus d'informations, consultez [Ajouter un serveur de fédération à une batterie de serveurs de fédération](add-a-federation-server-to-a-federation-server-farm.md).  
  
> [!NOTE]
> Quand vous atteignez la page **spécifier le serveur de Fédération principal et un compte de service** dans l' **Assistant Configuration du serveur de fédération AD FS**, entrez les informations du compte de service que vous avez enregistrées lors de la préparation de la migration de AD FS. Pour plus d’informations, consultez [préparer la migration du serveur de fédération AD FS 2,0](prepare-to-migrate-a-wid-farm.md). 
>  
> Quand vous atteignez la page **spécifier le nom du service FS (Federation Service)** , veillez à sélectionner le même certificat SSL que celui que vous avez enregistré dans [préparer la migration du serveur de fédération AD FS 2,0](prepare-to-migrate-a-wid-farm.md).  
  
13. Mettez à jour vos pages Web AD FS sur ce dernier serveur dans votre batterie WID. Si vous avez sauvegardé vos pages Web AD FS personnalisées pendant la préparation de la migration, utilisez vos données de sauvegarde pour remplacer les pages Web par défaut AD FS qui ont été créées par défaut dans le répertoire **%systemdrive%\inetpub\adfs\ls** à la suite de l’AD FS configuration sur Windows Server 2012.  
  
14. Ajoutez le dernier serveur de votre batterie de serveurs WID que vous venez de mettre à niveau vers Windows Server 2012 vers l’équilibreur de charge.  
  
15. Restaurez toutes les personnalisations AD FS restantes, telles que les magasins d'attributs personnalisés.  
  
## <a name="next-steps"></a>Étapes suivantes
 [Préparer la migration du serveur de fédération AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Préparer la migration du serveur proxy de fédération AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrer le serveur de fédération AD FS 2,0](migrate-the-ad-fs-fed-server.md)   
 [Migrer le serveur proxy de fédération AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrer les agents web AD FS 1.1](migrate-the-ad-fs-web-agent.md)