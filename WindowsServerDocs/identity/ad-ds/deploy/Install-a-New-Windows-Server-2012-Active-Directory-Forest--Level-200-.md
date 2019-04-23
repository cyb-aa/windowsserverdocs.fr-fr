---
ms.assetid: b3d6fb87-c4d4-451c-b3de-a53d2402d295
title: Installer une nouvelle forêt Active Directory Windows Server 2012 (niveau 200)
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 187db7e201e98ae97268b96c2e4faa202a9a5372
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874830"
---
# <a name="install-a-new-windows-server-2012-active-directory-forest-level-200"></a>Installer une nouvelle forêt Active Directory Windows Server 2012 (niveau 200)

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique présente les bases de la nouvelle fonctionnalité de promotion du contrôleur de domaine des services de domaine Active Directory Windows Server 2012. Dans Windows Server 2012, les services de domaine Active Directory remplacent l'outil Dcpromo par un système de déploiement basé sur le Gestionnaire de serveur et Windows PowerShell.  
  
-   [Administration simplifiée des Services de domaine Active Directory](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SimplifiedAdmin)  
  
-   [Vue d’ensemble technique](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_TechOverview)  
  
-   [Déploiement d’une forêt avec le Gestionnaire de serveur](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SMForest)  
  
-   [Déploiement d’une forêt avec Windows PowerShell](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_PSForest)  
  
## <a name="BKMK_SimplifiedAdmin"></a>Administration simplifiée des Services de domaine Active Directory  
Windows Server 2012 présente la future génération de l'administration simplifiée des services de domaine Active Directory et incarne la nouvelle conception de domaine la plus radicale depuis Windows 2000 Server. L'administration simplifiée AD DS tire ses enseignements de douze années d'Active Directory pour créer une expérience d'administration mieux prise en charge, plus souple et plus intuitive pour les architectes et administrateurs. Cela revient à créer des versions de technologies existantes ainsi qu'à étendre les fonctions des composants fournis dans Windows Server 2008 R2.  
  
### <a name="what-is-ad-ds-simplified-administration"></a>Qu'est-ce que l'administration simplifiée AD DS ?  
L'administration simplifiée AD DS réinvente le déploiement de domaine. Ces fonctionnalités sont notamment :  
  
-   Le déploiement de rôle AD DS fait maintenant partie de la nouvelle architecture de Gestionnaire de serveur et permet une installation à distance.  
  
-   Le moteur de configuration et de déploiement des services AD DS est maintenant Windows PowerShell, même pendant l'utilisation d'un programme d'installation graphique.  
  
-   La promotion inclut maintenant la vérification de la configuration requise qui valide la disponibilité de la forêt et du domaine pour le nouveau contrôleur de domaine, ce qui réduit le risque d'échec des promotions.  
  
-   Le niveau fonctionnel de forêt Windows Server 2012 n'implémente pas de nouvelles fonctionnalités et le niveau fonctionnel de domaine est requis uniquement pour un sous-ensemble de nouvelles fonctionnalités Kerberos, ce qui décharge les administrateurs du besoin fréquent d'un environnement de contrôleur de domaine homogène.  
  
### <a name="purpose-and-benefits"></a>Objectif et avantages  
Ces modifications peuvent sembler plus complexes et non plus simples. Cependant, pour la nouvelle conception du processus de déploiement des services AD DS, il a été possible de regrouper de nombreuses étapes et meilleures pratiques en moins d'actions plus faciles. Cela signifie, par exemple, que la configuration graphique d'un nouveau contrôleur de domaine répliqué est effectuée à l'aide de huit boîtes de dialogue contre douze auparavant. La création d'une forêt Active Directory requiert une *seule* commande Windows PowerShell avec *un* seul argument : le nom du domaine.  
  
Pourquoi accorder une telle importance à Windows PowerShell dans Windows Server 2012 ? Avec l'évolution des traitements distribués, Windows PowerShell propose un moteur unique pour la configuration et la maintenance à partir des interfaces graphique et de ligne de commande. Il permet de créer des scripts de composant très complets en offrant aux professionnels de l'informatique les mêmes fonctions de programmation qu'offre une API aux développeurs. Avec l'omniprésence de l'informatique cloud, Windows PowerShell permet aussi finalement d'administrer un serveur à distance, un ordinateur sans interface graphique ayant les mêmes fonctions de gestion qu'un autre avec un écran et une souris.  
  
Un administrateur des services AD DS expérimenté devrait constater que ses connaissances acquises sont hautement pertinentes. Un administrateur débutant trouvera que le processus d'apprentissage est beaucoup plus léger.  
  
## <a name="BKMK_TechOverview"></a>Vue d’ensemble technique  
  
### <a name="what-you-should-know-before-you-begin"></a>Ce que vous devez savoir avant de commencer  
Cette rubrique suppose que vous connaissez les versions précédentes des services de domaine Active Directory et ne fournit pas de détails élémentaires sur leurs objectifs ni leurs fonctionnalités. Pour plus d'informations sur les services AD DS, voir les pages du portail TechNet dont les liens figurent ci-dessous :  
  
-   [Services de domaine Active Directory pour Windows Server 2008 R2](https://technet.microsoft.com/library/dd378801(WS.10).aspx)  
  
-   [Services de domaine Active Directory pour Windows Server 2008](https://technet.microsoft.com/library/dd378891(WS.10).aspx)  
  
-   [Référence technique de Windows Server](https://technet.microsoft.com/library/cc739127(WS.10).aspx)  
  
### <a name="functional-descriptions"></a>Descriptions fonctionnelles  
  
#### <a name="ad-ds-role-installation"></a>Installation de rôle AD DS  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_SelectServerRoles.gif)  
  
L'installation des services de domaine Active Directory utilise le Gestionnaire de serveur et Windows PowerShell, comme tous les autres rôles et fonctionnalités dans Windows Server 2012. Le programme Dcpromo.exe ne fournit plus d'options de configuration de l'interface graphique utilisateur.  
  
Vous utilisez un Assistant graphique dans le Gestionnaire de serveur ou le module ServerManager pour Windows PowerShell à la fois dans les installations locales et à distance. En exécutant plusieurs instances de ces Assistants ou applets de commande et en ciblant différents serveurs, vous pouvez déployer les services AD DS vers plusieurs contrôleurs de domaine à la fois et à partir d'une seule console. Même si ces nouvelles fonctionnalités ne sont pas à compatibilité descendante avec Windows Server 2008 R2 ni des systèmes d'exploitation antérieurs, vous pouvez aussi utiliser encore l'application Dism.exe introduite dans Windows Server 2008 R2 pour une installation de rôle en local à partir de la ligne de commande classique.  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSAddWindowsFeature.png)  
  
#### <a name="ad-ds-role-configuration"></a>Configuration de rôle AD DS  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_DeploymentConfiguration_Forest.gif)  
  
Configuration de répertoire les Services de domaine Active « auparavant appelée DCPROMO » est maintenant une opération discrète à partir de l’installation du rôle. Après avoir installé le rôle AD DS, un administrateur configure le serveur comme un contrôleur de domaine en utilisant un Assistant distinct dans le Gestionnaire de serveur ou le module Windows PowerShell ADDSDeployment.  
  
La configuration de rôle AD DS s'appuie sur douze ans d'expérience sur le terrain et configure maintenant les contrôleurs de domaine selon les meilleures pratiques Microsoft les plus récentes. Par exemple, DNS (Domain Name System) et les catalogues globaux sont installés par défaut sur chaque contrôleur de domaine.  
  
L’Assistant de configuration du Gestionnaire de serveur AD DS fusionne de nombreuses boîtes de dialogue individuelles en moins d’invites et ne masque plus les paramètres dans un mode « avancé ». L'intégralité du processus de promotion est contenue dans une boîte de dialogue de taille dynamique pendant l'installation. L'Assistant et le module Windows PowerShell ADDSDeployment vous indiquent les problèmes de sécurité et changements notables, avec des liens vers des informations supplémentaires.  
  
Dcpromo.exe demeure dans Windows Server 2012 pour les installations de ligne de commande sans assistance uniquement et n'exécute plus l'Assistant Installation graphique. Il est vivement conseillé de ne plus utiliser Dcpromo.exe pour les installations sans assistance et de le remplacer par le module ADDSDeployment, car l'exécutable maintenant déconseillé ne sera pas inclus dans la prochaine version de Windows.  
  
Ces nouvelles fonctionnalités ne sont pas à compatibilité descendante avec Windows Server 2008 R2 ni des systèmes d'exploitation plus anciens.  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDSForest.png)  
  
> [!IMPORTANT]  
> Dcpromo.exe ne contient plus d'Assistant graphique et n'installe plus des fichiers binaires de rôles ni de fonctionnalités. Si vous tentez d'exécuter Dcpromo.exe à partir de l'interpréteur de commandes de l'Explorateur, le message suivant s'affiche :  
>   
> « L’Assistant Installation des Services de domaine Active Directory est déplacé dans le Gestionnaire de serveur. Pour plus d’informations, consultez https://go.microsoft.com/fwlink/?LinkId=220921. »  
>   
> Si vous tentez d'exécuter Dcpromo.exe /unattend, les fichiers binaires sont quand même installés, comme dans les systèmes d'exploitation antérieurs, mais l'avertissement suivant s'affiche :  
>   
> « Le dcpromo opération sans assistance est remplacée par le module ADDSDeployment pour Windows PowerShell. Pour plus d’informations, consultez https://go.microsoft.com/fwlink/?LinkId=220924. »  
>   
> Windows Server 2012 déconseille dcpromo.exe qui ne sera pas inclus dans les futures versions de Windows, et aucune autre amélioration ne lui sera apportée dans ce système d'exploitation. Les administrateurs doivent arrêter son utilisation et passer aux modules Windows PowerShell pris en charge s'ils veulent créer des contrôleurs de domaine à partir de la ligne de commande.  
  
#### <a name="prerequisite-checking"></a>Vérification de la configuration requise  
La configuration de contrôleur de domaine implémente également une phase de vérification de la configuration requise qui évalue la forêt et le domaine avant de poursuivre la promotion du contrôleur de domaine. Cette phase inclut la disponibilité des rôles FSMO, les privilèges d'utilisateur, la compatibilité du schéma étendu et d'autres exigences. Cette nouvelle conception atténue les problèmes si la promotion du contrôleur de domaine démarre, puis s'arrête à mi-chemin avec une erreur de configuration fatale. Elle réduit le risque de trouver des métadonnées de contrôleur de domaine orphelines dans la forêt ou un serveur qui croit à tort qu'il est contrôleur de domaine.  
  
## <a name="BKMK_SMForest"></a>Déploiement d’une forêt avec le Gestionnaire de serveur  
Cette section explique comme installer le premier contrôleur de domaine dans un domaine racine de forêt à l'aide du Gestionnaire de serveur sur un ordinateur Windows Server 2012 graphique.  
  
### <a name="server-manager-ad-ds-role-installation-process"></a>Processus d'installation de rôle AD DS avec le Gestionnaire de serveur  
Le diagramme ci-dessous illustre le processus d'installation de rôle des services de domaine Active Directory, qui commence quand vous exécutez ServerManager.exe et se termine juste avant la promotion du contrôleur de domaine.  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_servermanagerdeployment.png)  
  
#### <a name="server-pool-and-add-roles"></a>Pool de serveurs et ajout de rôles  
Tous les ordinateurs Windows Server 2012 accessibles à partir de l'ordinateur exécutant le Gestionnaire de serveur peuvent être mis en pool. Une fois mis en pool, vous sélectionnez ces serveurs pour une installation à distance des services AD DS ou toute autre option de configuration possible dans le Gestionnaire de serveur.  
  
Pour ajouter des serveurs, choisissez l'une des méthodes suivantes :  
  
-   Cliquez sur **Ajouter d'autres serveurs à gérer** dans la fenêtre de bienvenue du tableau de bord.  
  
-   Cliquez sur le menu **Gérer**, puis sélectionnez **Ajouter des serveurs**.  
  
-   Cliquez avec le bouton droit sur **Tous les serveurs** et choisissez **Ajouter des serveurs**  
  
La boîte de dialogue Ajouter des serveurs s'affiche :  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddServers.png)  
  
Vous avez alors trois moyens d'ajouter des serveurs au pool pour les utiliser ou les regrouper :  
  
-   Recherche Active Directory (utilise LDAP, requiert que les ordinateurs appartiennent à un domaine, autorise le filtrage du système d'exploitation et prend en charge les caractères génériques)  
  
-   Recherche DNS (utilise l'alias DNS ou l'adresse IP via une diffusion ARP ou NetBIOS ou une recherche WINS, n'autorise pas le filtrage du système d'exploitation et ne prend pas en charge les caractères génériques)  
  
-   Importation (utilise une liste au format texte des serveurs séparés par CR/LF)  
  
Cliquez sur **Rechercher maintenant** pour retourner une liste des serveurs de ce même domaine Active Directory auquel l'ordinateur est joint, puis cliquez sur un ou plusieurs noms de serveur dans la liste. Cliquez sur la flèche droite pour ajouter les serveurs à la liste **Sélectionné**. Utilisez la boîte de dialogue **Ajouter des serveurs** pour ajouter des serveurs sélectionnés aux groupes de rôles du tableau de bord. Vous pouvez également cliquer sur **Gérer**, puis sur **Créer un groupe de serveurs** ou sur **Créer un groupe de serveurs** dans la fenêtre **BIENVENUE DANS GESTIONNAIRE DE SERVEUR** du tableau de bord pour créer des groupes de serveurs personnalisés.  
  
> [!NOTE]  
> La procédure Ajouter des serveurs ne valide pas le fait qu’un serveur est en ligne ou accessible. Toutefois, tous les serveurs inaccessibles sont marqués d'un indicateur dans l'affichage Facilité de gestion du Gestionnaire de serveur à la prochaine actualisation.  
  
Vous pouvez installer des rôles à distance sur n'importe quel ordinateur Windows Server 2012 ajouté au pool, comme illustré ci-dessous :  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/tADDS_SMI_TR_AddRolesFeatures.png)  
  
Vous ne pouvez pas gérer entièrement des serveurs exécutant des systèmes d'exploitation antérieurs à Windows Server 2012. La sélection **Ajouter des rôles et fonctionnalités** exécute le module Windows PowerShell ServerManager **Install-WindowsFeature**.  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddADDSToAnotherServer.png)  
  
Vous pouvez également utiliser le tableau de bord du Gestionnaire de serveur sur un contrôleur de domaine existant pour sélectionner l'installation des services AD DS sur un serveur distant avec le rôle déjà présélectionné en cliquant avec le bouton droit sur la vignette du tableau de bord des services AD DS et en sélectionnant **Ajouter AD DS à un autre serveur**. Cela revient à appeler **Install-WindowsFeature AD-Domain-Services**.  
  
L'ordinateur sur lequel vous exécutez le Gestionnaire de serveur se met en pool automatiquement. Pour installer le rôle AD DS ici, il vous suffit de cliquer sur le menu **Gérer** et sur **Ajouter des rôles et fonctionnalités**.  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ManageAddRoles.png)  
  
#### <a name="installation-type"></a>Type d’installation  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectInstallationType.png)  
  
La boîte de dialogue **Type d'installation** fournit une option qui ne prend pas en charge les services de domaine Active Directory : **Installation basée sur un scénario des services Bureau à distance**. Cette option autorise uniquement les services Bureau à distance dans une charge de travail distribuée multiserveur. Si vous la sélectionnez, les services de domaine Active Directory ne peuvent pas être installés.  
  
Laissez toujours la sélection par défaut en place lors de l'installation des services de domaine Active Directory : **Installation basée sur un rôle ou une fonctionnalité**.  
  
#### <a name="server-selection"></a>Sélection du serveur  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectDestinationServer.png)  
  
La boîte de dialogue **Sélection du serveur** vous permet de choisir l'un des serveurs précédemment ajoutés au pool, tant qu'il est accessible. Le serveur local exécutant le Gestionnaire de serveur est automatiquement disponible.  
  
En outre, vous pouvez sélectionner des fichiers VHD Hyper-V hors connexion avec le système d'exploitation Windows Server 2012 et le Gestionnaire de serveur leur ajoute directement le rôle via la maintenance des composants. Cela vous permet d'approvisionner les serveurs virtuels avec les composants nécessaires avant de les configurer davantage.  
  
#### <a name="server-roles-and-features"></a>Rôles et fonctionnalités de serveur  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectServerRoles.png)  
  
Sélectionnez le rôle **Services de domaine Active Directory** si vous envisagez de promouvoir un contrôleur de domaine. Toutes les fonctionnalités d'administration et tous les services requis Active Directory sont automatiquement installes, même s'ils font soi-disant partie d'un autre rôle ou ne semblent pas être sélectionnés dans l'interface du Gestionnaire de serveur.  
  
Le Gestionnaire de serveur présente également une boîte de dialogue d'information qui indique les fonctionnalités de gestion installées implicitement par ce rôle ; cela est équivalent à l'argument **-IncludeManagementTools** .  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddFeaturesDialog.gif)  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectFeatures.png)  
  
D'autres **Fonctionnalités** peuvent être ajoutées ici si vous le souhaitez.  
  
#### <a name="active-directory-domain-services"></a>Services de domaine Active Directory  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ADDSIntro.png)  
  
La boîte de dialogue **Services de domaine Active Directory** fournit des informations limitées sur la configuration requise et les meilleures pratiques. Il agit principalement comme une confirmation que vous avez choisi le rôle AD DS « Si cet écran n’apparaît pas, vous n’avez pas sélectionné AD DS.  
  
#### <a name="confirmation"></a>Confirmation  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Confirmation.png)  
  
La boîte de dialogue **Confirmation** est le point de contrôle final avant le début de l'installation du rôle. Elle permet de redémarrer l'ordinateur si nécessaire après l'installation du rôle, mais l'installation des services AD DS ne nécessite pas de redémarrage.  
  
En cliquant sur **Installer**, vous confirmez que vous êtes prêt à commencer l'installation du rôle. Vous ne pouvez pas annuler une installation de rôle une fois qu'elle a commencé.  
  
#### <a name="results"></a>Résultats  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Results.png)  
  
La boîte de dialogue **Résultats** indique la progression et l'état de l'installation en cours. L'installation de rôle continue que le Gestionnaire de serveur soit fermé ou non.  
  
La vérification des résultats de l'installation est toujours recommandée. Si vous fermez la boîte de dialogue **Résultats** avant la fin de l'installation, vous pouvez vérifier les résultats à l'aide de l'indicateur de notification du Gestionnaire de serveur. Le Gestionnaire de serveur affiche également un message d'avertissement pour tous les serveurs qui ont installé le rôle AD DS mais qui n'ont pas été davantage configurés en tant que contrôleurs de domaine.  
  
**Notifications de tâche**  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_TaskNotofications.png)  
  
**Détails AD DS**  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ADDSDetails.png)  
  
**Détails de la tâche**  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_TaskDetails.png)  
  
#### <a name="promote-to-domain-controller"></a>Promouvoir en contrôleur de domaine  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Promote.png)  
  
À la fin de l'installation du rôle AD DS, vous pouvez poursuivre la configuration en utilisant le lien **Promouvoir ce serveur en contrôleur de domaine** . Cette opération est requise pour transformer le serveur en contrôleur de domaine, mais n'est pas nécessaire pour exécuter l'Assistant Configuration immédiatement. Par exemple, vous pouvez vouloir uniquement approvisionner les serveurs avec les fichiers binaires AD DS avant de les envoyer à une autre filiale pour une configuration ultérieure. En ajoutant le rôle AD DS avant l'expédition, vous gagnez du temps quand il arrive à destination. Vous suivez également la meilleure pratique qui consiste à ne pas garder un contrôleur de domaine hors connexion pendant des jours ou des semaines. Enfin, vous avez la possibilité de mettre à jour les composants avant la promotion du contrôleur de domaine, ce qui vous épargne au moins un autre redémarrage.  
  
La sélection de ce lien à une étape ultérieure appelle les applets de commande ADDSDeployment : **install-addsforest**, **install-addsdomain** ou **install-addsdomaincontroller**.  
  
### <a name="uninstallingdisabling"></a>Désinstallation/Désactivation  
Vous supprimez le rôle AD DS comme n'importe quel autre rôle, que vous ayez ou non promu le serveur en contrôleur de domaine. Toutefois, la suppression du rôle AD DS nécessite un redémarrage quand elle est terminée.  
  
La suppression des rôles dans les services de domaine Active Directory est différente de l'installation, car elle nécessite une rétrogradation du contrôleur de domaine pour être effectuée. Cela est nécessaire pour empêcher que les fichiers binaires de rôle d'un contrôleur de domaine ne soient désinstallés sans un nettoyage des métadonnées approprié dans la forêt. Pour plus d’informations, consultez [la rétrogradation des contrôleurs de domaine et de domaines &#40;niveau 200&#41;](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
> [!WARNING]  
> La suppression des rôles AD DS avec Dism.exe ou le module DISM Windows PowerShell après la promotion vers un contrôleur de domaine n'est pas prise en charge et empêche le démarrage normal du serveur.  
>   
> À la différence du Gestionnaire de serveur ou du module de déploiement des services AD DS pour Windows PowerShell, DISM est un système de maintenance natif sans connaissance inhérente des services AD DS ni de leur configuration. N'utilisez pas Dism.exe ni le module DISM Windows PowerShell pour désinstaller le rôle AD DS sauf si le serveur n'est plus un contrôleur de domaine.  
  
### <a name="create-an-ad-ds-forest-root-domain-with-server-manager"></a>Créer un domaine racine de forêt AD DS avec le Gestionnaire de serveur  
Le diagramme suivant illustre le processus de configuration des services de domaine Active Directory quand vous avez auparavant installé le rôle AD DS et démarré l'**Assistant Configuration des services de domaine Active Directory** à l'aide du Gestionnaire de serveur.  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_forestdeploy2.png)  
  
#### <a name="deployment-configuration"></a>Configuration du déploiement  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddNewForest.png)  
  
Le Gestionnaire de serveur commence la promotion de chaque contrôleur de domaine par la page **Configuration de déploiement** . Dans cette page et les pages suivantes, les options disponibles et les champs requis varient selon l’opération de déploiement que vous sélectionnez.  
  
Pour créer une forêt Active Directory, cliquez sur **Ajouter une nouvelle forêt**. Vous devez indiquer un nom de domaine racine valide ; le nom ne peut pas être en une partie (par exemple, le nom doit être *contoso.com* ou équivalent et non uniquement *contoso*) et doit satisfaire aux conditions d'attribution des noms de domaine DNS.  
  
Pour plus d’informations sur les noms de domaine valides, voir l’article de la Base de connaissances [Conventions d’affectation de noms dans Active Directory pour les ordinateurs, domaines, sites et unités d’organisation](https://support.microsoft.com/kb/909264)  
  
> [!WARNING]  
> Ne créez pas de forêts Active Directory portant le même nom qu'un DNS externe. Par exemple, si votre URL DNS Internet est http://contoso.com, vous devez choisir un autre nom pour votre forêt interne éviter les problèmes de compatibilité future. Ce nom doit être unique et faire l'objet d'une utilisation peu probable en termes de trafic web. Par exemple, corp.contoso.com.  
  
Une nouvelle forêt ne nécessite pas de nouvelles informations d'identification pour le compte Administrateur du domaine. Le processus de promotion du contrôleur de domaine utilise les informations d'identification du compte Administrateur intégré à partir du premier contrôleur de domaine utilisé pour créer la racine de forêt. Il n'existe aucun moyen (par défaut) de désactiver ni de verrouiller le compte Administrateur intégré et il peut s'agir du seul point d'entrée dans une forêt si les autres comptes du domaine administratif ne sont pas utilisables. Il est essentiel de connaître le mot de passe avant le déploiement d'une nouvelle forêt.  
  
**DomainName** nécessite un nom DNS de domaine complet valide et est requis.  
  
#### <a name="domain-controller-options"></a>Options du contrôleur de domaine  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_DCOptions_Forest.gif)  
  
La page **Options du contrôleur de domaine** vous permet de configurer le **niveau fonctionnel de forêt** et le **niveau fonctionnel de domaine** pour le nouveau domaine racine de forêt. Par défaut, ces paramètres sont Windows Server 2012 dans un domaine racine de forêt. Le niveau fonctionnel de forêt Windows Server 2012 ne fournit pas de nouvelles fonctionnalités sur le niveau fonctionnel de forêt Windows Server 2008 R2. Le niveau fonctionnel du domaine Windows Server 2012 est requis que pour implémenter les nouveaux paramètres Kerberos « toujours fournir des revendications » et « Échouer les demandes d’authentification non blindées. » Utilisation principale des niveaux fonctionnels dans Windows Server 2012 consiste à limiter la participation dans les domaine aux contrôleurs de domaine qui répond aux exigences minimales le système d’exploitation. En d’autres termes, vous pouvez spécifier de Windows Server 2012 domaine fonctionnel niveau uniquement contrôleurs de domaine qui exécutent Windows Server 2012 peuvent héberger le domaine.  Windows Server 2012 implémente un nouvel indicateur de contrôleur de domaine appelé **DS_WIN8_REQUIRED** dans le **DSGetDcName** fonction de NetLogon qui localise exclusivement les contrôleurs de domaine Windows Server 2012. Vous bénéficiez ainsi de la souplesse d'une forêt plus homogène ou hétérogène qui permet aux systèmes d'exploitation d'être exécutés sur des contrôleurs de domaine.  
  
Pour plus d’informations sur la localisation d’un contrôleur de domaine, voir [Fonctions du service d’annuaire](https://msdn.microsoft.com/library/ms675900(VS.85).aspx).  
  
La seule fonction de contrôleur de domaine configurable est l'option Serveur DNS. Microsoft recommande que tous les contrôleurs de domaine fournissent des services DNS à des fins de haute disponibilité dans les environnements distribués. Cette option est donc sélectionnée par défaut pendant l'installation d'un contrôleur de domaine dans n'importe quel mode ou domaine. Les options de contrôleur de domaine en lecture seule et de catalogue global ne sont pas disponibles pendant la création d'un domaine racine de forêt ; le premier contrôleur de domaine doit être un catalogue global et ne peut pas être un contrôleur de domaine en lecture seule.  
  
Le **Mot de passe du mode de restauration des services d'annuaire** spécifié doit respecter la stratégie de mot de passe appliquée au serveur. Celle-ci, par défaut, ne requiert pas un mot de passe fort, mais seulement un mot de passe non vide. Choisissez toujours un mot de passe fort complexe ou, de préférence, une phrase secrète.  
  
#### <a name="dns-options-and-dns-delegation-credentials"></a>Options DNS et informations d'identification de délégation DNS  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestDNSOptions.png)  
  
La page **Options DNS** vous permet de configurer la délégation DNS et de fournir d'autres informations d'identification d'administration DNS.  
  
Vous ne pouvez pas configurer la délégation ni les options DNS dans l'Assistant Configuration des services de domaine Active Directory pendant l'installation d'un nouveau domaine racine de forêt Active Directory où vous avez sélectionné **Serveur DNS** dans la page **Options du contrôleur de domaine**. L'option **Créer une délégation DNS** est disponible pendant la création d'une zone DNS racine de forêt dans une infrastructure de serveur DNS existante. Cette option vous permet de fournir d'autres informations d'identification d'administration DNS avec les droits de mise à jour de la zone DNS.  
  
Pour plus d’informations sur la nécessité de créer une délégation DNS, voir [Présentation de la délégation de zone](https://technet.microsoft.com/library/cc771640.aspx).  
  
#### <a name="additional-options"></a>Autres options  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestAdditionalOptions.png)  
  
La page **Options supplémentaires** indique le nom NetBIOS du domaine et vous permet de le remplacer. Par défaut, le nom de domaine NetBIOS correspond à la partie la plus à gauche du nom de domaine complet fourni dans la page **Configuration de déploiement** . Par exemple, si vous avez indiqué le nom de domaine complet corp.contoso.com, le nom de domaine NetBIOS par défaut est CORP.  
  
Si le nom contient au maximum 15 caractères et n'est en conflit avec aucun autre nom NetBIOS, il n'est pas modifié. S'il est en conflit avec un autre nom NetBIOS, un numéro est ajouté au nom. Si le nom contient plus de 15 caractères, l'Assistant propose une suggestion tronquée unique. Dans les deux cas, l'Assistant valide d'abord que le nom n'est pas déjà utilisé via une recherche WINS et une diffusion NetBIOS.  
  
Pour plus d’informations sur les noms de domaine valides, voir l’article de la Base de connaissances [Conventions d’affectation de noms dans Active Directory pour les ordinateurs, domaines, sites et unités d’organisation](https://support.microsoft.com/kb/909264)  
  
#### <a name="paths"></a>Chemins d’accès  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestPaths.png)  
  
La page **Chemins d’accès** vous permet de remplacer les emplacements de dossier par défaut de la base de données AD DS, des journaux de transaction de base de données et du partage SYSVOL. Les emplacements par défaut sont toujours dans des sous-répertoires de %systemroot% (autrement dit, C:\Windows).  
  
#### <a name="review-options-and-view-script"></a>Examiner les options et Afficher le script  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestReviewOptions.png)  
  
La page **Examiner les options** vous permet de valider vos paramètres et de vérifier qu'ils répondent à vos exigences avant le démarrage de l'installation. Notez que vous avez encore la possibilité d'arrêter l'installation quand vous utilisez le Gestionnaire de serveur. Il s'agit simplement d'une option permettant de confirmer vos paramètres avant de poursuivre la configuration  
  
La page **Examiner les options** du Gestionnaire de serveur offre également un bouton **Afficher le script** facultatif pour créer un fichier texte Unicode qui contient la configuration ADDSDeployment actuelle sous forme d’un script Windows PowerShell unique. Vous pouvez ainsi utiliser l’interface graphique Gestionnaire de serveur sous forme d’un studio de déploiement Windows PowerShell. Utilisez l’Assistant Configuration des services de domaine Active Directory pour configurer les options, exportez la configuration, puis annulez l’Assistant. Ce processus crée un exemple valide et correct du point de vue syntaxique pour permettre des modifications ultérieures ou une utilisation directe. Exemple :  
  
```powershell 
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSForest `  
-CreateDNSDelegation `  
-DatabasePath "C:\Windows\NTDS" `  
-DomainMode "Win2012" `  
-DomainName "corp.contoso.com" `  
-DomainNetBIOSName "CORP" `  
-ForestMode "Win2012" `  
-InstallDNS:$true `  
-LogPath "C:\Windows\NTDS" `  
-NoRebootOnCompletion:$false `  
-SYSVOLPath "C:\Windows\SYSVOL"  
-Force:$true  
  
```  
  
> [!NOTE]  
> Le Gestionnaire de serveur renseigne généralement tous les arguments avec des valeurs pendant la promotion et ne s'appuie pas sur les valeurs par défaut (car elles peuvent changer entre les futures versions de Windows ou les Service Packs). La seule exception concerne l'argument **-safemodeadministratorpassword** (qui est délibérément omis du script). Pour forcer une demande de confirmation, omettez la valeur en cas d'exécution interactive de l'applet de commande.  
  
#### <a name="prerequisites-check"></a>Vérification de la configuration requise  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestPrereqCheck.png)  
  
La fonctionnalité **Vérification de la configuration requise** est nouvelle dans la configuration de domaine AD DS. Cette nouvelle phase valide que la configuration du serveur est capable de prendre en charge une nouvelle forêt AD DS.  
  
Pendant l'installation d'un nouveau domaine racine de forêt, l'Assistant Configuration des services de domaine Active Directory du Gestionnaire de serveur appelle une série de tests modulaires. Ces tests vous alertent avec des suggestions d'opérations de réparation. Vous pouvez exécuter les tests autant de fois que nécessaire. Le processus du contrôleur de domaine ne peut pas continuer tant que tous les tests de configuration requise ne sont pas réussis.  
  
La fonctionnalité **Vérification de la configuration requise** met également en évidence des informations pertinentes, telles que les modifications de sécurité qui affectent les systèmes d'exploitation plus anciens.  
  
Pour plus d'informations sur les vérifications spécifiques de la configuration requise, consultez la page [Prerequisite Checking](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
#### <a name="installation"></a>Installation  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestInstallation.png)  
  
Lorsque la page **Installation** s'affiche, la configuration du contrôleur de domaine commence et ne peut pas être arrêtée ni annulée. Les opérations détaillées s'affichent dans cette page et sont écrites dans les journaux :  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
> [!NOTE]  
> Vous pouvez exécuter plusieurs Assistants pour l'installation des rôles et la configuration des services de domaine Active Directory à partir de la même console du Gestionnaire de serveur en même temps.  
  
#### <a name="results"></a>Résultats  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
La page **Résultats** indique la réussite ou l'échec de la promotion et toute information d'administration importante. Le contrôleur de domaine redémarre automatiquement après 10 secondes.  
  
## <a name="BKMK_PSForest"></a>Déploiement d’une forêt avec Windows PowerShell  
Cette section explique comme installer le premier contrôleur de domaine dans un domaine racine de forêt à l'aide de Windows PowerShell sur un ordinateur Windows Server 2012 de base.  
  
### <a name="windows-powershell-ad-ds-role-installation-process"></a>Processus d'installation de rôle AD DS avec Windows PowerShell  
En implémentant quelques applets de commande de déploiement ServerManager simples dans vos processus de déploiement, vous concrétisez davantage la vision de l'administration simplifiée AD DS.  
  
La figure suivante illustre le processus d'installation de rôle des services de domaine Active Directory, qui commence quand vous exécutez **PowerShell.exe** et se termine juste avant la promotion du contrôleur de domaine.  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_servermanagerdeployment_powershell.png)  
  
|||  
|-|-|  
|Applet de commande ServerManager|Arguments (les arguments en **gras** sont obligatoires. Les arguments en *italique* peuvent être spécifiés à l'aide de Windows PowerShell ou de l'Assistant Configuration des services de domaine Active Directory.)|  
|Install-WindowsFeature/Add-WindowsFeature|***-Name***<br /><br />*-Restart*<br /><br />*-IncludeAllSubFeature*<br /><br />*-IncludeManagementTools*<br /><br />-Source<br /><br />*-ComputerName*<br /><br />-Credential<br /><br />-LogPath<br /><br />*-Vhd*<br /><br />*-ConfigurationFilePath*|  
  
> [!NOTE]  
> Même s'il n'est pas requis, l'argument **-IncludeManagementTools** est vivement conseillé pendant l'installation des fichiers binaires du rôle AD DS.  
  
Le module ServerManager expose les parties de l'installation du rôle, de l'état et de la suppression du nouveau module DISM pour Windows PowerShell. Cette disposition simplifie la plupart des tâches et réduit le besoin d'utiliser directement le puissant (mais dangereux en cas d'utilisation incorrecte) module DISM.  
  
Utilisez **Get-Command** pour exporter les alias et applets de commande dans ServerManager.  
  
```powershell  
Get-Command -module ServerManager  
```  
  
Exemple :  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSGetCommand.png)  
  
Pour ajouter le rôle Services de domaine Active Directory, exécutez simplement **Install-WindowsFeature** avec le nom de rôle AD DS en tant qu'argument. Comme le Gestionnaire de serveur, tous les services requis implicites du rôle AD DS sont automatiquement installés.  
  
```powershell  
Install-WindowsFeature -name AD-Domain-Services  
```  
  
Si vous voulez également installer les outils de gestion des services AD DS, ce qui est vivement conseillé, indiquez l'argument **-IncludeManagementTools** :  
  
```powershell  
Install-WindowsFeature -name AD-Domain-Services -IncludeManagementTools  
```  
  
Exemple :  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallWinFeature.png)  
  
Pour répertorier l'ensemble des fonctionnalités et rôles avec leur état d'installation, utilisez **Get-WindowsFeature** sans arguments. Spécifiez l'argument **-ComputerName** pour l'état d'installation à partir d'un serveur distant.  
  
```powershell  
Get-WindowsFeature  
```  
  
Comme **Get-WindowsFeature** n'a pas de mécanisme de filtrage, vous devez utiliser **Where-Object** avec un pipeline pour trouver des fonctionnalités spécifiques. Le pipeline est un canal utilisé entre plusieurs applets de commande pour transmettre les données et l'applet de commande Where-Object agit comme un filtre. La variable **$_** intégrée joue le rôle de l'objet actuel qui traverse le pipeline avec toutes les propriétés qu'il peut contenir.  
  
```powershell  
Get-WindowsFeature | where-object <options>  
```  
  
Par exemple, pour retrouver toutes les fonctionnalités contenant « Active Dir » dans leur propriété **Display Name**, utilisez :  
  
```powershell  
Get-WindowsFeature | where displayname -like "*active dir*"  
```  
  
D'autres exemples sont illustrés ci-dessous :  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSGetWindowsFeature.png)  
  
Pour plus d’informations sur d’autres opérations Windows PowerShell avec des pipelines et Where-Object, voir [Définition et utilisation des pipelines dans Windows PowerShell](https://technet.microsoft.com/library/ee176927.aspx).  
  
Notez également que Windows PowerShell 3.0 a considérablement simplifié les arguments de ligne de commande nécessaires dans cette opération de pipeline. Windows PowerShell 2.0 aurait requis les éléments suivants :  
  
```powershell  
Get-WindowsFeature | where {$_.displayname - like "*active dir*"}  
```  
  
En utilisant le pipeline Windows PowerShell, vous pouvez créer des résultats lisibles. Exemple :  
  
```powershell  
Install-WindowsFeature | Format-List  
Install-WindowsFeature | select-object | Format-List  
  
```  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDS.png)  
  
Notez comment l'utilisation de l'applet de commande **Select-Object** avec l'argument **-expandproperty** retourne des données intéressantes :  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDSWithTools.png)  
  
> [!NOTE]  
> L'argument **Select-Object -expandproperty** ralentit légèrement les performances globales de l'installation.  
  
### <a name="BKMK_PS"></a>Créer un domaine racine de forêt AD DS avec Windows PowerShell  
Pour installer une nouvelle forêt Active Directory à l'aide du module ADDSDeployment, utilisez l'applet de commande suivante :  
  
```powershell  
Install-addsforest  
```  
  
L'applet de commande **Install-AddsForest** comprend uniquement deux phases (vérification de la configuration requise et installation). Les deux figures ci-dessous illustrent la phase d'installation avec l'argument requis minimal **-domainname**.  
  
|||  
|-|-|  
|Applet de commande ADDSDeployment|Arguments (les arguments en **gras** sont obligatoires. Les arguments en *italique* peuvent être spécifiés à l'aide de Windows PowerShell ou de l'Assistant Configuration des services de domaine Active Directory.)|  
|install-addsforest|-Confirm<br /><br />*-CreateDNSDelegation*<br /><br />*-DatabasePath*<br /><br />*-DomainMode*<br /><br />***-DomainName***<br /><br />***-DomainNetBIOSName***<br /><br />*-DNSDelegationCredential*<br /><br />*-ForestMode*<br /><br />-Force<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />-NoDnsOnNetwork<br /><br />-NoRebootOnCompletion<br /><br />*-SafeModeAdministratorPassword*<br /><br />-SkipAutoConfigureDNS<br /><br />-SkipPreChecks<br /><br />*-SYSVOLPath*<br /><br />*-Whatif*|  
  
> [!NOTE]  
> L'argument **-DomainNetBIOSName** est requis si vous voulez modifier le nom de 15 caractères automatiquement généré en fonction du préfixe du nom de domaine DNS ou si le nom compte plus de 15 caractères.  
  
L'applet de commande ADDSDeployment **Configuration de déploiement** du Gestionnaire de serveur équivalente et les arguments sont les suivants :  
  
```powershell  
Install-ADDSForest  
-DomainName <string>  
```  
  
Les arguments de l'applet de commande ADDSDeployment Options du contrôleur de domaine du Gestionnaire de serveur équivalents sont les suivants :  
  
```powershell  
-ForestMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-DomainMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-InstallDNS <{$false | $true}>  
-SafeModeAdministratorPassword <secure string>  
  
```  
  
Les arguments **Install-ADDSForest** suivent les mêmes paramètres par défaut que le Gestionnaire de serveur s'ils ne sont pas spécifiés.  
  
Le fonctionnement de l'argument **SafeModeAdministratorPassword** est spécial :  
  
-   S'ils ne sont *pas spécifiés* en tant qu'arguments, l'applet de commande vous invite à entrer et à confirmer un mot de passe masqué. Il s’agit du mode d’utilisation préféré en cas d’exécution interactive de l’applet de commande.  
  
    Par exemple, pour créer une forêt nommée corp.contoso.com et être invité à entrer et à confirmer un mot de passe masqué :  
  
    ```powershell  
    Install-ADDSForest "DomainName corp.contoso.com  
    ```  
  
-   S'ils sont spécifiés *avec une valeur*, la valeur doit être une chaîne sécurisée. Il ne s’agit pas du mode d’utilisation préféré en cas d’exécution interactive de l’applet de commande.  
  
Par exemple, vous pouvez manuellement inviter l'utilisateur à entrer un mot de passe sous forme d'une chaîne sécurisée à l'aide de l'applet de commande **Read-Host**.  
  
```powershell  
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)  
```  
  
> [!WARNING]  
> Étant donné que l’option précédente ne confirme pas le mot de passe, faites preuve de prudence, car le mot de passe n’est pas visible.  
  
Vous pouvez également fournir une chaîne sécurisée sous forme d'une variable en texte clair convertie, bien que ceci soit fortement déconseillé.  
  
```powershell  
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
```  
  
Enfin, vous pouvez stocker le mot de passe obscurci dans un fichier, puis le réutiliser plus tard, sans que le mot de passe en texte clair ne s'affiche. Exemple :  
  
```powershell  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> Il n'est pas recommandé de fournir ni de stocker un mot de passe en texte clair ou obfusqué. Toute personne qui exécute cette commande dans un script ou qui regarde par-dessus votre épaule connaît le mot de passe DSRM de ce contrôleur de domaine. Toute personne ayant accès au fichier peut annuler ce mot de passe obfusqué. Munie de cette information, elle peut ouvrir une session sur un contrôleur de domaine en mode DSRM et finir par emprunter l'identité du contrôleur de domaine lui-même, en élevant ses privilèges au niveau le plus élevé d'une forêt Active Directory. Une autre procédure utilisant **System.Security.Cryptography** pour chiffrer les données du fichier texte est conseillée, mais n'est pas traitée ici. Le mieux est d'éviter totalement tout stockage de mot de passe.  
  
L'applet de commande ADDSDeployment offre une autre option permettant d'ignorer la configuration automatique des paramètres des clients DNS, des redirecteurs et des indications de racine. Vous ne pouvez pas ignorer cette option de configuration quand vous utilisez le Gestionnaire de serveur. Cet argument n'est important que si vous avez installé le rôle Serveur DNS avant de configurer le contrôleur de domaine :  
  
```powershell  
-SkipAutoConfigureDNS  
```  
  
Le fonctionnement de **DomainNetBIOSName** est également spécial :  
  
-   Si l'argument **DomainNetBIOSName** n'est pas spécifié avec un nom de domaine NetBIOS et que le nom de domaine avec préfixe en une partie dans l'argument **DomainName** contient au maximum 15 caractères, la promotion continue avec un nom généré automatiquement.  
  
-   Si l'argument **DomainNetBIOSName** n'est pas spécifié avec un nom de domaine NetBIOS et que le nom de domaine avec préfixe en une partie dans l'argument **DomainName** contient au minimum 16 caractères, la promotion échoue.  
  
-   Si l'argument **DomainNetBIOSName** est spécifié avec un nom de domaine NetBIOS de 15 caractères au maximum, la promotion continue avec ce nom spécifié.  
  
-   Si l'argument **DomainNetBIOSName** est spécifié avec un nom de domaine NetBIOS de 16 caractères au minimum, la promotion échoue.  
  
L'argument de l'applet de commande ADDSDeployment Options supplémentaires du Gestionnaire de serveur équivalent est le suivant :  
  
```powershell  
-domainnetbiosname <string>  
```  
  
Les arguments de l'applet de commande ADDSDeployment **Chemins d'accès** du Gestionnaire de serveur équivalents sont les suivants :  
  
```powershell  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
  
```  
  
Utilisez l'argument **Whatif** facultatif avec l'applet de commande **Install-ADDSForest** pour passer en revue les informations de configuration. Cela vous permet de voir les valeurs explicites et implicites des arguments d'une applet de commande.  
  
Exemple :  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSPaths.png)  
  
Vous ne pouvez pas ignorer la fonctionnalité **Vérification de la configuration requise** quand vous utilisez le Gestionnaire de serveur, mais vous pouvez ignorer le processus quand vous utilisez l'applet de commande de déploiement des services AD DS avec l'argument suivant :  
  
```powershell  
-skipprechecks  
```  
  
> [!WARNING]  
> Microsoft déconseille d'ignorer la vérification de la configuration requise, car cela peut aboutir à une promotion partielle du contrôleur de domaine ou à l'endommagement de la forêt AD DS.  
  
Notez comment, de la même façon que le Gestionnaire de serveur, **Install-ADDSForest** vous rappelle que la promotion redémarre automatiquement le serveur.  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSReboot.png)  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallProgress.png)  
  
Pour accepter automatiquement l'invite de redémarrage, utilisez les arguments **-force** ou **-confirm:$false** avec n'importe quelle applet de commande Windows PowerShell ADDSDeployment. Pour empêcher le serveur de redémarrer automatiquement à la fin de la promotion, utilisez l'argument **-norebootoncompletion**.  
  
> [!WARNING]  
> Il n'est pas conseillé de remplacer le redémarrage. Le contrôleur de domaine doit redémarrer pour fonctionner correctement.  
  
## <a name="see-also"></a>Voir aussi  
[Services de domaine Active Directory (portail TechNet)](https://technet.microsoft.com/library/cc770946(WS.10).aspx)  
[Services de domaine Active Directory pour Windows Server 2008 R2](https://technet.microsoft.com/library/dd378801(WS.10).aspx)  
[Services de domaine Active Directory pour Windows Server 2008](https://technet.microsoft.com/library/dd378891(WS.10).aspx)  
[Référence technique de Windows Server (Windows Server 2003)](https://technet.microsoft.com/library/cc739127(WS.10).aspx)  
[Centre d’administration Active Directory : Prise en main (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd560651(WS.10).aspx)  
[Administration d’Active Directory avec Windows PowerShell (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd378937(WS.10).aspx)  
[Demandez à the Directory Services Team (Blog du Support technique officielle Microsoft Commercial)](http://blogs.technet.com/b/askds)  
  

