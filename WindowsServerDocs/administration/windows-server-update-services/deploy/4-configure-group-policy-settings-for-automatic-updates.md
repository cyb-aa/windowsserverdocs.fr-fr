---
title: Étape 4-configurer les paramètres de stratégie de groupe pour Mises à jour automatiques
description: Rubrique Windows Server Update Service (WSUS)-configurer les paramètres de stratégie de groupe pour Mises à jour automatiques est l’étape 4 dans un processus en quatre étapes pour le déploiement de WSUS
ms.prod: windows-server
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
ms.openlocfilehash: a01d8881e8f0f7ca6feff691938f926a12460db0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361665"
---
# <a name="step-4-configure-group-policy-settings-for-automatic-updates"></a>Étape 4 : Configurer les paramètres de stratégie de groupe pour Mises à jour automatiques

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Dans un environnement Active Directory, vous pouvez utiliser stratégie de groupe pour définir la manière dont les ordinateurs et les utilisateurs (mentionnés dans ce document en tant que clients WSUS) peuvent interagir avec les mises à jour Windows pour obtenir des mises à jour automatiques à partir de Windows Server Update Services (WSUS).

Cette rubrique contient deux sections principales :

[Stratégie de groupe paramètres pour les mises à jour du client WSUS](#group-policy-settings-for-wsus-client-updates), qui fournit des instructions et des informations de comportement détaillées sur les paramètres du planificateur de maintenance et de Windows Update de stratégie de groupe qui contrôlent la façon dont les clients WSUS peuvent interagir avec Windows Update à obtenir les mises à jour automatiques.

Les [informations supplémentaires](#supplemental-information) comportent les sections suivantes :

-   [Accès aux paramètres de Windows Update dans stratégie de groupe](#accessing-the-windows-update-settings-in-group-policy), qui fournit des instructions générales sur l’utilisation de l’éditeur de gestion des stratégie de groupe et des informations sur l’accès aux paramètres des extensions de stratégie des services de mise à jour et du planificateur de maintenance dans le groupe Renvoi.

-   [Modifications apportées à WSUS en ce qui concerne ce guide](#changes-to-wsus-relevant-to-this-guide): pour les administrateurs connaissant WSUS 3,2 et les versions antérieures, cette section fournit un bref résumé des principales différences entre la version actuelle et la version antérieure de WSUS en rapport avec ce guide.

-   [Termes et définitions](#terms-and-definitions): définitions pour les différents termes relatifs aux services WSUS et mises à jour qui sont utilisés dans ce guide.

## <a name="group-policy-settings-for-wsus-client-updates"></a>Paramètres de stratégie de groupe pour les mises à jour du client WSUS
Cette section fournit des informations sur les trois extensions de stratégie de groupe. Dans ces extensions, vous trouverez les paramètres que vous pouvez utiliser pour configurer la façon dont les clients WSUS peuvent interagir avec Windows Update pour recevoir des mises à jour automatiques.

-   [Configuration de l’ordinateur &gt; Windows Update paramètres de stratégie](#computer-configuration--windows-update-policy-settings)

-   [Configuration de l’ordinateur &gt; paramètres de stratégie du planificateur de maintenance](#computer-configuration--maintenance-scheduler-policy-settings)

-   [Configuration utilisateur &gt; Windows Update paramètres de stratégie](#user-configuration--windows-update-policy-settings)

> [!NOTE]
> Cette rubrique part du principe que vous utilisez déjà et que vous êtes familiarisé avec stratégie de groupe. Si vous n’êtes pas familiarisé avec stratégie de groupe, il est recommandé de passer en revue les informations contenues dans la section [informations supplémentaires](#supplemental-information) de ce document avant de tenter de configurer les paramètres de stratégie pour WSUS.

### <a name="computer-configuration--windows-update-policy-settings"></a>Configuration de l’ordinateur > Windows Update paramètres de stratégie
Cette section fournit des détails sur les paramètres de stratégie basés sur l’ordinateur suivants :

-   [Autoriser l’installation immédiate Mises à jour automatiques](#allow-automatic-updates-immediate-installation)

-   [Autoriser les non-administrateurs à recevoir des notifications de mise à jour](#allow-non-administrators-to-receive-update-notifications)

-   [Autoriser les mises à jour signées à partir d’un emplacement intranet du service de mise à jour Microsoft](#allow-signed-updates-from-an-intranet-microsoft-update-service-location)

-   [Fréquence de détection de Mises à jour automatiques](#automatic-updates-detection-frequency)

-   [Configurer Mises à jour automatiques](#configure-automatic-updates)

-   [Redémarrage différé pour les installations planifiées](#delay-restart-for-scheduled-installations)

-   [Ne pas définir l’option par défaut sur « installer les mises à jour et les arrêter » dans la boîte de dialogue arrêter Windows](#do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog)

-   [Ne pas afficher l’option « installer les mises à jour et éteindre » dans la boîte de dialogue arrêter Windows](#do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog)

-   [Activer le ciblage côté client](#enable-client-side-targeting)

-   [Activation de la gestion de l’alimentation Windows Update pour réveiller automatiquement l’ordinateur afin d’installer les mises à jour planifiées](#enabling-windows-update-power-management-to-automatically-wake-up-the-computer-to-install-scheduled-updates)

-   [Aucun redémarrage automatique avec les utilisateurs connectés pour les installations de mises à jour automatiques planifiées](#no-auto-restart-with-logged-on-users-for-scheduled-automatic-updates-installations)

-   [Relancer l’invite de redémarrage avec les installations planifiées](#re-prompt-for-restart-with-scheduled-installations)

-   [Replanifier les installations planifiées Mises à jour automatiques](#reschedule-automatic-updates-scheduled-installations)

-   [Spécifier l’emplacement intranet du service de mise à jour Microsoft](#specify-intranet-microsoft-update-service-location)

-   [Activer les mises à jour recommandées via Mises à jour automatiques](#turn-on-recommended-updates-via-automatic-updates)

-   [Activer les notifications de logiciels](#turn-on-software-notifications)

Dans le éditeur, Windows Update stratégies pour la configuration basée sur l’ordinateur se trouvent dans le chemin d’accès : *PolicyName* > **Configuration ordinateur** > **stratégies** > **modèles d’administration** **composants Windows**@no__t-**7  >  Windows Update**.

> [!NOTE]
> Par défaut, ces paramètres ne sont pas configurés.

#### <a name="allow-automatic-updates-immediate-installation"></a>Autoriser l’installation immédiate des mises à jour automatiques
Spécifie si Mises à jour automatiques installera automatiquement les mises à jour qui n’interrompent pas les services Windows ou ne redémarrent pas Windows.

|Pris en charge sur :|Exclus|
|---------|-------|
|Les systèmes d’exploitation Windows qui se trouvent toujours dans les [produits Microsoft prennent en charge le cycle de vie](https://support.microsoft.com/gp/lifeselect).|Null|

> [!NOTE]
> Si le paramètre de stratégie « configurer le Mises à jour automatiques » a la valeur **désactivé**, cette stratégie n’a aucun effet.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie que les mises à jour ne sont pas installées immédiatement. Les administrateurs locaux peuvent modifier ce paramètre à l’aide de l’éditeur de stratégie de groupe local.|
|**Activé**|Spécifie que Mises à jour automatiques installe immédiatement les mises à jour une fois qu’elles ont été téléchargées et prêtes à être installées.|
|**Désactivé**|Spécifie que les mises à jour ne sont pas installées immédiatement.|

**Options:** Il n’existe aucune option pour ce paramètre.

#### <a name="allow-non-administrators-to-receive-update-notifications"></a>Autoriser les non-administrateurs à recevoir des notifications de mise à jour
Spécifie si les utilisateurs non-administrateurs reçoivent des notifications de mise à jour en fonction du paramètre de stratégie configurer Mises à jour automatiques.

|Pris en charge sur :|Exclus|
|---------|-------|
|Les systèmes d’exploitation Windows qui se trouvent toujours dans les [produits Microsoft prennent en charge le cycle de vie](https://support.microsoft.com/gp/lifeselect).|Pour plus d’informations, consultez le tableau ci-dessous.|

> [!NOTE]
> Si le paramètre de stratégie « configurer le Mises à jour automatiques » est désactivé ou n’est pas configuré, ce paramètre de stratégie n’a aucun effet.

> [!IMPORTANT]
> à compter de Windows 8 et de Windows RT, ce paramètre de stratégie est activé par défaut. Dans toutes les versions antérieures de Windows, il est désactivé par défaut.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie que les utilisateurs verront toujours une fenêtre de contrôle de compte et qu’ils auront besoin d’autorisations élevées pour effectuer ces tâches. Un administrateur local peut modifier ce paramètre à l’aide de l’éditeur de stratégie de groupe local.|
|**Activé**|Spécifie que la mise à jour et la Microsoft Update automatiques Windows incluent les non-administrateurs lors de la détermination de l’utilisateur connecté qui recevra les notifications de mise à jour. Les utilisateurs non-administrateurs pourront installer tout contenu de mise à jour facultatif, recommandé et IMPORTANT pour lequel ils ont reçu une notification. Les utilisateurs ne voient pas une fenêtre de contrôle de compte d’utilisateur et ils n’ont pas besoin d’autorisations élevées pour installer ces mises à jour, sauf dans le cas de mises à jour qui contiennent une interface utilisateur, un contrat de licence utilisateur final ou des modifications de paramètres Windows Update.<br /><br />Il existe deux situations dans lesquelles l’effet de ce paramètre dépend de l’ordinateur d’exploitation :<br /><br />1.  **Masquer** ou **restaurer** les mises à jour<br />2.  **Annuler** une installation de mise à jour<br /><br />Sous Windows Vista ou Windows XP, si ce paramètre de stratégie est activé, les utilisateurs ne verront pas une fenêtre de contrôle de compte d’utilisateur et ils n’ont pas besoin d’autorisations élevées pour masquer, restaurer ou annuler des mises à jour.<br /><br />Dans Windows Vista, si ce paramètre de stratégie est activé, les utilisateurs ne verront pas une fenêtre de contrôle de compte d’utilisateur et ils n’ont pas besoin d’autorisations élevées pour masquer, restaurer ou annuler des mises à jour. Si ce paramètre de stratégie n’est pas activé, les utilisateurs verront toujours une fenêtre de contrôle de compte et ils auront besoin d’autorisations élevées pour masquer, restaurer ou annuler les mises à jour.<br /><br />Dans Windows 7, ce paramètre de stratégie n’a aucun effet. Les utilisateurs verront toujours une fenêtre de contrôle de compte et ils auront besoin d’autorisations élevées pour effectuer ces tâches.<br /><br />Dans Windows 8 et Windows RT, ce paramètre de stratégie n’a aucun effet.|
|**Désactivé**|Spécifie que seuls les administrateurs connectés reçoivent des notifications de mise à jour. **Remarque :** Dans Windows 8 et Windows RT, ce paramètre de stratégie est activé par défaut. Dans toutes les versions antérieures de Windows, il est désactivé par défaut.|

**Options:** Il n’existe aucune option pour ce paramètre.

#### <a name="allow-signed-updates-from-an-intranet-microsoft-update-service-location"></a>Autoriser les mises à jour signées d'un emplacement intranet du service de mise à jour Microsoft
Spécifie si Mises à jour automatiques accepte les mises à jour signées par des entités autres que Microsoft lorsque la mise à jour se trouve sur un emplacement intranet du service de mise à jour Microsoft.

|Pris en charge sur :|Exclus|
|---------|-------|
|Les systèmes d’exploitation Windows qui se trouvent toujours dans les [produits Microsoft prennent en charge le cycle de vie](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!NOTE]
> Les mises à jour à partir d’un service autre qu’un service intranet de mise à jour Microsoft doivent toujours être signées par Microsoft et ne sont pas affectées par ce paramètre de stratégie.

> [!NOTE]
> Cette stratégie n’est pas prise en charge sur Windows RT. L’activation de cette stratégie n’aura aucun effet sur les ordinateurs exécutant Windows RT.

**Options:** Il n’existe aucune option pour ce paramètre.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie que les mises à jour à partir d’un emplacement intranet du service de mise à jour Microsoft doivent être signées par Microsoft.|
|**Activé**|Spécifie que Mises à jour automatiques accepte les mises à jour reçues via un emplacement intranet du service de mise à jour Microsoft s’ils sont signés par un certificat trouvé dans le magasin de certificats « Éditeurs approuvés » de l’ordinateur local.|
|**Désactivé**|Spécifie que les mises à jour à partir d’un emplacement intranet du service de mise à jour Microsoft doivent être signées par Microsoft.|

**Options:** Il n’existe aucune option pour ce paramètre.

#### <a name="always-automatically-restart-at-the-scheduled-time"></a>Toujours redémarrer automatiquement à l’heure planifiée
Spécifie si un minuteur de redémarrage commence toujours immédiatement Windows Update après l’installation des mises à jour importantes, au lieu d’envoyer d’abord une notification aux utilisateurs sur l’écran de connexion pendant au moins deux jours.

|Pris en charge sur :|Exclus|
|---------|-------|
|Les systèmes d’exploitation Windows qui se trouvent toujours dans les [produits Microsoft prennent en charge le cycle de vie](https://support.microsoft.com/gp/lifeselect).|Null|

> [!NOTE]
> Si le paramètre de stratégie « aucun redémarrage automatique avec des utilisateurs connectés pour les installations de mises à jour automatiques planifiées » est activé, cette stratégie n’a aucun effet.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie que Windows Update ne modifiera pas le comportement de redémarrage de l’ordinateur.|
|**Activé**|Spécifie qu’un minuteur de redémarrage commence toujours immédiatement Windows Update après l’installation des mises à jour importantes, au lieu d’envoyer d’abord une notification aux utilisateurs sur l’écran de connexion pendant au moins deux jours.<br /><br />Le minuteur de redémarrage peut être configuré pour démarrer avec n’importe quelle valeur comprise entre 15 et 180 minutes. Lorsque le minuteur est en cours d’exécution, le redémarrage se poursuit même si l’ordinateur possède des utilisateurs connectés.|
|**Désactivé**|Spécifie que Windows Update ne modifiera pas le comportement de redémarrage de l’ordinateur.|

**Options :** si ce paramètre est activé, vous pouvez spécifier la durée qui doit s’écouler après l’installation des mises à jour avant le redémarrage forcé de l’ordinateur.

#### <a name="automatic-updates-detection-frequency"></a>Fréquence de détection des mises à jour automatiques
Spécifie la durée en heures pendant laquelle Windows attendra avant de vérifier la disponibilité de nouvelles mises à jour. La durée exacte est déterminée en utilisant ce nombre d’heures moins un pourcentage compris entre zéro et vingt pour-cent du nombre d’heures spécifié. Par exemple, si cette stratégie est utilisée pour spécifier une fréquence de détection de 20 heures, tous les clients auxquels cette stratégie est appliquée recherchent les mises à jour n’importe où entre 16 et 20 heures.

|Pris en charge sur :|Exclus|
|---------|-------|
|Les systèmes d’exploitation Windows qui se trouvent toujours dans les [produits Microsoft prennent en charge le cycle de vie](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!NOTE]
> Le paramètre « spécifier l’emplacement intranet du service de mise à jour Microsoft » doit être activé pour que cette stratégie soit appliquée.
>
> Si le paramètre de stratégie « configurer le Mises à jour automatiques » est désactivé, cette stratégie n’a aucun effet.

> [!NOTE]
> Cette stratégie n’est pas prise en charge sur Windows RT. L’activation de cette stratégie n’aura aucun effet sur les ordinateurs exécutant Windows RT.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie que Windows recherchera les mises à jour disponibles à l’intervalle par défaut de 22 heures.|
|**Activé**|Spécifie que Windows recherchera les mises à jour disponibles à l’intervalle spécifié.|
|**Désactivé**|Spécifie que Windows recherchera les mises à jour disponibles à l’intervalle par défaut de 22 heures.|

**Options :** si ce paramètre est activé, vous pouvez spécifier l’intervalle de temps (en heures) pendant lequel Windows Update attend avant de vérifier les mises à jour.

#### <a name="configure-automatic-updates"></a>Configurer les mises à jour automatiques
Spécifie si les mises à jour automatiques sont activées sur cet ordinateur.

|Pris en charge sur :|Exclus|
|---------|-------|
|Les systèmes d’exploitation Windows qui se trouvent toujours dans les [produits Microsoft prennent en charge le cycle de vie](https://support.microsoft.com/gp/lifeselect).|Windows RT|

Si cette option est activée, vous devez sélectionner l’une des quatre options fournies dans ce paramètre de stratégie de groupe.

Pour utiliser ce paramètre, sélectionnez **activé**, puis dans **options** sous **configurer la mise à jour automatique**, sélectionnez l’une des options (2, 3, 4 ou 5).

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie que l’utilisation de mises à jour automatiques n’est pas spécifiée au niveau de la stratégie de groupe. Toutefois, un administrateur d’ordinateur peut toujours configurer les mises à jour automatiques dans le panneau de configuration.|
|**Activé**|Spécifie que Windows reconnaît lorsque l’ordinateur est en ligne et utilise sa connexion Internet pour rechercher les mises à jour disponibles dans Windows Update.<br /><br />Lorsqu’il est activé, les administrateurs locaux sont autorisés à utiliser le panneau de configuration Windows Update pour sélectionner une option de configuration de leur choix. Toutefois, les administrateurs locaux ne sont pas autorisés à désactiver la configuration de Mises à jour automatiques.<br /><br />-   **2-notifier le téléchargement et notifier l’installation**<br />    Lorsque Windows Update trouve des mises à jour qui s’appliquent à l’ordinateur, les utilisateurs sont avertis que des mises à jour sont prêtes à être téléchargées. Les utilisateurs peuvent ensuite exécuter Windows Update pour télécharger et installer les mises à jour disponibles.<br />-   **3-téléchargement automatique et notification pour l’installation** (paramètre par défaut)<br />    Windows Update recherche les mises à jour applicables et les télécharge en arrière-plan. l’utilisateur n’est pas notifié ou interrompu pendant le processus. Une fois les téléchargements terminés, les utilisateurs sont avertis que des mises à jour sont prêtes à être installées. Les utilisateurs peuvent ensuite exécuter Windows Update pour installer les mises à jour téléchargées.<br />-   **4-téléchargement automatique et planification de l’installation**<br />    Vous pouvez spécifier la planification à l’aide des options de ce paramètre de stratégie de groupe. Si aucune planification n’est spécifiée, la planification par défaut pour toutes les installations est tous les jours à 3:00 h 00. Si des mises à jour nécessitent un redémarrage pour terminer l’installation, Windows redémarre automatiquement l’ordinateur. (si un utilisateur est connecté à l’ordinateur lorsque Windows est prêt à redémarrer, l’utilisateur est averti et peut choisir de retarder le redémarrage.) **Remarque :** à partir de Windows 8, vous pouvez définir des mises à jour à installer pendant la maintenance automatique au lieu d’utiliser un calendrier spécifique lié à Windows Update. La maintenance automatique installe les mises à jour lorsque l’ordinateur n’est pas utilisé et évite d’installer les mises à jour lorsque l’ordinateur fonctionne sur batterie. Si la maintenance automatique ne parvient pas à installer les mises à jour dans les jours, Windows Update installera immédiatement les mises à jour. Les utilisateurs seront ensuite avertis d’un redémarrage en attente. Un redémarrage en attente n’aura lieu que s’il n’y a aucun risque de perte de données accidentelle.    Vous pouvez spécifier des options de planification dans les paramètres du planificateur de maintenance *éditeur, qui*se trouvent dans le chemin d’accès, règles de configuration de l'**ordinateur** >   > **stratégies** > **modèles d’administration** >  **Composants Windows** > **planification**de la maintenance 1 limite d’activation de la**maintenance automatique**. Consultez la section de cette référence intitulée : [Paramètres du planificateur de maintenance](#computer-configuration--maintenance-scheduler-policy-settings), pour la définition des détails.    **5-autoriser l’administrateur local à choisir un paramètre**<br />: Spécifie si les administrateurs locaux sont autorisés à utiliser le Mises à jour automatiques panneau de configuration pour sélectionner une option de configuration de leur choix, par exemple, si les administrateurs locaux peuvent choisir une heure d’installation planifiée.<br />    Les administrateurs locaux ne seront pas autorisés à désactiver la configuration des mises à jour automatiques.|
|**Désactivé**|Spécifie que toutes les mises à jour du client qui sont disponibles à partir du service Windows Update public doivent être téléchargées manuellement à partir d’Internet et installées.|

#### <a name="delay-restart-for-scheduled-installations"></a>Redémarrage différé pour les installations planifiées
Spécifie la durée d’attente de Mises à jour automatiques avant de procéder à un redémarrage planifié.

|Pris en charge sur :|Exclus|
|---------|-------|
|Les systèmes d’exploitation Windows qui se trouvent toujours dans les [produits Microsoft prennent en charge le cycle de vie](https://support.microsoft.com/gp/lifeselect).|Null|

> [!NOTE]
> Cette stratégie s’applique uniquement lorsque Mises à jour automatiques est configurée pour effectuer des installations planifiées de mises à jour. Si le paramètre de stratégie « configurer le Mises à jour automatiques » est désactivé, cette stratégie n’a aucun effet.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie qu’après l’installation des mises à jour, le délai d’attente par défaut de quinze minutes s’écoulera avant le redémarrage planifié.|
|**Activé**|Spécifie que lorsque l’installation est terminée, un redémarrage planifié se produit une fois que le nombre de minutes spécifié a expiré.|
|**Désactivé**|Spécifie qu’après l’installation des mises à jour, le délai d’attente par défaut de quinze minutes s’écoulera avant le redémarrage planifié.|

**Options :** si ce paramètre est activé, vous pouvez utiliser cette option pour spécifier la durée (en minutes) pendant laquelle mises à jour automatiques attend avant de procéder à un redémarrage planifié.

#### <a name="do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog"></a>Ne pas ajuster l’option par défaut pour installer les mises à jour et les arrêter dans la boîte de dialogue arrêter Windows
Ce paramètre de stratégie vous permet de spécifier si l’option **installer les mises à jour et les arrêts** est autorisée comme choix par défaut dans la boîte de dialogue **arrêt de Windows** .

|Pris en charge sur :|Exclus|
|---------|-------|
|Les systèmes d’exploitation Windows qui se trouvent toujours dans les [produits Microsoft prennent en charge le cycle de vie](https://support.microsoft.com/gp/lifeselect).|Null|

> [!NOTE]
> Ce paramètre de stratégie n’a aucun impact *si les***stratégies**de**configuration**de l’ordinateur  >  de la configuration de l’ordinateur  >   > **modèles d’administration** **composants Windows** >  @no__t **-9 Windows Update @no_** _ t-11**ne pas afficher l’option « installer les mises à jour et éteindre » dans le paramètre de stratégie de dialogue arrêter Windows** est activé.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Indique que l’option **installer les mises à jour et les arrêts** est l’option par défaut dans la boîte de dialogue **arrêter Windows** si des mises à jour sont disponibles pour l’installation au moment où l’utilisateur sélectionne l’option arrêter pour arrêter l’ordinateur.|
|**Activé**|Si vous activez ce paramètre de stratégie, le dernier choix de l’utilisateur (par exemple, mettre en veille prolongée ou redémarrer) est l’option par défaut dans la boîte de dialogue **arrêt de Windows** , que l’option **installer les mises à jour et les arrêts** soit disponible dans la **Que voulez-vous faire ?** menus.|
|**Désactivé**|Indique que l’option **installer les mises à jour et les arrêts** est l’option par défaut dans la boîte de dialogue **arrêter Windows** si des mises à jour sont disponibles pour l’installation au moment où l’utilisateur sélectionne l’option arrêter pour arrêter l’ordinateur.|

**Options:** Il n’existe aucune option pour ce paramètre.

#### <a name="do-not-connect-to-any-windows-update-internet-locations"></a>Ne pas se connecter à des emplacements Internet Windows Update
Même lorsque Windows Update est configuré pour recevoir des mises à jour d’un service intranet de mise à jour, il récupère régulièrement les informations du service de Windows Update public afin d’autoriser les connexions futures à Windows Update et à d’autres services, tels que les Microsoft Update ou Microsoft Store.

L’activation de cette stratégie désactive la fonctionnalité permettant de récupérer périodiquement les informations du service de mise à jour Windows Server public et peut entraîner la connexion aux services publics, tels que Microsoft Store, de cesser de fonctionner.

|Pris en charge sur :|Exclus|
|---------|-------|
|à compter de Windows Server 2012 R2, Windows 8.1 ou Windows RT 8,1, les systèmes d’exploitation Windows qui se trouvent toujours dans leurs [produits Microsoft prennent en charge le cycle de vie](https://support.microsoft.com/gp/lifeselect).|Null|

> [!NOTE]
> Cette stratégie s’applique uniquement lorsque l’ordinateur est configuré pour se connecter à un service intranet de mise à jour à l’aide du paramètre de stratégie « spécifier l’emplacement intranet du service de mise à jour Microsoft ».

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Le comportement par défaut pour récupérer des informations du service de mise à jour Windows Server public persiste.|
|**Activé**|Spécifie que les ordinateurs ne récupéreront pas les informations du service de mise à jour Windows Server public.|
|**Désactivé**|Le comportement par défaut pour récupérer des informations du service de mise à jour Windows Server public persiste.|

**Options:** Il n’existe aucune option pour ce paramètre.

#### <a name="do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog"></a>Ne pas afficher les mises à jour d’installation et les options arrêter dans la boîte de dialogue arrêter Windows
Spécifie si l’option **installer les mises à jour et les arrêts** s’affiche dans la boîte de dialogue **arrêter Windows** .

|Pris en charge sur :|Exclus|
|---------|-------|
|Les systèmes d’exploitation Windows qui se trouvent toujours dans les [produits Microsoft prennent en charge le cycle de vie](https://support.microsoft.com/gp/lifeselect).|Null|

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie que l’option **installer les mises à jour et les arrêts** est disponible dans la boîte de dialogue **arrêter Windows** si des mises à jour sont disponibles lorsque l’utilisateur sélectionne l’option arrêter pour arrêter l’ordinateur. Un administrateur local peut modifier ce paramètre à l’aide de la stratégie locale.|
|**Activé**|Spécifie que l’option **installer les mises à jour et les arrêts** ne s’affiche pas dans la boîte de dialogue **arrêt de Windows** , même si les mises à jour sont disponibles pour installation lorsque l’utilisateur sélectionne l’option arrêter pour arrêter l’ordinateur.|
|**Désactivé**|Spécifie que l’option **installer les mises à jour et éteindre** est l’option par défaut dans la boîte de dialogue **arrêter Windows** si des mises à jour sont disponibles pour l’installation au moment où l’utilisateur sélectionne l’option arrêter pour arrêter l’ordinateur.|

**Options:** Il n’existe aucune option pour ce paramètre.

#### <a name="enable-client-side-targeting"></a>Activer le ciblage côté client
Spécifie le ou les noms de groupe cibles qui sont configurés dans la console WSUS pour recevoir des mises à jour de WSUS.

|Pris en charge sur :|Exclus|
|---------|-------|
|Les systèmes d’exploitation Windows qui se trouvent toujours dans les [produits Microsoft prennent en charge le cycle de vie](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!NOTE]
> Cette stratégie s’applique uniquement lorsque cet ordinateur est configuré pour prendre en charge les noms de groupes cibles spécifiés dans WSUS. Si le nom du groupe cible n’existe pas dans WSUS, il sera ignoré jusqu’à ce qu’il soit créé. Si le paramètre de stratégie « spécifier l’emplacement intranet du service de mise à jour Microsoft » est désactivé ou n’est pas configuré, cette stratégie n’a aucun effet.

> [!NOTE]
> Cette stratégie n’est pas prise en charge sur Windows RT. L’activation de cette stratégie n’aura aucun effet sur les ordinateurs exécutant Windows RT.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie qu’aucune information de groupe cible n’est envoyée à WSUS. Un administrateur local peut modifier ce paramètre à l’aide d’une stratégie locale.|
|**Activé**|Spécifie que les informations de groupe cible spécifiées sont envoyées à WSUS, ce qui l’utilise pour déterminer les mises à jour qui doivent être déployées sur cet ordinateur. Si WSUS prend en charge plusieurs groupes cibles, vous pouvez utiliser cette stratégie pour spécifier plusieurs noms de groupes, séparés par des points-virgules, si vous avez ajouté les noms de groupes cibles dans la liste des groupes d’ordinateurs dans WSUS. Dans le cas contraire, un seul groupe doit être indiqué.|
|**Désactivé**|Spécifie qu’aucune information de groupe cible n’est envoyée à WSUS.|

**Options:** Utilisez cet espace pour spécifier un ou plusieurs noms de groupes cibles.

#### <a name="enabling-windows-update-power-management-to-automatically-wake-up-the-computer-to-install-scheduled-updates"></a>Activation de la gestion de l’alimentation Windows Update pour réveiller automatiquement l’ordinateur afin d’installer les mises à jour planifiées
Spécifie si Windows Update utilise les fonctionnalités de gestion de l’alimentation ou d’options d’alimentation de Windows pour réveiller automatiquement l’ordinateur de la mise en veille prolongée si des mises à jour sont planifiées pour l’installation.

L’ordinateur est automatiquement en éveil uniquement si Windows Update est configuré pour installer automatiquement les mises à jour. Si l’ordinateur est en veille prolongée lorsque l’heure d’installation planifiée se produit et que des mises à jour doivent être appliquées, Windows Update utilise les fonctionnalités de gestion de l’alimentation ou d’options d’alimentation de Windows pour mettre automatiquement en éveil l’ordinateur pour installer les mises à jour. Windows Update met également en éveil l’ordinateur et installe une mise à jour si une échéance d’installation se produit.

L’ordinateur n’est pas en éveil, sauf s’il existe des mises à jour à installer. Si l’ordinateur fonctionne sur batterie, lorsque Windows Update le sort de veille, il n’installe pas les mises à jour et l’ordinateur repasse automatiquement en mode veille prolongée en deux minutes.

|Pris en charge sur :|Exclus|
|---------|-------|
|à compter de Windows Vista et de Windows Server 2008 (Windows 7), les systèmes d’exploitation Windows qui se trouvent toujours dans leurs [produits Microsoft prennent en charge le cycle de vie](https://support.microsoft.com/gp/lifeselect).|Null|

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Windows Update ne sort pas l’ordinateur de la mise en veille prolongée pour installer les mises à jour. Un administrateur local peut modifier ce paramètre à l’aide d’une stratégie locale.|
|**Activé**|Windows Update fait sortir l’ordinateur de la veille prolongée pour installer les mises à jour dans les conditions précédemment listées.|
|**Désactivé**|Windows Update ne sort pas l’ordinateur de la mise en veille prolongée pour installer les mises à jour.|

**Options:** Il n’existe aucune option pour ce paramètre.

#### <a name="no-auto-restart-with-logged-on-users-for-scheduled-automatic-updates-installations"></a>Pas de redémarrage automatique avec des utilisateurs connectés pour les installations planifiées de mises à jour automatiques
Spécifie que pour effectuer une installation planifiée, Mises à jour automatiques attend que l’ordinateur soit redémarré par tout utilisateur connecté, au lieu de provoquer le redémarrage automatique de l’ordinateur.

|Pris en charge sur :|Exclus|
|---------|-------|
|Les systèmes d’exploitation Windows qui se trouvent toujours dans les [produits Microsoft prennent en charge le cycle de vie](https://support.microsoft.com/gp/lifeselect).|Null|

> [!NOTE]
> Cette stratégie s’applique uniquement lorsque Mises à jour automatiques est configurée pour effectuer des installations planifiées de mises à jour. Si le paramètre de stratégie « configurer le Mises à jour automatiques » est désactivé, cette stratégie n’a aucun effet.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie que Mises à jour automatiques avertira l’utilisateur que l’ordinateur redémarrera automatiquement dans cinq minutes pour terminer l’installation.|
|**Activé**|Certaines mises à jour nécessitent le redémarrage de l’ordinateur pour que les mises à jour soient prises en compte. Si l’État est défini sur activé, Mises à jour automatiques ne redémarre pas automatiquement un ordinateur pendant une installation planifiée si un utilisateur est connecté à l’ordinateur. Au lieu de cela, Mises à jour automatiques avertit l’utilisateur qu’il doit redémarrer l’ordinateur.|
|**Désactivé**|Spécifie que Mises à jour automatiques avertira l’utilisateur que l’ordinateur redémarrera automatiquement dans cinq minutes pour terminer l’installation.|

**Options:** Il n’existe aucune option pour ce paramètre.

#### <a name="re-prompt-for-restart-with-scheduled-installations"></a>Redemander un redémarrage avec les installations planifiées
Spécifie la durée d’attente de Mises à jour automatiques avant de redemander un redémarrage planifié.

|Pris en charge sur :|Exclus|
|---------|-------|
|Les systèmes d’exploitation Windows qui se trouvent toujours dans les [produits Microsoft prennent en charge le cycle de vie](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!IMPORTANT]
> Cette stratégie s’applique uniquement lorsque Mises à jour automatiques est configurée pour effectuer des installations planifiées de mises à jour. Si le paramètre de stratégie « configurer le Mises à jour automatiques » est désactivé, cette stratégie n’a aucun effet.

> [!NOTE]
> Cette stratégie n’a aucun effet sur les ordinateurs exécutant Windows RT.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Un redémarrage planifié se produit dix minutes après l’avertissement de l’invite de redémarrage du message. Un administrateur local peut modifier ce paramètre à l’aide d’une stratégie locale.|
|**Activé**|Spécifie qu’après le report de l’invite précédente pour le redémarrage, un redémarrage planifié se produit après le nombre de minutes spécifié.|
|**Désactivé**|Un redémarrage planifié se produit dix minutes après l’avertissement de l’invite de redémarrage du message.|

**Options:** Lorsque cette option est activée, vous pouvez utiliser cette option pour spécifier (en minutes) la durée qui doit s’écouler avant que les utilisateurs ne soient de nouveau invités à un redémarrage planifié.

#### <a name="reschedule-automatic-updates-scheduled-installations"></a>Replanifier les installations planifiées des mises à jour automatiques
Spécifie la durée d’attente de Mises à jour automatiques après un démarrage de l’ordinateur, avant de procéder à une installation planifiée qui a été précédemment manquée.

Si l’État est défini sur **non configuré**, une installation planifiée manquée se produit une minute après le prochain démarrage de l’ordinateur.

|Pris en charge sur :|Exclus|
|---------|-------|
|Les systèmes d’exploitation Windows qui se trouvent toujours dans les [produits Microsoft prennent en charge le cycle de vie](https://support.microsoft.com/gp/lifeselect).|Null|

> [!NOTE]
> Cette stratégie s’applique uniquement lorsque Mises à jour automatiques est configurée pour effectuer des installations planifiées de mises à jour. Si le paramètre de stratégie « configurer le Mises à jour automatiques » est désactivé, cette stratégie n’a aucun effet.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie qu’une installation planifiée manquée aura lieu une minute après le prochain démarrage de l’ordinateur.|
|**Activé**|Spécifie qu’une installation planifiée qui n’a pas eu lieu précédemment aura lieu le nombre de minutes spécifié après le prochain démarrage de l’ordinateur.|
|**Désactivé**|Spécifie qu’une installation planifiée manquée aura lieu lors de la prochaine installation planifiée.|

**Options:** Lorsque ce paramètre de stratégie est activé, vous pouvez l’utiliser pour spécifier un nombre de minutes après le prochain démarrage de l’ordinateur, qu’une installation planifiée qui n’a pas eu lieu précédemment aura lieu.

#### <a name="specify-intranet-microsoft-update-service-location"></a>Spécifier l’emplacement intranet du service de mise à jour Microsoft
Spécifie un serveur intranet qui héberge les mises à jour provenant de Microsoft Update. Vous pouvez ensuite utiliser WSUS pour mettre à jour automatiquement les ordinateurs sur votre réseau.

|Pris en charge sur :|Exclus|
|---------|-------|
|Les systèmes d’exploitation Windows qui se trouvent toujours dans les [produits Microsoft prennent en charge le cycle de vie](https://support.microsoft.com/gp/lifeselect).|Windows RT.|

Ce paramètre vous permet de spécifier un serveur WSUS sur votre réseau qui fonctionnera comme un service de mise à jour interne. Au lieu d’utiliser les mises à jour Microsoft sur Internet, les clients WSUS recherchent les mises à jour qui s’appliquent à ce service.

Pour utiliser ce paramètre, vous devez définir deux valeurs de nom de serveur : le serveur à partir duquel le client détecte et télécharge les mises à jour, ainsi que le serveur sur lequel les stations de travail mises à jour chargent les statistiques. Les valeurs ne doivent pas être différentes si les deux services sont configurés sur le même serveur.

> [!NOTE]
> Si le paramètre de stratégie « configurer le Mises à jour automatiques » est désactivé, cette stratégie n’a aucun effet.

> [!NOTE]
> Cette stratégie n’est pas prise en charge sur Windows RT. L’activation de cette stratégie n’aura aucun effet sur les ordinateurs exécutant Windows RT.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Si Mises à jour automatiques n’est pas désactivé par la stratégie ou par la préférence de l’utilisateur, cette stratégie spécifie que les clients se connectent directement au site Windows Update sur Internet.|
|**Activé**|Spécifie que le client se connecte au serveur WSUS spécifié, au lieu de Windows Update, pour rechercher et télécharger des mises à jour. L’activation de ce paramètre signifie que les utilisateurs finaux de votre organisation n’ont pas besoin de passer par un pare-feu pour obtenir des mises à jour et qu’il vous donne la possibilité de tester les mises à jour avant de les déployer.|
|**Désactivé**|Si Mises à jour automatiques n’est pas désactivé par la stratégie ou par la préférence de l’utilisateur, cette stratégie spécifie que les clients se connectent directement au site Windows Update sur Internet.|

**Options:** Lorsque ce paramètre de stratégie est activé, vous devez spécifier le service intranet de mise à jour qui sera utilisé par les clients WSUS lors de la détection des mises à jour, ainsi que le serveur Internet Statistics sur lequel les clients WSUS mis à jour chargent les statistiques. Exemples de valeurs


|                    Option de paramétrage :                    |    Exemple de valeur :    |
|-------------------------------------------------------|----------------------|
| Configurer le service intranet de mise à jour pour la détection des mises à jour |  http://wsus01:8530  |
|          Définir le serveur intranet Statistics           | http://IntranetUpd01 |

#### <a name="turn-on-recommended-updates-via-automatic-updates"></a>Activer les mises à jour recommandées via Mises à jour automatiques
Spécifie si Mises à jour automatiques fournira des mises à jour importantes et recommandées à partir de WSUS.

|Pris en charge sur :|Exclus|
|---------|-------|
|à compter de Windows Vista, les systèmes d’exploitation Windows qui se trouvent toujours dans leurs [produits Microsoft prennent en charge le cycle de vie](https://support.microsoft.com/gp/lifeselect).|Null|

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie que Mises à jour automatiques continuera à fournir des mises à jour importantes s’il est déjà configuré pour ce faire.|
|**Activé**|Spécifie que Mises à jour automatiques installera les mises à jour recommandées et les mises à jour importantes de WSUS.|
|**Désactivé**|Spécifie que Mises à jour automatiques continuera à fournir des mises à jour importantes s’il est déjà configuré pour ce faire.|

**Options:** Il n’existe aucune option pour ce paramètre.

#### <a name="turn-on-software-notifications"></a>Activer les notifications de logiciels
Ce paramètre de stratégie vous permet de contrôler si les utilisateurs voient les messages de notification améliorés détaillés concernant les logiciels proposés du service Microsoft Update. Les messages de notification améliorés communiquent la valeur et favorisent l’installation et l’utilisation de logiciels facultatifs. Ce paramètre de stratégie est destiné à être utilisé dans des environnements peu gérés dans lesquels vous autorisez l’utilisateur final à accéder au service Microsoft Update.

Si vous n’utilisez pas le service Microsoft Update, le paramètre de stratégie « notifications logicielles » n’a aucun effet.

Si le paramètre de stratégie « configurer le Mises à jour automatiques » est désactivé ou n’est pas configuré, le paramètre de stratégie « notifications de logiciels » n’a aucun effet.

|Pris en charge sur :|Exclus|
|---------|-------|
|à compter de Windows Server 2008 (Windows Vista) et de Windows 7, les systèmes d’exploitation Windows qui se trouvent toujours dans leurs [produits Microsoft prennent en charge le cycle de vie](https://support.microsoft.com/gp/lifeselect).|Null|

> [!NOTE]
> Par défaut, ce paramètre de stratégie est désactivé.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Les utilisateurs sur les ordinateurs qui exécutent Windows 7 ne bénéficient pas de messages pour les applications facultatives. Les utilisateurs sur les ordinateurs qui exécutent Windows Vista ne disposent pas de messages pour les applications ou mises à jour facultatives. Un administrateur local peut modifier ce paramètre à l’aide du panneau de configuration ou d’une stratégie locale.|
|**Activé**|Si vous activez ce paramètre de stratégie, un message de notification s’affiche sur l’ordinateur de l’utilisateur lorsque le logiciel proposé est disponible. L’utilisateur peut cliquer sur la notification pour ouvrir Windows Update et obtenir des informations supplémentaires sur le logiciel ou l’installer. L’utilisateur peut également cliquer sur **fermer ce message** ou **m’afficher plus tard** pour différer la notification, le cas échéant.<br /><br />Dans Windows 7, ce paramètre de stratégie ne contrôle que les notifications détaillées des applications facultatives. Dans Windows Vista, ce paramètre de stratégie contrôle les notifications détaillées pour les applications et mises à jour facultatives.|
|**Désactivé**|Spécifie que les utilisateurs qui exécutent Windows 7 ne recevront pas de messages de notification détaillés pour les applications facultatives et que les utilisateurs qui exécutent Windows Vista ne recevront pas de messages de notification détaillés pour les applications facultatives ou les mises à jour facultatives.|

**Options:** Il n’existe aucune option pour ce paramètre.

### <a name="computer-configuration--maintenance-scheduler-policy-settings"></a>Configuration de l’ordinateur > les paramètres de stratégie du planificateur de maintenance
Dans le paramètre configurer le Mises à jour automatiques, vous avez sélectionné l’option **4 : Téléchargement automatique et planification de l’installation**, vous pouvez spécifier les paramètres du planificateur de maintenance planifier dans la console GPMC pour les ordinateurs exécutant Windows 8 et Windows RT. Si vous n’avez pas sélectionné l’option 4 dans le paramètre « configurer le Mises à jour automatiques », vous n’avez pas besoin de configurer ces paramètres dans le cadre des mises à jour automatiques. Les paramètres du planificateur de maintenance se trouvent dans le chemin d’accès : *PolicyName* > **Configuration ordinateur** > **stratégies** > **modèles d’administration** **composants Windows** >   > **maintenance Scheduler**. L’extension du planificateur de maintenance de stratégie de groupe contient les paramètres suivants :

-   [Limite d’activation de maintenance automatique](#automatic-maintenance-activation-boundary)

-   [Délai aléatoire de maintenance automatique](#automatic-maintenance-random-delay)

-   [Stratégie de réveil automatique](#automatic-wakeup-policy)

#### <a name="automatic-maintenance-activation-boundary"></a>Limite de l’activation de la maintenance automatique
Cette stratégie vous permet de configurer le paramètre « limite d’activation de maintenance automatique ».

La limite d’activation de maintenance est l’heure planifiée quotidienne à laquelle la maintenance automatique démarre.

|Pris en charge sur :|Exclus|
|---------|-------|
|Les systèmes d’exploitation Windows qui se trouvent toujours dans les [produits Microsoft prennent en charge le cycle de vie](https://support.microsoft.com/gp/lifeselect).|Null|

> [!NOTE]
> Ce paramètre est lié à l’option 4 dans **configurer mises à jour automatiques**. Si vous n’avez pas sélectionné l’option 4 dans **configurer mises à jour automatiques**, il n’est pas nécessaire de configurer ce paramètre.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Si ce paramètre de stratégie n’est pas configuré, l’heure planifiée quotidienne telle que spécifiée sur les ordinateurs clients dans le panneau de configuration du **Centre**de maintenance  > **automatique** s’applique.|
|**Activé**|L’activation de ce paramètre de stratégie remplace tous les paramètres par défaut ou modifiés configurés sur les ordinateurs clients dans **le panneau de configuration**@no__t **-1** **maintenance automatique**  >  (ou dans certaines versions du client, **maintenance**).|
|**Désactivé**|Si vous définissez ce paramètre de stratégie sur **désactivé**, l’heure planifiée quotidienne telle que spécifiée dans le **centre**de maintenance  > **maintenance automatique**, dans le panneau de configuration s’applique.|

#### <a name="automatic-maintenance-random-delay"></a>Délai aléatoire de maintenance automatique
Ce paramètre de stratégie vous permet de configurer le délai aléatoire d’activation de maintenance automatique.

Le délai aléatoire de maintenance correspond à la durée pendant laquelle la maintenance automatique retardera à partir de sa limite d’activation. Ce paramètre est utile pour les machines virtuelles où une maintenance aléatoire peut être une exigence en matière de performances.

|Pris en charge sur :|Exclus|
|---------|-------|
|Les systèmes d’exploitation Windows qui se trouvent toujours dans les [produits Microsoft prennent en charge le cycle de vie](https://support.microsoft.com/gp/lifeselect).|Null|

> [!NOTE]
> Ce paramètre est lié à l’option 4 dans **configurer mises à jour automatiques**. Si vous n’avez pas sélectionné l’option 4 dans **configurer mises à jour automatiques**, il n’est pas nécessaire de configurer ce paramètre.

Par défaut, lorsque cette option est activée, le délai aléatoire de maintenance régulière est défini sur **PT4H**.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Un délai aléatoire de quatre heures est appliqué à **automatique**.|
|**Activé**|La maintenance automatique retarde le démarrage de sa limite d’activation jusqu’à la durée spécifiée.|
|**Désactivé**|Aucun délai aléatoire n’est appliqué à la maintenance automatique.|

#### <a name="automatic-wakeup-policy"></a>Stratégie de réveil automatique
Ce paramètre de stratégie vous permet de configurer la stratégie de mise en éveil automatique de la maintenance.

La stratégie de mise en éveil de la maintenance spécifie si la maintenance automatique doit effectuer une demande de mise en éveil sur l’ordinateur d’exploitation pour une maintenance quotidienne planifiée.

|Pris en charge sur :|Exclus|
|---------|-------|
|Les systèmes d’exploitation Windows qui se trouvent toujours dans les [produits Microsoft prennent en charge le cycle de vie](https://support.microsoft.com/gp/lifeselect).|Null|

> [!NOTE]
> Si la stratégie de mise en éveil de l’ordinateur d’exploitation est explicitement désactivée, ce paramètre n’a aucun effet.

> [!NOTE]
> Ce paramètre est lié à l’option 4 dans **configurer mises à jour automatiques**. Si vous n’avez pas sélectionné l’option 4 dans **configurer mises à jour automatiques**, il n’est pas nécessaire de configurer ce paramètre.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Si vous ne configurez pas ce paramètre de stratégie, le paramètre de mise en éveil tel que spécifié **dans le panneau de configuration** > **maintenance automatique** s’applique.|
|**Activé**|Si vous activez ce paramètre de stratégie, la maintenance automatique tente de définir une stratégie de mise en éveil du système d’exploitation et effectue une requête de mise en éveil pour l’heure planifiée quotidienne, si nécessaire.|
|**Désactivé**|Si vous désactivez ce paramètre de stratégie, le paramètre de mise en éveil tel que spécifié **dans le panneau de configuration** > **maintenance automatique** s’applique.|

### <a name="user-configuration--windows-update-policy-settings"></a>Configuration de l’utilisateur > Windows Update paramètres de stratégie
Cette section fournit des détails sur les paramètres de stratégie basés sur l’utilisateur suivants :

-   [Ne pas afficher l’option « installer les mises à jour et éteindre » dans la boîte de dialogue arrêter Windows](#do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog)

-   [Ne pas définir l’option par défaut sur « installer les mises à jour et les arrêter » dans la boîte de dialogue arrêter Windows](#do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog)

-   [supprimer l’accès pour utiliser toutes les fonctionnalités de Windows Update](#remove-access-to-use-all-windows-update-features)

Dans la console GPMC, les paramètres utilisateur pour les mises à jour automatiques de l’ordinateur se trouvent dans le chemin d’accès suivant : *PolicyName* > **Configuration utilisateur** > **stratégies** > **modèles d’administration** **composants Windows** >   > **Windows Update**. Les paramètres sont répertoriés dans l’ordre dans lequel ils apparaissent dans les extensions Configuration ordinateur et configuration utilisateur dans stratégie de groupe, lorsque l’onglet **paramètres** de la stratégie Windows Update est sélectionné pour trier les paramètres par ordre alphabétique.

> [!NOTE]
> Par défaut, sauf indication contraire, ces paramètres ne sont pas configurés.

> [!TIP]
> pour chacun de ces paramètres, vous pouvez utiliser les étapes suivantes pour activer, désactiver ou naviguer entre les paramètres :

#### <a name="do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog-box"></a>Ne pas afficher l’option « installer les mises à jour et éteindre » dans la boîte de dialogue arrêter Windows
Spécifie si l’option **installer les mises à jour et les arrêts** s’affiche dans la boîte de dialogue **arrêter Windows** .

|Pris en charge sur :|Exclus|
|---------|-------|
|Les systèmes d’exploitation Windows qui se trouvent toujours dans les [produits Microsoft prennent en charge le cycle de vie](https://support.microsoft.com/gp/lifeselect).|Null|

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie que l’option **installer les mises à jour et arrêter** apparaît dans la boîte de dialogue **arrêter Windows** si des mises à jour sont disponibles lorsque l’utilisateur sélectionne l’option arrêter pour arrêter l’ordinateur.|
|**Activé**|Si vous activez ce paramètre de stratégie, l’option **installer les mises à jour et arrêter** n’apparaît pas dans la boîte de dialogue **arrêter Windows** , même si des mises à jour sont disponibles pour l’installation lorsque l’utilisateur sélectionne l’option arrêter pour arrêter l’ordinateur.|
|**Désactivé**|Spécifie que l’option **installer les mises à jour et arrêter** apparaît dans la boîte de dialogue **arrêter Windows** si des mises à jour sont disponibles lorsque l’utilisateur sélectionne l’option arrêter pour arrêter l’ordinateur.|

**Options:** Il n’existe aucune option pour ce paramètre.

#### <a name="do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog-box"></a>Ne pas définir l’option par défaut sur « installer les mises à jour et les arrêter » dans la boîte de dialogue arrêter Windows
Spécifie si l’option **installer les mises à jour et les arrêts** est autorisée comme choix par défaut dans la boîte de dialogue **arrêter Windows** .

|Pris en charge sur :|Exclus|
|---------|-------|
|Les systèmes d’exploitation Windows qui se trouvent toujours dans les [produits Microsoft prennent en charge le cycle de vie](https://support.microsoft.com/gp/lifeselect).|Null|

> [!NOTE]
> Ce paramètre de stratégie n’a aucun impact *si la configuration de l'* utilisateur de la**configuration**de l’utilisateur  >  de la Configuration de l’utilisateur  > **stratégies** > **modèles d’administration** **composants Windows** >  @no__t **-9 Windows Update @no__** t-11**n’affiche pas l’option « installer les mises à jour et éteindre » dans la boîte de dialogue arrêt de Windows** est activée.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie si l’option **installer les mises à jour et éteindre** est l’option par défaut dans la boîte de dialogue **arrêter Windows** si des mises à jour sont disponibles pour l’installation au moment où l’utilisateur sélectionne l’option arrêter pour arrêter l’ordinateur.|
|**Activé**|Spécifie si la dernière option arrêter le choix (par exemple, mettre en veille prolongée ou redémarrer) est l’option par défaut dans la boîte de dialogue **arrêt de Windows** , que l' **option installer les mises à jour et arrêter** soit disponible dans la boîte de dialogue **que voulez-vous faire ? vous voulez que l’ordinateur fasse ?** menus.|
|**Désactivé**|Spécifie si l’option **installer les mises à jour et éteindre** est l’option par défaut dans la boîte de dialogue **arrêter Windows** si des mises à jour sont disponibles pour l’installation au moment où l’utilisateur sélectionne l’option arrêter pour arrêter l’ordinateur.|

**Options:** Il n’existe aucune option pour ce paramètre.

#### <a name="remove-access-to-use-all-windows-update-features"></a>Supprimer l’accès à l’utilisation de toutes les fonctionnalités Windows Update
Ce paramètre vous permet de supprimer l’accès client WSUS aux Windows Update.

|Pris en charge sur :|Exclus|
|---------|-------|
|Les systèmes d’exploitation Windows qui se trouvent toujours dans les [produits Microsoft prennent en charge le cycle de vie](https://support.microsoft.com/gp/lifeselect).|Null|

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Les utilisateurs sont en mesure de se connecter au site Web Windows Update.|
|**Activé**|**Important :** si l’activation est activée, toutes les fonctionnalités de Windows Update sont supprimées. Cela comprend le blocage de l’accès au site Web Windows Update sur https://windowsupdate.microsoft.com, à partir du lien Windows Update dans le menu Démarrer ou l’écran d’accueil, ainsi que dans le menu **Outils** d’Internet Explorer. La mise à jour automatique de Windows est également désactivée ; l’utilisateur ne sera pas averti et ne recevra pas les mises à jour critiques de Windows Update. Ce paramètre empêche également Device Manager d’installer automatiquement les mises à jour de pilotes à partir du site Web Windows Update.<br /><br />Lorsque cette option est activée, vous pouvez configurer l’une des options de notification suivantes :<br /><br />-   **0-ne pas afficher de notifications**<br />    Ce paramètre supprime tous les accès aux fonctionnalités de Windows Update et aucune notification ne s’affiche.<br />-   **1-afficher les notifications de redémarrage requis**<br />    Ce paramètre affiche les notifications concernant les redémarrages nécessaires à l’exécution d’une installation. **Remarque :** Sur les ordinateurs exécutant Windows 8 et Windows RT, si cette stratégie est activée, seules les notifications liées au redémarrage et l’incapacité à détecter les mises à jour sont affichées. Les options de notification ne sont pas prises en charge. Les notifications sur l’écran de connexion s’affichent toujours.|
|**Désactivé**|Les utilisateurs sont en mesure de se connecter au site Web Windows Update.|

**Options:** Consultez **activé** dans le tableau pour ce paramètre.

## <a name="supplemental-information"></a>Informations supplémentaires
Cette section fournit des informations supplémentaires sur l’utilisation de l’ouverture et de l’enregistrement des paramètres WSUS dans les stratégies de groupe, ainsi que des définitions pour les termes utilisés dans ce guide. Pour les administrateurs connaissant les versions antérieures de WSUS (WSUS 3,2 et versions antérieures), il existe un tableau qui résume brièvement les différences entre les versions de WSUS.

### <a name="accessing-the-windows-update-settings-in-group-policy"></a>Accès aux paramètres de Windows Update dans stratégie de groupe
La procédure qui suit décrit comment ouvrir la console GPMC sur votre contrôleur de domaine. La procédure décrit ensuite comment ouvrir un objet stratégie de groupe au niveau du domaine existant pour la modification, ou créer un objet de stratégie de groupe de niveau domaine et l’ouvrir pour le modifier.

> [!NOTE]
> Pour effectuer cette procédure, vous devez être membre du groupe **Admins du domaine** ou d’un groupe équivalent.

##### <a name="to-open-or-add-and-open-a-group-policy-object"></a>Pour ouvrir ou ajouter et ouvrir un objet stratégie de groupe

1.  Sur votre contrôleur de domaine, accédez à **Gestionnaire de serveur**, > **Tools**, > **stratégie de groupe Management**. Le Console de gestion des stratégies de groupe s’ouvre.

2.  Dans le volet gauche, développez votre forêt. Par exemple, double-cliquez sur **Forest : example.com**.

3.  Dans le volet gauche, double-cliquez sur **domaines**, puis double-cliquez sur le domaine pour lequel vous souhaitez gérer un objet stratégie de groupe. Par exemple, double-cliquez sur **example.com**.

4.  Faites une des actions suivantes :

    -  **Pour ouvrir un objet de stratégie de groupe existant au niveau du domaine pour le modifier**, double-cliquez sur le domaine qui contient l’objet stratégie de groupe que vous souhaitez gérer, cliquez avec le bouton droit sur la stratégie de domaine que vous souhaitez gérer, puis cliquez sur **modifier**. Stratégie de groupe Management Editor (éditeur) s’ouvre.

    -  **Pour créer un nouvel objet stratégie de groupe et l’ouvrir pour le modifier**:
        1.  Cliquez avec le bouton droit sur le domaine pour lequel vous souhaitez créer un objet stratégie de groupe, puis cliquez sur **créer un objet de stratégie de groupe dans ce domaine, et le lier ici**.

        2.  Dans **nouvel**objet de stratégie de groupe, dans **nom**, tapez un nom pour le nouvel objet stratégie de groupe, puis cliquez sur **OK**.

        3.  Cliquez avec le bouton droit sur votre nouvel objet stratégie de groupe, puis cliquez sur **modifier**. ÉDITEUR s’ouvre.

##### <a name="to-open-the-windows-update-or-maintenance-scheduler-extensions-of-group-policy"></a>Pour ouvrir le Windows Update ou les extensions du planificateur de maintenance de stratégie de groupe

1.  Dans stratégie de groupe éditeur de gestion, effectuez l’une des opérations suivantes :

    -   **Ouvrez la configuration de l’ordinateur > Windows Update extension de stratégie de groupe**. Accédez à : *PolicyName* > **Configuration ordinateur** > **stratégies** / **modèles d’administration** **composants Windows**@no__t-**7  >  Windows Update**.

    -   **Ouvrez la configuration utilisateur > Windows Update extension de stratégie de groupe**. Accédez à : *PolicyName* > **Configuration utilisateur** > **stratégies** > **modèles d’administration** **composants Windows** >   > **Windows Update**.

    -   **Ouvrez la configuration de l’ordinateur > l’extension du planificateur de maintenance de stratégie de groupe**. Dans GPOE, accédez à la**configuration d’ordinateur**de *PolicyName* >   > **stratégies** > **modèles d’administration** > **composants Windows** > **maintenance Scheduler**.

Pour plus d’informations sur la stratégie de groupe, consultez [stratégie de groupe vue d’ensemble](https://technet.microsoft.com/library/hh831791.aspx(v=ws.12)).

> [!TIP]
> Une fois que vous avez ouvert l’extension de stratégie de groupe souhaitée, vous pouvez utiliser les étapes suivantes pour activer, désactiver ou naviguer entre les paramètres :

##### <a name="to-configure-group-policy-settings"></a>Pour configurer les paramètres de stratégie de groupe

1.  Dans *ExtensionOfGroupPolicy*, double-cliquez sur le paramètre que vous souhaitez afficher ou modifier.

2.  Pour configurer le paramètre, effectuez l’une des opérations suivantes :

    -   Pour conserver l’état non spécifié par défaut du paramètre, sélectionnez **non configuré**.

    -   Pour activer le paramètre, sélectionnez **activé**.

    -   Pour désactiver le paramètre, sélectionnez **désactivé**.

3.  Dans **options**, si des options sont répertoriées, conservez les valeurs par défaut ou modifiez-les si nécessaire.

4.  Faites une des actions suivantes :

    -   Pour enregistrer vos modifications et passer au paramètre suivant, cliquez sur **appliquer**, puis sur **paramètre suivant**.

    -   Pour enregistrer vos modifications et fermer la boîte de dialogue, cliquez sur **OK**.

    -   Pour ignorer toutes les modifications non enregistrées et fermer la boîte de dialogue, cliquez sur **Annuler**.

### <a name="changes-to-wsus-relevant-to-this-guide"></a>Modifications apportées à WSUS en ce qui concerne ce guide
Le tableau suivant récapitule les principales différences entre les versions actuelles et précédentes de WSUS qui sont pertinentes pour ce guide.

|Versions de Windows Server et WSUS|Description|
|------------------|--------|
| Windows Server 2012 R2 avec WSUS 6,0 et versions ultérieures|à partir de Windows Server 2012, le rôle serveur WSUS est intégré au système d’exploitation et les paramètres de stratégie de groupe associés pour les clients WSUS sont, par défaut, inclus dans stratégie de groupe.|
| Windows Server 2008 (et les versions antérieures de Windows Server) avec WSUS 3,2 et versions antérieures|Dans Windows Server 2008 (et les versions antérieures de Windows Server) utilisant WSUS 3,2 (et versions antérieures), les paramètres de stratégie de groupe qui régissent les clients WSUS ne sont pas inclus dans ces systèmes d’exploitation Windows Server. Les paramètres de stratégie se trouvent dans le modèle d’administration WSUS, **wuau. adm**. Dans ces versions de serveur, le modèle d’administration WSUS doit d’abord être ajouté à la Console de gestion des stratégies de groupe (GPMC) pour que les paramètres du client WSUS puissent être configurés.|

### <a name="terms-and-definitions"></a>Termes et définitions
La liste suivante répertorie les termes utilisés dans ce guide.

|Terme|Définition|
|----|-------|
|Mises à jour automatiques|**Un service qui s’exécute sur des ordinateurs Windows** (mises à jour automatiques) : Fait référence au composant ordinateur client intégré aux systèmes d’exploitation Microsoft Windows Vista, Windows Server 2003, Windows XP et Windows 2000 avec SP3 pour l’extraction des mises à jour de Microsoft Update ou Windows Update.<br /><br />**Référence occasionnelle** (mises à jour automatiques) : Terme utilisé pour décrire le moment où l’agent de Windows Update planifie et télécharge automatiquement les mises à jour.|
|serveur autonome|Utilisez pour faire référence à un serveur Windows Server Update Services (WSUS) en aval sur lequel les administrateurs peuvent gérer les composants WSUS.|
|serveur en aval|Utilisez pour faire référence à un serveur Windows Server Update Services (WSUS) qui obtient les mises à jour à partir d’un autre serveur WSUS plutôt qu’à partir de Microsoft Update ou Windows Update.|
|Extension de stratégie de groupe (et : extension de stratégie de groupe|Collection de paramètres dans stratégie de groupe utilisés pour contrôler la façon dont les utilisateurs et les ordinateurs (auxquels les stratégies s’appliquent) peuvent configurer et utiliser différents services et fonctionnalités Windows. Les administrateurs peuvent utiliser WSUS avec stratégie de groupe pour la configuration côté client du client Mises à jour automatiques, afin de s’assurer que les utilisateurs finaux ne peuvent pas désactiver ou contourner les stratégies de mise à jour d’entreprise.<br /><br />WSUS ne nécessite pas l’utilisation d’Active Directory ou de stratégie de groupe. La configuration du client peut également être appliquée à l’aide de la stratégie de groupe locale ou en modifiant le Registre Windows.|
|service de mise à jour interne|Référence occasionnelle à une infrastructure réseau qui utilise un ou plusieurs serveurs WSUS pour distribuer des mises à jour.|
|serveur réplica|Utilisez pour faire référence à un serveur Windows Server Update Services (WSUS) en aval qui reflète les approbations et les paramètres sur le serveur en amont auquel il est connecté. Vous ne pouvez pas gérer WSUS sur un serveur de réplication.|
|Microsoft Update|**Un site de téléchargement Microsoft basé sur Internet :** Un site Internet Microsoft qui stocke et distribue les mises à jour pour les ordinateurs Windows (pilotes de périphérique), les systèmes d’exploitation Windows et d’autres produits logiciels Microsoft.|
|Services de mise à jour logicielle (SUS)|SUS était le produit prédécesseur de Windows Server Update Services (WSUS).|
|« mises à jour »|Une collection de révisions logicielles, des correctifs, des service packs, des Feature Packs et des pilotes de périphériques qui peuvent être installés sur un ordinateur pour étendre les fonctionnalités, ou améliorer les performances et la sécurité.|
|Fichiers de mise à jour|Fichiers requis pour installer une mise à jour sur un ordinateur.|
|informations de mise à jour (également appelées métadonnées de mise à jour)|Les informations relatives à une mise à jour, par opposition aux fichiers binaires de mise à jour d’un package de mise à jour. Par exemple, les métadonnées fournissent des informations sur les propriétés d’une mise à jour, ce qui vous permet de savoir ce que la mise à jour est utile. Les métadonnées incluent également les termes du contrat de licence logiciel Microsoft. Le package de métadonnées téléchargé pour une mise à jour est généralement plus petit que le package de fichiers de mise à jour réel.|
|mettre à jour la source|Emplacement vers lequel un serveur de Windows Server Update Services (WSUS) se synchronise pour obtenir les fichiers de mise à jour. Cet emplacement peut être Microsoft Update ou un serveur WSUS en amont.|
|serveur en amont|Serveur Windows Server Update Services (WSUS) qui fournit des fichiers de mise à jour à un autre serveur WSUS, qui est à son tour appelé serveur en aval.|
|Windows Server Update Services (WSUS)|Programme de rôle serveur qui s’exécute sur un ou plusieurs ordinateurs Windows Server sur un réseau d’entreprise. Une infrastructure WSUS vous permet de gérer les mises à jour des ordinateurs de votre réseau à installer.<br /><br />Vous pouvez utiliser WSUS pour approuver ou refuser des mises à jour avant la publication, pour forcer l’installation des mises à jour à une date donnée et pour obtenir des rapports détaillés sur les mises à jour nécessaires pour chaque ordinateur de votre réseau. Vous pouvez configurer WSUS pour approuver automatiquement certaines classes de mises à jour (mises à jour critiques, mises à jour de sécurité, service packs, pilotes, etc.). WSUS vous permet également d’approuver les mises à jour pour la « détection » uniquement, afin que vous puissiez voir quels ordinateurs nécessitent une mise à jour donnée sans avoir à installer les mises à jour.<br /><br />Dans une implémentation WSUS, au moins un serveur WSUS du réseau doit être en mesure de se connecter à Microsoft Update pour obtenir les mises à jour disponibles. En fonction de la configuration et de la sécurité du réseau, l’administrateur peut déterminer le nombre d’autres serveurs se connectant directement à Microsoft Update.<br /><br />Vous pouvez configurer un serveur WSUS pour obtenir des mises à jour sur Internet à partir de tels emplacements :<br /><br />-le Microsoft Update public<br />-le Windows Update public<br />-Microsoft Store|
|Windows Update|**Un site de téléchargement Microsoft basé sur Internet :** Un site Internet Microsoft qui stocke et distribue les mises à jour pour les ordinateurs Windows (pilotes de périphériques) et les systèmes d’exploitation Windows.<br /><br />**service de l’ordinateur :** Nom du service Windows Update qui s’exécute sur les ordinateurs. Windows Update détecte, télécharge et installe les mises à jour sur les ordinateurs Windows.<br /><br />En fonction des configurations de l’ordinateur et de la stratégie, l’agent de Windows Update peut télécharger des mises à jour à partir de :<br /><br />-Microsoft Update<br />-Windows Update<br />-Microsoft Store<br />-Un service de mise à jour (WSUS) Internet<br /><br />les ordinateurs qui ne sont pas gérés dans un environnement WSUS utilisent généralement Windows Update pour se connecter directement à Internet, Windows Update, Microsoft Update ou Microsoft Store pour obtenir des mises à jour.|
|Client WSUS|Ordinateur qui reçoit les mises à jour d’un service de mise à jour intranet WSUS.<br /><br />Dans le cas de stratégie de groupe paramètres qui contrôlent l’interaction de l’utilisateur final avec Mises à jour automatiques : un utilisateur d’un ordinateur dans un environnement WSUS.|
