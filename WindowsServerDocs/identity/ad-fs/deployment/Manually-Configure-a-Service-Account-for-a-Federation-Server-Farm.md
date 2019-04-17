---
ms.assetid: 5a1ae56b-adcb-447e-9e34-c0629d7cb241
title: "Configurer manuellement un compte de Service pour une batterie de serveurs de fédération"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5b5a8d198f93772903ea9b0a2b4b01075799bf0f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="manually-configure-a-service-account-for-a-federation-server-farm"></a>Configurer manuellement un compte de Service pour une batterie de serveurs de fédération

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Si vous envisagez de configurer un environnement de batterie de serveurs de fédération dans Active Directory Federation Services \(AD FS\), vous devez créer et configurer un compte de service dédié dans \(AD DS\) des Services de domaine Active Directory où résidera la batterie de serveurs. Puis, vous configurez chaque serveur de fédération dans la batterie de serveurs à utiliser ce compte. Lorsque vous souhaitez autoriser les ordinateurs client sur le réseau d’entreprise pour s’authentifier auprès des serveurs de fédération dans une batterie AD FS à l’aide de l’authentification Windows intégrée, vous devez effectuer les tâches suivantes dans votre organisation.  
  
> [!NOTE]  
> Vous devez effectuer les tâches de cette procédure qu’une seule fois pour la batterie de serveurs de fédération entière. Ultérieurement, lorsque vous créez un serveur de fédération à l’aide de l’Assistant Configuration du serveur de fédération AD FS, vous devez spécifier ce même compte sur le **compte de Service** page de l’Assistant sur chaque serveur de fédération dans la batterie de serveurs.  
  
#### <a name="create-a-dedicated-service-account"></a>Créer un compte de service dédié  
  
1.  Créer un compte de service/par l’utilisateur dédié dans la forêt Active Directory qui se trouve dans l’organisation de fournisseur d’identité. Ce compte est nécessaire pour le protocole d’authentification Kerberos de fonctionner dans un scénario de batterie de serveurs et pour permettre l’authentification via pass\ sur chacun des serveurs de fédération. Utilisez ce compte uniquement dans le cadre de la batterie de serveurs de fédération.  
  
2.  Modifier les propriétés du compte utilisateur, puis sélectionnez le **mot de passe n’expire jamais** case à cocher. Cette action garantit que la fonction de ce compte de service n’est pas interrompue en raison des exigences de modification de mot de passe de domaine.  
  
    > [!NOTE]  
    > À l’aide du compte Service réseau pour ce compte dédié entraînera aléatoirement des échecs lors de tentatives d’accès par le biais de l’authentification Windows intégrée, à la suite de tickets Kerberos ne pas validés d’un serveur à un autre.  
  
#### <a name="to-set-the-spn-of-the-service-account"></a>Pour définir le SPN du compte de service  
  
1.  Étant donné que l’identité du pool d’applications pour le pool d’applications AD FS s’exécute comme un compte de service/par l’utilisateur de domaine, vous devez configurer le nom Principal de Service \(SPN\) pour ce compte dans le domaine avec l’outil de ligne de commande Setspn.exe. Setspn.exe est installé par défaut sur les ordinateurs exécutant Windows Server 2008. Sur un ordinateur qui est joint au même domaine dans lequel réside le compte de service/par l’utilisateur, exécutez la commande suivante:  
  
    ```  
    setspn -a host/<server name> <service account>  
    ```  
  
    Par exemple, dans un scénario dans lequel tous les serveurs de fédération sont en cluster sous le nom d’hôte de système de nom de domaine \(DNS\) fs.fabrikam.com et le nom du compte de service qui est attribué pour le pool d’applications AD FS s’appelle adfs2farm, tapez la commande suivante et appuyez sur ENTRÉE:  
  
    ```  
    setspn -a host/fs.fabrikam.com adfs2farm  
    ```  
  
    Il est nécessaire terminer cette tâche une seule fois pour ce compte.  
  
2.  Une fois l’identité du pool d’applications AD FS est modifiée pour le compte de service, définir l’accès contrôle listes \(ACLs\) sur la base de données SQL Server pour autoriser l’accès en lecture à ce nouveau compte afin que le pool d’applications AD FS peut lire les données de stratégie.  
  

