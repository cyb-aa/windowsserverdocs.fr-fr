---
title: Installer et configurer Windows Server Essentials ou Expérience Windows Server Essentials
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 48ea6cd4-3955-4aaf-9236-2515a6c3e730
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b0323c8ce2ac69ade9adeca7e948c728e4791f09
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311660"
---
# <a name="install-and-configure-windows-server-essentials-or-windows-server-essentials-experience"></a>Installer et configurer Windows Server Essentials ou Expérience Windows Server Essentials

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Windows Server Essentials est un premier serveur idéal pour les petites entreprises comptant jusqu’à 25 utilisateurs et 50 appareils. Pour les organisations disposant d’un maximum de 100 utilisateurs et de 200 appareils, vous pouvez désormais utiliser Windows Server 2012 R2 avec le rôle expérience Windows Server Essentials installé. Cette rubrique décrit les deux scénarios.  
  
L’expérience Windows Server Essentials est un rôle de Windows Server 2016 qui vous permet de tirer parti de toutes les fonctionnalités (telles que les Accès web à distance et la sauvegarde de PC) disponibles dans Windows Server Essentials sans les verrous et les limites appliqués dans.  Windows Server Essentials. Ce rôle serveur est également disponible dans Windows Server Essentials et est activé par défaut.
  
Avant d’installer Windows Server Essentials ou le rôle expérience Essentials, notez les limitations suivantes.  
  
|Expérience Windows Server Essentials dans Windows Server Essentials|Expérience Windows Server Essentials dans Windows Server 2016
|----|----|
|-Doit être le contrôleur de domaine à la racine de la forêt et du domaine, et doit contenir tous les rôles FSMO.<br /><br /> -Ne peut pas être installé dans un environnement avec un domaine Active Directory préexistant (Toutefois, il existe une période de grâce de 21 jours pour l’exécution des migrations).|-Ne doit pas nécessairement être un contrôleur de domaine s’il est installé dans un environnement avec un domaine Active Directory préexistant.<br /><br /> -Si un domaine Active Directory n’existe pas, l’installation du rôle créera un domaine Active Directory et le serveur deviendra le contrôleur de domaine à la racine de la forêt et du domaine, contenant tous les rôles FSMO.  
|Peut uniquement être déployé dans un domaine unique.|Peut uniquement être déployé dans un domaine unique.  
|Un contrôleur de domaine en lecture seule ne peut pas exister dans votre domaine.|Un contrôleur de domaine en lecture seule ne peut pas exister dans votre domaine.

> [!NOTE]
>  Pour télécharger des versions d'évaluation des systèmes d'exploitation, visitez le centre d'évaluation TechNet :  
>   
>  [Télécharger Windows Server 2016](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016)  
>   
>  [Télécharger Windows Server Essentials](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016-essentials)
  
## <a name="installation-options"></a>Options d’installation  
 Ce document fournit des instructions pas à pas pour installer et configurer Windows Server Essentials. Selon votre environnement, vous disposez des options d'installation suivantes :  
  
-    Windows Server Essentials (avec le rôle expérience Windows Server Essentials activé par défaut)  
  
-    Windows Server 2016 avec le rôle expérience Windows Server Essentials installé  
 
|Environnement de déploiement|Description|Section connexe|  
|----------------------------|-----------------|---------------------|  
|Nouvel environnement Active Directory|Vous pouvez installer Windows Server Essentials pour créer un environnement Active Directory.|[Déployer Windows Server Essentials pour configurer un nouvel environnement de Active Directory](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_NewAD)|  
|Environnement Active Directory existant|Vous pouvez installer Windows Server Essentials dans un environnement Active Directory existant.|[Déployer Windows Server Essentials dans un environnement de Active Directory existant](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_ExistingAD)|  
|Environnement virtuel|Vous pouvez déployer Windows Server Essentials en tant qu'ordinateur virtuel.|[Virtualiser votre environnement](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_VirtualWSE)|  
|Déploiement automatisé|Vous pouvez automatiser le déploiement de Windows Server Essentials à l'aide de Windows PowerShell.|[Installer et configurer Windows Server Essentials à l’aide de Windows PowerShell](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_PowerShell)|  
  
## <a name="before-you-begin"></a>Avant de commencer  
 Avant de commencer l'installation, passez en revue la documentation suivante :  
  
-   [Présentation du produit Windows Server Essentials](https://www.microsoft.com/server-cloud/windows-server-essentials/windows-server-2012-r2-essentials.aspx)  
  

-   [Configuration requise pour Windows Server Essentials](../get-started/system-requirements.md)   

  
##  <a name="deploy-windows-server-essentials-to-set-up-a-new-active-directory-environment"></a><a name="BKMK_NewAD"></a>Déployer Windows Server Essentials pour configurer un nouvel environnement de Active Directory  
 Windows Server Essentials permet de configurer rapidement un environnement Active Directory et les composants serveur associés.  
  
###  <a name="deploying-windows-server-essentials"></a><a name="BKMK_WSEDeploy"></a>Déploiement de Windows Server Essentials  
 Si vous utilisez Windows Server Essentials, l’expérience Windows Server Essentials est déjà activée. Toutefois, vous devez effectuer certaines étapes pour configurer votre serveur.  
  
##### <a name="to-configure-windows-server-essentials-on-a-physical-server"></a>Pour configurer Windows Server Essentials sur un serveur physique  
  
1. Après la page **Bienvenue** de Windows, l'**Assistant Configurer Windows Server Essentials** est visible sur le Bureau.  
  
2. Suivez les instructions pour exécuter l'Assistant comme suit :  
  
   1.  Dans la page **Configurer Windows Server Essentials**, cliquez sur **Suivant**.  
  
   2.  Dans **Paramètres d'heure**, assurez-vous que la date, l'heure et le fuseau horaire sont corrects, puis cliquez sur **Suivant**.  
  
   3.  Dans **Informations sur la société**, tapez le nom de votre société, tel que **Contoso,Ltd.** , puis cliquez sur **Suivant**. Si vous le souhaitez, vous pouvez modifier le nom de serveur et le nom de domaine interne.  
  
   4.  Dans **Créer un compte administrateur**, tapez un nouveau nom de compte d'administrateur et un nouveau mot de passe.  
  
       > [!NOTE]
       >  N'utilisez pas le nom de compte et le mot de passe par défaut **Administrateur** .  
  
   5.  Cliquez sur **Configurer**.  
  
3. Le serveur redémarre plusieurs fois pendant le processus de configuration et les ouvertures de session sont automatiques jusqu'à la fin de la configuration. Ce processus prend environ 20 minutes.  
  
4. Sur le Bureau, cliquez sur l'icône du tableau de bord pour démarrer le tableau de bord du serveur. Dans la page d'**accueil**, effectuez les tâches **Mise en route** répertoriées sous l'onglet **Installation**.  
  
   Une fois la configuration du serveur terminée, le serveur qui exécute Windows Server Essentials est configuré comme contrôleur de domaine.  
  
###  <a name="deploying-the-windows-server-essentials-experience-role-in-windows-server-2012-r2-standard-and-datacenter"></a><a name="BKMK_DeployWSERole"></a>Déploiement du rôle expérience Windows Server Essentials dans Windows Server 2012 R2 Standard et Datacenter  
 Vous pouvez utiliser Gestionnaire de serveur pour activer et configurer le rôle expérience Windows Server Essentials dans Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter à l’aide de la procédure suivante.  
  
##### <a name="to-deploy-the-windows-server-essentials-experience-role-in-windows-server-2012-r2"></a>Pour déployer le rôle Expérience Windows Server Essentials dans Windows Server 2012 R2  
  
1.  Ouvrez une session sur votre serveur en tant qu'administrateur local.  
  
2.  Ouvrez le **Gestionnaire de serveur**, puis cliquez sur **Ajouter des rôles et des fonctionnalités**.  
  
3.  Dans **Sélectionner des rôles de serveurs**, sélectionnez le rôle **Expérience Windows Server Essentials**. Dans la boîte de dialogue, cliquez sur **Ajouter des fonctionnalités**, puis cliquez sur **Suivant**.  
  
4.  Dans **Fonctionnalités**, cliquez sur **Suivant**.  
  
5.  Passez en revue la description du rôle **Expérience Windows Server Essentials**, puis cliquez sur **Suivant**.  
  
6.  Dans les pages qui suivent, cliquez sur **Suivant** puis, dans la page de confirmation, cliquez sur **Installer**.  
  
7.  Une fois l’installation terminée, l’expérience Windows Server Essentials doit être indiquée comme rôle serveur dans Gestionnaire de serveur.  
  
8.  Dans la zone de notification du Gestionnaire de serveur, cliquez sur l'indicateur, puis sur **Configurer Windows Server Essentials**.  
  
9. (Facultatif) Modifiez le nom du serveur si nécessaire.  
  
    > [!IMPORTANT]
    >  Vous ne pouvez pas modifier le nom du serveur une fois que vous avez configuré Windows Server Essentials.  
  

10. Suivez les instructions de l’Assistant pour configurer Windows Server Essentials comme décrit précédemment dans la section [déploiement de Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) .  

10. Suivez les instructions de l’Assistant pour configurer Windows Server Essentials comme décrit précédemment dans la section [déploiement de Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) .  

  
##  <a name="deploy-windows-server-essentials-in-an-existing-active-directory-environment"></a><a name="BKMK_ExistingAD"></a>Déployer Windows Server Essentials dans un environnement de Active Directory existant  
 Vous pouvez également déployer Windows Server Essentials si votre organisation possède déjà un environnement Active Directory. En outre, vous pouvez choisir si vous voulez déployer Windows Server Essentials en tant que contrôleur de domaine.  
  
> [!IMPORTANT]
>  Cette option est disponible uniquement si vous déployez le rôle expérience Windows Server Essentials dans Windows Server 2012 R2 Standard ou Windows Server 2012 R2 Datacenter.  
  
#### <a name="to-deploy-windows-server-essentials-in-an-existing-active-directory-environment"></a>Pour déployer Windows Server Essentials dans un environnement Active Directory existant  
  
1.  (Facultatif) Modifiez le nom du serveur si nécessaire.  
  
    > [!IMPORTANT]
    >  Vous ne pouvez pas modifier le nom du serveur une fois que vous avez configuré Windows Server Essentials.  
  
2.  Joignez votre serveur Windows Server Essentials à un domaine existant en procédant comme suit :  
  
    1.  Si vous voulez que ce serveur soit un contrôleur de domaine, configurez-le en tant que contrôleur de domaine répliqué.  
  
    2.  Si vous ne voulez pas que ce serveur soit un contrôleur de domaine, joignez-le à votre domaine en utilisant les outils natifs de Windows.  
  
3.  Redémarrez le serveur et ouvrez une session sur le serveur en tant qu'administrateur de domaine.  
  
4.  Ouvrez le Gestionnaire de serveur, puis cliquez sur **Ajouter des rôles et des fonctionnalités**.  
  
5.  Dans les pages qui suivent, cliquez sur **Suivant**.  
  
6.  Dans **Sélectionner des rôles de serveurs**, sélectionnez **Expérience Windows Server Essentials**. Dans la boîte de dialogue, cliquez sur **Ajouter des fonctionnalités**, puis cliquez sur **Suivant**.  
  
7.  Dans **Fonctionnalités**, cliquez sur **Suivant**.  
  
8.  Passez en revue la description du rôle **Expérience Windows Server Essentials**, puis cliquez sur **Suivant**.  
  
9. Dans les pages qui suivent, cliquez sur **Suivant** puis, dans la page de confirmation, cliquez sur **Installer**.  
  
10. Une fois l’installation terminée, l’expérience Windows Server Essentials est indiquée comme rôle serveur dans Gestionnaire de serveur.  
  
11. Dans la zone de notification d'indicateur dans **Gestionnaire de serveur**, cliquez sur l'indicateur, puis sur **Configurer Windows Server Essentials**.  
  
12. Suivez l'Assistant pour configurer Windows Server Essentials. En fonction de votre configuration Active Directory, vous êtes informé si vous configurez Windows Server Essentials sur un contrôleur de domaine ou en tant que membre du domaine. Cliquez sur **Configurer** pour commencer la configuration. Le processus de configuration prend environ 10 minutes.  
  
##  <a name="virtualize-your-environment"></a><a name="BKMK_VirtualWSE"></a>Virtualiser votre environnement  
  Windows Server Essentials, Windows Server 2012 R2 Standard et Windows Server 2012 R2 Datacenter peuvent être exécutés en tant que machines virtuelles. Vous pouvez exécuter des ordinateurs virtuels en utilisant les outils de gestion Hyper-V sur un serveur Hyper-V. Du point de vue des licences, Windows Server Essentials vous permet de configurer le rôle Hyper-V et de virtualiser votre environnement. La licence vous permet de configurer un autre système d’exploitation invité qui exécute Windows Server Essentials. En fonction de la configuration de « ¢ s » du fournisseur système, Windows Server Essentials vous permet de configurer un environnement virtualisé en toute transparence.  
  
#### <a name="to-deploy-windows-server-essentials-as-a-virtual-machine"></a>Pour déployer Windows Server Essentials en tant qu'ordinateur virtuel  
  
1.  Après la page d’accueil de Windows (en fonction de la configuration de « ¢ s » du fournisseur système, la page **avant de commencer** fournit une option permettant de configurer Windows Server Essentials en tant qu’instance virtuelle ou sur du matériel physique. La disponibilité de ces options est prédéfinie par votre fournisseur de système et les deux options peuvent ne pas être toujours disponibles. Pour installer Windows Server Essentials en tant que machine virtuelle, dans **installer Windows Server Essentials**, sélectionnez **installer en tant qu’instance virtuelle**, puis cliquez sur **configurer**.  
  
2.  L'Assistant configure un ordinateur virtuel, ce qui prend environ cinq minutes.  
  

3.  Ensuite, configurez Windows Server Essentials comme décrit précédemment dans la section [déploiement de Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) .  

3.  Ensuite, configurez Windows Server Essentials comme décrit précédemment dans la section [déploiement de Windows Server Essentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) .  

  
##  <a name="install-and-configure-windows-server-essentials-by-using-windows-powershell"></a><a name="BKMK_PowerShell"></a>Installer et configurer Windows Server Essentials à l’aide de Windows PowerShell  
 Vous pouvez automatiser l'installation de Windows Server Essentials à l'aide des applets de commande Windows PowerShell.  
  
#### <a name="to-install-windows-server-essentials-by-using-windows-powershell"></a>Pour installer Windows Server Essentials à l'aide de Windows PowerShell  
  
1.  Ouvrez la console Windows PowerShell à partir d'une invite de commandes avec élévation de privilèges.  
  
2.  Installez le rôle expérience Windows Server Essentials à l’aide de la commande suivante :  
  
    ```  
    Add-WindowsFeature ServerEssentialsRole  
    ```  
  
3.  Exécutez `Get-Help Start-WssConfigurationService`.  
  
    1.  Exécutez la commande suivante pour démarrer la configuration de Windows Server Essentials comme contrôleur de domaine :  
  
        ```  
        Start-WssConfigurationService -CompanyName "ContosoTest" -DNSName "ContosoTest.com" -NetBiosName "ContosoTest" -ComputerName "YourServerName  œNewAdminCredential $cred  
        ```  
  
    2.  Exécutez la commande suivante pour démarrer la configuration de Windows Server Essentials comme membre d'un domaine existant. Vous devez être membre des groupes Administrateurs de l'entreprise et Administrateurs du domaine dans Active Directory pour effectuer cette tâche :  
  
        ```  
        Start-WssConfigurationService  œCredential <Your Credential>  
  
        ```  
  
4.  Surveillez la progression de l'installation en utilisant les commandes suivantes :  
  
    -   Pour afficher l’état de l’installation dans la barre de progression, exécutez `Get-WssConfigurationStatus  œShowProgress`.  
  
    -   Pour obtenir la progression immédiate sans la barre de progression, exécutez `Get-WssConfigurationStatus`.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Nouveautés de Windows Server Essentials](../get-started/what-s-new.md)  
  
-   [Installer Windows Server Essentials](Install-Windows-Server-Essentials.md)  
  
-   [Prise en main de Windows Server Essentials](../get-started/get-started.md)
