---
title: Connexion dans Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 05/07/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 149a5d34-43b7-4b9e-99e7-9f2294ab9ddb
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 98292e0715ea662712e596d89646a735e22ba3f9
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435977"
---
# <a name="get-connected-in-windows-server-essentials"></a>Connexion dans Windows Server Essentials

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 Vous pouvez connecter vos ordinateurs au serveur Windows Server Essentials en utilisant le logiciel Connecteur. Le logiciel Connecteur est installé quand vous connectez un ordinateur au serveur à l'aide de l'Assistant Connexion d'un ordinateur au serveur. Vous pouvez démarrer cet Assistant en tapant **http://<servername\>/ se connecter**, où **< nom_serveur\>**  est le nom de votre serveur.  

 Dans cette rubrique :  


-   [Préparer la connexion des ordinateurs au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_A)  

-   [Connecter des ordinateurs au serveur en utilisant le logiciel Connecteur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_B)  

-   [Utiliser Launchpad](Get-Connected-in-Windows-Server-Essentials.md#BKMK_C)  

-   [Préparer la connexion des ordinateurs au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_A)  

-   [Connecter des ordinateurs au serveur en utilisant le logiciel Connecteur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_B)  

-   [Utiliser Launchpad](Get-Connected-in-Windows-Server-Essentials.md#BKMK_C)  


##  <a name="BKMK_A"></a> Préparer la connexion des ordinateurs au serveur  
 Cette section décrit le logiciel Connecteur, les systèmes d'exploitation pris en charge par Windows Server Essentials, les conditions préalables à remplir avant de connecter vos ordinateurs au serveur, ainsi que les changements apportés par le serveur aux ordinateurs quand vous exécutez le logiciel Connecteur.  


-   [Vue d’ensemble du logiciel de connecteur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_1)  

-   [Conditions préalables pour connecter un ordinateur au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_2)  

-   [Conditions préalables pour connecter un ordinateur Mac au réseau](Get-Connected-in-Windows-Server-Essentials.md#BKMK_3)  

-   [Systèmes d’exploitation pris en charge pour les ordinateurs clients](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4)  

-   [Modifications apportées par le serveur à un ordinateur client](Get-Connected-in-Windows-Server-Essentials.md#BKMK_5)  

-   [Informations de nom et mot de passe utilisateur réseau](Get-Connected-in-Windows-Server-Essentials.md#BKMK_6)  

-   [Compte d’administrateur de serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_7)  

-   [Supprimer un ordinateur d’un domaine Windows](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)  

###  <a name="BKMK_1"></a> Vue d’ensemble du logiciel de connecteur  
 Le logiciel Connecteur du système d'exploitation Windows Server Essentials connecte les ordinateurs de votre réseau au serveur Windows Server Essentials. Quand vous connectez des ordinateurs au serveur, le logiciel Connecteur vous permet de sauvegarder automatiquement les ordinateurs et de surveiller leur intégrité. Le logiciel Connecteur vous permet également de configurer et d'administrer à distance le serveur Windows Server Essentials. Le logiciel Connecteur est installé quand vous connectez un ordinateur client au serveur. Pour obtenir des instructions détaillées sur la connexion des ordinateurs clients au serveur Windows Server Essentials, consultez la section [Connect computers to the server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9) plus loin dans cette rubrique.  

-   [Vue d’ensemble du logiciel de connecteur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_1)  

-   [Conditions préalables pour connecter un ordinateur au serveur](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_2)  

-   [Conditions préalables pour connecter un ordinateur Mac au réseau](Get-Connected-in-Windows-Server-Essentials.md#BKMK_3)  

-   [Systèmes d’exploitation pris en charge pour les ordinateurs clients](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4)  

-   [Modifications apportées par le serveur à un ordinateur client](Get-Connected-in-Windows-Server-Essentials.md#BKMK_5)  

-   [Informations de nom et mot de passe utilisateur réseau](Get-Connected-in-Windows-Server-Essentials.md#BKMK_6)  

-   [Compte d’administrateur de serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_7)  

-   [Supprimer un ordinateur d’un domaine Windows](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)  

###  <a name="BKMK_1"></a> Vue d’ensemble du logiciel de connecteur  
 Le logiciel Connecteur du système d'exploitation Windows Server Essentials connecte les ordinateurs de votre réseau au serveur Windows Server Essentials. Quand vous connectez des ordinateurs au serveur, le logiciel Connecteur vous permet de sauvegarder automatiquement les ordinateurs et de surveiller leur intégrité. Le logiciel Connecteur vous permet également de configurer et d'administrer à distance le serveur Windows Server Essentials. Le logiciel Connecteur est installé quand vous connectez un ordinateur client au serveur. Pour obtenir des instructions détaillées sur la connexion des ordinateurs clients au serveur Windows Server Essentials, consultez la section [Connect computers to the server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9) plus loin dans cette rubrique.  


###  <a name="BKMK_2"></a> Conditions préalables pour connecter un ordinateur au serveur  
 Vous devez remplir les conditions requises suivantes pour pouvoir connecter un ordinateur au réseau :  

-   L'installation de Windows Server Essentials est finie, et le serveur est en cours d'exécution. Le logiciel Connecteur quitte son programme d'installation, s'il ne peut pas communiquer avec le serveur.  


-   L'ordinateur client exécute un système d'exploitation pris en charge. Pour plus d'informations, voir [Systèmes d'exploitation pris en charge pour les ordinateurs clients](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4).


-   L'ordinateur client doit disposer d'une connexion valide à Internet.  

-   L'ordinateur client se trouve sur le même sous-réseau IP que le serveur qui exécute Windows Server Essentials quand l'ordinateur client se trouve sur le même réseau que le serveur.  

-   .NET Framework 4.5 est installé sur l'ordinateur client. Le logiciel Connecteur l'installe automatiquement sur l'ordinateur.  

-   L'ordinateur client répond aux conditions minimales concernant le système :  

    -   Processeur 1,4 GHz ou plus rapide  

    -   1 Go de RAM minimum  

    -   1 Go d'espace disque disponible (une partie de cet espace est libérée après l'installation)  

-   La partition de démarrage (c'est-à-dire, la partition de disque où est installé le système d'exploitation Windows) est formatée avec le système de fichiers NTFS.  

-   Le nom d'ordinateur ne dépasse pas 15 caractères.  

-   Le nom d'ordinateur n'inclut pas de trait de soulignement (_).  

-   Les paramètres de date et d'heure de l'ordinateur sont alignés sur les paramètres du serveur.  

-   Un ordinateur client ne peut être connecté qu'à un seul serveur Windows Server Essentials à la fois.  

-   Un ordinateur client qui est déjà membre d'un autre domaine Active Directory ne peut pas se joindre à un domaine Windows Server Essentials.  

> [!NOTE]
> 
>  Dans un déploiement de client local pour Windows Server Essentials ou Windows Server Essentials, vous pouvez connecter des ordinateurs au serveur sans les ajouter au domaine Windows Server Essentials. Cette méthode n'est pas disponible pour tous les systèmes d'exploitation clients pris en charge. Les fonctionnalités, telles que la stratégie de groupe et les réseaux privés virtuels (VPN), qui nécessitent qu'un ordinateur soit connecté au domaine, ne sont pas disponibles. Pour plus d’informations sur les exigences et obtenir des instructions, consultez [Connect computers to a Windows Server Essentials server without joining the domain](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  

 Pour obtenir des instructions pas à pas sur la connexion d'un ordinateur au serveur exécutant Windows Server Essentials, consultez [Connecter des ordinateurs au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

>  Dans un déploiement de client local pour Windows Server Essentials ou Windows Server Essentials, vous pouvez connecter des ordinateurs au serveur sans les ajouter au domaine Windows Server Essentials. Cette méthode n'est pas disponible pour tous les systèmes d'exploitation clients pris en charge. Les fonctionnalités, telles que la stratégie de groupe et les réseaux privés virtuels (VPN), qui nécessitent qu'un ordinateur soit connecté au domaine, ne sont pas disponibles. Pour plus d’informations sur les exigences et obtenir des instructions, consultez [Connect computers to a Windows Server Essentials server without joining the domain](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  

 Pour obtenir des instructions pas à pas sur la connexion d'un ordinateur au serveur exécutant Windows Server Essentials, consultez [Connecter des ordinateurs au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  


###  <a name="BKMK_3"></a> Conditions préalables pour connecter un ordinateur Mac au réseau  
 Vous devez remplir les conditions requises suivantes pour pouvoir connecter un ordinateur Mac au réseau :  

-   L'installation du système d'exploitation serveur est finie, et le serveur est en cours d'exécution. Le logiciel Connecteur ne s'installe pas, s'il ne peut pas communiquer avec le serveur.  

-   L'ordinateur exécute Mac OS X 10.5 (Leopard) ou une version ultérieure.  

-   L'ordinateur est sur le même sous-réseau IP que le serveur.  

-   L'ordinateur doit disposer d'une connexion valide à Internet.  

-   Vérifiez que l'ordinateur répond aux conditions minimales concernant le système :  

    -   Processeur 1,4 GHz ou plus rapide  

    -   1 Go de RAM minimum  

    -   1 Go d'espace disque disponible (une partie de cet espace est libérée après l'installation)  

-   Un ordinateur client ne peut être connecté qu'à un seul serveur à la fois.  

###  <a name="BKMK_4"></a> Systèmes d’exploitation pris en charge pour les ordinateurs clients  
 Windows Server Essentials fournit le même ensemble de fonctionnalités pour tous les ordinateurs clients pris en charge. Ces fonctionnalités comprennent la jonction de domaine, Launchpad et les notifications d'intégrité côté client.  

> [!IMPORTANT]
>  Windows Server Essentials ne prend pas en charge la jonction des ordinateurs exécutant les éditions Familiale, Starter ou Media Center de Windows au domaine. Par ailleurs, vous ne pouvez pas utiliser l'accès web à distance pour vous connecter à ces ordinateurs.  

#### <a name="windows-server-essentials"></a>Windows Server Essentials  
  Cette section s’applique à un serveur exécutant Windows Server Essentials, ou à un serveur exécutant Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter avec le rôle expérience Windows Server Essentials installé. Les systèmes d’exploitation suivants sont pris en charge :  

 **Systèmes d’exploitation Windows 7**  

- SP1 base accueil de Windows 7 (x 86 et x64)  

- Windows 7 Édition familiale Premium SP1 (x 86 et x64)  

- Windows 7 Professionnel SP1 (x 86 et x64)  

- Windows 7 Ultimate SP1 (x 86 et x64)  

- Windows 7 Enterprise SP1 (x 86 et x64)  

- Windows 7 Starter SP1 (x 86)  

  **Systèmes d’exploitation Windows 8**  

- Windows 8  

- Windows 8 Pro  

- Windows 8 Entreprise  

  **Systèmes d’exploitation Windows 8.1**  

- Windows 8.1  

- Windows 8.1 Professionnel  

- Windows 8.1 Entreprise  

  **Systèmes d’exploitation Windows 10**  

- Windows 10  

- Windows 10 Professionnel  

- Windows 10 Entreprise  

- Windows 10 Éducation  

  **Ordinateurs clients Mac**  

- Mac OS X v10.5 Leopard  

- Mac OS X v10.6 Snow Leopard  

- Mac OS X v10.7 Lion  

- Mac OS X v10.8 Mountain Lion  

> [!NOTE]
>  Vous pouvez afficher l’intégrité et l’état de la sauvegarde d’un ordinateur Mac à partir du tableau de bord Windows Server Essentials. Toutefois, vous ne pouvez pas configurer la sauvegarde de l'ordinateur ou démarrer une sauvegarde à partir du tableau de bord. Par ailleurs, vous ne pouvez pas utiliser l'accès web à distance pour vous connecter à un ordinateur Mac.  

#### <a name="windows-server-essentials"></a>Windows Server Essentials  
  Cette section s’applique à un serveur exécutant Windows Server Essentials. Les systèmes d’exploitation suivants sont pris en charge :  

 **Systèmes d’exploitation Windows 7**  

- Windows 7 Édition Familiale Basique (x86 et x64)  

- Windows 7 Édition familiale Premium (x86 et x64)  

- Windows 7 Professionnel (x86 et x64)  

- Windows 7 Ultimate (x86 et x64)  

- Windows 7 entreprise (x86 et x64)  

- Windows 7 Starter (x86)  

  **Systèmes d’exploitation Windows 8**  

- Windows 8  

- Windows 8 Pro  

- Windows 8 Entreprise  

  **Systèmes d’exploitation Windows 10**  

- Windows 10  

- Windows 10 Professionnel  

- Windows 10 Entreprise  

- Windows 10 Éducation  

  **Ordinateurs clients Mac**  

- Mac OS X v10.5 Leopard  

- Mac OS X v10.6 Snow Leopard  

- Mac OS X v10.7 Lion  

> [!NOTE]
>  Vous pouvez afficher l'état de sauvegarde et d'intégrité d'un ordinateur Mac à partir du tableau de bord Windows Server Essentials. Toutefois, vous ne pouvez pas configurer la sauvegarde de l'ordinateur ou démarrer une sauvegarde à partir du tableau de bord. Par ailleurs, vous ne pouvez pas utiliser l'accès web à distance pour vous connecter à un ordinateur Mac.  

###  <a name="BKMK_5"></a> Modifications apportées par le serveur à un ordinateur client  
 Quand vous connectez un ordinateur au serveur, le logiciel Windows Server Essentials apporte plusieurs changements à l'ordinateur pour que ce dernier et le serveur puissent fonctionner conjointement.  

 Le logiciel effectue les opérations suivantes :  

-   Il installe le logiciel Connecteur sur l'ordinateur.  

-   Il installe Microsoft .NET Framework 4.5 sur l'ordinateur, si cela n'est pas déjà fait.  

-   Il crée des raccourcis sur le Bureau de l'ordinateur vers le tableau de bord et Launchpad.  

-   Il configure les ports du Pare-feu Windows de l'ordinateur pour rendre opérationnelles les fonctionnalités suivantes :  

    -   réseau de base ;  

    -   Services Bureau à distance  

-   Il apporte les changements suivants à l'ordinateur pour faciliter les sauvegardes :  

    -   création de tâches planifiées pour l'exécution de sauvegardes automatiques ;  

    -   installation de services qui gèrent les opérations de sauvegarde du serveur ;  

    -   installation d'un pilote de périphérique virtuel utilisé durant les processus de restauration des fichiers et des dossiers.  

-   Il installe l'Agent d'intégrité pour détecter les problèmes et créer les notifications d'alerte correspondantes.  

-   Il crée des tâches planifiées sur l'ordinateur pour les évaluations d'intégrité périodiques et la synchronisation des définitions d'alerte d'intégrité.  

-   Il ajoute des services à l'ordinateur, que ce dernier utilise pour communiquer avec le serveur et d'autres fonctionnalités Windows Server Essentials.  

-   Il ouvre le port TCP 3389 des ordinateurs clients qui exécutent des clients Windows pour permettre l'exécution des services Bureau à distance.  

-   Déploie un VPN sur l’ordinateur client et fournit une expérience simple clic si la fonctionnalité VPN est activée sur Windows Server Essentials, ou fournit une expérience de connexion automatique si la fonctionnalité VPN est activée sur Windows Server Essentials  


 Pour plus d'informations sur la connexion de votre ordinateur au serveur, voir [Connecter des ordinateurs au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

###  <a name="BKMK_6"></a> Informations de nom et mot de passe utilisateur réseau  
 Vous pouvez obtenir les informations relatives à votre nom d'utilisateur et mot de passe réseau auprès de la personne qui gère votre serveur. Vous pouvez utiliser ces informations d'identification pour connecter votre ordinateur au serveur et accéder aux informations de ce dernier.  

###  <a name="BKMK_6"></a> Informations de nom et mot de passe utilisateur réseau  
 Vous pouvez obtenir les informations relatives à votre nom d'utilisateur et mot de passe réseau auprès de la personne qui gère votre serveur. Vous pouvez utiliser ces informations d'identification pour connecter votre ordinateur au serveur et accéder aux informations de ce dernier. 


 Si vous êtes l'administrateur du serveur, vous pouvez créer les informations d'identification réseau en ajoutant un compte d'utilisateur sous l'onglet **Utilisateurs** du tableau de bord. Pour plus d'informations sur les comptes d'utilisateur, voir [Gérer les comptes d'utilisateur à l'aide du tableau de bord](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8).  

###  <a name="BKMK_7"></a> Compte d’administrateur de serveur  
 Vous devez fournir un nom de compte d'administrateur réseau et son mot de passe pour pouvoir installer le logiciel Connecteur. Un compte d'administrateur réseau permet à l'utilisateur de gérer le réseau local de votre organisation. En outre, il permet de gérer les périphériques réseau tels que les commutateurs et les routeurs.  

 Voici, par exemple, des tâches qui peuvent être effectuées à l'aide d'un compte d'administrateur réseau :  

- installation d'applications en réseau et autres logiciels ;  

- gestion du stockage sur le serveur ;  

- distribution de mises à jour logicielles ;  

- exécution de sauvegardes régulières ;  

- surveillance des activités quotidiennes sur le réseau.  

  Dans Windows Server Essentials, Windows Server Essentials et Windows Server 2012 R2 avec le rôle expérience Windows Server Essentials installé, vous pouvez affecter l’administrateur réseau niveau aux comptes d’utilisateur d’accès. Cela permet d'accorder les autorisations nécessaires pour effectuer des tâches d'administrateur réseau. Quand un utilisateur se voit affecter le niveau d'accès d'administrateur réseau, l'invite du **contrôle d'accès d'utilisateur** s'ouvre pour toute tâche qui nécessite des autorisations d'administrateur.  

###  <a name="BKMK_8"></a> Supprimer un ordinateur d’un domaine Windows  
 Pour supprimer un ordinateur de son domaine, vous êtes invité à indiquer le nom d'utilisateur et le mot de passe du compte de domaine.  

##### <a name="to-remove-a-computer-from-a-windows-domain"></a>Pour supprimer un ordinateur d'un domaine Windows  

1.  Cliquez sur **Démarrer**, cliquez avec le bouton droit sur **Ordinateur**, puis cliquez sur **Propriétés**.  

2.  Sous **Paramètres de nom d'ordinateur, de domaine et de groupe de travail**, cliquez sur **Modifier les paramètres**.  

    > [!NOTE]
    >  Si vous êtes invité à entrer un mot de passe d'administrateur ou une confirmation, tapez le mot de passe de domaine ou sa confirmation.  

3.  Dans la boîte de dialogue **Propriétés système**, cliquez sur l'onglet **Nom de l'ordinateur**, puis sur **Changer**.  

4.  Dans la boîte de dialogue **Modification du nom ou du domaine de l'ordinateur** , sous **Membre de**, cliquez sur **Groupe de travail**, puis effectuez l'une des opérations suivantes :  

    1.  Pour rejoindre un groupe de travail existant, tapez le nom du groupe de travail à rejoindre, puis cliquez sur **OK**.  

    2.  Pour créer un groupe de travail, tapez le nom du groupe de travail à créer, puis cliquez sur **OK**.  

        > [!NOTE]
        >  Votre ordinateur est supprimé du domaine et votre compte d'ordinateur est désactivé dans ce dernier.  

##  <a name="BKMK_B"></a> Connecter des ordinateurs au serveur en utilisant le logiciel Connecteur  
 Cette section permet d'accéder aux procédures et informations qui vous aideront à installer le logiciel Connecteur, à connecter votre ordinateur au serveur et à résoudre les problèmes de connexion des ordinateurs au serveur.  


-   [Connecter des ordinateurs au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)  

-   [Connecter des ordinateurs à un serveur Windows Server Essentials sans joindre le domaine](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)  

-   [Installer le logiciel Connecteur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)  

-   [Déplacer des données de l’ordinateur et les paramètres manuellement](Get-Connected-in-Windows-Server-Essentials.md#BKMK_12)  

-   [Transférer plusieurs profils utilisateur pendant le déploiement de l’ordinateur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Transfer)  

-   [Désinstaller le logiciel Connecteur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)  

-   [Déconnecter votre ordinateur à partir d’ou vous reconnecter à votre ordinateur au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_14)  

-   [Comment fonctionne la sauvegarde avec la mise en veille et modes de mise en veille prolongée](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Sleep)  

-   [Connecter des ordinateurs au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)  

-   [Connecter des ordinateurs à un serveur Windows Server Essentials sans joindre le domaine](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)  

-   [Installer le logiciel Connecteur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)  

-   [Déplacer des données de l’ordinateur et les paramètres manuellement](Get-Connected-in-Windows-Server-Essentials.md#BKMK_12)  

-   [Transférer plusieurs profils utilisateur pendant le déploiement de l’ordinateur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Transfer)  

-   [Désinstaller le logiciel Connecteur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)  

-   [Déconnecter votre ordinateur à partir d’ou vous reconnecter à votre ordinateur au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_14)  

-   [Comment fonctionne la sauvegarde avec la mise en veille et modes de mise en veille prolongée](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Sleep)  


###  <a name="BKMK_9"></a> Connecter des ordinateurs au serveur  
 Lorsque vous vous connectez un ordinateur à un serveur qui exécute Windows Server Essentials ou Windows Server 2012 R2 avec le rôle expérience Windows Server Essentials installé, assurez-vous que votre ordinateur client dispose d’une connexion valide à Internet.  

 Effectuez la procédure suivante sur tous les ordinateurs clients pour les connecter à votre serveur.  

 Pour finir la procédure, vous avez besoin des informations suivantes :  

-   nom d'utilisateur et mot de passe de la personne qui doit utiliser l'ordinateur sur le nouveau réseau ;  

-   nom d'utilisateur et mot de passe du compte d'administrateur local de l'ordinateur.  

> [!NOTE]
> 
>  Dans un déploiement de client local pour Windows Server Essentials ou Windows Server Essentials, vous pouvez connecter des ordinateurs au serveur sans les ajouter au domaine Windows Server Essentials. Cette méthode n'est pas disponible pour tous les systèmes d'exploitation clients pris en charge. Les fonctionnalités, telles que la stratégie de groupe et les réseaux privés virtuels (VPN), qui nécessitent qu'un ordinateur soit connecté au domaine, ne sont pas disponibles. Pour plus d’informations sur les exigences et obtenir des instructions, consultez [Connect computers to a Windows Server Essentials server without joining the domain](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  
> 
>  Dans un déploiement de client local pour Windows Server Essentials ou Windows Server Essentials, vous pouvez connecter des ordinateurs au serveur sans les ajouter au domaine Windows Server Essentials. Cette méthode n'est pas disponible pour tous les systèmes d'exploitation clients pris en charge. Les fonctionnalités, telles que la stratégie de groupe et les réseaux privés virtuels (VPN), qui nécessitent qu'un ordinateur soit connecté au domaine, ne sont pas disponibles. Pour plus d’informations sur les exigences et obtenir des instructions, consultez [Connect computers to a Windows Server Essentials server without joining the domain](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  


##### <a name="to-connect-a-client-computer-to-the-server"></a>Pour connecter un ordinateur client au serveur  

1.  Connectez-vous à l’ordinateur que vous voulez connecter au serveur.  

    > [!NOTE]
    >  Si cet ordinateur comporte plusieurs comptes d'utilisateur, connectez-vous à l'aide du compte d'utilisateur dont vous souhaitez conserver les documents, images et préférences personnelles, une fois l'ordinateur connecté au serveur.  

2.  Ouvrez un navigateur Internet, par exemple Internet Explorer.  

3.  Dans la barre d’adresses, tapez **http://<servername\>/se connecter**, puis appuyez sur ENTRÉE.  

    > [!NOTE]
    >  Si votre ordinateur est dans un emplacement distant en dehors du réseau de Windows Server Essentials, pour exécuter la connexion à un ordinateur à l’Assistant de serveur, tapez **http://<domainname\>/ se connecter** dans la barre d’adresses de votre navigateur web (() où < domaine\> est le nom de domaine de votre organisation). Contactez votre administrateur réseau pour obtenir les informations relatives à votre nom de domaine.  

4.  La page **Connectez votre ordinateur au serveur** apparaît. Faites une des actions suivantes :  

    -   Pour un ordinateur exécutant le système d'exploitation Windows, cliquez sur **Télécharger le logiciel pour Windows**.  

    -   Pour un ordinateur qui exécute Mac OS X ou ultérieur, cliquez sur **Télécharger les logiciels pour Mac**.  

5.  Dans le message d'avertissement de sécurité relatif au téléchargement du fichier, cliquez sur **Exécuter**.  

6.  Si le message Contrôle de compte d’utilisateur apparaît, cliquez sur **Oui** ou tapez le nom d’utilisateur local et le mot de passe, si vous y êtes invité.  

7.  L’Assistant Connecter un ordinateur au serveur s’affiche. Procédez comme suit pour terminer l'Assistant :  

    1.  Acceptez le Contrat de Licence Utilisateur Final.  

    2.  Dans la page **Rechercher mon serveur** , détectez automatiquement le serveur dans les réseaux locaux et sélectionnez celui auquel vous souhaitez vous connecter. Ou, si vous disposez des informations nécessaires, vous pouvez entrer manuellement l'adresse du domaine ou le nom du serveur.  

    3.  À la page **Entrez vos nouveaux nom d'utilisateur et mot de passe réseau**, procédez comme suit :  

        -   S’il s’agit du premier ordinateur que vous connectez au serveur et s’il s’agit de celui que vous utiliserez pour administrer le serveur, utilisez le compte d’administrateur créé au cours de l’installation.  

        -   Pour tous les autres ordinateurs, créez d’abord un compte d’utilisateur réseau sur le serveur à l’aide du tableau de bord. Créez le compte d’utilisateur avec des privilèges d’utilisateur standard ou d’administrateur, en fonction des tâches qui sont effectuées par la personne utilisant l’ordinateur.  

    4.  Si votre ordinateur exécute Windows 8, Windows 8.1 ou Windows 10, ignorez cette étape. Si votre ordinateur exécute Windows 7 et que vous possédez des documents, images ou préférences personnelles (environnements de Bureau, économiseurs d’écran ou favoris Internet Explorer) que vous souhaitez conserver après vous être connecté au nouveau réseau, sélectionnez l’option **Déplacer mes données et paramètres vers mon nouveau compte d’utilisateur réseau** de la page **Choisissez si vous voulez déplacer vos données et paramètres existants**de l’Assistant.  

    5.  Dans la page **Choisissez si vous voulez mettre en éveil cet ordinateur pour en effectuer une sauvegarde**, choisissez si vous voulez mettre en éveil l’ordinateur de façon automatique afin de créer une sauvegarde.  

    6.  Suivez les instructions suivantes dans l'Assistant pour connecter votre ordinateur au réseau.  

8.  Une fois votre ordinateur connecté au réseau, utilisez votre nouveau nom d’utilisateur et votre mot de passe pour vous connecter à l’ordinateur.  

    > [!NOTE]
    >  Lorsque vous vous connectez à un ordinateur qui exécute Windows 8 pour la première fois à l’aide de votre compte réseau, une fois la connexion au serveur effectuée, des instructions relatives à la migration de fichiers et d’applications à partir de l’ancien compte d’utilisateur s’affichent. Suivez les instructions à la page **Comment effectuer une migration des fichiers et des applications depuis mon ancien compte utilisateur ?** pour migrer tous les fichiers et applications vers le compte utilisateur réseau.  

9. Une fois que l’ordinateur est connecté au serveur, les raccourcis vers les TrayApp de connecteur et le tableau de bord du serveur s’affichent dans le menu Démarrer, qui peut être utilisé comme suit (si votre ordinateur exécute Windows 8, Windows 8.1 ou Windows 10, le tableau de bord et le connecteur TrayApp sera disponible à partir de l’écran d’accueil de l’ordinateur) :  

    -   Si votre ordinateur exécute Windows 8, Windows 8.1 ou Windows 10, le tableau de bord et le connecteur TrayApp sera sous forme d’application.  

    -   À partir de l'application de la barre d'état Connecteur, vous pouvez activer ou désactiver la fonctionnalité **Me maintenir connecté à distance**. Vous pouvez également double-cliquer sur l'application de la barre d'état pour démarrer Launchpad. Launchpad vous permet d'accéder au raccourci Dossiers partagés, de configurer les sauvegardes de l'ordinateur, de traiter les alertes et d'ouvrir le site de l'accès web à distance.  

    -   À l'aide du lien **Tableau de bord**, vous pouvez administrer votre serveur.  

###  <a name="BKMK_10"></a> Connecter des ordinateurs à un serveur Windows Server Essentials sans joindre le domaine  
 Cette rubrique décrit comment ajouter un ordinateur Windows 7, Windows 8, Windows 8.1 ou Windows 10 à un réseau Windows Server Essentials sans joindre l’ordinateur au domaine Windows Server Essentials dans un déploiement de client local. Cette méthode de connexion est prise en charge dans Windows Server Essentials et Windows Server Essentials.  

 Il s'agit d'une solution de remplacement de la méthode habituelle, qui demande de joindre l'ordinateur au domaine Windows Server Essentials. Avec cette méthode, si l'ordinateur se trouve dans un autre domaine, il doit être supprimé de ce dernier pour pouvoir être ajouté au domaine Windows Server Essentials.  

#### <a name="feature-limits"></a>Limitations des fonctionnalités  
 Certaines fonctionnalités sont limitées quand un ordinateur client n'est pas ajouté au domaine Windows Server Essentials :  

-   Toutes les fonctionnalités qui nécessitent que l’ordinateur soit joint au domaine ? y compris les informations d’identification de domaine, la stratégie de groupe et réseaux privés virtuels (VPN) ? ne sont pas disponibles.  

-   Les modules complémentaires tiers qui nécessitent que l'ordinateur soit joint au domaine ne fonctionneront pas correctement.  

-   Cette méthode ne peut pas être utilisée pour connecter un ordinateur externe au serveur.  

#### <a name="prerequisites"></a>Prérequis  

-   L'ordinateur doit disposer d'une connexion physique au réseau local.  

-   L'un des systèmes d'exploitation suivants doit être installé sur l'ordinateur :  

    -   Windows 10 Professionnel, Windows 10 Entreprise  

    -    Windows 8.1 Professionnel, entreprise de Windows 8.1, Windows 8 Professionnel, Windows 8 entreprise  

    -    Windows 7 Professionnel (x86 et x64), Windows 7 Enterprise (x86 et x64), Windows 7 Ultimate (x86 et x64)  


-   L'ordinateur doit répondre à toutes les autres conditions requises pour les ordinateurs clients dans Windows Server Essentials. Pour plus d'informations, voir [Conditions préalables pour connecter un ordinateur au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_2).  


-   Pour activer une connexion sans joindre le domaine, vous devez vous connecter à l'ordinateur avec un compte membre du groupe Administrateurs local.  

-   Pour connecter l'ordinateur au serveur Windows Server Essentials, vous devez disposer des informations suivantes sur le compte :  

    -   Compte avec des droits d'administrateur sur Windows Server Essentials (votre compte).  

    -   Nom d'utilisateur et mot de passe du compte de domaine de la personne qui utilise l'ordinateur. Le compte de domaine doit également disposer de droits d'administrateur sur le serveur Windows Server Essentials.  

#### <a name="connect-to-a-windows-server-essentials-network"></a>Se connecter à un réseau Windows Server Essentials  
 Après avoir vérifié que toutes les conditions préalables ont été remplies, connectez l'ordinateur au réseau Windows Server Essentials.  

###### <a name="to-connect-a-computer-in-a-different-domain-to-a-windows-server-essentials-network"></a>Pour connecter un ordinateur situé dans un domaine distinct à un réseau Windows Server Essentials  

1.  Connectez-vous à l'ordinateur client avec un compte membre du groupe Administrateurs local.  

2.  Ouvrez une invite de commandes avec des droits d'administrateur.  

    -   Dans Windows 10, cliquez sur le **Démarrer** bouton, sélectionnez **toutes les applications** -> **outils du système Windows** -> **invite de commandes**, cliquez sur invite de commandes, puis cliquez sur **exécuter en tant qu’administrateur**.  

    -   Dans Windows 8, sur le **Démarrer** , tapez **commande** puis appuyez sur ENTRÉE. Dans les résultats, cliquez sur **Invite de commandes**, puis sur **Exécuter en tant qu'administrateur**.  

    -   Dans Windows 7, sur le **Démarrer** menu, entrez **commande** dans la zone de recherche, cliquez sur **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.  

3.  À l'invite de commandes, tapez la commande suivante et appuyez sur Entrée :  

    ```  
    reg add "HKLM\SOFTWARE\Microsoft\Windows Server\ClientDeployment" /v SkipDomainJoin /t REG_DWORD /d 1  
    ```  


4.  Terminez les étapes dans [Connect computers to the server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  


####  <a name="BKMK_SecondServer"></a> Joindre un second serveur au réseau  

###### <a name="to-join-a-second-server-to-the-network"></a>Pour joindre un second serveur au réseau  

1.  Connectez-vous au serveur que vous souhaitez connecter au réseau Windows Server Essentials.  

2.  Ouvrez un navigateur Internet et dans la barre d’adresses, tapez **http://<servername\>/se connecter**, où *< nom_serveur\>*  est le nom du serveur exécutant Windows Server Essentials et appuyez sur ENTRÉE.  

3.  Si la Configuration de sécurité renforcée d'Internet Explorer est activée sur le serveur que vous essayez de connecter au réseau Windows Server Essentials, procédez comme suit. Sinon, ignorez cette étape.  

    1.  Pour accepter le message de blocage, cliquez sur **Fermer**.  

    2.  Ajouter le **http://<servername\>/se connecter** site Web pour les sites Web de confiance comme suit :  

        1.  Dans le volet de navigation du navigateur, cliquez sur **Outils**, puis sur **Options Internet**.  

        2.  Cliquez sur l'onglet **Sécurité** , puis sur **Sites de confiance**.  

        3.  Cliquez sur **Sites**.  

        4.  Le site web doit être indiqué dans le champ **Ajouter ce site Web à la zone**. Cliquez sur **Ajouter**.  

        5.  Cliquez sur **Fermer**, puis sur **OK**.  

    3.  Actualisez la page web.  


    4.  Pour connecter le second serveur à un serveur qui exécute Windows Server Essentials, suivez les instructions décrites dans [Connecter des ordinateurs au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  


~~~
> [!NOTE]
>  Connecting a second server to a server running Windows Server Essentials differs from connecting to a client computer as follows:  
>   
>  -   There is no user profile migration; therefore, the current profile will not be migrated.  
> -   You cannot back up the second server by using client computer backup, and there is no option to wake up the computer for backup.  
~~~

 Une fois que vous avez joint le second serveur à un serveur qui exécute Windows Server Essentials, les fonctionnalités suivantes sont accessibles au serveur connecté :  

- Les fonctionnalités d'état des mises à jour et des alertes s'exécutent de la même façon que sur les autres ordinateurs clients.  

- Les fonctionnalités en ligne et hors connexion s'exécutent de la même façon que sur les autres ordinateurs clients.  

- Vous pouvez vous connecter à votre second serveur à l'aide de l'accès web à distance.  

- Le second serveur est inclus dans les rapports d'intégrité, car Windows Server Essentials génère les alertes liées à ce serveur.  

  La gestion du second serveur à partir du serveur qui exécute Windows Server Essentials se distingue de la gestion des autres ordinateurs clients, comme suit :  

- Il n'y a aucun point d'entrée pour l'application de la barre d'état, Launchpad et le client de tableau de bord.  

- Le second serveur est répertorié dans le groupe **Serveurs** sous l'onglet **Périphériques**.  

- Comme la sauvegarde de l'ordinateur client n'est pas prise en charge pour le second serveur, l'état de la sauvegarde indique **Non pris en charge**. En outre, si vous sélectionnez le second serveur et si vous cliquez avec le bouton droit, aucune tâche associée de sauvegarde et restauration ne s'affiche pour le second serveur.  

- Si vous sélectionnez le second serveur, et si vous cliquez ensuite sur la tâche **Afficher les propriétés du serveur**, aucun onglet **Sauvegarde** ne s'affiche dans la page de propriétés du serveur.  

- Comme il n'existe pas de Centre de sécurité sur un système d'exploitation Windows Server, l'état de la sécurité du second serveur indique **Non applicable**.  

- L'état de la stratégie de groupe du second serveur indique **Non applicable**.  

###  <a name="BKMK_11"></a> Installer le logiciel Connecteur  
 Le logiciel Connecteur dans Windows Server Essentials est installé quand vous connectez votre ordinateur au serveur à l'aide de l'Assistant Connexion d'un ordinateur au serveur. Vous pouvez lancer cet Assistant en tapant **http://<ServerName\>/ se connecter** dans la barre d’adresses de votre navigateur web (où *< nom_serveur\>*  est le nom de votre serveur).  

> [!NOTE]
>  Si votre ordinateur est dans un emplacement distant, pour exécuter la connexion à un ordinateur à l’Assistant de serveur, tapez **http://<domainname\>/ se connecter** dans la barre d’adresses de votre navigateur web (où *< domaine\>*  est le nom de domaine de votre organisation). Contactez votre administrateur réseau pour obtenir les informations relatives à votre nom de domaine.  

 Le logiciel Connecteur effectue les opérations suivantes :  

-   il connecte votre ordinateur à Windows Server Essentials ;  

-   il sauvegarde automatiquement votre ordinateur chaque nuit (si vous configurez le serveur pour créer des sauvegardes du client) ;  

-   il aide l'administrateur à surveiller l'intégrité de votre ordinateur ;  

-   il vous permet de configurer et d'administrer à distance Windows Server Essentials à partir de votre ordinateur à usage privé.  


 Pour obtenir des instructions pas à pas sur la connexion de votre ordinateur au serveur Windows Server Essentials, consultez [Connect computers to the server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).   


###  <a name="BKMK_12"></a> Déplacer des données de l’ordinateur et les paramètres manuellement  
  Windows Server Essentials et Windows Server Essentials prend en charge de migration de profil utilisateur uniquement pour les ordinateurs clients qui exécutent le système d’exploitation Windows 7. Quand vous connectez un ordinateur Windows 7 au serveur, l'Assistant Connexion de l'ordinateur au serveur peut faire migrer automatiquement le profil utilisateur.  

 Le profil utilisateur ne peut pas être transféré automatiquement lors de la connexion d’un Windows 8, Windows 8.1 ou Windows 10 sur le serveur. Toutefois, sur un ordinateur Windows 8, vous pouvez utiliser l'outil Transfert de fichiers et paramètres Windows pour transférer les données et paramètres de l'utilisateur local d'origine vers l'ordinateur joint à un domaine. Pour ce faire, vous devez être un administrateur sur l'ordinateur Windows 8 source et sur l'ordinateur Windows 8 de destination. Pour plus d’informations sur l’utilisation de l’outil Transfert de fichiers et paramètres Windows, consultez l’[article 2735227](https://support.microsoft.com/kb/2735227) de la Base de connaissances Microsoft.  

###  <a name="BKMK_Transfer"></a> Transférer plusieurs profils utilisateur pendant le déploiement de l’ordinateur  
 Avant de connecter un ordinateur exécutant le système d'exploitation Windows 7 ou Windows 7 SP1 au serveur Windows Server Essentials, pour transférer plusieurs profils utilisateur locaux, vous devez d'abord créer les comptes d'utilisateur réseau correspondants sur le serveur. Pour plus d’informations sur la création des comptes d’utilisateur réseau, consultez [Add a user account](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1).  

 Migration du profil utilisateur est uniquement pris en charge sur un ordinateur exécutant Windows 7 (Windows Server Essentials) ou Windows 7 SP1 (pour Windows Server Essentials). Quand vous connectez un ordinateur au serveur Windows Server Essentials à l'aide de l'Assistant Connexion de l'ordinateur au serveur, vous disposez d'une option qui permet de déplacer les données et paramètres utilisateur des anciens comptes d'utilisateur locaux vers les nouveaux comptes d'utilisateur réseau. Pour ce faire, dans la page **Déplacer les données et paramètres utilisateur existants** de l'Assistant, mappez les comptes d'utilisateur réseau aux comptes d'utilisateur locaux présents sur l'ordinateur pour transférer plusieurs profils utilisateur situés sur l'ordinateur client.  

###  <a name="BKMK_13"></a> Désinstaller le logiciel Connecteur  
 Vous pouvez désinstaller le logiciel Connecteur d'un ordinateur à l'aide du Panneau de configuration. En règle générale, vous procédez ainsi s'il existe un problème avec le logiciel Connecteur ou si vous devez installer une version plus récente du logiciel Connecteur. Vous devez être connecté à l'ordinateur en tant qu'administrateur pour effectuer cette procédure.  

> [!IMPORTANT]
>  Si vous mettez à niveau le système d'exploitation sur un ordinateur client, le logiciel Connecteur est automatiquement désinstallé. Vous devez réinstaller le logiciel Connecteur une fois la mise à niveau effectuée. La méthode recommandée consiste à désinstaller le logiciel Connecteur avant d'effectuer la mise à niveau du système d'exploitation. La désinstallation du logiciel Connecteur, une fois la mise à niveau effectuée, est toujours possible. Toutefois, cela peut entraîner un état incohérent de l'ordinateur client par rapport au serveur tant que le logiciel Connecteur n'est pas désinstallé, puis réinstallé.  

##### <a name="to-uninstall-connector-software-from-a-computer"></a>Pour désinstaller le logiciel Connecteur à partir d'un ordinateur  

1.  À partir d’un ordinateur qui exécute Windows 7, Windows 8, Windows 8.1 ou Windows 10, ouvrez **le panneau de configuration**, puis, dans le **programmes** , cliquez sur **afficher les mises à jour installées**.  

2.  Dans la liste des programmes installés, sélectionnez **Connecteur Windows Server Essentials**, puis cliquez sur **Désinstaller**.  

3.  Dans la page d'avertissement, cliquez sur **Oui**.  

4.  Si la fenêtre **Contrôle de compte d'utilisateur** s'affiche, cliquez sur **Autoriser**.  

5.  Dans Windows Server Essentials, si la page du connecteur Windows Server Essentials suggère de fermer Launchpad, cliquez sur **OK**.  

6.  Attendez que le programme soit désinstallé. Une fois le logiciel supprimé, **Connecteur Windows Server Essentials** n'apparaît plus dans la liste des mises à jour ou programmes installés. En outre, les raccourcis vers Launchpad et le tableau de bord ne sont plus affichés sur le Bureau de l'ordinateur.  

> [!NOTE]
> - La désinstallation du logiciel Connecteur ne supprime pas l'ordinateur de la liste des ordinateurs affichés sous l'onglet **PÉRIPHÉRIQUES** du tableau de bord. Pour supprimer l'ordinateur du tableau de bord, consultez [Supprimer un ordinateur du serveur](../manage/Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3).  
>   -   Quand vous désinstallez le logiciel Connecteur, les dossiers partagés de l'ordinateur client qui ont été mappés au serveur ne sont pas supprimés. Vous devez supprimer manuellement les dossiers partagés mappés au serveur.  
> 
> -   La désinstallation du logiciel Connecteur n'entraîne pas l'arrêt de la jonction de l'ordinateur au domaine d'origine. Vous devez supprimer manuellement la jonction de l'ordinateur au domaine. Pour obtenir des instructions, consultez [Supprimer un ordinateur d'un domaine Windows](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8).  


###  <a name="BKMK_14"></a> Déconnecter votre ordinateur à partir d’ou vous reconnecter à votre ordinateur au serveur  
 Pour déconnecter un ordinateur du serveur, procédez comme suit :  


1. Désinstaller le logiciel Connecteur de l'ordinateur à l'aide du Panneau de configuration. Pour obtenir des instructions pas à pas, consultez [Uninstall the Connector software](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13).   


2. Détachez l'ordinateur du domaine Windows Server Essentials et le joindre au groupe de travail. Pour obtenir des instructions pas à pas sur la jonction de Windows à un groupe de travail, consultez [Rejoindre ou créer un groupe de travail](https://windows.microsoft.com/windows7/Join-or-create-a-workgroup).  

3. Supprimez l'ordinateur du serveur à l'aide du tableau de bord. Pour obtenir des instructions pas à pas, consultez [Remove a computer from the server](../manage/Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3).  

   Pour reconnecter un ordinateur au serveur qui a été précédemment déconnecté de votre réseau serveur Windows Server Essentials, procédez comme suit :  


4. Désinstaller le logiciel Connecteur de l'ordinateur à l'aide du Panneau de configuration. Pour obtenir des instructions pas à pas, consultez [Uninstall the Connector software](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13).  

5. Détachez l'ordinateur du domaine Windows Server Essentials et le joindre au groupe de travail. Pour obtenir des instructions pas à pas sur la jonction de Windows à un groupe de travail, consultez [Rejoindre ou créer un groupe de travail](https://windows.microsoft.com/windows7/Join-or-create-a-workgroup).  

6. Connectez l'ordinateur au serveur à l'aide de l'Assistant Connect Computer. Pour obtenir des instructions pas à pas, consultez [Connect computers to the server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

###  <a name="BKMK_Sleep"></a> Comment fonctionne la sauvegarde avec la mise en veille et modes de mise en veille prolongée  
 Si vous sélectionnez l'option **Sortir cet ordinateur du mode veille pour la sauvegarde** quand vous connectez un ordinateur au serveur, l'ordinateur sort automatiquement du mode veille ou veille prolongée, chaque jour, comme indiqué dans la planification de sauvegarde pour pouvoir être sauvegardé. À la fin de la sauvegarde, l'ordinateur retourne en mode veille ou veille prolongée, selon ses paramètres de gestion de l'alimentation. Si vous ne sélectionnez pas cette option, le serveur n'effectue aucune sauvegarde quand l'ordinateur est en veille ou veille prolongée. Pour plus d’informations, consultez [gérer la sauvegarde Client](../manage/Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md).  

##  <a name="BKMK_C"></a> Utiliser Launchpad  
 Vous pouvez utiliser Launchpad pour accéder aux ressources partagées à partir du serveur Windows Server Essentials, effectuer des sauvegardes d'ordinateur et répondre aux alertes d'intégrité du système.  

-   [Vue d’ensemble de LaunchPad](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md)  

-   [Utiliser Launchpad avec un ordinateur Mac](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md#BKMK_Mac)  

## <a name="see-also"></a>Voir aussi  

-   [Résoudre les problèmes de connexion des ordinateurs au serveur](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md)  

-   [Gérer des comptes d’utilisateurs](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md)  


-   [Utiliser des dossiers partagés](Use-Shared-Folders-in-Windows-Server-Essentials.md)  

-   [Travailler à distance](Work-Remotely-in-Windows-Server-Essentials.md)  

-   [Lire des médias numériques](Play-Digital-Media-in-Windows-Server-Essentials.md)

-   [Utiliser des dossiers partagés](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)  

-   [Travailler à distance](../use/Work-Remotely-in-Windows-Server-Essentials.md)  

-   [Lire des médias numériques](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)

