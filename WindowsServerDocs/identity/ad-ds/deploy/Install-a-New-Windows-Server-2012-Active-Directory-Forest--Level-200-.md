---
ms.assetid: b3d6fb87-c4d4-451c-b3de-a53d2402d295
title: "Installer une nouvelle forêt d’ActiveDirectory de Windows Server2012 (niveau 200)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b8a7502a1b9d27b0f61353f2544182a64d311496
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="install-a-new-windows-server-2012-active-directory-forest-level-200"></a>Installer une nouvelle forêt d’ActiveDirectory de Windows Server2012 (niveau 200)

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique décrit la fonctionnalité du contrôleur de la promotion de Services de domaine ActiveDirectory Windows Server2012 domaine nouveau au niveau de l’introduction. Dans Windows Server2012, les services ADDS remplace l’outil Dcpromo par un gestionnaire de serveur et du système de déploiement basé sur Windows PowerShell.  
  
-   [Administration simplifiée des Services de domaine ActiveDirectory](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SimplifiedAdmin)  
  
-   [Vue d’ensemble technique](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_TechOverview)  
  
-   [Déploiement d’une forêt avec le Gestionnaire de serveur](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SMForest)  
  
-   [Déploiement d’une forêt avec Windows PowerShell](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_PSForest)  
  
## <a name="BKMK_SimplifiedAdmin"></a>Administration simplifiée des Services de domaine ActiveDirectory  
Windows Server2012 introduit la nouvelle génération d’ActiveDirectory domaine Administration simplifiée des Services et est la plus radicale domaine nouveau considération depuis Windows2000 Server. Administration simplifiée ADDS tire ses enseignements de douze années d’ActiveDirectory et rend une expérience d’administration plus intuitive plus pris en charge, plus souple pour les architectes et administrateurs. Cela revient à créer de nouvelles versions des technologies existantes ainsi qu’à étendre les fonctionnalités des composants fournis dans Windows Server2008R2.  
  
### <a name="what-is-ad-ds-simplified-administration"></a>Nouveautés des services ADDS l’Administration simplifiée?  
Administration simplifiée ADDS réinvente le déploiement de domaine. Certaines de ces fonctionnalités sont les suivantes:  
  
-   Déploiement de rôle ADDS fait maintenant partie de la nouvelle architecture de gestionnaire de serveur et permet l’installation à distance.  
  
-   Le moteur de déploiement et la configuration des services ADDS est maintenant Windows PowerShell, même si vous utilisez un programme d’installation graphique.  
  
-   La promotion inclut maintenant la vérification requise qui valide la disponibilité de la forêt et du domaine pour le nouveau contrôleur de domaine, ce qui réduit le risque d’échec des promotions.  
  
-   La forêt de Windows Server2012 fonctionnel niveau n’implémente pas de nouvelles fonctionnalités et le niveau fonctionnel du domaine est requis uniquement pour un sous-ensemble de nouvelles fonctionnalités Kerberos, ce qui décharge les administrateurs de fréquent besoin d’un environnement de contrôleur de domaine homogène.  
  
### <a name="purpose-and-benefits"></a>Objectif et avantages  
Ces modifications peuvent sembler plus complexes et non plus simples. Dans la nouvelle conception du processus de déploiement des services ADDS, cependant, il était possible de regrouper de nombreuses étapes et meilleures pratiques en moins d’actions plus faciles. Par exemple, cela signifie que la configuration graphique d’un nouveau contrôleur de domaine répliqué est désormais huit boîtes de dialogue plutôt que les douze. La création d’une nouvelle forêt ActiveDirectory nécessite un *unique* commande Windows PowerShell avec uniquement *un* argument: le nom du domaine.  
  
Pourquoi est-il accorder une telle importance à Windows PowerShell dans Windows Server2012? Comme les traitements distribués, Windows PowerShell permet un moteur unique pour la configuration et de maintenance à partir des graphiques et interfaces de ligne de commande. Il permet de script complet de tout composant avec le même offrant pour les professionnels de l’informatique qui accorde à une API pour les développeurs. Basé sur le cloud computing seront omniprésence, Windows PowerShell met également enfin la possibilité pour administrer à distance un serveur sur lequel un ordinateur sans interface graphique a les mêmes fonctionnalités de gestion qu’un autre avec un moniteur et une souris.  
  
Un administrateur de services ADDS expérimenté devrait s’afficher ses connaissances acquises hautement pertinentes. Un administrateur débutant trouve une courbe d’apprentissage beaucoup plus plates.  
  
## <a name="BKMK_TechOverview"></a>Vue d’ensemble technique  
  
### <a name="what-you-should-know-before-you-begin"></a>Ce que vous devez savoir avant de commencer  
Cette rubrique suppose que vous connaissez les versions précédentes des Services de domaine ActiveDirectory et ne fournit pas de détails élémentaires sur leurs objectifs ni leurs fonctionnalités. Pour plus d’informations sur les services ADDS, voir les pages du portail TechNet liées ci-dessous:  
  
-   [Services de domaine ActiveDirectory pour Windows Server2008R2](https://technet.microsoft.com/library/dd378801(WS.10).aspx)  
  
-   [Services de domaine ActiveDirectory pour Windows Server2008](https://technet.microsoft.com/library/dd378891(WS.10).aspx)  
  
-   [Référence technique de Windows Server](https://technet.microsoft.com/library/cc739127(WS.10).aspx)  
  
### <a name="functional-descriptions"></a>Descriptions fonctionnelles  
  
#### <a name="ad-ds-role-installation"></a>Installation du rôle de service d’annuaire ActiveDirectory  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_SelectServerRoles.gif)  
  
L’installation des Services de domaine ActiveDirectory utilise le Gestionnaire de serveur et Windows PowerShell, comme tous les autres rôles et fonctionnalités dans Windows Server2012. Le programme Dcpromo.exe ne fournit plus options de configuration d’interface graphique utilisateur.  
  
Vous utilisez un Assistant graphique dans le Gestionnaire de serveur ou le module ServerManager pour Windows PowerShell dans les installations locales et distantes. En exécutant plusieurs instances de ces Assistants ou les applets de commande et en ciblant différents serveurs, vous pouvez déployer les services ADDS sur plusieurs contrôleurs de domaine simultanément, à partir d’une console unique. Bien que ces nouvelles fonctionnalités ne sont pas descendante avec Windows Server2008R2 ou des systèmes d’exploitation antérieurs, vous pouvez toujours utiliser l’application Dism.exe introduite dans Windows Server2008R2 pour l’installation du rôle en local à partir de la ligne de commande classique.  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSAddWindowsFeature.png)  
  
#### <a name="ad-ds-role-configuration"></a>Configuration du rôle de service d’annuaire ActiveDirectory  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_DeploymentConfiguration_Forest.gif)  
  
Configuration de Services de domaine ActiveDirectory «auparavant appelée DCPROMO» est maintenant une opération discrète effectuée à partir de l’installation du rôle. Après avoir installé le rôle ADDS, un administrateur configure le serveur comme un contrôleur de domaine à l’aide d’un Assistant distinct dans le Gestionnaire de serveur ou le module WindowsPowerShellADDSDeployment.  
  
Configuration de rôle ADDS s’appuie sur douze ans d’expérience sur le terrain et configure maintenant les contrôleurs de domaine basés sur les meilleures pratiques de Microsoft la plus récentes. Par exemple, le système de nom de domaine et les catalogues globaux installent par défaut sur chaque contrôleur de domaine.  
  
L’Assistant de configuration du Gestionnaire de serveur ADDS fusionne de nombreuses boîtes de dialogue individuelles en moins d’invites et ne masque plus les paramètres dans un mode «avancé». Le processus de promotion entier est dans une boîte de dialogue taille lors de l’installation. L’Assistant et le module WindowsPowerShellADDSDeployment vous affichent les modifications importantes et les questions de sécurité, ainsi que des liens vers des informations supplémentaires.  
  
Dcpromo.exe demeure dans Windows Server2012 pour les installations sans assistance de ligne de commande uniquement et ne s’exécute plus l’Assistant installation graphique. Il est recommandé que vous ne plus utiliser Dcpromo.exe pour les installations sans assistance et le remplacez par le module ADDSDeployment, comme l’exécutable maintenant déconseillé ne pas être inclus dans la prochaine version de Windows.  
  
Ces nouvelles fonctionnalités ne sont pas la compatibilité descendante avec Windows Server2008R2 ou des systèmes d’exploitation plus anciens.  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDSForest.png)  
  
> [!IMPORTANT]  
> Dcpromo.exe ne contient plus d’un Assistant graphique et n’installe plus de fichiers binaires de rôle ou une fonctionnalité. Essayez d’exécuter Dcpromo.exe à partir de la renvoie shell Explorer:  
>   
> «L’Assistant Installation des Services de domaine ActiveDirectory a été déplacé dans le Gestionnaire de serveur. Pour plus d’informations, voir https://go.microsoft.com/fwlink/?LinkId=220921.»  
>   
> Une tentative d’exécution de Dcpromo.exe//unattend toujours installe les fichiers binaires, comme dans les systèmes d’exploitation précédents, mais vous avertit:  
>   
> «Le dcpromo opération sans assistance est remplacée par le module ADDSDeployment pour Windows PowerShell. Pour plus d’informations, voir https://go.microsoft.com/fwlink/?LinkId=220924.»  
>   
> Windows Server2012 déconseille dcpromo.exe et ne sera pas inclus dans les futures versions de Windows, et il recevra autres améliorations dans ce système d’exploitation. Les administrateurs doivent arrêter son utilisation et basculez en les modules Windows PowerShell pris en charge s’ils souhaitent créer des contrôleurs de domaine à partir de la ligne de commande.  
  
#### <a name="prerequisite-checking"></a>La vérification de la configuration requise  
Configuration du contrôleur de domaine implémente également une phase de vérification de la configuration requise qui évalue la forêt et le domaine avant de poursuivre la promotion du contrôleur de domaine. Cela inclut la disponibilité du rôle FSMO, des privilèges d’utilisateur, la compatibilité du schéma étendu et autres exigences. Cette nouvelle conception atténue les problèmes où la promotion du contrôleur de domaine démarre et puis s’arrête à mi-chemin avec une erreur irrécupérable de configuration. Cela réduit le risque de métadonnées du contrôleur de domaine orphelines dans la forêt ou un serveur qui croit à tort qu’il est un contrôleur de domaine.  
  
## <a name="BKMK_SMForest"></a>Déploiement d’une forêt avec le Gestionnaire de serveur  
Cette section explique comment installer le premier contrôleur de domaine dans un domaine racine de forêt à l’aide du Gestionnaire de serveur sur un ordinateur Windows Server2012 graphique.  
  
### <a name="server-manager-ad-ds-role-installation-process"></a>Processus d’Installation de rôle Gestionnaire de serveur ADDS  
Le diagramme ci-dessous illustre le processus d’installation du rôle de Services de domaine ActiveDirectory commence quand vous exécutez ServerManager.exe et se terminant directement avant la promotion du contrôleur de domaine.  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_servermanagerdeployment.png)  
  
#### <a name="server-pool-and-add-roles"></a>Serveur du Pool et ajout de rôles  
Tous les ordinateurs Windows Server2012accessibles à partir de l’ordinateur exécutant le Gestionnaire de serveur sont éligibles pour le regroupement. Une fois mis en pool, vous sélectionnez ces serveurs pour une installation à distance des services ADDS ou toute autre option de configuration possible dans le Gestionnaire de serveur.  
  
Pour ajouter des serveurs, choisissez l’une des opérations suivantes:  
  
-   Cliquez sur **ajouter d’autres serveurs à gérer** sur la vignette accueil du tableau de bord  
  
-   Cliquez sur le **gérer** et sélectionnez **ajouter des serveurs**  
  
-   Avec le bouton droit **tous les serveurs** et choisissez **ajouter des serveurs**  
  
Ceci fait apparaître la boîte de dialogue Ajouter des serveurs:  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddServers.png)  
  
Cela vous donne de trois façons d’ajouter des serveurs au pool pour utiliser ou les regrouper:  
  
-   Recherche ActiveDirectory (utilise LDAP, requiert que les ordinateurs appartiennent à un domaine, autorise le filtrage du système d’exploitation et prend en charge les caractères génériques)  
  
-   Recherche DNS (utilise l’alias DNS ou l’adresse IP via une diffusion ARP ou NetBIOS ou une recherche WINS, n’autorise pas les caractères génériques filtrage ou de la prise en charge de système d’exploitation)  
  
-   Importation (utilise une liste de fichiers texte des serveurs séparés par CR/LF)  
  
Cliquez sur **Rechercher maintenant** pour retourner une liste des serveurs à partir de ce même domaine ActiveDirectory que l’ordinateur est joint à, cliquez sur un ou plusieurs noms de serveur à partir de la liste des serveurs. Cliquez sur la flèche droite pour ajouter les serveurs à la **sélectionnés** liste. Utilisez le **ajouter des serveurs** boîte de dialogue pour ajouter des serveurs sélectionnés aux groupes de rôles du tableau de bord. Ou cliquez sur **gérer**, puis cliquez sur **créer un groupe de serveur**, ou cliquez sur **créer un groupe de serveur** du tableau de bord **Bienvenue dans Gestionnaire de serveur** pour créer des groupes de serveurs personnalisés.  
  
> [!NOTE]  
> La procédure d’ajout de serveurs ne valide pas qu’un serveur est en ligne ou accessible. Toutefois, tous les serveurs inaccessibles indicateur dans l’affichage de la facilité de gestion dans le Gestionnaire de serveur à la prochaine actualisation  
  
Vous pouvez installer des rôles à distance sur n’importe quel ordinateur Windows Server2012ordinateurs ajouté au pool, comme indiqué:  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/tADDS_SMI_TR_AddRolesFeatures.png)  
  
Vous ne pouvez pas gérer totalement les serveurs qui exécutent des systèmes d’exploitation antérieurs à Windows Server2012. Le **Ajout de rôles et fonctionnalités** sélection est en cours d’exécution Module WindowsPowerShellServerManager **Install-WindowsFeature**.  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddADDSToAnotherServer.png)  
  
Vous pouvez également utiliser le tableau de bord du Gestionnaire de serveur sur un contrôleur de domaine existant pour sélectionner l’installation de serveur distant ADDS avec le rôle déjà présélectionné en cliquant avec le bouton droit sur la vignette du tableau de bord les services ADDS et en sélectionnant **ajouter les services ADDS à un autre serveur**. Cela revient à appeler **Install-WindowsFeature AD-Domain-Services**.  
  
L’ordinateur que vous utilisez le Gestionnaire de serveur se met en pool automatiquement. Pour installer le rôle ADDS ici, cliquez simplement sur le **gérer** menu, cliquez sur **Ajout de rôles et fonctionnalités**.  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ManageAddRoles.png)  
  
#### <a name="installation-type"></a>Type d’installation  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectInstallationType.png)  
  
Le **Type d’Installation** boîte de dialogue fournit une option qui ne prend pas en charge les Services de domaine ActiveDirectory: le **scénario en fonction des Services Bureau à distance-installation**. Cette option autorise uniquement le Service Bureau à distance dans une charge de travail distribuée multiserveur. Si vous l’activez, les services ADDS ne peut pas installer.  
  
Laissez toujours la sélection par défaut en place lors de l’installation des services ADDS: **Installation basée sur un rôle ou une fonctionnalité**.  
  
#### <a name="server-selection"></a>Sélection du serveur  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectDestinationServer.png)  
  
Le **sélection du serveur** boîte de dialogue vous permet de choisir parmi un des serveurs précédemment ajoutés au pool, tant qu’il est accessible. Le serveur local exécutant le Gestionnaire de serveur est automatiquement disponible.  
  
En outre, vous pouvez sélectionner les fichiers de disque dur virtuel Hyper-V hors connexion avec le système d’exploitation Windows Server2012 et le Gestionnaire de serveur ajoute le rôle leur directement par le biais de maintenance des composants. Cela permet d’approvisionner les serveurs virtuels avec les composants nécessaires avant de les configurer davantage.  
  
#### <a name="server-roles-and-features"></a>Fonctionnalités et rôles de serveur  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectServerRoles.png)  
  
Sélectionnez le **les Services de domaine ActiveDirectory** rôle si vous envisagez de promouvoir un contrôleur de domaine. Toutes les fonctionnalités d’administration ActiveDirectory et les services requis installent automatiquement, même s’ils font soi-disant partie d’un autre rôle ou n’apparaissent pas sélectionnés dans l’interface du Gestionnaire de serveur.  
  
Le Gestionnaire de serveur affiche également une boîte de dialogue d’information qui montre les fonctionnalités de gestion de ce rôle installe implicitement; cela est équivalent à la **- IncludeManagementTools** argument.  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddFeaturesDialog.gif)  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_SelectFeatures.png)  
  
Autres **fonctionnalités** peuvent être ajoutées ici si vous le souhaitez.  
  
#### <a name="active-directory-domain-services"></a>Services de domaine ActiveDirectory  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ADDSIntro.png)  
  
Le **les Services de domaine ActiveDirectory** boîte de dialogue fournit des informations limitées sur la configuration requise et meilleures pratiques. Il agit principalement comme une confirmation que vous avez choisi le rôle ADDS «Si cet écran n’apparaît pas, vous n’avez pas sélectionné ADDS.  
  
#### <a name="confirmation"></a>Confirmation  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Confirmation.png)  
  
Le **Confirmation** boîte de dialogue est le point de contrôle final avant le démarrage de l’installation du rôle. Il offre une option pour redémarrer l’ordinateur si nécessaire après l’installation du rôle, mais installation ADDS ne nécessite pas un redémarrage.  
  
En cliquant sur **installer**, vous confirmez que vous êtes prêt à commencer l’installation du rôle. Vous ne pouvez pas annuler une installation de rôle une fois qu’il commence.  
  
#### <a name="results"></a>Résultats  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Results.png)  
  
Le **résultats** boîte de dialogue affiche la progression et l’état actuel de l’installation. Installation du rôle continue que si le Gestionnaire de serveur est fermé.  
  
Vérification des résultats de l’installation est toujours une meilleure pratique. Si vous fermez le **résultats** boîte de dialogue avant la fin de l’installation, vous pouvez vérifier les résultats à l’aide de l’indicateur de notification du Gestionnaire de serveur. Gestionnaire de serveur affiche également un message d’avertissement pour tous les serveurs qui ont installé le rôle ADDS, mais ne pas été davantage configurés en tant que contrôleurs de domaine.  
  
**Notifications de tâche**  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_TaskNotofications.png)  
  
**Détails ADDS**  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ADDSDetails.png)  
  
**Détails de la tâche**  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_TaskDetails.png)  
  
#### <a name="promote-to-domain-controller"></a>Promouvoir en contrôleur de domaine  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_Promote.png)  
  
À la fin de l’installation du rôle ADDS, vous pouvez poursuivre la configuration à l’aide de la **promouvoir ce serveur en contrôleur de domaine** lien. Cela est nécessaire pour que le serveur à un contrôleur de domaine, mais n’est pas nécessaire d’exécuter l’Assistant configuration immédiatement. Par exemple, vous voudrez uniquement approvisionner les serveurs avec les fichiers binaires ADDS avant de les envoyer à une autre filiale pour une configuration ultérieure. En ajoutant le rôle ADDS avant la livraison, vous gagnez du temps quand il atteint sa destination. Vous suivez également la meilleure pratique de ne pas garder un contrôleur de domaine hors connexion pendant des jours ou semaines. Enfin, cela vous permet de mettre à jour des composants avant la promotion du contrôleur de domaine, qui vous épargne au moins un redémarrage ultérieur.  
  
Sélection de ce lien plus tard appelle les applets de commande ADDSDeployment: **install-addsforest**, **install-addsdomain**, ou **install-addsdomaincontroller**.  
  
### <a name="uninstallingdisabling"></a>Désactivation/de désinstallation  
Vous supprimez le rôle ADDS comme n’importe quel autre rôle, quelle que soit l’indique si le serveur à un contrôleur de domaine a été promu. Toutefois, la suppression du rôle ADDS nécessite un redémarrage à la fin.  
  
Suppression du rôle Services de domaine ActiveDirectory est différente de l’installation, elle nécessite une rétrogradation du contrôleur de domaine avant de pouvoir terminer. Cela est nécessaire pour empêcher un contrôleur de domaine d’avoir des fichiers binaires de rôle désinstallés sans un nettoyage des métadonnées approprié dans la forêt. Pour plus d’informations, voir [la rétrogradation des contrôleurs de domaine et les domaines et #40; niveau 200 et #41;](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
> [!WARNING]  
> Suppression de rôles ADDS avec Dism.exe ou le module DISM Windows PowerShell après que la promotion du contrôleur de domaine n’est pas prise en charge et empêche le serveur de démarrer normalement.  
>   
> Contrairement aux Gestionnaire de serveur ou le module de déploiement des services ADDS pour Windows PowerShell, DISM est un système de maintenance natif qui dispose d’aucune connaissance inhérente des services ADDS ou sa configuration. N’utilisez pas Dism.exe ou le module DISM Windows PowerShell pour désinstaller le rôle ADDS, sauf si le serveur n’est plus un contrôleur de domaine.  
  
### <a name="create-an-ad-ds-forest-root-domain-with-server-manager"></a>Créer un domaine racine de forêt ADDS avec le Gestionnaire de serveur  
Le diagramme suivant illustre le processus de configuration des Services de domaine ActiveDirectory, dans le cas où vous avez précédemment installé le rôle ADDS et démarré le **Assistant Configuration de Services de domaine ActiveDirectory** à l’aide du Gestionnaire de serveur.  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_forestdeploy2.png)  
  
#### <a name="deployment-configuration"></a>Configuration de déploiement  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_AddNewForest.png)  
  
Le Gestionnaire de serveur commence chaque promotion du contrôleur de domaine avec le **Configuration de déploiement** page. Les options disponibles et les champs obligatoires modifier sur cette page et les pages suivantes, en fonction de l’opération de déploiement que vous sélectionnez.  
  
Pour créer une nouvelle forêt ActiveDirectory, cliquez sur **ajouter une nouvelle forêt**. Vous devez fournir un nom de domaine racine valide; le nom ne peut pas être une partie (par exemple, le nom doit être *contoso.com* ou similaire et pas seulement *contoso*) et doivent utiliser autorisées exigences de noms de domaine DNS.  
  
Pour plus d’informations sur les noms de domaine valides, voir l’article [conventions de dénomination dans ActiveDirectory pour les ordinateurs, domaines, sites et unités d’organisation](https://support.microsoft.com/kb/909264).  
  
> [!WARNING]  
> Ne créez pas de forêts ActiveDirectory avec le même nom qu’un nom DNS externe. Par exemple, si votre URL DNS Internet est http://contoso.com, vous devez choisir un autre nom pour votre forêt interne éviter les problèmes de compatibilité future. Ce nom doit être unique et peu de chances de trafic web. Par exemple: corp.contoso.com.  
  
Une nouvelle forêt ne nécessite pas de nouvelles informations d’identification pour le compte d’administrateur du domaine. Le processus de promotion de contrôleur de domaine utilise les informations d’identification du compte administrateur intégré à partir du premier contrôleur de domaine utilisé pour créer la racine de forêt. Il n’existe aucun moyen (par défaut) pour désactiver ou verrouiller le compte administrateur intégré et il peut être le seul point d’entrée dans une forêt si les autres comptes d’administration de domaine sont inutilisables. Il est essentiel de connaître le mot de passe avant de déployer une nouvelle forêt.  
  
**DomainName** nécessite un nom DNS de domaine complet valide et est requis.  
  
#### <a name="domain-controller-options"></a>Options du contrôleur de domaine  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_DCOptions_Forest.gif)  
  
Le **Options du contrôleur de domaine** vous permet de configurer le **niveau fonctionnel de forêt** et **niveau fonctionnel du domaine** le nouveau domaine racine de forêt. Par défaut, ces paramètres sont Windows Server2012, un nouveau domaine racine de forêt. Le niveau fonctionnel de forêt Windows Server2012 ne fournit pas de nouvelles fonctionnalités sur le niveau fonctionnel de forêt Windows Server2008R2. Le niveau fonctionnel du domaine Windows Server2012 est requis que pour implémenter les nouveaux paramètres Kerberos «toujours fournir des revendications» et «Rejeter les demandes d’authentification non blindées». Utilisation principale des niveaux fonctionnels dans Windows Server2012 consiste à limiter la participation dans les domaine aux contrôleurs de domaine qui répondent aux exigences minimales des systèmes d’exploitation. En d’autres termes, vous pouvez spécifier de Windows Server2012 domaine fonctionnel niveau uniquement contrôleurs de domaine qui exécutent Windows Server2012 peuvent héberger le domaine.  Windows Server2012 implémente un nouvel indicateur de contrôleur de domaine appelé **DS_WIN8_REQUIRED** dans les **DSGetDcName** fonction de NetLogon qui localise exclusivement les contrôleurs de domaine Windows Server2012. Cela vous permet de la souplesse d’une forêt plus homogène ou hétérogène qui les systèmes d’exploitation sont autorisés à exécuter sur les contrôleurs de domaine.  
  
Pour plus d’informations sur le contrôleur de domaine emplacement, passez en revue [fonctions du Service d’annuaire](https://msdn.microsoft.com/library/ms675900(VS.85).aspx).  
  
La fonction de contrôleur de domaine configurable uniquement est l’option de serveur DNS. Microsoft recommande que tous les contrôleurs de domaine fournissent des services DNS pour la haute disponibilité dans les environnements distribués, c’est pourquoi cette option est sélectionnée par défaut lors de l’installation d’un contrôleur de domaine dans n’importe quel mode ou un domaine. Le catalogue Global et en lecture seule options du contrôleur de domaine ne sont pas disponibles lors de la création d’un domaine racine de forêt; le premier contrôleur de domaine doit être un catalogue global et ne peut pas être un contrôleur de domaine seule (RODC) en lecture.  
  
Spécifié **passe en Mode restauration des Services Directory** doit respecter la stratégie de mot de passe appliquée au serveur, qui ne nécessite pas un mot de passe fort; par défaut uniquement une non vide. Choisissez toujours un mot de passe fort complexe ou, de préférence, une phrase secrète.  
  
#### <a name="dns-options-and-dns-delegation-credentials"></a>Options DNS et les informations d’identification de délégation DNS  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestDNSOptions.png)  
  
Le **Options DNS** page vous permet de configurer la délégation DNS et de fournir d’autres informations d’identification d’administration DNS.  
  
Vous ne pouvez pas configurer la délégation ni les options DNS dans l’Assistant Configuration des Services de domaine ActiveDirectory lors de l’installation d’un nouveau domaine ActiveDirectory forêt racine où vous avez sélectionné le **serveur DNS** sur le **Options du contrôleur de domaine** page. Le **créer une délégation DNS** option est disponible lorsque vous créez une zone DNS de forêt racine dans une infrastructure de serveur DNS. Cette option vous permet de fournir autre DNS informations d’identification administratives qui disposent des droits pour mettre à jour de zone DNS.  
  
Pour plus d’informations sur la nécessité de créer une délégation DNS, voir [présentation la délégation de Zone](https://technet.microsoft.com/library/cc771640.aspx).  
  
#### <a name="additional-options"></a>Options supplémentaires  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestAdditionalOptions.png)  
  
Le **Options supplémentaires** page affiche le nom NetBIOS du domaine et vous permet de remplacer. Par défaut, le nom de domaine NetBIOS correspond à l’étiquette la plus à gauche du nom de domaine complet fourni sur le **Configuration de déploiement** page. Par exemple, si vous avez indiqué le nom de domaine complet de corp.contoso.com, le nom de domaine NetBIOS par défaut est CORP.  
  
Si le nom est de 15caractères ou moins et n’est pas en conflit avec un autre nom NetBIOS, il est modifié. Si elle n’est en conflit avec un autre nom NetBIOS, un numéro est ajouté au nom. Si le nom est plus de 15caractères, l’Assistant fournit une suggestion tronquée unique. Dans les deux cas, l’Assistant vérifie tout d’abord le nom n’est pas déjà en cours d’utilisation via une recherche WINS et une diffusion NetBIOS.  
  
Pour plus d’informations sur les noms de domaine valides, voir l’article [conventions de dénomination dans ActiveDirectory pour les ordinateurs, domaines, sites et unités d’organisation](https://support.microsoft.com/kb/909264).  
  
#### <a name="paths"></a>Chemins d’accès  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestPaths.png)  
  
Le **chemins d’accès** page vous permet de remplacer les emplacements de dossier par défaut de la base de données ADDS, les journaux des transactions de base de données, et du partage SYSVOL. Les emplacements par défaut sont toujours dans des sous-répertoires de %SystemRoot% (autrement dit, C:\Windows).  
  
#### <a name="review-options-and-view-script"></a>Examiner les Options et afficher le Script  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestReviewOptions.png)  
  
Le **examiner les Options** page vous permet de valider vos paramètres et assurez-vous qu’ils répondent à vos besoins avant de commencer l’installation. Cela n’est pas la possibilité d’arrêter l’installation lors de l’utilisation du Gestionnaire de serveur. Il s’agit simplement d’une option pour confirmer vos paramètres avant de poursuivre la configuration  
  
Le **examiner les Options** page dans le Gestionnaire de serveur offre également une option **afficher le Script** bouton pour créer un fichier texte Unicode qui contient la configuration ADDSDeployment actuelle sous forme de script Windows PowerShell unique. Cela vous permet d’utiliser l’interface graphique du Gestionnaire de serveur comme un studio de déploiement de Windows PowerShell. Utilisez l’Assistant Configuration des Services de domaine ActiveDirectory pour configurer les options, exportez la configuration et annulez ensuite l’Assistant. Ce processus crée un exemple valide et la syntaxe correct pour modification supplémentaire ou une utilisation directe. Par exemple:  
  
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
> Le Gestionnaire de serveur renseigne généralement tous les arguments avec des valeurs pendant la promotion et ne s’appuie pas sur les valeurs par défaut (car elles peuvent changer entre les versions futures de Windows ou des service packs). La seule exception est le **- safemodeadministratorpassword** argument (qui est délibérément omis du script). Pour forcer une demande de confirmation, omettez la valeur en cas d’exécution interactive de l’applet de commande.  
  
#### <a name="prerequisites-check"></a>Vérification des conditions préalables  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestPrereqCheck.png)  
  
Le **vérification des conditions requises** est une nouvelle fonctionnalité de configuration du domaine ADDS. Cette nouvelle phase valide que la configuration du serveur est capable de prendre en charge une nouvelle forêt ADDS.  
  
Lorsque vous installez un domaine racine de forêt, l’Assistant Gestionnaire de serveur ActiveDirectory domaine Services Configuration appelle une série de tests modulaires. Ces tests vous alerte avec les options de réparation suggérés. Vous pouvez exécuter les tests autant de fois que nécessaire. Le processus de contrôleur de domaine ne peut pas continuer tant que la configuration requise de tous les tests passer.  
  
Le **vérification des conditions requises** exposent également des informations pertinentes, telles que les modifications de sécurité qui affectent les anciens systèmes d’exploitation.  
  
Pour plus d’informations sur les vérifications spécifiques de la configuration requise, voir [Prerequisite Checking](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking).  
  
#### <a name="installation"></a>Installation  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestInstallation.png)  
  
Lorsque le **Installation** page s’affiche, la configuration de contrôleur de domaine commence et ne peut pas être interrompue ou annulée. Les opérations détaillées s’affichent sur cette page et sont écrites dans les journaux:  
  
-   %Systemroot%\Debug\Dcpromo.log  
  
-   %SystemRoot%\debug\dcpromoui.log  
  
> [!NOTE]  
> Vous pouvez exécuter plusieurs installation de rôle et des Assistants de configuration ADDS à partir de la même console du Gestionnaire de serveur simultanément.  
  
#### <a name="results"></a>Résultats  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
Le **résultats** page indique la réussite ou l’échec de la promotion et toute information d’administration importante. Le contrôleur de domaine redémarre automatiquement après 10secondes.  
  
## <a name="BKMK_PSForest"></a>Déploiement d’une forêt avec Windows PowerShell  
Cette section explique comment installer le premier contrôleur de domaine dans un domaine racine de forêt à l’aide de Windows PowerShell sur un ordinateur de base Windows Server2012.  
  
### <a name="windows-powershell-ad-ds-role-installation-process"></a>Processus d’Installation de WindowsPowerShellADDS rôle  
En implémentant quelques applets de déploiement ServerManager simples dans vos processus de déploiement, vous Concrétisez davantage que la vision des services ADDS l’administration simplifiée.  
  
La figure suivante illustre le processus d’installation du rôle de Services de domaine ActiveDirectory commence quand vous exécutez **PowerShell.exe** et de fin directement avant la promotion du contrôleur de domaine.  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/adds_servermanagerdeployment_powershell.png)  
  
|||  
|-|-|  
|Applet de commande ServerManager|Arguments (**gras** arguments sont requis. *En italique* arguments peuvent être spécifiés à l’aide de Windows PowerShell ou l’Assistant de Configuration ADDS.)|  
|Install-WindowsFeature/Add-WindowsFeature|***-Nom***<br /><br />*-Redémarrage*<br /><br />*-IncludeAllSubFeature*<br /><br />*-IncludeManagementTools*<br /><br />-Source<br /><br />*-ComputerName*<br /><br />-Credential<br /><br />-LogPath<br /><br />*-Disque dur virtuel*<br /><br />*-ConfigurationFilePath*|  
  
> [!NOTE]  
> Temps pas nécessaire, l’argument **- IncludeManagementTools** est vivement recommandé lorsque vous installez les binaires du rôle ADDS  
  
Le module ServerManager expose les parties installation, l’état et suppression de rôle du nouveau module DISM pour Windows PowerShell. Cette disposition simplifie la plupart des tâches et réduit la nécessité pour une utilisation directe du puissant (mais dangereux en cas de l’utilisation incorrecte) module DISM.  
  
Utilisez **Get-Command** pour exporter les alias et les applets de commande dans ServerManager.  
  
```powershell  
Get-Command -module ServerManager  
```  
  
Par exemple:  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSGetCommand.png)  
  
Pour ajouter le rôle Services de domaine ActiveDirectory, il suffit d’exécuter le **Install-WindowsFeature** avec le nom de rôle ADDS en tant qu’argument. Comme le Gestionnaire de serveur, tous les services requis implicites du rôle ADDS installer automatiquement.  
  
```powershell  
Install-WindowsFeature -name AD-Domain-Services  
```  
  
Si vous voulez également que les outils de gestion de domaine ActiveDirectory installés, et il est fortement recommandé - puis fournir le **- IncludeManagementTools** argument:  
  
```powershell  
Install-WindowsFeature -name AD-Domain-Services -IncludeManagementTools  
```  
  
Par exemple:  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallWinFeature.png)  
  
Pour répertorier toutes les fonctionnalités et rôles avec leur état d’installation, utilisez **Get-WindowsFeature** sans arguments. Spécifiez **- ComputerName** argument pour l’état d’installation à partir d’un serveur distant.  
  
```powershell  
Get-WindowsFeature  
```  
  
Étant donné que **Get-WindowsFeature** n’a pas de filtrage mécanisme, vous devez utiliser **Where-Object** avec un pipeline pour trouver des fonctionnalités spécifiques. Le pipeline est un canal utilisé entre plusieurs applets de commande pour transmettre des données et l’applet de commande Where-Object agit comme un filtre. Intégré **$_** variable agissant en tant que l’objet actuel qui traverse le pipeline avec toutes les propriétés qu’il contient.  
  
```powershell  
Get-WindowsFeature | where-object <options>  
```  
  
Par exemple, pour retrouver toutes les fonctionnalités contenant «Active Dir» dans leur **nom d’affichage** propriété, utilisez:  
  
```powershell  
Get-WindowsFeature | where displayname -like "*active dir*"  
```  
  
Autres exemples sont illustrés ci-dessous:  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSGetWindowsFeature.png)  
  
Pour plus d’informations sur d’autres opérations Windows PowerShell avec des pipelines et Where-Object, voir [et utilisation des pipelines dans Windows PowerShell](https://technet.microsoft.com/library/ee176927.aspx).  
  
Notez également que WindowsPowerShell3.0 a considérablement simplifié les arguments de ligne de commande nécessaires dans cette opération de pipeline. WindowsPowerShell2.0 aurait requis:  
  
```powershell  
Get-WindowsFeature | where {$_.displayname - like "*active dir*"}  
```  
  
En utilisant le pipeline Windows PowerShell, vous pouvez créer des résultats lisibles. Par exemple:  
  
```powershell  
Install-WindowsFeature | Format-List  
Install-WindowsFeature | select-object | Format-List  
  
```  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDS.png)  
  
Notez comment l’utilisation du **Select-Object** applet de commande avec le **- expandproperty** argument renvoie des données intéressantes:  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallADDSWithTools.png)  
  
> [!NOTE]  
> Le **Select-Object - expandproperty** argument ralentit légèrement les performances globales de l’installation.  
  
### <a name="BKMK_PS"></a>Créer un domaine racine de forêt ADDS avec Windows PowerShell  
Pour installer une nouvelle forêt ActiveDirectory à l’aide du module ADDSDeployment, utilisez l’applet de commande suivante:  
  
```powershell  
Install-addsforest  
```  
  
Le **Install-AddsForest** applet de commande comprend uniquement deux phases (vérification de la configuration requise et installation). Les deux figures ci-dessous illustrent la phase d’installation avec l’argument requis minimal de **- domainname**.  
  
|||  
|-|-|  
|Applet de commande ADDSDeployment|Arguments (**gras** arguments sont requis. *En italique* arguments peuvent être spécifiés à l’aide de Windows PowerShell ou l’Assistant de Configuration ADDS.)|  
|Install-Addsforest|-Confirmer<br /><br />*-CreateDNSDelegation*<br /><br />*-DatabasePath*<br /><br />*DomainMode-*<br /><br />***-DomainName***<br /><br />***-DomainNetBIOSName***<br /><br />*-DNSDelegationCredential*<br /><br />*-ForestMode*<br /><br />-Force<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />-NoDnsOnNetwork<br /><br />-NoRebootOnCompletion<br /><br />*-SafeModeAdministratorPassword*<br /><br />-SkipAutoConfigureDNS<br /><br />-SkipPreChecks<br /><br />*-SYSVOLPath*<br /><br />*-Whatif*|  
  
> [!NOTE]  
> Le **- DomainNetBIOSName** argument est requis si vous souhaitez modifier le nom de 15caractères automatiquement généré basé sur le préfixe de nom de domaine DNS ou si le nom dépasse 15caractères.  
  
Le Gestionnaire de serveur équivalents **Configuration de déploiement** applet de commande ADDSDeployment et les arguments sont:  
  
```powershell  
Install-ADDSForest  
-DomainName <string>  
```  
  
Les arguments d’applet de commande ADDSDeployment Options du contrôleur de domaine Server Manager équivalents sont:  
  
```powershell  
-ForestMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-DomainMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-InstallDNS <{$false | $true}>  
-SafeModeAdministratorPassword <secure string>  
  
```  
  
Le **Install-ADDSForest** arguments suivent les mêmes paramètres par défaut en tant que gestionnaire de serveur, le cas contraire.  
  
Le **SafeModeAdministratorPassword** de l’argument est spécial:  
  
-   Si *ne pas spécifié* en tant qu’argument, l’applet de commande vous invite à entrer et à confirmer un mot de passe masqué. Il s’agit du mode d’utilisation préféré cas d’exécution interactive de l’applet de commande.  
  
    Par exemple, pour créer une nouvelle forêt nommée corp.contoso.com et être invité à entrer et à confirmer un mot de passe masqué:  
  
    ```powershell  
    Install-ADDSForest "DomainName corp.contoso.com  
    ```  
  
-   Si spécifié *avec une valeur*, la valeur doit être une chaîne sécurisée. Cela n’est pas le mode d’utilisation préféré cas d’exécution interactive de l’applet de commande.  
  
Par exemple, vous pouvez demander manuellement un mot de passe à l’aide de la **Read-Host** applet de commande pour inviter l’utilisateur d’une chaîne sécurisée:  
  
```powershell  
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)  
```  
  
> [!WARNING]  
> Comme l’option précédente ne confirme pas le mot de passe, faites preuve de prudence: le mot de passe n’est pas visible.  
  
Vous pouvez également fournir une chaîne sécurisée en tant qu’une variable en texte clair convertie, bien que ceci soit fortement déconseillé.  
  
```powershell  
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
```  
  
Enfin, vous pouvez stocker le mot de passe obscurci dans un fichier et réutiliser plus tard, sans le mot de passe ne s’affiche. Par exemple:  
  
```powershell  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> Fournir ou de stocker un mot de passe de texte clair ou obfusqué n’est pas recommandée. Toute personne qui exécute cette commande dans un script ou qui regarde par-dessus votre épaule connaît le mot de passe DSRM de ce contrôleur de domaine. Toute personne ayant accès au fichier peut annuler ce mot de passe obfusqué. Avec cette information, ils peuvent se connecter à un contrôleur de domaine démarré en mode DSRM et finir par emprunter l’identité du contrôleur de domaine lui-même, en élevant ses privilèges au niveau plus élevé d’une forêt ActiveDirectory. Un ensemble d’étapes à l’aide supplémentaire **System.Security.Cryptography** pour chiffrer le fichier texte données sont conseillée, mais hors de portée. La meilleure pratique consiste à éviter totalement tout stockage de mot de passe.  
  
L’applet de commande ADDSDeployment offre une autre option pour ignorer la configuration automatique des paramètres de client DNS, des redirecteurs et des indications de racine. Vous ne pouvez pas ignorer cette option de configuration lorsque vous utilisez le Gestionnaire de serveur. Cet argument n’est important que si vous avez installé le rôle serveur DNS avant de configurer le contrôleur de domaine:  
  
```powershell  
-SkipAutoConfigureDNS  
```  
  
Le **DomainNetBIOSName** opération est également spéciale:  
  
-   Si le **DomainNetBIOSName** argument n’est pas spécifié avec un nom de domaine NetBIOS et le nom de domaine de préfixe en une partie dans le **DomainName** argument est de 15caractères ou moins, puis la promotion continue avec un nom généré automatiquement.  
  
-   Si le **DomainNetBIOSName** argument n’est pas spécifié avec un nom de domaine NetBIOS et le nom de domaine de préfixe en une partie dans le **DomainName** argument est de 16caractères ou plus, puis la promotion échoue.  
  
-   Si le **DomainNetBIOSName** argument est spécifié avec un nom de domaine NetBIOS de 15caractères au maximum, puis la promotion continue avec ce nom spécifié.  
  
-   Si le **DomainNetBIOSName** argument est spécifié avec un nom de domaine NetBIOS de 16caractères ou plus, puis la promotion échoue.  
  
L’argument d’applet de commande ADDSDeployment Options supplémentaires de gestionnaire de serveur équivalent est:  
  
```powershell  
-domainnetbiosname <string>  
```  
  
Le Gestionnaire de serveur équivalents **chemins d’accès** sont des arguments de l’applet de commande ADDSDeployment:  
  
```powershell  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
  
```  
  
Utilisez l’option **Whatif** argument avec la **Install-ADDSForest** applet de commande pour passer en revue les informations de configuration. Cela vous permet de voir les valeurs explicites et implicites des arguments d’une applet de commande.  
  
Par exemple:  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSPaths.png)  
  
Vous ne pouvez pas ignorer le **vérification** lorsque à l’aide du Gestionnaire de serveur, mais vous pouvez ignorer le processus lors de l’utilisation de l’applet de commande de déploiement des services ADDS à l’aide de l’argument suivant:  
  
```powershell  
-skipprechecks  
```  
  
> [!WARNING]  
> Microsoft déconseille d’ignorer la vérification en tant qu’il peut entraîner une promotion du contrôleur de domaine partielle ou endommagée forêt ADDS.  
  
Notez comment, comme le Gestionnaire de serveur, **Install-ADDSForest** vous rappelle que la promotion redémarre automatiquement le serveur.  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSReboot.png)  
  
![Installer une nouvelle forêt](media/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-/ADDS_PSInstallProgress.png)  
  
Pour accepter automatiquement l’invite de redémarrage, utilisez la **-force** ou **-confirmer: $false** arguments avec une applet de commande WindowsPowerShellADDSDeployment. Pour empêcher le serveur de redémarrer automatiquement à la fin de la promotion, utilisez le **- norebootoncompletion** argument.  
  
> [!WARNING]  
> Il est déconseillé de remplacer le redémarrage. Le contrôleur de domaine doit redémarrer pour fonctionner correctement.  
  
## <a name="see-also"></a>Voir aussi  
[Services de domaine ActiveDirectory (portail TechNet)](https://technet.microsoft.com/library/cc770946(WS.10).aspx)  
[Services de domaine ActiveDirectory pour Windows Server2008R2](https://technet.microsoft.com/library/dd378801(WS.10).aspx)  
[Services de domaine ActiveDirectory pour Windows Server2008](https://technet.microsoft.com/library/dd378891(WS.10).aspx)  
[Référence technique de Windows Server (Windows Server2003)](https://technet.microsoft.com/library/cc739127(WS.10).aspx)  
[Centre d’administration ActiveDirectory: Prise en main (Windows Server2008R2)](https://technet.microsoft.com/library/dd560651(WS.10).aspx)  
[Administration d’ActiveDirectory avec Windows PowerShell (Windows Server2008R2)](https://technet.microsoft.com/library/dd378937(WS.10).aspx)  
[Demandez à l’équipe de Services d’annuaire (Blog du Support technique Microsoft officiel Commercial)](http://blogs.technet.com/b/askds)  
  

