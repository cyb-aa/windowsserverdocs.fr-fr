---
ms.assetid: 5a1ae56b-adcb-447e-9e34-c0629d7cb241
title: Configurer manuellement un compte de service pour une batterie de serveurs de fédération
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 7d215c80c03236df9479aff8046981741dfc83e2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838150"
---
# <a name="manually-configure-a-service-account-for-a-federation-server-farm"></a>Configurer manuellement un compte de service pour une batterie de serveurs de fédération

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si vous envisagez de configurer un environnement de batterie de serveurs de fédération dans Active Directory Federation Services \(AD FS\), vous devez créer et configurer un compte de service dédié dans les Services de domaine Active Directory \(AD DS\) où résidera la batterie de serveurs. Configurez ensuite chaque serveur de fédération de la batterie pour qu'il utilise ce compte. Lorsque vous souhaitez autoriser les ordinateurs client sur le réseau d’entreprise pour s’authentifier auprès des serveurs de fédération dans une batterie de serveurs AD FS à l’aide de l’authentification intégrée de Windows, vous devez effectuer les tâches suivantes dans votre organisation.  

> [!IMPORTANT]
> À compter d’AD FS 3.0 (Windows Server 2012 R2), AD FS prend en charge l’utilisation d’un [groupe compte de Service administré](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview) \(gMSA\) comme compte de service.  Il s’agit de l’option recommandée, car il supprime la nécessité de gérer le mot de passe de compte de service au fil du temps.  Ce document décrit le cas de remplacement de l’utilisation d’un compte de service traditionnel, comme dans les domaines en cours d’exécution un Windows Server 2008 R2 ou le plus haut niveau fonctionnel du domaine \(niveau fonctionnel du domaine\).

> [!NOTE]  
> Vous ne devez exécuter les tâches de cette procédure qu’une seule fois pour l’ensemble de la batterie de serveurs de fédération. Plus tard, lorsque vous créez un serveur de fédération à l’aide de l’Assistant Configuration du serveur de fédération AD FS, vous devez spécifier le même compte sur le **compte de Service** page de l’Assistant sur chaque serveur de fédération dans la batterie de serveurs.  
  
#### <a name="create-a-dedicated-service-account"></a>Créer un compte de service dédié  
  
1.  Créer un utilisateur dédié\/compte dans la forêt Active Directory qui se trouve dans l’organisation de fournisseur d’identité de service. Ce compte est nécessaire pour le protocole d’authentification Kerberos fonctionne dans un scénario de batterie de serveurs et pour permettre le pass\-via l’authentification sur chacun des serveurs de fédération. Utilisez ce compte uniquement dans le cadre de la batterie de serveurs de fédération.  
  
2.  Modifiez les propriétés du compte d'utilisateur, puis cochez la case **Le mot de passe n'expire jamais**. Cela garantit que ce compte de service pourra continuer d'être utilisé même en cas de changement de mot de passe du domaine.  
  
    > [!NOTE]  
    > L'utilisation du compte de service réseau pour ce compte dédié entraînera aléatoirement des échecs lors de tentatives d'accès réalisées via l'authentification intégrée de Windows, car les tickets Kerberos ne pourront pas être validés d'un serveur à l'autre.  
  
#### <a name="to-set-the-spn-of-the-service-account"></a>Définir le SPN du compte de service  
  
1.  Étant donné que l’identité de pool d’applications pour le pool d’applications AD FS est en cours d’exécution en tant qu’un utilisateur de domaine\/service de compte, vous devez configurer le nom de principal du Service \(SPN\) pour ce compte dans le domaine avec la commande Setspn.exe\-outil en ligne. Setspn.exe est installé par défaut sur les ordinateurs exécutant Windows Server 2008. Exécutez la commande suivante sur un ordinateur qui est joint au même domaine où l’utilisateur\/se trouve le compte de service :  
  
    ```  
    setspn -a host/<server name> <service account>  
    ```  
  
    Par exemple, dans un scénario dans la fédération tous les serveurs sont en cluster sous le système de nom de domaine \(DNS\) fs.fabrikam.com de nom d’hôte et le nom du compte de service qui est affecté pour le pool d’applications AD FS est nommé adfs2farm, tapez la commande comme suit, puis appuyez sur ENTRÉE :  
  
    ```  
    setspn -a host/fs.fabrikam.com adfs2farm  
    ```  
  
    Effectuez cette opération une seule fois pour ce compte.  
  
2.  Une fois l’identité du pool d’applications AD FS est modifiée pour le compte de service, définissez les listes de contrôle d’accès \(ACL\) sur la base de données SQL Server pour permettre un accès en lecture à ce nouveau compte afin que le pool d’applications AD FS peut lire les données de la stratégie.  
  

