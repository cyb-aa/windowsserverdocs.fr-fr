---
title: Get Connected in Windows Server Essentials
description: "Décrit comment utiliser WindowsServerEssentials"
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
ms.openlocfilehash: 1d7f3c33f1254c8dbe4af8bdf5baa4144c134248
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="get-connected-in-windows-server-essentials"></a>Get Connected in Windows Server Essentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials
  
 Vous pouvez connecter vos ordinateurs au serveur Windows Server Essentials à l’aide du logiciel connecteur. Le logiciel connecteur est installé lorsque vous vous connectez un ordinateur au serveur à l’aide de la connexion d’un ordinateur à l’Assistant de serveur. Vous pouvez démarrer cet Assistant en tapant **http://<servername\>/connect**, où **< servername\ >** est le nom de votre serveur.  
  
 Dans cette rubrique:  
  

-   [Préparer la connexion des ordinateurs au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_A)  
  
-   [Connecter des ordinateurs au serveur en utilisant le logiciel Connecteur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_B)  
  
-   [Utiliser Launchpad](Get-Connected-in-Windows-Server-Essentials.md#BKMK_C)  

-   [Préparer la connexion des ordinateurs au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_A)  
  
-   [Connecter des ordinateurs au serveur en utilisant le logiciel Connecteur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_B)  
  
-   [Utiliser Launchpad](Get-Connected-in-Windows-Server-Essentials.md#BKMK_C)  

  
##  <a name="BKMK_A"></a>Préparer la connexion des ordinateurs au serveur  
 Cette section décrit le logiciel Connecteur, les systèmes d’exploitation pris en charge par Windows Server Essentials, les tâches préalables qui doivent être effectuées avant de connecter vos ordinateurs au serveur et les modifications apportées par le serveur sur les ordinateurs lorsque vous exécutez le logiciel connecteur.  
  

-   [Vue d’ensemble du logiciel de connecteur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Configuration requise pour la connexion d’un ordinateur au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Conditions préalables pour connecter un ordinateur Mac au réseau](Get-Connected-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Systèmes d’exploitation pris en charge pour les ordinateurs clients](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Modifications apportées par le serveur à un ordinateur client](Get-Connected-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Informations de nom et mot de passe utilisateur réseau](Get-Connected-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Compte d’administrateur s Server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Supprimer un ordinateur d’un domaine Windows](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)  
  
###  <a name="BKMK_1"></a>Vue d’ensemble du logiciel de connecteur  
 Le logiciel Connecteur pour le système d’exploitation Windows Server Essentials connecte les ordinateurs de votre réseau vers le serveur Windows Server Essentials. Lorsque vous vous connectez des ordinateurs au serveur, le logiciel Connecteur vous permet de sauvegarder les ordinateurs et de surveiller leur intégrité automatiquement. Le logiciel Connecteur vous permet également de configurer et administrer à distance le serveur Windows Server Essentials. Le logiciel connecteur est installé lorsque vous vous connectez un ordinateur client au serveur. Pour obtenir des instructions détaillées sur la connexion des ordinateurs clients vers le serveur Windows Server Essentials, consultez [connecter des ordinateurs au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9) plus loin dans cette rubrique.  

-   [Vue d’ensemble du logiciel de connecteur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Configuration requise pour la connexion d’un ordinateur au serveur](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Conditions préalables pour connecter un ordinateur Mac au réseau](Get-Connected-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [Systèmes d’exploitation pris en charge pour les ordinateurs clients](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4)  
  
-   [Modifications apportées par le serveur à un ordinateur client](Get-Connected-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [Informations de nom et mot de passe utilisateur réseau](Get-Connected-in-Windows-Server-Essentials.md#BKMK_6)  
  
-   [Compte d’administrateur s Server](Get-Connected-in-Windows-Server-Essentials.md#BKMK_7)  
  
-   [Supprimer un ordinateur d’un domaine Windows](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8)  
  
###  <a name="BKMK_1"></a>Vue d’ensemble du logiciel de connecteur  
 Le logiciel Connecteur pour le système d’exploitation Windows Server Essentials connecte les ordinateurs de votre réseau vers le serveur Windows Server Essentials. Lorsque vous vous connectez des ordinateurs au serveur, le logiciel Connecteur vous permet de sauvegarder les ordinateurs et de surveiller leur intégrité automatiquement. Le logiciel Connecteur vous permet également de configurer et administrer à distance le serveur Windows Server Essentials. Le logiciel connecteur est installé lorsque vous vous connectez un ordinateur client au serveur. Pour obtenir des instructions détaillées sur la connexion des ordinateurs clients vers le serveur Windows Server Essentials, consultez [connecter des ordinateurs au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9) plus loin dans cette rubrique.  

  
###  <a name="BKMK_2"></a>Configuration requise pour la connexion d’un ordinateur au serveur  
 Les conditions suivantes doivent être remplies avant de vous connecter à un ordinateur au réseau:  
  
-   L’installation de Windows Server Essentials est terminée et que le serveur est en cours d’exécution. Le logiciel Connecteur quitte son installation si elle ne peut pas communiquer avec le serveur.  
  

-   L’ordinateur client exécute un système d’exploitation pris en charge. Pour plus d’informations, voir [pris en charge des systèmes d’exploitation pour les ordinateurs clients](Get-Connected-in-Windows-Server-Essentials.md#BKMK_4).

  
-   L’ordinateur client doit disposer d’une connexion valide à Internet.  
  
-   L’ordinateur client est sur le même sous-réseau IP que le serveur qui exécute Windows Server Essentials quand l’ordinateur client est sur le même réseau que le serveur.  
  
-   L’ordinateur client a .NET Framework 4.5 est installé sur ce dernier. Le logiciel Connecteur l’installe automatiquement sur l’ordinateur.  
  
-   L’ordinateur client répond à la configuration minimale requise suivante:  
  
    -   Processeur 1,4 GHz ou plus rapide  
  
    -   1 Go de RAM ou plus  
  
    -   1 Go d’espace disque disponible (une partie de cet espace est libérée après l’installation)  
  
-   La partition de démarrage (autrement dit, la partition de disque où est installé le système d’exploitation Windows) est formatée avec le système de fichiers NTFS.  
  
-   Le nom de l’ordinateur n’inclut pas plus de 15 caractères.  
  
-   Le nom de l’ordinateur n’inclut pas d’un trait de soulignement (_).  
  
-   Les paramètres de date et heure ordinateur s’alignent sur les paramètres sur le serveur.  
  
-   Un ordinateur client peut être connecté à un seul serveur Windows Server Essentials à un moment donné.  
  
-   Un ordinateur client qui est déjà joint à un autre domaine Active Directory ne peut pas joindre un domaine Windows Server Essentials.  
  
> [!NOTE]

>  Dans un déploiement de client local pour Windows Server Essentials ou Windows Server Essentials, vous pouvez connecter des ordinateurs au serveur sans les ajouter au domaine Windows Server Essentials. Cette méthode n’est pas disponible pour tous les systèmes d’exploitation clients pris en charge et des fonctionnalités, telles que la stratégie de groupe et les réseaux privés virtuels (VPN), qui nécessitent qu’un ordinateur est connecté au domaine, ne sont pas disponibles. Pour la configuration requise et des instructions, voir [connecter des ordinateurs à un serveur Windows Server Essentials sans joindre le domaine](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  
  
 Pour obtenir des instructions pas à pas connecter un ordinateur au serveur exécutant Windows Server Essentials, consultez [connecter des ordinateurs au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

>  Dans un déploiement de client local pour Windows Server Essentials ou Windows Server Essentials, vous pouvez connecter des ordinateurs au serveur sans les ajouter au domaine Windows Server Essentials. Cette méthode n’est pas disponible pour tous les systèmes d’exploitation clients pris en charge et des fonctionnalités, telles que la stratégie de groupe et les réseaux privés virtuels (VPN), qui nécessitent qu’un ordinateur est connecté au domaine, ne sont pas disponibles. Pour la configuration requise et des instructions, voir [connecter des ordinateurs à un serveur Windows Server Essentials sans joindre le domaine](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  
  
 Pour obtenir des instructions pas à pas connecter un ordinateur au serveur exécutant Windows Server Essentials, consultez [connecter des ordinateurs au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

  
###  <a name="BKMK_3"></a>Conditions préalables pour connecter un ordinateur Mac au réseau  
 Les conditions suivantes doivent être remplies avant de vous connecter à un ordinateur Mac au réseau:  
  
-   L’installation du système d’exploitation serveur est terminée et que le serveur est en cours d’exécution. Le logiciel Connecteur n’installera pas s’il ne peut pas communiquer avec le serveur.  
  
-   L’ordinateur exécute Mac OS X 10.5 (Leopard) ou version ultérieure.  
  
-   L’ordinateur est sur le même sous-réseau IP que le serveur.  
  
-   L’ordinateur doit disposer d’une connexion valide à Internet.  
  
-   Assurez-vous que l’ordinateur répond à la configuration minimale requise suivante:  
  
    -   Processeur 1,4 GHz ou plus rapide  
  
    -   1 Go de RAM ou plus  
  
    -   1 Go d’espace disque disponible (une partie de cet espace est libérée après l’installation)  
  
-   Un ordinateur client peut être connecté à un seul serveur à un moment donné.  
  
###  <a name="BKMK_4"></a>Systèmes d’exploitation pris en charge pour les ordinateurs clients  
 Windows Server Essentials fournit le même ensemble de fonctionnalités pour tous les ordinateurs clients pris en charge. Ces fonctionnalités incluent la jonction de domaine, Launchpad et les notifications de contrôle d’intégrité côté client.  
  
> [!IMPORTANT]
>  Windows Server Essentials ne prend pas en charge jonction des ordinateurs exécutant les versions d’accueil, Starter ou Media Center de Windows au domaine. En outre, vous ne pouvez pas utiliser l’accès Web à distance pour se connecter à ces ordinateurs.  
  
#### <a name="windows-server-essentials"></a>Windows Server Essentials  
  Cette section s’applique à un serveur exécutant Windows Server Essentials, ou à un serveur exécutant Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter avec le rôle expérience Windows Server Essentials installé. Les systèmes d’exploitation suivants sont pris en charge:  
  
 **Systèmes d’exploitation Windows 7**  
  
-    SP1 base accueil de Windows 7 (x 86 et x64)  
  
-    Windows 7 Édition familiale Premium SP1 (x 86 et x64)  
  
-    Windows 7 Professionnel SP1 (x 86 et x64)  
  
-    SP1 Édition intégrale de Windows 7 (x 86 et x64)  
  
-    Windows 7 Entreprise SP1 (x 86 et x64)  
  
-    Windows 7 Édition Starter SP1 (x 86)  
  
 **Systèmes d’exploitation Windows 8**  
  
-   Windows8  
  
-   Windows 8 Professionnel  
  
-   Windows8 entreprise  
  
 **Systèmes d’exploitation Windows 8.1**  
  
-   Windows8.1  
  
-   Windows 8.1 Professionnel  
  
-   Windows8.1 entreprise  
  
 **Systèmes d’exploitation Windows 10**  
  
-   Windows10  
  
-   Windows 10 Professionnel  
  
-   Windows10 entreprise  
  
-   Windows10 éducation  
  
 **Ordinateurs client Mac**  
  
-   Mac OS X v10.5 Leopard  
  
-   Mac OS X v10.6 neige Leopard  
  
-   Mac OS X versions 10.7 Lion  
  
-   Mac OS X v10.8 montagne Lion  
  
> [!NOTE]
>  Vous pouvez afficher l’intégrité et l’état de la sauvegarde d’un ordinateur Mac à partir du tableau de bord Windows Server Essentials. Toutefois, vous ne peut pas configurer la sauvegarde de l’ordinateur ou démarrer une sauvegarde à partir du tableau de bord. En outre, vous ne pouvez pas utiliser l’accès Web à distance pour se connecter à un ordinateur Mac.  
  
#### <a name="windows-server-essentials"></a>Windows Server Essentials  
  Cette section s’applique à un serveur exécutant WindowsServerEssentials. Les systèmes d’exploitation suivants sont pris en charge:  
  
 **Systèmes d’exploitation Windows 7**  
  
-    Windows 7 Édition Familiale Basique (x86 et x64)  
  
-    Windows 7 Édition familiale Premium (x86 et x64)  
  
-    Windows 7 Professionnel (x86 et x64)  
  
-    Windows 7 Édition intégrale (x86 et x64)  
  
-    Windows 7 entreprise (x86 et x64)  
  
-    Windows 7 Édition Starter (x86)  
  
 **Systèmes d’exploitation Windows 8**  
  
-   Windows8  
  
-   Windows 8 Professionnel  
  
-   Windows8 entreprise  
  
 **Systèmes d’exploitation Windows 10**  
  
-   Windows10  
  
-   Windows 10 Professionnel  
  
-   Windows10 entreprise  
  
-   Windows10 éducation  
  
 **Ordinateurs client Mac**  
  
-   Mac OS X v10.5 Leopard  
  
-   Mac OS X v10.6 neige Leopard  
  
-   Mac OS X versions 10.7 Lion  
  
> [!NOTE]
>  Vous pouvez afficher l’intégrité et l’état de la sauvegarde d’un ordinateur Mac à partir du tableau de bord Windows Server Essentials. Toutefois, vous ne peut pas configurer la sauvegarde de l’ordinateur ou démarrer une sauvegarde à partir du tableau de bord. En outre, vous ne pouvez pas utiliser l’accès Web à distance pour se connecter à un ordinateur Mac.  
  
###  <a name="BKMK_5"></a>Modifications apportées par le serveur à un ordinateur client  
 Lorsque vous vous connectez un ordinateur au serveur, le logiciel Windows Server Essentials apporte un certain nombre de modifications apportées à l’ordinateur afin de l’ordinateur et le serveur peuvent travailler ensemble.  
  
 Le logiciel effectue les opérations suivantes:  
  
-   Installe le logiciel Connecteur sur l’ordinateur  
  
-   Installe Microsoft .NET Framework 4.5 sur l’ordinateur s’il n’est pas déjà installé  
  
-   Crée des raccourcis sur le bureau s le Launchpad et le tableau de bord  
  
-   Configure les ports du pare-feu Windows sur l’ordinateur pour autoriser les fonctionnalités suivantes utiliser:  
  
    -   Réseau de base  
  
    -   Services Bureau à distance  
  
-   Apporte les modifications suivantes à l’ordinateur pour faciliter les sauvegardes:  
  
    -   Crée des tâches planifiées pour exécuter des sauvegardes automatiques  
  
    -   Installe les services qui gèrent les opérations de sauvegarde avec le serveur  
  
    -   Installe un pilote de périphérique virtuel qui est utilisé au cours de processus de restauration des fichiers et dossiers  
  
-   Installe l’Agent d’intégrité pour détecter les problèmes et pour créer les notifications d’alerte correspondantes  
  
-   Crée des tâches planifiées sur l’ordinateur pour les évaluations d’intégrité périodiques et de synchroniser les définitions d’alertes d’intégrité  
  
-   Ajoute des services à l’ordinateur, l’ordinateur utilise pour communiquer avec le serveur et avec d’autres fonctionnalités de Windows Server Essentials  
  
-   Ouvre le port TCP 3389 des ordinateurs clients qui exécutent des clients Windows pour autoriser l’exécution des Services Bureau à distance  
  
-   Déploie un VPN sur l’ordinateur client et fournit une expérience simple clic si la fonctionnalité VPN est activée sur Windows Server Essentials, ou fournit une expérience privé si la fonctionnalité VPN est activée sur Windows Server Essentials  
  

 Pour plus d’informations sur la connexion de votre ordinateur au serveur, consultez [connecter des ordinateurs au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  
  
###  <a name="BKMK_6"></a>Informations de nom et mot de passe utilisateur réseau  
 Vous pouvez obtenir vos informations nom et mot de passe d’utilisateur réseau à partir de la personne qui gère votre serveur. Vous pouvez utiliser ces informations d’identification pour connecter votre ordinateur au serveur et accéder aux informations à partir du serveur.  
  
###  <a name="BKMK_6"></a>Informations de nom et mot de passe utilisateur réseau  
 Vous pouvez obtenir vos informations nom et mot de passe d’utilisateur réseau à partir de la personne qui gère votre serveur. Vous pouvez utiliser ces informations d’identification pour connecter votre ordinateur au serveur et accéder aux informations à partir du serveur. 

  
 Si vous êtes l’administrateur du serveur, vous pouvez créer les informations d’identification réseau en ajoutant un compte d’utilisateur à partir de la **utilisateurs** onglet du tableau de bord. Pour plus d’informations sur les comptes d’utilisateur, voir [gérer les comptes d’utilisateur à l’aide du tableau de bord](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8).  
  
###  <a name="BKMK_7"></a>Compte d’administrateur s Server  
 Vous devez être en mesure de fournir un nom de compte d’administrateur réseau et le mot de passe pour installer le logiciel connecteur. Un compte d’administrateur réseau permet à l’utilisateur gérer le réseau local de votre organisation et vous aide à gérer et mettre à jour des périphériques réseau tels que les commutateurs et les routeurs.  
  
 Les tâches qui peuvent être effectuées à l’aide d’un compte d’administrateur réseau peuvent inclure:  
  
-   L’installation des applications en réseau et autres logiciels  
  
-   Gestion du stockage sur le serveur  
  
-   Distribution des mises à jour logicielles  
  
-   Effectuer des sauvegardes régulières  
  
-   Surveillance des activités quotidiennes sur le réseau  
  
 Dans Windows Server Essentials, Windows Server Essentials et Windows Server 2012 R2 avec le rôle expérience Windows Server Essentials installé, vous pouvez affecter l’administrateur réseau niveau à n’importe quel compte d’utilisateur d’accès. Il accorde les autorisations requises pour effectuer des tâches de l’administrateur réseau. Lorsqu’un utilisateur est affecté à l’administrateur réseau niveau d’accès du **le contrôle d’accès utilisateur** invite de commandes s’ouvre pour toute tâche qui nécessite des autorisations d’administrateur.  
  
###  <a name="BKMK_8"></a>Supprimer un ordinateur d’un domaine Windows  
 Pour supprimer un ordinateur de son domaine, vous serez invité pour le nom d’utilisateur et mot de passe du compte de domaine.  
  
##### <a name="to-remove-a-computer-from-a-windows-domain"></a>Pour supprimer un ordinateur à partir d’un domaine Windows  
  
1.  Cliquez sur **Démarrer**, avec le bouton droit **ordinateur**, puis cliquez sur **propriétés**.  
  
2.  Sous **paramètres de groupe de travail, le domaine et nom de l’ordinateur**, cliquez sur **modifier les paramètres**.  
  
    > [!NOTE]
    >  Si vous êtes invité à un mot de passe administrateur ou une confirmation, tapez le mot de passe de domaine ou la confirmation.  
  
3.  Dans le **propriétés système** boîte de dialogue, cliquez sur le **nom de l’ordinateur** onglet, puis cliquez sur **modification**.  
  
4.  Dans le **modification du nom ou du domaine d’ordinateur** la boîte de dialogue sous **membre**, cliquez sur **groupe de travail**, puis effectuez l’une des opérations suivantes:  
  
    1.  Pour rejoindre un groupe de travail existant, tapez le nom du groupe de travail que vous souhaitez joindre, puis cliquez sur **OK**.  
  
    2.  Pour créer un groupe de travail, tapez le nom du groupe de travail que vous souhaitez créer, puis cliquez sur **OK**.  
  
        > [!NOTE]
        >  Votre ordinateur est supprimé du domaine et votre compte d’ordinateur dans ce domaine sera désactivé.  
  
##  <a name="BKMK_B"></a>Connecter des ordinateurs au serveur en utilisant le logiciel Connecteur  
 Cette section fournit l’accès aux procédures et informations qui vous aideront à installer le logiciel Connecteur, connectez votre ordinateur au serveur et résoudre les problèmes de connexion des ordinateurs au serveur.  
  

-   [Connecter des ordinateurs au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Connecter des ordinateurs à un serveur Windows Server Essentials sans joindre le domaine](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Installer le logiciel Connecteur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Déplacer les données de l’ordinateur et les paramètres manuellement](Get-Connected-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [Transférer plusieurs profils utilisateur pendant le déploiement de l’ordinateur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Transfer)  
  
-   [Désinstaller le logiciel Connecteur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [Déconnecter votre ordinateur à partir ou reconnectez votre ordinateur au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [Comment fonctionne la sauvegarde avec mise en veille et modes de mise en veille prolongée](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Sleep)  

-   [Connecter des ordinateurs au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9)  
  
-   [Connecter des ordinateurs à un serveur Windows Server Essentials sans joindre le domaine](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10)  
  
-   [Installer le logiciel Connecteur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_11)  
  
-   [Déplacer les données de l’ordinateur et les paramètres manuellement](Get-Connected-in-Windows-Server-Essentials.md#BKMK_12)  
  
-   [Transférer plusieurs profils utilisateur pendant le déploiement de l’ordinateur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Transfer)  
  
-   [Désinstaller le logiciel Connecteur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13)  
  
-   [Déconnecter votre ordinateur à partir ou reconnectez votre ordinateur au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_14)  
  
-   [Comment fonctionne la sauvegarde avec mise en veille et modes de mise en veille prolongée](Get-Connected-in-Windows-Server-Essentials.md#BKMK_Sleep)  

  
###  <a name="BKMK_9"></a>Connecter des ordinateurs au serveur  
 Lorsque vous vous connectez un ordinateur à un serveur qui exécute Windows Server Essentials ou Windows Server 2012 R2 avec le rôle expérience Windows Server Essentials installé, assurez-vous que votre ordinateur client dispose d’une connexion valide à Internet.  
  
 Effectuez la procédure suivante sur tous les ordinateurs clients afin de les connecter à votre serveur.  
  
 Pour terminer la procédure, vous avez besoin des informations suivantes:  
  
-   Le nom d’utilisateur et mot de passe de la personne qui utilisera l’ordinateur sur le nouveau réseau  
  
-   Le nom d’utilisateur et mot de passe du compte d’administrateur local ordinateur s  
  
> [!NOTE]

>  Dans un déploiement de client local pour Windows Server Essentials ou Windows Server Essentials, vous pouvez connecter des ordinateurs au serveur sans les ajouter au domaine Windows Server Essentials. Cette méthode n’est pas disponible pour tous les systèmes d’exploitation clients pris en charge et des fonctionnalités, telles que la stratégie de groupe et les réseaux privés virtuels (VPN), qui nécessitent qu’un ordinateur est connecté au domaine, ne sont pas disponibles. Pour la configuration requise et des instructions, voir [connecter des ordinateurs à un serveur Windows Server Essentials sans joindre le domaine](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  

>  Dans un déploiement de client local pour Windows Server Essentials ou Windows Server Essentials, vous pouvez connecter des ordinateurs au serveur sans les ajouter au domaine Windows Server Essentials. Cette méthode n’est pas disponible pour tous les systèmes d’exploitation clients pris en charge et des fonctionnalités, telles que la stratégie de groupe et les réseaux privés virtuels (VPN), qui nécessitent qu’un ordinateur est connecté au domaine, ne sont pas disponibles. Pour la configuration requise et des instructions, voir [connecter des ordinateurs à un serveur Windows Server Essentials sans joindre le domaine](Get-Connected-in-Windows-Server-Essentials.md#BKMK_10).  

  
##### <a name="to-connect-a-client-computer-to-the-server"></a>Pour connecter un ordinateur client au serveur  
  
1.  Ouvrez une session sur l’ordinateur que vous souhaitez vous connecter au serveur.  
  
    > [!NOTE]
    >  Si cet ordinateur comporte plusieurs comptes d’utilisateur, ouvrez une session en utilisant le compte d’utilisateur qui a des documents, images et préférences personnelles que vous souhaitez conserver après que vous être connecté au serveur de l’ordinateur.  
  
2.  Ouvrez un navigateur Internet, telles qu’Internet Explorer.  
  
3.  Dans la barre d’adresses, tapez **http://<servername\>/Connect**, puis appuyez sur ENTRÉE.  
  
    > [!NOTE]
    >  Si votre ordinateur est dans un emplacement distant en dehors du réseau de Windows Server Essentials, pour exécuter la connexion à un ordinateur à l’Assistant serveur, tapez **http://<domainname\>/connect** dans la barre d’adresses de votre navigateur web (où < domain\ > est le nom de domaine de votre organisation). Vous pouvez obtenir des informations sur les nom de domaine auprès de votre administrateur réseau.  
  
4.  Le **connecter votre ordinateur au serveur** page s’affiche. Effectuez l’une des opérations suivantes:  
  
    -   Pour un ordinateur exécutant le système d’exploitation Windows, cliquez sur **télécharger le logiciel pour Windows**.  
  
    -   Pour un ordinateur exécutant Mac OS X ou version ultérieure, cliquez sur **télécharger le logiciel pour Mac**.  
  
5.  Dans le message d’avertissement du sécurité de téléchargement de fichier, cliquez sur **exécuter**.  
  
6.  Si le message contrôle de compte d’utilisateur apparaît, cliquez sur **Oui** ou tapez le nom d’utilisateur local et le mot de passe, si vous y êtes invité.  
  
7.  La connexion de qu'un ordinateur à l’Assistant serveur s’affiche. Procédez comme suit pour terminer l’Assistant:  
  
    1.  Acceptez le contrat de licence utilisateur final.  
  
    2.  Sur le **rechercher mon serveur** page, détecter automatiquement le serveur dans les réseaux locaux et sélectionnez le serveur que vous souhaitez vous connecter. Ou, si vous disposez des informations, vous pouvez entrer manuellement l’adresse de domaine ou le nom de votre serveur s.  
  
    3.  Sur le **tapez votre nouveau nom d’utilisateur réseau et le mot de passe** page, procédez comme suit:  
  
        -   S’il s’agit du premier ordinateur que vous vous connectez au serveur, et s’il s’agit de l’ordinateur que vous utiliserez pour administrer le serveur, utilisez le compte d’administrateur que vous avez créé lors de l’installation.  
  
        -   Pour tous les autres ordinateurs, tout d’abord créer un compte d’utilisateur réseau sur le serveur à l’aide du tableau de bord. Créer le compte d’utilisateur administrateur ou Standard des privilèges d’utilisateur, basées sur les tâches qui sont effectuées par la personne à l’aide de l’ordinateur.  
  
    4.  Si votre ordinateur exécute Windows 8, Windows 8.1 ou Windows 10, ignorez cette étape. Si votre ordinateur exécute Windows 7, et si vous avez des documents, images ou préférences personnelles (par exemple, des arrière-plans du bureau, économiseurs d’écran ou Favoris Internet Explorer) que vous souhaitez conserver après vous joindre l’ordinateur au nouveau réseau, sur le **choisir si vous voulez déplacer vos données existantes et les paramètres** page de l’Assistant, sélectionnez le **déplacer mes données et les paramètres vers mon nouveau compte d’utilisateur réseau**.  
  
    5.  Choisissez si vous souhaitez mettre en éveil automatiquement l’ordinateur pour créer une sauvegarde sur le **choisir si vous voulez mettre en éveil cet ordinateur pour créer sa sauvegarde** page.  
  
    6.  Suivez les instructions restantes de l’Assistant pour joindre l’ordinateur au réseau.  
  
8.  Une fois que vous joignez votre ordinateur au réseau, utilisez votre nouveau nom d’utilisateur et un mot de passe pour vous connecter à l’ordinateur.  
  
    > [!NOTE]
    >  Lorsque vous ouvrez une session sur un ordinateur qui exécute Windows 8 pour la première fois à l’aide de votre compte réseau, une fois la connexion au serveur, des instructions pour la migration de fichiers et applications à partir de l’ancien compte d’utilisateur s’affichent. Suivez les instructions à le **comment migrer les fichiers et applications à partir de mon ancien compte d’utilisateur? ** page pour migrer tous les fichiers et applications vers le compte d’utilisateur réseau.  
  
9. Une fois l’ordinateur est connecté au serveur, des raccourcis pour le connecteur TrayApp et le tableau de bord du serveur s’affichent dans le menu Démarrer, qui peut être utilisé comme suit (si votre ordinateur exécute Windows 8, Windows 8.1 ou Windows 10, le tableau de bord et le connecteur TrayApp sera disponible à partir de l’écran de démarrage d’ordinateur s):  
  
    -   Si votre ordinateur exécute Windows 8, Windows 8.1 ou Windows 10, le tableau de bord et TrayApp connecteur seront consultables en tant qu’une application.  
  
    -   À partir de la TrayApp connecteur, vous pouvez activer ou désactiver le **maintenir la connexion à distance connecté** fonctionnalité. Vous pouvez également double-cliquer sur le TrayApp pour démarrer Launchpad. À partir du Launchpad, vous pouvez accéder au raccourci dossiers partagés, configurer des sauvegardes de l’ordinateur, résoudre les alertes et ouvrez le site Web accès Web à distance.  
  
    -   À partir de la **tableau de bord** lien, vous pouvez administrer votre serveur.  
  
###  <a name="BKMK_10"></a>Connecter des ordinateurs à un serveur Windows Server Essentials sans joindre le domaine  
 Cette rubrique décrit comment ajouter un ordinateur Windows 7, Windows 8, Windows 8.1 ou Windows 10 à un réseau Windows Server Essentials sans joindre l’ordinateur au domaine Windows Server Essentials dans un déploiement de client local. Cette méthode de connexion est prise en charge dans Windows Server Essentials et Windows Server Essentials.  
  
 Il s’agit d’une alternative à la méthode habituelle, qui demande de joindre l’ordinateur au domaine Windows Server Essentials. Avec cette méthode, si l’ordinateur est dans un autre domaine, il doit être supprimé de ce domaine avant qu’il peut être ajouté au domaine Windows Server Essentials.  
  
#### <a name="feature-limits"></a>Limitations des fonctionnalités  
 Certaines fonctionnalités sont limitées lorsqu’un ordinateur client n’est pas ajouté au domaine Windows Server Essentials:  
  
-   Toutes les fonctionnalités qui nécessitent que l’ordinateur soit joint au domaine?, y compris les informations d’identification de domaine, la stratégie de groupe et réseaux privés virtuels (VPN)? ne sont pas disponibles.  
  
-   Les modules complémentaires tiers qui nécessitent que l’ordinateur soit joint au domaine ne fonctionnera pas correctement.  
  
-   Cette méthode ne peut pas être utilisée pour connecter un ordinateur externe au serveur.  
  
#### <a name="prerequisites"></a>Conditions préalables  
  
-   L’ordinateur doit disposer d’une connexion physique au réseau local.  
  
-   Un des systèmes d’exploitation suivants doit être installé sur l’ordinateur:  
  
    -   Windows 10 Professionnel, Windows 10 entreprise  
  
    -    Windows 8.1 Pro, Windows 8.1 entreprise, Windows 8 Professionnel, Windows 8 entreprise  
  
    -    Windows 7 Professionnel (x86 et x64), Windows 7 entreprise (x86 et x64), Windows 7 Édition intégrale (x86 et x64)  
  

-   L’ordinateur doit répondre à tous les autres conditions requises pour les ordinateurs clients dans Windows Server Essentials. Pour plus d’informations, voir [conditions préalables pour connecter un ordinateur au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_2).  

  
-   Pour activer une connexion sans joindre le domaine, vous devez vous connecter l’ordinateur avec un compte qui est membre du groupe Administrateurs local.  
  
-   Pour vous connecter à l’ordinateur au serveur Windows Server Essentials, vous devez les informations de compte suivante:  
  
    -   Un compte avec des droits d’administrateur sur Windows Server Essentials (votre compte).  
  
    -   Le nom d’utilisateur et mot de passe pour le compte de domaine de la personne qui utilisera l’ordinateur. Le compte de domaine doit également disposer de droits d’administrateur sur le serveur Windows Server Essentials.  
  
#### <a name="connect-to-a-windows-server-essentials-network"></a>Se connecter à un réseau Windows Server Essentials  
 Après avoir vérifié que toutes les conditions préalables sont remplies, connectez l’ordinateur au réseau Windows Server Essentials.  
  
###### <a name="to-connect-a-computer-in-a-different-domain-to-a-windows-server-essentials-network"></a>Pour connecter un ordinateur dans un domaine différent à un réseau Windows Server Essentials  
  
1.  Connectez-vous à l’ordinateur client avec un compte qui est membre du groupe Administrateurs local.  
  
2.  Ouvrez une invite de commandes avec des droits d’administrateur.  
  
    -   Dans Windows 10, cliquez sur le **Démarrer** bouton **toutes les applications** -> **système Windows** -> **invite de commandes**, avec le bouton droit d’invite de commandes, puis cliquez sur **exécuter en tant qu’administrateur**.  
  
    -   Dans Windows 8, sur le **Démarrer** , tapez **commande** et appuyez sur ENTRÉE. Dans les résultats, cliquez sur **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.  
  
    -   Dans Windows 7, sur le **Démarrer** menu, entrez **commande** dans la zone de recherche, cliquez sur **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur**.  
  
3.  À l’invite de commandes, tapez la commande suivante et appuyez sur ENTRÉE:  
  
    ```  
    reg add "HKLM\SOFTWARE\Microsoft\Windows Server\ClientDeployment" /v SkipDomainJoin /t REG_DWORD /d 1  
    ```  
  

4.  Effectuez les étapes de [connecter des ordinateurs au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

  
####  <a name="BKMK_SecondServer"></a>Joindre un second serveur au réseau  
  
###### <a name="to-join-a-second-server-to-the-network"></a>Pour joindre un second serveur au réseau  
  
1.  Ouvrez une session sur le serveur que vous souhaitez vous connecter au réseau Windows Server Essentials.  
  
2.  Ouvrez un navigateur Internet et dans la barre d’adresses, tapez **http://<servername\>/Connect**, où *< servername\ >* est le nom du serveur exécutant Windows Server Essentials, et appuyez sur ENTRÉE.  
  
3.  Si la Configuration de sécurité renforcée d’Internet Explorer est activée sur le serveur que vous essayez de vous connecter au réseau Windows Server Essentials, procédez comme suit; Sinon, ignorez cette étape.  
  
    1.  Pour accepter le message de blocage, cliquez sur **fermer**.  
  
    2.  Ajouter le **http://<servername\>/Connect** site Web pour les sites Web de confiance en procédant comme suit:  
  
        1.  Dans le volet de navigation de navigateur, cliquez sur **outils**, puis cliquez sur **Options Internet**.  
  
        2.  Cliquez sur le **sécurité** onglet, puis cliquez sur **sites de confiance**.  
  
        3.  Cliquez sur **Sites**.  
  
        4.  Le site Web doit être indiqué dans le **ajouter ce site Web à la zone** champ. Cliquez sur **ajouter**.  
  
        5.  Cliquez sur **fermer**, puis cliquez sur **OK**.  
  
    3.  Actualisez la page web.  
  

    4.  Pour connecter le second serveur à un serveur exécutant Windows Server Essentials, suivez les instructions de [connecter des ordinateurs au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  

  
    > [!NOTE]
    >  Connexion d’un second serveur à un serveur exécutant Windows Server Essentials se distingue de se connecter à un ordinateur client comme suit:  
    >   
    >  -   Il n’existe aucune migration de profil utilisateur; Par conséquent, le profil actuel n’est pas migré.  
    > -   Vous ne pouvez pas sauvegarder le second serveur à l’aide de sauvegarde de l’ordinateur client, et il n’existe aucune option permettant de sortir l’ordinateur pour la sauvegarde.  
  
 Après avoir joint le second serveur à un serveur qui exécute Windows Server Essentials, les fonctionnalités suivantes sont fournies au serveur connecté:  
  
-   Mise à jour et des fonctionnalités d’état alertes fonctionnent de la même manière que les autres ordinateurs clients.  
  
-   Les fonctionnalités en ligne et hors connexion sont les mêmes que les autres ordinateurs clients.  
  
-   Vous pouvez vous connecter à votre second serveur à l’aide de l’accès Web à distance.  
  
-   Le second serveur figureront dans les rapports d’intégrité, car Windows Server Essentials génère les alertes liées à ce serveur.  
  
 Gestion du second serveur à partir du serveur qui exécute Windows Server Essentials se distingue de la gestion des autres ordinateurs clients comme suit:  
  
-   Il n’y aura aucun point d’entrée pour TrayApp, Launchpad et Client du tableau de bord.  
  
-   Le second serveur est répertorié dans le **serveurs** groupe sur le **périphériques** onglet.  
  
-   Étant donné que la sauvegarde de l’ordinateur client n’est pas prise en charge pour le second serveur, l’état de sauvegarde s’affiche en tant que **ne pas pris en charge**. En outre, si vous sélectionnez le second serveur et avec le bouton droit, il n’existe aucun sauvegarde et de restauration des tâches associées sont affichées pour le second serveur.  
  
-   Si vous sélectionnez le second serveur, puis cliquez sur le **permet d’afficher les propriétés du serveur** de tâches, il est sans **sauvegarde** onglet affiché sur la page de propriétés de serveur s.  
  
-   Dans la mesure où il n’y a pas de centre de sécurité sur un système d’exploitation Windows, l’état de sécurité deuxième serveur s’affiche sous la forme **non applicable**.  
  
-   Le second serveur s état de la stratégie de groupe affiche en tant que **non applicable**.  
  
###  <a name="BKMK_11"></a>Installer le logiciel Connecteur  
 Le logiciel connecteur dans Windows Server Essentials est installé lorsque vous vous connectez votre ordinateur au serveur à l’aide de la connexion d’un ordinateur à l’Assistant de serveur. Vous pouvez lancer cet Assistant en tapant **http://<ServerName\>/connect** dans la barre d’adresses de votre navigateur web (où *< ServerName\ >* est le nom de votre serveur).  
  
> [!NOTE]
>  Si votre ordinateur est dans un emplacement distant, pour exécuter la connexion à un ordinateur à l’Assistant serveur, tapez **http://<domainname\>/connect** dans la barre d’adresses de votre navigateur web (où *< domain\ >* est le nom de domaine de votre organisation). Vous pouvez obtenir des informations sur les nom de domaine auprès de votre administrateur réseau.  
  
 Le logiciel connecteur effectue les opérations suivantes:  
  
-   Se connecte votre ordinateur à Windows Server Essentials  
  
-   Sauvegarde automatiquement votre ordinateur chaque nuit (si vous configurez le serveur pour créer des sauvegardes client)  
  
-   Aide l’administrateur à surveiller l’intégrité de votre ordinateur  
  
-   Vous permet de configurer et administrer à distance Windows Server Essentials à partir de votre ordinateur personnel  
  

 Pour obtenir des instructions détaillées sur la connexion de votre ordinateur au serveur Windows Server Essentials, consultez [connecter des ordinateurs au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).   

  
###  <a name="BKMK_12"></a>Déplacer les données de l’ordinateur et les paramètres manuellement  
  Windows Server Essentials et Windows Server Essentials en charge la migration de profil utilisateur uniquement pour les ordinateurs clients qui exécutent le système d’exploitation Windows 7. Lorsque vous vous connectez un ordinateur Windows 7 au serveur, la connexion de l’ordinateur à l’Assistant serveur peut faire migrer automatiquement le profil utilisateur.  
  
 Le profil utilisateur ne peut pas être transféré automatiquement lors de la connexion d’un Windows 8, Windows 8.1 ou Windows 10 sur le serveur. Toutefois, sur un ordinateur Windows 8, vous pouvez utiliser transfert Windows pour transférer des données et les paramètres de l’utilisateur local d’origine à l’ordinateur joint au domaine. Pour ce faire, vous devez être un administrateur sur l’ordinateur Windows 8 source et l’ordinateur de destination Windows 8. Pour plus d’informations sur l’utilisation de transfert Windows pour transférer des fichiers et paramètres, voir [article 2735227](https://support.microsoft.com/kb/2735227) dans la Base de connaissances Microsoft.  
  
###  <a name="BKMK_Transfer"></a>Transférer plusieurs profils utilisateur pendant le déploiement de l’ordinateur  
 Avant de vous connecter à un ordinateur exécutant le système d’exploitation de Windows 7 SP1 ou de Windows 7 sur le serveur Windows Server Essentials, pour transférer plusieurs profils utilisateur locaux que vous devez d’abord créer les comptes d’utilisateur réseau correspondants sur le serveur. Pour plus d’informations sur la création des comptes d’utilisateur réseau, voir [ajouter un compte d’utilisateur](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1).  
  
 Migration de profil utilisateur est uniquement pris en charge sur un ordinateur exécutant Windows 7 (pour Windows Server Essentials) ou Windows 7 SP1 (pour Windows Server Essentials). Lorsque vous vous connectez un ordinateur au serveur Windows Server Essentials à l’aide de la connexion de l’ordinateur à l’Assistant de serveur, vous disposez d’une option pour déplacer les données et paramètres utilisateur des anciens comptes d’utilisateur locaux vers les nouveaux comptes d’utilisateur réseau. Pour ce faire, sur le **déplacer les données utilisateur existantes et des paramètres** page de l’Assistant, mappez les comptes d’utilisateur réseau aux comptes d’utilisateur local qui existe sur l’ordinateur pour transférer plusieurs profils utilisateur qui sont trouvent sur l’ordinateur client.  
  
###  <a name="BKMK_13"></a>Désinstaller le logiciel Connecteur  
 Vous pouvez désinstaller le logiciel Connecteur à partir d’un ordinateur à l’aide du Panneau de configuration. Vous serez généralement le faire s’il existe un problème avec le logiciel connecteur ou si vous avez besoin installer une version plus récente du logiciel connecteur. Vous devez être connecté à l’ordinateur en tant qu’administrateur pour effectuer cette procédure.  
  
> [!IMPORTANT]
>  Si vous mettez à niveau le système d’exploitation sur un ordinateur client, le logiciel connecteur est automatiquement désinstallé. Vous devez réinstaller le logiciel Connecteur une fois la mise à niveau terminée. La méthode recommandée consiste à désinstaller le logiciel Connecteur avant que vous mettez à niveau le système d’exploitation. La désinstallation du logiciel de connecteur une fois la mise à niveau terminée est toujours acceptable; Toutefois, il peut entraîner un état incohérent de l’ordinateur client avec le serveur jusqu'à ce que le logiciel connecteur est désinstallé et réinstallé.  
  
##### <a name="to-uninstall-connector-software-from-a-computer"></a>Pour désinstaller le logiciel Connecteur à partir d’un ordinateur  
  
1.  À partir d’un ordinateur qui exécute Windows 7, Windows 8, Windows 8.1 ou Windows 10, ouvrez **le panneau de configuration**, puis, dans le **programmes** , cliquez sur **afficher les mises à jour installées**.  
  
2.  Dans la liste des programmes installés, sélectionnez **connecteur Windows Server Essentials**, puis cliquez sur **désinstaller**.  
  
3.  Sur la page d’avertissement, cliquez sur **Oui**.  
  
4.  Si le **contrôle de compte d’utilisateur** fenêtre s’affiche, cliquez sur **autoriser**.  
  
5.  Dans Windows Server Essentials, si la page du connecteur Windows Server Essentials suggère de fermer Launchpad, cliquez sur **OK**.  
  
6.  Attendez que le programme à désinstaller. Une fois le logiciel supprimé, **connecteur Windows Server Essentials** n’apparaît plus dans la liste des programmes installés ou les mises à jour. En outre, les raccourcis vers Launchpad et le tableau de bord ne sont plus affichés sur le bureau de l’ordinateur.  
  
> [!NOTE]
>  -   La désinstallation du logiciel connecteur ne supprime pas l’ordinateur à partir de la liste des ordinateurs qui sont affichés sur le **périphériques** onglet du tableau de bord. Pour supprimer l’ordinateur à partir du tableau de bord, consultez [supprimer un ordinateur à partir du serveur](../manage/Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3).  
> -   Lorsque vous désinstallez le logiciel Connecteur, les dossiers partagés sur l’ordinateur client qui ont été mappés au serveur ne sont pas supprimés. Vous devez supprimer manuellement les dossiers partagés qui sont mappées sur le serveur.  

> -   La désinstallation du logiciel Connecteur n’effectue pas l’ordinateur annuler la jonction au domaine d’origine. Vous devez supprimer manuellement la jonction l’ordinateur du domaine. Pour obtenir des instructions, voir [supprimer un ordinateur d’un domaine Windows](Get-Connected-in-Windows-Server-Essentials.md#BKMK_8).  

  
###  <a name="BKMK_14"></a>Déconnecter votre ordinateur à partir ou reconnectez votre ordinateur au serveur  
 Pour déconnecter un ordinateur à partir du serveur, vous devez effectuer les étapes suivantes:  
  

1.  Désinstallez le logiciel Connecteur de l’ordinateur à l’aide du Panneau de configuration. Pour obtenir des instructions pas à pas, voir [désinstaller le logiciel Connecteur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13).   

  
2.  Détachez l’ordinateur du domaine Windows Server Essentials et le joindre au groupe de travail. Pour obtenir des instructions pas à pas pour la jonction de Windows à un groupe de travail [rejoindre ou créer un groupe de travail](https://windows.microsoft.com/windows7/Join-or-create-a-workgroup).  
  
3.  Supprimer l’ordinateur du serveur à l’aide du tableau de bord. Pour obtenir des instructions pas à pas, voir [supprimer un ordinateur à partir du serveur](../manage/Manage-Devices-in-Windows-Server-Essentials.md#BKMK_3).  
  
 Pour reconnecter un ordinateur au serveur qui a été précédemment déconnecté de votre réseau de serveur Windows Server Essentials, vous devez effectuer les étapes suivantes:  
  

1.  Désinstallez le logiciel Connecteur de l’ordinateur à l’aide du Panneau de configuration. Pour obtenir des instructions pas à pas, voir [désinstaller le logiciel Connecteur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_13).  
  
2.  Détachez l’ordinateur du domaine Windows Server Essentials et le joindre au groupe de travail. Pour obtenir des instructions pas à pas pour la jonction de Windows à un groupe de travail [rejoindre ou créer un groupe de travail](https://windows.microsoft.com/windows7/Join-or-create-a-workgroup).  
  
3.  Connectez l’ordinateur au serveur à l’aide de l’Assistant Connect Computer. Pour obtenir des instructions pas à pas, voir [connecter des ordinateurs au serveur](Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  
  
###  <a name="BKMK_Sleep"></a>Comment fonctionne la sauvegarde avec mise en veille et modes de mise en veille prolongée  
 Si vous sélectionnez le **sortir cet ordinateur pour la sauvegarde** option lorsque vous vous connectez un ordinateur au serveur, l’ordinateur sera automatiquement sortir du mode veille ou veille prolongée chaque jour, comme indiqué dans la planification de sauvegarde afin qu’il puisse être sauvegardé. Une fois la sauvegarde terminée, l’ordinateur retourne en veille ou veille prolongée, selon ses paramètres de gestion de l’alimentation. Si vous ne sélectionnez pas cette option, le serveur sauvegardera pas un ordinateur si l’ordinateur est en veille ou veille prolongée. Pour plus d’informations, voir [gérer la sauvegarde Client](../manage/Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md).  
  
##  <a name="BKMK_C"></a>Utiliser Launchpad  
 Vous pouvez utiliser Launchpad pour accéder aux ressources partagées à partir du serveur Windows Server Essentials, effectuer des sauvegardes de l’ordinateur et répondre aux alertes d’intégrité système.  
  
-   [Vue d’ensemble de LaunchPad](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md)  
  
-   [Utiliser Launchpad avec un ordinateur Mac](../manage/Overview-of-the-Launchpad-in-Windows-Server-Essentials.md#BKMK_Mac)  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Résoudre les problèmes de connexion des ordinateurs au serveur](../support/Troubleshoot-connecting-computers-to-the-server-in-Windows-Server-Essentials.md)  
  
-   [Gérer les comptes d’utilisateur](../manage/Manage-User-Accounts-in-Windows-Server-Essentials.md)  
  

-   [Utiliser des dossiers partagés](Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [Travailler à distance](Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Lire des médias numériques](Play-Digital-Media-in-Windows-Server-Essentials.md)

-   [Utiliser des dossiers partagés](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md)  
  
-   [Travailler à distance](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Lire des médias numériques](../use/Play-Digital-Media-in-Windows-Server-Essentials.md)

