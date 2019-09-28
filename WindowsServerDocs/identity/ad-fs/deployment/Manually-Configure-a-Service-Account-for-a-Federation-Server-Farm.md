---
ms.assetid: 5a1ae56b-adcb-447e-9e34-c0629d7cb241
title: Configurer manuellement un compte de service pour une batterie de serveurs de fédération
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8240903b3c446d4f02ca93dc053e520480f5e8ca
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359491"
---
# <a name="manually-configure-a-service-account-for-a-federation-server-farm"></a>Configurer manuellement un compte de service pour une batterie de serveurs de fédération

Si vous envisagez de configurer un environnement de batterie de serveurs de Fédération dans Services ADFS \(AD FS @ no__t-1, vous devez créer et configurer un compte de service dédié dans Active Directory Domain Services \(AD DS @ no__t-3 où la batterie de serveurs va résider. Configurez ensuite chaque serveur de fédération de la batterie pour qu'il utilise ce compte. Vous devez effectuer les tâches suivantes dans votre organisation lorsque vous souhaitez autoriser les ordinateurs clients du réseau d’entreprise à s’authentifier auprès de l’un des serveurs de Fédération d’une batterie de serveurs AD FS à l’aide de l’authentification intégrée de Windows.  

> [!IMPORTANT]
> À partir de AD FS 3,0 (Windows Server 2012 R2), AD FS prend en charge l’utilisation d’un [compte de service géré de groupe](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview) \(gMSA @ no__t-2 comme compte de service.  Il s’agit de l’option recommandée, car elle élimine la nécessité de gérer le mot de passe du compte de service au fil du temps.  Ce document décrit l’autre cas d’utilisation d’un compte de service traditionnel, par exemple dans les domaines qui exécutent toujours un niveau fonctionnel de domaine Windows Server 2008 R2 ou antérieur \(DFL @ no__t-1.

> [!NOTE]  
> Vous ne devez exécuter les tâches de cette procédure qu’une seule fois pour l’ensemble de la batterie de serveurs de fédération. Plus tard, lorsque vous créerez un serveur de Fédération à l’aide de l’Assistant Configuration du serveur de fédération AD FS, vous devez spécifier ce même compte sur la page de l’Assistant **compte de service** sur chaque serveur de Fédération de la batterie de serveurs.  
  
#### <a name="create-a-dedicated-service-account"></a>Créer un compte de service dédié  
  
1.  Créez un compte User @ no__t-0Service dédié dans la forêt Active Directory qui se trouve dans l’organisation du fournisseur d’identité. Ce compte est nécessaire pour que le protocole d’authentification Kerberos fonctionne dans un scénario de batterie de serveurs et autorise l’authentification Pass @ no__t-0through sur chacun des serveurs de Fédération. Utilisez ce compte uniquement dans le cadre de la batterie de serveurs de Fédération.  
  
2.  Modifiez les propriétés du compte d'utilisateur, puis cochez la case **Le mot de passe n'expire jamais**. Cela garantit que ce compte de service pourra continuer d'être utilisé même en cas de changement de mot de passe du domaine.  
  
    > [!NOTE]  
    > L'utilisation du compte de service réseau pour ce compte dédié entraînera aléatoirement des échecs lors de tentatives d'accès réalisées via l'authentification intégrée de Windows, car les tickets Kerberos ne pourront pas être validés d'un serveur à l'autre.  
  
#### <a name="to-set-the-spn-of-the-service-account"></a>Définir le SPN du compte de service  
  
1.  Étant donné que l’identité du pool d’applications pour l’AD FS AppPool s’exécute en tant que compte d’utilisateur de domaine @ no__t-0Service, vous devez configurer le nom de principal du service \(SPN @ no__t-2 pour ce compte dans le domaine à l’aide de la commande SetSPN. exe @ no__t-3line. Setspn. exe est installé par défaut sur les ordinateurs exécutant Windows Server 2008. Exécutez la commande suivante sur un ordinateur joint au même domaine que celui où se trouve le compte User @ no__t-0Service :  
  
    ```  
    setspn -a host/<server name> <service account>  
    ```  
  
    Par exemple, dans un scénario dans lequel tous les serveurs de Fédération sont mis en cluster sous le nom de domaine System \(DNS @ no__t-1 Host Name fs.fabrikam.com et le nom de compte de service affecté à l’AD FS AppPool est nommé adfs2farm, tapez la commande comme suit et Appuyez ensuite sur entrée :  
  
    ```  
    setspn -a host/fs.fabrikam.com adfs2farm  
    ```  
  
    Effectuez cette opération une seule fois pour ce compte.  
  
2.  Une fois que la AD FS l’identité AppPool est remplacée par le compte de service, définissez les listes de contrôle d’accès \(ACLs @ no__t-1 sur la base de données SQL Server pour autoriser l’accès en lecture à ce nouveau compte afin que le pool d’AD FS puisse lire les données de stratégie.  
  

