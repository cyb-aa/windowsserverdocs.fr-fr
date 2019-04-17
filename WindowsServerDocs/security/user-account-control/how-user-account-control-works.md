---
title: "Fonctionnement du contrôle de compte d’utilisateur"
description: "Sécurité de Windows Server"
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
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="how-user-account-control-works"></a>Fonctionnement du contrôle de compte d’utilisateur

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Contrôle de compte d’utilisateur (UAC) permet d’empêcher les programmes malveillants (également appelés malware) d’endommager un ordinateur et aide les organisations à déployer un bureau mieux administré. Avec UAC, les applications et les tâches sont toujours exécutent dans le contexte de sécurité d’un compte non administrateur, sauf si un administrateur autorise expressément un accès de niveau administrateur au système. UAC peut bloquer l’installation automatique des applications non autorisées et empêcher les modifications accidentelles de paramètres système.

## <a name="uac-process-and-interactions"></a>Interactions et processus UAC
Chaque application qui requiert le jeton d’accès administrateur doit demander à l’administrateur de consentement. La seule exception est la relation qui existe entre le parent et enfant de processus. Les processus enfants héritent le jeton d’accès utilisateur à partir du processus parent. Processus parent et enfant, cependant, posséder le même niveau d’intégrité. Windows Server 2012 protège les processus en marquant leurs niveaux d’intégrité. Niveaux d’intégrité sont des mesures de confiance. Une application d’intégrité du «haute» est une qui effectue des tâches qui modifient les données système, par exemple, une application, de partitionnement pendant qu’une application de l’intégrité de «faible» est celui qui exécute des tâches qui peuvent compromettre le système d’exploitation, par exemple, un navigateur Web. Les applications avec des niveaux inférieurs de l’intégrité ne peut pas modifier les données dans les applications avec des niveaux d’intégrité plus élevés. Lorsqu’un utilisateur standard tente d’exécuter une application qui nécessite un jeton d’accès administrateur, l’UAC exige que l’utilisateur fournisse des informations d’identification administrateur valides.

Afin de mieux comprendre comment ce processus se produit qu’il est important de consulter les détails du processus d’ouverture de session de Windows Server 2012.

### <a name="windows-server-2012-logon-process"></a>Processus d’ouverture de session Windows Server 2012
L’illustration suivante montre comment le processus d’ouverture de session pour un administrateur diffère du processus d’ouverture de session pour un utilisateur standard.

![Illustration montrant comment le processus d’ouverture de session pour un administrateur diffère du processus d’ouverture de session pour un utilisateur standard](../media/How-User-Account-Control-Works/UACWindowsLogonProcess.gif)

Par défaut, les administrateurs et les utilisateurs standard accèdent aux ressources et exécutent des applications dans le contexte de sécurité des utilisateurs standard. Lorsqu’un utilisateur ouvre une session sur un ordinateur, le système crée un jeton d’accès pour l’utilisateur. Le jeton d’accès contient des informations sur le niveau d’accès accordé à l’utilisateur, notamment des identificateurs de sécurité (SID) et des privilèges Windows.

Lorsqu’un administrateur ouvre une session, deux jetons d’accès sont créés pour l’utilisateur: un jeton d’accès utilisateur standard et un jeton d’accès administrateur. Le jeton d’accès utilisateur standard contient les mêmes informations spécifiques à l’utilisateur en tant que le jeton d’accès administrateur, mais les privilèges d’administration Windows et les SID sont supprimés. Le jeton d’accès utilisateur standard est utilisé pour démarrer des applications qui n’effectuent pas les tâches d’administration (applications utilisateur standard). Le jeton d’accès utilisateur standard est ensuite utilisé pour afficher le bureau (Explorer.exe). Explorer.exe est le processus parent duquel tous les autres processus lancés par l’utilisateur héritent leur jeton d’accès. Par conséquent, toutes les applications exécutées en tant qu’utilisateur standard, sauf si un utilisateur fournit son consentement ou des informations d’identification pour approuver une application pour utiliser un jeton d’accès administrateur total.

Un utilisateur qui est membre du groupe Administrateurs peut ouvrir une session, naviguer sur le Web et lire le courrier électronique lors de l’utilisation d’un jeton d’accès utilisateur standard. Lorsque l’administrateur doit effectuer une tâche qui requiert le jeton d’accès administrateur, Windows Server 2012 automatiquement invite l’utilisateur pour approbation. Cette demande s’appelle une invite d’élévation, et son comportement peut être configuré à l’aide du composant logiciel enfichable Stratégie de sécurité locale (Secpol.msc) ou la stratégie de groupe.

> [!NOTE]
> Le terme «élever» est utilisé pour faire référence au processus dans Windows Server 2012 qui invite l’utilisateur à fournir son consentement ou des informations d’identification pour utiliser un jeton d’accès administrateur complet.

### <a name="the-uac-user-experience"></a>L’expérience utilisateur UAC
Lorsque l’UAC est activé, l’expérience utilisateur pour les utilisateurs standard est différente de celle des administrateurs en Mode d’approbation administrateur. La méthode recommandée et plus sécurisée de l’exécution de Windows Server 2012 consiste à rendre votre utilisateur principal un compte d’utilisateur standard. Exécution en tant qu’utilisateur standard permet d’optimiser la sécurité d’un environnement géré. Avec le composant d’élévation UAC intégré, les utilisateurs standard peuvent exécuter facilement une tâche d’administration en entrant les informations d’identification valides pour un compte d’administrateur local. La valeur par défaut, le composant d’élévation UAC intégré pour les utilisateurs standard est l’invite d’informations d’identification.

L’alternative à l’exécution en tant qu’utilisateur standard consiste à exécuter en tant qu’administrateur en Mode d’approbation administrateur. Avec le composant d’élévation UAC intégré, les membres du groupe Administrateurs local peuvent facilement effectuer une tâche administrative en fournissant l’approbation. La valeur par défaut, le composant d’élévation UAC intégré pour un compte d’administrateur en Mode d’approbation administrateur est appelé l’invite de consentement. Le comportement d’invite UAC élévation peut être configuré à l’aide du composant logiciel enfichable Stratégie de sécurité locale (Secpol.msc) ou la stratégie de groupe.

**Les invites de consentement et d’informations d’identification**

Avec UAC activé, Windows Server 2012 demande le consentement ou invite à entrer les informations d’identification d’un compte d’administrateur local valide avant de démarrer un programme ou une tâche nécessitant un jeton d’accès administrateur complet. Cette invite garantit qu’aucun logiciel malveillant ne peut être installé en mode silencieux.

**L’invite de consentement**

La demande de consentement est présentée lorsqu’un utilisateur tente d’effectuer une tâche qui nécessite un jeton d’accès d’administration. Voici une capture d’écran de l’invite de consentement UAC.

![Capture d’écran de l’invite de consentement UAC](../media/How-User-Account-Control-Works/UACConsentPrompt.gif)

**L’invite d’informations d’identification**

La demande d’informations d’identification est présentée lorsqu’un utilisateur standard tente d’effectuer une tâche qui nécessite un jeton d’accès d’administration. Ce comportement de l’invite utilisateur standard par défaut peut être configuré à l’aide du composant logiciel enfichable Stratégie de sécurité locale (Secpol.msc) ou la stratégie de groupe. Les administrateurs peuvent également être amenés à fournir leurs informations d’identification en définissant le contrôle de compte d’utilisateur: comportement de l’invite d’élévation pour les administrateurs en définissant la valeur à l’invite d’informations d’identification de stratégie de Mode d’approbation administrateur.

La capture d’écran suivante est un exemple de l’invite d’informations d’identification de compte d’utilisateur.

![Capture d’écran montrant un exemple de l’invite d’informations d’identification de compte d’utilisateur](../media/How-User-Account-Control-Works/UACCredentialPrompt.gif)

**Invites d’élévation UAC**

Les invites d’élévation UAC sont classées par couleur pour être spécifiques à l’application, l’activation de l’identification immédiate de risque de sécurité d’une application. Lorsqu’une application tente d’exécuter avec un jeton d’accès complet d’un administrateur, Windows Server 2012 commence par analyser le fichier exécutable pour déterminer son éditeur. Les applications sont tout d’abord classées en trois catégories basées sur l’Éditeur du fichier exécutable: Windows Server 2012, Éditeur vérifié (signé) et l’éditeur ne sont ne pas vérifiées (non signé). Le diagramme suivant illustre la façon dont Windows Server 2012 détermine quels invite d’élévation couleur à présenter à l’utilisateur.

Le codage invite élévation est la suivante:

-   Arrière-plan rouge avec une icône de bouclier rouge: l’application est bloquée par la stratégie de groupe ou émane d’un éditeur bloqué.

-   Arrière-plan bleu avec une icône de bouclier bleue et dorée: l’application est une application d’administration Windows Server 2012, par exemple, un élément du Panneau de configuration.

-   Arrière-plan bleu avec une icône de bouclier bleue: l’application est signée à l’aide de Authenticode et est approuvée par l’ordinateur local.

-   Arrière-plan jaune avec une icône de bouclier jaune: l’application est signée ou non signée mais n’est ne pas encore approuvé par l’ordinateur local.

**Icône de bouclier**

Les éléments de certains le panneau de configuration, tel que **propriétés de Date et heure**, contiennent une combinaison de l’administrateur et les opérations de l’utilisateur standard. Les utilisateurs standards peuvent afficher l’horloge et changer le fuseau horaire, mais un jeton d’accès administrateur complet est requis pour modifier l’heure système locale. Voici une capture d’écran de la **propriétés de Date et heure** élément du Panneau de configuration.

![Affichage de capture d’écran du ** Date et heure propriétés ** le panneau de configuration](../media/How-User-Account-Control-Works/UACShieldIcon.gif)

L’icône de bouclier sur le **modifier la date et l’heure** bouton indique que le processus requiert un jeton d’accès administrateur complet et affiche une invite d’élévation UAC.

**Sécurisation de l’invite d’élévation**

Le processus d’élévation est davantage sécurisé en dirigeant l’invite vers le bureau sécurisé. Les invites de consentement et d’informations d’identification sont affichées sur le bureau sécurisé par défaut dans Windows Server 2012. Seuls les processus Windows peuvent accéder au bureau sécurisé. Pour obtenir des niveaux plus élevés de sécurité, nous vous recommandons de conserver le **contrôle de compte d’utilisateur: passer au bureau sécurisé lors d’une demande d’élévation** paramètre de stratégie est activé.

Lorsqu’un fichier exécutable demande l’élévation, le bureau interactif, également appelé bureau de l’utilisateur, celle-ci est passé au bureau sécurisé. Le bureau sécurisé estompe bureau de l’utilisateur et affiche une invite d’élévation doit obtenir une réponse avant de continuer. Lorsque l’utilisateur clique sur Oui ou non, le bureau revient au bureau de l’utilisateur.

Les programmes malveillants peuvent présenter une imitation du bureau sécurisé, mais lorsque le contrôle de compte d’utilisateur: comportement de l’invite d’élévation pour les administrateurs en mode d’approbation administrateur est définie à l’invite de consentement, les programmes malveillants n’obtiennent pas d’élévation si l’utilisateur clique sur Oui sur l’imitation. Si le paramètre de stratégie est défini à l’invite d’informations d’identification, les logiciels malveillants imitant l’invite d’informations d’identification peuvent être en mesure de rassembler les informations d’identification de l’utilisateur. Toutefois, le programme malveillant ne peut pas accéder à privilèges élevés et le système dispose d’autres protections le logiciel malveillant de prendre le contrôle de l’interface utilisateur, même avec un mot de passe collecté.

Alors que les programmes malveillants peuvent présenter une imitation du bureau sécurisé, ce problème ne peut pas se produire, sauf si un utilisateur a précédemment installé le logiciel malveillant sur l’ordinateur. Étant donné que les processus nécessitant un jeton d’accès administrateur ne peut pas s’installer en mode silencieux lorsque UAC est activé, l’utilisateur doit explicitement fournir un consentement en cliquant sur **Oui** ou en fournissant des informations d’identification d’administrateur. Le comportement spécifique de l’invite d’élévation UAC dépend de la stratégie de groupe.

## <a name="uac-architecture"></a>Architecture UAC
Le diagramme suivant décrit en détail l’architecture UAC.

![Diagramme décrivant l’architecture UAC](../media/How-User-Account-Control-Works/UACArchitecture.gif)

Pour mieux comprendre chaque composant, consultez le tableau ci-dessous:

|Composant|Description|
|-------|--------|
|**Utilisateur**||
|Utilisateur effectue l’opération nécessitant des privilèges|Si l’opération modifie le système de fichiers ou le Registre, la virtualisation est appelée. Toutes les autres opérations appellent ShellExecute.|
|ShellExecute|ShellExecute appelle CreateProcess. ShellExecute recherche l’erreur ERROR_ELEVATION_REQUIRED dans CreateProcess. S’il reçoit l’erreur, ShellExecute appelle le service d’informations sur l’Application pour tenter d’effectuer la tâche demandée à l’invite de commandes avec élévation de privilèges.|
|CreateProcess|Si l’application nécessite une élévation, CreateProcess refuse l’appel avec ERROR_ELEVATION_REQUIRED.|
|**Système**||
|Service informations d’application|Un service système qui démarre des applications qui nécessitent un ou plusieurs privilèges ou droits d’utilisateurs à exécuter, telle que les tâches d’administration locales et les applications qui requièrent des niveaux d’intégrité plus élevés. Le démarrage du service informations sur l’Application vous aide à de telles applications en créant un nouveau processus pour l’application avec un jeton d’accès complet d’un utilisateur administratif lorsqu’une élévation est requise et (selon la stratégie de groupe) de consentement est fourni par l’utilisateur à le faire.|
|En élevant une installation d’ActiveX|Si ActiveX n’est pas installé, le système vérifie le niveau du curseur UAC. Si ActiveX est installé, le **contrôle de compte d’utilisateur: passer au bureau sécurisé lors d’une demande d’élévation** paramètre de stratégie de groupe est activée.|
|Vérifier le niveau de curseur UAC|Compte d’utilisateur a désormais quatre niveaux de notification d’et un curseur à utiliser pour sélectionner le niveau de notification:<br /><br /><ul><li>Élevé<br /><br />    Si le curseur est défini sur **toujours m’avertir**, le système vérifie si le bureau sécurisé est activé.</li><li>Moyenne<br /><br />    Si le curseur est défini sur **par défaut-avertir uniquement quand des programmes tentent d’apporter des modifications à mon ordinateur**, le **contrôle de compte d’utilisateur: élever uniquement les fichiers exécutables signés et validés** paramètre de stratégie est activé:<br /><br /><ul><li>Si le paramètre de stratégie est activé, la validation de chemin d’accès de certification infrastructure à clé publique (PKI) est appliquée à un fichier exécutable donné avant qu’il est autorisé à exécuter.</li><li>Si le paramètre de stratégie n’est pas activée (par défaut), la validation de chemin d’accès de certification PKI n’est pas appliquée avant qu’un fichier exécutable donné soit autorisé à s’exécuter. Le **contrôle de compte d’utilisateur: passer au bureau sécurisé lors d’une demande d’élévation** paramètre de stratégie de groupe est activée.</li></ul></li><li>Faible<br /><br />    Si le curseur est défini sur **m’avertir uniquement quand des programmes tentent apporter des modifications à mon ordinateur (ne pas estomper bureau)**, le processus CreateProcess est appelé.</li><li>Ne jamais m’avertir<br /><br />    Si le curseur est défini sur **ne jamais m’avertir lorsque**, sera invite UAC ne jamais m’avertir lorsqu’un programme est tente d’installer ou essayez d’effectuer toute modification sur l’ordinateur. **Important:** ce paramètre n’est pas recommandé. Ce paramètre est le même que le **contrôle de compte d’utilisateur: comportement de l’invite d’élévation pour les administrateurs en Mode d’approbation administrateur** paramètre de stratégie **élever sans invite**.</li></ul>|
|Bureau sécurisé est activé|Le **contrôle de compte d’utilisateur: passer au bureau sécurisé lors d’une demande d’élévation** paramètre de stratégie est activé:<br /><br />-Si le bureau sécurisé est activé, toutes les demandes d’élévation passent au bureau sécurisé, indépendamment des paramètres de stratégie de comportement d’invite pour les administrateurs et les utilisateurs standard.<br />-Si le bureau sécurisé n’est pas activé, toutes les demandes d’élévation passent au bureau interactif de l’utilisateur et les paramètres par utilisateur pour les administrateurs et les utilisateurs standard sont utilisés.|
|CreateProcess|CreateProcess appelle AppCompat, Fusion et Installer la détection pour évaluer si l’application nécessite une élévation. Le fichier exécutable est alors examiné pour déterminer son niveau d’exécution demandé, qui est stocké dans le manifeste d’application pour le fichier exécutable. CreateProcess échoue si le niveau d’exécution demandé spécifié dans le manifeste ne correspond pas au jeton d’accès et retourne une erreur (ERROR_ELEVATION_REQUIRED) à ShellExecute. |
|AppCompat|La base de données AppCompat stocke les informations dans les entrées correctif de compatibilité des applications pour une application.|
|Fusion|La base de données de Fusion stocke les informations des manifestes d’application qui décrivent les applications. Le schéma de manifeste est mis à jour pour ajouter un nouveau champ de niveau d’exécution demandé.|
|Détection de programme d’installation|Détection de programme d’installation détecte des fichiers exécutables de programme d’installation, ce qui empêche les installations à partir d’en cours d’exécution sans connaissance et le consentement de l’utilisateur.|
|**Noyau**||
|Virtualisation|La technologie de virtualisation permet de s’assurer que les applications non conformes n’échoueront pas en mode silencieux à exécuter ou échouent de manière que la cause ne peut pas être déterminée. UAC fournit également la virtualisation de fichiers et de Registre et de la journalisation pour les applications qui écrivent dans des zones protégées.|
|Système de fichiers et du Registre|La virtualisation de fichiers et de Registre par utilisateur redirige Registre par ordinateur et les demandes d’écriture de fichiers vers des emplacements par utilisateur équivalente. Demandes de lecture sont redirigées vers l’emplacement par utilisateur virtualisé seconde tout d’abord et à l’emplacement par ordinateur.|

Il existe un changement de compte d’utilisateur Windows Server 2012 à partir de versions précédentes de Windows. Le nouveau curseur jamais désactive UAC complètement. Le nouveau paramètre sera:

-   Maintenir le service de contrôle de compte utilisateur en cours d’exécution.

-   Cause demande d’élévation tous les lancées par les administrateurs pour être acceptée automatiquement sans afficher d’invite un compte d’utilisateur.

-   Refuser automatiquement toutes les demandes d’élévation pour les utilisateurs standard.

> [!IMPORTANT]
> Pour désactiver entièrement UAC, vous devez désactiver la stratégie **contrôle de compte d’utilisateur: exécuter tous les administrateurs en Mode d’approbation administrateur**.

> [!WARNING]
> Applications adaptées ne fonctionnera pas sur Windows Server 2012 lorsque le compte d’utilisateur est désactivé.

### <a name="virtualization"></a>Virtualisation
Étant donné que les administrateurs système dans les environnements d’entreprise tentent de sécuriser les systèmes, de nombreuses applications de (LOB) de métier sont conçues pour utiliser uniquement un jeton d’accès utilisateur standard. Par conséquent, les administrateurs informatiques est inutile de remplacer la majorité des applications lors de l’exécution de Windows Server 2012 avec l’UAC est activé.

 Windows Server 2012 inclut la technologie de virtualisation de fichiers et de Registre pour les applications qui ne sont pas compatibles UAC et qui nécessitent un jeton d’accès administrateur pour s’exécuter correctement. La virtualisation permet de s’assurer que même les applications qui ne sont pas compatibles UAC sont compatibles avec Windows Server 2012. Lorsqu’une application d’administration qui n’est pas compatible UAC tente d’écrire dans un répertoire protégé, tels que les fichiers programme, UAC donne à sa propre vue virtualisée de la ressource qu’il tente de modifier l’application. La copie virtualisée est gérée dans le profil utilisateur. Cette stratégie crée une copie séparée du fichier virtuel pour chaque utilisateur qui exécute l’application non conforme.

La plupart des tâches d’application fonctionnent correctement à l’aide des fonctionnalités de virtualisation. La virtualisation permet à la majorité des applications à s’exécuter, mais il est à court terme et non une solution à long terme. Les développeurs d’applications doivent modifier leurs applications pour être compatibles avec le programme de logo Windows Server 2012 dès que possible, plutôt que d’utiliser la virtualisation de fichiers, dossiers et du Registre.

La virtualisation n’est pas dans l’option dans les scénarios suivants:

1.  La virtualisation ne s’applique pas aux applications qui sont élevées et s’exécuter avec un jeton d’accès administrateur total.

2.  La virtualisation prend en charge les applications 32 bits uniquement. Les applications 64 bits non élevés simplement recevoir un message accès refusé lorsqu’ils tentent d’acquérir un handle (un identificateur unique) pour un objet Windows. Les applications Windows 64 bits natives sont nécessaires pour être compatible avec le compte d’utilisateur et à écrire des données dans les emplacements corrects.

3.  La virtualisation est désactivée pour une application si l’application inclut un manifeste d’application avec un attribut de niveau d’exécution demandé.

### <a name="request-execution-levels"></a>Demander des niveaux d’exécution
Un manifeste d’application est un fichier XML qui décrit et identifie les assemblys côte à côte, privés et partagés pour une application doit être relié au moment de l’exécution. Dans Windows Server 2012, le manifeste d’application contient des entrées pour assurer la compatibilité des applications UAC. Les applications d’administration qui incluent une entrée dans le manifeste d’application invite l’utilisateur pour l’autorisation de jeton d’accès de l’utilisateur d’accès. Même s’il manque une entrée dans le manifeste d’application, la plupart des applications d’administration peuvent s’exécuter sans modification à l’aide de correctifs de compatibilité des applications. Correctifs de compatibilité des applications sont des entrées de base de données qui permettent aux applications qui ne sont pas compatibles UAC de fonctionner correctement avec Windows Server 2012.

Toutes les applications compatibles UAC doivent avoir un niveau d’exécution demandé ajouté au manifeste de l’application. Si l’application requiert un accès administratif au système, puis marque l’application avec une exécution demandé au niveau de «exiger administrateur» permet de s’assurer que le système identifie ce programme comme une application d’administration et effectue les étapes d’élévation nécessaires. A demandé l’exécution de niveaux de spécifier les privilèges requis pour une application.

### <a name="installer-detection-technology"></a>Technologie de détection de programme d’installation
Programmes d’installation sont des applications conçues pour déployer des logiciels. La plupart des programmes d’installation d’écrire dans les répertoires système et les clés de Registre. Ces emplacements protégés du système sont généralement accessible en écriture uniquement par un administrateur dans la technologie de détection de programme d’installation, ce qui signifie que les utilisateurs standard n’ont pas de droits d’accès suffisants pour installer des programmes. Windows Server 2012 détecte de manière heuristique les programmes d’installation et les informations d’identification d’administrateur de requêtes ou une approbation de l’administrateur pour pouvoir exécuter avec des privilèges d’accès. Windows Server 2012 détecte également de manière heuristique les mises à jour et les programmes qui désinstallent les applications. Un des objectifs de contrôle de compte utilisateur consiste à empêcher des installations de s’exécuter sans une connaissance de l’utilisateur et de consentement, car les programmes d’installation écrivent dans des zones protégées du système de fichiers et du Registre.

Détection de programme d’installation s’applique uniquement aux:

-   fichiers exécutables 32 bits.

-   Applications sans un attribut de niveau d’exécution demandé.

-   Processus interactifs qui s’exécutent en tant qu’utilisateur standard avec UAC est activé.

Avant la création d’un processus 32 bits, les attributs suivants sont vérifiés pour déterminer s’il s’agit d’un programme d’installation:

-   Le nom de fichier comprend les mots clés tels que «install», «configuration» ou «mise à jour».

-   Les champs de ressources de contrôle de version contient les mots clés suivants: fournisseur, nom de la société, nom du produit, Description du fichier, nom de fichier d’origine, nom interne et nom d’exportation.

-   Mots clés dans le manifeste côte à côte sont incorporés dans le fichier exécutable.

-   Mots clés dans les entrées StringTable spécifiques sont liées dans le fichier exécutable.

-   Attributs de clé dans les données de script de ressources sont liées dans le fichier exécutable.

-   Il existe des séquences ciblées d’octets dans le fichier exécutable.

> [!NOTE]
> Les mots clés et les séquences d’octets sont dérivés des caractéristiques communes observées dans diverses technologies d’installation.

> [!NOTE]
> Le contrôle de compte d’utilisateur: Détecter les installations d’applications et demander l’élévation doit être activée pour la détection de programme d’installation détecter les programmes d’installation. Ce paramètre est activé par défaut et peut être configuré localement à l’aide du composant logiciel enfichable Stratégie de sécurité locale (Secpol.msc) ou configuré pour le domaine, unité d’organisation ou des groupes spécifiques par la stratégie de groupe (Gpedit.msc).


