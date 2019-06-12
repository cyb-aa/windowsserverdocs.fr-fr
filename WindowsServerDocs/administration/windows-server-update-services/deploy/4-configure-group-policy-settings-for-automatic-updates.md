---
title: 'Étape 4 : configurer les paramètres de stratégie de groupe pour les mises à jour automatiques'
description: Rubrique de Windows Server Update Service (WSUS) - configurer des paramètres de stratégie de groupe pour les mises à jour automatiques est l’étape 4 dans un processus en quatre étapes pour le déploiement de WSUS
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 62177d05-d832-4ea8-bca4-47a8cd34a19c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9205565486b75edcd550174fc89990a5aa2d69b7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439852"
---
# <a name="step-4-configure-group-policy-settings-for-automatic-updates"></a>Étape 4 : Configurer les paramètres de stratégie de groupe pour les mises à jour automatiques

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dans un environnement active directory, vous pouvez utiliser la stratégie de groupe pour définir comment les ordinateurs et utilisateurs (dans ce document en tant que les clients WSUS) peuvent interagir avec les mises à jour de Windows pour obtenir des mises à jour automatiques à partir de Windows Server Update Services (WSUS).

Cette rubrique contient deux sections principales :

[Paramètres Stratégie de groupe pour les mises à jour du client WSUS](#group-policy-settings-for-wsus-client-updates), qui fournit des recommandations et comportement des détails sur les paramètres de mise à jour de Windows et le Planificateur de Maintenance de la stratégie de groupe qui contrôlent comment les clients WSUS peuvent interagir avec la mise à jour de Windows Pour obtenir les mises à jour automatiques.

[Obtenir des informations supplémentaires](#supplemental-information) contient les sections suivantes :

-   [Les paramètres de mise à jour de Windows dans la stratégie de groupe de l’accès à](#accessing-the-windows-update-settings-in-group-policy), qui fournit des indications générales sur l’utilisation de l’éditeur de gestion de stratégie de groupe et d’informations sur l’accès aux extensions de stratégie de Services de mise à jour et les paramètres de planificateur de Maintenance dans Stratégie de groupe.

-   [Modifications au serveur WSUS pertinents pour ce guide](#changes-to-wsus-relevant-to-this-guide): pour les administrateurs familiarisés avec WSUS 3.2 et versions antérieures, cette section donne un bref résumé des principales différences entre la version actuelle et passée de WSUS pertinents pour ce guide.

-   [Termes et définitions](#terms-and-definitions): définitions pour différents termes se rapportant aux services WSUS et mise à jour qui sont utilisés dans ce guide.

## <a name="group-policy-settings-for-wsus-client-updates"></a>Paramètres de stratégie de groupe pour les mises à jour du client WSUS
Cette section fournit des informations sur trois extensions de stratégie de groupe. Dans ces extensions, vous trouverez les paramètres que vous pouvez utiliser pour configurer comment les clients WSUS peuvent interagir avec la mise à jour de Windows pour recevoir des mises à jour automatiques.

-   [Configuration d’ordinateur &gt; les paramètres de stratégie de mise à jour de Windows](#computer-configuration--windows-update-policy-settings)

-   [Configuration d’ordinateur &gt; les paramètres de stratégie du Planificateur de Maintenance](#computer-configuration--maintenance-scheduler-policy-settings)

-   [Configuration de l’utilisateur &gt; les paramètres de stratégie de mise à jour de Windows](#user-configuration--windows-update-policy-settings)

> [!NOTE]
> Cette rubrique suppose que vous avez déjà utiliserez et que vous êtes familiarisé avec la stratégie de groupe. Si vous n’êtes pas familiarisé avec la stratégie de groupe, il est recommandé que vous passez en revue les informations contenues dans le [des informations supplémentaires](#supplemental-information) section de ce document avant de tenter de configurer les paramètres de stratégie pour WSUS.

### <a name="computer-configuration--windows-update-policy-settings"></a>Configuration ordinateur > Paramètres de stratégie de mise à jour de Windows
Cette section fournit des détails sur les paramètres de stratégie ordinateur suivants :

-   [Autoriser l’installation immédiate des mises à jour automatiques](#allow-automatic-updates-immediate-installation)

-   [Autoriser des non-administrateurs à recevoir des notifications de mise à jour](#allow-non-administrators-to-receive-update-notifications)

-   [Autoriser les mises à jour signées à partir d’un emplacement de service de mise à jour Microsoft de l’intranet](#allow-signed-updates-from-an-intranet-microsoft-update-service-location)

-   [Fréquence de détection automatique des mises à jour](#automatic-updates-detection-frequency)

-   [Configurer les mises à jour automatiques](#configure-automatic-updates)

-   [Délai de redémarrage pour les installations planifiées](#delay-restart-for-scheduled-installations)

-   [Option par défaut « Installer les mises à jour et arrêter » dans la boîte de dialogue Arrêter bas Windows ne s’ajustent pas](#do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog)

-   [Ne pas afficher l’option « Installer les mises à jour et éteindre » dans la boîte de dialogue Arrêter bas Windows](#do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog)

-   [Autoriser le ciblage côté client](#enable-client-side-targeting)

-   [L’activation de Windows mise à jour de gestion de l’alimentation sortir de veille automatiquement l’ordinateur pour installer les mises à jour planifiées](#enabling-windows-update-power-management-to-automatically-wake-up-the-computer-to-install-scheduled-updates)

-   [Pas de redémarrage automatique avec des utilisateurs connectés pour automatique planifiée met à jour des installations](#no-auto-restart-with-logged-on-users-for-scheduled-automatic-updates-installations)

-   [Redemander un redémarrage avec les installations planifiées](#re-prompt-for-restart-with-scheduled-installations)

-   [Replanifier les installations planifiées des mises à jour automatiques](#reschedule-automatic-updates-scheduled-installations)

-   [Spécifier l’emplacement intranet du service de mise à jour Microsoft](#specify-intranet-microsoft-update-service-location)

-   [Activer les mises à jour recommandées par le biais de mises à jour automatiques](#turn-on-recommended-updates-via-automatic-updates)

-   [Activer les Notifications de logiciel](#turn-on-software-notifications)

Dans le GPME, les stratégies de mise à jour de Windows pour la configuration basée sur l’ordinateur sont situés dans le chemin d’accès : *PolicyName* > **Configuration ordinateur** > **stratégies** > **modèles d’administration**  >  **Les composants Windows** > **mise à jour Windows**.

> [!NOTE]
> Par défaut, ces paramètres ne sont pas configurés.

#### <a name="allow-automatic-updates-immediate-installation"></a>Autoriser l’installation immédiate des mises à jour automatiques
Spécifie si les mises à jour automatiques installe automatiquement les mises à jour sans interruption des services de Windows ou redémarrez Windows.

|Pris en charge :|À l’exclusion :|
|---------|-------|
|Systèmes d’exploitation Windows qui sont toujours dans leur [politique de Support des produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Null|

> [!NOTE]
> Si le paramètre de stratégie « Configurer les mises à jour automatique » est défini **désactivé**, cette stratégie n’a aucun effet.

|||
|-|-|
|**État de paramètre de stratégie**|**Behavior**|
|**Non configuré**|Spécifie que les mises à jour ne sont pas installées immédiatement. Les administrateurs locaux peuvent modifier ce paramètre à l’aide de l’éditeur de stratégie de groupe locale.|
|**Activé**|Spécifie que les mises à jour automatique installe immédiatement mises à jour une fois qu’ils sont téléchargés et prêts à installer.|
|**Désactivé**|Spécifie que les mises à jour ne sont pas installées immédiatement.|

**Options :** Il n’existe aucune option pour ce paramètre.

#### <a name="allow-non-administrators-to-receive-update-notifications"></a>Autoriser des non-administrateurs à recevoir des notifications de mise à jour
Spécifie si les utilisateurs non-administrateurs doit recevoir des notifications de mise à jour en fonction du paramètre de stratégie de configurer des mises à jour automatique.

|Pris en charge :|À l’exclusion :|
|---------|-------|
|Systèmes d’exploitation Windows qui sont toujours dans leur [politique de Support des produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Afficher les détails dans le tableau ci-dessous.|

> [!NOTE]
> Si le paramètre de stratégie « Configurer les mises à jour automatique » est désactivé ou n’est pas configuré, ce paramètre de stratégie n’a aucun effet.

> [!IMPORTANT]
> à compter de Windows 8 et Windows RT, ce paramètre de stratégie est activé par défaut. Dans toutes les versions précédentes de Windows, il est désactivé par défaut.

|||
|-|-|
|**État de paramètre de stratégie**|**Behavior**|
|**Non configuré**|Spécifie que les utilisateurs seront toujours voir une fenêtre contrôle de compte et nécessitent des autorisations élevées pour effectuer ces tâches. Un administrateur local peut modifier ce paramètre à l’aide de l’éditeur de stratégie de groupe locale.|
|**Activé**|Spécifie que la mise à jour automatique de Windows et Microsoft Update inclura les non-administrateurs lors de la détermination de l’utilisateur connecté destinée à recevoir les notifications de mise à jour. Les utilisateurs non administratifs sera en mesure d’installer tout le contenu facultatif, recommandée et importante mise à jour pour lequel il a reçu une notification. Les utilisateurs ne verront pas une fenêtre contrôle de compte d’utilisateur et des autorisations élevées pour installer ces mises à jour, sauf dans le cas de mises à jour contenant les modifications apportées aux paramètres d’Interface utilisateur, contrat de licence utilisateur final ou Windows Update n’a pas besoin.<br /><br />Il existe deux situations où l’effet de ce paramètre dépend de l’ordinateur d’exploitation :<br /><br />1.  **Masquer** ou **restaurer** mises à jour<br />2.  **Annuler** une installation de mise à jour<br /><br />Dans Windows Vista ou Windows XP, si ce paramètre de stratégie est activé, les utilisateurs ne verront pas une fenêtre contrôle de compte d’utilisateur et sans avoir besoin d’autorisations élevées pour masquer, restaurer ou annuler les mises à jour.<br /><br />Dans Windows Vista, si ce paramètre de stratégie est activé, les utilisateurs ne verront pas une fenêtre contrôle de compte d’utilisateur et sans avoir besoin d’autorisations élevées pour masquer, restaurer ou annuler les mises à jour. Si ce paramètre de stratégie n’est pas activé, les utilisateurs seront affiche toujours une fenêtre contrôle de compte, et ils nécessitent des autorisations élevées pour masquer, restaurer ou annuler les mises à jour.<br /><br />Dans Windows 7, ce paramètre de stratégie n’a aucun effet. Les utilisateurs seront affiche toujours une fenêtre contrôle de compte, et ils nécessitent des autorisations élevées pour effectuer ces tâches.<br /><br />Dans Windows 8 et Windows RT, ce paramètre de stratégie n’a aucun effet.|
|**Désactivé**|Spécifie que les administrateurs uniquement connecté à recevoir des notifications de mise à jour. **Remarque :** Dans Windows 8 et Windows RT, ce paramètre de stratégie est activé par défaut. Dans toutes les versions précédentes de Windows, il est désactivé par défaut.|

**Options :** Il n’existe aucune option pour ce paramètre.

#### <a name="allow-signed-updates-from-an-intranet-microsoft-update-service-location"></a>Autoriser les mises à jour signées d'un emplacement intranet du service de mise à jour Microsoft
Spécifie si les mises à jour automatiques accepte les mises à jour qui sont signées par des entités autres que Microsoft lors de la mise à jour se trouve sur un emplacement intranet du service de mise à jour Microsoft.

|Pris en charge :|À l’exclusion :|
|---------|-------|
|Systèmes d’exploitation Windows qui sont toujours dans leur [politique de Support des produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!NOTE]
> Mises à jour à partir d’un service autre que d’un service intranet de mise à jour Microsoft doivent toujours être signés par Microsoft, et ils ne sont pas affectés par ce paramètre de stratégie.

> [!NOTE]
> Cette stratégie n’est pas pris en charge sur RT de Windows. L’activation de cette stratégie n’aura pas d’effet sur les ordinateurs exécutant RT de Windows.

**Options :** Il n’existe aucune option pour ce paramètre.

|||
|-|-|
|**État de paramètre de stratégie**|**Behavior**|
|**Non configuré**|Spécifie que les mises à jour à partir d’un emplacement de service de mise à jour Microsoft intranet doivent être signés par Microsoft.|
|**Activé**|Spécifie que les mises à jour automatiques accepte les mises à jour reçues via un intranet emplacement du service Microsoft update s’ils sont signés par un certificat trouvé dans le magasin de certificats « Éditeurs approuvés » de l’ordinateur local.|
|**Désactivé**|Spécifie que les mises à jour à partir d’un emplacement de service de mise à jour Microsoft intranet doivent être signés par Microsoft.|

**Options :** Il n’existe aucune option pour ce paramètre.

#### <a name="always-automatically-restart-at-the-scheduled-time"></a>Toujours redémarrer automatiquement à l’heure planifiée
Spécifie si une horloge de redémarrage commence toujours immédiatement après que la mise à jour Windows installe des mises à jour importantes, au lieu des premiers utilisateurs de notification sur l’écran de connexion pendant au moins deux jours.

|Pris en charge :|À l’exclusion :|
|---------|-------|
|Systèmes d’exploitation Windows qui sont toujours dans leur [politique de Support des produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Null|

> [!NOTE]
> Si le paramètre de stratégie « pas de redémarrage automatique avec des utilisateurs connectés pour automatique planifiée mises à jour les installations » est activé, cette stratégie n’a aucun effet.

|||
|-|-|
|**État de paramètre de stratégie**|**Behavior**|
|**Non configuré**|Spécifie que la mise à jour de Windows ne modifie pas le comportement de redémarrage de l’ordinateur.|
|**Activé**|Spécifie qu’une horloge de redémarrage commence toujours immédiatement après que la mise à jour Windows installe des mises à jour importantes, au lieu des premiers utilisateurs de notification sur l’écran de connexion pendant au moins deux jours.<br /><br />L’horloge de redémarrage peut être configuré pour démarrer avec n’importe quelle valeur de 15 à 180 minutes. Lorsque la minuterie est écoulé, le redémarrage sera effectuée même si l’ordinateur a des utilisateurs connectés.|
|**Désactivé**|Spécifie que la mise à jour de Windows ne modifie pas le comportement de redémarrage de l’ordinateur.|

**Options :** si ce paramètre est activé, vous pouvez spécifier la quantité de temps qui doit s’écouler après l’installent des mises à jour avant un redémarrage forcé se produit.

#### <a name="automatic-updates-detection-frequency"></a>Fréquence de détection des mises à jour automatiques
Spécifie la durée en heures pendant laquelle Windows attendra avant de vérifier la disponibilité de nouvelles mises à jour. La durée exacte est déterminée en utilisant ce nombre d’heures moins un pourcentage compris entre zéro et vingt pour-cent du nombre d’heures spécifié. Par exemple, si cette stratégie est utilisée pour spécifier une fréquence de détection de 20 heures, tous les clients auxquels cette stratégie est appliquée vérifiera les mises à jour n’importe où entre 16 et 20 heures.

|Pris en charge :|À l’exclusion :|
|---------|-------|
|Systèmes d’exploitation Windows qui sont toujours dans leur [politique de Support des produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!NOTE]
> Le paramètre « Spécifier intranet emplacement de service de mise à jour Microsoft » doit être activé pour que l’effet de cette stratégie.
>
> Si le paramètre de stratégie « Configurer les mises à jour automatique » est désactivée, cette stratégie n’a aucun effet.

> [!NOTE]
> Cette stratégie n’est pas pris en charge sur RT de Windows. L’activation de cette stratégie n’aura pas d’effet sur les ordinateurs exécutant RT de Windows.

|||
|-|-|
|**État de paramètre de stratégie**|**Behavior**|
|**Non configuré**|Spécifie que Windows rechercheront des mises à jour disponibles à l’intervalle par défaut de 22 heures.|
|**Activé**|Spécifie que Windows rechercheront des mises à jour disponibles à l’intervalle spécifié.|
|**Désactivé**|Spécifie que Windows rechercheront des mises à jour disponibles à l’intervalle par défaut de 22 heures.|

**Options :** si ce paramètre est activé, vous pouvez spécifier l’intervalle de temps (en heures), mise à jour de Windows attend avant de vérifier les mises à jour.

#### <a name="configure-automatic-updates"></a>Configurer les mises à jour automatiques
Spécifie spécifier si les mises à jour automatiques sont activées sur cet ordinateur.

|Pris en charge :|À l’exclusion :|
|---------|-------|
|Systèmes d’exploitation Windows qui sont toujours dans leur [politique de Support des produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

Si activé, vous devez sélectionner une des quatre options proposées dans ce paramètre de stratégie de groupe.

Pour utiliser ce paramètre, sélectionnez **activé**, puis dans **Options** sous **configurer la mise à jour automatique**, sélectionnez une des options (2, 3, 4 ou 5).

|||
|-|-|
|**État de paramètre de stratégie**|**Behavior**|
|**Non configuré**|Spécifie que l’utilisation de mises à jour automatiques n’est pas spécifiée au niveau de la stratégie de groupe. Toutefois, un administrateur de l’ordinateur peut toujours configurer des mises à jour automatiques dans le panneau de configuration.|
|**Activé**|Spécifie que Windows reconnaît lorsque l’ordinateur est en ligne et utilise sa connexion Internet pour rechercher des mises à jour de Windows pour les mises à jour disponibles.<br /><br />Lorsque l’option est activée, les administrateurs locaux pourront pour utiliser le panneau de configuration de mise à jour de Windows pour sélectionner une option de configuration de leur choix. Toutefois, les administrateurs locaux ne pourrez pas désactiver la configuration pour les mises à jour automatiques.<br /><br />-   **2 - notification de téléchargement et notification des installations**<br />    Lors de la mise à jour de Windows recherche les mises à jour qui s’appliquent à l’ordinateur, les utilisateurs seront informés que les mises à jour sont prêtes pour le téléchargement. Les utilisateurs peuvent exécuter puis mise à jour de Windows pour télécharger et installer les mises à jour disponibles.<br />-   **3 - téléchargement automatique et notification des installations** (paramètre par défaut)<br />    Mise à jour de Windows recherche les mises à jour applicables et les télécharge en arrière-plan ; l’utilisateur n’est pas notifié ou interrompu au cours du processus. Lorsque les téléchargements sont terminés, les utilisateurs sont avertis qu’il n’y a prêts à installer les mises à jour. Les utilisateurs peuvent exécuter puis mise à jour de Windows pour installer les mises à jour téléchargées.<br />-   **4 - téléchargement automatique et planification des installations**<br />    Vous pouvez spécifier la planification en utilisant les options de ce paramètre de stratégie de groupe. Si aucune planification n’est spécifiée, le calendrier par défaut pour toutes les installations seront tous les jours à 3 h 00. Si les mises à jour nécessitent un redémarrage pour terminer l’installation, Windows redémarre automatiquement l’ordinateur. (si un utilisateur est connecté à l’ordinateur lorsque Windows est prêt à redémarrer, l’utilisateur sera être informé et la possibilité de retarder le redémarrage.) **Remarque :** à partir de Windows 8, vous pouvez définir des mises à jour à installer lors de la maintenance automatique au lieu d’utiliser une planification spécifique liée à la mise à jour de Windows. Maintenance automatique installera les mises à jour lorsque l’ordinateur n’est pas en cours d’utilisation et éviter d’installer les mises à jour lorsque l’ordinateur fonctionne sur batterie. Si la maintenance automatique ne peut pas installer les mises à jour dans les jours, mise à jour de Windows Installer les mises à jour immédiatement. Les utilisateurs seront ensuite être avertis d’un redémarrage en attente. Un redémarrage en attente a lieu uniquement s’il n’existe aucun risque de perte accidentelle de données.    Vous pouvez spécifier des options de planification dans les paramètres du Planificateur de Maintenance GPME, qui sont trouvent dans le chemin d’accès, *PolicyName* > **ordinateur Configuration**  >  **Stratégies** > **modèles d’administration** > **les composants Windows** > **Maintenance Planificateur** > **limite de l’Activation de la Maintenance automatique**. Consultez la section de cette référence intitulée : [Paramètres du Planificateur de maintenance](#computer-configuration--maintenance-scheduler-policy-settings), pour les détails des paramètres.    **5 - autoriser l’administrateur local à choisir les paramètres**<br />-Spécifie si les administrateurs locaux sont autorisés à utiliser le panneau de configuration mises à jour automatiques pour sélectionner une option de configuration de leur choix, par exemple, si les administrateurs locaux peuvent choisir une heure d’installation planifiée.<br />    Les administrateurs locaux ne seront pas autorisés à désactiver la configuration des mises à jour automatiques.|
|**Désactivé**|Spécifie que les mises à jour du client sont disponibles à partir du service de mise à jour Windows publique doivent être téléchargés à partir d’Internet manuellement et installés.|

#### <a name="delay-restart-for-scheduled-installations"></a>Délai de redémarrage pour les installations planifiées
Spécifie la durée de qu'attente avant de procéder à un redémarrage planifié mises à jour automatiques.

|Pris en charge :|À l’exclusion :|
|---------|-------|
|Systèmes d’exploitation Windows qui sont toujours dans leur [politique de Support des produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Null|

> [!NOTE]
> Cette stratégie s’applique uniquement lorsque les mises à jour automatiques est configuré pour effectuer des installations de mises à jour planifiées. Si le paramètre de stratégie « Configurer les mises à jour automatique » est désactivé, cette stratégie n’a aucun effet.

|||
|-|-|
|**État de paramètre de stratégie**|**Behavior**|
|**Non configuré**|Spécifie qu’une fois les mises à jour sont installées, le temps d’attente par défaut de quinze minutes doit s’écouler avant tout redémarrage planifié se produit.|
|**Activé**|Spécifie que lorsque l’installation est terminée, un redémarrage planifié se produira une fois que le nombre de minutes spécifié a expiré.|
|**Désactivé**|Spécifie qu’une fois les mises à jour sont installées, le temps d’attente par défaut de quinze minutes doit s’écouler avant tout redémarrage planifié se produit.|

**Options :** si ce paramètre est activé, vous pouvez utiliser cette option pour spécifier la quantité de temps (en minutes) des mises à jour automatiques attend avant de procéder à un redémarrage planifié.

#### <a name="do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog"></a>Option par défaut pour installer les mises à jour et arrêter dans la boîte de dialogue Arrêter bas Windows ne s’ajustent pas
Ce paramètre de stratégie vous permet de spécifier si le **installer les mises à jour et arrêter** option est autorisée en tant que le choix par défaut dans le **arrêté bas Windows** boîte de dialogue.

|Pris en charge :|À l’exclusion :|
|---------|-------|
|Systèmes d’exploitation Windows qui sont toujours dans leur [politique de Support des produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Null|

> [!NOTE]
> Ce paramètre de stratégie n’a aucun impact si le *PolicyName* > **ordinateur Configuration** > **stratégies**  >  **Modèles d’administration** > **les composants Windows** > **mise à jour Windows** > **ne s’affichent pas » Installer les mises à jour et arrêter » une option dans la boîte de dialogue Arrêter bas Windows** paramètre de stratégie est activé.

|||
|-|-|
|**État de paramètre de stratégie**|**Behavior**|
|**Non configuré**|Spécifie que **installer les mises à jour et arrêter** sera l’option par défaut dans le **arrêté bas Windows** boîte de dialogue si les mises à jour sont disponibles pour l’installation au moment où l’utilisateur sélectionne l’option Arrêter à arrêter l’ordinateur.|
|**Activé**|Si vous activez ce paramètre de stratégie, dernière option d’arrêt l’utilisateur (par exemple, mise en veille prolongée ou redémarrer), est l’option par défaut dans le **arrêté bas Windows** boîte de dialogue, indépendamment de si le **installer les mises à jour et arrêter**  option est disponible dans le **que voulez-vous faire ?** menu.|
|**Désactivé**|Spécifie que **installer les mises à jour et arrêter** sera l’option par défaut dans le **arrêté bas Windows** boîte de dialogue si les mises à jour sont disponibles pour l’installation au moment où l’utilisateur sélectionne l’option Arrêter à arrêter l’ordinateur.|

**Options :** Il n’existe aucune option pour ce paramètre.

#### <a name="do-not-connect-to-any-windows-update-internet-locations"></a>Ne pas se connecter à des emplacements Internet Windows Update
Même lorsque la mise à jour de Windows est configurée pour recevoir des mises à jour à partir d’un service intranet de mise à jour, il sera périodiquement récupérer les informations dans le service public de mettre à jour de Windows pour activer les connexions futures pour mettre à jour de Windows et d’autres services, tels que Microsoft Update ou de Microsoft Store.

L’activation de cette stratégie désactive la fonctionnalité permettant de récupérer régulièrement les informations à partir de Windows Server Update Service public, et il peut ralentir les connexions des services publics tels que Microsoft Store pour cesser de fonctionner.

|Pris en charge :|À l’exclusion :|
|---------|-------|
|en commençant par Windows Server 2012 R2, Windows 8.1 ou Windows RT 8.1, systèmes d’exploitation Windows qui sont toujours dans leur [politique de Support des produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Null|

> [!NOTE]
> Cette stratégie s’applique uniquement lorsque l’ordinateur est configuré pour se connecter à un service intranet de mise à jour en utilisant le paramètre de stratégie « Spécifier intranet Microsoft mise à jour l’emplacement service ».

|||
|-|-|
|**État de paramètre de stratégie**|**Behavior**|
|**Non configuré**|Le comportement par défaut pour récupérer des informations à partir de Windows Server Update Service public persiste.|
|**Activé**|Spécifie que les ordinateurs ne récupérera pas d’informations à partir de Windows Server Update Service public.|
|**Désactivé**|Le comportement par défaut pour récupérer des informations à partir de Windows Server Update Service public persiste.|

**Options :** Il n’existe aucune option pour ce paramètre.

#### <a name="do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog"></a>Ne pas afficher les mises à jour de l’installation et l’option Arrêter dans la boîte de dialogue Arrêter bas Windows
Spécifie si le **installer les mises à jour et arrêter** option est affichée dans le **arrêté bas Windows** boîte de dialogue.

|Pris en charge :|À l’exclusion :|
|---------|-------|
|Systèmes d’exploitation Windows qui sont toujours dans leur [politique de Support des produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Null|

|||
|-|-|
|**État de paramètre de stratégie**|**Behavior**|
|**Non configuré**|Spécifie que le **installer les mises à jour et arrêter** option est disponible dans le **arrêté bas Windows** boîte de dialogue si les mises à jour sont disponibles lorsque l’utilisateur sélectionne l’option Arrêter pour arrêter l’ordinateur. Un administrateur local peut modifier ce paramètre à l’aide de stratégie locale.|
|**Activé**|Spécifie que **installer les mises à jour et arrêter** n’apparaît pas comme un choix dans le **arrêté bas Windows** boîte de dialogue, même si les mises à jour sont disponibles pour l’installation lorsque l’utilisateur sélectionne l’option Arrêter à arrêter l’ordinateur.|
|**Désactivé**|Spécifie que le **installer les mises à jour et arrêter** option n’est pas l’option par défaut dans le **arrêté bas Windows** boîte de dialogue si les mises à jour sont disponibles pour l’installation au moment où l’utilisateur sélectionne l’arrêter option permettant d’arrêter l’ordinateur.|

**Options :** Il n’existe aucune option pour ce paramètre.

#### <a name="enable-client-side-targeting"></a>Activer le ciblage côté client
Spécifie le nom du groupe cible ou les noms qui sont configurés dans la console WSUS qui doivent recevoir des mises à jour à partir de WSUS.

|Pris en charge :|À l’exclusion :|
|---------|-------|
|Systèmes d’exploitation Windows qui sont toujours dans leur [politique de Support des produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!NOTE]
> Cette stratégie s’applique uniquement lorsque cet ordinateur est configuré pour prendre en charge les noms de groupe cible spécifié dans WSUS. Si le nom du groupe cible n’existe pas dans WSUS, il sera ignoré jusqu'à ce qu’il est créé. Si le paramètre de stratégie « Spécifier intranet emplacement de service de mise à jour Microsoft » est désactivé ou non configuré, cette stratégie n’a aucun effet.

> [!NOTE]
> Cette stratégie n’est pas pris en charge sur RT de Windows. L’activation de cette stratégie n’aura pas d’effet sur les ordinateurs exécutant RT de Windows.

|||
|-|-|
|**État de paramètre de stratégie**|**Behavior**|
|**Non configuré**|Spécifie qu’aucune information de groupe cible n’est envoyée au serveur WSUS. Un administrateur local peut modifier ce paramètre à l’aide d’une stratégie locale.|
|**Activé**|Spécifie que les informations de groupe cible spécifié sont envoyées au serveur WSUS, ce qui les utilise pour déterminer quelles mises à jour doivent être déployées sur cet ordinateur. Si WSUS prend en charge plusieurs groupes cibles, vous pouvez utiliser cette stratégie pour spécifier plusieurs noms de groupe, séparés par des points-virgules, si vous avez ajouté les noms de groupe cible dans la liste de groupes d’ordinateurs dans WSUS. Dans le cas contraire, un seul groupe doit être indiqué.|
|**Désactivé**|Spécifie qu’aucune information de groupe cible n’est envoyée au serveur WSUS.|

**Options :** Utilisez cet espace pour spécifier un ou plusieurs noms de groupe cible.

#### <a name="enabling-windows-update-power-management-to-automatically-wake-up-the-computer-to-install-scheduled-updates"></a>L’activation de Windows mise à jour de gestion de l’alimentation sortir de veille automatiquement l’ordinateur pour installer les mises à jour planifiées
Spécifie si Windows Update utilise les fonctionnalités de gestion de l’alimentation Windows ou des Options d’alimentation pour sortir de veille automatiquement l’ordinateur à partir de la mise en veille prolongée s’il existe des mises à jour de l’installation planifiées.

L’ordinateur sera sortir de veille automatiquement uniquement si la mise à jour de Windows est configuré pour installer les mises à jour automatiquement. Si l’ordinateur est en veille prolongée lors de l’heure d’installation planifiée se produit et il existe des mises à jour à appliquer, mise à jour de Windows utilise les fonctionnalités de gestion de l’alimentation Windows ou des Options d’alimentation pour sortir de veille automatiquement l’ordinateur pour installer les mises à jour. Mettre à jour de Windows sera également sortir les ordinateurs et d’installer une mise à jour si l’échéance d’installation se produit.

L’ordinateur ne sera pas sortir sauf s’il existe des mises à jour à installer. Si l’ordinateur fonctionne sur batterie, lors de la mise à jour de Windows il sort de veille, il n’installera pas les mises à jour et l’ordinateur sera automatiquement renvoyé à la mise en veille prolongée dans deux minutes.

|Pris en charge :|À l’exclusion :|
|---------|-------|
|en commençant par Windows Vista et Windows Server 2008 (Windows 7), systèmes d’exploitation Windows qui sont toujours dans leur [politique de Support des produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Null|

|||
|-|-|
|**État de paramètre de stratégie**|**Behavior**|
|**Non configuré**|Mise à jour de Windows ne sort pas de veille l’ordinateur à partir de la mise en veille prolongée pour installer les mises à jour. Un administrateur local peut modifier ce paramètre à l’aide d’une stratégie locale.|
|**Activé**|Mise à jour Windows sort l’ordinateur à partir de la mise en veille prolongée pour installer les mises à jour dans les conditions répertoriées précédemment.|
|**Désactivé**|Mise à jour de Windows ne sort pas de veille l’ordinateur à partir de la mise en veille prolongée pour installer les mises à jour.|

**Options :** Il n’existe aucune option pour ce paramètre.

#### <a name="no-auto-restart-with-logged-on-users-for-scheduled-automatic-updates-installations"></a>Pas de redémarrage automatique avec des utilisateurs connectés pour les installations planifiées de mises à jour automatiques
Indique que pour effectuer une installation planifiée, mises à jour automatiques attend le redémarrage par tout utilisateur qui n’est connecté, au lieu de provoquer l’ordinateur à redémarrer automatiquement l’ordinateur.

|Pris en charge :|À l’exclusion :|
|---------|-------|
|Systèmes d’exploitation Windows qui sont toujours dans leur [politique de Support des produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Null|

> [!NOTE]
> Cette stratégie s’applique uniquement lorsque les mises à jour automatiques est configuré pour effectuer des installations de mises à jour planifiées. Si le paramètre de stratégie « Configurer les mises à jour automatique » est désactivé, cette stratégie n’a aucun effet.

|||
|-|-|
|**État de paramètre de stratégie**|**Behavior**|
|**Non configuré**|Spécifie que les mises à jour automatiques informent l’utilisateur que l’ordinateur sera automatiquement redémarré dans les cinq minutes pour terminer l’installation.|
|**Activé**|Certaines mises à jour nécessitent l’ordinateur pour être redémarré pour que les mises à jour prendra effet. Si l’état est défini sur activé, mises à jour automatiques ne redémarre pas un ordinateur automatiquement pendant une installation planifiée si un utilisateur est connecté à l’ordinateur. Au lieu de cela, les mises à jour automatiques informera l’utilisateur à redémarrer l’ordinateur.|
|**Désactivé**|Spécifie que les mises à jour automatiques informent l’utilisateur que l’ordinateur sera automatiquement redémarré dans les cinq minutes pour terminer l’installation.|

**Options :** Il n’existe aucune option pour ce paramètre.

#### <a name="re-prompt-for-restart-with-scheduled-installations"></a>Redemander un redémarrage avec les installations planifiées
Spécifie la quantité de temps pour les mises à jour automatiques à attendre avant de redemander redémarrage planifié.

|Pris en charge :|À l’exclusion :|
|---------|-------|
|Systèmes d’exploitation Windows qui sont toujours dans leur [politique de Support des produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!IMPORTANT]
> Cette stratégie s’applique uniquement lorsque les mises à jour automatiques est configuré pour effectuer des installations de mises à jour planifiées. Si le paramètre de stratégie « Configurer les mises à jour automatique » est désactivé, cette stratégie n’a aucun effet.

> [!NOTE]
> Cette stratégie n’a aucun effet sur les ordinateurs exécutant RT de Windows.

|||
|-|-|
|**État de paramètre de stratégie**|**Behavior**|
|**Non configuré**|Un redémarrage planifié se produit après le redémarrage de l’invite pour les dix minutes message disparaît. Un administrateur local peut modifier ce paramètre à l’aide d’une stratégie locale.|
|**Activé**|Spécifie que, après que la première demande de redémarrage a été reporté à plus tard, un redémarrage planifié se produit après le nombre spécifié de l’expiration de minutes.|
|**Désactivé**|Un redémarrage planifié se produit après le redémarrage de l’invite pour les dix minutes message disparaît.|

**Options :** Lorsque l’option est activée, vous pouvez utiliser cette option de paramètre pour spécifier (en minutes) de la durée de temps qui doit s’écouler avant que les utilisateurs sont invités à nouveau sur un redémarrage planifié.

#### <a name="reschedule-automatic-updates-scheduled-installations"></a>Replanifier les installations planifiées des mises à jour automatiques
Spécifie la quantité de temps pour les mises à jour automatiques d’attente après un démarrage de l’ordinateur, avant de procéder à une installation planifiée qui a été manquée précédemment.

Si l’état est défini sur **pas configuré**, une installation planifiée manquée se produira une minute après l’ordinateur suivant démarré.

|Pris en charge :|À l’exclusion :|
|---------|-------|
|Systèmes d’exploitation Windows qui sont toujours dans leur [politique de Support des produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Null|

> [!NOTE]
> Cette stratégie s’applique uniquement lorsque les mises à jour automatiques est configuré pour effectuer des installations de mises à jour planifiées. Si le paramètre de stratégie « Configurer les mises à jour automatique » est désactivé, cette stratégie n’a aucun effet.

|||
|-|-|
|**État de paramètre de stratégie**|**Behavior**|
|**Non configuré**|Spécifie qu’une installation planifiée manquée produira une minute après l’ordinateur suivant démarré.|
|**Activé**|Spécifie qu’une installation planifiée qui n’a pas eu lieu précédemment se produira le nombre de minutes après le prochain démarrage de l’ordinateur spécifié.|
|**Désactivé**|Spécifie qu’une installation planifiée manquée se produira avec la prochaine installation planifiée.|

**Options :** Lorsque ce paramètre de stratégie est activé, vous pouvez l’utiliser pour spécifier un nombre de minutes après le prochain démarrage de l’ordinateur, qu’une installation planifiée ne prenait pas place plus haut se produit.

#### <a name="specify-intranet-microsoft-update-service-location"></a>Spécifier l’emplacement intranet du service de mise à jour Microsoft
Spécifie un serveur intranet qui héberge les mises à jour provenant de Microsoft Update. Vous pouvez ensuite utiliser WSUS pour mettre à jour automatiquement les ordinateurs sur votre réseau.

|Pris en charge :|À l’exclusion :|
|---------|-------|
|Systèmes d’exploitation Windows qui sont toujours dans leur [politique de Support des produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT.|

Ce paramètre vous permet de spécifier un serveur WSUS sur votre réseau qui ne peut fonctionner comme un service de mise à jour interne. Au lieu d’utiliser Microsoft Updates sur Internet, les clients WSUS recherchera ce service pour les mises à jour qui s’appliquent.

Pour utiliser ce paramètre, vous devez définir deux valeurs de nom de serveur : le serveur à partir duquel le client détecte et télécharge les mises à jour et le serveur à laquelle les statistiques de téléchargement des stations de travail mis à jour. Les valeurs ne sont pas nécessairement différents si les deux services sont configurés sur le même serveur.

> [!NOTE]
> Si le paramètre de stratégie « Configurer les mises à jour automatique » est désactivé, cette stratégie n’a aucun effet.

> [!NOTE]
> Cette stratégie n’est pas pris en charge sur RT de Windows. L’activation de cette stratégie n’aura pas d’effet sur les ordinateurs exécutant RT de Windows.

|||
|-|-|
|**État de paramètre de stratégie**|**Behavior**|
|**Non configuré**|Si les mises à jour automatiques n’est pas désactivé par une stratégie ou une préférence utilisateur, cette stratégie spécifie que les clients se connectent directement au site Windows Update sur Internet.|
|**Activé**|Spécifie que le client se connecte au serveur WSUS spécifié, au lieu de la mise à jour de Windows, pour rechercher et télécharger les mises à jour. L’activation de ce paramètre signifie que les utilisateurs finaux de votre organisation n’ont pas à passer par un pare-feu pour obtenir des mises à jour, et vous donne l’occasion de tester les mises à jour avant de les déployer.|
|**Désactivé**|Si les mises à jour automatiques n’est pas désactivé par une stratégie ou une préférence utilisateur, cette stratégie spécifie que les clients se connectent directement au site Windows Update sur Internet.|

**Options :** Lorsque ce paramètre de stratégie est activé, vous devez spécifier le service intranet de mise à jour que les clients WSUS utilisera lors de la détection des mises à jour, et le serveur de statistiques Internet auquel mis à jour les clients WSUS téléchargera les statistiques. Exemples de valeurs


|                    Option de paramètre :                    |    Exemple de valeur :    |
|-------------------------------------------------------|----------------------|
| Configurer le service intranet de mise à jour pour la détection des mises à jour |  http://wsus01:8530  |
|          Configurer le serveur intranet de statistiques           | http://IntranetUpd01 |

#### <a name="turn-on-recommended-updates-via-automatic-updates"></a>Activer les mises à jour recommandées par le biais de mises à jour automatiques
Spécifie si les mises à jour automatiques fournira IMPORTANT et mises à jour recommandées à partir de WSUS.

|Pris en charge :|À l’exclusion :|
|---------|-------|
|à partir de Windows Vista, systèmes d’exploitation Windows qui sont toujours dans leur [politique de Support des produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Null|

|||
|-|-|
|**État de paramètre de stratégie**|**Behavior**|
|**Non configuré**|Spécifie que les mises à jour automatiques va continuer à assurer des mises à jour importantes s’il est déjà configuré pour ce faire.|
|**Activé**|Spécifie que les mises à jour automatiques installera mises à jour recommandées et les mises à jour importantes à partir de WSUS.|
|**Désactivé**|Spécifie que les mises à jour automatiques va continuer à assurer des mises à jour importantes s’il est déjà configuré pour ce faire.|

**Options :** Il n’existe aucune option pour ce paramètre.

#### <a name="turn-on-software-notifications"></a>Activer les Notifications de logiciel
Ce paramètre de stratégie vous permet de contrôler si les utilisateurs voient des messages de notification améliorée détaillées sur le logiciel complet à partir du service Microsoft Update. Messages de notification améliorée transmettent la valeur et promouvoir l’installation et l’utilisation du logiciel facultatif. Ce paramètre de stratégie est conçu pour une utilisation dans des environnements mal gérés dans laquelle vous autorisez l’accès de l’utilisateur final au service Microsoft Update.

Si vous n’utilisez pas le service Microsoft Update, le paramètre de stratégie « Logiciel Notifications » n’a aucun effet.

Si le paramètre de stratégie « Configurer les mises à jour automatique » est désactivé ou n’est pas configuré, le paramètre de stratégie « Logiciel Notifications » n’a aucun effet.

|Pris en charge :|À l’exclusion :|
|---------|-------|
|à partir de Windows Server 2008 (Windows Vista) et Windows 7, systèmes d’exploitation Windows qui sont toujours dans leur [politique de Support des produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Null|

> [!NOTE]
> Par défaut, ce paramètre de stratégie est désactivé.

|||
|-|-|
|**État de paramètre de stratégie**|**Behavior**|
|**Non configuré**|Les utilisateurs sur les ordinateurs qui exécutent Windows 7 ne propose pas de messages pour les applications facultatives. Les utilisateurs sur les ordinateurs qui exécutent Windows Vista ne propose pas de messages pour les applications facultatives ou mises à jour. Un administrateur local peut modifier ce paramètre en utilisant le panneau de configuration ou une stratégie locale.|
|**Activé**|Si vous activez ce paramètre de stratégie, un message de notification s’affiche sur l’ordinateur quand proposée ce logiciel est disponible. L’utilisateur peut cliquer la notification pour ouvrir la mise à jour de Windows et obtenir plus d’informations sur le logiciel ou l’installer. L’utilisateur peut également cliquer sur **fermez ce message** ou **afficher ultérieurement** pour différer la notification comme il convient.<br /><br />Dans Windows 7, ce paramètre de stratégie contrôle uniquement des notifications détaillées pour les applications facultatives. Dans Windows Vista, ce paramètre de stratégie contrôle les notifications détaillées pour les applications facultatives et les mises à jour.|
|**Désactivé**|Spécifie que les utilisateurs qui exécutent Windows 7 ne seront pas proposées des messages de notification détaillée pour les applications facultatives et les utilisateurs qui exécutent Windows Vista ne seront pas proposées de messages de notification détaillée pour les applications facultatives ou mises à jour facultatives.|

**Options :** Il n’existe aucune option pour ce paramètre.

### <a name="computer-configuration--maintenance-scheduler-policy-settings"></a>Configuration d’ordinateur > Paramètres de stratégie du Planificateur de Maintenance
Dans le paramètre configurer les mises à jour automatiques, vous avez sélectionné l’option **4 - téléchargement automatique et planification des installations**, vous pouvez spécifier les paramètres du Planificateur de Maintenance dans la console GPMC pour les ordinateurs exécutant Windows 8 et RT. Windows de planification Si vous n’avez pas sélectionné l’option 4 dans le paramètre « Configurer les mises à jour automatique », il est inutile de configurer ces paramètres à des fins de mises à jour automatiques. Paramètres du Planificateur de maintenance sont situés dans le chemin d’accès : *PolicyName* > **ordinateur Configuration** > **stratégies** > **modèles d’administration**  >  **Les composants Windows** > **Planificateur de Maintenance**. L’extension du Planificateur de Maintenance de la stratégie de groupe contient les paramètres suivants :

-   [Limite de l’Activation de la Maintenance automatique](#automatic-maintenance-activation-boundary)

-   [Délai aléatoire de Maintenance automatique](#automatic-maintenance-random-delay)

-   [Stratégie de réactivation automatique](#automatic-wakeup-policy)

#### <a name="automatic-maintenance-activation-boundary"></a>Limite de l’activation de la maintenance automatique
Cette stratégie vous permet de configurer le paramètre « Limite de l’activation de la Maintenance automatique ».

La limite de l’activation de maintenance est l’heure planifiée quotidienne au niveau duquel commence la Maintenance automatique.

|Pris en charge :|À l’exclusion :|
|---------|-------|
|Systèmes d’exploitation Windows qui sont toujours dans leur [politique de Support des produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Null|

> [!NOTE]
> Ce paramètre est lié à l’option 4 **configurer les mises à jour automatiques**. Si vous n’avez pas sélectionné l’option 4 dans **configurer les mises à jour automatiques**, il n’est pas nécessaire de configurer ce paramètre.

|||
|-|-|
|**État de paramètre de stratégie**|**Behavior**|
|**Non configuré**|Si ce paramètre de stratégie n’est pas configuré, quotidienne planifiée temps tel que spécifié sur les ordinateurs clients dans le **centre de maintenance** > **Maintenance automatique** le panneau de configuration s’applique.|
|**Activé**|L’activation de ce paramètre de stratégie remplace toute valeur par défaut ou modifié des paramètres configurés sur les ordinateurs clients dans **le panneau de configuration** > **centre de maintenance**  >   **Maintenance automatique** (ou dans certaines versions de client, **Maintenance**).|
|**Désactivé**|Si vous définissez ce paramètre de stratégie sur **désactivé**, l’heure quotidienne planifiée comme spécifié dans le **centre de maintenance** > **Maintenance automatique**, dans le contrôle Panneau de configuration s’applique.|

#### <a name="automatic-maintenance-random-delay"></a>Délai aléatoire de Maintenance automatique
Ce paramètre de stratégie vous permet de configurer le délai aléatoire de Maintenance automatique d’activation.

Le délai aléatoire de maintenance est la quantité de temps jusqu’auquel la Maintenance automatique retardera à partir de sa limite d’activation. Ce paramètre est utile pour les machines virtuelles où maintenance aléatoire peut être une exigence de performance.

|Pris en charge :|À l’exclusion :|
|---------|-------|
|Systèmes d’exploitation Windows qui sont toujours dans leur [politique de Support des produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Null|

> [!NOTE]
> Ce paramètre est lié à l’option 4 **configurer les mises à jour automatiques**. Si vous n’avez pas sélectionné l’option 4 dans **configurer les mises à jour automatiques**, il n’est pas nécessaire de configurer ce paramètre.

Par défaut, lorsque l’option est activée, le délai aléatoire de maintenance régulière est défini **PT4H**.

|||
|-|-|
|**État de paramètre de stratégie**|**Behavior**|
|**Non configuré**|Un délai aléatoire de quatre heures est appliqué à **automatique**.|
|**Activé**|Maintenance automatique retardera à partir de sa limite de l’activation par jusqu'à la quantité de temps spécifiée.|
|**Désactivé**|Aucun délai aléatoire n’est appliqué à la Maintenance automatique.|

#### <a name="automatic-wakeup-policy"></a>Stratégie de réactivation automatique
Ce paramètre de stratégie vous permet de configurer la stratégie de réactivation Maintenance automatique.

La stratégie de réactivation maintenance Spécifie si la Maintenance automatique une demande de mise en éveil à distance à l’ordinateur d’exploitation pour une maintenance quotidienne planifiée.

|Pris en charge :|À l’exclusion :|
|---------|-------|
|Systèmes d’exploitation Windows qui sont toujours dans leur [politique de Support des produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Null|

> [!NOTE]
> Si l’exploitation de l’ordinateur power-mise en éveil stratégie est explicitement désactivée, ce paramètre n’a aucun effet.

> [!NOTE]
> Ce paramètre est lié à l’option 4 **configurer les mises à jour automatiques**. Si vous n’avez pas sélectionné l’option 4 dans **configurer les mises à jour automatiques**, il n’est pas nécessaire de configurer ce paramètre.

|||
|-|-|
|**État de paramètre de stratégie**|**Behavior**|
|**Non configuré**|Si vous ne configurez pas ce paramètre de stratégie, éveil définition comme spécifié dans le **centre de maintenance** > **Maintenance automatique** le panneau de configuration s’applique.|
|**Activé**|Si vous activez ce paramètre de stratégie, la Maintenance automatique tente de définir une stratégie de réactivation du système d’exploitation et d’effectuer une demande de réactivation pour l’heure planifiée quotidienne, si nécessaire.|
|**Désactivé**|Si vous désactivez ce paramètre de stratégie, éveil définition comme spécifié dans le **centre de maintenance** > **Maintenance automatique** le panneau de configuration s’applique.|

### <a name="user-configuration--windows-update-policy-settings"></a>Configuration de l’utilisateur > Paramètres de stratégie de mise à jour de Windows
Cette section fournit des détails sur les paramètres de stratégie utilisateur suivants :

-   [Ne pas afficher d’option « Installer les mises à jour et arrêter » dans la boîte de dialogue d’arrêt vers le bas Windows](#do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog)

-   [Ne pas modifier l’option par défaut « Installer les mises à jour et arrêter » dans la boîte de dialogue d’arrêt vers le bas Windows](#do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog)

-   [supprimer l’accès pour utiliser toutes les fonctionnalités de mise à jour de Windows](#remove-access-to-use-all-windows-update-features)

Dans la console GPMC, les paramètres utilisateur pour les mises à jour automatique d’ordinateurs sont situés dans le chemin d’accès : *PolicyName* > **Configuration utilisateur** > **stratégies** > **modèles d’administration**  >  **Les composants Windows** > **mise à jour Windows**. Les paramètres sont répertoriés dans le même ordre, telles qu’elles apparaissent dans la Configuration d’ordinateur et les extensions de Configuration de l’utilisateur dans la stratégie de groupe, lorsque la **paramètres** onglet de la stratégie de mise à jour de Windows est sélectionné pour trier les paramètres par ordre alphabétique.

> [!NOTE]
> Par défaut, sauf indication contraire, ces paramètres ne sont pas configurés.

> [!TIP]
> pour chacun de ces paramètres, vous pouvez utiliser les étapes suivantes pour activer, désactiver ou naviguer entre les paramètres :

#### <a name="do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog-box"></a>Ne pas afficher d’option « Installer les mises à jour et arrêter » dans la boîte de dialogue d’arrêt vers le bas Windows
Spécifie si le **installer les mises à jour et arrêter** option est affichée dans le **arrêté bas Windows** boîte de dialogue.

|Pris en charge :|À l’exclusion :|
|---------|-------|
|Systèmes d’exploitation Windows qui sont toujours dans leur [politique de Support des produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Null|

|||
|-|-|
|**État de paramètre de stratégie**|**Behavior**|
|**Non configuré**|Spécifie que le **installer les mises à jour et arrêter** option s’affiche dans le **arrêté bas Windows** boîte de dialogue si les mises à jour sont disponibles lorsque l’utilisateur sélectionne l’option Arrêter pour arrêter l’ordinateur.|
|**Activé**|Si vous activez ce paramètre de stratégie, **installer les mises à jour et arrêter** n’apparaît pas comme un choix dans le **arrêté bas Windows** boîte de dialogue, même si les mises à jour sont disponibles pour l’installation lorsque l’utilisateur sélectionne le Option Arrêter pour arrêter l’ordinateur.|
|**Désactivé**|Spécifie que le **installer les mises à jour et arrêter** option s’affiche dans le **arrêté bas Windows** boîte de dialogue si les mises à jour sont disponibles lorsque l’utilisateur sélectionne l’option Arrêter pour arrêter l’ordinateur.|

**Options :** Il n’existe aucune option pour ce paramètre.

#### <a name="do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog-box"></a>Ne pas modifier l’option par défaut « Installer les mises à jour et arrêter » dans la boîte de dialogue d’arrêt vers le bas Windows
Spécifie si le **installer les mises à jour et arrêter** option est autorisée en tant que le choix par défaut dans le **arrêté bas Windows** boîte de dialogue.

|Pris en charge :|À l’exclusion :|
|---------|-------|
|Systèmes d’exploitation Windows qui sont toujours dans leur [politique de Support des produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Null|

> [!NOTE]
> Ce paramètre de stratégie n’a aucun impact si le *PolicyName* > **Configuration utilisateur** > **stratégies**  >   **Modèles d’administration** > **les composants Windows** > **mise à jour Windows** > **ne s’affichent pas » Installer les mises à jour et arrêter » une option dans la boîte de dialogue d’arrêt vers le bas Windows** est activé.

|||
|-|-|
|**État de paramètre de stratégie**|**Behavior**|
|**Non configuré**|Spécifie si le **installer les mises à jour et arrêter** option n’est pas l’option par défaut dans le **arrêté bas Windows** boîte de dialogue si les mises à jour sont disponibles pour l’installation au moment où l’utilisateur sélectionne l’arrêt Option enfoncée pour arrêter l’ordinateur.|
|**Activé**|Spécifie si dernière option d’arrêt l’utilisateur (par exemple, mise en veille prolongée ou redémarrer) est l’option par défaut dans le **arrêté bas Windows** boîte de dialogue, indépendamment de si le **option Installer les mises à jour et arrêter** est disponible dans le **que voulez-vous faire ?** menu.|
|**Désactivé**|Spécifie si le **installer les mises à jour et arrêter** option n’est pas l’option par défaut dans le **arrêté bas Windows** boîte de dialogue si les mises à jour sont disponibles pour l’installation au moment où l’utilisateur sélectionne l’arrêt Option enfoncée pour arrêter l’ordinateur.|

**Options :** Il n’existe aucune option pour ce paramètre.

#### <a name="remove-access-to-use-all-windows-update-features"></a>Supprimer l’accès à l’utilisation de toutes les fonctionnalités Windows Update
Ce paramètre vous permet de supprimer l’accès client WSUS à Windows Update.

|Pris en charge :|À l’exclusion :|
|---------|-------|
|Systèmes d’exploitation Windows qui sont toujours dans leur [politique de Support des produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Null|

|||
|-|-|
|**État de paramètre de stratégie**|**Behavior**|
|**Non configuré**|Les utilisateurs peuvent se connecter au site Web Windows Update.|
|**Activé**|**IMPORTANT :** si vous permettent, toutes les fonctionnalités de mise à jour Windows sont supprimés. Cela inclut les bloque l’accès au site Web Windows Update à https://windowsupdate.microsoft.com, à partir du lien hypertexte de mise à jour de Windows sur l’écran de démarrage ou du menu Démarrer, ainsi que sur le **outils** menu dans Internet Explorer. La mise à jour automatique de Windows est également désactivée ; l’utilisateur ne sera ni être averti ni de recevoir des mises à jour critiques à partir de Windows Update. Ce paramètre empêche également le Gestionnaire de périphériques d’installer automatiquement les mises à jour de pilote à partir du site Web Windows Update.<br /><br />Lorsque l’option est activée, vous pouvez configurer une des options de notification suivantes :<br /><br />-   **0 - ne pas afficher toutes les notifications**<br />    Ce paramètre supprime tous les accès aux fonctionnalités de mise à jour de Windows et aucune notification ne s’affichera.<br />-   **1 - afficher redémarrage requis de notifications**<br />    Ce paramètre affiche les notifications sur les redémarrages sont nécessaires pour terminer une installation. **Remarque :** Sur les ordinateurs exécutant Windows 8 et Windows RT, si cette stratégie est activée, les notifications uniquement liées redémarrages et l’impossibilité de détecter les mises à jour s’affichera. Les options de notification ne sont pas pris en charge. Notifications sur l’écran de connexion sont toujours affichées.|
|**Désactivé**|Les utilisateurs peuvent se connecter au site Web Windows Update.|

**Options :** Consultez **activé** dans la table pour ce paramètre.

## <a name="supplemental-information"></a>Informations supplémentaires
Cette section fournit plus d’informations sur l’utilisation d’ouverture et enregistrement des paramètres WSUS dans les stratégies de groupe et les définitions des termes utilisés dans ce guide. Pour les administrateurs familiarisés avec les versions précédentes de WSUS (WSUS 3.2 et versions antérieures), il est une table qui résume brièvement les différences entre les versions WSUS.

### <a name="accessing-the-windows-update-settings-in-group-policy"></a>Accès aux paramètres de mise à jour de Windows dans la stratégie de groupe
La procédure qui suit décrit comment ouvrir la console GPMC sur votre contrôleur de domaine. Puis, la procédure décrit comment ouvrir un existant au niveau du domaine objet (stratégie de groupe) pour la modifier, ou créer un GPO au niveau du domaine et pour ouvrir pour modification.

> [!NOTE]
> Vous devez être un membre de la **Admins du domaine** groupe ou un équivalent, pour effectuer cette procédure.

##### <a name="to-open-or-add-and-open-a-group-policy-object"></a>Pour ouvrir ou ajouter et ouvrir un objet de stratégie de groupe

1.  Sur votre contrôleur de domaine, accédez à **le Gestionnaire de serveur**, > **outils**, > **Group Policy Management**. La Console de gestion de stratégie de groupe s’ouvre.

2.  Dans le volet gauche, développez votre forêt. Par exemple, double-cliquez sur **forêt : example.com**.

3.  Dans le volet gauche, double-cliquez sur **domaines**, puis double-cliquez sur le domaine pour lequel vous souhaitez gérer un objet de stratégie de groupe. Par exemple, double-cliquez sur **example.com**.

4.  Faites une des actions suivantes :

    -  **Pour ouvrir un objet GPO au niveau du domaine existant pour la modification**, double-cliquez sur le domaine qui contient l’objet de stratégie de groupe que vous souhaitez gérer, avec le bouton droit de la stratégie de domaine que vous souhaitez gérer, puis cliquez sur **modifier**. Éditeur de gestion des stratégies de groupe (GPME) s’ouvre.

    -  **Pour créer un nouvel objet de stratégie de groupe et ouvrir pour modification**:
        1.  Cliquez sur le domaine pour lequel vous souhaitez créer un nouvel objet de stratégie de groupe, puis cliquez sur **créer un objet GPO dans ce domaine et le lier ici**.

        2.  Dans **nouvel objet GPO**, dans **nom**, tapez un nom pour le nouvel objet de stratégie de groupe, puis cliquez sur **OK**.

        3.  Avec le bouton droit de votre nouvel objet de stratégie de groupe, puis cliquez sur **modifier**. GPME s’ouvre.

##### <a name="to-open-the-windows-update-or-maintenance-scheduler-extensions-of-group-policy"></a>Pour ouvrir les extensions de mise à jour de Windows ou le Planificateur de Maintenance de la stratégie de groupe

1.  Dans l’éditeur de gestion des stratégies de groupe, effectuez l’une des opérations suivantes :

    -   **Ouvrez la Configuration de l’ordinateur > extension de mise à jour Windows de la stratégie de groupe**. Accédez à : *PolicyName* > **ordinateur Configuration** > **stratégies** / **modèles d’administration**  >  **Les composants Windows** > **mise à jour Windows**.

    -   **Ouvrez la Configuration utilisateur > extension de mise à jour Windows de la stratégie de groupe**. Accédez à : *PolicyName* > **Configuration utilisateur** > **stratégies** > **modèles d’administration**  >  **Les composants Windows** > **mise à jour Windows**.

    -   **Ouvrez la Configuration de l’ordinateur > extension Planificateur de Maintenance de la stratégie de groupe**. Dans GPOE, accédez à *PolicyName* > **ordinateur Configuration** > **stratégies** > **administratif Modèles** > **les composants Windows** > **Planificateur de Maintenance**.

Pour plus d’informations sur la stratégie de groupe, consultez [Group Policy Overview](https://technet.microsoft.com/library/hh831791.aspx(v=ws.12)).

> [!TIP]
> Une fois que vous avez ouvert l’extension de stratégie de groupe que vous souhaitez, vous pouvez utiliser les étapes suivantes pour activer, désactiver ou naviguer entre les paramètres :

##### <a name="to-configure-group-policy-settings"></a>Pour configurer les paramètres de stratégie de groupe

1.  Dans *ExtensionOfGroupPolicy*, double-cliquez sur le paramètre que vous souhaitez afficher ou modifier.

2.  Pour configurer le paramètre, effectuez l’une des opérations suivantes :

    -   Pour conserver la valeur par défaut n’est pas spécifié l’état de configuration, sélectionnez **pas configuré**.

    -   Pour activer le paramètre, sélectionnez **activé**.

    -   Pour désactiver le paramètre, sélectionnez **désactivé**.

3.  Dans **Options**, si toutes les options sont répertoriées, conservez les valeurs par défaut ou modifiez-les si nécessaire.

4.  Faites une des actions suivantes :

    -   Pour enregistrer vos modifications et continuer au paramètre suivant, cliquez sur **appliquer**, puis cliquez sur **le paramètre suivant**.

    -   Pour enregistrer vos modifications et fermer la boîte de dialogue, cliquez sur **OK**.

    -   Pour ignorer toutes les modifications non enregistrées et fermer la boîte de dialogue, cliquez sur **Annuler**.

### <a name="changes-to-wsus-relevant-to-this-guide"></a>Modifications apportées au serveur WSUS pertinents pour ce guide
Le tableau suivant récapitule les principales différences entre les versions actuelles et passées de WSUS qui s’appliquent à ce guide.

|Versions de Windows Server et WSUS|Description|
|------------------|--------|
| Windows Server 2012 R2 avec WSUS 6.0 et versions ultérieures|à compter de Windows Server 2012, le rôle serveur WSUS est intégré au système d’exploitation et les paramètres de stratégie de groupe associés pour les clients WSUS sont, par défaut, inclus dans la stratégie de groupe.|
| Windows Server 2008 (et les versions antérieures de Windows Server) avec WSUS 3.2 et versions antérieures|Dans Windows Server 2008 (et versions antérieures de Windows Server) à l’aide de WSUS versions 3.2 (et versions antérieures), les paramètres de stratégie de groupe qui régissent les clients WSUS ne sont pas inclus dans ces systèmes d’exploitation de Windows Server. Les paramètres de stratégie sont dans le modèle d’administration WSUS, **wuau.adm**. Dans ces versions de serveur, le modèle d’administration WSUS doit d’abord être ajouté dans la Console de gestion des stratégies de groupe (GPMC) avant de pouvoir configurer les paramètres du client WSUS.|

### <a name="terms-and-definitions"></a>Termes et définitions
Voici une liste des termes utilisés dans ce guide.

|Terme|Définition|
|----|-------|
|Mises à jour automatiques|**Un service qui s’exécute sur les ordinateurs Windows** (mises à jour automatiques) : Fait référence au composant d’ordinateur client intégré à la Microsoft Windows Vista, Windows Server 2003, Windows XP et Windows 2000 avec SP3 les systèmes d’exploitation pour obtenir des mises à jour à partir de Microsoft Update ou Windows Update.<br /><br />**Référence informel** (mises à jour automatiques) : Le terme utilisé pour décrire quand l’Agent de mise à jour Windows planifie et télécharge automatiquement les mises à jour.|
|serveur autonome|Utilisez cette option pour faire référence à un serveur Windows Server Update Services (WSUS) en aval sur lequel les administrateurs peuvent gérer les composants WSUS.|
|serveur en aval|Utilisez cette option pour faire référence à un serveur Windows Server Update Services (WSUS) qui obtient les mises à jour à partir d’un autre serveur WSUS, plutôt qu’à partir de Microsoft Update ou Windows Update.|
|Extension de stratégie de groupe (et : extension de stratégie de groupe|Collection de paramètres de stratégie de groupe qui sont utilisés pour contrôler comment les utilisateurs et ordinateurs (auxquels les stratégies s’appliquent) peuvent configurer et utiliser divers services de Windows et les fonctionnalités. Les administrateurs peuvent utiliser WSUS avec la stratégie de groupe pour la configuration côté client du client mises à jour automatiques, afin de garantir que les utilisateurs finaux ne peut pas désactiver ou contourner les stratégies de mise à jour d’entreprise.<br /><br />WSUS ne nécessite pas l’utilisation d’active directory ou de stratégie de groupe. Configuration du client peut également être appliquée à l’aide de stratégie de groupe locale ou en modifiant le Registre Windows.|
|service de mise à jour interne|Une référence occasionnelle à une infrastructure réseau qui utilise un ou plusieurs serveurs WSUS pour distribuer des mises à jour.|
|serveur de réplication|Utilisez cette option pour faire référence à un serveur Windows Server Update Services (WSUS) en aval qui reflète les approbations et les paramètres sur le serveur en amont auquel il est connecté. Vous ne pouvez pas gérer WSUS sur un serveur de réplication.|
|Microsoft Update|**Un site de téléchargement Microsoft basés sur Internet :** Un site de Microsoft Internet qui stocke et distribue les mises à jour pour les ordinateurs Windows (pilotes de périphérique), les systèmes d’exploitation Windows et autres logiciels Microsoft.|
|Software Update Services (SUS)|SUS a été produit prédécesseur pour Windows Server Update Services (WSUS).|
|« mises à jour »|Une d’une collection de versions de logiciels, les correctifs logiciels, les service packs, les packs de fonctionnalités et des pilotes de périphérique qui peuvent être installés sur un ordinateur pour étendre les fonctionnalités ou améliorent les performances et la sécurité.|
|fichiers de mise à jour|Les fichiers requis pour installer une mise à jour sur un ordinateur.|
|informations de mise à jour (également connu sous le nom des métadonnées de mise à jour)|Informations relatives à une mise à jour, plutôt que les fichiers binaires de mise à jour dans un package de mise à jour. Par exemple, les métadonnées fournissent des informations pour les propriétés d’une mise à jour, ce qui vous permet de savoir ce que la mise à jour est utile. Les métadonnées incluent également les termes du contrat de licence de logiciel Microsoft. Le package de métadonnées téléchargé pour une mise à jour est généralement plus petit que le package de fichiers de mise à jour réelle.|
|source de mise à jour|L’emplacement auquel un serveur Windows Server Update Services (WSUS) synchronise pour obtenir les fichiers de mise à jour. Cet emplacement peut être Microsoft Update ou un serveur WSUS en amont.|
|serveur en amont|Un serveur Windows Server Update Services (WSUS) qui fournit les fichiers de mise à jour vers un autre serveur WSUS, ce qui à son tour correspond à un serveur en aval.|
|Windows Server Update Services (WSUS)|Un programme de rôle de serveur qui s’exécute sur un ou plusieurs ordinateurs Windows Server sur un réseau d’entreprise. Une infrastructure WSUS vous permet de gérer les mises à jour pour les ordinateurs sur votre réseau afin d’installer.<br /><br />Vous pouvez utiliser WSUS pour approuver ou refuser des mises à jour avant la mise en production, pour forcer les mises à jour à installer à une date donnée, et pour obtenir des rapports sur les mises à jour de chaque ordinateur sur votre réseau étendus nécessite. Vous pouvez configurer WSUS pour approuver certaines classes de mises à jour automatiquement (mises à jour critiques, mises à jour de sécurité, les service packs, pilotes, etc.). WSUS vous permet également d’approuver les mises à jour pour « détection » uniquement, afin que vous puissiez voir quels ordinateurs nécessitent une mise à jour donnée sans avoir à installer les mises à jour.<br /><br />Dans une implémentation WSUS, au moins un serveur WSUS du réseau doit être en mesure de se connecter à Microsoft Update pour obtenir les mises à jour disponibles. Selon la configuration et de sécurité réseau, l’administrateur peut déterminer combien d’autres serveurs se connectant directement à Microsoft Update.<br /><br />Vous pouvez configurer un serveur WSUS pour obtenir des mises à jour sur Internet à partir de ces emplacements en tant que :<br /><br />-le public de Microsoft Update<br />-la mise à jour publique de Windows<br />-   Microsoft Store|
|Windows Update|**Un site de téléchargement Microsoft basés sur Internet :** Un site de Microsoft Internet qui stocke et distribue les mises à jour pour les ordinateurs Windows (pilotes de périphérique) et les systèmes d’exploitation Windows.<br /><br />**service de l’ordinateur :** Le nom du service de mise à jour de Windows qui s’exécute sur les ordinateurs. Mise à jour de Windows détecte, télécharge et installe les mises à jour sur les ordinateurs Windows.<br /><br />Selon l’ordinateur et les configurations de stratégie, l’Agent de mise à jour de Windows peuvent télécharger des mises à jour à partir de :<br /><br />-   Microsoft Update<br />-   Windows Update<br />-   Microsoft Store<br />-Un service de mise à jour d’Internet (réseau) (WSUS)<br /><br />les ordinateurs qui ne sont pas gérés dans un environnement basé sur WSUS généralement utilisera mise à jour de Windows pour se connecter directement, via Internet - mise à jour de Windows, Microsoft Update ou Microsoft Store pour obtenir des mises à jour.|
|Client WSUS|Un ordinateur qui reçoit des mises à jour à partir d’un service de mise à jour WSUS intranet.<br /><br />Dans le cas des paramètres de stratégie de groupe qui contrôlent l’interaction de l’utilisateur final avec les mises à jour automatiques : un utilisateur d’un ordinateur dans un environnement de WSUS.|
