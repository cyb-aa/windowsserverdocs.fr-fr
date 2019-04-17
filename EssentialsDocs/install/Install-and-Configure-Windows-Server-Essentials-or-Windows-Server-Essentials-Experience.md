---
title: "Installer et configurer WindowsServerEssentials ou expérience WindowsServerEssentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 48ea6cd4-3955-4aaf-9236-2515a6c3e730
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 8a2310b178663c6ca32a4e07d11656f1aaf2a11b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="install-and-configure-windows-server-essentials-or-windows-server-essentials-experience"></a>Installer et configurer WindowsServerEssentials ou expérience WindowsServerEssentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

WindowsServerEssentials est un premier serveur idéal pour les petites entreprises comptant jusqu'à 25utilisateurs et 50appareils. Pour les organisations comptant jusqu'à 100utilisateurs et 200appareils, vous pouvez désormais utiliser Windows Server2012R2 avec le rôle expérience WindowsServerEssentials installé. Cette rubrique décrit les deux scénarios.  
  
Expérience WindowsServerEssentials est un rôle dans Windows Server2016 qui vous permet de tirer parti de toutes les fonctionnalités (telles que la sauvegarde de l’accès Web à distance et PC) qui sont disponibles dans WindowsServerEssentials sans les verrous et les limites qui sont appliquées dans WindowsServerEssentials. Ce rôle de serveur est également disponible dans WindowsServerEssentials et est activé par défaut.
  
Avant d’installer WindowsServerEssentials ou le rôle expérience Essentials, notez les limitations suivantes.  
  
|Expérience WindowsServerEssentials dans WindowsServerEssentials|Expérience WindowsServerEssentials dans Windows Server2016
|----|----|
|-Doit être le contrôleur de domaine à la racine de la forêt et le domaine et détenir tous les rôles FSMO.<br /><br /> -Ne peut pas être installé dans un environnement avec un domaine ActiveDirectory existant (Toutefois, il est une période de grâce de 21jours pour effectuer des migrations).|-N’a pas à être un contrôleur de domaine s’il est installé dans un environnement avec un domaine ActiveDirectory existant.<br /><br /> -Si un domaine ActiveDirectory n’existe pas, l’installation du rôle créera un domaine ActiveDirectory et le serveur deviendra le contrôleur de domaine à la racine de la forêt et du domaine, contenant tous les rôles FSMO.  
|Peut uniquement être déployé dans un domaine unique.|Peut uniquement être déployé dans un domaine unique.  
|Un contrôleur de domaine en lecture seule ne peut pas exister dans votre domaine.|Un contrôleur de domaine en lecture seule ne peut pas exister dans votre domaine.

> [!NOTE]
>  Pour télécharger des versions d’évaluation des systèmes d’exploitation, visitez le centre d’évaluation TechNet:  
>   
>  [Télécharger Windows Server2016](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016)  
>   
>  [Télécharger WindowsServerEssentials](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016-essentials)
  
## <a name="installation-options"></a>Options d’installation  
 Ce document fournit des instructions pas à pas pour installer et configurer WindowsServerEssentials. Selon votre environnement réseau, vous disposez des options d’installation suivantes à votre disposition:  
  
-    WindowsServerEssentials (avec le rôle expérience WindowsServerEssentials activé par défaut)  
  
-    Windows Server2016 avec le rôle expérience WindowsServerEssentials  
 
|Environnement de déploiement|Description|Section connexe|  
|----------------------------|-----------------|---------------------|  
|Nouvel environnement ActiveDirectory|Vous pouvez installer WindowsServerEssentials pour créer un environnement ActiveDirectory.|[Déployer WindowsServerEssentials pour configurer un environnement ActiveDirectory](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_NewAD)|  
|Environnement ActiveDirectory existant|Vous pouvez installer WindowsServerEssentials dans un environnement ActiveDirectory existant.|[Déployer WindowsServerEssentials dans un environnement ActiveDirectory existant](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_ExistingAD)|  
|Environnement virtuel|Vous pouvez déployer WindowsServerEssentials en tant qu’un ordinateur virtuel.|[Virtualiser votre environnement](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_VirtualWSE)|  
|Déploiement automatisé|Vous pouvez automatiser le déploiement de WindowsServerEssentials à l’aide de Windows PowerShell.|[Installer et configurer WindowsServerEssentials à l’aide de Windows PowerShell](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_PowerShell)|  
  
## <a name="before-you-begin"></a>Avant de commencer  
 Avant de commencer l’installation, passez en revue la documentation suivante:  
  
-   [Présentation de WindowsServerEssentials](https://www.microsoft.com/server-cloud/windows-server-essentials/windows-server-2012-r2-essentials.aspx)  
  

-   [Configuration système requise pour WindowsServerEssentials](../get-started/system-requirements.md)   

  
##  <a name="BKMK_NewAD"></a>Déployer WindowsServerEssentials pour configurer un environnement ActiveDirectory  
 WindowsServerEssentials fournit un moyen pour pouvoir configurer rapidement un environnement ActiveDirectory et les composants serveur associés.  
  
###  <a name="BKMK_WSEDeploy"></a>Déploiement de WindowsServerEssentials  
 Si vous utilisez WindowsServerEssentials, expérience WindowsServerEssentials est déjà activé. Toutefois, vous devez effectuer certaines étapes pour configurer votre serveur.  
  
##### <a name="to-configure-windows-server-essentials-on-a-physical-server"></a>Pour configurer WindowsServerEssentials sur un serveur physique  
  
1.  Après le Windows **Bienvenue** page, le **Assistant Configurer WindowsServerEssentials** est visible sur votre bureau.  
  
2.  Suivez les instructions pour terminer l’Assistant comme suit:  
  
    1.  Sur le **configurer WindowsServerEssentials**, cliquez sur **suivant**.  
  
    2.  Dans **paramètres**, assurez-vous que votre date et heure fuseau horaire sont corrects, puis cliquez sur **suivant**.  
  
    3.  Dans **informations sur la société**, tapez le nom de votre société, tel que **Contoso, Ltd.**, puis cliquez sur **suivant**. Si vous le souhaitez, vous pouvez modifier le nom de domaine interne et le nom de serveur.  
  
    4.  Dans **créer d’administration réseau**, tapez un nouveau nom du compte administrateur et le mot de passe.  
  
        > [!NOTE]
        >  N’utilisez pas la valeur par défaut **administrateur** nom de compte et mot de passe.  
  
    5.  Cliquez sur **configurer**.  
  
3.  Le serveur redémarre plusieurs fois pendant le processus de configuration, et les ouvertures de session sera automatique jusqu'à ce que la configuration est terminée. Ce processus prend environ 20minutes.  
  
4.  Sur le bureau, cliquez sur l’icône du tableau de bord pour démarrer le serveur du tableau de bord. Sur le **accueil** page, complétez le **prise en main** tâches répertoriées sur le **le programme d’installation** onglet.  
  
 Après avoir terminé la configuration du serveur, le serveur qui exécute WindowsServerEssentials être configuré comme contrôleur de domaine.  
  
###  <a name="BKMK_DeployWSERole"></a>Déploiement du rôle expérience WindowsServerEssentials dans Windows Server2012R2 Standard Edition et Datacenter  
 Vous pouvez utiliser le Gestionnaire de serveur pour activer et configurer le rôle expérience WindowsServerEssentials dans Windows Server2012R2 Standard ou Windows Server2012R2 Datacenter à l’aide de la procédure suivante.  
  
##### <a name="to-deploy-the-windows-server-essentials-experience-role-in-windows-server-2012-r2"></a>Pour déployer le rôle expérience WindowsServerEssentials dans Windows Server2012R2  
  
1.  Connectez-vous à votre serveur en tant qu’un administrateur local.  
  
2.  Ouvrez **le Gestionnaire de serveur**, puis cliquez sur **Ajout de rôles et fonctionnalités**.  
  
3.  Dans **sélectionner des rôles de serveur**, sélectionnez le **expérience WindowsServerEssentials** rôle. Dans la boîte de dialogue, cliquez sur **ajouter des fonctionnalités**, puis cliquez sur **suivant**.  
  
4.  Dans **fonctionnalités**, cliquez sur **suivant**.  
  
5.  Passez en revue les **expérience WindowsServerEssentials** description du rôle, puis cliquez sur **suivant**.  
  
6.  Dans les pages qui suivent, cliquez sur **suivant**, puis cliquez sur la page de confirmation, **installer**.  
  
7.  Une fois l’installation terminée, l’expérience WindowsServerEssentials doit être répertorié comme rôle serveur dans le Gestionnaire de serveur.  
  
8.  Dans la zone de notification d’indicateur dans le Gestionnaire de serveur, cliquez sur l’indicateur, puis cliquez sur **configurer WindowsServerEssentials**.  
  
9. (Facultatif) Modifier le nom du serveur si nécessaire.  
  
    > [!IMPORTANT]
    >  Vous ne pouvez pas modifier le nom du serveur une fois que vous avez configuré WindowsServerEssentials.  
  

10. Suivez l’Assistant pour configurer WindowsServerEssentials comme décrit précédemment dans la [Deploying WindowsServerEssentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) section.  

10. Suivez l’Assistant pour configurer WindowsServerEssentials comme décrit précédemment dans la [Deploying WindowsServerEssentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) section.  

  
##  <a name="BKMK_ExistingAD"></a>Déployer WindowsServerEssentials dans un environnement ActiveDirectory existant  
 Vous pouvez également déployer WindowsServerEssentials si votre organisation possède déjà un environnement ActiveDirectory existant. En outre, vous pouvez choisir si vous voulez déployer WindowsServerEssentials comme contrôleur de domaine.  
  
> [!IMPORTANT]
>  Cette option est uniquement disponible si vous déployez le rôle expérience WindowsServerEssentials dans Windows Server2012R2 Standard ou Windows Server2012R2 Datacenter.  
  
#### <a name="to-deploy-windows-server-essentials-in-an-existing-active-directory-environment"></a>Pour déployer WindowsServerEssentials dans un environnement ActiveDirectory existant  
  
1.  (Facultatif) Modifier le nom du serveur si nécessaire.  
  
    > [!IMPORTANT]
    >  Vous ne pouvez pas modifier le nom du serveur une fois que vous avez configuré WindowsServerEssentials.  
  
2.  Joignez votre serveur exécutant WindowsServerEssentials à un domaine existant comme suit:  
  
    1.  Si vous souhaitez que ce serveur soit un contrôleur de domaine, configurez le serveur en tant que contrôleur de domaine répliqué.  
  
    2.  Si vous ne souhaitez pas que ce serveur soit un contrôleur de domaine, joindre ce serveur à votre domaine en utilisant les outils natifs de Windows.  
  
3.  Redémarrez le serveur et ouvrez une session sur le serveur en tant qu’administrateur de domaine.  
  
4.  Ouvrez le Gestionnaire de serveur, puis cliquez sur **Ajout de rôles et fonctionnalités **.  
  
5.  Sur les pages qui suivent, cliquez sur **suivant **.  
  
6.  Dans **sélectionner des rôles de serveur**, sélectionnez **expérience WindowsServerEssentials **. Dans la boîte de dialogue, cliquez sur **ajouter des fonctionnalités**, puis cliquez sur **suivant**.  
  
7.  Dans **fonctionnalités**, cliquez sur **suivant**.  
  
8.  Passez en revue les **expérience WindowsServerEssentials** description, puis cliquez sur **suivant **.  
  
9. Dans les pages qui suivent, cliquez sur **suivant**, puis cliquez sur la page de confirmation, **installer**.  
  
10. Une fois l’installation terminée, l’expérience de WindowsServerEssentials s’afficheront en tant que rôle serveur dans le Gestionnaire de serveur.  
  
11. Dans la zone de notification d’indicateur dans **le Gestionnaire de serveur**, cliquez sur l’indicateur, puis cliquez sur **configurer WindowsServerEssentials**.  
  
12. Suivez l’Assistant pour configurer WindowsServerEssentials. Selon votre configuration ActiveDirectory, vous serez informé si vous configurez WindowsServerEssentials sur un contrôleur de domaine ou en tant que membre du domaine. Cliquez sur **configurer** pour commencer la configuration. Le processus de configuration prend environ 10minutes.  
  
##  <a name="BKMK_VirtualWSE"></a>Virtualiser votre environnement  
  WindowsServerEssentials, Windows Server2012R2 Standard et Datacenter de Windows Server2012R2 peuvent être exécutée en tant qu’ordinateurs virtuels. Vous pouvez exécuter des ordinateurs virtuels en utilisant les outils de gestion Hyper-V sur un serveur exécutant Hyper-V. À partir du point de vue de la gestion des licences, WindowsServerEssentials vous permet de configurer le rôle Hyper-V et de virtualiser votre environnement. La licence vous permet de configurer un autre système d’exploitation invité qui exécute WindowsServerEssentials. En fonction de votre fournisseur de système «la configuration de s, WindowsServerEssentials vous permet de configurer un environnement virtualisé en toute transparence.  
  
#### <a name="to-deploy-windows-server-essentials-as-a-virtual-machine"></a>Pour déployer WindowsServerEssentials en tant qu’un ordinateur virtuel  
  
1.  Après la page d’accueil de Windows (en fonction de votre fournisseur «la s configuration système), le **avant de commencer** page fournit une option pour configurer WindowsServerEssentials en tant qu’une instance virtuelle ou sur du matériel physique. La disponibilité de ces options est prédéfinie par votre fournisseur de système et les deux options peuvent ne pas être toujours disponibles. Pour installer WindowsServerEssentials en tant qu’un ordinateur virtuel, dans **installer WindowsServerEssentials**, sélectionnez **installer en tant qu’instance virtuelle**, puis cliquez sur **configurer **.  
  
2.  L’Assistant configure un ordinateur virtuel qui prend environ cinq minutes.  
  

3.  Ensuite, configurez WindowsServerEssentials comme décrit précédemment dans la [Deploying WindowsServerEssentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) section.  

3.  Ensuite, configurez WindowsServerEssentials comme décrit précédemment dans la [Deploying WindowsServerEssentials](Install-and-Configure-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md#BKMK_WSEDeploy) section.  

  
##  <a name="BKMK_PowerShell"></a>Installer et configurer WindowsServerEssentials à l’aide de Windows PowerShell  
 Vous pouvez automatiser l’installation de WindowsServerEssentials à l’aide des applets de commande Windows PowerShell.  
  
#### <a name="to-install-windows-server-essentials-by-using-windows-powershell"></a>Pour installer WindowsServerEssentials à l’aide de Windows PowerShell  
  
1.  Ouvrez la console Windows PowerShell à partir d’une invite de commandes avec élévation de privilèges.  
  
2.  Installer le rôle expérience WindowsServerEssentials à l’aide de la commande suivante:  
  
    ```  
    Add-WindowsFeature ServerEssentialsRole  
    ```  
  
3.  Exécutez `Get-Help Start-WssConfigurationService`.  
  
    1.  Exécutez la commande suivante pour démarrer la configuration de WindowsServerEssentials comme contrôleur de domaine:  
  
        ```  
        Start-WssConfigurationService -CompanyName "ContosoTest" -DNSName "ContosoTest.com" -NetBiosName "ContosoTest" -ComputerName "YourServerName  œNewAdminCredential $cred  
        ```  
  
    2.  Exécutez la commande suivante pour démarrer la configuration de WindowsServerEssentials comme membre d’un domaine existant. Vous devez être membre du groupe Administrateurs de l’entreprise et groupe Administrateurs du domaine dans ActiveDirectory pour effectuer cette tâche:  
  
        ```  
        Start-WssConfigurationService  œCredential <Your Credential>  
  
        ```  
  
4.  Surveiller la progression de l’installation en utilisant les commandes suivantes:  
  
    -   Pour que l’état d’installation affiche sur la barre de progression, exécutez `Get-WssConfigurationStatus  œShowProgress`.  
  
    -   Pour obtenir la progression immédiate sans la barre de progression, exécutez `Get-WssConfigurationStatus`.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Quelles sont les nouveautés dans WindowsServerEssentials](../get-started/what-s-new.md)  
  
-   [Installer WindowsServerEssentials](Install-Windows-Server-Essentials.md)  
  
-   [Prise en main WindowsServerEssentials](../get-started/get-started.md)
