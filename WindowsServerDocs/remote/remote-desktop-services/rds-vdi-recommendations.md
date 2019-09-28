---
title: Configuration recommandée pour les postes de travail VDI
description: Paramètres et configuration recommandés pour réduire la charge pour les postes de travail Windows 10 1607 (10.0.1393) utilisés comme images VDI
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 12/18/2018
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2a44dc9f-c221-4bf7-89c3-fb4c86a90f8c
author: jaimeo
manager: dougkim
ms.openlocfilehash: 9e2c4012184614826ffd762394d89c25acabf374
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403866"
---
# <a name="recommended-settings-for-vdi-desktops"></a>Paramètres recommandés pour les postes de travail VDI

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows 10

Microsoft Desktop Virtualization accélère la préparation des utilisateurs en détectant automatiquement les configurations de périphériques et les conditions du réseau et en activant le programme d’installation instantanée des applications d’entreprise et des postes de travail. En outre, il permet aux équipes informatiques de fournir un accès aux applications héritées pendant la migration vers Windows 10.

Bien que le réglage de base du système d’exploitation Windows 10 soit très efficace, vous pouvez l’affiner pour l’environnement Microsoft Virtual Desktop Infrastructure (VDI) de votre entreprise. Dans l’environnement VDI, plusieurs services et tâches d’arrière-plan et désactivés par défaut.

Cette rubrique n’est pas un modèle à suivre à la lettre, mais plutôt un repère ou un point de départ. Certaines recommandations peuvent entraîner la désactivation des fonctionnalités que vous souhaitez utiliser. Nous vous recommandons donc d’envisager les avantages et les désavantages de l’ajustement de vos paramètres selon votre situation.

Ces instructions et paramètres recommandés sont pertinents pour Windows 10 1607 (version 10.0.1393).

> [!NOTE]  
> Tous les paramètres qui ne sont pas spécifiquement mentionnés dans cette rubrique peuvent être laissés à leurs valeurs par défaut (ou définis selon vos besoins et stratégies). Cela n’aura aucun impact notable sur les fonctionnalités de l’infrastructure VDI.

Lorsque vous créez une image pour le déploiement du VDI, veillez à utiliser l’option **Current Branch**. Si vous souhaitez en savoir plus sur l’option Current Branch, veuillez consulter l’article [Informations sur les publications pour Windows 10](https://technet.microsoft.com/windows/release-info.aspx).

## <a name="creating-the-windows-10-image"></a>Création de l’image Windows 10
La première étape consiste à installer une image de référence de Windows 10 1607 (version 10.0.1393) sur un ordinateur virtuel ou physique. L’installation sur un ordinateur virtuel est facile et vous permet d’enregistrer les versions du fichier de disque dur virtuel (VHD), au cas où vous souhaitez restaurer une version antérieure.

Pendant l’installation, vous pouvez choisir **Configuration rapide** ou **Personnaliser**. Les paramètres accessibles via l’option **Personnaliser** sont réglables en utilisant une stratégie de groupe. Par conséquent, la méthode d’installation du système d’exploitation de base n’est pas particulièrement importante.


Si vous avez choisi **Personnaliser**, vous pouvez ajuster ces paramètres lors de l’installation :

## <a name="in-customize-settings"></a>Dans « Personnaliser les paramètres »

Vous pouvez également les ajuster après l’installation, en utilisant l’éditeur de stratégie de groupe. Pour cela, veuillez consulter la section « Paramètres de stratégie de groupe » de cette rubrique.

|Paramètre|Valeur par défaut|Valeur recommandée pour une utilisation du VDI|  
|-------------------|----------|--------------|
|**Personnalisation**| | |
|Personnaliser la voix, la frappe et l’entrée manuscrite en envoyant vos données d’entrée à Microsoft.|    Activé| Désactivé|
|Envoyer des données de saisie clavier et manuscrite à Microsoft pour améliorer la plateforme de reconnaissance et de suggestion.|  Activé| Désactivé|
|Autorisez les applications à utiliser votre identifiant de publicité.|  Activé| Désactivé|
|Autorisez Skype (s’il est installé) à vous aider à vous connecter aux amis de votre carnet d’adresses et à vérifier votre numéro de téléphone mobile. Des frais d’envoi de SMS et de données peuvent s’appliquer.|    Activé| Désactivé|
|**Emplacement**| | |
|Activez la fonctionnalité Localiser mon appareil et laissez Windows et les applications vous localiser en demandant notamment l’historique des emplacements| Activé| Désactivé|
|Connectivité et rapport d’erreurs| | |
|Se connecter automatiquement, selon les suggestions fournies, aux points d'accès ouverts. Tous les réseaux ne sont pas sécurisés.|    Activé| Désactivé|
|Connectez-vous automatiquement et temporairement aux points d’accès afin de vérifier si des services réseau payants sont disponibles.| Activé| Désactivé|
|Envoyez des rapports de diagnostics et d’utilisation complets à Microsoft. La désactivation de cette option envoie uniquement des données de base.| Activé| Désactivé|
|**Navigateur, protection et mise à jour**| | |
|Utiliser les services en ligne SmartScreen pour favoriser la protection contre le contenu et les téléchargements malveillants présents sur des sites chargés par les navigateurs Windows et les applications issues du Windows Store|    Activé| Activé (Désactivé s’il n’existe aucun accès à Internet)
|Utiliser la prédiction de page pour améliorer la lecture, accélérer la navigation et optimiser votre expérience dans les navigateurs Windows. Vos données de navigation seront envoyées à Microsoft.| Activé| Désactivé|
|Obtenez des mises à jour et envoyez-en sur les PC connectés à Internet afin d’accélérer les téléchargements des applications et des mises à jour Windows Update|   Activé| Désactivé|

Une fois l’installation terminée, vous pouvez continuer à ajuster les paramètres en commençant par **Paramètres Windows**.

## <a name="in-windows-settings"></a>Dans Paramètres Windows
Pour accéder à la fenêtre Paramètres Windows, cliquez sur **Démarrer** (l’icône Windows sur la barre des tâches), puis sur l’icône **Paramètres** (en forme d’engrenage).

### <a name="in-the-system-area-of-windows-settings"></a>Dans la zone « Système » de la fenêtre Paramètres de Windows
Dans la fenêtre Paramètres Windows, cliquez sur l’icône **Système** pour accéder aux paramètres relatifs au système. Certains d'entre eux doivent être ajustés pour optimiser l’utilisation de VDI. Voici les plus importants :

#### <a name="apps-and-features"></a>Applications et fonctionnalités

Pour supprimer une application, en l’excluant ainsi à partir de votre image de VDI, cliquez sur l’application, puis cliquez sur **Désinstaller**. Si **Désinstaller** apparaît en grisé, cela veut dire que vous ne pouvez pas supprimer l’application ainsi. Vous pourrez probablement la supprimer avec Windows PowerShell, ou en essayant la procédure suivante :
1. Cliquez sur **Gérer les fonctionnalités facultatives** (cette option est située juste en dessous du titre **Applications et fonctionnalités** sur la même page).
2. Cliquez sur la fonctionnalité facultative de votre choix, puis sur **Désinstaller**.

Nous vous recommandons de supprimer les fonctionnalités suivantes :
- **Contacter le support technique**
- **Contenu du mode démo pour l’anglais (États-Unis)**
- **Contenu du mode démo neutre**
- **Assistance rapide**

#### <a name="default-apps"></a>Applications par défaut

Cette zone définit l’application à utiliser par défaut pour certaines fonctionnalités génériques telles que le courrier électronique, la navigation sur le web et les cartes. Si vous souhaitez utiliser une autre application pour une fonctionnalité particulière, cliquez sur l’entrée en cours, puis sur l’application que vous préférez utiliser dans l’image VDI. Si vous souhaitez utiliser une application tierce (non fournie par Microsoft), vous devez l’installer avant de régler ce paramètre.

#### <a name="notifications-and-actions"></a>Notifications et actions

Ces valeurs recommandées réduisent le nombre de notifications et l’activité du réseau en arrière-plan dans un environnement VDI :

|Paramètre|Valeur par défaut|Valeur recommandée pour une utilisation de VDI|  
|-------------------|----------|--------------|
|Obtenir les notifications des applications et des autres expéditeurs| Activé| Désactivé|
|Afficher les notifications dans l’écran de verrouillage.|    Activé| Désactivé|
|Afficher les alarmes, rappels et appels VoIP entrants sur l’écran de verrouillage.|   Activé| Désactivé|
|Afficher des conseils, astuces et suggestions lorsque vous utilisez Windows.|    Activé| Désactivé|


#### <a name="offline-maps"></a>Cartes hors connexion

Ce paramètre est uniquement applicable si l’application Cartes est installée. Sa valeur par défaut est **Activé**. Nous vous recommandons de le définir sur **Désactivé** si vous utilisez un VDI. 

#### <a name="tablet-mode"></a>Mode Tablette

|Paramètre|Valeur par défaut|Valeur recommandée pour une utilisation de VDI|  
|-------------------|----------|--------------|
|Lorsque je me connecte|    Utiliser le mode approprié à mon matériel|   Utiliser le mode bureau|
|Lorsque cet appareil active ou désactive automatiquement le mode|    Toujours me demander confirmation avant de changer de mode| Ne pas me demander et ne pas changer de mode|
|Masquer les icônes d’application sur la barre des tâches en mode tablette|  Activé| Désactivé|


### <a name="in-the-devices-area-of-windows-settings"></a>Dans la zone « Appareils » de la page Paramètres Windows
Dans la page Paramètres Windows, cliquez sur l’icône **Appareils** pour accéder à plusieurs paramètres relatifs au système. Certains d'entre eux doivent être ajustés pour optimiser l’utilisation de VDI. Voici les plus importants :

#### <a name="autoplay"></a>Exécution automatique

|Paramètre|Valeur par défaut|Valeur recommandée pour une utilisation de VDI|  
|-------------------|----------|--------------|
|Utiliser l’exécution automatique pour tous les médias et tous les périphériques|    Activé| Désactivé|
|Lecteur amovible :|Choisir un paramètre par défaut|Ne rien faire|
|Carte mémoire|Choisir un paramètre par défaut|Ne rien faire|

### <a name="in-the-personalization-area-of-windows-settings"></a>Dans la zone « Personnalisation » de la page Paramètres Windows
Dans la page Paramètres Windows, cliquez sur l’icône **Personnalisation** pour accéder à plusieurs paramètres relatifs au système. Certains d'entre eux doivent être ajustés pour optimiser l’utilisation de VDI. Voici les plus importants :

#### <a name="background"></a>Arrière-plan
Parfois, un arrière-plan noir par défaut inciter les utilisateurs à croire que l’ordinateur ne répond plus. En modifiant la couleur d’arrière-plan, vous pouvez éviter ce genre de méprise. Pour cela, procédez comme suit:
1. Dans la zone **arrière-plan**, cliquez sur le menu déroulant.
2. Pour modifier la couleur d’arrière-plan, cliquez sur **Couleur unie**, puis sur une autre couleur que le noir. Vous pouvez également cliquer sur **Image**, puis sélectionner une image à utiliser comme arrière-plan.

#### <a name="start"></a>Début

|Paramètre|Valeur par défaut|Valeur recommandée pour une utilisation de VDI|  
|-------------------|----------|--------------|
|Afficher occasionnellement les suggestions dans l’écran d’accueil|    Activé| Désactivé|
|Afficher les applications les plus utilisées|Activé|Désactivé|
|Afficher les applications récemment ajoutées|Activé|Désactivé|
|Afficher les éléments récemment ouverts dans les Listes de raccourcis de l’écran d’accueil ou la barre des tâches|Activé|Désactivé|

#### <a name="taskbar"></a>Barre des tâches
Le paramètre par défaut consiste à utiliser les boutons de barre des tâches de grande taille (autrement dit, la valeur « Désactivé » pour **Utiliser des petits boutons de barre des tâches**). Ce paramètre entraîne l’élément Cortana à utiliser une grande partie de la barre des tâches. Pour éviter ce problème, définissez **Utiliser des petits boutons de barre des tâches** sur « Activé ». Si vous préférez que les éléments de la barre des tâches restent plus grands, mais souhaitez que Cortana n’occupe pas beaucoup d’espace, faites un clic droit sur la barre des tâches, pointez sur **Cortana**, puis dans le menu qui s’affiche, sélectionnez **Masquée**.

### <a name="in-the-privacy-area-of-windows-settings"></a>Dans la zone « Confidentialité » de la page Paramètres Windows
Dans la page Paramètres Windows, cliquez sur l’icône **Confidentialité** pour accéder à plusieurs paramètres relatifs au système. Certains d'entre eux doivent être ajustés pour optimiser l’utilisation de VDI. Voici les plus importants :

#### <a name="general"></a>Général
Certains de ces paramètres sont également définis dans la fenêtre « Personnaliser les paramètres », décrite au début de cette rubrique.

|Paramètre|Valeur par défaut|Valeur recommandée pour une utilisation de VDI|  
|-------------------|----------|--------------|
|Laisser les applications utiliser mon identifiant de publicité (la désactivation de cette option réinitialise votre identifiant)|  Activé| Désactivé|
|Laissez les sites Web accéder à ma liste de langues pour fournir du contenu local|Activé|Désactivé|
|Autoriser les applications installées sur vos autres appareils à exécuter des applications et à poursuivre les expériences entamées sur cet appareil|Activé|Désactivé|

#### <a name="camera"></a>Appareil photo

La valeur par défaut pour « Autoriser les applications à utiliser ma caméra » est **Activé**. Nous vous recommandons de le définir sur **Désactivé** si vous utilisez un VDI.


#### <a name="microphone"></a>Microphone

La valeur par défaut pour « Autoriser les applications à utiliser mon microphone » est **Activé**. Nous vous recommandons de le définir sur **Désactivé** si vous utilisez un VDI.

#### <a name="notifications"></a>Notifications

La valeur par défaut pour « Autoriser les applications à accéder à mes notifications » est **Activé**. Nous vous recommandons de le définir sur **Désactivé** si vous utilisez un VDI.

#### <a name="contacts"></a>Contacts

La valeur par défaut pour « Autoriser les applications à accéder à mes contacts » est **Activé**. Nous vous recommandons de le définir sur **Désactivé** si vous utilisez un VDI.

#### <a name="calendar"></a>Calendrier

La valeur par défaut pour « Autoriser les applications à accéder à mon calendrier » est **Activé**. Nous vous recommandons de le définir sur **Désactivé** si vous utilisez un VDI.

#### <a name="call-history"></a>Historique des appels

La valeur par défaut pour « Autoriser les applications à accéder à mon historique des appels » est **Activé**. Nous vous recommandons de le définir sur **Désactivé** si vous utilisez un VDI.

#### <a name="email"></a>E-mail

La valeur par défaut pour « Autoriser les applications à accéder à et à envoyer un courrier électronique » est **Activé**. Nous vous recommandons de le définir sur **Désactivé** si vous utilisez un VDI.

#### <a name="messaging"></a>Messagerie

La valeur par défaut pour « Autoriser les applications à lire ou envoyer des messages (SMS ou MMS) » est **Activé**. Nous vous recommandons de le définir sur **Désactivé** si vous utilisez un VDI.

#### <a name="radios"></a>Radios

La valeur par défaut pour « Laisser les applications contrôler les radios » est **Activé**. Nous vous recommandons de le définir sur **Désactivé** si vous utilisez un VDI.

#### <a name="other-devices"></a>Autres appareils

La valeur par défaut pour « Permettre à vos applications de partager et de synchroniser automatiquement des informations avec des périphériques sans fil non couplés explicitement avec votre PC, votre tablette ou votre téléphone » est **Activé**. La valeur recommandée est **Désactivé** si vous utilisez un VDI.

#### <a name="feedback-and-diagnostics"></a>Commentaires et diagnostics

La valeur par défaut pour « Windows demande à recevoir mes commentaires » est **Automatiquement**. Nous vous recommandons de le définir sur **Jamais** si vous utilisez un VDI.

#### <a name="background-apps"></a>Applications en arrière-plan
Les applications répertoriées ici sont définies par défaut sur **Activé**, ce qui leur permet de recevoir des informations, d’envoyer des notifications et de se mettre à jour automatiquement, qu’elles soient utilisées ou non. Nous vous recommandons de désactiver (en définissant le paramètre sur **Désactivé**) toutes les applications qui ne doivent pas être exécutées en arrière-plan dans votre image VDI.

### <a name="update-and-security"></a>Mise à jour et sécurité
#### <a name="windows-update"></a>Windows Update
Dans la zone **Mettre à jour les paramètres**, cliquez sur **Options avancées** pour ajuster ces paramètres :

|Paramètre|Valeur par défaut|Valeur recommandée pour une utilisation de VDI|  
|-------------------|----------|--------------|
|Me communiquer les mises à jour d'autres produits Microsoft lorsque je mets à jour Windows|    désactivé|    activé|
|Différer les mises à jour de fonctionnalité|désactivé|activé|
|Utiliser mes infos de connexion pour terminer automatiquement la configuration de mon appareil après une mise à jour |désactivé|Dépend de la configuration spécifique du VDI|

Dans la page **Options avancées**, cliquez sur **Choisir le mode de distribution des mises à jour** pour accéder au paramètre « Mises à jour provenant de plusieurs emplacements ». Sa valeur par défaut est **Activé**. Nous vous recommandons de le définir sur **Désactivé** si vous utilisez un VDI.

## <a name="in-control-panel-and-other-system-utilities"></a>Dans le Panneau de configuration et d’autres utilitaires système

Vous pouvez régler les paramètres présentés dans cette section en y accédant via le Panneau de configuration ou en ouvrant l’utilitaire directement.

> [!NOTE]  
> Tous les paramètres qui ne sont pas spécifiquement mentionnés dans cette rubrique peuvent être laissés à leurs valeurs par défaut (ou définis selon vos besoins et stratégies). Cela n’aura aucun impact notable sur les fonctionnalités de l’infrastructure VDI.


### <a name="task-scheduler"></a>Planificateur de tâches
Le moyen le plus rapide pour ouvrir le Planificateur de tâches consiste à appuyer sur la touche Windows, puis à taper *Planificateur de tâches* ou *taskschd.msc*. Dans les résultats affichés, cliquez sur **Planificateur de tâches** pour ouvrir l’utilitaire. Dans le Planificateur de tâches, développez successivement **Bibliothèque du Planificateur de tâches**, **Microsoft**, puis **Windows**. Vous avez désormais accès à la liste des collections de tâches. Pour modifier le statut d’une tâche planifiée, faites un clic droit dessus, puis cliquez sur le statut de votre choix (en règle générale, **Désactivé** si vous utilisez un VDI).

|Collection de tâches|Nom de la tâche|État par défaut|Statut recommandé si vous utilisez un VDI|  
|-------------------|-------------|----------|--------------|
|Programme d’amélioration du produit||||
||Consolidator|Activé|Désactivée|
||KernelCeipTask|Activé|Désactivée|
||UsbCeip|Activé|Désactivée|
|Defrag||||
||ScheduledDefrag|Activé|Désactivée|
|Emplacement||||
||Notifications|Activé|Désactivée|
||WindowsActionDialog|Activé|Désactivée|
|Maintenance||||
||WinSAT|Activé|Désactivée|
|Cartes||||
||MapsToastTask|Activé|Désactivée|
||MapsUpdateTask|Activé|Désactivée|
|Compte haut débit mobile||||
||Analyseur de métadonnées MNO|Activé|Désactivée|
|Diagnostics de l’efficacité énergétique||||
||Analyze System|Activé|Désactivée|
|Environnement de récupération||||
||VerifyWinRE|Activé|Désactivée|
|Démonstration commerciale||||
||CleanupOfflineContent|Activé|Désactivée|
|Shell||||
||FamilySafetyMonitor|Activé|Désactivée|
||FamilySafetyRefreshTask|Activé|Désactivée|
|Rapport d’erreurs Windows||||
||QueueReporting|Activé|Désactivée|
|Partage de fichiers multimédias Windows||||
||UpdateLibrary|Activé|Désactivée|

Cliquez sur **Windows** à nouveau pour réduire ce niveau, puis cliquez sur **XblGameSave**. Cela vous permet d’accéder aux tâches **XBLGameSaveTask** et **XBLGameSaveTaskLogon**. Vous pouvez définir leurs statuts sur **Désactivé**.

### <a name="performance-monitor"></a>Analyseur de performances
Le moyen le plus rapide pour ouvrir l’Analyseur de performances consiste à appuyer sur la touche Windows, puis de taper *Analyseur de performances* ou *perfmon.msc*. Dans les résultats affichés, cliquez sur **Analyseur de performances**. Dans l’Analyseur de performances, cliquez sur **Ensembles de collecteurs de données**, puis double-cliquez sur **Sessions de suivi d’événements**. Faites un clic droit sur **WiFiSession**. Si son statut est défini sur **En cours d’exécution**, cliquez sur **Arrêter**.

Cliquez sur **StartupEventTraceSessions**, puis faites un clic droit sur **ReadyBoot**. Si son statut est défini sur En cours d’exécution, cliquez sur **Arrêter**. Cliquez sur **Sessions de suivi d’événements**, faites un clic droit sur **ReadyBoot**, puis cliquez sur **Propriétés**. Dans la boîte de dialogue qui s’ouvre, cliquez sur l’onglet **Session de suivi**. Décochez la case en regard d’**Activé**.

### <a name="services"></a>Services
Le moyen le plus rapide pour gérer les Services consiste à appuyer sur la touche Windows, puis à taper *services*. Dans les résultats affichés, cliquez sur **Services**. Nous vous recommandons de désactiver les services suivants si vous utilisez un VDI. Toutefois, vous devrez peut-être effectuer des tests pour vous assurer qu’ils ne vous sont pas nécessaires. Pour désactiver un service, dans le composant logiciel enfichable **Services**, cliquez sur le nom du service à désactiver, puis sur **Propriétés**. Sous l’onglet **Général**, cliquez sur le menu déroulant en regard de **Type de démarrage**, puis sur **Désactivé**. Cliquez sur **OK**.

- BranchCache
- Optimisation de la distribution
- Service hôte WDIServiceHost
- Service Point d'accès sans fil mobile Windows
- Gestionnaire d'authentification Xbox Live
- Jeu sauvegardé sur Xbox Live
- Service de mise en réseau Xbox Live

### <a name="file-explorer-options"></a>Options de l'Explorateur de fichiers
Appuyez sur la touche Windows, puis tapez *Panneau de configuration*. Dans les résultats affichés, cliquez sur **le Panneau de configuration**. Dans le Panneau de configuration, cliquez sur **Options de l’Explorateur de fichiers**. Dans la boîte de dialogue qui s’ouvre, cliquez sur l’onglet **Rechercher**, puis dans la zone **Lors des recherches sur des emplacements non indexés**, décochez la case en regard de **Inclure les répertoires système**. Cliquez sur **OK** pour enregistrer.

### <a name="flash-settings"></a>Paramètres Flash
Appuyez sur la touche Windows, puis tapez *Panneau de configuration*. Dans les résultats affichés, cliquez sur **le Panneau de configuration**. Dans le panneau de configuration, cliquez sur **Flash Player** pour ouvrir le Gestionnaire des paramètres de Flash Player. Sous l’onglet **Enregistrement**, sélectionnez l’option **Empêcher tous les sites d’enregistrer des informations sur cet ordinateur**. Dans la boîte de dialogue qui s’ouvre, cliquez sur **OK**.

Sous l’onglet **Caméra et microphone**, dans la zone **Paramètres de la caméra et du microphone**, sélectionnez l’option **Empêcher tous les sites d’utiliser la caméra et le microphone**.

Sous l’onglet **Lecture**, dans la zone **Mise en réseau coopérative**, sélectionnez l’option **Empêcher tous les sites de recourir à la mise en réseau coopérative**. Fermez le Gestionnaire des paramètres de Flash Player.

### <a name="internet-options"></a>Options Internet
Appuyez sur la touche Windows, puis tapez *Panneau de configuration*. Dans les résultats affichés, cliquez sur **le Panneau de configuration**. Dans le Panneau de configuration, cliquez sur **Options Internet** pour ouvrir les propriétés d’Internet. Dans la zone **Page de démarrage**, entrez l’URL du site web que vous voulez que les utilisateurs voient en tant que page d’accueil dans leurs navigateurs. Cela peut être un site web de votre entreprise, ou une page d’accueil vide, en entrant *about:blank*.

Dans la zone **Historique de navigation**, cochez la case en regard de **Supprimer l’historique de navigation en quittant le navigateur**.

### <a name="power-options"></a>Options d’alimentation
Appuyez sur la touche Windows, puis tapez *Panneau de configuration*. Dans les résultats affichés, cliquez sur **le Panneau de configuration**. Dans le panneau de configuration, cliquez sur **Options d’alimentation** pour ouvrir la fenêtre Options d’alimentation. Dans la zone **Choisir ou personnaliser un mode de gestion de l’alimentation**, cliquez sur la flèche vers le bas pour **Afficher les modes supplémentaires**, puis sélectionnez l’option **Performances élevées**. Ce paramètre a très peu d’impact sur l’hôte VDI.

### <a name="system"></a>System
Appuyez sur la touche Windows, puis tapez *Panneau de configuration*. Dans les résultats affichés, cliquez sur **le Panneau de configuration**. Dans le panneau de configuration, cliquez sur **Système** pour ouvrir la fenêtre Système. Dans le volet de gauche, cliquez sur **Paramètres système avancés**. Dans la boîte de dialogue qui s’ouvre, cliquez sur l’onglet **Avancé**. Dans la zone **Performances**, cliquez sur **Paramètres**. Puis sous l’onglet **Effets visuels**, sélectionnez l’option **Ajuster afin d’obtenir les meilleures performances**. Cliquez sur **OK** pour enregistrer et quitter.

## <a name="group-policy-settings"></a>Paramètres de stratégie de groupe

Pour modifier les paramètres Stratégie de groupe, appuyez la touche Windows, puis tapez *Stratégie de groupe* ou *gpedit.msc*. Dans les résultats affichés, cliquez sur **Modifier la stratégie de groupe** pour ouvrir l’Éditeur d'objets de stratégie de groupe.

> [!NOTE]  
> Tous les paramètres qui ne sont pas spécifiquement mentionnés dans cette rubrique peuvent être laissés à leurs valeurs par défaut (ou définis selon vos besoins et stratégies). Cela n’aura aucun impact notable sur les fonctionnalités de l’infrastructure VDI.

Sous **Configuration ordinateur**, développez **Paramètres Windows**, puis **Paramètres de sécurité**. Cliquez sur **Stratégies du gestionnaire de listes de réseaux**, puis double-cliquez sur **Tous les réseaux**. Dans la boîte de dialogue qui s’ouvre, dans la zone **Emplacement réseau**, sélectionnez l’option **L’utilisateur ne peut pas changer l’emplacement**. Cliquez sur **OK** pour enregistrer.

Réduisez le niveau **Paramètres Windows**, puis développez **Modèles d’administration**. Cliquez sur **Réseau** ou développez ce niveau, puis ajustez chaque paramètre comme suit en double-cliquant dessus, puis en sélectionnant l’option correspondant à la valeur indiquée ici en cliquant sur **OK** :

|Zone Paramètre|Paramètre|Valeur recommandée pour une utilisation de VDI|  
|-------------------|-------|----------|
|Service de transfert intelligent en arrière-plan (BITS)|||
||Ne pas autoriser le client BITS à utiliser le cache de filiale Windows|Activé|
||Ne pas autoriser l’utilisation de l’ordinateur en tant que client de mise en cache partagé entre systèmes homologues BITS|Activé|
||Ne pas autoriser l’utilisation de l’ordinateur en tant que serveur de mise en cache partagé entre systèmes homologues BITS|Activé|
||Autoriser la mise en cache partagé entre systèmes homologues BITS|Désactivée|
|BranchCache||
||Activer BranchCache|Désactivée|
|Authentification de la zone d’accès sans fil||
||Activer l’authentification de la zone d’accès sans fil|Désactivée|
|Activer les services réseau pair à pair Microsoft||
||Désactiver les services réseau pair à pair Microsoft|Activé|
|Fichiers hors connexion||
||Autoriser ou interdire l’utilisation de la fonctionnalité de fichiers hors connexion|Désactivée|

Réduisez le niveau **Réseau**, puis développez le niveau **Système**. Ajustez chaque paramètre comme suit en double-cliquant dessus, puis en sélectionnant l’option correspondant à la valeur indiquée ici en cliquant sur **OK** :

|Zone Paramètre|Paramètre|Valeur recommandée pour une utilisation de VDI|  
|-------------------|----------|--------------|
|Installation de périphériques||
||Ne pas envoyer de rapport d’erreurs Windows lors de l’installation d’un pilote générique sur un périphérique|Activé|
||Empêcher la création de point de restauration système lors d’une activité d’un périphérique demandant habituellement la création d’un point de restauration|Activé|
||Empêcher la récupération des métadonnées de périphérique depuis Internet|Activé|
||Empêcher Windows d’envoyer un rapport d’erreurs lorsqu’un pilote de périphérique demande un logiciel supplémentaire au cours de l’installation|Activé|
||Désactiver les bulles « Nouveau matériel détecté » pendant l’installation de périphériques|Activé|

Développez le niveau **Filesystem**, double-cliquez sur **NTFS**, double-cliquez sur **Options de création de noms courts**, sélectionnez l’option **Activé**, puis, dans le menu déroulant **Options** sélectionnez  **Activer sur tous les volumes**. Cliquez sur **OK** pour enregistrer.

Réduisez le niveau **Filesystem**, puis développez **Gestion de la communication Internet**. Cliquez sur **Paramètres de communication Internet**. Ajustez chaque paramètre comme suit en double-cliquant dessus, puis en sélectionnant l’option **Activé**, puis en cliquant sur **OK** :

- Désactiver les liens « Events.asp » de l’observateur d’événements
- Désactiver le partage des données de personnalisation de l'écriture manuscrite
- Désactiver le signalement d’erreurs de la reconnaissance de l’écriture manuscrite
- Désactiver le contenu « Le saviez-vous ? » du Centre d’aide et de support content
- Désactiver la recherche dans la Base de connaissances Microsoft du Centre d’aide et de support
- Désactiver l’Assistant Connexion Internet si l’adresse URL de connexion fait référence à Microsoft.com
- Désactiver le téléchargement à partir d’Internet pour les Assistants Publication de sites Web et Commande en ligne via Internet
- Désactiver le service d’association de fichier Internet
- Désactiver l’inscription si l’adresse URL de connexion fait référence à microsoft.com
- Désactiver l’option Commander des photos de la Gestion des images
- Désactiver l’option Publier sur le Web de la Gestion des fichiers
- Désactiver le Programme d’amélioration des services pour Windows Messenger
- Désactiver le Programme d’amélioration de l’expérience utilisateur Windows
- Désactiver Rapport d’erreurs Windows
- Désactiver la recherche de pilotes de périphériques sur Windows Update

Cliquez sur **Gestion de l’alimentation**, puis double-cliquez sur **Sélectionner un mode de gestion d’alimentation actif**. Sélectionnez l’option **Activé**, puis, dans le menu déroulant **Options**, sélectionnez **Performances élevées**. Cliquez sur **OK** pour enregistrer.

Cliquez sur **Récupération**, puis double-cliquez sur **Autoriser la restauration de l’état par défaut du système**. Sélectionnez l’option **Activé**, puis cliquez sur **OK** pour enregistrer.

Développez **Dépannage et diagnostics**. Cliquez sur **Maintenance planifiée**, double-cliquez sur **Configurer le comportement de la maintenance planifiée**, puis sélectionnez l’option **Désactivé**. Cliquez sur **OK** pour enregistrer.

Cliquez sur chacune des zones de paramètres suivantes, puis double-cliquez sur **Configurer le niveau d'exécution des scénarios**, sélectionnez l’option **Désactivé**, puis cliquez sur **OK** pour enregistrer :

- Diagnostics des performances de démarrage Windows
- Diagnostic de fuite de mémoire Windows
- Programme de détection et de résolution de carence des ressources Windows
- Diagnostics des performances de l’arrêt de Windows
- Diagnostics des performances de la veille/reprise Windows
- Diagnostics des performances de réactivité du système Windows

Réduisez **Système**, puis développez **Composants Windows**. Ajustez chaque paramètre comme suit en double-cliquant dessus, puis en sélectionnant l’option correspondant à la valeur indiquée ici en cliquant sur **OK** :

|Zone Paramètre|Paramètre|Valeur recommandée pour une utilisation de VDI|  
|-------------------|-------|----------|
|Processus d’ajout de fonctionnalités à Windows 10|||
||Empêcher l’exécution de l’assistant|Activé|
|Stratégies d’exécution automatique|||
||Définir le comportement par défaut du programme Autorun|Sélectionnez Activé, puis, dans le menu déroulant **Options**, sélectionnez **N’exécuter aucune commande Autorun**|
|Contenu cloud|||
||Ne pas afficher les Conseils Windows|Activé|
||Désactiver les expériences consommateur Microsoft|Activé|
|Collecte des données et versions d’évaluation Preview|||
||Autoriser la télémétrie|Sélectionnez Activé, puis, dans le menu déroulant **Options**, sélectionnez **1 : De base**|
||Désactiver les fonctionnalités ou paramètres des versions préliminaires|     Désactivée|
||Ne pas afficher les notifications de commentaires|       Activé|
||Activer/désactiver le contrôle de l’utilisateur sur les builds d’Insider|      Désactivée|
|Gestionnaire de fenêtres du Bureau|||
||Ne pas autoriser l’invocation de Rotation 3D|       Activé|
||Ne pas autoriser les animations de fenêtres|       Activé|
||Utiliser une couleur unie pour l’arrière-plan du menu Démarrer|     Activé|
|Interface utilisateur latérale|||
||Autoriser le balayage latéral|     Désactivée|
||Désactiver les astuces|        Activé|
|Explorateur de fichiers|||
||Ne pas afficher la notification « Nouvelle application installée »|     Activé|
|Explorateur de jeux|||
||Désactiver le téléchargement d’informations sur les jeux|     Activé|
||Désactiver les mises à jour de jeux|        Activé|
||Désactiver le suivi de l’heure de la dernière utilisation d’un jeu du dossier Jeux|     Activé|
|Groupement résidentiel|||
||Empêcher l'ordinateur de rejoindre un groupe résidentiel|        Activé|
|Internet Explorer|||
||Autoriser les services Microsoft à fournir des suggestions améliorées à mesure que l’utilisateur entre du texte dans la barre d’adresses|        Désactivée|
||Désactiver la vérification périodique des mises à jour de logiciels Internet Explorer|        Activé|
||Désactiver l’affichage de l’écran de démarrage|        Activé|
||Installer automatiquement de nouvelles versions d’Internet Explorer|      Désactivée|
||Empêcher la participation au Programme d’amélioration de l’expérience utilisateur|     Activé|
||Empêcher l’exécution de l’Assistant Première exécution d’aller directement à la page d’accueil|   Sélectionnez Activé, puis, dans le menu déroulant **Options**, sélectionnez **Aller directement à la page d’accueil**|
||Définir le développement de processus d’onglet|Sélectionnez Activé, puis tapez ce qui suit dans la zone **Développement de processus d’onglet** : *Faible*.|
||Spécifier le comportement par défaut d’un nouvel onglet|Sélectionnez Activé, puis, dans le menu déroulant **Options**, sélectionnez **Page du nouvel onglet**|
||Désactiver les notifications de performances des modules complémentaires|        Activé|
||Désactiver la géolocalisation du navigateur|     Activé|
||Désactiver Rouvrir la dernière session de navigation|        Activé|
||Désactiver les suggestions de tous les moteurs de recherche installés par l’utilisateur|        Activé|
||Activer Sites suggérés|       Désactivée|

Au même niveau que paramètre **Internet Explorer** que vous avez ajusté dans la table précédente, notez un autre niveau de dossiers allant d’**Accélérateurs** à **Barres d’outils**. En d’autres termes, vous êtes maintenant à Stratégie ordinateur local > Configuration ordinateur > Modèles administration > Composants Windows > Internet Explorer. 

Ouvrez dossier **Supprimer l’historique de navigation**, double-cliquez sur **Autoriser la suppression de l’historique de navigation en quittant**, sélectionnez **Activer**, puis cliquez sur **OK**pour enregistrer et quitter.

Dans le coin supérieur gauche de l’Éditeur d'objets de stratégie de groupe, cliquez sur la flèche Précédent pour revenir au niveau **Internet Explorer**. Double-cliquez sur **Paramètres Internet**, double-cliquez sur **Paramètres avancés**, puis réglez les paramètres des sous-dossiers comme suit :

|Définition du dossier sous **Paramètres avancés**|Paramètre|Valeur recommandée pour une utilisation de VDI|  
|-------------------|-------|----------|
|**Navigation**|||
||Désactiver la détection des numéros de téléphone|Activé|
|**Multimédia**|||
||Autoriser Internet Explorer à lire les fichiers multimédias qui utilisent des codecs de remplacement|Désactivée|

Revenez au niveau **Internet Explorer**, puis double-cliquez sur **Paramètres Internet**. Dans ce dossier, définissez ces deux paramètres sous **Saisie semi-automatique** sur **Activé** :

- Désactiver les suggestions d’URL
- Désactiver la saisie semi-automatique de recherche Windows

Revenez en arrière de quatre niveaux, jusqu’à **Composants Windows**, double-cliquez sur **Emplacement et capteurs**, puis définissez ces trois paramètres sur **Activé** (à chaque fois, cliquez sur **OK** pour enregistrer et quitter) :

- Désactiver l’emplacement
- Désactiver le script d’emplacement
- Désactiver les capteurs

Au niveau de **Emplacement et capteurs**, double-cliquez sur **Service de localisation Windows**, puis définissez **Désactiver le service de localisation Windows** sur **Activé**. Cliquez sur **OK** pour enregistrer et quitter.

Dans le volet gauche, cliquez sur **Cartes**, définissez ces paramètres sur valeur **Activé**. À chaque fois, cliquez sur **OK** pour enregistrer et quitter :

- Désactiver le téléchargement automatique et la mise à jour des données de carte
- Désactiver le trafic réseau non sollicité dans la page de paramètres des cartes hors connexion

Dans le volet de gauche, entrez dans chacun des sous-dossiers de paramètres suivants et réglez les paramètres individuels comme suit :

|Réglages du dossier sous **Composants Windows**|Paramètre|Valeur recommandée pour une utilisation de VDI|  
|-------------------|-------|----------|
|**OneDrive**|||
||Empêcher l’utilisation de OneDrive pour le stockage de fichiers|Activé|
||Enregistrer les documents sur OneDrive par défaut|Désactivée|
|**Flux RSS**|||
||Empêcher la découverte automatique des flux et des composants Web Slice|Activé|
|**Recherche**|||
||Autoriser Cortana|        Désactivée|
||Autoriser Cortana au-dessus de l’écran de verrouillage|      Désactivée|
||Autoriser la recherche et autoriser Cortana à utiliser la localisation|     Désactivée|
||Ne pas autoriser la recherche web|      Activé|
||Ne pas rechercher sur le web ou afficher des résultats web dans la recherche|        Activé|
||Empêcher l’ajout d’emplacements UNC à l’index depuis le Panneau de configuration|     Activé|
||Empêcher l’indexation des fichiers dans le cache des fichiers hors connexion|        Activé|
|**Microsoft Store**|||
||Désactiver la proposition d’effectuer une mise à jour vers la dernière version de Windows|Activé|
|**Rapport d’erreurs Windows**|||
||Envoyer automatiquement des images mémoire pour les rapports d’erreurs générés par le système d’exploitation|       Désactivée|
||Désactiver le rapport d’erreurs Windows|      Activé|
|**Windows Installer**|||
||Contrôler la taille maximale du cache de fichiers de base|  Sélectionnez Activé, puis, dans la zone **Options**, définissez le paramètre **Taille maximale du cache de fichiers de base** sur *5*.|
||Désactiver la création de points de vérification pour la Restauration du système|      Activé|
|**Windows Mail 7.0**|||
||Désactiver la fonctionnalité Communautés|Activé|
|**Lecteur Windows Media**|||
||Ne pas afficher les boîtes de dialogue de configuration à la première exécution du Lecteur|       Activé|
||Empêcher le partage de médias|        Activé|
|**Centre de mobilité Windows**|||
||Désactiver le Centre de mobilité Windows|Activé|
|**Analyse de fiabilité Windows**|||
||Configurer les fournisseurs WMI du service de fiabilité|Désactivée|
|**Windows Update**|||
||Autoriser l’installation immédiate des mises à jour automatiques|       Activé|
||Supprimer l’accès à toutes les fonctionnalités Windows Update|     Activé|
|Dans le dossier **Windows Update**, ouvrez **Différer les mises à jour de Windows**|||
||Choisir quand recevoir les mises à jour des fonctionnalités|Sélectionnez Activé, puis, dans la zone **Options** cliquez sur le menu déroulant en regard de **Sélectionner le niveau de disponibilité de branche des mises à jour des fonctionnalités que vous voulez recevoir** pour sélectionner **Current Branch for Business**. Définissez le paramètre **Après la publication d’une mise à jour des fonctionnalités, différer sa réception pendant ce nombre de jours** sur *180 jours*.
||Choisir quand recevoir les mises à jour qualité|Sélectionnez Activé, puis, dans la zone **Options**, définissez le paramètre **Après la publication d’une mise à jour qualité, différer sa réception pendant ce nombre de jour** sur *30 jours*, puis cochez la case en regard de **Suspendre les mises à jour qualité**.

Dans le volet de gauche de l’Éditeur d'objets de stratégie de groupe, cliquez sur **Configuration utilisateur**. Dans le volet gauche, cliquez sur **Modèles d'administration**, puis entrez dans chacun des sous-dossiers de paramètres suivants et réglez les paramètres individuels comme suit :

|Réglages du dossier sous **Modèles d’administration**|Paramètre|Valeur recommandée pour une utilisation de VDI|  
|-------------------|-------|----------|
|**Bureau**|||
||Ne pas ajouter de partages des documents récemment ouverts dans Emplacements réseau|Activé|
|Dans le dossier **Bureau**, ouvrez **Active Directory**|||
||Taille maximale des recherches dans Active Directory|Sélectionnez Activé, puis dans la zone **Options**, définissez l’option **Nombre d’objets renvoyés** sur *5000*.|
|**Menu Démarrage et barre des tâches**|||
||Effacer la liste des programmes récents pour les nouveaux utilisateurs|     Activé|
||Ne pas afficher ni suivre les éléments des listes de raccourcis à partir d'emplacements distants|        Activé|
||Désactiver les bulles de notification d’annonce de fonctionnalité|     Activé|
||Désactiver le suivi utilisateur|       Activé|
|Dans le dossier **Menu Démarrer et barre des tâches**, ouvrez **Notifications**|||
||Désactiver les notifications toast|Activé|
|Dans le dossier **Composants Windows**, ouvrez :|||
|**Contenu cloud**|||
||Désactiver toutes les fonctionnalités Windows à la une|Activé|
|**Explorateur de fichiers**|||
||Désactiver la mise en cache des miniatures|       Activé|
||Désactiver l’affichage des entrées de recherche récentes de la zone de recherche de l’Explorateur de fichiers|        Activé|
||Désactiver la mise en cache des miniatures dans les fichiers masqués thumbs.db|      Activé|

## <a name="microsoft-store-apps"></a>Applications du Microsoft Store
Nous vous recommandons de supprimer certaines applications Microsoft Store de votre image VDI. Ainsi, vous pourrez réduire l’utilisation du processeur et économiser de l’espace disque. Voici une liste d’applications dont nous vous recommandons la suppression :

- Obtenir Office
- Skype (Preview)
- Prise en main (en particulier s’il n’y a aucune connexion à Internet)
- Hub de commentaires
- Microsoft Solitaire Collection
- Wi-Fi payants et Cellulaire

Pour personnaliser le profil utilisateur par défaut utilisé pour créer des images VDI, utilisez le compte Administrateur intégré. S’il n'est pas déjà activé, vous pouvez le faire en utilisant Utilisateurs locaux et Groupes dans Gestion de l’ordinateur. Puis, connectez-vous à votre compte Administrateur pour mener à bien la procédure suivante.

> [!NOTE]  
> Ne supprimez pas les applications système telles que l’application Store. Elles sont difficiles à réinstaller. D’autres applications peuvent être facilement réinstallées à partir du Store.

### <a name="delete-unwanted-apps-from-the-administrator-user-profile"></a>Supprimer les applications indésirables à partir du profil d’utilisateur Administrateur
1. Dans Windows PowerShell, exécutez `Get-AppxPackage | ft PackageFamilyName` pour afficher la liste des applications installées.
2. Exécutez les cmdlets en suivant le format de cet exemple pour chaque gestionnaire de package d’application à désinstaller :

    `Get-AppxPackage *messaging* | Remove-AppxPackage`

    `Get-AppxPackage *WindowsMaps* | Remove-AppxPackage`

    `Get-AppxPackage *ZuneMusic* | Remove-AppxPackage`

### <a name="delete-the-payload-of-unwanted-store-apps"></a>Supprimer la charge utile d’applications de Store indésirables
Cela empêche la réinstallation de telles applications.
1. Utilisez cette cmdlet pour répertorier les applications du Store et d’autres éléments ayant provisionné des données dans le stockage : `Get-AppxProvisionedPackage -Online`.
2. Supprimer un package donné avec `Remove-AppxProvisionedPackage -Online -PackageName MyAppPackage`, à l’aide de du MyAppPackage approprié retourné à l’étape 1. Par exemple, pour supprimer le package de Zune, exécutez `Remove-AppxProvisionedPackage -Online -PackageName Microsoft.ZuneMusic_2019.17012.10311.0_neutral_~_8wekyb3d8bbwe`.

## <a name="removing-other-items"></a>Suppression d’autres éléments
Vous pouvez supprimer l’application et l’icône de OneDrive, désactiver les icônes système et supprimer les mises à jour téléchargées.

### <a name="remove-onedrive-icon-and-app"></a>Supprimer l’application et l’icône de OneDrive
1. Cliquez sur **Démarrer** et faites défiler la liste d’applications jusqu’à l’icône **OneDrive**.
2. Cliquez sur l’icône **OneDrive**, pointez sur **Plus**, puis cliquez sur **Ouvrir l’emplacement du fichier**.
3. Faites un clic droit sur l’cône **OneDrive** dans son emplacement de fichier, puis cliquez sur **Supprimer**.

Pour supprimer l’application OneDrive :
1. Cliquez sur **Démarrer** et faites défiler la liste d’applications jusqu’à l’icône **OneDrive**.
2. Faites un clic droit sur l’icône **OneDrive**, puis cliquez sur **Désinstaller**. La fenêtre Programmes et fonctionnalités s’ouvre.
3. Dans Programmes et fonctionnalités, cliquez sur **Microsoft OneDrive**, puis sur **Désinstaller**.

### <a name="programs-and-features-from-previous-versions-of-control-panel"></a>Programmes et fonctionnalités (à partir des versions précédentes du Panneau de configuration)
1. Cliquez sur le bouton **Démarrer**, tapez *Contrôle*, puis appuyez sur ENTRÉE.
2. Cliquez ou double-cliquez sur **Programmes et fonctionnalités**.
3. Dans la zone la plus à gauche, sous **Page d’accueil du Panneau de configuration**, appuyez ou cliquez sur **Activer ou désactiver des fonctionnalités Windows**. Une nouvelle interface utilisateur s’ouvre.
4. Décochez les cases en regard des éléments inutiles dans l’image de base, par exemple : **Support de partage de fichiers SMB 1.0/CIFS**.

### <a name="turn-system-icons-off"></a>Désactiver les icônes système
1. Cliquez sur le bouton **Démarrer**, puis sur **Paramètres** (l’icône en forme d’engrenage).
2. Dans la zone de texte **Rechercher un paramètre**, tapez *Barre des tâches*, puis cliquez sur **Paramètres de la barre des tâches**.
3. Faites défiler la section **Barre des tâches** jusqu'à la section **Zone de notification**.
4. Cliquez ou appuyez sur **Activer ou désactiver les icônes système**, puis activez ou désactivez chaque icône système pour l’image, selon vos besoins.

### <a name="delete-downloaded-updates"></a>Supprimer les mises à jour téléchargées
1. Dans l’Explorateur de fichiers, accédez à **C:\Windows\Software Distribution\Download**.
2. Supprimez tous les fichiers et dossiers dans ce répertoire.













 













