---
title: Fonctionnement du contrôle de compte d’utilisateur
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tpm
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da83ddb2-6182-417c-aa8e-0b47b2e17d13
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 7f5ad234959ebfe0687b8bc10f9e43f2092252f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823550"
---
# <a name="how-user-account-control-works"></a>Fonctionnement du contrôle de compte d’utilisateur

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Le Contrôle de compte d’utilisateur (UAC) contribue à éviter que des programmes malveillants n’endommagent un ordinateur tout en permettant aux organisations de déployer un bureau mieux administré. Avec UAC, les applications et les tâches sont toujours exécutées dans le contexte de sécurité d’un compte non administrateur, sauf si un administrateur autorise spécifiquement l’accès administrateur au système. UAC peut bloquer l’installation automatique des applications non autorisées et empêche les modifications accidentelles aux paramètres système.

## <a name="uac-process-and-interactions"></a>Interactions et les processus de l’UAC
Chaque application requérant le jeton d’accès administrateur doit inviter l’administrateur à consentir. La seule exception à cette règle est la relation qui existe entre les processus parent et enfant. Processus enfant héritent du jeton d’accès utilisateur du processus parent. Les deux processus parent et enfant doivent cependant avoir le même niveau d’intégrité. Windows Server 2012 protège les processus en marquant leurs niveaux d’intégrité. Les niveaux d’intégrité sont des mesures de la confiance. Une application à intégrité « élevée » est une qui effectue des tâches qui modifient les données système, telle qu’un disque de partitionnement d’application, pendant une application à intégrité « faible » exécute des tâches susceptibles de compromettre le système d’exploitation, comme un navigateur Web. Applications avec des niveaux d’intégrité inférieur ne peut pas modifier les données dans les applications avec des niveaux d’intégrité plus élevées. Lorsqu’un utilisateur standard tente d’exécuter une application qui requiert un jeton d’accès administrateur, l’UAC exige que l’utilisateur fournisse des informations d’identification administrateur valides.

Pour mieux comprendre comment ce processus se produit qu’il est important d’examiner les détails du processus d’ouverture de session Windows Server 2012.

### <a name="windows-server-2012-logon-process"></a>Processus d’ouverture de session Windows Server 2012
L’illustration suivante montre comment le processus d’ouverture de session pour un administrateur diffère du processus d’ouverture de session pour un utilisateur standard.

![Illustration montrant comment le processus d’ouverture de session pour un administrateur diffère du processus d’ouverture de session pour un utilisateur standard](../media/How-User-Account-Control-Works/UACWindowsLogonProcess.gif)

Par défaut, les utilisateurs standard et les administrateurs accèdent aux ressources et exécutent des applications dans le contexte de sécurité des utilisateurs standard. Lorsqu’un utilisateur ouvre une session sur un ordinateur, le système crée un jeton d’accès pour cet utilisateur. Ce jeton d’accès contient des informations sur le niveau d’accès accordé à l’utilisateur, notamment des identificateurs de sécurité et des privilèges Windows spécifiques.

Lorsqu’un administrateur ouvre une session, deux jetons d’accès sont créés pour l’utilisateur : un jeton d’accès utilisateur standard et un jeton d’accès administrateur. Le jeton d’accès utilisateur standard contient les mêmes informations spécifiques à l’utilisateur en tant que le jeton d’accès administrateur, mais les privilèges d’administrateur de Windows et les SID sont supprimés. Le jeton d’accès utilisateur standard est utilisé pour démarrer des applications qui n’effectuent pas les tâches d’administration (les applications utilisateur standard). Le jeton d’accès utilisateur standard est ensuite utilisé pour afficher le bureau (Explorer.exe). Explorer.exe est le processus parent duquel tous les autres processus lancés par l’utilisateur héritent leur jeton d’accès. Par conséquent, toutes les applications s’exécutent avec des privilèges d’utilisateur standard, sauf si un utilisateur fournit son consentement ou des informations d’identification pour utiliser un jeton d’accès administrateur total dans une application.

Un utilisateur qui est membre du groupe Administrateurs peut ouvrir une session, naviguer sur le Web et lire les messages électroniques lors de l’utilisation d’un jeton d’accès utilisateur standard. Lorsque l’administrateur doit effectuer une tâche qui nécessite le jeton d’accès administrateur, Windows Server 2012 automatiquement invite l’utilisateur pour l’approbation. Cette demande s’appelle une invite d’élévation. Vous pouvez configurer son comportement à l’aide du composant logiciel enfichable Stratégie de sécurité locale (Secpol.msc) ou de la stratégie de groupe.

> [!NOTE]
> Le terme « élever » est utilisé pour faire référence au processus dans Windows Server 2012 qui invite l’utilisateur à donner son consentement ou informations d’identification pour utiliser un jeton d’accès administrateur complet.

### <a name="the-uac-user-experience"></a>L’expérience utilisateur UAC
Lorsque UAC est activé, l’expérience utilisateur pour les utilisateurs standard est différent de celui des administrateurs en Mode d’approbation administrateur. La méthode recommandée et plus sûre de l’exécution de Windows Server 2012 consiste à apporter à votre utilisateur principal de compte d’utilisateur standard. L’exécution en tant qu’utilisateur standard contribue à optimiser la sécurité d’un environnement géré. Avec le composant d’élévation UAC intégré, les utilisateurs standard peuvent exécuter facilement une tâche administrative en entrant les informations d’identification valides pour un compte d’administrateur local. La valeur par défaut, le composant d’élévation UAC intégré pour les utilisateurs standard est l’invite d’informations d’identification.

L’alternative à l’exécution en tant qu’utilisateur standard consiste à exécuter en tant qu’administrateur en Mode approbation administrateur. Avec le composant d’élévation UAC intégré, les membres du groupe Administrateurs local peuvent facilement effectuer une tâche administrative en fournissant l’approbation. La valeur par défaut, le composant d’élévation UAC intégré pour un compte administrateur en Mode d’approbation administrateur est appelé l’invite de consentement. Le comportement invite d’élévation UAC peut être configuré à l’aide du composant logiciel enfichable Stratégie de sécurité locale (Secpol.msc) ou la stratégie de groupe.

**Les invites de consentement et informations d’identification**

Avec l’UAC activé, Windows Server 2012 demande le consentement ou vous invite à entrer des informations d’identification d’un compte d’administrateur local valide avant de démarrer un programme ou une tâche requérant un jeton d’accès administrateur complet. Cette invitation assure qu’aucun logiciel malveillant ne peut être installé en mode silencieux.

**Demande de consentement**

L’invite de consentement est présentée lorsqu’un utilisateur tente d’effectuer une tâche qui nécessite un jeton d’accès d’administration. Voici une capture d’écran de l’invite de consentement UAC.

![Capture d’écran de l’invite de consentement UAC](../media/How-User-Account-Control-Works/UACConsentPrompt.gif)

**Demande d’informations d’identification**

La demande d’informations d’identification est présentée lorsqu’un utilisateur standard tente d’effectuer une tâche nécessitant un jeton d’accès administrateur. Le comportement des invites par défaut d’utilisateur standard peut être configuré à l’aide du composant logiciel enfichable Stratégie de sécurité locale (Secpol.msc) ou de la stratégie de groupe. Les administrateurs peuvent également être requis pour fournir leurs informations d’identification en définissant le contrôle de compte d’utilisateur : Comportement de l’invite d’élévation pour les administrateurs dans la stratégie de Mode d’approbation administrateur si valeur invite d’informations d’identification.

La capture d’écran suivante est un exemple de l’invite d’informations d’identification de compte d’utilisateur.

![Capture d’écran montrant un exemple de l’invite d’informations d’identification de compte d’utilisateur](../media/How-User-Account-Control-Works/UACCredentialPrompt.gif)

**Invites d’élévation UAC**

Les invites d’élévation du contrôle de compte d’utilisateur sont codées avec des couleurs différentes selon l’application, ce qui permet d’identifier immédiatement le risque de sécurité potentiel de l’application. Lorsqu’une application tente de s’exécuter avec un jeton d’accès complet d’un administrateur, Windows Server 2012 analyse d’abord le fichier exécutable pour déterminer son serveur de publication. Les applications sont tout d’abord séparées en trois catégories basées sur le serveur de publication du fichier exécutable :  Windows Server 2012, Éditeur vérifié (signé) et éditeur non vérifié (non signé). Le diagramme suivant illustre la façon dont Windows Server 2012 détermine quelle invite d’élévation de couleur à présenter à l’utilisateur.

L’élévation couleurs d’invitations est comme suit :

-   Arrière-plan rouge avec une icône de bouclier rouge : L’application est bloquée par la stratégie de groupe ou émane d’un éditeur qui est bloqué.

-   Arrière-plan bleu avec une icône de bouclier bleue et dorée : L’application est une application administrative Windows Server 2012, comme un élément du panneau.

-   Arrière-plan bleu avec une icône de bouclier bleue : L’application est signée à l’aide d’Authenticode et est approuvée par l’ordinateur local.

-   Arrière-plan jaune avec une icône de bouclier jaune : L’application est signée ou non signée mais n’est ne pas encore approuvé par l’ordinateur local.

**Icône de bouclier**

Certains éléments du Panneau de configuration, tels que **Propriétés de dates et d’heures**, contiennent une combinaison d’opérations d’utilisateur standard et d’administrateur. Les utilisateurs standard peuvent afficher l’horloge et changer le fuseau horaire, mais un jeton d’accès administrateur complet est requis pour modifier l’heure système locale. L’image suivante est une capture d’écran de l’élément du Panneau de configuration **Propriétés de dates et d’heures**.

![Écran montrant la ** élément de Date et heure ** propriétés du panneau.](../media/How-User-Account-Control-Works/UACShieldIcon.gif)

L’icône de bouclier sur le bouton **Changer la date et l’heure** indique que le processus requiert un jeton d’accès administrateur complet et qu’il va afficher une invite d’élévation du contrôle de compte d’utilisateur.

**Sécurisation de l’invite d’élévation**

La sécurisation du processus d’élévation est renforcée car l’invite est envoyée sur le bureau sécurisé. Les invites de consentement et informations d’identification sont affichés sur le bureau sécurisé par défaut dans Windows Server 2012. Seuls les processus Windows peuvent accéder au bureau sécurisé. Pour obtenir des niveaux plus élevés de sécurité, nous vous recommandons de conserver le **contrôle de compte d’utilisateur : Passer au bureau sécurisé lors d’une demande d’élévation** paramètre de stratégie est activé.

Lorsqu’un fichier exécutable demande l’élévation, le bureau interactif, également appelé bureau de l’utilisateur, passe au bureau sécurisé. Le bureau sécurisé s’estompe bureau de l’utilisateur et affiche une invite d’élévation doit obtenir une réponse avant de continuer. Lorsque l’utilisateur clique sur Oui ou non, le bureau repasse au bureau de l’utilisateur.

Les logiciels malveillants peuvent présenter une imitation du bureau sécurisé, mais lorsque le contrôle de compte d’utilisateur : Comportement de l’invite d’élévation pour les administrateurs en Mode d’approbation administrateur paramètre de stratégie est défini à l’invite de consentement, le logiciel malveillant n’obtient pas d’élévation si l’utilisateur clique sur Oui sur l’imitation. Si le paramètre de stratégie est défini à l’invite pour les informations d’identification, les logiciels malveillants imitant l’invite d’informations d’identification peuvent être en mesure de rassembler les informations d’identification de l’utilisateur. Toutefois, le logiciel malveillant ne peut pas accéder à privilèges élevés et le système dispose d’autres protections bloquant les programmes malveillants de prendre le contrôle de l’interface utilisateur même avec un mot de passe collecté.

Si les logiciels malveillants peuvent présenter une imitation du bureau sécurisé, ce problème ne peut pas se produire sauf si un utilisateur a précédemment installé les logiciels malveillants sur l’ordinateur. Comme les processus nécessitant un jeton d’accès administrateur ne peuvent pas s’installer en mode silencieux lorsque le contrôle de compte d’utilisateur est activé, l’utilisateur doit explicitement fournir un consentement en cliquant sur **Oui** ou en fournissant des informations d’identification d’administrateur. Le comportement spécifique de l’invite d’élévation UAC dépend de la stratégie de groupe.

## <a name="uac-architecture"></a>Architecture UAC
Le diagramme suivant montre l’architecture du contrôle de compte d’utilisateur en détail.

![Diagramme décrivant l’architecture UAC](../media/How-User-Account-Control-Works/UACArchitecture.gif)

Pour mieux comprendre chaque composant, consultez le tableau ci-dessous :

|Component|Description|
|-------|--------|
|**Utilisateur**||
|Utilisateur effectue l’opération qui requiert des privilèges|Si l’opération modifie le système de fichiers ou le Registre, la virtualisation est appelée. Toutes les autres opérations appellent ShellExecute.|
|ShellExecute|ShellExecute appelle CreateProcess. ShellExecute recherche l’erreur ERROR_ELEVATION_REQUIRED dans CreateProcess. S’il reçoit l’erreur, ShellExecute appelle le service Informations sur l’application pour tenter d’effectuer la tâche demandée à l’aide de l’invite avec élévation de privilèges.|
|CreateProcess|Si l’application nécessite une élévation, CreateProcess rejette l’appel avec ERROR_ELEVATION_REQUIRED.|
|**Système**||
|Service informations d’application|Service système qui démarre des applications nécessitant un ou plusieurs privilèges ou droits d’utilisateur élevés pour s’exécuter (des tâches d’administration locales, par exemple), et des applications nécessitant des niveaux d’intégrité plus élevés. Le démarrage du service informations d’Application permet de donner son consentement de telles applications en créant un nouveau processus pour l’application avec un jeton d’accès complet d’un utilisateur administratif lorsqu’une élévation est requise et (selon la stratégie de groupe) est fourni par l’utilisateur à le faire.|
|Élévation d’une installation d’ActiveX|Si ActiveX n’est pas installé, le système vérifie le niveau de curseur UAC. Si ActiveX est installé, le **contrôle de compte d’utilisateur : Passer au bureau sécurisé lors d’une demande d’élévation** paramètre de stratégie de groupe est activée.|
|Vérifier le niveau de curseur UAC|L’UAC possède désormais les quatre niveaux de notification au choix et un curseur à utiliser pour sélectionner le niveau de notification :<br /><br /><ul><li>Élevée<br /><br />    Si le curseur est défini sur **Toujours m’avertir**, le système vérifie si le Bureau sécurisé est activé.</li><li>Moyen<br /><br />    Si le curseur est défini sur **par défaut-notifier m’uniquement quand des programmes tentent d’apporter des modifications à mon ordinateur**, le **contrôle de compte d’utilisateur : Élever uniquement les fichiers exécutables signés et validés** paramètre de stratégie est vérifié :<br /><br /><ul><li>Si le paramètre de stratégie est activé, la validation du chemin d’accès de certification infrastructure à clé publique (PKI) est appliquée pour un fichier exécutable donné avant qu’il est autorisé à exécuter.</li><li>Si le paramètre de stratégie n’est pas activé, la validation du chemin de certification PKI n’est pas appliquée avant qu’un fichier exécutable donné soit autorisé à s’exécuter. Le paramètre de stratégie **Contrôle de compte d'utilisateur : Passer au bureau sécurisé lors d’une demande d’élévation** paramètre de stratégie de groupe est activée.</li></ul></li><li>Faible<br /><br />    Si le curseur est défini sur **M’avertir uniquement quand des programmes tentent d’apporter des modifications à mon ordinateur (ne pas estomper mon Bureau).**, le processus CreateProcess est appelé.</li><li>Ne jamais m’avertir<br /><br />    Si le curseur est défini sur **ne jamais m’avertir lorsque**, sera invite UAC ne jamais m’avertir lorsqu’un programme est tente d’installer ou tente d’effectuer toute modification sur l’ordinateur. **Important :**     Ce paramètre n’est pas recommandé. Ce paramètre est équivalent au paramètre la **contrôle de compte d’utilisateur : Comportement de l’invite d’élévation pour les administrateurs en Mode d’approbation administrateur** paramètre de stratégie pour **élever sans invite**.</li></ul>|
|Bureau sécurisé activé|Le paramètre de stratégie **Contrôle de compte d'utilisateur : Passer au bureau sécurisé lors d’une demande d’élévation** paramètre de stratégie est vérifié :<br /><br />-Si le bureau sécurisé est activé, toutes les demandes d’élévation passent au bureau sécurisé indépendamment des paramètres de stratégie de comportement d’invite pour les administrateurs et les utilisateurs standard.<br />-Si le bureau sécurisé n’est pas activé, toutes les demandes d’élévation passent au bureau de l’utilisateur interactif, et les paramètres par utilisateur pour les administrateurs et les utilisateurs standard sont utilisés.|
|CreateProcess|CreateProcess appelle AppCompat, Fusion et programme d’installation de détection pour évaluer si l’application nécessite une élévation. Le fichier exécutable est alors examiné pour déterminer son niveau d’exécution demandé, qui est stocké dans le manifeste d’application du fichier exécutable. CreateProcess échoue si le niveau d’exécution demandé spécifié dans le manifeste ne correspond pas au jeton d’accès et s’il renvoie une erreur (ERROR_ELEVATION_REQUIRED) à ShellExecute. |
|AppCompat|La base de données AppCompat stocke les informations dans les entrées correctif de compatibilité des applications pour une application.|
|Fusion|La base de données de Fusion stocke les informations à partir de manifestes d’application qui décrivent les applications. Le schéma de manifeste est mis à jour pour ajouter un nouveau champ de niveau d’exécution demandé.|
|Détection du programme d’installation|Installer detection détecte les fichiers exécutables du programme d’installation, qui vous aide à empêcher les installations à partir d’en cours d’exécution sans connaissance et le consentement de l’utilisateur.|
|**Kernel**||
|Virtualisation|Technologie de virtualisation assure que les applications non conformes n’échouent pas en mode silencieux à exécuter ou échouent de manière qu’il est impossible de déterminer la cause. UAC fournit également la virtualisation de fichiers et du Registre et de journalisation pour les applications qui écrivent dans des zones protégées.|
|Système de fichiers et Registre|La virtualisation de fichiers et du Registre par utilisateur redirige Registre par ordinateur et les demandes d’écriture de fichiers vers des emplacements utilisateur équivalente. Les demandes de lecture sont d’abord redirigées vers l’emplacement par utilisateur virtualisé, puis vers l’emplacement par ordinateur.|

Il existe une modification de compte d’utilisateur Windows Server 2012 à partir de versions précédentes de Windows. Le nouveau curseur va jamais désactiver UAC complètement. Le nouveau paramètre sera :

-   Maintenir le service de contrôle de compte utilisateur en cours d’exécution.

-   Cause élévation toutes les demandes initiée par les administrateurs à être approuvée automatiquement sans afficher d’un UAC invite.

-   Refuser automatiquement toutes les demandes d’élévation pour les utilisateurs standard.

> [!IMPORTANT]
> Pour désactiver complètement le contrôle de compte utilisateur, vous devez désactiver la stratégie **contrôle de compte d’utilisateur : Exécuter tous les administrateurs en Mode d’approbation administrateur**.

> [!WARNING]
> Applications personnalisées ne fonctionnent pas sur Windows Server 2012 lorsque UAC est désactivé.

### <a name="virtualization"></a>Virtualisation
Étant donné que les administrateurs système dans les entreprises tentent de sécuriser les systèmes, de nombreuses applications métier sont conçues pour n’utiliser qu’un jeton d’accès utilisateur standard. Par conséquent, les administrateurs informatiques n’avez pas besoin de remplacer la majorité des applications lors de l’exécution de Windows Server 2012 avec l’UAC activé.

 Windows Server 2012 comprend la technologie de virtualisation de fichiers et de Registre pour les applications qui ne sont pas conformes à l’UAC et qui requièrent un jeton d’accès administrateur pour s’exécuter correctement. La virtualisation assure que même les applications qui ne sont pas compatibles UAC sont compatibles avec Windows Server 2012. Lorsqu’une application administrative qui n’est pas conformes à l’UAC tente d’écrire dans un répertoire protégé, tel que Program Files, l’UAC donne à l’application sa propre vue virtualisée de la ressource qu’il tente de modifier. La copie virtualisée est gérée dans le profil utilisateur. Cette stratégie crée une copie distincte du fichier virtualisée pour chaque utilisateur qui exécute l’application non conforme.

La plupart des tâches d’application fonctionnent correctement à l’aide des fonctionnalités de virtualisation. Bien que la virtualisation permet une majorité des applications de s’exécuter, il est à court terme et non une solution à long terme. Les développeurs d’applications doivent modifier leurs applications pour être compatibles avec le programme de logo Windows Server 2012 dès que possible, plutôt que de compter sur la virtualisation de fichiers, dossiers et du Registre.

La virtualisation n’est pas une possibilité dans les scénarios suivants :

1.  La virtualisation ne s’applique pas aux applications qui sont élevées et s’exécuter avec un jeton d’accès administratif complet.

2.  La virtualisation ne prend en charge que les applications 32 bits. Les applications 64 bits avec élévation de privilèges non simplement recevoir un message accès refusé lorsqu’ils tentent d’acquérir un descripteur (un identificateur unique) à un objet de Windows. Applications Windows 64 bits natives sont requises pour être compatible avec UAC et d’écrire des données dans les emplacements corrects.

3.  La virtualisation est désactivée pour une application si l’application inclut un manifeste d’application avec un attribut de niveau d’exécution demandé.

### <a name="request-execution-levels"></a>Niveaux d’exécution de requête
Un manifeste d’application est un fichier XML qui décrit et identifie les assembly côte à côte, privés et partagés, avec lesquels une application doit établir une liaison au moment de l’exécution. Dans Windows Server 2012, le manifeste d’application comprend des entrées pour des raisons de compatibilité des applications UAC. Les applications administratives qui incluent une entrée dans le manifeste d’application invitent l’utilisateur à l’autorisation d’accès du jeton d’accès de l’utilisateur. Même s’il manque une entrée dans le manifeste d’application, la plupart des applications d’administration peuvent s’exécuter sans modification grâce aux correctifs de compatibilité des applications. Correctifs de compatibilité des applications sont des entrées de base de données qui permettent aux applications qui ne sont pas compatibles avec UAC de fonctionner correctement avec Windows Server 2012.

Toutes les applications conformes à UAC doivent avoir un niveau d’exécution demandé ajouté au manifeste d’application. Si l’application requiert un accès administratif au système, puis marquer l’application avec un niveau d’exécution demandé de « requiert un administrateur » garantit que le système identifie ce programme comme une application administrative et exécute le étapes d’élévation nécessaires. Les niveaux d’exécution demandés indiquent les privilèges requis pour une application.

### <a name="installer-detection-technology"></a>Technologie de détection de programme d’installation
Programmes d’installation sont des applications conçues pour déployer des logiciels. La plupart des programmes d’installation écrivent dans les répertoires système et les clés du Registre. Ces emplacements protégés du système sont généralement accessible en écriture uniquement par un administrateur dans la technologie de détection de programme d’installation, ce qui signifie que les utilisateurs standard n’ont pas d’accès suffisants pour installer des programmes. Windows Server 2012 détecte de manière heuristique les programmes d’installation et les informations d’identification d’administrateur de requêtes ou l’approbation de l’administrateur pour s’exécuter avec des privilèges d’accès. Windows Server 2012 détecte également de manière heuristique les mises à jour et des programmes qui désinstallent les applications. L’un des objectifs conceptuels du contrôle de compte d’utilisateur est d’empêcher l’exécution des installations à l’insu ou sans le consentement de l’utilisateur, car les programmes d’installation écrivent dans des zones protégées du système de fichiers et du Registre.

Détection de programme d’installation s’applique uniquement aux :

-   fichiers exécutables 32 bits.

-   Applications sans attribut de niveau d’exécution demandé.

-   Processus interactifs qui s’exécutent en tant qu’utilisateur standard avec l’UAC activé.

Avant qu’un processus 32 bits est créé, les attributs suivants sont vérifiés pour déterminer s’il s’agit d’un programme d’installation :

-   Le nom de fichier inclut des mots clés tels que « install », « setup » ou « update ».

-   Champs de ressources de contrôle de version contiennent les mots clés suivants : Fournisseur, nom de la société, nom du produit, Description du fichier, nom de fichier d’origine, nom interne et nom de l’exportation.

-   Les mots clés contenus dans le manifeste côte à côte sont incorporés dans le fichier exécutable.

-   Mots clés dans les entrées StringTable spécifiques liées dans le fichier exécutable.

-   Attributs de clé dans les données de script de ressources sont liées dans le fichier exécutable.

-   Il existe des séquences d’octets ciblées au sein du fichier exécutable.

> [!NOTE]
> Les mots clés et les séquences d’octets sont dérivés des caractéristiques communes observées dans de nombreuses technologies de programmes d’installation.

> [!NOTE]
> Le contrôle de compte d’utilisateur : Détecter les installations d’applications et demander l’élévation doit être activée pour la détection de programme d’installation puisse détecter les programmes d’installation. Ce paramètre est activé par défaut et peut être configuré localement à l’aide du composant logiciel enfichable Stratégie de sécurité locale (Secpol.msc), ou bien configuré pour le domaine OU ou des groupes spécifiques à l’aide de la stratégie de groupe (Gpedit.msc).


