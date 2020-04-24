---
title: 'Étape 4 : Configurer les paramètres de stratégie de groupe pour la fonctionnalité Mises à jour automatiques'
description: Rubrique Windows Server Update Services (WSUS) – La configuration de paramètres de stratégie de groupe pour la fonctionnalité Mises à jour automatiques est la quatrième et dernière étape du déploiement de WSUS
ms.prod: windows-server
ms.technology: manage-wsus
ms.topic: article
ms.assetid: 62177d05-d832-4ea8-bca4-47a8cd34a19c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d678c139ae2327eeecdff2731f1edb57d358a28a
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80828842"
---
# <a name="step-4-configure-group-policy-settings-for-automatic-updates"></a>Étape 4 : Configurer les paramètres de stratégie de groupe pour la fonctionnalité Mises à jour automatiques

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dans un environnement Active Directory, vous pouvez utiliser une stratégie de groupe pour définir la manière dont les ordinateurs et les utilisateurs (mentionnés dans ce document en tant que clients WSUS) peuvent interagir avec les mises à jour Windows pour obtenir des mises à jour automatiques à partir de Windows Server Update Services (WSUS).

Cette rubrique contient deux sections principales :

La section [Paramètres de stratégie de groupe pour les mises à jour de clients WSUS](#group-policy-settings-for-wsus-client-updates) fournit des instructions normatives et des informations comportementales sur les paramètres de Windows Update et du Planificateur de maintenance de la stratégie de groupe, qui contrôlent la manière dont les clients WSUS peuvent interagir avec Windows Update pour obtenir des mises à jour automatiques.

La section [Informations supplémentaires](#supplemental-information) contient les sections suivantes :

-   La section [Accès aux paramètres de Windows Update dans la Stratégie de groupe](#accessing-the-windows-update-settings-in-group-policy) fournit des instructions générales sur l’utilisation de l’éditeur de Gestion des stratégies de groupe, ainsi que des informations sur l’accès aux extensions de stratégie Mettre à jour les services et aux paramètres du Planificateur de maintenance dans la Stratégie de groupe.

-   La section [Modifications apportées aux services WSUS en lien avec ce guide](#changes-to-wsus-relevant-to-this-guide) destinée aux administrateurs connaissant WSUS 3.2 et les versions antérieures fournit un bref résumé des principales différences entre les versions actuelle et antérieure des services WSUS en rapport avec ce guide.

-   La section [Termes et définitions](#terms-and-definitions) contient des définitions des différents termes relatifs aux services WSUS et aux services de mise à jour utilisés dans ce guide.

## <a name="group-policy-settings-for-wsus-client-updates"></a>Paramètres de stratégie de groupe pour les mises à jour de clients WSUS
Cette section fournit des informations sur les trois extensions de stratégie de groupe. Ces extensions contiennent les paramètres que vous pouvez utiliser pour configurer la façon dont les clients WSUS peuvent interagir avec Windows Update pour recevoir des mises à jour automatiques.

-   [Configuration ordinateur &gt; Paramètres de stratégie de Windows Update](#computer-configuration--windows-update-policy-settings)

-   [Configuration ordinateur &gt; Paramètres de stratégie du Planificateur de maintenance](#computer-configuration--maintenance-scheduler-policy-settings)

-   [Configuration utilisateur &gt; Paramètres de stratégie de Windows Update](#user-configuration--windows-update-policy-settings)

> [!NOTE]
> Cette rubrique part du principe que vous utilisez déjà une stratégie de groupe et que vous êtes familiarisé avec les stratégies de groupe. Si vous n’êtes pas familiarisé avec les stratégies de groupe, il est recommandé de passer en revue les informations contenues dans la section [Informations supplémentaires](#supplemental-information) de ce document avant de tenter de configurer les paramètres de stratégie pour WSUS.

### <a name="computer-configuration--windows-update-policy-settings"></a>Configuration ordinateur > Paramètres de stratégie de Windows Update
Cette section fournit des détails sur les paramètres de stratégie basés sur l’ordinateur suivants :

-   [Autoriser l’installation immédiate des mises à jour automatiques](#allow-automatic-updates-immediate-installation)

-   [Autoriser les non-administrateurs à recevoir les notifications de mise à jour](#allow-non-administrators-to-receive-update-notifications)

-   [Autoriser les mises à jour signées provenant d’un emplacement intranet du service de mise à jour Microsoft](#allow-signed-updates-from-an-intranet-microsoft-update-service-location)

-   [Fréquence de détection des mises à jour automatiques](#automatic-updates-detection-frequency)

-   [Configurer la fonctionnalité Mises à jour automatiques](#configure-automatic-updates)

-   [Délai de redémarrage pour les installations planifiées](#delay-restart-for-scheduled-installations)

-   [Ne pas définir l’option par défaut sur « Installer les mises à jour et éteindre » dans la boîte de dialogue Arrêt de Windows](#do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog)

-   [Ne pas afficher l’option « Installer les mises à jour et éteindre » dans la boîte de dialogue Arrêt de Windows](#do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog)

-   [Activer le ciblage côté client](#enable-client-side-targeting)

-   [Activation de la fonctionnalité de gestion de l’alimentation par Windows Update pour la sortie de veille automatique du système lors de l’installation de mises à jour planifiées](#enabling-windows-update-power-management-to-automatically-wake-up-the-computer-to-install-scheduled-updates)

-   [Pas de redémarrage automatique avec des utilisateurs connectés pour les installations planifiées de mises à jour automatiques](#no-auto-restart-with-logged-on-users-for-scheduled-automatic-updates-installations)

-   [Redemander un redémarrage avec les installations planifiées](#re-prompt-for-restart-with-scheduled-installations)

-   [Replanifier les installations planifiées des mises à jour automatiques](#reschedule-automatic-updates-scheduled-installations)

-   [Spécifier l’emplacement intranet du service de mise à jour Microsoft](#specify-intranet-microsoft-update-service-location)

-   [Activer les mises à jour recommandées par la fonctionnalité Mises à jour automatiques](#turn-on-recommended-updates-via-automatic-updates)

-   [Activer les notifications d’applications](#turn-on-software-notifications)

Dans l’éditeur de gestion des stratégies de groupe (GPME), les stratégies Windows Update pour la configuration informatique se trouvent dans le chemin d’accès : *PolicyName* > **Configuration ordinateur** > **Stratégies** > **Modèles d’administration** > **Composants Windows** > **Windows Update**.

> [!NOTE]
> Par défaut, ces paramètres ne sont pas configurés.

#### <a name="allow-automatic-updates-immediate-installation"></a>Autoriser l’installation immédiate des mises à jour automatiques
Spécifie si la fonctionnalité Mises à jour automatiques installera automatiquement les mises à jour qui n’interrompent pas les services Windows ou ne redémarrent pas Windows.

|Pris en charge sur :|Sauf :|
|---------|-------|
|systèmes d’exploitation Windows qui se trouvent toujours dans leur [cycle de vie du support de produits Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

> [!NOTE]
> Si le paramètre de stratégie Configurer les mises à jour automatiques est défini sur **Désactivé**, cette stratégie est sans effet.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie que les mises à jour ne sont pas immédiatement installées. Les administrateurs locaux peuvent modifier ce paramètre à l’aide de l’Éditeur de stratégie de groupe locale.|
|**Activé**|Spécifie que la fonctionnalité Mises à jour automatiques installe les mises à jour immédiatement après qu’elles ont été téléchargées et qu’elles sont prêtes pour installation.|
|**Désactivé**|Spécifie que les mises à jour ne sont pas immédiatement installées.|

**Options :** Il n’existe aucune option pour ce paramètre.

#### <a name="allow-non-administrators-to-receive-update-notifications"></a>Autoriser les non-administrateurs à recevoir les notifications de mise à jour
Spécifie si les utilisateurs non-administrateurs reçoivent des notifications de mise à jour en fonction du paramètre de stratégie Configurer les mises à jour automatiques.

|Pris en charge sur :|Sauf :|
|---------|-------|
|systèmes d’exploitation Windows qui se trouvent toujours dans leur [cycle de vie du support de produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Pour de détails, voir le tableau ci-dessous.|

> [!NOTE]
> Si le paramètre de stratégie Configurer les mises à jour automatiques est désactivé ou n’est pas configuré, ce paramètre de stratégie est sans effet.

> [!IMPORTANT]
> Depuis Windows 8 et Windows RT, ce paramètre de stratégie est activé par défaut. Dans toutes les versions antérieures de Windows, il est désactivé par défaut.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie que les utilisateurs voient toujours une fenêtre Compte de contrôle et qu’ils ont besoin d’autorisations élevées pour accomplir ces tâches. Un administrateur local peut modifier ce paramètre à l’aide de l’Éditeur de stratégie de groupe locale.|
|**Activé**|Spécifie que la fonctionnalité Mises à jour automatiques de Windows et Microsoft Update incluent des non-administrateurs lors de la détermination de l’utilisateur connecté qui recevra les notifications de mise à jour. Les utilisateurs non-administrateurs peuvent installer tout contenu de mise à jour facultatif, recommandé et IMPORTANT pour lequel ils reçoivent une notification. Les utilisateurs ne voient pas de fenêtre Contrôle de compte d’utilisateur et ils n’ont pas besoin d’autorisations élevées pour installer ces mises à jour, sauf en cas de mises à jour incluant des modifications de paramètres d’Interface utilisateur, de Contrat de licence utilisateur final ou de Windows Update.<p>Il existe deux situations dans lesquelles l’effet de ce paramètre dépend de l’ordinateur d’exploitation :<p>1.  **Masquer** ou **Restaurer** des mises à jour<br />2.  **Annuler** l’installation d’une mise à jour<p>Sous Windows Vista ou Windows XP, si ce paramètre de stratégie est activé, les utilisateurs ne voient pas de fenêtre Contrôle de compte d’utilisateur et n’ont pas besoin d’autorisations élevées pour masquer, restaurer ou annuler des mises à jour.<p>Sous Windows Vista, si ce paramètre de stratégie est activé, les utilisateurs ne voient pas de fenêtre Contrôle de compte d’utilisateur et n’ont pas besoin d’autorisations élevées pour masquer, restaurer ou annuler des mises à jour. Si ce paramètre de stratégie n’est pas activé, les utilisateurs voient toujours une fenêtre Contrôle de compte et ont besoin d’autorisations élevées pour masquer, restaurer ou annuler des mises à jour.<p>Sous Windows 7, ce paramètre de stratégie est sans effet. Les utilisateurs voient toujours une fenêtre Compte de contrôle et ils ont besoin d’autorisations élevées pour accomplir ces tâches.<p>Dans Windows 8 et Windows RT, ce paramètre de stratégie est sans effet.|
|**Désactivé**|Spécifie que seuls les administrateurs connectés reçoivent des notifications de mise à jour. **Remarque :** Dans Windows 8 et Windows RT, ce paramètre de stratégie est activé par défaut. Dans toutes les versions antérieures de Windows, il est désactivé par défaut.|

**Options :** Il n’existe aucune option pour ce paramètre.

#### <a name="allow-signed-updates-from-an-intranet-microsoft-update-service-location"></a>Autoriser les mises à jour signées provenant d’un emplacement intranet du service de mise à jour Microsoft
Spécifie si la fonctionnalité Mises à jour automatiques accepte les mises à jour signées par des entités autres que Microsoft quand elles se trouvent dans un emplacement intranet du service de mise à jour Microsoft.

|Pris en charge sur :|Sauf :|
|---------|-------|
|systèmes d’exploitation Windows qui se trouvent toujours dans leur [cycle de vie du support de produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!NOTE]
> Les mises à jour provenant d’un service autre qu’un service intranet de mise à jour Microsoft doivent toujours être signées par Microsoft et ne sont pas concernées par ce paramètre de stratégie.

> [!NOTE]
> Cette stratégie n’est pas prise en charge par Windows RT. L’activation de cette stratégie est sans effet sur les ordinateurs exécutant Windows RT.

**Options :** Il n’existe aucune option pour ce paramètre.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie que les mises provenant d’un emplacement intranet du service de mise à jour Microsoft doivent être signées par Microsoft.|
|**Activé**|Spécifie que la fonctionnalité Mises à jour automatiques accepte les mises à jour provenant d’un emplacement intranet du service de mise à jour Microsoft si elles sont signées par un certificat trouvé dans le magasin de certificats Éditeurs approuvés de l’ordinateur local.|
|**Désactivé**|Spécifie que les mises provenant d’un emplacement intranet du service de mise à jour Microsoft doivent être signées par Microsoft.|

**Options :** Il n’existe aucune option pour ce paramètre.

#### <a name="always-automatically-restart-at-the-scheduled-time"></a>Toujours redémarrer automatiquement à l’heure planifiée
Spécifie si un minuteur de redémarrage démarre toujours immédiatement après que Windows Update a installé des mises à jour importantes, au lieu d’envoyer d’abord une notification aux utilisateurs restés sur l’écran de connexion pendant au moins deux jours.

|Pris en charge sur :|Sauf :|
|---------|-------|
|systèmes d’exploitation Windows qui se trouvent toujours dans leur [cycle de vie du support de produits Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

> [!NOTE]
> Si le paramètre de stratégie Pas de redémarrage automatique avec des utilisateurs connectés pour les installations planifiées de mises à jour automatiques est activé, cette stratégie est sans effet.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie que Windows Update ne modifiera pas le comportement de redémarrage de l’ordinateur.|
|**Activé**|Spécifie si un minuteur de redémarrage démarre toujours immédiatement après que Windows Update a installé des mises à jour importantes, au lieu d’envoyer d’abord une notification aux utilisateurs restés sur l’écran de connexion pendant au moins deux jours.<p>Le minuteur de redémarrage peut être configuré pour démarrer avec n’importe quelle valeur comprise entre 15 et 180 minutes. Quand le temps configuré pour le minuteur expire, le redémarrage se poursuit même si des utilisateurs sont connectés à l’ordinateur.|
|**Désactivé**|Spécifie que Windows Update ne modifiera pas le comportement de redémarrage de l’ordinateur.|

**Options :** si ce paramètre est activé, vous pouvez spécifier le temps qui doit s’écouler entre la fin d’installation des mises à jour et un redémarrage forcé de l’ordinateur.

#### <a name="automatic-updates-detection-frequency"></a>Fréquence de détection des mises à jour automatiques
Spécifie la durée en heures pendant laquelle Windows attend avant de vérifier la disponibilité de nouvelles mises à jour. La durée d’attente exacte est déterminée en utilisant le nombre d’heures spécifiée ici moins un pourcentage compris entre zéro et vingt pour cent du nombre d’heures spécifié. Par exemple, si cette stratégie est utilisée pour spécifier une fréquence de détection de 20 heures, tous les clients auxquels cette stratégie est appliquée vérifient la disponibilité de mises à jour entre les 16e et 20e heures.

|Pris en charge sur :|Sauf :|
|---------|-------|
|systèmes d’exploitation Windows qui se trouvent toujours dans leur [cycle de vie du support de produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!NOTE]
> Le paramètre Spécifier l’emplacement intranet du service de mise à jour Microsoft doit être activé pour que cette stratégie prenne effet.
>
> Si le paramètre de stratégie Configurer les mises à jour automatiques est désactivé, cette stratégie est sans effet.

> [!NOTE]
> Cette stratégie n’est pas prise en charge par Windows RT. L’activation de cette stratégie est sans effet sur les ordinateurs exécutant Windows RT.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie que Windows recherche les mises à jour disponibles à l’intervalle par défaut de 22 heures.|
|**Activé**|Spécifie que Windows recherche les mises à jour disponibles à l’intervalle spécifié.|
|**Désactivé**|Spécifie que Windows recherche les mises à jour disponibles à l’intervalle par défaut de 22 heures.|

**Options :** si ce paramètre est activé, vous pouvez spécifier l’intervalle de temps (en heures) pendant lequel Windows Update attend avant de rechercher les mises à jour.

#### <a name="configure-automatic-updates"></a>Configurer la fonctionnalité Mises à jour automatiques
Spécifie si les mises à jour automatiques sont activées sur cet ordinateur.

|Pris en charge sur :|Sauf :|
|---------|-------|
|systèmes d’exploitation Windows qui se trouvent toujours dans leur [cycle de vie du support de produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

Si cette option est activée, vous devez sélectionner l’une des quatre options fournies dans ce paramètre de stratégie de groupe.

Pour utiliser ce paramètre, sélectionnez **Activé**, puis, dans **Options**, sous **Configuration de la mise à jour automatique**, sélectionnez l’une des options (2, 3, 4 ou 5).

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie que l’utilisation de la fonctionnalité Mises à jour automatiques n’est pas spécifiée au niveau de la stratégie de groupe. Toutefois, un administrateur de l’ordinateur peut toujours configurer la fonctionnalité Mises à jour automatiques dans le Panneau de configuration.|
|**Activé**|Spécifie que Windows reconnaît lorsque l’ordinateur est en ligne et utilise sa connexion Internet pour rechercher des mises à jour disponibles dans Windows Update.<p>Quand l’option est activée, les administrateurs locaux sont autorisés à utiliser le Panneau de configuration Windows Update pour sélectionner une option de configuration de leur choix. Cependant, les administrateurs locaux ne sont pas autorisés à désactiver la configuration de la fonctionnalité Mises à jour automatiques.<p>-   **2 - Notification des téléchargements et des installations**<br />    Quand Windows Update trouve des mises à jour qui s’appliquent à l’ordinateur, les utilisateurs sont notifiés que des mises à jour sont prêtes pour téléchargement. Les utilisateurs peuvent ensuite exécuter Windows Update pour télécharger et installer des mises à jour disponibles.<br />-   **3 - Téléchargement automatique et notification des installations** (paramètre par défaut)<br />    Windows Update recherche les mises à jour applicables et les télécharge en arrière-plan. L’utilisateur n’est pas notifié ou interrompu pendant le processus. Une fois les téléchargements terminés, les utilisateurs sont notifiés que des mises à jour sont prêtes pour installation. Les utilisateurs peuvent ensuite exécuter Windows Update pour installer les mises à jour téléchargées.<br />-   **4 - Téléchargement automatique et planification des installations**<br />    Vous pouvez spécifier la planification à l’aide des options de ce paramètre de stratégie de groupe. Si aucune planification n’est spécifiée, la planification par défaut pour toutes les installations est chaque jour à 3:00 h. Si des mises à jour nécessitent un redémarrage pour terminer l’installation, Windows redémarre automatiquement l’ordinateur (si un utilisateur est connecté à l’ordinateur quand Windows est prêt à redémarrer, l’utilisateur est notifié et peut choisir de retarder le redémarrage). **Remarque :** depuis Windows 8, vous pouvez configurer l’installation des mises à jour lors de la maintenance automatique au lieu d’utiliser une planification spécifique liée à Windows Update. La maintenance automatique installe les mises à jour quand l’ordinateur n’est pas utilisé, et évite de les installer quand l’ordinateur fonctionne sur batterie. Si la maintenance automatique ne parvient pas à installer les mises à jour dans les quelques jours qui suivent, Windows Update installe les mises à jour directement. Les utilisateurs sont ensuite notifiés qu’un redémarrage est en attente. Un redémarrage en attente n’a lieu que s’il n’y a aucun risque de perte de données accidentelle.    Vous pouvez spécifier les options de planification dans les paramètres du Planificateur de maintenance de l’Éditeur de gestion des stratégies de groupe, qui se trouvent dans le chemin d’accès *PolicyName* > **Configuration ordinateur** > **Stratégies** > **Modèles d’administration** > **Composants Windows** > **Planificateur de maintenance** > **Limite d’activation de la Maintenance automatique**. Consultez la section de ce document de référence intitulée : [Paramètres du Planificateur de maintenance](#computer-configuration--maintenance-scheduler-policy-settings), pour les détails de paramétrage.    **5 - Autoriser l’administrateur local à choisir les paramètres**<br />- Spécifie si les administrateurs locaux sont autorisés à utiliser le Panneau de configuration Mises à jour automatiques pour sélectionner une option de configuration telle que l’heure d’installation planifiée.<br />    Les administrateurs locaux ne sont pas autorisés à désactiver la configuration des mises à jour automatiques.|
|**Désactivé**|Spécifie que toutes les mises à jour des clients disponibles sur le service Windows Update public doivent être téléchargées manuellement à partir d’Internet et installées.|

#### <a name="delay-restart-for-scheduled-installations"></a>Délai de redémarrage pour les installations planifiées
Spécifie le temps pendant lequel la fonctionnalité Mises à jour automatiques attend avant d’opérer un redémarrage planifié.

|Pris en charge sur :|Sauf :|
|---------|-------|
|systèmes d’exploitation Windows qui se trouvent toujours dans leur [cycle de vie du support de produits Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

> [!NOTE]
> Cette stratégie s’applique uniquement lorsque la fonctionnalité Mises à jour automatiques est configurée pour effectuer des installations planifiées de mises à jour. Si le paramètre de stratégie Configurer les mises à jour automatiques est désactivé, cette stratégie est sans effet.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie qu’après l’installation des mises à jour, le délai d’attente par défaut de quinze minutes s’écoule avant le redémarrage planifié.|
|**Activé**|Spécifie que, lorsque l’installation est terminée, un redémarrage planifié se produit après l’expiration du nombre de minutes spécifié.|
|**Désactivé**|Spécifie qu’après l’installation des mises à jour, le délai d’attente par défaut de quinze minutes s’écoule avant le redémarrage planifié.|

**Options :** si ce paramètre est activé, vous pouvez utiliser cette option pour spécifier le temps (en minutes) pendant lequel la fonctionnalité Mises à jour automatiques attend avant d’opérer un redémarrage planifié.

#### <a name="do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog"></a>Ne pas définir l’option par défaut sur « Installer les mises à jour et éteindre » dans la boîte de dialogue Arrêt de Windows
Ce paramètre de stratégie vous permet de spécifier si l’option **Installer les mises à jour et éteindre** est autorisée comme choix par défaut dans la boîte de dialogue **Arrêt de Windows**.

|Pris en charge sur :|Sauf :|
|---------|-------|
|systèmes d’exploitation Windows qui se trouvent toujours dans leur [cycle de vie du support de produits Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

> [!NOTE]
> Ce paramètre de stratégie n’a aucune incidence si le paramètre de stratégie *NomStratégie* > **Configuration ordinateur** > **Stratégies** > **Modèles d’administration** > **Composants Windows** > **Windows Update** > **Ne pas afficher l’option Installer les mises à jour et éteindre dans la boîte de dialogue Arrêt de Windows** est activé.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie que l’option **Installer les mises à jour et éteindre** est l’option par défaut dans la boîte de dialogue **Arrêt de Windows** si des mises à jour sont disponibles pour installation au moment où l’utilisateur sélectionne l’option Arrêter pour arrêter l’ordinateur.|
|**Activé**|Si vous activez ce paramètre de stratégie, le dernier choix d’arrêt de l’utilisateur (par exemple, Mettre en veille prolongée ou Redémarrer) est l’option par défaut dans la boîte de dialogue **Arrêt de Windows**, indépendamment du fait que l’option **Installer les mises à jour et éteindre** soit ou non disponible dans le menu **Sue voulez-vous faire ?** .|
|**Désactivé**|Spécifie que l’option **Installer les mises à jour et éteindre** est l’option par défaut dans la boîte de dialogue **Arrêt de Windows** si des mises à jour sont disponibles pour installation au moment où l’utilisateur sélectionne l’option Arrêter pour arrêter l’ordinateur.|

**Options :** Il n’existe aucune option pour ce paramètre.

#### <a name="do-not-connect-to-any-windows-update-internet-locations"></a>Ne pas se connecter à des emplacements Internet Windows Update
Même quand Windows Update est configuré pour recevoir des mises à jour à partir d’un service de mise à jour intranet, il extrait régulièrement des informations du service Windows Update public afin d’activer les connexions ultérieures à Windows Update et à d’autres services tels que Microsoft Update ou Microsoft Store.

L’activation de cette stratégie a pour effet de désactiver la fonctionnalité chargée de récupérer périodiquement des informations du service Windows Server Update public, et peut avoir pour effet que la connexion à des services publics tels que Microsoft Store cesse de fonctionner.

|Pris en charge sur :|Sauf :|
|---------|-------|
|depuis Windows Server 2012 R2, Windows 8.1 ou Windows RT 8.1, les systèmes d’exploitation Windows qui se trouvent toujours dans leur [cycle de vie du support de produits Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

> [!NOTE]
> Cette stratégie s’applique uniquement quand cet ordinateur est configuré pour se connecter à un service de mise à jour intranet à l’aide du paramètre de stratégie Spécifier l’emplacement intranet du service de mise à jour Microsoft.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Le comportement par défaut pour récupérer des informations du service Windows Server Update public persiste.|
|**Activé**|Spécifie que les ordinateurs ne récupèrent pas d’informations du service Windows Server Update public.|
|**Désactivé**|Le comportement par défaut pour récupérer des informations du service Windows Server Update public persiste.|

**Options :** Il n’existe aucune option pour ce paramètre.

#### <a name="do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog"></a>Ne pas afficher l’option « Installer les mises à jour et éteindre » dans la boîte de dialogue Arrêt de Windows
Spécifie si l’option **Installer les mises à jour et éteindre** s’affiche dans la boîte de dialogue **Arrêt de Windows**.

|Pris en charge sur :|Sauf :|
|---------|-------|
|systèmes d’exploitation Windows qui se trouvent toujours dans leur [cycle de vie du support de produits Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie que l’option **Installer les mises à jour et éteindre** est disponible dans la boîte de dialogue **Arrêt de Windows** si des mises à jour sont disponibles quand l’utilisateur sélectionne l’option Arrêter pour arrêter l’ordinateur. Un administrateur local peut modifier ce paramètre à l’aide d’une stratégie locale.|
|**Activé**|Spécifie que l’option **Installer les mises à jour et éteindre** n’apparaît pas dans la boîte de dialogue **Arrêt de Windows**, même si des mises à jour sont disponibles pour installation lorsque l’utilisateur sélectionne l’option Arrêter pour arrêter l’ordinateur.|
|**Désactivé**|Spécifie que l’option **Installer les mises à jour et éteindre** est l’option par défaut dans la boîte de dialogue **Arrêt de Windows** si des mises à jour sont disponibles pour installation au moment où l’utilisateur sélectionne l’option Arrêter pour arrêter l’ordinateur.|

**Options :** Il n’existe aucune option pour ce paramètre.

#### <a name="enable-client-side-targeting"></a>Activer le ciblage côté client
Spécifie le ou les noms de groupes cibles qui sont configurés dans la console WSUS pour recevoir des mises à jour de WSUS.

|Pris en charge sur :|Sauf :|
|---------|-------|
|systèmes d’exploitation Windows qui se trouvent toujours dans leur [cycle de vie du support de produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!NOTE]
> Cette stratégie s’applique uniquement quand cet ordinateur est configuré pour prendre en charge les noms de groupes cibles spécifiés dans WSUS. Si le nom du groupe cible n’existe pas dans WSUS, il est ignoré jusqu’à ce qu’il soit créé. Si le paramètre de stratégie Spécifier l’emplacement intranet du service de Mise à jour Microsoft est désactivé ou n’est pas configuré, cette stratégie est sans effet.

> [!NOTE]
> Cette stratégie n’est pas prise en charge par Windows RT. L’activation de cette stratégie est sans effet sur les ordinateurs exécutant Windows RT.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie qu’aucune information sur le groupe cible n’est envoyée à WSUS. Un administrateur local peut modifier ce paramètre à l’aide d’une stratégie locale.|
|**Activé**|Spécifie que les informations sur le groupe cible spécifié sont envoyées à WSUS qui les utilise pour déterminer les mises à jour à déployer sur cet ordinateur. Si WSUS prend en charge plusieurs groupes cibles, vous pouvez utiliser cette stratégie pour spécifier plusieurs noms de groupes, séparés par des points-virgules, si vous avez ajouté les noms de groupes cibles dans la liste des groupes d’ordinateurs dans WSUS. Dans le cas contraire, vous devez spécifier un seul groupe.|
|**Désactivé**|Spécifie qu’aucune information sur le groupe cible n’est envoyée à WSUS.|

**Options :** Utilisez cet espace pour spécifier un ou plusieurs noms de groupes cibles.

#### <a name="enabling-windows-update-power-management-to-automatically-wake-up-the-computer-to-install-scheduled-updates"></a>Activation de la gestion de l’alimentation de Windows Update pour que l’ordinateur sorte de veille automatiquement afin d’installer les mises à jour planifiées
Spécifie si Windows Update utilise les fonctionnalités Gestion de l’alimentation ou Options d’alimentation de Windows pour sortir automatiquement l’ordinateur de la veille prolongée si des mises à jour sont planifiées pour installation.

L’ordinateur ne sort de veille automatiquement que si Windows Update est configuré pour installer automatiquement les mises à jour. Si l’ordinateur est en veille prolongée lorsque l’heure d’installation planifiée arrive et que des mises à jour doivent être appliquées, Windows Update utilise les fonctionnalités Gestion de l’alimentation ou Options d’alimentation de Windows pour sortir de veille automatiquement l’ordinateur afin d’installer les mises à jour. Windows Update sort également de veille l’ordinateur et installe une mise à jour si une échéance d’installation se produit.

L’ordinateur ne sort de veille que s’il existe des mises à jour à installer. Si l’ordinateur fonctionne sur batterie, lorsque Windows Update le sort de veille, il n’installe pas les mises à jour et l’ordinateur repasse automatiquement en veille prolongée après deux minutes.

|Pris en charge sur :|Sauf :|
|---------|-------|
|depuis Windows Vista et Windows Server 2008 (Windows 7), les systèmes d’exploitation Windows qui se trouvent toujours dans leur [cycle de vie du support de produits Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Windows Update ne sort pas l’ordinateur de la veille prolongée pour installer les mises à jour. Un administrateur local peut modifier ce paramètre à l’aide d’une stratégie locale.|
|**Activé**|Windows Update fait sortir l’ordinateur de la veille prolongée pour installer les mises à jour dans les conditions précédemment listées.|
|**Désactivé**|Windows Update ne sort pas l’ordinateur de la veille prolongée pour installer les mises à jour.|

**Options :** Il n’existe aucune option pour ce paramètre.

#### <a name="no-auto-restart-with-logged-on-users-for-scheduled-automatic-updates-installations"></a>Pas de redémarrage automatique avec des utilisateurs connectés pour les installations planifiées de mises à jour automatiques
Spécifie que, pour effectuer une installation planifiée, la fonctionnalité Mises à jour automatiques attend que l’ordinateur soit redémarré par un utilisateur connecté, au lieu de redémarrer l’ordinateur automatiquement.

|Pris en charge sur :|Sauf :|
|---------|-------|
|systèmes d’exploitation Windows qui se trouvent toujours dans leur [cycle de vie du support de produits Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

> [!NOTE]
> Cette stratégie s’applique uniquement lorsque la fonctionnalité Mises à jour automatiques est configurée pour effectuer des installations planifiées de mises à jour. Si le paramètre de stratégie Configurer les mises à jour automatiques est désactivé, cette stratégie est sans effet.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie que la fonctionnalité Mises à jour automatiques envoient une notification à l’utilisateur pour lui indiquer que l’ordinateur redémarrera automatiquement dans cinq minutes pour terminer l’installation.|
|**Activé**|Certaines mises à jour nécessitent le redémarrage de l’ordinateur pour prendre effet. Quand l’état est défini sur Activé, la fonctionnalité Mises à jour automatiques ne redémarre pas automatiquement un ordinateur pendant une installation planifiée si un utilisateur est connecté à l’ordinateur. Au lieu de cela, la fonctionnalité Mises à jour automatiques notifie l’utilisateur qu’il doit redémarrer l’ordinateur.|
|**Désactivé**|Spécifie que la fonctionnalité Mises à jour automatiques envoient une notification à l’utilisateur pour lui indiquer que l’ordinateur redémarrera automatiquement dans cinq minutes pour terminer l’installation.|

**Options :** Il n’existe aucune option pour ce paramètre.

#### <a name="re-prompt-for-restart-with-scheduled-installations"></a>Redemander un redémarrage avec les installations planifiées
Spécifie le temps pendant lequel la fonctionnalité Mises à jour automatiques attend avant d’annoncer à nouveau un redémarrage planifié.

|Pris en charge sur :|Sauf :|
|---------|-------|
|systèmes d’exploitation Windows qui se trouvent toujours dans leur [cycle de vie du support de produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT|

> [!IMPORTANT]
> Cette stratégie s’applique uniquement lorsque la fonctionnalité Mises à jour automatiques est configurée pour effectuer des installations planifiées de mises à jour. Si le paramètre de stratégie Configurer les mises à jour automatiques est désactivé, cette stratégie est sans effet.

> [!NOTE]
> Cette stratégie est sans effet sur les ordinateurs qui exécutent Windows RT.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Un redémarrage planifié se produit dix minutes après que le message d’invite de redémarrage a été ignoré. Un administrateur local peut modifier ce paramètre à l’aide d’une stratégie locale.|
|**Activé**|Spécifie qu’après le report de l’invite précédente à redémarrer, un redémarrage planifié s’affiche après le nombre de minutes spécifié.|
|**Désactivé**|Un redémarrage planifié se produit dix minutes après que le message d’invite de redémarrage a été ignoré.|

**Options :** Lorsque cette option est activée, vous pouvez utiliser ce paramètre pour spécifier (en minutes) le temps qui doit s’écouler avant que les utilisateurs soient de nouveau avertis d’un redémarrage planifié.

#### <a name="reschedule-automatic-updates-scheduled-installations"></a>Replanifier les installations planifiées des mises à jour automatiques
Spécifie le temps pendant lequel la fonctionnalité Mises à jour automatiques doit attendre entre le démarrage d’un ordinateur et l’exécution d’une installation planifiée qui a été manquée.

Si l’état est défini sur **Non configuré**, une installation planifiée manquée se produit une minute après le prochain démarrage de l’ordinateur.

|Pris en charge sur :|Sauf :|
|---------|-------|
|systèmes d’exploitation Windows qui se trouvent toujours dans leur [cycle de vie du support de produits Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

> [!NOTE]
> Cette stratégie s’applique uniquement lorsque la fonctionnalité Mises à jour automatiques est configurée pour effectuer des installations planifiées de mises à jour. Si le paramètre de stratégie Configurer les mises à jour automatiques est désactivé, cette stratégie est sans effet.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie qu’une installation planifiée manquée se produit une minute après le prochain démarrage de l’ordinateur.|
|**Activé**|Spécifie qu’une installation planifiée qui n’a pas eu lieu précédemment aura lieu le nombre de minutes spécifié après le prochain démarrage de l’ordinateur.|
|**Désactivé**|Spécifie qu’une installation planifiée manquée aura lieu lors de la prochaine installation planifiée.|

**Options :** Lorsque ce paramètre de stratégie est activé, vous pouvez l’utiliser pour spécifier un nombre de minutes suivant le prochain démarrage de l’ordinateur après lequel une installation planifiée manquée se produira.

#### <a name="specify-intranet-microsoft-update-service-location"></a>Spécifier l’emplacement Intranet du service de mise à jour Microsoft
Spécifie un serveur intranet qui héberge les mises à jour provenant de Microsoft Update. Vous pouvez ensuite utiliser WSUS pour mettre à jour automatiquement des ordinateurs sur votre réseau.

|Pris en charge sur :|Sauf :|
|---------|-------|
|systèmes d’exploitation Windows qui se trouvent toujours dans leur [cycle de vie du support de produits Microsoft](https://support.microsoft.com/gp/lifeselect).|Windows RT.|

Ce paramètre permet de spécifier un serveur WSUS sur votre réseau, qui fonctionnera en tant que service de mise à jour interne. Au lieu d’utiliser les mises à jour Microsoft disponibles sur Internet, les clients WSUS recherchent dans ce service les mises à jour applicables.

Pour utiliser ce service, vous devez définir les valeurs de noms de deux serveurs : celui à partir duquel le client détecte et télécharge les mises à jour, et celui vers lequel les stations de travail mises à jour chargent les statistiques. Les valeurs ne doivent pas nécessairement être différentes si les deux services sont configurés sur le même serveur.

> [!NOTE]
> Si le paramètre de stratégie Configurer les mises à jour automatiques est désactivé, cette stratégie est sans effet.

> [!NOTE]
> Cette stratégie n’est pas prise en charge par Windows RT. L’activation de cette stratégie est sans effet sur les ordinateurs exécutant Windows RT.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Si la fonctionnalité Mises à jour automatiques n’est pas désactivée par une stratégie ou une préférence de l’utilisateur, cette stratégie spécifie que les clients se connectent directement au site Windows Update sur Internet.|
|**Activé**|Spécifie que le client se connecte au serveur WSUS spécifié, plutôt qu’à Windows Update, pour rechercher et télécharger des mises à jour. L’activation de ce paramètre évite aux utilisateurs au sein de votre organisation de devoir passer par un pare-feu pour obtenir les mises à jour et vous permet de tester les mises à jour avant de les déployer.|
|**Désactivé**|Si la fonctionnalité Mises à jour automatiques n’est pas désactivée par une stratégie ou une préférence de l’utilisateur, cette stratégie spécifie que les clients se connectent directement au site Windows Update sur Internet.|

**Options :** Lorsque ce paramètre de stratégie est activé, vous devez spécifier le service de mise à jour intranet que les clients WSUS utilisent lors de la détection de mises à jour, ainsi que le serveur Internet de statistiques sur lequel les clients WSUS mis à jour chargent les statistiques. Exemples de valeurs


|                    Option de paramétrage :                    |    Exemple de valeur :    |
|-------------------------------------------------------|----------------------|
| Configurer le service intranet de mise à jour pour la détection des mises à jour |  http://wsus01:8530  |
|          Configurer le serveur intranet de statistiques           | http://IntranetUpd01 |

#### <a name="turn-on-recommended-updates-via-automatic-updates"></a>Activer les mises à jour recommandées par la fonctionnalité Mises à jour automatiques
Spécifie si la fonctionnalité Mises à jour automatiques fournit des mises à jour IMPORTANTES et recommandées à partir de WSUS.

|Pris en charge sur :|Sauf :|
|---------|-------|
|depuis Windows Vista, les systèmes d’exploitation Windows qui se trouvent toujours dans leur [cycle de vie du support de produits Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie que la fonctionnalité Mises à jour automatiques continue de fournir des mises à jour IMPORTANTES si elle est déjà configurée pour ce faire.|
|**Activé**|Spécifie que la fonctionnalité Mises à jour automatiques installe les mises à jour recommandées et IMPORTANTES de WSUS.|
|**Désactivé**|Spécifie que la fonctionnalité Mises à jour automatiques continue de fournir des mises à jour IMPORTANTES si elle est déjà configurée pour ce faire.|

**Options :** Il n’existe aucune option pour ce paramètre.

#### <a name="turn-on-software-notifications"></a>Activer les notifications d’applications
Ce paramètre de stratégie vous permet de contrôler si les utilisateurs voient des messages de notification améliorés détaillés concernant des logiciels proposés du service Microsoft Update. Les messages de notification améliorés communiquent la valeur et promeuvent l’installation et l’utilisation de logiciels facultatifs. Ce paramètre de stratégie est destiné à être utilisé dans des environnements peu managés dans lesquels vous autorisez l’utilisateur final à accéder au service Microsoft Update.

Si vous n’utilisez pas le service Microsoft Update, le paramètre de stratégie Notifications d’application est sans effet.

Si le paramètre de stratégie Configurer les mises à jour automatiques est désactivé ou n’est pas configuré, le paramètre de stratégie Notifications d’application est sans effet.

|Pris en charge sur :|Sauf :|
|---------|-------|
|depuis Windows Server 2008 (Windows Vista) et Windows 7, les systèmes d’exploitation Windows qui se trouvent toujours dans leur [cycle de vie du support de produits Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

> [!NOTE]
> Par défaut, ce paramètre de stratégie est désactivé.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Les utilisateurs sur des ordinateurs exécutant Windows 7 ne reçoivent pas de messages sur des applications facultatives. Les utilisateurs sur des ordinateurs qui exécutent Windows Vista ne disposent pas de messages pour des applications ou mises à jour facultatives. Un administrateur local peut modifier ce paramètre à l’aide du Panneau de configuration ou d’une stratégie locale.|
|**Activé**|Si vous activez ce paramètre de stratégie, un message de notification s’affiche sur l’ordinateur de l’utilisateur lorsque le logiciel proposé est disponible. L’utilisateur peut cliquer sur la notification pour ouvrir Windows Update et obtenir des informations supplémentaires sur le logiciel ou l’installer. L’utilisateur peut également cliquer sur **Fermer ce message** ou **Afficher plus tard** pour différer la notification si nécessaire.<p>Dans Windows 7, ce paramètre de stratégie contrôle uniquement les notifications détaillées pour des applications facultatives. Dans Windows 7, ce paramètre de stratégie contrôle les notifications détaillées pour des applications et mises à jour facultatives.|
|**Désactivé**|Spécifie que les utilisateurs exécutant Windows 7 ne reçoivent pas de messages de notification détaillés pour des applications facultatives, et que les utilisateurs exécutant  Windows Vista ne reçoivent pas de messages de notification détaillés pour des applications ou mises à jour facultatives.|

**Options :** Il n’existe aucune option pour ce paramètre.

### <a name="computer-configuration--maintenance-scheduler-policy-settings"></a>Configuration ordinateur > paramètres de stratégie du Planificateur de maintenance
Dans le paramètre Configurer les mises à jour automatiques, si vous avez sélectionné l’option **4 - Téléchargement automatique et planification des installations**, vous pouvez spécifier les paramètres de planification du Planificateur de maintenance dans la Console de gestion des stratégies de groupe (GPMC) pour des ordinateurs exécutant Windows 8 et Windows RT. Si vous n’avez pas sélectionné l’option 4 dans le paramètre Configurer les mises à jour automatiques, vous n’avez pas besoin de configurer ces paramètres pour les mises à jour automatiques. Les paramètres du Planificateur de maintenance se trouvent dans le chemin d’accès suivant : *PolicyName* > **Configuration ordinateur** > **Stratégies** > **Modèles d’administration** > **Composants Windows** > **Planificateur de maintenance**. L’extension Planificateur de maintenance de la stratégie de groupe contient les paramètres suivants :

-   [Limite de l’activation de la Maintenance automatique](#automatic-maintenance-activation-boundary)

-   [Délai aléatoire de maintenance automatique](#automatic-maintenance-random-delay)

-   [Stratégie de sortie de veille automatique](#automatic-wakeup-policy)

#### <a name="automatic-maintenance-activation-boundary"></a>Limite de l’activation de la maintenance automatique
Cette stratégie vous permet de configurer le paramètre Limite de l’activation de la Maintenance automatique.

La limite de l’activation de la maintenance est l’heure planifiée quotidienne à laquelle la maintenance automatique démarre.

|Pris en charge sur :|Sauf :|
|---------|-------|
|systèmes d’exploitation Windows qui se trouvent toujours dans leur [cycle de vie du support de produits Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

> [!NOTE]
> Ce paramètre est lié à l’option 4 dans **Configurer les mises à jour automatiques**. Si vous n’avez pas sélectionné l’option 4 dans **Configurer les mises à jour automatiques**, il n’est pas nécessaire de configurer ce paramètre.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Si ce paramètre de stratégie n’est pas configuré, l’heure planifiée quotidienne spécifiée sur les ordinateurs clients dans le Panneau de configuration, dans **Centre de maintenance** > **Maintenance automatique**, s’applique.|
|**Activé**|L’activation de ce paramètre de stratégie remplace tous les paramètres par défaut ou modifiés configurés sur les ordinateurs clients dans **Panneau de configuration** > **Centre de maintenance** > **Maintenance automatique** (ou, dans certaines versions de client **Maintenance**).|
|**Désactivé**|Si vous définissez ce paramètre de stratégie sur **Désactivé**, l’heure planifiée quotidienne telle que spécifiée dans le Panneau de configuration, dans **Centre de maintenance** > **Maintenance automatique**, s’applique.|

#### <a name="automatic-maintenance-random-delay"></a>Délai aléatoire de maintenance automatique
Ce paramètre de stratégie vous permet de configurer le délai aléatoire d’activation de la Maintenance automatique.

Le délai aléatoire de maintenance correspond à la durée pendant laquelle la maintenance automatique retardera à partir de sa limite d’activation. Ce paramètre est utile pour les machines virtuelles où une maintenance aléatoire peut être une exigence en matière de performances.

|Pris en charge sur :|Sauf :|
|---------|-------|
|systèmes d’exploitation Windows qui se trouvent toujours dans leur [cycle de vie du support de produits Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

> [!NOTE]
> Ce paramètre est lié à l’option 4 dans **Configurer les mises à jour automatiques**. Si vous n’avez pas sélectionné l’option 4 dans **Configurer les mises à jour automatiques**, il n’est pas nécessaire de configurer ce paramètre.

Par défaut, lorsque cette option est activée, le délai aléatoire de maintenance régulière est défini sur **PT4H**.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Un délai aléatoire de quatre heures est appliqué à **Automatique**.|
|**Activé**|La maintenance automatique retarde le démarrage de la durée spécifiée à partir de sa limite d’activation.|
|**Désactivé**|Aucun délai aléatoire n’est appliqué à la Maintenance automatique.|

#### <a name="automatic-wakeup-policy"></a>Stratégie de sortie de veille automatique
Ce paramètre de stratégie vous permet de configurer la stratégie de sortie de veille pour la Maintenance automatique.

La stratégie de sortie de veille pour la maintenance spécifie si la Maintenance automatique doit envoyer une demande de sortie de veille à l’ordinateur d’exploitation pour une maintenance quotidienne planifiée.

|Pris en charge sur :|Sauf :|
|---------|-------|
|systèmes d’exploitation Windows qui se trouvent toujours dans leur [cycle de vie du support de produits Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

> [!NOTE]
> Si la stratégie de sortie de veille de l’ordinateur d’exploitation est explicitement désactivée, ce paramètre est sans effet.

> [!NOTE]
> Ce paramètre est lié à l’option 4 dans **Configurer les mises à jour automatiques**. Si vous n’avez pas sélectionné l’option 4 dans **Configurer les mises à jour automatiques**, il n’est pas nécessaire de configurer ce paramètre.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Si vous ne configurez pas ce paramètre de stratégie, le paramètre de sortie de veille spécifié dans le Panneau de configuration, dans **Centre de maintenance** > **Maintenance automatique** s’applique.|
|**Activé**|Si vous activez ce paramètre de stratégie, la Maintenance automatique tente de définir une stratégie de sortie de veille du système d’exploitation et envoie une demande de sortie de veille pour l’heure planifiée quotidienne, si nécessaire.|
|**Désactivé**|Si vous désactivez ce paramètre de stratégie, le paramètre de sortie de veille spécifié dans le Panneau de configuration, dans **Centre de maintenance** > **Maintenance automatique** s’applique.|

### <a name="user-configuration--windows-update-policy-settings"></a>Configuration utilisateur > Paramètres de stratégie de Windows Update
Cette section fournit des détails sur les paramètres de stratégie basés sur l’utilisateur suivants :

-   [Ne pas afficher l’option Installer les mises à jour et éteindre dans la boîte de dialogue Arrêt de Windows](#do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog)

-   [Ne pas définir l’option par défaut sur Installer les mises à jour et éteindre dans la boîte de dialogue Arrêt de Windows](#do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog)

-   [Supprimer l’accès à l’utilisation de toutes les fonctionnalités Windows Update](#remove-access-to-use-all-windows-update-features)

Dans la Console de gestion des stratégies de groupe (GPMC), les paramètres utilisateur pour les mises à jour automatiques de l’ordinateur se trouvent dans le chemin d’accès suivant : *PolicyName* > **Configuration utilisateur** > **Stratégies** > **Modèles d’administration** > **Composants Windows** > **Windows Update**. Les paramètres sont répertoriés dans le même ordre que dans les extensions Configuration ordinateur et Configuration utilisateur dans la Stratégie de groupe, lorsque l’onglet **Paramètres** de la stratégie Windows Update est sélectionné pour trier les paramètres par ordre alphabétique.

> [!NOTE]
> Par défaut, sauf indication contraire, ces paramètres ne sont pas configurés.

> [!TIP]
> Pour chacun de ces paramètres, vous pouvez procéder comme suit pour activer et désactiver les paramètres, ou naviguer entre eux :

#### <a name="do-not-display-install-updates-and-shut-down-option-in-shut-down-windows-dialog-box"></a>Ne pas afficher l’option « Installer les mises à jour et éteindre » dans la boîte de dialogue Arrêt de Windows
Spécifie si l’option **Installer les mises à jour et éteindre** s’affiche dans la boîte de dialogue **Arrêt de Windows**.

|Pris en charge sur :|Sauf :|
|---------|-------|
|systèmes d’exploitation Windows qui se trouvent toujours dans leur [cycle de vie du support de produits Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie que l’option **Installer les mises à jour et éteindre** est disponible dans la boîte de dialogue **Arrêt de Windows** si des mises à jour sont disponibles quand l’utilisateur sélectionne l’option Arrêter pour arrêter l’ordinateur.|
|**Activé**|Si vous activez ce paramètre de stratégie, l’option **Installer les mises à jour et éteindre** n’apparaît pas dans la boîte de dialogue **Arrêt de Windows**, même si des mises à jour sont disponibles pour installation lorsque l’utilisateur sélectionne l’option Arrêter pour arrêter l’ordinateur.|
|**Désactivé**|Spécifie que l’option **Installer les mises à jour et éteindre** est disponible dans la boîte de dialogue **Arrêt de Windows** si des mises à jour sont disponibles quand l’utilisateur sélectionne l’option Arrêter pour arrêter l’ordinateur.|

**Options :** Il n’existe aucune option pour ce paramètre.

#### <a name="do-not-adjust-default-option-to-install-updates-and-shut-down-in-shut-down-windows-dialog-box"></a>Ne pas définir l’option par défaut sur Installer les mises à jour et éteindre dans la boîte de dialogue Arrêt de Windows
Spécifie si l’option **Installer les mises à jour et éteindre** s’affiche comme choix par défaut dans la boîte de dialogue **Arrêt de Windows**.

|Pris en charge sur :|Sauf :|
|---------|-------|
|systèmes d’exploitation Windows qui se trouvent toujours dans leur [cycle de vie du support de produits Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

> [!NOTE]
> Ce paramètre de stratégie n’a aucune incidence si le paramètre de stratégie *PolicyName* > **Configuration utilisateur** > **Stratégies** > **Modèles d’administration** > **Composants Windows** > **Windows Update** > **Ne pas afficher l’option Installer les mises à jour et éteindre dans la boîte de dialogue Arrêt de Windows** est activé.

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Spécifie si l’option **Installer les mises à jour et éteindre** est l’option par défaut dans la boîte de dialogue **Arrêt de Windows** si des mises à jour sont disponibles pour installation au moment où l’utilisateur sélectionne l’option Arrêter pour arrêter l’ordinateur.|
|**Activé**|Spécifie si le dernier choix d’arrêt de l’utilisateur (par exemple, Mettre en veille prolongée ou Redémarrer) est l’option par défaut dans la boîte de dialogue **Arrêt de Windows**, indépendamment du fait que l’option **Installer les mises à jour et éteindre** soit ou non disponible dans le menu **Sue voulez-vous faire ?** .|
|**Désactivé**|Spécifie si l’option **Installer les mises à jour et éteindre** est l’option par défaut dans la boîte de dialogue **Arrêt de Windows** si des mises à jour sont disponibles pour installation au moment où l’utilisateur sélectionne l’option Arrêter pour arrêter l’ordinateur.|

**Options :** Il n’existe aucune option pour ce paramètre.

#### <a name="remove-access-to-use-all-windows-update-features"></a>Supprimer l’accès à l’utilisation de toutes les fonctionnalités Windows Update
Ce paramètre vous permet de supprimer l’accès du client WSUS à Windows Update.

|Pris en charge sur :|Sauf :|
|---------|-------|
|systèmes d’exploitation Windows qui se trouvent toujours dans leur [cycle de vie du support de produits Microsoft](https://support.microsoft.com/gp/lifeselect).|null|

|||
|-|-|
|**État du paramètre de stratégie**|**Comportement**|
|**Non configuré**|Les utilisateurs sont en mesure de se connecter au site web Windows Update.|
|**Activé**|**IMPORTANT :** si l’option est activée, toutes les fonctionnalités de Windows Update sont supprimées. Cela inclut le blocage de l’accès au site web Windows Update à l’adresse https://windowsupdate.microsoft.com, à partir du le lien hypertexte Windows Update sur le menu Démarrer ou l’écran Démarrer, ainsi que sur le menu **Outils** dans Internet Explorer. La mise à jour automatique de Windows est également désactivée. L’utilisateur ne reçoit pas mises à jour critiques et n’en est pas informé par Windows Update. Ce paramètre de stratégie empêche également le Gestionnaire de périphériques d’installer automatiquement les mises à jour des pilotes à partir du site web Windows Update.<p>Lorsque cette option est activée, vous pouvez configurer l’une des options de notification suivantes :<p>-   **0 - Ne pas afficher de notifications**<br />    Ce paramètre supprime tous les accès aux fonctionnalités de Windows Update, et aucune notification ne s’affiche.<br />-   **1 - Afficher des notifications de redémarrage requis**<br />    Ce paramètre affiche les notifications concernant les redémarrages nécessaires pour l’exécution d’une installation. **Remarque :** Sur des ordinateurs exécutant Windows 8 et Windows RT, si cette stratégie est activée, seules les notifications liées aux redémarrages et à l’impossibilité de détecter les mises à jour s’affichent. Les options d’authentification ne sont pas prises en charge. Les notifications sur l’écran de connexion s’affichent toujours.|
|**Désactivé**|Les utilisateurs sont en mesure de se connecter au site web Windows Update.|

**Options :** Consultez **Activé** dans le tableau pour ce paramètre.

## <a name="supplemental-information"></a>Informations supplémentaires
Cette section fournit des informations supplémentaires sur l’utilisation de l’ouverture et de l’enregistrement des paramètres WSUS dans les stratégies de groupe, ainsi que des définitions de termes utilisés dans ce guide. Pour les administrateurs connaissant les versions antérieures de WSUS (WSUS 3.2 et versions antérieures), il existe un tableau qui résume brièvement les différences entre les versions de WSUS.

### <a name="accessing-the-windows-update-settings-in-group-policy"></a>Accès aux paramètres de Windows Update dans Stratégie de groupe
La procédure qui suit décrit comment ouvrir la Console de gestion des stratégies de groupe (GPMC) sur votre contrôleur de domaine. La procédure décrit ensuite comment ouvrir un objet de stratégie de groupe (GPO, Group Policy Object) au niveau du domaine pour le modifier, ou créer un tel objet et l’ouvrir pour le modifier.

> [!NOTE]
> Pour effectuer cette procédure, vous devrez ouvrir une session en tant que membre du groupe **Admins du domaine** ou équivalent.

##### <a name="to-open-or-add-and-open-a-group-policy-object"></a>Pour ouvrir ou ajouter et ouvrir un objet de stratégie de groupe

1.  Sur votre contrôleur de domaine, accédez à **Gestionnaire de serveur**, > **Outils**, > **Gestion des stratégies de groupe**. La Console de gestion des stratégies de groupe s’ouvre.

2.  Dans le volet gauche, développez votre forêt. Par exemple, double-cliquez sur **forêt : example.com**.

3.  Dans le volet gauche, double-cliquez sur **Domaines**, puis sur le domaine pour lequel vous souhaitez gérer un objet de stratégie de groupe. Par exemple, double-cliquez sur **example.com**.

4.  Effectuez l'une des opérations suivantes :

    -  **Pour ouvrir un objet de stratégie de groupe existant au niveau du domaine afin de le modifier**, double-cliquez sur le domaine contenant l’objet de stratégie de groupe à gérer, cliquez avec le bouton droit sur la stratégie de domaine à gérer, puis cliquez sur **Modifier**. L’Éditeur de gestion des stratégies de groupe (GPME) s’ouvre.

    -  **Pour créer un objet de stratégie de groupe et l’ouvrir afin de le modifier** :
        1.  Cliquez avec le bouton droit sur le domaine pour lequel vous souhaitez créer un objet stratégie de groupe, puis cliquez sur **Créer un objet GPO dans ce domaine, et le lier ici**.

        2.  Dans **Nouvel objet GPO**, dans **Nom**, tapez le nom du nouvel objet de stratégie de groupe, puis cliquez sur **OK**.

        3.  Cliquez avec le bouton droit sur votre nouvel objet de stratégie de groupe, puis sélectionnez **Modifier**. L’Éditeur de gestion des stratégies de groupe (GPME) s’ouvre.

##### <a name="to-open-the-windows-update-or-maintenance-scheduler-extensions-of-group-policy"></a>Pour ouvrir l’extension Windows Update ou Planificateur de maintenance de la stratégie de groupe

1.  Dans Éditeur de gestion des stratégies de groupe, effectuez l’une des opérations suivantes :

    -   **Ouvrez l’extension Configuration ordinateur > Windows Update de la stratégie de groupe**. Accédez à : *PolicyName* > **Configuration ordinateur** > **Stratégies** / **Modèles d’administration** > **Composants Windows** > **Windows Update**.

    -   **Ouvrez l’extension Configuration utilisateur > Windows Update de la stratégie de groupe**. Accédez à : *PolicyName* > **Configuration utilisateur** > **Stratégies** > **Modèles d’administration** > **Composants Windows** > **Windows Update**.

    -   **Ouvrez l’extension Configuration ordinateur > Planificateur de maintenance de la stratégie de groupe**. Dans l’Éditeur d’objets de stratégie de groupe, accédez à *PolicyName* > **Configuration ordinateur** > **Stratégies** > **Modèles d’administration** > **Composants Windows** > **Planificateur de maintenance**.

Pour plus d’informations sur la stratégie de groupe, voir [Vue d’ensemble de la stratégie de groupe](https://technet.microsoft.com/library/hh831791.aspx(v=ws.12)).

> [!TIP]
> Après avoir ouvert l’extension de stratégie de groupe souhaitée, vous pouvez procéder comme suit pour activer et désactiver les paramètres, ou naviguer entre eux :

##### <a name="to-configure-group-policy-settings"></a>Pour configurer les paramètres de stratégie de groupe

1.  Dans *ExtensionOfGroupPolicy*, double-cliquez sur le paramètre à afficher ou à modifier.

2.  Pour configurer le paramètre, effectuez l’une des opérations suivantes :

    -   Pour conserver l’état non spécifié par défaut du paramètre, sélectionnez **Non configuré**.

    -   Pour activer le paramètre, sélectionnez **Activé**.

    -   Pour désactiver le paramètre, sélectionnez **désactivé**.

3.  Dans **options**, si des options sont répertoriées, conservez les valeurs par défaut ou modifiez-les si nécessaire.

4.  Effectuez l'une des opérations suivantes :

    -   Pour enregistrer vos modifications et passer au paramètre suivant, cliquez sur **Appliquer**, puis cliquez sur **Paramètre suivant**.

    -   Pour enregistrer vos modifications et fermer la boîte de dialogue, cliquez sur **OK**.

    -   Pour ignorer toutes les modifications non enregistrées et fermer la boîte de dialogue, cliquez sur **Annuler**.

### <a name="changes-to-wsus-relevant-to-this-guide"></a>Modifications apportées à WSUS en rapport avec ce guide
Le tableau suivant récapitule les principales différences entre les versions actuelle et précédente de WSUS qui sont pertinentes pour ce guide.

|Versions précédentes de Windows Server et WSUS|Description|
|------------------|--------|
| Windows Server 2012 R2 avec WSUS 6.0 et versions ultérieures|Depuis Windows Server 2012, le rôle serveur WSUS est intégré avec le système d’exploitation, et les paramètres de Stratégie de groupe associés pour les clients WSUS sont, par défaut, inclus dans la Stratégie de groupe.|
| Windows Server 2008 (et versions antérieures de Windows Server) avec WSUS 3.2 et versions antérieures|Dans Windows Server 2008 (et versions antérieures de Windows Server) utilisant WSUS versions 3.2 et antérieures, les paramètres de Stratégie de groupe qui régissent les clients WSUS ne sont pas inclus dans ces systèmes d’exploitation Windows Server. Les paramètres de stratégie se trouvent dans le modèle d’administration WSUS, **wuau.adm**. Dans ces versions de serveur, le modèle d’administration WSUS doit d’abord être ajouté à la Console de gestion des stratégies de groupe (GPMC) avant que les paramètres du client WSUS puissent être configurés.|

### <a name="terms-and-definitions"></a>Termes et définitions
Vous trouverez ci-dessous une liste de termes utilisés dans ce guide.

|Terme|Définition|
|----|-------|
|Mises à jour automatiques|**Service qui s’exécute sur des ordinateurs Windows** (Mises à jour automatiques) : Fait référence au composant ordinateur client intégré aux systèmes d’exploitation Microsoft Windows Vista, Windows Server 2003, Windows XP et Windows 2000 avec SP3 pour l’obtention de mises à jour de Microsoft Update ou de Windows Update.<p>**Référence informelle** (mises à jour automatiques) : Terme utilisé pour décrire le moment où l’agent Windows Update planifie et télécharge automatiquement des mises à jour.|
|serveur autonome|Serveur Windows Server Update Services (WSUS) en aval sur lequel les administrateurs peuvent gérer des composants WSUS.|
|serveur en aval|Serveur Windows Server Update Services (WSUS) qui obtient des mises à jour d’un autre serveur WSUS plutôt que de Microsoft Update ou Windows Update.|
|Extension de stratégie de groupe|Collection de paramètres dans une Stratégie de groupe utilisés pour contrôler la façon dont les utilisateurs et les ordinateurs (auxquels les stratégies s’appliquent) peuvent configurer et utiliser différents services et fonctionnalités Windows. Les administrateurs peuvent utiliser WSUS avec une Stratégie de groupe pour la configuration côté client du client de mises à jour automatiques, afin de s’assurer que les utilisateurs finaux ne puissent pas désactiver ou contourner les stratégies de mise à jour d’entreprise.<p>WSUS ne nécessite pas l’utilisation d’Active Directory ou de Stratégie de groupe. La configuration du client peut également être appliquée à l’aide d’une stratégie de groupe locale ou en modifiant le Registre Windows.|
|service de mise à jour interne|Référence informelle à une infrastructure réseau qui utilise un ou plusieurs serveurs WSUS pour distribuer des mises à jour.|
|serveur réplica|Serveur Windows Server Update Services (WSUS) en aval qui reflète les approbations et paramètres sur le serveur en amont auquel il est connecté. Vous ne pouvez pas gérer WSUS sur un serveur réplica.|
|Microsoft Update|**Site de téléchargement Microsoft basé sur Internet :** Site Internet Microsoft qui stocke et distribue des mises à jour pour des ordinateurs Windows (pilotes de périphérique), des systèmes d’exploitation Windows et d’autres produits logiciels Microsoft.|
|Software Update Services (SUS)|SUS était le produit prédécesseur de Windows Server Update Services (WSUS).|
|mises à jour|Collection quelconque de révisions logicielles, correctifs, packs de service, packs de fonctionnalités et des pilotes de périphériques qui peuvent être installés sur un ordinateur pour en étendre les fonctionnalités ou en améliorer les performances et la sécurité.|
|fichiers de mise à jour|Fichiers requis pour installer une mise à jour sur un ordinateur.|
|informations de mise à jour (également appelées métadonnées de mise à jour)|Informations relatives à une mise à jour, par opposition aux fichiers binaires de mise à jour d’un package de mise à jour. Par exemple, les métadonnées fournissent des informations sur les propriétés d’une mise à jour, qui vous permet de savoir l’utilité de la mise à jour. Les métadonnées incluent également les Termes du contrat de licence logiciel Microsoft. Le package de métadonnées téléchargé pour une mise à jour est généralement plus petit que le package des fichiers de mise à jour.|
|source de mise à jour|Emplacement avec lequel un serveur Windows Server Update Services (WSUS) se synchronise pour obtenir des fichiers de mise à jour. Cet emplacement peut être Microsoft Update ou un serveur WSUS en amont.|
|serveur en amont|Serveur Windows Server Update Services (WSUS) fournissant des fichiers de mise à jour à un autre serveur WSUS appelé serveur en aval.|
|Windows Server Update Services (WSUS)|Programme de rôle serveur qui s’exécute sur un ou plusieurs ordinateurs Windows Server dans un réseau d’entreprise. Une infrastructure WSUS vous permet de gérer les mises à jour à installer sur les ordinateurs de votre réseau.<p>Vous pouvez utiliser WSUS pour approuver ou refuser des mises à jour avant publication, pour forcer l’installation de mises à jour à une date donnée et pour obtenir des rapports détaillés sur les mises à jour donc chaque ordinateur de votre réseau a besoin. Vous pouvez configurer WSUS pour approuver automatiquement certaines classes de mises à jour (mises à jour critiques, mises à jour de sécurité, packs de service, pilotes, etc.). WSUS vous permet également d’approuver des mises à jour pour la détection uniquement, afin de voir les ordinateurs qui nécessiteront une mise à jour donnée sans avoir à l’installer.<p>Dans une implémentation WSUS, au moins un serveur WSUS du réseau doit pouvoir se connecter à Microsoft Update pour obtenir les mises à jour disponibles. En fonction de la configuration et de la sécurité du réseau, l’administrateur peut déterminer le nombre d’autres serveurs se connectant directement à Microsoft Update.<p>Vous pouvez configurer un serveur WSUS pour obtenir des mises à jour sur Internet à partir d’emplacements tels que les suivants :<p>-   Microsoft Update public<br />-   Windows Update public<br />-   Microsoft Store|
|Windows Update|**Site de téléchargement Microsoft basé sur Internet :** Site Microsoft Internet qui stocke et distribue les mises à jour pour ordinateurs Windows (pilotes de périphériques) et systèmes d’exploitation Windows.<p>**service d’ordinateur :** Nom du service Windows Update qui s’exécute sur les ordinateurs. Windows Update détecte, télécharge et installe les mises à jour sur les ordinateurs Windows.<p>En fonction des configurations de l’ordinateur et de la stratégie, l’agent Windows Update peut télécharger des mises à jour à partir des emplacements suivants :<p>-   Microsoft Update<br />-   Windows Update<br />-   Microsoft Store<br />-   Un service (WSUS) de mise à jour (réseau) Internet<p>Les ordinateurs non gérés dans un environnement WSUS utilisent généralement Windows Update pour se connecter directement (via Internet) à Windows Update, Microsoft Update ou Microsoft Store pour obtenir des mises à jour.|
|Client WSUS|Ordinateur qui reçoit des mises à jour d’un service de mise à jour intranet WSUS.<p>Dans le cas de paramètres de stratégie de groupe qui contrôlent l’interaction de l’utilisateur final avec la fonctionnalité Mises à jour automatiques, utilisateur d’un ordinateur dans un environnement WSUS.|
