---
title: Optimisation de Windows 10 version 1909 pour un rôle VDI (Virtual Desktop Infrastructure)
description: Paramètres et configuration recommandés pour réduire la charge pour les postes de travail Windows 10, version 1909 utilisés comme images VDI.
ms.prod: windows-server
ms.reviewer: robsmi
ms.technology: remote-desktop-services
ms.author: helohr
ms.topic: article
author: heidilohr
manager: lizross
ms.date: 02/19/2020
ms.openlocfilehash: 44aa465773674625fa392a644ffb188140138bde
ms.sourcegitcommit: 1c75e4b3f5895f9fa33efffd06822dca301d4835
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77519594"
---
# <a name="optimizing-windows-10-version-1909-for-a-virtual-desktop-infrastructure-vdi-role"></a>Optimisation de Windows 10 version 1909 pour un rôle VDI (Virtual Desktop Infrastructure)

Cet article vous aide à choisir les paramètres pour Windows 10, version 1909 (build 18363) qui doivent normalement permettre des performances optimales dans un environnement VDI. Tous les paramètres de ce guide sont des recommandations à considérer, mais ne sont en aucune façon des exigences.

Les principaux moyens d’optimiser les performances de Windows 10 dans un environnement VDI sont de réduire au minimum les opérations où les graphiques des applications sont redessinés, de limiter les activités en arrière-plan qui n’ont pas un intérêt majeur pour l’environnement VDI et de réduire les processus en cours d’exécution au strict minimum. Un objectif secondaire est de réduire l’utilisation de l’espace disque dans l’image de base au strict minimum. Avec les implémentations de VDI, la taille d’image la plus petite possible, ou « gold », peut légèrement réduire l’utilisation de la mémoire sur l’hyperviseur et permettre une légère diminution des opérations réseau nécessaires pour délivrer l’image du poste de travail au consommateur.

> [!NOTE]
> Ces paramètres recommandés peuvent être appliqués à d’autres installations de Windows 10 1909, notamment celles qui se trouvent sur des machines physiques ou d’autres machines virtuelles. Aucune recommandation de cet article ne doit normalement affecter la prise en charge de Windows 10 1909.

## <a name="vdi-optimization-principles"></a>Principes d’optimisation de l’infrastructure VDI

Un environnement VDI présente une session de poste de travail complète, y compris les applications, à un utilisateur de l’ordinateur sur un réseau. Le vecteur de la distribution réseau peut être un réseau local ou Internet. Les environnements VDI utilisent une image de système d’exploitation « de base », qui devient alors la base pour les postes de travail présentés par la suite aux utilisateurs. Il existe des variations des implémentations de VDI, comme « persistante », « non persistante » et « session de poste de travail ». Le type « persistante » préserve les modifications apportées au système d’exploitation de poste de travail VDI d’une session à l’autre. Le type « non persistante » ne préserve pas les modifications apportées au système d’exploitation de poste de travail VDI d’une session à l’autre. Pour l’utilisateur, ce poste de travail n’est pas très différent d’un autre appareil virtuel ou physique, hormis qu’il est accessible via un réseau.

Les paramètres d’optimisation sont placés sur un appareil de référence. Une machine virtuelle serait un endroit idéal pour créer l’image, car l’état peut être enregistré, des points de contrôle peuvent être créés et des sauvegardes peuvent être effectuées. Une installation par défaut du système d’exploitation est effectuée sur la machine virtuelle de base. Cette machine virtuelle de base est ensuite optimisée en supprimant les applications inutiles, en installant les mises à jour Windows, en installant d’autres mises à jour, en supprimant les fichiers temporaires et en appliquant les paramètres.

Il existe d’autres types d’infrastructure VDI, par exemple la session Bureau à distance (RDS) et le [Bureau virtuel Windows](https://azure.microsoft.com/services/virtual-desktop/) (Windows Virtual Desktop) récemment publié. La présentation détaillée de ces technologies n’est pas abordée dans cet article. Ce dernier se concentre sur les paramètres de l’image de base Windows, sans référence à d’autres facteurs de l’environnement, tels que l’optimisation de l’hôte.

La sécurité et la stabilité sont les principales priorités de Microsoft en ce qui concerne les produits et les services. Les clients d’entreprise peuvent choisir d’utiliser l’application Sécurité Windows intégrée, suite de services qui fonctionnent bien avec ou sans Internet. Pour les environnements VDI non connectés à Internet, les signatures de sécurité peuvent être téléchargées plusieurs fois par jour, car Microsoft peut publier plusieurs mises à jour de signatures par jour. Ces signatures peuvent ensuite être fournies aux machines virtuelles VDI et planifiées pour être installées en production, que la VDI soit persistante ou non persistante. De cette façon, la protection des machines virtuelles est la plus à jour possible.

Certains paramètres de sécurité ne sont pas applicables aux environnements VDI qui ne sont pas connectés à Internet et ne peuvent donc pas participer à la sécurité cloud. D’autres paramètres peuvent être utilisés par les appareils Windows « normaux », tels que l’expérience du cloud ou le Windows Store. La suppression de l’accès aux fonctionnalités inutilisées réduit l’encombrement, la bande passante réseau et la surface d’attaque.

En ce qui concerne les mises à jour, Windows 10 utilise un algorithme de mise à jour mensuelle. Il n’est donc pas nécessaire que les clients effectuent eux-mêmes les mises à jour. Dans la plupart des cas, les administrateurs VDI contrôlent le processus de mise à jour par le biais d’un processus d’arrêt des machines virtuelles basées sur une image « principale » ou « gold », descellent cette image en lecture seule, la corrigent, puis la re scellent et la remettent en production. Ainsi, il n’est pas nécessaire que les machines virtuelles VDI vérifient Windows Update. Dans certains cas, par exemple, les machines virtuelles VDI persistantes, des procédures de mise à jour corrective normales ont lieu. Windows Update ou Microsoft Intune peut également être utilisé. System Center Configuration Manager peut être utilisé pour gérer les mises à jour et autres distributions de packages. Il appartient à chaque organisation de déterminer la meilleure approche pour mettre à jour l’infrastructure VDI.

> [!TIP]  
> Un script qui implémente les optimisations présentées dans cette rubrique, ainsi qu’un fichier d’exportation d’objet de stratégie de groupe que vous pouvez importer avec **LGPO.exe**, est disponible dans [TheVDIGuys](https://github.com/TheVDIGuys) sur GitHub.

Ce script a été conçu pour répondre à votre environnement et à vos exigences. Le code principal est en PowerShell, et le travail est effectué à l’aide de fichiers d’entrée (texte brut), avec les fichiers d’exportation de l’Outil d’objet de stratégie de groupe local (LGPO). Ces fichiers contiennent des listes d’applications à supprimer et des services à désactiver. Si vous ne souhaitez pas supprimer une application particulière ou désactiver un service particulier, modifiez le fichier texte correspondant et supprimez l’élément. Enfin, il existe des paramètres de stratégie locale qui peuvent être importés dans votre appareil. Il est préférable d’avoir des paramètres dans l’image de base, plutôt que d’appliquer les paramètres à l’aide de la stratégie de groupe, car certains paramètres sont effectifs au redémarrage suivant ou quand un composant est utilisé pour la première fois.

### <a name="persistent-vdi"></a>VDI persistante

La VDI persistante est à la base une machine virtuelle qui enregistre les états du système d’exploitation entre les redémarrages. D’autres couches logicielles de la solution VDI fournissent aux utilisateurs un accès simple et direct aux machines virtuelles qui leur sont affectées, souvent avec une solution d’authentification unique.

Il existe plusieurs implémentations différentes de la VDI persistante :

- Machine virtuelle traditionnelle, où la machine virtuelle a son propre fichier de disque virtuel, démarre normalement, enregistre les modifications d’une session à l’autre. La différence est la façon dont l’utilisateur accède à cette machine virtuelle. Il peut y avoir un portail web auquel l’utilisateur se connecte et qui le dirige automatiquement vers la ou les machines virtuelles VDI qui lui sont affectées.

- Machine virtuelle persistante basée sur une image, éventuellement avec des disques virtuels personnels. Dans ce type d’implémentation, il y a une image de base/gold sur un ou plusieurs serveurs hôtes. Une machine virtuelle est créée, et un ou plusieurs disques virtuels sont créés et affectés à ce disque pour le stockage persistant.

    - Quand la machine virtuelle est démarrée, une copie de l’image de base est lue dans la mémoire de cette machine virtuelle. Au même moment, un disque virtuel persistant est affecté à cette machine virtuelle, avec les modifications précédentes du système d’exploitation fusionnées via un processus complexe.

    - Les modifications comme les écritures du journal des événements, les écritures du journal, etc., sont redirigées vers le disque virtuel en lecture/écriture affecté à cette machine virtuelle.

    - Dans ce cas, la maintenance du système d’exploitation et des applications peut fonctionner normalement, en utilisant des logiciels de maintenance traditionnels comme Windows Server Update Services ou d’autres technologies de gestion.

    - La différence entre une machine VDI persistante et une machine virtuelle « normale » est la relation avec l’image principale/gold. À un moment donné, les mises à jour doivent être appliquées à l’image principale. C’est là que les implémentations décident comment les modifications persistantes de l’utilisateur sont gérées. Dans certains cas, le disque avec les modifications est ignoré et/ou réinitialisé, ce qui se traduit par la définition d’un nouveau point de contrôle. Il se peut aussi que les modifications apportées par l’utilisateur soient conservées par le biais de mises à jour de qualité mensuelles et que la base soit réinitialisée après une mise à jour des fonctionnalités.

### <a name="non-persistent-vdi"></a>VDI non persistante

Quand une implémentation de la VDI non persistante est basée sur une image de base ou « gold », les optimisations sont principalement effectuées dans l’image de base, puis via des paramètres locaux et des stratégies locales.

Avec la VDI non persistante basés sur une image, l’image de base est en lecture seule. Quand une machine virtuelle non persistante est démarrée, une copie de l’image de base est diffusée en continu à la machine virtuelle. L’activité qui se produit lors du démarrage et par la suite jusqu’au redémarrage suivant est redirigée vers un emplacement temporaire. En général, des emplacements réseau sont fournis aux utilisateurs pour stocker leurs données. Dans certains cas, le profil de l’utilisateur est fusionné avec la machine virtuelle standard pour fournir ses paramètres à cet utilisateur.

Un aspect important de la VDI non persistante basée sur une image est la maintenance. Les mises à jour du système d’exploitation et des composants sont généralement délivrées une fois par mois. Avec la VDI basée sur une image, un ensemble de processus doivent être effectués pour obtenir les mises à jour de l’image :

- Sur un hôte donné, toutes les machines virtuelles présentes sur cet hôte qui sont dérivées de l’image de base doivent être arrêtées/désactivées. Cela signifie que les utilisateurs sont redirigés vers d’autres machines virtuelles.

- L’image de base est ensuite ouverte et démarrée. Toutes les activités de maintenance sont ensuite effectuées, comme les mises à jour du système d’exploitation, les mises à jour de .NET, les mises à jour des applications, etc.

- Les nouveaux paramètres qui doivent être appliqués le sont à ce moment.

- Toutes les autres opérations de maintenance sont effectuées à ce moment-là.

- L’image de base est ensuite arrêtée.

- L’image de base est scellée et définie comme pouvant être remise en production.

- Les utilisateurs peuvent se reconnecter.

> [!NOTE]
> Windows 10 effectue automatiquement un ensemble de tâches de maintenance de façon périodique. Par défaut, une tâche planifiée est définie pour s’exécuter tous les jours à 3h00. Cette tâche planifiée exécute une liste de tâches, notamment le nettoyage de Windows Update. Vous pouvez voir toutes les catégories de maintenance qui se produisent automatiquement avec cette commande PowerShell :
>
>```powershell
>Get-ScheduledTask | ? {$_.Settings.MaintenanceSettings}
>```
>

Un des défis liés à la VDI non persistante est que quand un utilisateur se déconnecte, presque toutes les activités du système d’exploitation sont abandonnées. Le profil de l’utilisateur et/ou l’état peuvent être enregistrés dans un emplacement centralisé, mais la machine virtuelle elle-même abandonne presque toutes les modifications qui ont été apportées depuis le dernier démarrage. Par conséquent, les optimisations destinées à un ordinateur Windows qui enregistre l’état d’une session à l’autre ne sont pas applicables.

Selon l’architecture de la machine virtuelle VDI, des choses comme la prérécupération et SuperFetch ne vont pas aider d’une session à l’autre, car toutes les optimisations sont abandonnées au redémarrage de la machine virtuelle. L’indexation peut être une perte partielle de ressources, tout comme les optimisations des disques, par exemple une défragmentation traditionnelle.

> [!NOTE]
> Si vous préparez une image à l’aide de la virtualisation et que vous êtes connecté à Internet lors du processus de création de l’image, à la première ouverture de session, vous devez différer les mises à jour des fonctionnalités en accédant à **Paramètres** **Windows Update**.

### <a name="to-sysprep-or-not-sysprep"></a>Utiliser Sysprep ou pas

Windows 10 a une fonctionnalité intégrée appelée [Outil de préparation du système](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview) (System Preparation Tool, souvent abrégé en « Sysprep »). L’outil Sysprep est utilisé pour préparer une image Windows 10 personnalisée pour la duplication. Le processus de Sysprep garantit que le système d’exploitation qui en résulte est correctement configuré pour une exécution en production.

Il existe des raisons pour et contre l’exécution de Sysprep. Dans le cas de VDI, vous pouvez avoir besoin de personnaliser le profil utilisateur par défaut qui doit être utilisé comme modèle de profil pour les utilisateurs suivants qui se connectent en utilisant cette image. Vous pouvez avoir des applications à installer, mais aussi vouloir contrôler les paramètres pour chaque application.

L’alternative consiste à utiliser une image .ISO standard à partir de laquelle faire les installations, éventuellement en utilisant un fichier de réponses d’installation sans assistance, et une séquence de tâches pour installer ou supprimer des applications. Vous pouvez aussi utiliser une séquence de tâches pour définir des paramètres de stratégie locale dans l’image, par exemple en utilisant l’[outil Utilitaire Objet Stratégie de groupe locale (LGPO, Local Group Policy Object)](https://docs.microsoft.com/archive/blogs/secguide/lgpo-exe-local-group-policy-object-utility-v1-0).

### <a name="supportability"></a>Prise en charge

Chaque fois que des valeurs par défaut de Windows sont changées, des questions se posent sur la prise en charge. Une fois qu’une image VDI (machine virtuelle ou session) est personnalisée, toutes les modifications apportées à l’image doivent être suivies dans un journal des modifications. Lors de la résolution des problèmes, une image peut souvent être isolée dans un pool et configurée en vue d’une analyse des problèmes. Une fois qu’un problème a fait l’objet d’un suivi jusqu’à la cause racine, cette modification peut être déployée sur l’environnement de test en premier, et en dernier lieu sur la charge de travail de production.

Ce document évite intentionnellement d’aborder les services système, les stratégies ou les tâches qui affectent la sécurité. « Après cela, vient la maintenance Windows. La possibilité de traiter les images VDI en dehors des fenêtres de maintenance est supprimée, car celles-ci ont lieu quand la plupart des événements de maintenance se produisent dans les environnements VDI, *à l’exception des mises à jour logicielles de sécurité*. Microsoft a publié des conseils pour la sécurité Windows dans les environnements VDI. Pour plus d’informations, consultez [Guide de déploiement de l’antivirus Windows Defender dans un environnement d’infrastructure de bureau virtuel (VDI)](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/deployment-vdi-windows-defender-antivirus).

Tenez compte de la capacité de prise en charge lors de la modification des paramètres Windows par défaut. Des problèmes difficiles peuvent survenir lors de la modification de services système, de stratégies ou de tâches planifiées, au nom d’un durcissement ou d’une « simplification ». Consultez la base de connaissances Microsoft pour connaître les problèmes connus actuels concernant les paramètres par défaut modifiés. Les instructions de ce document et le script associé sur GitHub font l’objet d’une maintenance par rapport aux problèmes connus susceptibles de se produire. En outre, vous pouvez signaler des problèmes de plusieurs façons à Microsoft.

Vous pouvez utiliser votre moteur de recherche préféré avec les termes « "valeur de démarrage" site:support.microsoft.com » pour afficher les problèmes connus concernant les valeurs de démarrage par défaut pour les services.

Vous noterez peut-être que ce document et les scripts associés sur GitHub ne modifient aucune autorisation par défaut. Si vous souhaitez améliorer vos paramètres de sécurité, commencez par le projet appelé **AaronLocker**. Pour plus d’informations, consultez [ANNONCE : mise en liste verte d’applications avec « AaronLocker »](https://docs.microsoft.com/archive/blogs/aaron_margosis/announcing-application-whitelisting-with-aaronlocker).

#### <a name="vdi-optimization-categories"></a>Catégories d’optimisation de VDI

- Catégories de paramètres globaux du système d’exploitation :

    - Nettoyage d’applications UWP

    - Nettoyage des fonctionnalités facultatives

    - Paramètres de stratégie locale

    - Services système

    - Tâches planifiées

    - Appliquer les mises à jour de Windows (et d’autres mises à jour)

    - Suivis automatiques de Windows

    - Nettoyage de disque avant la finalisation (scellement) de l’image

    - Paramètres utilisateur

    - Paramètres de l’hyperviseur/hôte

### <a name="universal-windows-platform-uwp-application-cleanup"></a>Nettoyage d’applications de plateforme Windows universelle (UWP)

Un des objectifs d’une image VDI est d’être aussi légère que possible. Une des façons de réduire la taille de l’image consiste à supprimer les applications UWP qui ne sont pas utilisées dans l’environnement. Avec les applications UWP, il y a les fichiers d’application principaux, connus sous le nom de « charge utile ». Une petite quantité de données sont stockées dans le profil de chaque utilisateur pour les paramètres spécifiques aux applications. Il y a aussi une petite quantité de données dans le profil « Tous les utilisateurs ».

La connectivité et le minutage sont des facteurs importants dès lors qu’il est question du nettoyage des applications UWP. Si vous déployez votre image de base sur un appareil sans connectivité réseau, Windows 10 ne peut pas se connecter à Microsoft Store, ni télécharger des applications et essayer de les installer alors que vous essayez de les désinstaller. Ce peut être une bonne stratégie que de prendre le temps de personnaliser votre image, puis de mettre à jour le reste à un stade ultérieur du processus de création de l’image.

Si vous modifiez le fichier .WIM de base que vous utilisez pour installer Windows 10 et que vous supprimez les applications UWP non nécessaires du fichier .WIM avant l’installation, les applications ne sont pas installées dès le départ et le temps de création de votre profil est plus court. Plus loin dans cette section, vous trouverez plus d’informations sur la suppression des applications UWP du fichier .WIM de votre installation.

Une bonne stratégie pour la VDI consiste à provisionner les applications souhaitées dans l’image de base, puis à limiter ou à bloquer par la suite l’accès au Microsoft Store. Les applications du Store sont mises à jour périodiquement en arrière-plan sur les ordinateurs normaux. Les applications UWP peuvent être mises à jour pendant la fenêtre de maintenance quand d’autres mises à jour sont appliquées. Pour plus d’informations, consultez [Applications de plateforme Windows universelle (UWP)](https://docs.citrix.com/citrix-virtual-apps-desktops/manage-deployment/applications-manage/universal-apps.html)

#### <a name="delete-the-payload-of-uwp-apps"></a>Supprimer la charge utile des applications UWP

Les applications UWP qui ne sont pas nécessaires sont toujours présentes dans le système de fichiers, consommant une petite quantité d’espace disque. Pour les applications qui ne seront jamais nécessaires, la charge utile des applications UWP non souhaitées peut être supprimée de l’image de base en utilisant des commandes PowerShell.

En fait, si vous supprimez ces applications du fichier .WIM d’installation en utilisant les liens fournis plus loin dans cette section, vous devez être en mesure de commencer dès le début avec une liste très courte d’applications UWP.

Exécutez la commande suivante pour énumérer les applications UWP provisionnées à partir d’un système d’exploitation en cours d’exécution, comme dans cet exemple de sortie tronquée de PowerShell :

```powershell

    Get-AppxProvisionedPackage -Online

    DisplayName  : Microsoft.3DBuilder
    Version      : 13.0.10349.0  
    Architecture : neutral
    ResourceId   : \~ 
    PackageName  : Microsoft.3DBuilder_13.0.10349.0_neutral_\~_8wekyb3d8bbwe
    Regions      :
    ...
```

Les applications UWP qui sont provisionnées sur un système peuvent être supprimées lors de l’installation du système d’exploitation dans le cadre d’une séquence de tâches, ou plus tard une fois que le système d’exploitation est installé. Ceci peut être la méthode préférée, car elle rend modulaire le processus global de création ou de gestion d’une image. Une fois que vous développez les scripts, si quelque chose change dans une build ultérieure, vous modifiez un script existant au lieu de répéter le processus à partir de zéro. Voici quelques liens vers des informations à ce sujet :

[Suppression d’applications fournies avec Windows 10 pendant une séquence de tâches](https://blogs.technet.microsoft.com/mniehaus/2015/11/11/removing-windows-10-in-box-apps-during-a-task-sequence/)

[Suppression d’applications intégrées du fichier WIM de Windows 10 avec PowerShell - Version 1.3](https://gallery.technet.microsoft.com/Removing-Built-in-apps-65dc387b)

[Windows 10 1607 : Empêcher le retour d’applications lors du déploiement de la mise à jour d’une fonctionnalité](https://blogs.technet.microsoft.com/mniehaus/2016/08/23/windows-10-1607-keeping-apps-from-coming-back-when-deploying-the-feature-update/)

Exécutez ensuite la commande PowerShell [Remove-AppxProvisionedPackage](https://docs.microsoft.com/powershell/module/dism/remove-appxprovisionedpackage?view=win10-ps) pour supprimer les charges utiles des applications UWP :

```powershell
Remove-AppxProvisionedPackage -Online -PackageName
```

L’applicabilité doit être évaluée pour chaque application UWP dans chaque environnement. Vous voulez installer une installation par défaut de Windows 10 1909, puis noter les applications qui sont en cours d’exécution et consomment de la mémoire. Par exemple, vous pouvez envisager de supprimer des applications qui démarrent automatiquement, ou des applications qui affichent automatiquement des informations sur le menu Démarrer, comme Météo et Actualités, et qui ne sont pas utiles dans votre environnement.

>[!NOTE]
>Si vous utilisez les scripts disponibles sur GitHub, vous pouvez facilement contrôler les applications à supprimer avant l’exécution des scripts. Après avoir téléchargé les fichiers de script, recherchez le fichier « Win10_1909_AppxPackages.txt », modifiez-le, puis supprimez les entrées des applications que vous souhaitez conserver, telles que Calculatrice ou Pense-bêtes.

### <a name="manage-windows-optional-features-using-powershell"></a>Gérer les fonctionnalités facultatives de Windows à l’aide de PowerShell

Vous pouvez gérer les fonctionnalités facultatives de Windows à l’aide de PowerShell. Pour en savoir plus, consultez [Windows 10 : Gestion des fonctionnalités facultatives avec PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/39386.windows-10-managing-optional-features-with-powershell.aspx). Pour énumérer les fonctionnalités Windows installées, exécutez la commande PowerShell suivante :

```powershell
Get-WindowsOptionalFeature -Online
```

Vous pouvez activer ou désactiver une fonctionnalité facultative spécifique de Windows, comme l’illustre cet exemple :

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName "DirectPlay" -All
```

Vous pouvez désactiver des fonctionnalités dans l’image VDI, comme l’illustre cet exemple :

```powershell
Disable-WindowsOptionalFeature -Online -FeatureName “WindowsMediaPlayer”
```

Ensuite, vous souhaiterez peut-être supprimer le package du lecteur Windows Media. Il existe deux packages de lecteur Windows Media dans Windows 10 1909 :

```powershell
Get-WindowsPackage -Online -PackageName *media*

PackageName       : Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.1
Applicable        : True
Copyright        : Copyright (c) Microsoft Corporation. All Rights Reserved
Company         :
CreationTime       :
Description       : Play audio and video files on your local device and on the Internet.
InstallClient      : DISM Package Manager Provider
InstallPackageName    : Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.1.mum
InstallTime       : 3/19/2019 6:20:22 AM
...

Features         : {}

PackageName       : Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.449
Applicable        : True
Copyright        : Copyright (c) Microsoft Corporation. All Rights Reserved
Company         :
CreationTime       :
Description       : Play audio and video files on your local device and on the Internet.
InstallClient      : UpdateAgentLCU
InstallPackageName    : Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.449.mum
InstallTime       : 10/29/2019 5:15:17 AM
...
```

Si vous souhaitez supprimer le package du lecteur Windows Media (pour libérer environ 60 Mo d’espace disque) :

```powershell
 Remove-WindowsPackage -PackageName Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.1 -Online

 Remove-WindowsPackage -PackageName Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.1 -Online
```

#### <a name="enable-or-disable-windows-features-using-dism"></a>Activer ou désactiver des fonctionnalités Windows avec DISM

Vous pouvez utiliser l’outil intégré Dism.exe pour énumérer et contrôler les fonctionnalités facultatives de Windows. Un script DISM.exe peut être développé et exécuté pendant une séquence de tâches d’installation du système d’exploitation. La technologie Windows impliquée est appelée [Fonctionnalités à la demande](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities).

#### <a name="default-user-settings"></a>Paramètres utilisateur par défaut

Des personnalisations peuvent être apportées à un fichier de Registre Windows appelé « C:\Users\Default\NTUSER.DAT ». Tous les paramètres définis dans ce fichier sont appliqués à tous les profils utilisateur suivants créés à partir d’un appareil exécutant cette image. Vous pouvez contrôler les paramètres à appliquer au profil utilisateur par défaut, en modifiant le fichier « Win10_1909_DefaultUserSettings.txt ». L’un des paramètres qui peut mériter une attention particulière, et qui constitue une nouveauté parmi toutes les recommandations de paramètres, est un paramètre appelé **TaskbarSmallIcons**. Vous souhaiterez peut-être consulter votre base d’utilisateurs avant d’implémenter ce paramètre. **TaskbarSmallIcons** rend la barre des tâches Windows plus petite, consomme moins d’espace à l’écran, rend les icônes plus compactes et réduit l’interface de recherche. Les illustrations suivantes montrent l’interface, selon que ce paramètre est utilisé ou non :

Figure 1 : Barre des tâches normale dans Windows 10, version 1909

![Version standard de la barre des tâches Windows 10, version 1909](media/rds-vdi-recommendations-1909/standard-taskbar.png)

Figure 2 : Barre des tâches utilisant le paramètre de petites icônes

![Barre des tâches utilisant le paramètre de petites icônes](media/rds-vdi-recommendations-1909/taskbar-sm-icons.png)

En outre, pour réduire la transmission d’images sur l’infrastructure VDI, vous pouvez définir l’arrière-plan par défaut sur une couleur unie à la place de l’image Windows 10 par défaut. Vous pouvez également définir l’écran d’ouverture de session sur une couleur unie et désactiver l’effet de flou opaque à l’ouverture de session.

Les paramètres suivants sont appliqués à la ruche de Registre de profils utilisateur par défaut, principalement afin de réduire les animations. Si certains ou l’ensemble de ces paramètres ne sont pas souhaités, supprimez les paramètres à ne pas appliquer aux nouveaux profils utilisateur basés sur cette image. L’objectif de ces paramètres est d’activer les paramètres équivalents suivants :

Figure 3 : Propriétés système optimisées, options de performances

![Propriétés système optimisées, options de performances](media/rds-vdi-recommendations-1909/performance-options.png)

Voici les paramètres d’optimisation appliqués à la ruche de Registre de profils utilisateur par défaut pour optimiser les performances :

```
Delete HKLM\Temp\SOFTWARE\Microsoft\Windows\CurrentVersion\Run /v OneDriveSetup /f
add "HKLM\Temp\Control Panel\Desktop" /v DragFullWindows /t REG_SZ /d 0 /f
add "HKLM\Temp\Control Panel\Desktop" /v WallPaper /t REG_SZ /d "" /f
add "HKLM\Temp\Control Panel\Desktop\WindowMetrics" /v MinAnimate /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\DWM /v AccentColor /t REG_DWORD /d 4292311040 /f
add HKLM\Temp\Software\Microsoft\Windows\DWM /v ColorizationColor /t REG_DWORD /d 4292311040 /f
add HKLM\Temp\Software\Microsoft\Windows\DWM /v AlwaysHibernateThumbnails /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\DWM /v EnableAeroPeek /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\DWM /v AlwaysHibernateThumbnails /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v AutoCheckSelect /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v HideIcons /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v ListviewAlphaSelect /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v ListViewShadow /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v ShowInfoTip /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v TaskbarAnimations /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v TaskbarSmallIcons /t REG_DWORD /d 1 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced\People /v PeopleBand /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\AnimateMinMax /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\ComboBoxAnimation /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\ControlAnimations /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\DWMAeroPeekEnabled /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\DWMSaveThumbnailEnabled /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\MenuAnimation /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\SelectionFade /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\TaskbarAnimations /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\TooltipAnimation /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager /v SubscribedContent-338388Enabled /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager /v SubscribedContent-338389Enabled /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager /v SystemPaneSuggestionsEnabled /t REG_DWORD /d 0 /f
```

Dans les paramètres de stratégie locale, vous souhaiterez peut-être désactiver les images pour les arrière-plans dans la VDI.  Si vous souhaitez vraiment des images, vous pouvez créer des images d’arrière-plan personnalisées avec une profondeur de couleur réduite afin de limiter la bande passante réseau utilisée pour transmettre les informations d’image. Si vous décidez de ne pas spécifier d’image d’arrière-plan dans la stratégie locale, vous pouvez définir la couleur d’arrière-plan avant de définir la stratégie locale, car une fois celle-ci définie, l’utilisateur n’a aucun moyen de changer la couleur d’arrière-plan. Il peut être préférable de spécifier « (null) » comme image d’arrière-plan. La section suivante présente un autre paramètre de stratégie sur la non-utilisation de l’arrière-plan sur les sessions de protocole RDP (Remote Desktop Protocol).

### <a name="local-policy-settings"></a>Paramètres de stratégie locale

Beaucoup d’optimisations pour Windows 10 dans un environnement VDI peuvent être effectuées avec la stratégie Windows. Les paramètres figurant dans le tableau de cette section peuvent être appliqués localement à l’image de base/gold. Si les paramètres équivalents ne sont pas spécifiés d’une autre façon, telle que la stratégie de groupe, ils s’appliquent néanmoins.

Certaines décisions peuvent être basées sur les spécificités de l’environnement, par exemple :

- L’environnement VDI est-il autorisé à accéder à Internet ?

- La solution VDI est-elle persistante ou non persistante ?

Les paramètres suivants ont été choisis pour ne pas bloquer ou ne pas être en confit avec des paramètres n’ayant rien à voir avec la sécurité. Ces paramètres ont été choisis pour supprimer les paramètres ou désactiver les fonctionnalités qui peuvent ne pas être applicables aux environnements VDI.

| Paramètre de stratégie | Élément | Sous-élément | Valeur possible et commentaires|
| -------------- | ---- | -------- | ---------------------------- |
| Stratégie de l’ordinateur local \\ Configuration ordinateur \\ Paramètres Windows \\ Paramètres de sécurité | | | |
| Stratégies du gestionnaire de listes de réseaux | Toutes les propriétés des réseaux | Emplacement réseau | L’utilisateur ne peut pas changer l’emplacement |
| Stratégie de l’ordinateur local \\ Configuration ordinateur \\ Modèles d’administration \\ Panneau de configuration | | | |
| *Panneau de configuration | Autoriser les conseils en ligne | | Désactivé. Les paramètres ne contactent pas les services de contenu Microsoft pour récupérer des conseils et du contenu d’aide. |
| *Panneau de configuration\Personnalisation | Ne pas afficher l’écran de verrouillage | Activé. Ce paramètre détermine si l’écran de verrouillage apparaît aux utilisateurs. Si vous activez ce paramètre de stratégie, les utilisateurs qui ne doivent pas appuyer sur Ctrl+Alt+Suppr avant de se connecter voient la vignette qu’ils ont sélectionnée après le verrouillage de leur PC. |
| *Panneau de configuration\Personnalisation | Forcer un écran de verrouillage par défaut spécifique et une image d’ouverture de session | [![Interface utilisateur pour définir le chemin de l’écran de verrouillage](media/lock-screen-image-settings.png)](media/lock-screen-image-settings.png) | Activé. Ce paramètre vous permet de spécifier l’écran de verrouillage par défaut et l’image d’ouverture de session montrée quand aucun utilisateur n’est connecté, et il définit aussi l’image spécifiée comme image par défaut pour tous les utilisateurs : elle remplace l’image par défaut.<p>Nous vous recommandons d’utiliser une image non complexe de basse résolution afin que moins de données soient transmises sur le réseau chaque fois que l’image est affichée. |
| *Panneau de configuration\Options régionales et linguistiques\Personnalisation de l’écriture manuscrite | Désactiver l’apprentissage automatique | | Activé. Si vous activez ce paramètre de stratégie, l’apprentissage automatique s’arrête et les données stockées sont supprimées. Les utilisateurs ne peuvent pas configurer ce paramètre dans le Panneau de configuration. |
| Stratégie de l’ordinateur local \\ Configuration ordinateur \\ Modèles d’administration \\ Réseau | | | |
| Service de transfert intelligent en arrière-plan (BITS) | Ne pas autoriser le client BITS à utiliser le cache de filiale Windows |  | Activé |
| Service de transfert intelligent en arrière-plan (BITS) | Ne pas autoriser l’utilisation de l’ordinateur en tant que client de mise en cache partagé entre systèmes homologues BITS |  | Activé |
| Service de transfert intelligent en arrière-plan (BITS) | Ne pas autoriser l’utilisation de l’ordinateur en tant que serveur de mise en cache partagé entre systèmes homologues BITS |  | Activé |
| Service de transfert intelligent en arrière-plan (BITS) | Autoriser la mise en cache partagé entre systèmes homologues BITS |  | Désactivé |
| BranchCache | Activer BranchCache |  | Désactivé |
| *Polices | Activer les fournisseurs de polices |  | Désactivé. Windows ne se connecte pas à un fournisseur de polices en ligne et énumère seulement les polices installées localement. |
| Authentification de la zone d’accès sans fil | Activer l’authentification de la zone d’accès sans fil |  | Désactivé |
| Activer les services réseau pair à pair Microsoft | Désactiver les services réseau pair à pair Microsoft |  | Activé |
| Indicateur d’état de connectivité réseau | Spécifier l’interrogation passive. | Désactiver l’interrogation passive (case à cocher) | Activé. Utilisez ce paramètre si vous êtes sur un réseau isolé ou si vous utilisez une adresse IP statique. |
| Fichiers hors connexion | Autoriser ou interdire l’utilisation de fichiers hors connexion. |  | Désactivé |
| Paramètres TCPIP \\ Technologies de transition IPv6 | Définir l’état de Teredo | État désactivé | Activé. Dans l’état désactivé, aucune interface Teredo n’est présente sur l’hôte. |
| Service de réseau local sans fil \\ Paramètres de réseau local sans fil | Autoriser Windows à se connecter automatiquement à des points d’accès ouverts suggérés, à des réseaux partagés par des contacts et à des points d’accès offrant les services payants. |  | Désactivé. Les paramètres **Se connecter aux points d’accès ouverts suggérés**, **Se connecter aux réseaux partagés par mes contacts** et **Activer les services payants** sont désactivés, mais les utilisateurs sur cet appareil peuvent les activer. |
| Stratégie de l’ordinateur local \\ Configuration ordinateur \\ Modèles d’administration \\ Menu Démarrer et barre des tâches |  |  |  |
| *Notifications | Désactiver l’utilisation du réseau pour les notifications |  | Activé. Si vous activez ce paramètre, les applications et les fonctionnalités système ne peuvent pas recevoir des notifications du réseau depuis WNS ou avec les API de notification d’interrogation. |
| Stratégie de l’ordinateur local \\ Configuration ordinateur \\ Modèles d’administration \\ Système |  |  |  |
| Installation des périphériques | Ne pas envoyer de rapport d’erreurs Windows lors de l’installation d’un pilote générique sur un périphérique |  | Activé |
| Installation des périphériques | Empêcher la création de point de restauration système lors d’une activité d’un périphérique demandant habituellement la création d’un point de restauration. |  | Activé |
| Installation des périphériques | Empêcher la récupération des métadonnées de périphérique depuis Internet |  | Activé |
| Installation des périphériques | Empêcher Windows d’envoyer un rapport d’erreurs lorsqu’un pilote de périphérique demande un logiciel supplémentaire au cours de l’installation |  | Activé |
| Installation des périphériques | Désactiver les **bulles Nouveau matériel détecté** pendant l’installation de périphériques. |  | Activé |
| Système de fichiers \\ NTFS | Options de création de nom court | Désactivé sur tous les volumes | Activé |
| *Stratégie de groupe | Configurer les liaisons web-application avec les gestionnaires d’URI d’applications |  | Désactivé. Désactive les liaisons web-application désactive ; les URI HTTP(S) sont ouverts dans le navigateur par défaut au lieu de démarrer l’application associée. |
| *Stratégie de groupe | Poursuivre les expériences sur cet appareil. |  | Désactivé. L’appareil Windows n’est pas découvrable par d’autres appareils et ne peut pas participer à des expériences entre des appareils. |
| Gestion de la communication Internet \\ Paramètres de communication Internet | Désactiver l’accès à toutes les fonctionnalités Windows Update |  |Activé. Si vous activez ce paramètre de stratégie, toutes les fonctionnalités de Windows Update sont supprimées. Ceci inclut le blocage de l’accès au site Web Windows Update à l’adresse https://windowsupdate.microsoft.com, depuis le lien hypertexte Windows Update sur le menu Démarrer ainsi que sur le menu Outils dans Internet Explorer. La mise à jour automatique de Windows est également désactivée ; vous n’êtes pas notifié des mises à jour critiques par Windows Update et vous ne les recevez pas. Ce paramètre de stratégie empêche également le Gestionnaire de périphériques d’installer automatiquement les mises à jour des pilotes à partir du site web Windows Update. |
| Gestion de la communication Internet \\ Paramètres de communication Internet | Désactiver la mise à jour automatique des certificats racines |  |Activé. Si vous activez ce paramètre de stratégie, quand un certificat émis par une autorité racine non approuvée vous est présenté, votre ordinateur ne contacte pas le site web Windows Update pour voir si Microsoft a ajouté l’autorité de certification à sa liste des autorités approuvées. REMARQUE : Utilisez cette stratégie seulement si vous avez un moyen alternatif à la dernière liste de révocation des certificats. |
| Gestion de la communication Internet \\ Paramètres de communication Internet | Désactiver les liens « Events.asp » de l’observateur d’événements |  | Activé |
| Gestion de la communication Internet \\ Paramètres de communication Internet | Désactiver le partage des données de personnalisation de l'écriture manuscrite |  | Activé |
| Gestion de la communication Internet \\ Paramètres de communication Internet | Désactiver le signalement d’erreurs de la reconnaissance de l’écriture manuscrite |  | Activé |
| Gestion de la communication Internet \\ Paramètres de communication Internet | Désactiver le contenu « Le saviez-vous ? » du Centre d’aide et de support contenu |  | Activé |
| Gestion de la communication Internet \\ Paramètres de communication Internet | Désactiver la recherche dans la Base de connaissances Microsoft du Centre d’aide et de support |  | Activé |
| Gestion de la communication Internet \\ Paramètres de communication Internet | Désactiver l’Assistant Connexion Internet si l’adresse URL de connexion fait référence à microsoft.com |  | Activé |
| Gestion de la communication Internet \\ Paramètres de communication Internet | Désactiver le téléchargement à partir d’Internet pour les Assistants Publication de sites Web et Commande en ligne via Internet |  | Activé |
| Gestion de la communication Internet \\ Paramètres de communication Internet | Désactiver le service d’association de fichier Internet |  | Activé |
| Gestion de la communication Internet \\ Paramètres de communication Internet | Désactiver l’inscription si l’adresse URL de connexion fait référence à microsoft.com |  | Activé |
| Gestion de la communication Internet \\ Paramètres de communication Internet | Désactiver l’option Commander des photos de la Gestion des images |  | Activé |
| Gestion de la communication Internet \\ Paramètres de communication Internet | Désactiver l’option Publier sur le Web de la Gestion des fichiers |  | Activé |
| Gestion de la communication Internet \\ Paramètres de communication Internet | Désactiver le Programme d’amélioration des services pour Windows Messenger |  | Activé |
| Gestion de la communication Internet \\ Paramètres de communication Internet | Désactiver le Programme d’amélioration de l’expérience utilisateur Windows |  | Activé |
| Gestion de la communication Internet \\ Paramètres de communication Internet | Désactiver les tests actifs de l’Indicateur Windows de statut de connectivité réseau |  | Activé. Ce paramètre de stratégie désactive les tests actifs effectués par l’Indicateur Windows de statut de connectivité réseau (NCSI, Windows Network Connectivity Status Indicator) pour déterminer si votre ordinateur est connecté à Internet ou à un réseau plus limité. Dans le cadre de la détermination du niveau de connectivité, NCSI effectue un des deux tests actifs suivants : téléchargement d’une page à partir d’un serveur web dédié ou envoi d’une requête DNS pour une adresse dédiée. Si vous activez ce paramètre de stratégie, NCSI n’effectue aucun des deux tests actifs. Ceci peut réduire la capacité de NCSI et d’autres composants qui utilisent NCSI à déterminer l’accès à Internet) REMARQUE : Il existe d’autres stratégies qui vous permettent de rediriger les tests NCSI vers des ressources internes, si cette fonctionnalité est souhaitée. |
| Gestion de la communication Internet \\ Paramètres de communication Internet | Désactiver Rapport d’erreurs Windows |  | Activé |
| Gestion de la communication Internet \\ Paramètres de communication Internet | Désactiver la recherche de pilotes de périphériques sur Windows Update |  | Activé |
| Ouverture de session | Afficher la première animation de connexion |  | Désactivé |
| Ouverture de session | Désactiver les notifications d’application sur l’écran de verrouillage |  | Activé |
| Ouverture de session | Désactiver le son de démarrage de Windows |  | Activé |
| Gestion de l'alimentation | Sélectionner un mode de gestion d’alimentation actif | Hautes performances | Activé |
| Récupération | Autoriser la restauration de l’état par défaut du système |  | Désactivé |
| *Intégrité du stockage | Autoriser le téléchargement des mises à jour pour le modèle de prévision de défaillance de disque |  | Désactivé. Les mises à jour ne sont pas téléchargées pour le modèle de prévision de défaillance de disque. |
| *Services de temps Windows \\ Fournisseurs de temps | Activer le client NTP Windows |  | Désactivé. Si vous désactivez ou que vous ne configurez pas ce paramètre de stratégie, l’horloge de l’ordinateur local ne synchronise pas l’heure avec les serveurs NTP. REMARQUE : Manipulez ce paramètre avec précaution. Les appareils Windows joints à un domaine doivent utiliser **NT5DS**. Le contrôleur de domaine vers le contrôleur de domaine parent doit utiliser NTP. Le rôle PDCe doit utiliser NTP. Les machines virtuelles utilisent parfois des « améliorations » ou des « services d’intégration ». |
| Dépannage et diagnostics \\ Maintenance planifiée | Configurer le comportement de la maintenance planifiée |   | Désactivé |
| Dépannage et diagnostics \\ Diagnostics des performances de démarrage Windows | Configurer le niveau d’exécution des scénarios |   | Désactivé |
| Dépannage et diagnostics \\ Diagnostic de fuite de mémoire Windows | Configurer le niveau d’exécution des scénarios |   | Désactivé |
| Dépannage et diagnostics \\ Programme de détection et de résolution de carence des ressources | Configurer le niveau d’exécution des scénarios |   | Désactivé |
| Dépannage et diagnostics \\ Diagnostics des performances de l’arrêt de Windows | Configurer le niveau d’exécution des scénarios |   | Désactivé |
| Dépannage et diagnostics \\ Diagnostics des performances de la veille/reprise Windows | Configurer le niveau d’exécution des scénarios |   | Désactivé |
| Dépannage et diagnostics \\ Diagnostics des performances de réactivité du système Windows | Configurer le niveau d’exécution des scénarios |   | Désactivé |
| *Profils utilisateur | Désactiver l’identifiant de publicité |   | Activé. Si vous activez ce paramètre de stratégie, l’ID de publicité est désactivé. Les applications ne peuvent pas utiliser l’ID pour des expériences entre applications. |
| Stratégie de l’ordinateur local \\ Configuration ordinateur \\ Modèles d’administration \\ Composants Windows |  |  |  |
| Processus d’ajout de fonctionnalités à Windows 10 | Empêcher l’exécution de l’assistant |  | Activé |
| *Confidentialité de l’application | Empêcher l’exécution de l’assistant |  | Activé |
| *Confidentialité de l’application | Permettre aux applications Windows d’accéder aux informations de compte | Valeur par défaut pour toutes les applications : Forcer le refus | Activé. Si vous choisissez l’option **Forcer le refus**, les applications Windows ne sont pas autorisées à accéder aux informations de compte et les employés de votre organisation ne peuvent pas la changer. |
| *Confidentialité de l’application | Permettre aux applications Windows d’accéder à l’historique des appels | Valeur par défaut pour toutes les applications : Forcer le refus | Activé. Si vous choisissez l’option **Forcer le refus**, les applications Windows ne sont pas autorisées à accéder aux informations de compte et les employés de votre organisation ne peuvent pas la changer. |
| *Confidentialité de l’application | Permettre aux applications Windows d’accéder aux contacts | Valeur par défaut pour toutes les applications : Forcer le refus | Activé. Si vous choisissez l’option **Forcer le refus**, les applications Windows ne sont pas autorisées à accéder aux contacts et les employés de votre organisation ne peuvent pas la changer. |
| *Confidentialité de l’application | Permettre aux applications Windows d’accéder aux informations de diagnostic d’autres applications | Valeur par défaut pour toutes les applications : Forcer le refus | Activé. Si vous désactivez ou que vous ne configurez pas ce paramètre de stratégie, les employés de votre organisation peuvent décider si les applications Windows peuvent obtenir des informations de diagnostic sur les autres applications en utilisant Paramètres > Confidentialité sur l’appareil. |
| *Confidentialité de l’application | Permettre aux applications Windows d’accéder aux e-mails | Valeur par défaut pour toutes les applications : Forcer le refus | Activé. Si vous choisissez l’option **Forcer l’autorisation**, les applications Windows sont autorisées à accéder aux e-mails et les employés de votre organisation ne peuvent pas la changer. |
| *Confidentialité de l’application | Permettre aux applications Windows d’accéder à l’emplacement | Valeur par défaut pour toutes les applications : Forcer le refus | Activé. Si vous choisissez l’option **Forcer le refus**, les applications Windows ne sont pas autorisées à accéder à l’emplacement et les employés de votre organisation ne peuvent pas la changer. |
| *Confidentialité de l’application | Permettre aux applications Windows d’accéder aux messages | Valeur par défaut pour toutes les applications : Forcer le refus | Activé. Si vous choisissez l’option **Forcer le refus**, les applications Windows ne sont pas autorisées à accéder aux messages et les employés de votre organisation ne peuvent pas la changer. |
| *Confidentialité de l’application | Permettre aux applications Windows d’accéder aux données de mouvement | Valeur par défaut pour toutes les applications : Forcer le refus | Activé. Si vous choisissez l’option **Forcer le refus**, les applications Windows ne sont pas autorisées à accéder aux données de mouvement et les employés de votre organisation ne peuvent pas la changer. |
| *Confidentialité de l’application | Permettre aux applications Windows d’accéder aux notifications | Valeur par défaut pour toutes les applications : Forcer le refus | Activé. Si vous choisissez l’option **Forcer le refus**, les applications Windows ne sont pas autorisées à accéder aux notifications et les employés de votre organisation ne peuvent pas la changer. |
| *Confidentialité de l’application | Permettre aux applications Windows d’accéder aux tâches | Valeur par défaut pour toutes les applications : Forcer le refus | Activé. Si vous choisissez l’option **Forcer le refus**, les applications Windows ne sont pas autorisées à accéder aux tâches et les employés de votre organisation ne peuvent pas la changer. |
| *Confidentialité de l’application | Permettre aux applications Windows d’accéder au calendrier | Valeur par défaut pour toutes les applications : Forcer le refus | Activé. Si vous choisissez l’option **Forcer le refus**, les applications Windows ne sont pas autorisées à accéder au calendrier et les employés de votre organisation ne peuvent pas la changer. |
| *Confidentialité de l’application | Permettre aux applications Windows d’accéder à la caméra | Valeur par défaut pour toutes les applications : Forcer le refus | Activé. Si vous choisissez l’option **Forcer le refus**, les applications Windows ne sont pas autorisées à accéder à la caméra et les employés de votre organisation ne peuvent pas la changer. |
| *Confidentialité de l’application | Permettre aux applications Windows d’accéder au microphone | Valeur par défaut pour toutes les applications : Forcer le refus | Activé. Si vous choisissez l’option **Forcer le refus**, les applications Windows ne sont pas autorisées à accéder au microphone et les employés de votre organisation ne peuvent pas la changer. |
| *Confidentialité de l’application | Permettre aux applications Windows d’accéder aux appareils de confiance | Valeur par défaut pour toutes les applications : Forcer le refus | Activé. Si vous choisissez l’option **Forcer le refus**, les applications Windows ne sont pas autorisées à accéder aux appareils de confiance et les employés de votre organisation ne peuvent pas la changer. |
| *Confidentialité de l’application | Laisser les applications Windows communiquer avec des appareils non appairés | Valeur par défaut pour toutes les applications : Forcer le refus | Activé. Si vous choisissez l’option **Forcer le refus**, les applications Windows ne sont pas autorisées à communiquer avec des appareils sans fil non appairés et les employés de votre organisation ne peuvent pas la changer. |
| *Confidentialité de l’application | Permettre aux applications Windows d’accéder à des radios | Valeur par défaut pour toutes les applications : Forcer le refus | Activé. Si vous choisissez l’option **Forcer le refus**, les applications Windows n’ont pas d’accès permettant de contrôler des radios et les employés de votre organisation ne peuvent pas la changer. |
| *Confidentialité de l’application | Permettre aux applications Windows d’effectuer des appels téléphoniques | Valeur par défaut pour toutes les applications : Forcer le refus | Activé. Si vous choisissez l’option **Forcer le refus**, les applications Windows ne sont pas autorisées à effectuer des appels téléphoniques et les employés de votre organisation ne peuvent pas la changer. |
| *Confidentialité de l’application | Permettre aux applications Windows de s’exécuter en arrière-plan | Valeur par défaut pour toutes les applications : Forcer le refus | Activé. Si vous choisissez l’option **Forcer le refus**, les applications Windows ne sont pas autorisées à s’exécuter en arrière-plan et les employés de votre organisation ne peuvent pas la changer. |
| Stratégies d’exécution automatique | Définir le comportement par défaut du programme Autorun | N’exécuter aucune commande Autorun | Activé |
| *Stratégies d’exécution automatique | Désactiver l’exécution automatique |   | Activé. Si vous activez ce paramètre de stratégie, l’exécution automatique est désactivée sur les lecteurs de CD-ROM et de médias amovibles, ou elle est désactivée sur tous les lecteurs. |
| *Contenu cloud | Ne pas afficher les Conseils Windows | Activé. Ce paramètre de stratégie empêche les conseils Windows d’être montrés aux utilisateurs. |
| *Contenu cloud | Désactiver les expériences consommateur Microsoft | Activé. Si vous activez ce paramètre de stratégie, les utilisateurs ne voient plus les recommandations personnalisées provenant de Microsoft ni les notifications concernant leur compte Microsoft. |
| *Collecte des données et versions d’évaluation Preview | Autoriser la télémétrie | 0 - Sécurité [Entreprise uniquement] | Activé. Quand la valeur est définie sur 0, l’option s’applique seulement aux appareils exécutant les éditions Entreprise, Éducation, IoT ou Windows Server. |
| *Collecte des données et versions d’évaluation Preview | Ne pas afficher les notifications de commentaires |  | Activé |
| *Collecte des données et versions d’évaluation Preview | Activer/désactiver le contrôle de l’utilisateur sur les builds d’Insider  |  | Désactivé |
| Optimisation de la distribution | Mode de téléchargement | Mode de téléchargement : Simple (99) | 99 = Mode de téléchargement simple, sans peering. L’optimisation de la distribution télécharge seulement avec HTTP et n’essaie pas de contacter les services cloud d’optimisation de la distribution. |
| Gestionnaire de fenêtres du Bureau |  Ne pas autoriser l’invocation de Rotation 3D |  | Activé |
| Gestionnaire de fenêtres du Bureau |  Ne pas autoriser les animations de fenêtres |  | Activé |
| Gestionnaire de fenêtres du Bureau |  Utiliser une couleur unie pour l’arrière-plan du menu Démarrer |  | Activé |
| Interface utilisateur latérale |  Autoriser le balayage latéral |  | Désactivé |
| Interface utilisateur latérale |  Désactiver les astuces |  | Activé |
| Interface utilisateur latérale | Désactiver le suivi d’utilisation des applications |  | Activé |
| *Explorateur de fichiers |  Configurer Windows Defender SmartScreen |  | Désactivé. SmartScreen est désactivé pour tous les utilisateurs. Les utilisateurs ne sont pas avertis s’ils essaient d’exécuter des applications suspectes à partir d’Internet. REMARQUE : S’ils ne sont pas connectés à Internet, ceci empêche les ordinateurs d’essayer de contacter Microsoft pour obtenir des informations SmartScreen. |
| Explorateur de fichiers |  Ne pas afficher la notification **nouvelle application installée** |  | Activé |
| *Localiser mon appareil |  Activer/désactiver l’option Localiser mon appareil |  | Désactivé. Quand l’option Localiser mon appareil est désactivée, l’appareil et sa position ne sont pas enregistrés et la fonctionnalité Localiser mon appareil ne fonctionne pas. L’utilisateur ne peut pas non plus voir la position de la dernière utilisation de son digitaliseur actif sur son appareil. |
| Explorateur de fichiers | Désactiver la mise en cache des miniatures |  | Activé |
| Explorateur de fichiers | Désactiver l’affichage des entrées de recherche récentes de la zone de recherche de l’Explorateur de fichiers |  | Activé |
| Explorateur de fichiers | Désactiver la mise en cache des miniatures dans les fichiers masqués thumbs.db |  | Activé |
| Explorateur de jeux | Désactiver le téléchargement d’informations sur les jeux |  | Activé |
| Explorateur de jeux | Désactiver les mises à jour de jeux |  | Activé |
| Explorateur de jeux | Désactiver le suivi de l’heure de la dernière utilisation d’un jeu du dossier Jeux |  | Activé |
| Groupement résidentiel | Empêcher l'ordinateur de rejoindre un groupe résidentiel |  | Activé |
| *Internet Explorer | Autoriser les services Microsoft à fournir des suggestions améliorées à mesure que l’utilisateur entre du texte dans la barre d’adresses |  | Désactivé. Les utilisateurs ne reçoivent pas de suggestions améliorées lors de la saisie dans la barre d’adresses. En outre, les utilisateurs ne peuvent pas changer le paramètre Suggestions. |
| Internet Explorer | Désactiver la vérification périodique des mises à jour de logiciels Internet Explorer |  | Activé |
| Internet Explorer | Désactiver l’affichage de l’écran de démarrage |  | Activé |
| Internet Explorer | Installer automatiquement de nouvelles versions d’Internet Explorer |  | Désactivé |
| Internet Explorer | Empêcher la participation au Programme d’amélioration de l’expérience utilisateur |  | Activé |
| Internet Explorer | Empêcher l’exécution de l’Assistant Première exécution | Aller directement à la page d’accueil | Activé |
| Internet Explorer | Définir le développement de processus d’onglet | Faible | Activé |
| Internet Explorer | Spécifier le comportement par défaut d’un nouvel onglet | Page du nouvel onglet | Activé |
| Internet Explorer | Désactiver les notifications de performances des modules complémentaires |  | Activé |
| *Internet Explorer | Désactiver la fonctionnalité de saisie semi-automatique des adresses web |  | Activé. Si vous activez ce paramètre de stratégie, l’utilisateur ne voit pas de correspondances suggérées lors de la saisie d’adresses web. L’utilisateur ne peut pas changer la saisie semi-automatique pour définir des adresses web. |
| *Internet Explorer | Désactiver la géolocalisation du navigateur |  | Activé. Si vous activez ce paramètre de stratégie, la prise en charge de la géolocalisation du navigateur est désactivée. |
| *Internet Explorer | Désactiver Rouvrir la dernière session de navigation |  | Activé |
| Internet Explorer | Désactiver Rouvrir la dernière session de navigation |  | Activé |
| *Internet Explorer | Activer Sites suggérés |  | Désactivé. Si vous désactivez ce paramètre de stratégie, les points d’entrée et les fonctionnalités associées à cette fonctionnalité sont désactivés. |
| *Internet Explorer \\ Affichage de compatibilité | Désactiver l’affichage de compatibilité |  | Activé. Si vous activez ce paramètre de stratégie, l’utilisateur ne peut pas utiliser le bouton Affichage de compatibilité ou gérer la liste de sites Affichage de compatibilité. |
| *Internet Explorer \\ Panneau de configuration Internet \\ Onglet Avancé | Lire les animations dans les pages Web |  | Désactivé |
| *Internet Explorer \\ Panneau de configuration Internet \\ Onglet Avancé | Lire les vidéos dans les pages Web |  | Désactivé |
| *Internet Explorer \\ Panneau de configuration Internet \\ Onglet Avancé | Désactiver l’avance rapide avec la fonctionnalité de prédiction de page |  | Activé. Microsoft récupère votre historique de navigation pour améliorer la fonction d’avance rapide avec prédiction de page. Cette fonctionnalité n’est pas disponible dans le cas d’Internet Explorer pour le poste de travail. Si vous activez ce paramètre de stratégie, l’avance rapide avec prédiction de page est désactivée et la page web suivante n’est pas chargée en arrière-plan. |
| Internet Explorer \\ Paramètres Internet \\ Paramètres avancés \\ Navigation | Désactiver la détection des numéros de téléphone |  | Activé |
| *Emplacement et capteurs | Désactiver l’emplacement |  | Activé. Si vous activez ce paramètre de stratégie, la fonctionnalité d’emplacement est désactivée et tous les programmes sur cet ordinateur sont empêchés d’utiliser les informations d’emplacement provenant de la fonctionnalité d’emplacement. |
| Emplacement et capteurs | Désactiver les capteurs |  | Activé |
| Emplacement et capteurs \\ Service de localisation Windows | Désactiver le service de localisation Windows |  | Activé |
| *Cartes | Désactiver le téléchargement automatique et la mise à jour des données de carte |  | Activé. Si vous activez ce paramètre, le téléchargement et la mise à jour automatiques et des données des cartes sont désactivés. |
| *Cartes | Désactiver le trafic réseau non sollicité dans la page de paramètres des cartes hors connexion |  | Activé. Si vous activez ce paramètre de stratégie, les fonctionnalités qui génèrent du trafic réseau sur la page des paramètres de Cartes hors connexion sont désactivées. Remarque : Ceci peut désactiver toute la page des paramètres. |
| *Messagerie | Autoriser la synchronisation cloud du service de messagerie |  | Désactivé. Ce paramètre de stratégie permet la sauvegarde et la restauration des SMS sur les services cloud de Microsoft. |
| *Microsoft Edge | Autoriser les suggestions de liste déroulante de la barre d’adresse |  | Désactivé |
| *Microsoft Edge | Autoriser les mises à jour de la bibliothèque Livres |  | Désactivé. Désactive les listes de compatibilité dans Microsoft Edge. |
| *Microsoft Edge | Autoriser la liste de compatibilité Microsoft |  | Désactivé. Si vous désactivez ce paramètre, la liste de compatibilité Microsoft n’est pas utilisée dans le navigateur pendant la navigation. |
| *Microsoft Edge | Autoriser le contenu web dans la page Nouvel onglet |  | Désactivé. Indique à Edge d’ouvrir avec un contenu vide quand un nouvel onglet est ouvert. |
| *Microsoft Edge | Configurer le remplissage automatique |  | Désactivé. Désactive le remplissage automatique sur la barre d’adresses. |
| *Microsoft Edge | Configurer Ne pas me suivre |  | Activé. Si vous activez ce paramètre, les demandes Ne pas me suivre sont toujours envoyées aux sites web qui requièrent des informations de suivi. |
| *Microsoft Edge | Configurer un gestionnaire de mots de passe |  | Désactivé. Si vous désactivez ce paramètre, les employés ne peuvent pas utiliser le gestionnaire de mots de passe pour enregistrer leurs mots de passe en local. |
| *Microsoft Edge | Configurer les suggestions de recherche dans la barre d’adresse |  | Désactivé. Les utilisateurs ne peuvent pas voir les suggestions de recherche dans la barre d’adresse de Microsoft Edge. |
| *Microsoft Edge | Configurer des pages de démarrage |  | Activé. Si vous activez ce paramètre, vous pouvez configurer une ou plusieurs pages de démarrage. Si ce paramètre est activé, vous devez également inclure les URL vers les pages correspondantes en utilisant des crochets angulaires au format suivant <support.contoso.com><support.microsoft.com> Windows 10, version 1703 ou ultérieure : Si vous ne souhaitez pas envoyer le trafic à Microsoft, vous pouvez utiliser la valeur <about:blank>, qui est prise en compte pour les appareils joints ou non à un domaine, quand il s’agit de la seule URL configurée. |
| *Microsoft Edge | Configurer Windows Defender SmartScreen |  | Désactivé. Windows Defender SmartScreen est désactivé et les employés ne peuvent pas l’activer. REMARQUE : Prenez en compte ce paramètre dans l’environnement. S’ils ne sont pas connectés à Internet, ceci empêche les ordinateurs d’essayer de contacter Microsoft pour obtenir des informations SmartScreen. |
| *Microsoft Edge | Empêcher l’ouverture de la page web de première utilisation dans Microsoft Edge |  | Activé. Les utilisateurs ne peuvent pas voir la page de première exécution lors de l’ouverture de Microsoft Edge pour la première fois. |
| OneDrive | Empêcher OneDrive de générer du trafic réseau tant que l’utilisateur ne s’est pas connecté à OneDrive |  | Activé. Activez ce paramètre pour empêcher le client de synchronisation OneDrive (OneDrive.exe) de générer du trafic réseau (recherche de mises à jour, etc.) jusqu’à ce que l’utilisateur se connecte à OneDrive ou démarre la synchronisation des fichiers sur l’ordinateur local. |
| *OneDrive | Empêcher l’utilisation de OneDrive pour le stockage de fichiers |  | Activé. À moins que OneDrive soit utilisé en local ou hors site. |
| OneDrive | Enregistrer les documents sur OneDrive par défaut |  | Désactivé. À moins que OneDrive soit utilisé en local ou hors site. |
| Flux RSS | Empêcher la découverte automatique des flux et des composants Web Slice |  | Activé |
|*Flux RSS | Désactiver la synchronisation en arrière-plan pour les flux et les composants Web Slice |  | Activé. Si vous activez ce paramètre de stratégie, la possibilité de synchroniser les flux et les composants Web Slice en arrière-plan est désactivée. |
|*Recherche | Autoriser Cortana |  | Désactivé. Quand Cortana est désactivé, les utilisateurs peuvent néanmoins utiliser la recherche pour trouver des informations sur l’appareil. |
|Rechercher | Autoriser Cortana au-dessus de l’écran de verrouillage |   | Désactivé |
|*Recherche | Autoriser la recherche et autoriser Cortana à utiliser la localisation |  | Désactivé |
|Rechercher | Ne pas autoriser la recherche web |   | Activé |
|*Recherche | Ne pas rechercher sur le web ou afficher des résultats web dans la recherche |  | Activé. Si vous activez ce paramètre de stratégie, les requêtes ne sont pas effectuées sur le web et les résultats web ne s’affichent pas quand un utilisateur effectue une requête dans Recherche. |
|Rechercher | Empêcher l’ajout d’emplacements UNC à l’index depuis le Panneau de configuration |  | Activé |
|Rechercher | Empêcher l’indexation des fichiers dans le cache des fichiers hors connexion |  | Activé |
|*Recherche | Définir quelles informations sont partagées dans les informations des recherches anonymes |  | Activé. Partager des informations sur l’utilisation, mais ne pas partager l’historique des recherches, les informations de compte Microsoft ou un emplacement spécifique. |
|*Plateforme de protection de licence logicielle | Désactiver la validation AVC en ligne du client KMS | Activé. L’activation de ce paramètre empêche cet ordinateur d’envoyer des données à Microsoft sur son état d’activation. |
|*Voix | Autoriser la mise à jour automatique des données vocales |  | Désactivé. Ne recherche pas périodiquement les modèles vocaux mis à jour. |
|*Store | Désactiver le téléchargement automatique et l’installation des mises à jour |  | Activé. Si vous activez ce paramètre, le téléchargement et l’installation automatiques des mises à jour des applications sont désactivés. |
|*Store | Désactiver le téléchargement automatique des mises à jour sur les ordinateurs Windows 8 | Activé. Si vous activez ce paramètre, le téléchargement automatique des mises à jour des applications est désactivé. |
| Magasin | Désactiver la proposition d’effectuer une mise à jour vers la dernière version de Windows |  | Activé |
|*Synchroniser vos paramètres | Ne pas synchroniser | Autoriser les utilisateurs à activer la synchronisation (non sélectionné) | Activé. Si vous activez ce paramètre de stratégie, « Synchroniser vos paramètres » est désactivé, et aucun des groupes « Synchroniser vos paramètres » n’est synchronisé sur cet appareil. |
| Saisie de texte | Améliorer la reconnaissance de l’entrée manuscrite et de la saisie clavier |  | Désactivé |
| Antivirus Windows Defender \\ MAPS | Rejoindre Microsoft MAPS |  | Désactivé. Si vous désactivez ou que vous ne configurez pas ce paramètre, vous ne rejoignez pas Microsoft MAPS. |
| Antivirus Windows Defender \\ MAPS | Envoyer des exemples de fichiers pour lesquels une analyse supplémentaire est nécessaire | Ne jamais envoyer | Activé. Seulement si l’option de données de diagnostic MAPS n’a pas été choisie. |
| Antivirus Windows Defender \\ Rapports | Désactiver les notifications améliorées |  | Activé. Si vous activez ce paramètre, les notifications améliorées de l’antivirus Windows Defender ne sont pas affichées sur les clients. |
| Antivirus Windows Defender \\ Mises à jour des signatures | Définir l’ordre des sources pour le téléchargement des mises à jour des définitions | FileShares | Activé. Si vous activez ce paramètre, les sources de mise à jour des définitions sont contactées dans l’ordre spécifié. Une fois que les mises à jour des définitions ont été téléchargées avec succès à partir d’une source spécifiée, les sources restantes de la liste ne sont pas contactées. |
| Rapport d’erreurs Windows | Envoyer automatiquement des images mémoire pour les rapports d’erreurs générés par le système d’exploitation |  | Désactivé |
| Rapport d’erreurs Windows | Désactiver le rapport d’erreurs Windows |  | Activé |
| Enregistrement et diffusion de jeux Windows | Active ou désactive Enregistrement et diffusion de jeux Windows | | Désactivé |
| Windows Installer | Contrôler la taille maximale du cache de fichiers de base | 5 | Activé |
| Windows Installer | Désactiver la création de points de vérification pour la Restauration du système |  | Activé |
| Windows Mail | Désactiver la fonctionnalité Communautés |  | Activé |
| Lecteur Windows Media | Ne pas afficher les boîtes de dialogue de configuration à la première exécution du Lecteur |  | Activé |
| Lecteur Windows Media | Empêcher le partage de médias |  | Activé |
| Centre de mobilité Windows | Désactiver le Centre de mobilité Windows |  | Activé |
| Analyse de fiabilité Windows | Configurer les fournisseurs WMI du service de fiabilité |  | Désactivé |
| Windows Update | Autoriser l’installation immédiate des mises à jour automatiques |  | Activé |
| Windows Update | Ne pas se connecter à des emplacements Internet Windows Update |  | Activé. L’activation de cette stratégie désactive cette fonctionnalité et peut faire que la connexion à des services publics comme le Windows Store cesse de fonctionner. REMARQUE : Cette stratégie s’applique uniquement quand cet appareil est configuré pour se connecter à un service de mise à jour intranet à l’aide de la stratégie « Spécifier l’emplacement intranet du service de mise à jour Microsoft ». |
| Windows Update | Supprimer l’accès à toutes les fonctionnalités Windows Update |   | Activé |
| *Windows Update \\ Windows Update pour Entreprise | Gérer les versions d’évaluation | Définir le comportement pour la réception des versions d’évaluation : | Activé. La sélection de Désactiver les versions d’évaluation empêche l’installation des versions d’évaluation sur l’appareil. Ceci empêche les utilisateurs d’opter pour le programme Windows Insider via Paramètres -> Mise à jour et sécurité.<br>Désactivé. Désactive les versions d’évaluation. |
| *Windows Update \\ Windows Update pour Entreprise | Choisir le moment où les versions d’évaluation et les mises à jour des fonctionnalités sont reçues | Canal semi-annuel<br>Ajournement : 365 jours<br>Suspendre le démarrage : jj-mm-aaa. | Activé. Activez cette stratégie pour spécifier le niveau des versions d’évaluation ou des mises à jour des fonctionnalités à recevoir, et à quel moment. |
| Windows Update \\ Windows Update pour Entreprise | Choisir quand recevoir les mises à jour qualité | 1. 30 jours<br>2. Démarrage de la suspension des mises à jour qualité jj-mm-aaaa | Activé |
| Paramètres de stratégie personnalisée du trafic restreint de Windows | Empêcher OneDrive de générer du trafic réseau tant que l’utilisateur ne s’est pas connecté à OneDrive |  | Activé. Activez ce paramètre si vous voulez empêcher le client de synchronisation OneDrive (OneDrive.exe) de générer du trafic réseau (recherche de mises à jour, etc.) jusqu’à ce que l’utilisateur se connecte à OneDrive ou démarre la synchronisation des fichiers sur l’ordinateur local. |
| Paramètres de stratégie personnalisée du trafic restreint de Windows | Désactiver les notifications Windows Defender |  | Activé. Si vous activez ce paramètre de stratégie, Windows Defender n’envoie pas de notifications contenant des informations critiques sur l’intégrité et la sécurité de votre appareil. |
| Stratégie de l’ordinateur local \\ Configuration utilisateur \\ Modèles d’administration  |  |  |
|Panneau de configuration \\ Options régionales et linguistiques | Désactiver la proposition de prédictions de texte au cours de la frappe |  | Activé |
| Desktop (Expérience utilisateur) | Ne pas ajouter de partages des documents récemment ouverts dans Emplacements réseau |  | Activé |
| Desktop (Expérience utilisateur) | Désactiver les mouvements de souris réduisant la fenêtre Aero Shake |  | Activé |
| Poste de travail \\ Active Directory | Taille maximale des recherches dans Active Directory | 2 500 | Activé |
| Menu Démarrer et barre des tâches | Ne pas autoriser l’épinglage de l’application Windows Store à la barre des tâches |  | Activé |
| Menu Démarrer et barre des tâches | Ne pas afficher ni suivre les éléments des listes de raccourcis à partir d'emplacements distants |  | Activé |
| Menu Démarrer et barre des tâches | Ne pas utiliser la méthode basée sur la recherche pour déterminer les raccourcis du Bureau |  | Activé. Le système n’effectue pas la recherche finale sur le lecteur. Il affiche simplement un message expliquant que le fichier est introuvable. |
| Menu Démarrer et barre des tâches | Supprimer la barre Personnes de la barre des tâches |  | Activé. L’icône Personnes est supprimée de la barre des tâches, le bouton de bascule des paramètres correspondant est supprimé de la page des paramètres de la barre des tâches et les utilisateurs ne peuvent pas épingler des personnes à la barre des tâches. |
| Menu Démarrer et barre des tâches | Désactiver les bulles de notification d’annonce de fonctionnalité |  | Activé. Les utilisateurs ne peuvent pas épingler l’application Store à la barre des tâches. Si l’application Store est déjà épinglée à la barre des tâches, elle en est supprimée lors de la connexion suivante. |
| Menu Démarrer et barre des tâches | Désactiver le suivi utilisateur |  | Activé |
| Menu Démarrer et barre des tâches \\ Notifications | Désactiver les notifications toast |  | Activé |
| Composants Windows \\ Contenu cloud | Désactiver toutes les fonctionnalités Windows à la une |  | Activé |

### <a name="notes-about-network-connectivity-status-indicator"></a>Remarques sur l’Indicateur d’état de connectivité réseau

Les paramètres de stratégie de groupe ci-dessus incluent des paramètres pour désactiver la vérification du fait que le système est connecté à Internet. Si votre environnement ne se connecte pas du tout à Internet, ou s’il s’y connecte indirectement, vous pouvez définir un paramètre de stratégie de groupe pour supprimer l’icône Réseau de la barre des tâches. La raison pour laquelle vous pouvez souhaiter supprimer l’icône Réseau de la barre des tâches est que si vous désactivez les vérifications de la connectivité Internet, un drapeau jaune apparaît sur l’icône Réseau, même si le réseau peut fonctionner normalement. Si vous voulez supprimer l’icône Réseau via un paramètre de stratégie de groupe, vous pouvez le trouver à cet emplacement :

| Paramètre de stratégie | Élément | Sous-élément | Valeur possible et commentaires|
| -------------- | ---- | -------- | ---------------------------- |
| Windows Update ou Windows Update pour Entreprise | Choisir quand recevoir les mises à jour qualité | 1. 30 jours<br>2. Démarrage de la suspension des mises à jour qualité jj-mm-aaaa | Activé |
| Stratégie de l’ordinateur local \\ Configuration utilisateur \\ Modèles d’administration |  |  |  |
| Menu Démarrer et barre des tâches | Supprimer l’icône de mise en réseau |  | Activé. L’icône Mise en réseau n’est pas affichée dans la zone de notification système. |

Pour plus d’informations sur l’Indicateur d’état de la connexion réseau (NCSI), consultez [Gérer les points de terminaison de connexion pour Windows 10 entreprise, version 1903](https://docs.microsoft.com/windows/privacy/manage-windows-1903-endpoints) et [Gérer les connexions des composants du système d’exploitation Windows 10 aux services Microsoft](https://docs.microsoft.com/windows/privacy/manage-connections-from-windows-operating-system-components-to-microsoft-services).

### <a name="system-services"></a>Services système

Si vous envisagez de désactiver vos services système pour économiser des ressources, il est nécessaire de bien vérifier que le service considéré n’est pas d’une façon ou d’une autre un composant d’un autre service. Notez que certains services ne sont pas dans la liste, car leur désactivation n’est pas prise en charge.

La plupart de ces recommandations correspondent aux recommandations pour Windows Server 2016, installé avec Expérience utilisateur, indiquées dans [Conseils sur la désactivation de services système dans Windows Server 2016 avec Expérience utilisateur](https://docs.microsoft.com/windows-server/security/windows-services/security-guidelines-for-disabling-system-services-in-windows-server).

Beaucoup de services apparaissant comme de bons candidats à la désactivation sont définis avec un type de démarrage du service manuel. Cela signifie que le service ne démarre pas automatiquement et qu’il n’est pas démarré, sauf si un processus ou un événement déclenche une demande au service dont la désactivation est envisagée. Les services qui sont déjà configurés avec un type de démarrage manuel ne sont généralement pas listés ici.

> [!NOTE]
> Vous pouvez énumérer les services en cours d’exécution avec cet exemple de code PowerShell, en ne générant que le nom abrégé du service :

```powershell
 Get-Service | Where-Object {$_.Status -eq "Running"} | select -ExpandProperty Name
 ```

| Service Windows | Élément | Commentaires|
| -------------- | ---- | ---------------------------- |
| CDPUserService | Ce service utilisateur est utilisé pour les scénarios de plateforme d’appareils connectés | Il s’agit d’un service par utilisateur : le *modèle de service* doit donc être désactivé. |
| Expériences des utilisateurs connectés et télémétrie | Active les fonctionnalités qui prennent en charge les expériences utilisateur connectées et dans l’application. En outre, ce service gère la collecte et la transmission des informations de diagnostic et d'utilisation en fonction des événements (afin d'améliorer l'expérience et la qualité de la plateforme Windows) lorsque les paramètres des options de confidentialité des diagnostics et de l'utilisation sont activés sous Commentaires et Diagnostics. | Envisagez la désactivation si vous êtes sur un réseau déconnecté. |
| Données des contacts | Indexe les données des contacts pour la recherche rapide des contacts. Si vous arrêtez ou désactivez ce service, des contacts risquent de manquer dans vos résultats de recherche. | Il s’agit d’un service par utilisateur : le *modèle de service* doit donc être désactivé. |
| Service de stratégie de diagnostic | Permet la détection, le dépannage et la résolution des problèmes pour les composants Windows. Si ce service est arrêté, les diagnostics ne fonctionnent plus. | |
| Gestionnaire des cartes téléchargées | Service Windows pour l’accès des applications à des cartes téléchargées. Ce service est démarré à la demande par les applications accédant à des cartes téléchargées. La désactivation de ce service empêche les applications d’accéder à des cartes. | |
| Service de géolocalisation | Surveille l’emplacement actuel du système et gère les limites géographiques | |
| Service utilisateur GameDVR et Diffusion | Ce service utilisateur est utilisé pour les enregistrements des jeux et les diffusions en direct | Il s’agit d’un service par utilisateur : le modèle de service doit donc être désactivé. |
| MessagingService | Service prenant en charge la messagerie texte et les fonctionnalités associées. | Il s’agit d’un service par utilisateur : le *modèle de service* doit donc être désactivé. |
| Optimiser les lecteurs | Permet à l’ordinateur de s’exécuter plus efficacement en optimisant les fichiers sur les lecteurs de stockage. | Normalement, les solutions VDI ne bénéficient pas de l’optimisation des disques. Ces « lecteurs » ne sont pas des disques traditionnels et sont souvent simplement une allocation de stockage temporaire. |
| Superfetch | Gère et améliore le niveau de performance du système dans le temps. | En règle générale, n’améliore pas les performances sur VDI, en particulier la VDI non persistante, étant donné que l’état du système d’exploitation est abandonné à chaque redémarrage. |
| Service du clavier tactile et du volet d’écriture manuscrite | Active la fonctionnalité de clavier tactile et du volet d’écriture manuscrite et au stylet | |
| Rapport d’erreurs Windows | Permet le signalement des erreurs quand des programmes cessent de fonctionner ou de répondre, et permet de délivrer des solutions existantes. Permet également la génération des journaux pour les services de diagnostic et de réparation. Si ce service est arrêté, les rapports d’erreurs peuvent ne pas fonctionner correctement, et les résultats des services de diagnostic et de réparation peuvent ne pas être affichés. | Avec VDI, les diagnostics sont souvent effectués dans un scénario hors connexion et non pas dans le cadre de la production normale. En outre, certains clients désactivent de toute façon Rapport d’erreurs Windows. Rapport d’erreurs Windows consomme une petite quantité de ressources pour différentes choses, notamment les échecs d’installation d’un périphérique ou d’une mise à jour. |
| Service Partage réseau du Lecteur Windows Media | Partage des bibliothèques Lecteur Windows Media avec d’autres lecteurs réseau et des appareils multimédias en utilisant Plug and Play universel | Non nécessaire, sauf si les clients partagent des bibliothèques Lecteur Windows Media sur le réseau. |
| Service Point d’accès sans fil mobile | Donne la possibilité de partager une connexion de données cellulaires avec un autre appareil. | |
| Windows Search | Fournit l’indexation de contenu, la mise en cache de propriétés et les résultats de recherche pour les fichiers, les e-mails et d’autres contenus.                                                                    | Probablement non nécessaire en particulier avec la VDI non persistante |

#### <a name="per-user-services-in-windows"></a>Services par utilisateur dans Windows

Les services par utilisateur sont des services qui sont créés quand un utilisateur se connecte à Windows ou Windows Server, et qui sont arrêtés et supprimés quand cet utilisateur se déconnecte. Ces services s’exécutent dans le contexte de sécurité du compte d’utilisateur : ceci permet une meilleure gestion des ressources que l’approche précédente de l’exécution de ces types de services dans Explorer, associés à un compte préconfiguré, ou en tant que tâches.

[Services par utilisateur dans Windows 10 et Windows Server](https://docs.microsoft.com/windows/application-management/per-user-services-in-windows)

Si vous envisagez de changer une valeur de démarrage de service, la méthode recommandée consiste à ouvrir une invite de commandes .cmd avec élévation de privilèges et à exécuter l’outil Gestionnaire de contrôle des services « Sc.exe ». Pour plus d’informations sur l’utilisation de « Sc.exe », consultez [Sc](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc754599(v=ws.11)).

### <a name="scheduled-tasks"></a>Tâches planifiées

Comme d’autres éléments dans Windows, vérifiez qu’un élément n’est pas nécessaire avant d’envisager de le désactiver.

Les tâches de la liste suivante sont celles qui effectuent des optimisations ou des collectes de données sur les ordinateurs conservant leur état entre les redémarrages. Quand une tâche de machine virtuelle VDI redémarre et abandonne toutes les modifications effectuées depuis le dernier démarrage, les optimisations destinées aux ordinateurs physiques ne sont pas utiles.

Vous pouvez obtenir toutes les tâches planifiées en cours, y compris leur description, avec le code PowerShell suivant :

```powershell
 Get-ScheduledTask | Select-Object -Property TaskPath,TaskName,State,Description
```

>[!NOTE]
> Il existe plusieurs tâches qui ne peuvent pas être désactivées à l’aide d’un script, même si vous effectuez l’exécution avec élévation de privilèges. Nous vous recommandons de ne pas désactiver les tâches qui ne peuvent pas être désactivées à l’aide d’un script.

Nom de la tâche planifiée :

- Cellulaire
- Consolidator
- Diagnostics
- FamilySafetyMonitor
- FamilySafetyRefreshTask
- MaintenanceTasks
- MapsToastTask
- Compatibilité
- Microsoft-Windows-DiskDiagnosticDataCollector
- MNO
- NotificationTask
- PerformRemediation
- ProactiveScan
- ProcessMemoryDiagnosticEvents
- ProgramDataUpdater
- Proxy
- QueueReporting
- RecommendedTroubleshootingScanner
- ReconcileFeatures
- ReconcileLanguageResources
- RefreshCache
- RegIdleBackup
- ResPriStaticDbSync
- RunFullMemoryDiagnostic
- ScanForUpdates
- ScanForUpdatesAsUser
- Planifié
- ScheduledDefrag
- sihpostreboot
- SilentCleanup
- SmartRetry
- SpaceAgentTask
- SpaceManagerTask
- SpeechModelDownloadTask
- Sqm-Tasks
- SR
- StartComponentCleanup
- StartupAppTask
- StorageSense
- SyspartRepair
- Sysprep
- UninstallDeviceTask
- UpdateLibrary
- UpdateModelTask
- UsbCeip
- Usb-Notifications
- USO_UxBroker
- WiFi
- WIM-Hash-Management
- WindowsActionDialog
- WinSAT
- Dossiers
- WsSwapAssessmentTask
- XblGameSaveTask

### <a name="apply-windows-and-other-updates"></a>Appliquer les mises à jour de Windows (et d’autres mises à jour)

Qu’elles proviennent de Microsoft Update ou de vos ressources internes, appliquez les mises à jour disponibles, y compris les signatures Windows Defender. C’est le moment approprié pour appliquer d’autres mises à jour disponibles, y compris celles pour Microsoft Office s’il est installé et d’autres mises à jour logicielles. Si PowerShell reste dans l’image, vous pouvez télécharger la dernière aide disponible pour PowerShell en exécutant la commande [Update-Help](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/update-help?view=powershell-7).

#### <a name="servicing-the-operating-system-and-apps"></a>Maintenance du système d’exploitation et des applications

À un moment donné pendant le processus d’optimisation de l’image, les mises à jour Windows disponibles doivent être appliquées. Il existe un paramètre dans les paramètres de mise à jour de Windows 10 qui peut fournir des mises à jour supplémentaires :

![Mises à jour supplémentaires](media/rds-vdi-recommendations-1909/servicing.png)

C’est un bon choix dans le cas où vous vous apprêtez à installer des applications Microsoft, comme Microsoft Office, dans l’image de base. De cette façon, Office est à jour quand l’image est mise en service. Il existe également des mises à jour de .NET et de certains composants tiers, comme Adobe, qui ont des mises à jour disponibles via Windows Update.

Un point très important pour les machines virtuelles VDI non persistantes est celui des mises à jour de sécurité, y compris les fichiers de définition des logiciels de sécurité. Ces mises à jour peuvent être publiées une ou plusieurs fois par jour. Il doit y avoir un moyen de conserver ces mises à jour, notamment pour Windows Defender et des composants tiers.

Pour Windows Defender, il peut être préférable d’autoriser les mises à jour, même sur une VDI non persistante. Les mises à jour sont s’appliquer à presque chaque ouverture de session, mais il s’agit de petites mises à jour qui ne devraient pas poser de problème. En outre, la machine virtuelle n’est pas en retard sur les mises à jour, car seules les dernières mises à jour disponibles sont appliquées. La même chose s’applique pour les fichiers de définition tiers.

> [!NOTE]
> Les applications du Store (applications UWP) se mettent à jour via le Windows Store. Les versions modernes d’Office, comme Office 365, se mettent à jour via leurs propres mécanismes quand elles sont connectées directement à Internet, ou via des technologies de gestion quand ce n’est pas le cas.

### <a name="windows-system-startup-event-traces"></a>Suivis des événements de démarrage du système Windows

Par défaut, Windows est configuré pour collecter et enregistrer des données de diagnostic limitées. L’objectif est d’activer les diagnostics ou d’enregistrer des données si la résolution de problèmes est nécessaire. Les suivis système automatiques se trouvent à l’emplacement indiqué dans l’illustration suivante :

![Suivis système](media/rds-vdi-recommendations-1909/system-traces.png)

Certaines des traces affichées sous **Sessions de suivi d’événements** et **Sessions de trace des événements Startup** ne peuvent pas et ne doivent pas être arrêtées. D’autres, comme la trace « WiFiSession », peuvent être arrêtées. Pour arrêter une trace en cours d’exécution sous **Sessions de suivi d’événements**, cliquez avec le bouton droit sur la trace, puis cliquez sur « Arrêter ». Utilisez la procédure suivante pour empêcher les traces de démarrer automatiquement au démarrage :

1. Cliquez sur le dossier **Sessions de trace des événements Startup**.

2. Recherchez la trace qui vous intéresse, puis double-cliquez sur celle-ci.

3. Cliquez sur l’onglet **Session de suivi**.

4. Cliquez sur la zone intitulée **Activé** pour supprimer la coche.

5. Cliquez sur **OK**.

Voici quelques traces système dont vous pouvez envisager la désactivation pour une utilisation de VDI :

| Nom                    | Comment                       |
| ----------------------- | ----------------------------- |
| AppModel | Une collection de suivis, l’un d’entre eux concernant le téléphone |
| CloudExperienceHostOOBE | |
| DiagLog | |
| NtfsLog | |
| TileStore | |
| UBPM | |
| WiFiDriverIHVSession | Si vous n’utilisez pas d’appareil WiFi |
| WiFiSession | |
| WinPhoneCritical | |

### <a name="windows-defender-optimization-with-vdi"></a>Optimisation de Windows Defender avec VDI

Microsoft a récemment publié une documentation concernant Windows Defender dans un environnement VDI. Consultez [Guide de déploiement de l’antivirus Windows Defender dans un environnement d’infrastructure de bureau virtuel (VDI)](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/deployment-vdi-windows-defender-antivirus) pour plus d’informations.

L’article ci-dessus contient des procédures pour la maintenance de l’image « gold » de VDI et explique comment maintenir les clients VDI alors qu’ils sont en cours d’exécution. Pour réduire la bande passante réseau quand les ordinateurs VDI doivent mettre à jour leurs signatures Windows Defender, échelonnez les redémarrages et planifiez-les pour qu’ils aient lieu pendant les heures creuses quand c’est possible. Les mises à jour des signatures Windows Defender peuvent se trouver en interne sur des partages de fichiers ; là où c’est possible, placez ces partages de fichiers sur les mêmes segments réseau que les machines virtuelles VDI (ou sur des segments proches).

### <a name="client-network-performance-tuning-by-registry-settings"></a>Réglage des performances réseau du client à l’aide des paramètres du Registre

Certains paramètres du Registre peuvent accroître les performances du réseau. Ceci est particulièrement important dans les environnements où l’ordinateur ou VDI a une charge de travail qui est principalement basée sur le réseau. Les paramètres de cette section sont recommandés pour optimiser les performances en faveur du réseau, en configurant une mise en mémoire tampon supplémentaire et la mise en cache d’éléments comme les entrées de répertoire.

>[!NOTE]
> Certains paramètres de cette section sont uniquement basés sur le Registre et doivent être incorporés dans l’image de base avant que l’image soit déployée pour une utilisation en production.

Les paramètres suivants sont documentés dans le [Guide d’optimisation des performances de Windows Server 2016](https://docs.microsoft.com/windows-server/administration/performance-tuning/), publié sur Microsoft.com par le groupe des produits Windows.

#### <a name="disablebandwidththrottling"></a>DisableBandwidthThrottling

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DisableBandwidthThrottling`

S’applique à Windows 10 La valeur par défaut est **0**. Par défaut, le redirecteur SMB limite le débit sur les connexions réseau à latence élevée, dans certains cas afin d’éviter des délais d’attente liés au réseau. Définir cette valeur de Registre sur 1 désactive cette limitation, en activant un débit de transfert de fichiers plus élevé sur des connexions réseau à latence élevée. Envisagez de définir cette valeur sur **1**.

#### <a name="fileinfocacheentriesmax"></a>FileInfoCacheEntriesMax

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileInfoCacheEntriesMax` S’applique à Windows 10. La valeur par défaut est **64**, avec une plage valide allant de 1 à 65 536. Cette valeur permet de déterminer la quantité de métadonnées de fichier pouvant être mises en cache par le client. Une augmentation de la valeur peut réduire le trafic réseau et améliorer les performances quand le système doit accéder à un grand nombre de fichiers. Essayez en augmentant cette valeur à **1 024**.

#### <a name="directorycacheentriesmax"></a>DirectoryCacheEntriesMax

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DirectoryCacheEntriesMax`

S’applique à Windows 10 La valeur par défaut est **16**, avec une plage valide allant de 1 à 4 096. Cette valeur permet de déterminer la quantité d’informations de répertoire pouvant être mises en cache par le client. Augmenter la valeur peut réduire le trafic réseau et améliorer les performances lorsque des répertoires volumineux sont consultés. Envisagez d’augmenter cette valeur à **1 024**.

#### <a name="filenotfoundcacheentriesmax"></a>FileNotFoundCacheEntriesMax

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileNotFoundCacheEntriesMax`

S’applique à Windows 10 La valeur par défaut est **128**, avec une plage valide allant de 1 à 65 536. Cette valeur permet de déterminer la quantité d’informations de nom de fichier pouvant être mises en cache par le client. Une augmentation de la valeur peut réduire le trafic réseau et améliorer les performances quand le système doit accéder à un grand nombre de noms de fichier. Envisagez d’augmenter cette valeur à **2 048**.

#### <a name="dormantfilelimit"></a>DormantFileLimit

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DormantFileLimit`

S’applique à Windows 10 La valeur par défaut est **1 023**. Ce paramètre spécifie le nombre maximal de fichiers devant être laissés ouverts sur une ressource partagée une fois que l’application a fermé le fichier. Là où plusieurs milliers de clients se connectent à des serveurs SMB, réduisez cette valeur à **256**.

Vous pouvez configurer beaucoup de ces paramètres SMB à l’aide des applets de commande Windows PowerShell [Set-SmbClientConfiguration](https://docs.microsoft.com/powershell/module/smbshare/set-smbclientconfiguration?view=win10-ps) et [Set-SmbServerConfiguration](https://docs.microsoft.com/powershell/module/smbshare/set-smbserverconfiguration?view=win10-ps). Les paramètres de Registre uniquement peuvent être configurés à l’aide de Windows PowerShell, comme dans l’exemple suivant :

```powershell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters" RequireSecuritySignature -Value 0 -Force
```

Paramètres supplémentaires provenant des conseils donnés dans Ligne de base de la fonctionnalité limitée du trafic restreint Windows. Microsoft a publié une base de référence créée avec les mêmes procédures que les [Bases de référence de sécurité Windows](https://docs.microsoft.com/powershell/module/smbshare/set-smbserverconfiguration?view=win10-ps) pour les environnements qui ne sont pas connectés directement à Internet, ou si vous voulez réduire les données envoyées à Microsoft et à d’autres services.

Les paramètres de la [Ligne de base de la fonctionnalité limitée du trafic restreint Windows](https://docs.microsoft.com/windows/privacy/manage-connections-from-windows-operating-system-components-to-microsoft-services) sont signalés par un astérisque dans le tableau des paramètres de stratégie de groupe.

#### <a name="disk-cleanup-including-using-the-disk-cleanup-wizard"></a>Nettoyage de disque (y compris avec l’Assistant Nettoyage de disque)

Le nettoyage de disque peut être particulièrement utile avec les implémentations VDI de l’image principale/gold. Une fois l’image préparée, mise à jour et configurée, une des dernières tâches à effectuer est le nettoyage de disque. Il existe un outil intégré appelé « Assistant Nettoyage de disque » qui peut aider à nettoyer la plupart des zones potentielles d’économies d’espace disque. Sur une machine virtuelle sur laquelle très peu d’éléments sont installés, mais qui a été entièrement corrigée, vous pouvez généralement libérer 4 Go d’espace disque en exécutant Nettoyage de disque.

Voici des suggestions pour les différentes tâches de nettoyage de disque. Elles doivent toutes être testées avant d’être implémentées :

1. Exécutez l’Assistant Nettoyage de disque (avec élévation de privilèges) après avoir appliqué toutes les mises à jour. Incluez les catégories « Optimisation de la distribution » et « Nettoyage de Windows Update ». Ce processus peut être automatisé à l’aide de la ligne de commande `Cleanmgr.exe` avec l’option `/SAGESET:11`. L’option `/SAGESET` définit les valeurs de Registre qui peuvent être utilisées ultérieurement pour automatiser le nettoyage de disque, qui utilise toutes les options disponibles dans l’Assistant Nettoyage de disque.

    1. Sur une machine virtuelle de test, à partir d’une nouvelle installation, l’exécution de `Cleanmgr.exe /SAGESET:11` révèle qu’il existe seulement deux options de nettoyage de disque automatique activées par défaut :
    
        - Fichiers programme téléchargés

        - Fichiers Internet temporaires

    2. Si vous définissez plus d’options ou toutes les options, celles-ci sont enregistrées dans le Registre, en fonction de la valeur d’**Index** fournie dans la commande précédente (`Cleanmgr.exe /SAGESET:11`). Dans ce cas, nous allons utiliser la valeur `11` comme index, pour une procédure de nettoyage automatique du disque ultérieure.

    3. Une fois `Cleanmgr.exe /SAGESET:11` exécuté, plusieurs catégories d’options de nettoyage de disque vous sont proposées. Vous pouvez cocher toutes les options, puis cliquer sur **OK**. L’Assistant Nettoyage de disque disparaît et vos paramètres sont enregistrés dans le Registre.

2. Nettoyez votre stockage Cliché instantané des volumes, s’il y en a en cours d’utilisation.

    - Ouvrez une invite de commandes avec élévation de privilèges et exécutez la commande `vssadmin list shadows` puis la commande `vssadmin list shadowstorage`.
    
        Si la sortie de ces commandes est **Il n’existe aucun élément correspondant à la requête**, c’est qu’aucun stockage VSS n’est en cours d’utilisation.

3. Nettoyez les fichiers temporaires et les journaux. Depuis une invite de commandes avec élévation de privilèges, exécutez les commandes `Del C:\*.tmp /s`, `Del C:\Windows\Temp\.` et `Del %temp%\.`.

4. Supprimez tous les profils inutilisés sur le système en exécutant `wmic path win32_UserProfile where LocalPath="c:\users\<user>" Delete`.

### <a name="remove-onedrive-components"></a>Supprimer les composants OneDrive

La suppression de OneDrive implique la suppression du package, et la désinstallation et la suppression des fichiers *.lnk. L’exemple de code PowerShell suivant peut être utilisé pour aider à supprimer OneDrive de l’image et est inclus dans les scripts d’optimisation de l’infrastructure VDI GitHub :

```azurecli

Taskkill.exe /F /IM "OneDrive.exe"
Taskkill.exe /F /IM "Explorer.exe"` 
    if (Test-Path "C:\\Windows\\System32\\OneDriveSetup.exe")`
     { Start-Process "C:\\Windows\\System32\\OneDriveSetup.exe"`
         -ArgumentList "/uninstall"`
         -Wait }
    if (Test-Path "C:\\Windows\\SysWOW64\\OneDriveSetup.exe")`
     { Start-Process "C:\\Windows\\SysWOW64\\OneDriveSetup.exe"`
         -ArgumentList "/uninstall"`
         -Wait }
Remove-Item -Path
"C:\\Windows\\ServiceProfiles\\LocalService\\AppData\\Roaming\\Microsoft\\Windows\\Start Menu\\Programs\\OneDrive.lnk" -Force
Remove-Item -Path "C:\\Windows\\ServiceProfiles\\NetworkService\\AppData\\Roaming\\Microsoft\\Windows\\Start Menu\\Programs\\OneDrive.lnk" -Force \# Remove the automatic start item for OneDrive from the default user profile registry hive
Start-Process C:\\Windows\\System32\\Reg.exe -ArgumentList "Load HKLM\\Temp C:\\Users\\Default\\NTUSER.DAT" -Wait
Start-Process C:\\Windows\\System32\\Reg.exe -ArgumentList "Delete HKLM\\Temp\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Run /v OneDriveSetup /f" -Wait
Start-Process C:\\Windows\\System32\\Reg.exe -ArgumentList "Unload HKLM\\Temp" -Wait Start-Process -FilePath C:\\Windows\\Explorer.exe -Wait
```

Pour toute question ou interrogation sur les informations contenues dans ce document, contactez l’équipe de votre compte Microsoft, effectuez des recherches sur le blog VDI de Microsoft, postez un message sur les forums Microsoft ou contactez Microsoft pour des questions ou des interrogations.

## <a name="turn-windows-update-back-on"></a>Réactiver Windows Update

Si vous souhaitez réactiver Windows Update, comme dans le cas d’une infrastructure VDI persistante, suivez ces étapes :

- Réactivez ces paramètres de stratégie de groupe :

    - Stratégie Ordinateur local \\ Configuration ordinateur \\ Modèles d’administration \\ Système \\ Gestion de la communication Internet \\ Paramètre de communication Internet

        - Désactiver l’accès à toutes les fonctionnalités Windows Update (remplacer **activé** par **non configuré**).

    - Stratégie Ordinateur local \\ Configuration ordinateur \\Modèles d’administration \\ Composants Windows \\ Windows Update

        - Supprimer l’accès à toutes les fonctionnalités Windows Update (remplacer **activé** par **non configuré**)

        - Ne pas se connecter à des emplacements Internet Windows Update (remplacer **activé** par **non configuré**).

    - Stratégie Ordinateur local \\ Configuration ordinateur \\ Modèles d’administration \\ Composants Windows \\ Windows Update \\ Windows Update pour Entreprise

        - Choisir quand recevoir les mises à jour qualité (remplacer « activé » par « non configuré »)

    -   Stratégie Ordinateur local \\ Configuration ordinateur \\ Modèles d’administration \\ Composants Windows \\ Windows Update \\ Windows Update pour Entreprise

        - Choisir le moment où les versions d’évaluation et les mises à jour des fonctionnalités sont reçues (remplacer **activé** par **non configuré**)

-  Réactiver le ou les services

    - Mettez à jour le service Orchestrator (remplacer **désactivé** par **Automatique (Début différé)** ).

    - Modifiez les paramètres de Registre Windows suivants :

        - HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\PolicyState

            - DeferQualityUpdates (remplacer **1** par **0**)

        - HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\Settings

            - PausedQualityDate (supprimer toute valeur existante)

        - HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer\WAU

            - Désactivé

-  Réactiver les tâches planifiées

    - Bibliothèque du Planificateur de tâches \\ Microsoft \\ Windows \\ InstallService\\ ScanForUpdates

    - Bibliothèque du Planificateur de tâches \\ Microsoft \\ Windows \\ InstallService\\ ScanForUpdatesAsUser

Pour que tous ces paramètres prennent effet, redémarrez l’appareil. Si vous ne souhaitez pas que cet appareil se voit proposer des mises à jour de fonctionnalités, accédez à Paramètres \\ Windows Update \\ Options avancées \\ Choisir quand installer les mises à jour, puis définissez manuellement l’option **Une mise à jour des fonctionnalités inclut des améliorations et de nouvelles fonctionnalités. Elle peut être différée pendant ce nombre de jours** sur une valeur différente de zéro, telle que 180 ou 365.

### <a name="references"></a>Références

- [What is VDI (virtual desktop infrastructure)](https://www.citrix.com/glossary/vdi.html)

- [Sysprep échoue après avoir supprimé ou mis à jour les applications Microsoft Store qui incluent des images Windows intégrées](https://support.microsoft.com/help/2769827/sysprep-fails-after-you-remove-or-update-windows-store-apps-that-inclu).
