---
title: Configuration recommandée pour les bureaux VDI
description: Recommandé des paramètres et la configuration pour réduire la charge pour les ordinateurs de bureau Windows 10 1607 (10.0.1393) utilisé en tant qu’images VDI
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 24704373dedf6a44809b83f3df17bd073cee2bf8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818380"
---
# <a name="recommended-settings-for-vdi-desktops"></a>Paramètres recommandés pour les bureaux VDI

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10

Microsoft Desktop Virtualization détecte automatiquement les configurations de périphériques et les conditions du réseau pour créer les utilisateurs et en cours d’exécution plus tôt en activant le programme d’installation instantanée des applications d’entreprise et des postes de travail, et il apporte l’informatique pour fournir l’accès à l’ancien applications pendant la migration vers Windows 10.

Bien que le système d’exploitation Windows 10 est très bien réglé prêts à l’emploi, il existe des opportunités affiner supplémentaire spécifiquement pour l’environnement d’entreprise de Microsoft Virtual Desktop Infrastructure (VDI). Dans l’environnement VDI, plusieurs services d’arrière-plan et les tâches sont désactivées à partir du début.

Cette rubrique n’est pas un plan, mais plutôt un repère ou un point de départ. Certaines recommandations peuvent entraîner la désactivation des fonctionnalités que vous souhaitez utiliser, vous devez donc envisager les coûts par rapport à l’avantage d’ajustement de n’importe quel paramètre dans votre scénario particulier.

Ces instructions et les paramètres recommandés sont pertinentes pour Windows 10 1607 (version 10.0.1393).

> [!NOTE]  
> Tous les paramètres ne sont pas spécifiquement mentionnés dans cette rubrique peuvent être laissés à leurs valeurs par défaut (ou un ensemble par vos besoins et les stratégies) sans impact notable sur les fonctionnalités de l’infrastructure VDI.

Lorsque vous créez une image pour le déploiement de VDI, veillez à utiliser le **Current Branch**. Pour plus d’informations sur la branche actuelle, consultez [les informations de version de Windows 10](https://technet.microsoft.com/windows/release-info.aspx).

## <a name="creating-the-windows-10-image"></a>Création de l’image de Windows 10
La première étape consiste à installer une image de référence de Windows 10 1607 (version 10.0.1393) sur l’un ordinateur virtuel ou physique. L’installation à une machine virtuelle est facile et vous permet d’enregistrer les versions du fichier (VHD) sur le disque dur virtuel, au cas où vous souhaitez revenir à une version antérieure.

Pendant l’installation, vous pouvez choisir une **paramètres Express** ou **personnaliser**. Les paramètres offerte pendant la **personnaliser** option sont réglables en utilisant une stratégie de groupe, par conséquent, la méthode d’installation du système d’exploitation de base n’est pas particulièrement importante.


Si vous avez choisi **personnaliser**, vous pouvez ajuster ces paramètres lors de l’installation :

## <a name="in-customize-settings"></a>Dans « Personnaliser les paramètres »

Vous pouvez également ajuster ces après l’installation avec l’éditeur de stratégie de groupe ; consultez la section « Paramètres de stratégie de groupe » de cette rubrique.

|Paramètre|Valeur par défaut|Valeur recommandée pour une utilisation de l’infrastructure VDI|  
|-------------------|----------|--------------|
|**Personnalisation**| | |
|Personnalisez votre enregistrement vocal, taper et entrée d’encrage en envoyant vos données d’entrée à Microsoft.|    Activé| Désactivé|
|Envoyer en tapant et entrée manuscrite de données à Microsoft pour améliorer la plateforme de reconnaissance et de suggestion.|  Activé| Désactivé|
|Permettre aux applications à utiliser votre ID de publicité pour une expérience entre applications.|  Activé| Désactivé|
|Permettent de Skype (si installé) vous aident à vous connecter avec vos amis dans votre carnet d’adresses et vérifiez votre numéro de téléphone mobile. Les frais SMS et les données peuvent s’appliquer.|    Activé| Désactivé|
|**Emplacement**| | |
|Activer mon appareil et let Windows et des applications demandent votre emplacement, notamment l’historique des emplacements de recherche| Activé| Désactivé|
|Connectivité et le rapport d’erreurs| | |
|Se connecter automatiquement, selon les suggestions fournies, aux points d'accès ouverts. Pas tous les réseaux sont sécurisées.|    Activé| Désactivé|
|Se connecter automatiquement pour ouvrir les zones réactives temporairement pour voir si payé services réseau disponibles.| Activé| Désactivé|
|Envoyer des données d’utilisation et de diagnostic complet à Microsoft. Désactivation de cette option envoie uniquement les données de base.| Activé| Désactivé|
|**Navigateur, de protection et de mise à jour**| | |
|SmartScreen d’utilisation en ligne des services pour vous protéger contre les contenus malveillants et ne télécharge dans sites chargés par les navigateurs de Windows et des applications de Store|    Activé| Sur (s’il n’existe aucun accès à Internet, puis défini sur désactivé.)
|Utiliser la prédiction de page pour améliorer la lecture, d’accélérer la navigation et améliorer votre expérience globale dans les navigateurs de Windows. Les données de votre navigation seront envoyées à Microsoft.| Activé| Désactivé|
|Obtenir des mises à jour à partir d’et envoi à d’autres ordinateurs sur Internet pour accélérer l’application et téléchargements de Windows Update|   Activé| Désactivé|

Une fois l’installation terminée, vous pouvez continuer à ajuster les paramètres en commençant par **Windows paramètres**.

## <a name="in-windows-settings"></a>Dans les paramètres de Windows
Pour accéder aux paramètres de Windows, cliquez sur **Démarrer** (l’icône de Windows sur la barre des tâches), puis cliquez sur le **paramètres** icône (la forme d’un engrenage).

### <a name="in-the-system-area-of-windows-settings"></a>Dans la zone « Système » de paramètres de Windows
Dans la zone de paramètres de Windows, cliquez sur le **système** icône vous permet d’accéder à un nombre de paramètres liés au système. Certaines d'entre elles doivent être ajustées pour optimiser l’utilisation VDI--ces paramètres sont les plus importants :

#### <a name="apps-and-features"></a>Applications et fonctionnalités

Pour supprimer une application, en l’excluant ainsi à partir de votre image VDI, cliquez sur l’application, puis cliquez sur **désinstallation**. Si **désinstallation** est grisé, vous ne pouvez pas le supprimer par cette méthode ; vous pourrez le supprimer avec Windows PowerShell ou essayez les étapes suivantes :
1. Cliquez sur **gestion des fonctionnalités facultatives** (immédiatement en dessous le **applications et fonctionnalités** titre sur la même page).
2. Cliquez sur la fonctionnalité facultative, puis cliquez sur **désinstallation**.

Pour envisager de supprimer (le cas échéant) les fonctionnalités suivantes :
- **Contactez le support technique**
- **Contenu de démonstration de vente au détail anglais (États-Unis)**
- **Contenu de démonstration de vente au détail neutre**
- **Aide rapide**

#### <a name="default-apps"></a>Applications par défaut

Cette zone définit l’application à utiliser par défaut pour certaines fonctions génériques telles que le courrier électronique, la navigation sur le web et les mappages. Si vous souhaitez une autre application à utiliser pour une fonction particulière, cliquez sur l’entrée en cours, puis cliquez sur l’application que vous préférez être utilisé dans l’image de l’infrastructure VDI. Pour une application non Microsoft constituer un choix disponible, vous devez installer l’application avant de régler ce paramètre.

#### <a name="notifications-and-actions"></a>Notifications et actions

Ces valeurs recommandées réduira les notifications et l’activité du réseau en arrière-plan dans un environnement VDI :

|Paramètre|Valeur par défaut|Valeur recommandée pour une utilisation de l’infrastructure VDI|  
|-------------------|----------|--------------|
|Obtenir des notifications à partir d’applications et d’autres expéditeurs| Activé| Désactivé|
|Afficher les notifications sur l’écran de verrouillage.|    Activé| Désactivé|
|Afficher les appels entrants VoIP, les rappels et les alarmes sur l’écran de verrouillage.|   Activé| Désactivé|
|Afficher les conseils, astuces et des suggestions que vous utilisez Windows.|    Activé| Désactivé|


#### <a name="offline-maps"></a>Cartes hors connexion

Ce paramètre est uniquement applicable si l’application de cartes est installée. Sa valeur par défaut est **sur**; pour une utilisation VDI la valeur recommandée est **hors**. 

#### <a name="tablet-mode"></a>Mode tablette

|Paramètre|Valeur par défaut|Valeur recommandée pour une utilisation de l’infrastructure VDI|  
|-------------------|----------|--------------|
|Lorsque je me connecter|    Utiliser le mode approprié pour mon matériel|   Utiliser le mode bureau|
|Lorsque cet appareil bascule automatiquement en mode activé ou désactivé|    Toujours demander avant de changer| Ne plus me poser et ne pas basculer|
|Les options Masquer les icônes d’application sur la barre des tâches en mode tablette|  Activé| Désactivé|


### <a name="in-the-devices-area-of-windows-settings"></a>Dans la zone « Appareils » des paramètres de Windows
Dans la zone de paramètres de Windows, cliquez sur le **appareils** icône vous permet d’accéder à un nombre de paramètres liés au système. Certaines d'entre elles doivent être ajustées pour optimiser l’utilisation VDI--ces paramètres sont les plus importants :

#### <a name="autoplay"></a>Lecture automatique

|Paramètre|Valeur par défaut|Valeur recommandée pour une utilisation de l’infrastructure VDI|  
|-------------------|----------|--------------|
|Utiliser l’exécution automatique pour tous les périphériques et supports|    Activé| Désactivé|
|Lecteur amovible :|Choisissez une valeur par défaut|Ne rien faire|
|Carte mémoire|Choisissez une valeur par défaut|Ne rien faire|

### <a name="in-the-personalization-area-of-windows-settings"></a>Dans la zone « Personnalisation » des paramètres de Windows
Dans la zone de paramètres de Windows, cliquez sur le **personnalisation** icône vous permet d’accéder à un nombre de paramètres liés au système. Certaines d'entre elles doivent être ajustées pour optimiser l’utilisation VDI--ces paramètres sont les plus importants :

#### <a name="background"></a>Arrière-plan
Parfois, l’arrière-plan noir par défaut peut permettre à considérer que l’ordinateur ne répond pas. Modification de la couleur d’arrière-plan peut aider à clarifier les choses. Pour cela, procédez comme suit:
1. Dans le **arrière-plan** zone, cliquez sur le menu déroulant.
2. Pour modifier la couleur d’arrière-plan, cliquez sur **couleur unie**, puis cliquez sur une des couleurs que le noir. Alternativement, vous pouvez cliquer sur **image** , puis sélectionnez une image à utiliser comme arrière-plan.

#### <a name="start"></a>Début

|Paramètre|Valeur par défaut|Valeur recommandée pour une utilisation de l’infrastructure VDI|  
|-------------------|----------|--------------|
|Parfois afficher les suggestions dans Démarrer|    Activé| Désactivé|
|Applications d’afficher plus utilisé|Activé|Désactivé|
|Afficher les applications ajoutées récemment|Activé|Désactivé|
|Afficher les éléments récemment ouverts dans les listes de raccourcis de début ou de la barre des tâches|Activé|Désactivé|

#### <a name="taskbar"></a>Barre des tâches
Le paramètre par défaut consiste à utiliser les boutons de barre des tâches de grande taille (autrement dit, une valeur de « Désactivé » pour **utiliser les boutons de barre des tâches petite**). Ce paramètre entraîne l’élément de Cortana à utiliser un grand nombre de la zone de la barre des tâches. Pour éviter ce problème, définissez **utiliser les boutons de barre des tâches petite** sur « Activé ». Si vous préférez que les éléments de la barre des tâches restent plus grande, mais souhaitez ne pas que Cortana occuper beaucoup d’espace, avec le bouton droit de la barre des tâches, pointez sur **Cortana**et dans le menu qui s’envole out, sélectionnez **Hidden**.

### <a name="in-the-privacy-area-of-windows-settings"></a>Dans la zone « Confidentialité » des paramètres de Windows
Dans la zone de paramètres de Windows, cliquez sur le **confidentialité** icône vous permet d’accéder à un nombre de paramètres liés au système. Certaines d'entre elles doivent être ajustées pour optimiser l’utilisation VDI--ces paramètres sont les plus importants :

#### <a name="general"></a>Général
Certains de ces paramètres sont également définis dans la fenêtre « Personnaliser les paramètres », décrit au début de cette rubrique.

|Paramètre|Valeur par défaut|Valeur recommandée pour une utilisation de l’infrastructure VDI|  
|-------------------|----------|--------------|
|Permettre aux applications d’utiliser mon ID de publicité pour des expériences entre applications (désactivation de cette option sera de réinitialisation de votre ID)|  Activé| Désactivé|
|Laissez les sites Web accéder à ma liste de langues pour fournir du contenu local|Activé|Désactivé|
|Permettre aux applications sur d’autres appareils ouvrez applications et continuer d’expériences sur ce périphérique.|Activé|Désactivé|

#### <a name="camera"></a>Appareil photo

La valeur par défaut pour « Permettre aux applications utiliser mon appareil photo » est **sur**; pour une utilisation VDI la valeur recommandée est **hors**.


#### <a name="microphone"></a>Microphone

La valeur par défaut pour « Permettre aux applications utiliser mon microphone » est **sur**; pour une utilisation VDI la valeur recommandée est **hors**.

#### <a name="notifications"></a>Notifications

La valeur par défaut pour « Permettre les applications d’accéder à Mes notifications » est **sur**; pour une utilisation VDI la valeur recommandée est **hors**.

#### <a name="contacts"></a>Contacts

La valeur par défaut pour « Permettre les applications d’accéder à mes contacts » est **sur**; pour une utilisation VDI la valeur recommandée est **hors**.

#### <a name="calendar"></a>Calendrier

La valeur par défaut pour « Permettre les applications d’accéder à mon calendrier » est **sur**; pour une utilisation VDI la valeur recommandée est **hors**.

#### <a name="call-history"></a>Historique des appels

La valeur par défaut pour « Permettre aux applications mon historique des appels d’accès » est **sur**; pour une utilisation VDI la valeur recommandée est **hors**.

#### <a name="email"></a>E-mail

La valeur par défaut pour « Permettre aux applications d’accéder et envoyer un courrier électronique » est **sur**; pour une utilisation VDI la valeur recommandée est **hors**.

#### <a name="messaging"></a>Messagerie

La valeur par défaut pour « Permettre aux applications lire ou envoyer des messages (SMS ou MMS) » est **sur**; pour une utilisation VDI la valeur recommandée est **hors**.

#### <a name="radios"></a>Radios

La valeur par défaut pour « Permet de radios de contrôle d’applications » est **sur**; pour une utilisation VDI la valeur recommandée est **hors**.

#### <a name="other-devices"></a>Autres appareils

La valeur par défaut pour « Let vos applications automatiquement partageront et synchroniser des informations avec des appareils sans fil ne pas explicitement jumelés avec votre PC, une tablette ou téléphone » est **sur**; pour une utilisation VDI la valeur recommandée est **hors**.

#### <a name="feedback-and-diagnostics"></a>Commentaires et diagnostics

La valeur par défaut pour « Windows demandez à mes commentaires » est **automatiquement**; pour une utilisation de l’infrastructure VDI, la valeur recommandée est **jamais**.

#### <a name="background-apps"></a>Applications en arrière-plan
Applications répertoriées ont une valeur par défaut de **sur**, ce qui leur permet de recevoir des informations, envoyer des notifications, et mettre à jour eux-mêmes s’ils sont utilisés ou non. Vous devez désactiver (la valeur **hors**) toutes les applications vous ne souhaitez pas en cours d’exécution en arrière-plan dans l’image de l’infrastructure VDI.

### <a name="update-and-security"></a>Mise à jour et sécurité
#### <a name="windows-update"></a>Windows Update
Dans le **mettre à jour les paramètres** zone, cliquez sur **les options avancées** ajuster ces paramètres :

|Paramètre|Valeur par défaut|Valeur recommandée pour une utilisation de l’infrastructure VDI|  
|-------------------|----------|--------------|
|Donnez-moi des mises à jour pour d’autres produits Microsoft lors de la mise à jour Windows|    Désactivée|    sélectionné|
|Différer les mises à jour de fonctionnalité|Désactivée|sélectionné|
|Utiliser mes infos de connexion pour terminer automatiquement la configuration de mon appareil après une mise à jour |Désactivée|Varie selon la configuration spécifique d’infrastructure VDI|

Sur le **options avancées** , cliquez sur **choisir la façon dont les mises à jour sont remises** pour accéder à la valeur de « Mises à jour à partir de plusieurs endroits. » La valeur par défaut est **sur**; pour une utilisation VDI la valeur recommandée est **hors**.

## <a name="in-control-panel-and-other-system-utilities"></a>Dans le panneau de configuration et d’autres utilitaires système

Les paramètres dans cette section sont réglables en accédant via le panneau de configuration ou ouvrir l’utilitaire directement.

> [!NOTE]  
> Tous les paramètres ne sont pas spécifiquement mentionnés dans cette rubrique peuvent être laissés à leurs valeurs par défaut (ou un ensemble par vos besoins et les stratégies) sans impact notable sur les fonctionnalités de l’infrastructure VDI.


### <a name="task-scheduler"></a>Planificateur de tâches
Le moyen le plus rapide pour ouvrir le Planificateur de tâches consiste à transmettre le bouton de Windows et le type *Planificateur de tâches* ou *taskschd.msc*. Dans les résultats qui retournent, cliquez sur **Planificateur de tâches** pour ouvrir l’utilitaire. Dans le Planificateur de tâches, développez **bibliothèque du Planificateur de tâches**, développez **Microsoft**, puis développez **Windows**. Vous avez désormais accès à la liste des collections de tâches. Pour modifier l’état de chaque tâche planifiée, faites un clic droit, puis cliquez sur l’état souhaité (en règle générale, **désactivé** pour une utilisation de l’infrastructure VDI).

|Collection de tâches|Nom de la tâche|État par défaut|État recommandé pour une utilisation de l’infrastructure VDI|  
|-------------------|-------------|----------|--------------|
|Programme d’amélioration du produit||||
||Consolidateur|Enabled|Désactivée|
||KernelCeipTask|Enabled|Désactivée|
||UsbCeip|Enabled|Désactivée|
|la défragmentation||||
||ScheduledDefrag|Enabled|Désactivée|
|Location||||
||Notifications|Enabled|Désactivée|
||WindowsActionDialog|Enabled|Désactivée|
|Maintenance||||
||WinSAT|Enabled|Désactivée|
|Cartes||||
||MapsToastTask|Enabled|Désactivée|
||MapsUpdateTask|Enabled|Désactivée|
|Comptes de haut débit mobiles||||
||Analyseur de métadonnées MNO|Enabled|Désactivée|
|Diagnostics d’efficacité énergétique||||
||Analyser le système|Enabled|Désactivée|
|Environnement de récupération||||
||VerifyWinRE|Enabled|Désactivée|
|Démonstration de la vente au détail||||
||CleanupOfflineContent|Enabled|Désactivée|
|Shell||||
||FamilySafetyMonitor|Enabled|Désactivée|
||FamilySafetyRefreshTask|Enabled|Désactivée|
|Rapport d’erreurs Windows||||
||QueueReporting|Enabled|Désactivée|
|Partage de support de Windows||||
||UpdateLibrary|Enabled|Désactivée|

Cliquez sur **Windows** à nouveau pour réduire, puis cliquez sur **XblGameSave**. Cela vous permet d’accéder aux tâches **XBLGameSaveTask** et **XBLGameSaveTaskLogon**; ces deux peuvent être définies sur **désactivé**.

### <a name="performance-monitor"></a>Analyseur de performances
Le moyen le plus rapide pour ouvrir le moniteur de performances consiste à transmettre le bouton de Windows et le type *Analyseur de performances* ou *perfmon.msc*. Dans les résultats qui retournent, cliquez sur **Analyseur de performances**. Dans l’Analyseur de performances, cliquez sur **des ensembles de collecteurs de données** , puis double-cliquez sur **Sessions de suivi d’événements**. Avec le bouton droit **WiFiSession**; si elle est dans l’état par défaut de **en cours d’exécution**, puis cliquez sur **arrêter**.

Cliquez sur **StartupEventTraceSessions**, puis cliquez sur **ReadyBoot**; si elle est en cours d’exécution, cliquez sur **arrêter**. Cliquez sur **Sessions de suivi d’événements**, avec le bouton droit **ReadyBoot**, puis cliquez sur **propriétés**. Dans la boîte de dialogue qui s’ouvre, cliquez sur le **la Session de Trace** onglet. Effacer la **activé** case à cocher.

### <a name="services"></a>Services
Le moyen le plus rapide pour gérer les Services consiste à transmettre le bouton de Windows et le type *services*. Dans les résultats qui retournent, cliquez sur **Services**. Les services suivants sont de bons candidats pour la désactiver pour une utilisation dans des scénarios VDI. Toutefois, vous devrez peut-être effectuer des tests pour vérifier qu’ils ne sont pas nécessaires pour vos besoins. Pour désactiver un service, dans le **Services** -composant logiciel enfichable, cliquez sur le nom de service, puis cliquez sur **propriétés**. Sur le **général** , cliquez sur le **type de démarrage** menu déroulant, puis cliquez sur **désactivé**. Cliquez sur **OK**.

- BranchCache
- Optimisation de la distribution
- Hôte de Service de diagnostic
- Service de zone réactive mobiles Windows
- Xbox Live Auth Manager
- Xbox Live Game enregistrer
- Service de mise en réseau Xbox Live

### <a name="file-explorer-options"></a>Options de l’Explorateur de fichiers
Appuyez sur le bouton de Windows et le type *le panneau de configuration*. Dans les résultats qui retournent, cliquez sur **le panneau de configuration**. Dans le panneau de configuration, cliquez sur **les Options de l’Explorateur de fichiers**. Dans la boîte de dialogue qui s’ouvre, cliquez sur le **recherche** onglet, puis dans le **lors de la recherche des emplacements non indexés** zone, désactivez la case à cocher **inclure les répertoires système**. Cliquez sur **OK** à enregistrer.

### <a name="flash-settings"></a>Paramètres du Flash
Appuyez sur le bouton de Windows et le type *le panneau de configuration*. Dans les résultats qui retournent, cliquez sur **le panneau de configuration**. Dans le panneau de configuration, cliquez sur **Flash Player** pour ouvrir le Gestionnaire de paramètres de Flash Player. Sur le **stockage** , sélectionnez le bouton radio pour **bloquer tous les sites de stocker des informations sur cet ordinateur**. Dans la boîte de dialogue qui s’ouvre, cliquez sur **OK**.

Sur le **webcam et micro** sous l’onglet le **caméra et les paramètres du Microphone** zone, sélectionnez la case d’option pour **bloquer tous les sites à partir d’à l’aide de la caméra et le microphone**.

Sur le **lecture** sous l’onglet le **assistée par l’homologue de mise en réseau** zone, sélectionnez la case d’option pour **bloquer tous les sites à partir de la mise en réseau homologue assistance**. Fermez le Gestionnaire de paramètres de Flash Player.

### <a name="internet-options"></a>Options Internet
Appuyez sur le bouton de Windows et le type *le panneau de configuration*. Dans les résultats qui retournent, cliquez sur **le panneau de configuration**. Dans le panneau de configuration, cliquez sur **Options Internet** pour ouvrir les propriétés d’Internet. Dans le **page d’accueil** zone, entrez l’URL du site web que vous voulez que les utilisateurs voient en tant que la page d’accueil dans les navigateurs. Cela peut être un site web pour votre entreprise, ou vous pouvez le définir sur une page d’accueil vide en entrant *sur : vide*.

Dans le **l’historique de navigation** zone, sélectionnez la case à cocher **supprimer l’historique de navigation en quittant**.

### <a name="power-options"></a>Options d’alimentation
Appuyez sur le bouton de Windows et le type *le panneau de configuration*. Dans les résultats qui retournent, cliquez sur **le panneau de configuration**. Dans le panneau de configuration, cliquez sur **Options d’alimentation** pour ouvrir le panneau de configuration Options d’alimentation. Dans le **choisir ou personnaliser un mode d’alimentation** zone, cliquez sur la flèche vers le bas pour **afficher des plans supplémentaires**, puis sélectionnez le bouton radio pour **hautes performances**. Ce paramètre a très peu d’impact sur l’hôte VDI.

### <a name="system"></a>System
Appuyez sur le bouton de Windows et le type *le panneau de configuration*. Dans les résultats qui retournent, cliquez sur **le panneau de configuration**. Dans le panneau de configuration, cliquez sur **système** pour ouvrir le panneau de configuration système. Dans le volet gauche, cliquez sur **paramètres système avancés**. Dans la boîte de dialogue qui s’ouvre, cliquez sur le **avancé** onglet. Dans le **performances** zone, cliquez sur le **paramètres** bouton, puis sur **effets visuels** onglet dans la boîte de dialogue qui s’affiche, sélectionnez le **Adjust pour de meilleures performances**  case d’option. Cliquez sur **OK** pour enregistrer et quitter.

## <a name="group-policy-settings"></a>Paramètres de stratégie de groupe

Pour modifier les paramètres de stratégie de groupe, appuyez sur le bouton de Windows et le type *stratégie de groupe* ou *gpedit.msc*. Dans les résultats qui retournent, cliquez sur **modifier la stratégie de groupe** pour ouvrir l’éditeur de stratégie de groupe locale.

> [!NOTE]  
> Tous les paramètres ne sont pas spécifiquement mentionnés dans cette rubrique peuvent être laissés à leurs valeurs par défaut (ou un ensemble par vos besoins et les stratégies) sans impact notable sur les fonctionnalités de l’infrastructure VDI.

Sous **Configuration ordinateur**, développez **Windows paramètres**, puis développez **paramètres de sécurité**. Cliquez sur **stratégies réseau liste Manager**, puis double-cliquez sur **tous les réseaux**. Dans la boîte de dialogue qui s’ouvre, dans le **emplacement réseau** zone, sélectionnez la case d’option pour **utilisateur ne peut pas modifier l’emplacement**. Cliquez sur le **OK** bouton pour enregistrer.

Réduire **Windows paramètres**, puis développez **modèles d’administration**. Cliquez sur ou développez **réseau**, puis ajustez chaque paramètre comme suit en double-cliquant dessus, puis en sélectionnant le bouton radio pour la valeur indiquée en cliquant sur le **OK** bouton :

|Zone de paramétrage|Paramètre|Valeur recommandée pour une utilisation de l’infrastructure VDI|  
|-------------------|-------|----------|
|Service de transfert intelligent en arrière-plan (BITS)|||
||Ne pas autoriser le client BITS utilise Windows BranchCache|Enabled|
||Ne pas autoriser l’ordinateur d’agir comme un client de mise en cache BITS|Enabled|
||Ne pas autoriser l’ordinateur d’agir comme un serveur de mise en cache BITS|Enabled|
||Autoriser la mise en cache BITS|Désactivée|
|BranchCache||
||Activer BranchCache|Désactivée|
|Authentification de la zone réactive||
||Activer l’authentification de zone réactive|Désactivée|
|Services de mise en réseau Microsoft Peer-to-Peer||
||Arrêter les Services de mise en réseau Microsoft Peer-to-Peer|Enabled|
|Fichiers hors connexion||
||Autoriser ou interdire l’utilisation de la fonctionnalité fichiers hors connexion|Désactivée|

Réduire **réseau**, puis développez **système**. Ajuster chaque paramètre comme suit en double-cliquant dessus, puis en sélectionnant le bouton radio pour la valeur indiquée et en cliquant sur le **OK** bouton :

|Zone de paramétrage|Paramètre|Valeur recommandée pour une utilisation de l’infrastructure VDI|  
|-------------------|----------|--------------|
|Installation de périphérique||
||Ne pas envoyer un rapport d’erreurs Windows lorsqu’un pilote générique est installé sur un appareil|Enabled|
||Empêcher la création d’un point de restauration système pendant l’activité de l’appareil qui invitait normalement la création d’un point de restauration|Enabled|
||Empêcher la récupération de métadonnées d’appareil à partir d’Internet|Enabled|
||Empêcher l’envoi d’un rapport d’erreurs quand un logiciel supplémentaire de demandes de pilote de périphérique lors de l’installation de Windows|Enabled|
||Désactiver les bulles « Nouveau matériel détecté » pendant l’installation de l’appareil|Enabled|

Développez **Filesystem**, double-cliquez sur **NTFS**, double-cliquez sur **abrégée des options de création de nom**, sélectionnez la case pour **activé**, puis utilisez le **Options** menu déroulant pour sélectionner **activer sur tous les volumes**. Cliquez sur le **OK** bouton pour enregistrer.

Réduire **Filesystem**, puis développez **gestion de la Communication Internet**. Cliquez sur **paramètres de Communication Internet**. Ajuster chaque paramètre comme suit en double-cliquant dessus, puis en sélectionnant le bouton radio pour **activé**, puis en cliquant sur le **OK** bouton :

- Désactiver les liens « Events.asp » l’Observateur d’événements
- Désactiver le partage de données de personnalisation de l’écriture manuscrite
- Désactiver le signalement de l’erreur reconnaissance de l’écriture manuscrite
- Désactiver centre d’aide et Support « Le saviez-vous ? » content
- Désactiver la recherche d’aide et de Base de connaissances Microsoft Support Center
- Désactiver l’Assistant connexion Internet si l’adresse URL de connexion fait référence à Microsoft.com
- Désactiver le téléchargement d’Internet pour la publication Web et commande en ligne des Assistants
- Désactiver le service d’Association de fichier Internet
- Désactiver l’inscription si l’adresse URL de connexion fait référence à Microsoft.com
- Désactiver la tâche d’image « Commander des photos »
- Désactiver la tâche « Publier sur le Web » pour les fichiers et dossiers
- Désactiver le programme Windows Messenger d’amélioration
- Désactiver Windows Customer Experience Improvement Program
- Désactiver rapport d’erreurs Windows
- Désactiver la recherche de pilotes de périphérique Windows Update

Cliquez sur **gestion de l’alimentation** , puis double-cliquez sur **sélectionner un plan d’alimentation**. Sélectionnez le bouton radio pour **activé**, puis utilisez le **Options** menu déroulant pour sélectionner **hautes performances**. Cliquez sur le **OK** bouton pour enregistrer.

Cliquez sur **récupération**, puis double-cliquez sur **autoriser la restauration du système à l’état par défaut**. Sélectionnez le bouton radio pour **activé**, puis cliquez sur le **OK** bouton pour enregistrer.

Développez **dépannage et Diagnostics**. Cliquez sur **Maintenance planifiée**, double-cliquez sur **configurer le comportement Maintenance planifiée**, puis sélectionnez le bouton radio pour **désactivé**. Cliquez sur le **OK** bouton pour enregistrer.

Pour chacune des zones de paramètres suivant, cliquez dessus, puis double-cliquez sur **configurer le niveau de l’exécution du scénario**, sélectionnez la case pour **désactivé**, puis cliquez sur le **OK**bouton pour enregistrer :

- Diagnostics de performances de démarrage de Windows
- Diagnostics de fuite de mémoire Windows
- Résolution et détection de carence des ressources Windows
- Diagnostics de performances d’arrêt Windows
- Diagnostics des performances de veille/reprise Windows
- Diagnostics des performances de la réactivité du système Windows

Réduire **système**, puis développez **les composants Windows**. Ajuster chaque paramètre comme suit en double-cliquant dessus, puis en sélectionnant le bouton radio pour la valeur indiquée en cliquant sur le **OK** bouton :

|Zone de paramétrage|Paramètre|Valeur recommandée pour une utilisation de l’infrastructure VDI|  
|-------------------|-------|----------|
|Ajouter des fonctionnalités à Windows 10|||
||Empêcher l’exécution de l’Assistant|Enabled|
|Stratégies de lecture automatique|||
||Définir le comportement par défaut pour l’exécution automatique|Activé, puis utiliser le **Options** menu déroulant pour sélectionner **n’exécutez pas les commandes d’exécution automatique**|
|Contenu de cloud|||
||Ne pas afficher les conseils de Windows|Enabled|
||Désactiver les expériences consommateur Microsoft|Enabled|
|Collecte de données et les versions d’évaluation|||
||Autoriser la télémétrie|Activé, puis utiliser le **Options** menu déroulant pour sélectionner **1 – de base**|
||Désactiver les fonctionnalités ou paramètres des versions préliminaires|     Désactivée|
||Ne pas afficher les notifications de commentaires|       Enabled|
||Activer/désactiver le contrôle de l’utilisateur sur les builds d’Insider|      Désactivée|
|Gestionnaire de fenêtrage|||
||Ne pas autoriser l’invocation de Flip3D|       Enabled|
||Ne pas autoriser les animations de fenêtres|       Enabled|
||Utiliser une couleur unie pour l’arrière-plan de démarrage|     Enabled|
|Bord de l’interface utilisateur|||
||Autoriser le balayage de bord|     Désactivée|
||Désactiver les info-bulles|        Enabled|
|Explorateur de fichiers|||
||Ne plus afficher la notification de « nouvelle application installée »|     Enabled|
|Explorateur de jeux|||
||Désactiver le téléchargement des informations sur les jeux|     Enabled|
||Désactiver les mises à jour de jeu|        Enabled|
||Désactiver le suivi de la dernière heure de lecture de jeux dans le dossier jeux|     Enabled|
|Groupe résidentiel|||
||Empêcher l'ordinateur de rejoindre un groupe résidentiel|        Enabled|
|Internet Explorer|||
||Autoriser les services Microsoft à fournir des suggestions améliorées à mesure que l’utilisateur entre du texte dans la barre d’adresses|        Désactivée|
||Désactiver la vérification périodique des mises à jour de logiciels Internet Explorer|        Enabled|
||Désactiver l’affichage de l’écran de démarrage|        Enabled|
||Installer automatiquement de nouvelles versions d’Internet Explorer|      Désactivée|
||Empêcher la participation au programme d’amélioration du produit|     Enabled|
||Empêcher en cours d’exécution Assistant 1ère utilisation d’accéder directement à la page d’accueil|   Activé, puis utiliser le **Options** menu déroulant pour sélectionner **accéder directement à la page d’accueil**|
||Définir la croissance de processus d’onglet|Activé, puis tapez ce qui suit dans le **onglet processus croissance** zone : *Faible*.|
||Spécifier le comportement par défaut pour un nouvel onglet|Activé, puis utiliser le **Options** menu déroulant pour sélectionner **nouvelle page d’onglets**|
||Désactiver les notifications de performances des modules complémentaires|        Enabled|
||Désactiver la géolocalisation du navigateur|     Enabled|
||Désactiver le rouvrir une dernière Session de navigation|        Enabled|
||Désactiver les suggestions pour tous les fournisseurs installés par l’utilisateur|        Enabled|
||Activer les sites suggérés|       Désactivée|

Au même niveau que le **Internet Explorer** paramètres que vous venez d’ajustée dans le tableau précédent, notez un autre niveau de dossiers allant **accélérateurs** à **barres d’outils**. En d’autres termes, vous êtes maintenant à la stratégie ordinateur Local > Configuration ordinateur > modèles d’administration > composants de Windows > Internet Explorer. 

Ouvrez le **supprimer l’historique de navigation** dossier, double-cliquez sur **autorise la suppression de l’historique de navigation en quittant**, sélectionnez **activer**, puis cliquez sur **OK**pour enregistrer et quitter.

Utilisez la flèche précédent dans le coin supérieur gauche d’éditeur pour revenir à la **Internet Explorer** niveau. Double-cliquez sur **paramètres Internet**, double-cliquez sur **paramètres avancés**, puis réglez les paramètres dans les sous-dossiers comme suit :

|Définition du dossier sous **paramètres avancés**|Paramètre|Valeur recommandée pour une utilisation de l’infrastructure VDI|  
|-------------------|-------|----------|
|**Navigation**|||
||Désactiver la détection des numéros de téléphone|Enabled|
|**Multimédia**|||
||Autorisez Internet Explorer lire des fichiers multimédias qui utilisent d’autres codecs|Désactivée|

Accédez à sauvegarder sur le niveau de **Internet Explorer**, puis double-cliquez sur **paramètres Internet**. Dans ce dossier, définissez ces deux paramètres sous **la saisie semi-automatique** à **activé**:

- Désactiver les suggestions d’URL
- Désactiver la saisie semi-automatique de Windows Search

Accédez sauvegarder les quatre niveaux à **les composants Windows**, double-cliquez sur **localisation et capteurs**, puis affectez ces trois paramètres **activé** (pour chacune, cliquez sur  **OK** pour enregistrer et quitter) :

- Mettre hors tension de l’emplacement
- Désactiver l’emplacement de script
- Désactiver les capteurs

While au niveau de **localisation et capteurs**, double-cliquez sur **fournisseur de localisation de Windows** et définissez **désactiver le fournisseur de localisation de Windows** à **activé**. Cliquez sur **OK** pour enregistrer et quitter.

Dans le volet gauche, cliquez sur **Maps**, ces paramètres la valeur **activé**; pour chacun, puis cliquez sur **OK** pour enregistrer et quitter :

- Désactiver le téléchargement automatique et la mise à jour des données de carte
- Désactiver le trafic réseau non sollicité dans la page de paramètres des cartes hors connexion

Utilisez le volet gauche, entrez chaque sous-dossier de paramètres suivants et ajuster les paramètres comme suit :

|Dossier paramètres sous **composants de Windows**|Paramètre|Valeur recommandée pour une utilisation de l’infrastructure VDI|  
|-------------------|-------|----------|
|**OneDrive**|||
||Empêcher l’utilisation de OneDrive pour le stockage de fichiers|Enabled|
||Enregistrer des documents sur OneDrive par défaut|Désactivée|
|**Flux RSS**|||
||Empêcher la détection automatique des flux et les composants Web Slice|Enabled|
|**Recherche**|||
||Autoriser Cortana|        Désactivée|
||Autoriser Cortana par-dessus écran de verrouillage|      Désactivée|
||Autoriser la recherche et autoriser Cortana à utiliser la localisation|     Désactivée|
||Ne pas autoriser la recherche web|      Enabled|
||Ne pas rechercher sur le web ou afficher les résultats web dans la recherche|        Enabled|
||Empêcher l’ajout d’UNC emplacements à l’index dans le panneau de configuration|     Enabled|
||Empêcher l’indexation des fichiers dans le cache des fichiers hors connexion|        Enabled|
|**Microsoft Store**|||
||Désactiver l’offre de mise à jour vers la dernière version de Windows|Enabled|
|**Rapport d’erreurs Windows**|||
||Envoyer automatiquement les vidages de mémoire pour les rapports d’erreur générés par le système d’exploitation|       Désactivée|
||Désactiver le rapport d’erreurs Windows|      Enabled|
|**Windows Installer**|||
||Contrôle la taille maximale du cache de fichiers de base|  Activé, puis utilisez le spinbox dans le **Options** zone à définir **taille maximale du cache de fichier de base** à *5*.|
||Désactiver la création de points de contrôle de restauration du système|      Enabled|
|**Windows Mail**|||
||Désactiver la fonctionnalité de Communautés|Enabled|
|**Windows Media Player**|||
||Ne pas afficher les boîtes de dialogue première utilisation|       Enabled|
||Empêcher le partage de fichiers multimédias|        Enabled|
|**Le centre de mobilité Windows**|||
||Désactiver le centre de mobilité Windows|Enabled|
|**Analyse de fiabilité de Windows**|||
||Configurer les fournisseurs WMI de fiabilité|Désactivée|
|**Windows Update**|||
||Autoriser l’installation immédiate des mises à jour automatiques|       Enabled|
||Supprimer l’accès à toutes les fonctionnalités de mise à jour de Windows|     Enabled|
|Dans le **mise à jour Windows** dossier, ouvrez **différer la mise à jour Windows**|||
||Sélectionnez lors de la réception des mises à jour de fonctionnalité|Activé, puis dans le **Options** zone, utilisez la **sélectionner le niveau de disponibilité de branche pour les mises à jour de fonctionnalité que vous souhaitez recevoir** menu déroulant pour sélectionner **Current Branch for Business**. Définir le **après la publication d’une mise à jour de fonctionnalité, différer la réception des il pour ce nombre de jours** spinbox à *180 jours*.
||Sélectionnez lors de la réception des mises à jour qualité|Activé, puis, dans le **Options** zone, le **après la publication d’une mise à jour de qualité, différer la réception des il pour ce nombre de jours** spinbox à *30 jours* et sélectionnez la case à cocher **Suspendre les mises à jour qualité**.

Dans le volet gauche de l’éditeur de stratégie de groupe locale, cliquez sur **Configuration utilisateur**. Le volet gauche, cliquez sur **modèles d’administration** , puis entrez chaque sous-dossier de paramètres suivants et ajuster les paramètres comme suit :

|Dossier paramètres sous **modèles d’administration**|Paramètre|Valeur recommandée pour une utilisation de l’infrastructure VDI|  
|-------------------|-------|----------|
|**Bureau**|||
||N’ajoutez pas de partages des documents récemment ouverts vers des emplacements réseau|Enabled|
|Dans le **Desktop** dossier, ouvrez **Active Directory**|||
||Taille maximale de recherches Active Directory|Activé, puis dans le **Options** zone, permet de définir le spinbox **nombre d’objets renvoyés** à *5000*.|
|**Manu de début et de la barre des tâches**|||
||Effacer la liste des programmes récents pour les nouveaux utilisateurs|     Enabled|
||Ne pas afficher ni suivre les éléments des listes de raccourcis à partir d'emplacements distants|        Enabled|
||Désactiver les bulles de notifications de fonctionnalités existantes|     Enabled|
||Désactiver le suivi de l’utilisateur|       Enabled|
|Dans le **Menu Démarrer et barre des tâches** dossier, ouvrez **Notifications**|||
||Désactiver les notifications par toast|Enabled|
|Dans le **les composants Windows** dossier, ouvrez :|||
|**Contenu de cloud**|||
||Désactiver toutes les fonctionnalités Windows à la une|Enabled|
|**Explorateur de fichiers**|||
||Désactiver la mise en cache des images miniatures|       Enabled|
||Désactiver l’affichage des entrées de recherche récents dans la zone de recherche de l’Explorateur de fichiers|        Enabled|
||Désactiver la mise en cache de miniatures dans le fichier caché thumbs.db|      Enabled|

## <a name="microsoft-store-apps"></a>Applications du Microsoft Store
Il existe un nombre d’applications Microsoft Store que vous pouvez supprimer à partir de l’image de l’infrastructure VDI ; les supprimer pour réduire l’utilisation du processeur et économiser l’espace disque. Bons candidats pour la suppression sont les suivantes :

- Obtenir Office
- Skype (version préliminaire)
- Prise en main (en particulier s’il n’y a aucune connexion Internet)
- Hub de commentaires
- Collection de Microsoft Solitaire
- Payant Wi-Fi et cellulaire

Pour personnaliser le profil utilisateur par défaut utilisé pour la création d’images VDI, utilisez le compte administrateur intégré. S’il n’est pas déjà activé, faire à l’aide d’utilisateurs et groupes locaux dans Gestion de l’ordinateur. Ensuite vous connecter au compte d’administrateur pour effectuer les étapes suivantes.

> [!NOTE]  
> Ne supprimez pas les applications système telles que l’application de Store. Ils sont difficiles à réinstaller. Autres applications sont facilement reinstallable à partir du Store.

### <a name="delete-unwanted-apps-from-the-administrator-user-profile"></a>Supprimer les applications indésirables à partir du profil d’utilisateur administrateur
1. Dans Windows PowerShell, exécutez `Get-AppxPackage | ft PackageFamilyName` pour afficher la liste des applications installées.
2. Pour chaque gestionnaire de package de l’application à désinstaller, exécutez les applets de commande de cet exemple de format :

    `Get-AppxPackage *messaging* | Remove-AppxPackage`

    `Get-AppxPackage *WindowsMaps* | Remove-AppxPackage`

    `Get-AppxPackage *ZuneMusic* | Remove-AppxPackage`

### <a name="delete-the-payload-of-unwanted-store-apps"></a>Supprimer la charge utile d’applications de Store indésirables
Cela empêche les applications de réinstallation.
1. Applications de Store de la liste et d’autres éléments qui ont configuré des données dans le stockage avec cette applet de commande : `Get-AppxProvisionedPackage -Online`.
2. Supprimer un package donné avec `Remove-AppxProvisionedPackage -Online -PackageName MyAppPackage`, à l’aide de la MyAppPackage approprié retourné à l’étape 1. Par exemple, pour supprimer le package de Zune, vous exécuteriez `Remove-AppxProvisionedPackage -Online -PackageName Microsoft.ZuneMusic_2019.17012.10311.0_neutral_~_8wekyb3d8bbwe`.

## <a name="removing-other-items"></a>Suppression d’autres éléments
Vous pouvez supprimer l’application et l’icône de OneDrive, désactiver les icônes système et mises à jour téléchargées.

### <a name="remove-onedrive-icon-and-app"></a>Supprimer l’application et l’icône de OneDrive
1. Cliquez sur **Démarrer** et faites défiler vers le **OneDrive** icône.
2. Cliquez sur le **OneDrive** icône, pointez sur **plus**, puis cliquez sur **ouvrir l’emplacement du fichier**.
3. Cliquez sur le **OneDrive** icône dans son emplacement de fichier, puis cliquez sur **supprimer**.

Pour supprimer l’application OneDrive :
1. Cliquez sur **Démarrer** et faites défiler vers le **OneDrive** icône.
2. Cliquez sur le **OneDrive** icône, puis cliquez sur **désinstallation**. Programmes et ouvre de fonctionnalités.
3. Dans programmes et fonctionnalités, cliquez sur **Microsoft OneDrive** et cliquez sur **désinstallation**.

### <a name="programs-and-features-from-previous-versions-of-control-panel"></a>Programmes et fonctionnalités (à partir de versions précédentes du Panneau de configuration)
1. Poussez le **Démarrer** bouton, tapez *contrôle*, puis appuyez sur ENTRÉE.
2. Cliquez ou double-cliquez sur **programmes et fonctionnalités**.
3. À l’extrême gauche, sous **accueil du Panneau de contrôle**, appuyez ou cliquez sur **ou désactiver des fonctionnalités Windows activer**. Une nouvelle interface utilisateur s’ouvre.
4. Désactivez les cases à cocher pour tous les éléments que vous ne veulent pas ou n’avez besoin dans l’image de base, par exemple : **Prise en charge de partage de fichiers SMB 1.0/CIFS**.

### <a name="turn-system-icons-off"></a>Désactiver les icônes système
1. Push ou cliquez sur **Démarrer**, puis cliquez sur **paramètres** (l’icône d’engrenage).
2. Dans le **trouver un paramètre** zone de texte, tapez *barre des tâches*, puis cliquez sur **paramètres de la barre des tâches**.
3. Sous le **barre des tâches** section, faites défiler ou faites défiler jusqu'à la **zone de Notification** section.
4. Cliquez ou appuyez sur **activer ou désactiver les icônes système**, puis activer chaque icône système ou désactiver comme vous le souhaitez pour l’image.

### <a name="delete-downloaded-updates"></a>Supprimer des mises à jour téléchargées
1. À l’aide de l’Explorateur de fichiers, accédez à **C:\Windows\Software Distribution\Download**.
2. Supprimer tous les fichiers et dossiers dans ce répertoire.













 













