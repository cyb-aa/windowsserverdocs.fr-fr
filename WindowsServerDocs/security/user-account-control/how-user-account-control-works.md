---
title: Fonctionnement du contrôle de compte d’utilisateur
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f12a690c06a37a6e1c01674bcdb96d0a9010a245
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403335"
---
# <a name="how-user-account-control-works"></a>Fonctionnement du contrôle de compte d’utilisateur

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Le Contrôle de compte d’utilisateur (UAC) contribue à éviter que des programmes malveillants n’endommagent un ordinateur tout en permettant aux organisations de déployer un bureau mieux administré. Avec UAC, les applications et les tâches sont toujours exécutées dans le contexte de sécurité d’un compte non administrateur, sauf si un administrateur autorise spécifiquement l’accès administrateur au système. Le contrôle de compte d’utilisateur peut bloquer l’installation automatique d’applications non autorisées et empêcher les modifications accidentelles des paramètres système.

## <a name="uac-process-and-interactions"></a>Processus et interactions UAC
Chaque application qui requiert le jeton d’accès administrateur doit inviter l’administrateur à donner son consentement. La seule exception à cette règle est la relation qui existe entre les processus parent et enfant. Les processus enfants héritent du jeton d’accès utilisateur du processus parent. Les deux processus parent et enfant doivent cependant avoir le même niveau d’intégrité. Windows Server 2012 protège les processus en marquant leurs niveaux d’intégrité. Les niveaux d’intégrité sont des mesures de la confiance. Une application d’intégrité « élevée » exécute des tâches qui modifient les données système, telles qu’une application de partitionnement de disque, tandis qu’une application d’intégrité « faible » exécute des tâches qui pourraient compromettre le système d’exploitation, par exemple un navigateur Web. Les applications avec des niveaux d’intégrité inférieurs ne peuvent pas modifier les données dans les applications avec des niveaux d’intégrité plus élevés. Lorsqu’un utilisateur standard tente d’exécuter une application qui requiert un jeton d’accès administrateur, le contrôle de compte d’utilisateur exige que l’utilisateur fournisse des informations d’identification d’administrateur valides.

Pour mieux comprendre le fonctionnement de ce processus, il est important de passer en revue les détails du processus d’ouverture de session Windows Server 2012.

### <a name="windows-server-2012-logon-process"></a>Processus d’ouverture de session Windows Server 2012
L’illustration suivante montre comment le processus d’ouverture de session d’un administrateur diffère du processus d’ouverture de session pour un utilisateur standard.

![Illustration montrant comment le processus d’ouverture de session d’un administrateur diffère du processus d’ouverture de session pour un utilisateur standard](../media/How-User-Account-Control-Works/UACWindowsLogonProcess.gif)

Par défaut, les utilisateurs standard et les administrateurs accèdent aux ressources et exécutent des applications dans le contexte de sécurité des utilisateurs standard. Lorsqu’un utilisateur ouvre une session sur un ordinateur, le système crée un jeton d’accès pour cet utilisateur. Ce jeton d’accès contient des informations sur le niveau d’accès accordé à l’utilisateur, notamment des identificateurs de sécurité et des privilèges Windows spécifiques.

Lorsqu’un administrateur ouvre une session, deux jetons d’accès sont créés pour l’utilisateur : un jeton d’accès utilisateur standard et un jeton d’accès administrateur. Le jeton d’accès utilisateur standard contient les mêmes informations spécifiques à l’utilisateur que le jeton d’accès administrateur, mais les privilèges d’administration de Windows et les SID sont supprimés. Le jeton d’accès utilisateur standard est utilisé pour démarrer les applications qui n’effectuent pas de tâches d’administration (applications utilisateur standard). Le jeton d’accès utilisateur standard est ensuite utilisé pour afficher le bureau (Explorer. exe). Explorer.exe est le processus parent duquel tous les autres processus lancés par l’utilisateur héritent leur jeton d’accès. Par conséquent, toutes les applications s’exécutent avec des privilèges d’utilisateur standard, sauf si un utilisateur fournit son consentement ou des informations d’identification pour utiliser un jeton d’accès administrateur total dans une application.

Un utilisateur qui est membre du groupe administrateurs peut ouvrir une session, parcourir le Web et lire le courrier électronique tout en utilisant un jeton d’accès utilisateur standard. Lorsque l’administrateur doit effectuer une tâche qui requiert le jeton d’accès administrateur, Windows Server 2012 invite automatiquement l’utilisateur à approuver. Cette demande s’appelle une invite d’élévation. Vous pouvez configurer son comportement à l’aide du composant logiciel enfichable Stratégie de sécurité locale (Secpol.msc) ou de la stratégie de groupe.

> [!NOTE]
> Le terme « élever » est utilisé pour faire référence au processus dans Windows Server 2012 qui invite l’utilisateur à donner son consentement ou à fournir des informations d’identification pour utiliser un jeton d’accès administrateur complet.

### <a name="the-uac-user-experience"></a>Expérience utilisateur UAC
Lorsque le contrôle de compte d’utilisateur est activé, l’expérience utilisateur pour les utilisateurs standard est différente de celle des administrateurs en mode d’approbation administrateur. La méthode recommandée et la plus sécurisée pour exécuter Windows Server 2012 consiste à faire de votre compte d’utilisateur principal un compte d’utilisateur standard. L’exécution en tant qu’utilisateur standard contribue à optimiser la sécurité d’un environnement géré. Avec le composant d’élévation UAC intégré, les utilisateurs standard peuvent facilement effectuer une tâche administrative en entrant des informations d’identification valides pour un compte d’administrateur local. Le composant d’élévation UAC intégré par défaut pour les utilisateurs standard est l’invite d’informations d’identification.

L’alternative à l’exécution en tant qu’utilisateur standard consiste à exécuter en tant qu’administrateur en mode d’approbation administrateur. Avec le composant d’élévation UAC intégré, les membres du groupe Administrateurs local peuvent facilement effectuer une tâche administrative en fournissant une approbation. Le composant d’élévation UAC intégré par défaut pour un compte administrateur en mode d’approbation administrateur est appelé invite de consentement. Le comportement de l’invite d’élévation UAC peut être configuré à l’aide du composant logiciel enfichable Stratégie de sécurité locale (secpol. msc) ou stratégie de groupe.

**Les invites de consentement et d’informations d’identification**

Lorsque le contrôle de compte d’utilisateur est activé, Windows Server 2012 demande le consentement ou invite à entrer les informations d’identification d’un compte d’administrateur local valide avant de démarrer un programme ou une tâche qui nécessite un jeton d’accès administrateur complet. Cette invite garantit qu’aucun logiciel malveillant ne peut être installé en mode silencieux.

**Demande de consentement**

L’invite de consentement est présentée lorsqu’un utilisateur tente d’effectuer une tâche qui requiert le jeton d’accès administrateur d’un utilisateur. Voici une capture d’écran de l’invite de consentement du contrôle de compte d’utilisateur.

![Capture d’écran de l’invite de consentement du contrôle de compte d’utilisateur](../media/How-User-Account-Control-Works/UACConsentPrompt.gif)

**Demande d’informations d’identification**

La demande d’informations d’identification est présentée lorsqu’un utilisateur standard tente d’effectuer une tâche nécessitant un jeton d’accès administrateur. Le comportement des invites par défaut d’utilisateur standard peut être configuré à l’aide du composant logiciel enfichable Stratégie de sécurité locale (Secpol.msc) ou de la stratégie de groupe. Les administrateurs peuvent également être amenés à fournir leurs informations d’identification en définissant le paramètre de stratégie contrôle de compte d’utilisateur : comportement de l’invite d’élévation pour les administrateurs en mode d’approbation administrateur pour demander les informations d’identification.

La capture d’écran suivante est un exemple de l’invite d’informations d’identification UAC.

![Capture d’écran montrant un exemple d’invite d’informations d’identification UAC](../media/How-User-Account-Control-Works/UACCredentialPrompt.gif)

**Invites d’élévation UAC**

Les invites d’élévation du contrôle de compte d’utilisateur sont codées avec des couleurs différentes selon l’application, ce qui permet d’identifier immédiatement le risque de sécurité potentiel de l’application. Lorsqu’une application tente de s’exécuter avec le jeton d’accès complet d’un administrateur, Windows Server 2012 analyse tout d’abord le fichier exécutable pour déterminer son serveur de publication. Les applications sont d’abord divisées en trois catégories en fonction de l’éditeur du fichier exécutable : Windows Server 2012, Publisher vérifié (signé) et l’éditeur non vérifié (non signé). Le diagramme suivant illustre la façon dont Windows Server 2012 détermine l’invite d’élévation de couleur à présenter à l’utilisateur.

Le codage des couleurs de l’invite d’élévation est le suivant :

-   Arrière-plan rouge avec une icône de bouclier rouge : l’application est bloquée par la stratégie de groupe ou émane d’un éditeur bloqué.

-   Arrière-plan bleu avec une icône de bouclier bleue et jaune : l’application est une application administrative Windows Server 2012, telle qu’un élément du panneau de configuration.

-   Arrière-plan bleu avec une icône de bouclier bleue : l’application est signée à l’aide de la technologie Authenticode et elle est approuvée par l’ordinateur local.

-   Arrière-plan jaune avec une icône de bouclier jaune : l’application est signée ou non signée mais elle n’est pas encore approuvée par l’ordinateur local.

**Icône de bouclier**

Certains éléments du Panneau de configuration, tels que **Propriétés de dates et d’heures**, contiennent une combinaison d’opérations d’utilisateur standard et d’administrateur. Les utilisateurs standard peuvent afficher l’horloge et changer le fuseau horaire, mais un jeton d’accès administrateur complet est requis pour modifier l’heure système locale. L’image suivante est une capture d’écran de l’élément du Panneau de configuration **Propriétés de dates et d’heures**.

![Capture d’écran montrant les * * propriétés de date et d’heure * * élément du panneau de configuration](../media/How-User-Account-Control-Works/UACShieldIcon.gif)

L’icône de bouclier sur le bouton **Changer la date et l’heure** indique que le processus requiert un jeton d’accès administrateur complet et qu’il va afficher une invite d’élévation du contrôle de compte d’utilisateur.

**Sécurisation de l’invite d’élévation**

La sécurisation du processus d’élévation est renforcée car l’invite est envoyée sur le bureau sécurisé. Les invites de consentement et d’informations d’identification sont affichées sur le Bureau sécurisé par défaut dans Windows Server 2012. Seuls les processus Windows peuvent accéder au Bureau sécurisé. Pour obtenir des niveaux de sécurité plus élevés, il est recommandé de laisser le paramètre de stratégie **Contrôle de compte d’utilisateur : passer au Bureau sécurisé lors d’une demande d’élévation** activé.

Lorsqu’un fichier exécutable demande l’élévation, le bureau interactif, également appelé bureau de l’utilisateur, passe au bureau sécurisé. Le Bureau sécurisé estompe le Bureau de l’utilisateur et affiche une invite d’élévation à laquelle il faut répondre avant de continuer. Quand l’utilisateur clique sur Oui ou non, le bureau bascule sur le Bureau de l’utilisateur.

Les programmes malveillants peuvent présenter une imitation du Bureau sécurisé, mais lorsque le paramètre de stratégie contrôle de compte d’utilisateur : comportement de l’invite d’élévation pour les administrateurs en mode d’approbation administrateur est défini sur demander le consentement, le programme malveillant n’obtient pas d’élévation si l’utilisateur clique sur Oui. sur l’imitation. Si le paramètre de stratégie est défini sur demander des informations d’identification, les logiciels malveillants imitant l’invite d’informations d’identification peuvent être en mesure de rassembler les informations d’identification de l’utilisateur. Toutefois, le logiciel malveillant n’a pas de privilège élevé et le système dispose d’autres protections qui atténuent le contrôle de l’interface utilisateur par les programmes malveillants, même avec un mot de passe récolté.

Si les logiciels malveillants peuvent présenter une imitation du bureau sécurisé, ce problème ne peut pas se produire sauf si un utilisateur a précédemment installé les logiciels malveillants sur l’ordinateur. Comme les processus nécessitant un jeton d’accès administrateur ne peuvent pas s’installer en mode silencieux lorsque le contrôle de compte d’utilisateur est activé, l’utilisateur doit explicitement fournir un consentement en cliquant sur **Oui** ou en fournissant des informations d’identification d’administrateur. Le comportement spécifique de l’invite d’élévation UAC dépend de stratégie de groupe.

## <a name="uac-architecture"></a>Architecture UAC
Le diagramme suivant montre l’architecture du contrôle de compte d’utilisateur en détail.

![Diagramme détaillant l’architecture UAC](../media/How-User-Account-Control-Works/UACArchitecture.gif)

Pour mieux comprendre chaque composant, consultez le tableau ci-dessous :

|Component|Description|
|-------|--------|
|**Utilisateur**||
|L’utilisateur effectue une opération nécessitant un privilège|Si l’opération modifie le système de fichiers ou le Registre, la virtualisation est appelée. Toutes les autres opérations appellent ShellExecute.|
|ShellExecute|ShellExecute appelle CreateProcess. ShellExecute recherche l’erreur ERROR_ELEVATION_REQUIRED dans CreateProcess. S’il reçoit l’erreur, ShellExecute appelle le service Informations sur l’application pour tenter d’effectuer la tâche demandée à l’aide de l’invite avec élévation de privilèges.|
|CreateProcess|Si l’application nécessite une élévation, CreateProcess rejette l’appel avec ERROR_ELEVATION_REQUIRED.|
|**Requise**||
|Service d’informations d’application|Service système qui démarre des applications nécessitant un ou plusieurs privilèges ou droits d’utilisateur élevés pour s’exécuter (des tâches d’administration locales, par exemple), et des applications nécessitant des niveaux d’intégrité plus élevés. Le service d’informations d’application permet de démarrer de telles applications en créant un nouveau processus pour l’application avec le jeton d’accès complet d’un utilisateur administratif lorsque l’élévation est requise et (selon stratégie de groupe) le consentement est donné à l’utilisateur.|
|Élévation d’une installation ActiveX|Si ActiveX n’est pas installé, le système vérifie le niveau de curseur UAC. Si ActiveX est installé, le paramètre de stratégie de groupe **Contrôle de compte d’utilisateur : passer au Bureau sécurisé lors d’une demande d’élévation** est activé.|
|Vérifier le niveau du curseur UAC|Le contrôle de compte d’utilisateur a désormais un choix de quatre niveaux de notification et un curseur à utiliser pour sélectionner le niveau de notification :<br /><br /><ul><li>Élevé<br /><br />    Si le curseur est défini sur **Toujours m’avertir**, le système vérifie si le Bureau sécurisé est activé.</li><li>Moyen<br /><br />    Si le curseur est défini sur **Par défaut - M’avertir uniquement quand des programmes tentent d’apporter des modifications à mon ordinateur**, le paramètre de stratégie **Contrôle de compte d’utilisateur : élever uniquement les exécutables signés et validés** est activé :<br /><br /><ul><li>Si le paramètre de stratégie est activé, la validation du chemin d’accès de certification de l’infrastructure à clé publique (PKI) est appliquée pour un fichier exécutable donné avant qu’il soit autorisé à s’exécuter.</li><li>Si le paramètre de stratégie n’est pas activé, la validation du chemin de certification PKI n’est pas appliquée avant qu’un fichier exécutable donné soit autorisé à s’exécuter. Le paramètre de stratégie de groupe **Contrôle de compte d’utilisateur : passer au Bureau sécurisé lors d’une demande d’élévation** est activé.</li></ul></li><li>Faible<br /><br />    Si le curseur est défini sur **M’avertir uniquement quand des programmes tentent d’apporter des modifications à mon ordinateur (ne pas estomper mon Bureau).** , le processus CreateProcess est appelé.</li><li>Ne jamais notifier<br /><br />    Si le curseur est défini sur **ne jamais m’avertir quand**, l’invite du contrôle de compte d’utilisateur ne vous avertira pas quand un programme tente d’installer ou d’effectuer une modification sur l’ordinateur. **Important :**     Ce paramètre n’est pas recommandé. Ce paramètre est le même que lorsque vous définissez le paramètre **de stratégie contrôle de compte d’utilisateur : comportement de l’invite d’élévation pour les administrateurs en mode d’approbation Administrateur** sur **élever sans invite**.</li></ul>|
|Bureau sécurisé activé|Le paramètre de stratégie **Contrôle de compte d’utilisateur : passer au Bureau sécurisé lors d’une demande d’élévation** est activé :<br /><br />-Si le Bureau sécurisé est activé, toutes les demandes d’élévation sont dirigées vers le Bureau sécurisé, quels que soient les paramètres de stratégie de comportement d’invite pour les administrateurs et les utilisateurs standard.<br />-Si le Bureau sécurisé n’est pas activé, toutes les demandes d’élévation sont envoyées sur le Bureau de l’utilisateur interactif et les paramètres par utilisateur pour les administrateurs et les utilisateurs standard sont utilisés.|
|CreateProcess|CreateProcess appelle la détection AppCompat, fusion et installeur pour déterminer si l’application requiert une élévation. Le fichier exécutable est alors examiné pour déterminer son niveau d’exécution demandé, qui est stocké dans le manifeste d’application du fichier exécutable. CreateProcess échoue si le niveau d’exécution demandé spécifié dans le manifeste ne correspond pas au jeton d’accès et s’il renvoie une erreur (ERROR_ELEVATION_REQUIRED) à ShellExecute. |
|AppCompat|La base de données AppCompat stocke les informations dans les entrées de correctif de compatibilité des applications pour une application.|
|Fusion|La base de données fusion stocke des informations à partir de manifestes d’application qui décrivent les applications. Le schéma du manifeste est mis à jour pour ajouter un nouveau champ de niveau d’exécution demandé.|
|Détection du programme d’installation|La détection du programme d’installation détecte les fichiers exécutables d’installation, ce qui permet d’empêcher l’exécution des installations sans la connaissance et le consentement de l’utilisateur.|
|**Noyau**||
|Virtualisation|La technologie de virtualisation garantit que les applications non conformes ne parviennent pas à s’exécuter silencieusement ou échouent de façon à ce que la cause ne puisse pas être déterminée. Le contrôle de compte d’utilisateur fournit également la virtualisation et la journalisation des fichiers et du Registre pour les applications qui écrivent dans des zones protégées.|
|Système de fichiers et Registre|La virtualisation des fichiers et du Registre par utilisateur redirige le registre par ordinateur et les demandes d’écriture de fichier vers des emplacements par utilisateur équivalents. Les demandes de lecture sont d’abord redirigées vers l’emplacement par utilisateur virtualisé, puis vers l’emplacement par ordinateur.|

Une modification a été apportée sur le contrôle de compte d’utilisateur Windows Server 2012 des versions précédentes de Windows. Le nouveau curseur ne désactive jamais le contrôle de compte d’utilisateur complètement. Le nouveau paramètre :

-   Assurez-vous que le service UAC est en cours d’exécution.

-   Entraîne l’approbation automatique de toutes les demandes d’élévation initiées par les administrateurs sans afficher d’invite UAC.

-   Refuser automatiquement toutes les demandes d’élévation pour les utilisateurs standard.

> [!IMPORTANT]
> Pour désactiver complètement l’UAC, vous devez désactiver la stratégie **contrôle de compte d’utilisateur : exécuter tous les administrateurs en mode d’approbation Administrateur**.

> [!WARNING]
> Les applications personnalisées ne fonctionnent pas sur Windows Server 2012 lorsque le contrôle de compte d’utilisateur est désactivé.

### <a name="virtualization"></a>Virtualisation
Étant donné que les administrateurs système dans les entreprises tentent de sécuriser les systèmes, de nombreuses applications métier sont conçues pour n’utiliser qu’un jeton d’accès utilisateur standard. Par conséquent, les administrateurs informatiques n’ont pas besoin de remplacer la majorité des applications lors de l’exécution de Windows Server 2012 avec le contrôle de compte d’utilisateur activé.

 Windows Server 2012 comprend une technologie de virtualisation des fichiers et du Registre pour les applications qui ne sont pas compatibles avec le contrôle de compte d’utilisateur et qui requièrent un jeton d’accès administrateur pour s’exécuter correctement. La virtualisation garantit que même les applications qui ne sont pas compatibles avec le contrôle de compte d’utilisateur sont compatibles avec Windows Server 2012. Lorsqu’une application administrative qui n’est pas compatible avec le contrôle de compte d’utilisateur tente d’écrire dans un répertoire protégé, tel que Program Files, le contrôle de compte d’utilisateur donne à l’application sa propre vue virtualisée de la ressource qu’elle tente de modifier. La copie virtualisée est gérée dans le profil utilisateur. Cette stratégie crée une copie distincte du fichier virtualisé pour chaque utilisateur qui exécute l’application non conforme.

La plupart des tâches d’application fonctionnent correctement à l’aide des fonctionnalités de virtualisation. Bien que la virtualisation autorise l’exécution de la majorité des applications, il s’agit d’un correctif à long terme et non d’une solution à long terme. Les développeurs d’applications doivent modifier leurs applications pour qu’elles soient conformes au programme de logo Windows Server 2012 le plus rapidement possible, au lieu de s’appuyer sur la virtualisation des fichiers, des dossiers et du Registre.

La virtualisation n’est pas une possibilité dans les scénarios suivants :

1.  La virtualisation ne s’applique pas aux applications qui sont élevées et s’exécutent avec un jeton d’accès administratif complet.

2.  La virtualisation ne prend en charge que les applications 32 bits. Les applications 64 bits non élevées reçoivent simplement un message d’accès refusé lorsqu’ils tentent d’acquérir un handle (un identificateur unique) sur un objet Windows. Les applications natives Windows 64 bits doivent être compatibles avec le contrôle de compte d’utilisateur et écrire des données dans les emplacements appropriés.

3.  La virtualisation est désactivée pour une application si celle-ci comprend un manifeste d’application avec un attribut de niveau d’exécution demandé.

### <a name="request-execution-levels"></a>Niveaux d’exécution de la demande
Un manifeste d’application est un fichier XML qui décrit et identifie les assembly côte à côte, privés et partagés, avec lesquels une application doit établir une liaison au moment de l’exécution. Dans Windows Server 2012, le manifeste d’application comprend des entrées pour la compatibilité des applications UAC. Les applications administratives qui incluent une entrée dans le manifeste de l’application demandent à l’utilisateur l’autorisation d’accéder au jeton d’accès de l’utilisateur. Même s’il manque une entrée dans le manifeste d’application, la plupart des applications d’administration peuvent s’exécuter sans modification grâce aux correctifs de compatibilité des applications. Les correctifs de compatibilité des applications sont des entrées de base de données qui permettent aux applications qui ne sont pas compatibles avec le contrôle de compte d’utilisateur de fonctionner correctement avec Windows Server 2012.

Toutes les applications conformes au contrôle de compte d’utilisateur doivent avoir un niveau d’exécution demandé ajouté au manifeste de l’application. Si l’application nécessite un accès administratif au système, le fait de marquer l’application avec un niveau d’exécution demandé « exiger un administrateur » garantit que le système identifie ce programme en tant qu’application administrative et qu’il effectue l’opération étapes d’élévation nécessaires. Les niveaux d’exécution demandés indiquent les privilèges requis pour une application.

### <a name="installer-detection-technology"></a>Technologie de détection du programme d’installation
Les programmes d’installation sont des applications conçues pour déployer des logiciels. La plupart des programmes d’installation écrivent dans les répertoires système et les clés du Registre. Ces emplacements système protégés sont généralement accessibles en écriture uniquement par un administrateur dans la technologie de détection du programme d’installation, ce qui signifie que les utilisateurs standard ne disposent pas d’un accès suffisant pour installer les programmes. Windows Server 2012 détecte de manière heuristique les programmes d’installation et demande les informations d’identification de l’administrateur ou l’approbation de l’utilisateur administrateur pour qu’il s’exécute avec des privilèges d’accès. Windows Server 2012 détecte également de manière heuristique les mises à jour et les programmes de désinstallation des applications. L’un des objectifs conceptuels du contrôle de compte d’utilisateur est d’empêcher l’exécution des installations à l’insu ou sans le consentement de l’utilisateur, car les programmes d’installation écrivent dans des zones protégées du système de fichiers et du Registre.

La détection du programme d’installation s’applique uniquement aux éléments suivants :

-   fichiers exécutables 32 bits.

-   Applications sans attribut de niveau d’exécution demandé.

-   Processus interactifs s’exécutant en tant qu’utilisateur standard avec contrôle de compte d’utilisateur activé.

Avant la création d’un processus 32 bits, les attributs suivants sont vérifiés pour déterminer s’il s’agit d’un programme d’installation :

-   Le nom de fichier comprend des mots clés tels que « installer », « installation » ou « mise à jour ».

-   Les champs de gestion des versions des ressources contiennent les mots clés suivants : Fournisseur, Nom de la société, Nom du produit, Description du fichier, Nom de fichier initial, Nom interne et Nom d’exportation.

-   Les mots clés contenus dans le manifeste côte à côte sont incorporés dans le fichier exécutable.

-   Les mots clés dans des entrées StringTable spécifiques sont liés dans le fichier exécutable.

-   Les attributs de clé dans les données de script de ressources sont liés dans le fichier exécutable.

-   Il existe des séquences d’octets ciblées au sein du fichier exécutable.

> [!NOTE]
> Les mots clés et les séquences d’octets sont dérivés des caractéristiques communes observées dans de nombreuses technologies de programmes d’installation.

> [!NOTE]
> Le paramètre de stratégie contrôle de compte d’utilisateur : détecter les installations d’applications et demander l’élévation doit être activé pour que la détection du programme d’installation détecte les programmes d’installation. Ce paramètre est activé par défaut et peut être configuré localement à l’aide du composant logiciel enfichable Stratégie de sécurité locale (Secpol.msc), ou bien configuré pour le domaine OU ou des groupes spécifiques à l’aide de la stratégie de groupe (Gpedit.msc).


