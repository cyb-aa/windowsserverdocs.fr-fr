---
title: Processus des informations d’identification dans l’authentification Windows
description: Sécurité de Windows Server
ms.prod: windows-server
ms.technology: security-windows-auth
ms.topic: article
ms.assetid: 48c60816-fb8b-447c-9c8e-800c2e05b14f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 2de8383ce6a946dfdd80cfc027478c495037169b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857582"
---
# <a name="credentials-processes-in-windows-authentication"></a>Processus des informations d’identification dans l’authentification Windows

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique de référence pour les professionnels de l’informatique décrit comment l’authentification Windows traite les informations d’identification.

La gestion des informations d’identification Windows est le processus par lequel le système d’exploitation reçoit les informations d’identification du service ou de l’utilisateur et sécurise ces informations en vue d’une présentation future de la cible d’authentification. Dans le cas d’un ordinateur joint à un domaine, la cible d’authentification est le contrôleur de domaine. Les informations d’identification utilisées dans l’authentification sont des documents numériques qui associent l’identité de l’utilisateur à une certaine preuve d’authenticité, telle qu’un certificat, un mot de passe ou un code confidentiel.

Par défaut, les informations d’identification Windows sont validées par rapport à la base de données du gestionnaire de comptes de sécurité (SAM) sur l’ordinateur local, ou par rapport à Active Directory sur un ordinateur joint à un domaine, par le biais du service Winlogon. Les informations d’identification sont collectées via l’entrée d’utilisateur sur l’interface utilisateur d’ouverture de session ou par programme via l’interface de programmation d’applications (API) à présenter à la cible d’authentification.

Les informations de sécurité locales sont stockées dans le Registre sous **HKEY_LOCAL_MACHINE \Security**. Les informations stockées incluent les paramètres de stratégie, les valeurs de sécurité par défaut et les informations de compte, telles que les informations d’identification d’ouverture de session mises en cache. Une copie de la base de données SAM est également stockée ici, bien qu’elle soit protégée en écriture.

Le diagramme suivant montre les composants requis et les chemins d’accès que les informations d’identification effectuent via le système pour authentifier l’utilisateur ou le processus en vue d’une ouverture de session réussie.

![Diagramme qui montre les composants requis et les chemins d’accès que les informations d’identification effectuent via le système pour authentifier l’utilisateur ou le processus en vue d’une ouverture de session réussie.](../media/credentials-processes-in-windows-authentication/AuthN_LSA_Architecture_Client.gif)

Le tableau suivant décrit chaque composant qui gère les informations d’identification dans le processus d’authentification au point de connexion.

**Composants d’authentification pour tous les systèmes**

|Component|Description|
|-------|--------|
|Ouverture de session de l’utilisateur|Winlogon. exe est le fichier exécutable responsable de la gestion des interactions utilisateur sécurisées. Le service Winlogon lance le processus d’ouverture de session pour les systèmes d’exploitation Windows en passant les informations d’identification collectées par l’action de l’utilisateur sur le Bureau sécurisé (interface utilisateur de connexion) à l’autorité de sécurité locale (LSA) via secur32. dll.|
|Ouverture de session d’application|Les ouvertures de session d’application ou de service qui ne nécessitent pas d’ouverture de session interactive. La plupart des processus initiés par l’utilisateur s’exécutent en mode utilisateur à l’aide de secur32. dll, tandis que les processus lancés au démarrage, tels que les services, s’exécutent en mode noyau à l’aide de KsecDD. sys.<p>Pour plus d’informations sur le mode utilisateur et le mode noyau, consultez applications et mode utilisateur, services et mode noyau dans cette rubrique.|
|Secur32.dll|Plusieurs fournisseurs d’authentification qui constituent la base du processus d’authentification.|
|Lsasrv.dll|Le service serveur LSA, qui applique les stratégies de sécurité et agit comme gestionnaire de package de sécurité pour le LSA. Le LSA contient la fonction Negotiate, qui sélectionne le protocole NTLM ou Kerberos après avoir déterminé le protocole qui doit être exécuté.|
|Fournisseurs de support de sécurité|Ensemble de fournisseurs qui peuvent appeler individuellement un ou plusieurs protocoles d’authentification. L’ensemble de fournisseurs par défaut peut changer avec chaque version du système d’exploitation Windows, et les fournisseurs personnalisés peuvent être écrits.|
|Netlogon.dll|Les services exécutés par le service accès réseau sont les suivants :<p>-Gère le canal sécurisé de l’ordinateur (à ne pas confondre avec Schannel) sur un contrôleur de domaine.<br />-Transmet les informations d’identification de l’utilisateur via un canal sécurisé au contrôleur de domaine et retourne les identificateurs de sécurité (SID) de domaine et les droits d’utilisateur de l’utilisateur.<br />-Publie les enregistrements de ressource de service dans le système DNS (Domain Name System) et utilise le DNS pour résoudre les noms en adresses IP des contrôleurs de domaine.<br />: Implémente le protocole de réplication basé sur l’appel de procédure distante (RPC) pour synchroniser les contrôleurs de domaine principaux (PDC) et les contrôleurs de domaine de sauvegarde (BDC).|
|Samsrv. dll|Le gestionnaire de comptes de sécurité (SAM, Security Accounts Manager), qui stocke les comptes de sécurité locaux, applique les stratégies stockées localement et prend en charge les API.|
|Registre|Le registre contient une copie de la base de données SAM, des paramètres de stratégie de sécurité locale, des valeurs de sécurité par défaut et des informations de compte qui sont accessibles uniquement au système.|

Cette rubrique contient les sections suivantes :

-   [Entrée d’informations d’identification pour l’ouverture de session utilisateur](#BKMK_CrentialInputForUserLogon)

-   [Entrée des informations d’identification pour l’ouverture de session du service et de l’application](#BKMK_CredentialInputForApplicationAndServiceLogon)

-   [Autorité de sécurité locale](#BKMK_LSA)

-   [Validation et informations d’identification mises en cache](#BKMK_CachedCredentialsAndValidation)

-   [Stockage et validation des informations d’identification](#BKMK_CredentialStorageAndValidation)

-   [Base de données du gestionnaire de comptes de sécurité](#BKMK_SAM)

-   [Domaines locaux et domaines approuvés](#BKMK_LocalDomainsAndTrustedDomains)

-   [Certificats dans l’authentification Windows](#BKMK_CertificatesInWindowsAuthentication)

## <a name="credential-input-for-user-logon"></a><a name="BKMK_CrentialInputForUserLogon"></a>Entrée d’informations d’identification pour l’ouverture de session utilisateur
Dans Windows Server 2008 et Windows Vista, l’architecture GINA (Graphical Identification and Authentication) a été remplacée par un modèle de fournisseur d’informations d’identification, ce qui permet d’énumérer différents types d’ouverture de session via l’utilisation des vignettes d’ouverture de session. Les deux modèles sont décrits ci-dessous.

**Identification graphique et architecture d’authentification**

L’architecture GINA (Graphical Identification and Authentication) s’applique aux systèmes d’exploitation Windows Server 2003, Microsoft Windows 2000 Server, Windows XP et Windows 2000 Professional. Dans ces systèmes, chaque session interactive Logon crée une instance distincte du service Winlogon. L’architecture GINA est chargée dans l’espace de processus utilisé par Winlogon, reçoit et traite les informations d’identification, et effectue les appels aux interfaces d’authentification via LSALogonUser.

Les instances de Winlogon pour une ouverture de session interactive s’exécutent dans la session 0. La session 0 héberge les services système et d’autres processus critiques, y compris le processus de l’autorité de sécurité locale (LSA).

Le diagramme suivant illustre le processus d’information d’identification pour Windows Server 2003, Microsoft Windows 2000 Server, Windows XP et Microsoft Windows 2000 professionnel.

![Diagramme montrant le processus d’information d’identification pour Windows Server 2003, Microsoft Windows 2000 Server, Windows XP et Microsoft Windows 2000 professionnel](../media/credentials-processes-in-windows-authentication/AuthN_GINA_Architecture.gif)

**Architecture du fournisseur d’informations d’identification**

L’architecture du fournisseur d’informations d’identification s’applique aux versions désignées dans la liste **s’applique à** au début de cette rubrique. Dans ces systèmes, l’architecture d’entrée des informations d’identification a été remplacée par une conception extensible à l’aide de fournisseurs d’informations d’identification. Ces fournisseurs sont représentés par les différentes vignettes d’ouverture de session sur le Bureau sécurisé qui autorisent un nombre quelconque de scénarios de connexion : différents comptes pour le même utilisateur et différentes méthodes d’authentification, telles que le mot de passe, la carte à puce et la biométrie.

Avec l’architecture du fournisseur d’informations d’identification, Winlogon démarre toujours l’interface utilisateur d’ouverture de session après avoir reçu un événement de séquence de touches sécurisées. L’interface utilisateur de connexion interroge chaque fournisseur d’informations d’identification pour le nombre de types d’informations d’identification différents que le fournisseur est configuré pour énumérer. Les fournisseurs d’informations d’identification ont la possibilité de spécifier l’une de ces vignettes comme valeur par défaut. Une fois que tous les fournisseurs ont énuméré leurs vignettes, l’interface utilisateur d’ouverture de session les affiche à l’utilisateur. L’utilisateur interagit avec une vignette pour fournir ses informations d’identification. L’interface utilisateur d’ouverture de session soumet ces informations d’identification pour l’authentification.

Les fournisseurs d’informations d’identification ne sont pas des mécanismes de mise en œuvre. Ils sont utilisés pour rassembler et sérialiser les informations d’identification. L’autorité de sécurité locale et les packages d’authentification appliquent la sécurité.

Les fournisseurs d’informations d’identification sont inscrits sur l’ordinateur et sont responsables des éléments suivants :

-   Description des informations d’identification requises pour l’authentification.

-   Gestion de la communication et de la logique avec les autorités d’authentification externes.

-   Empaquetage des informations d’identification pour l’ouverture de session interactive et réseau.

L’empaquetage des informations d’identification pour l’ouverture de session interactive et réseau comprend le processus de sérialisation. En sérialisant les informations d’identification, plusieurs vignettes d’ouverture de session peuvent être affichées dans l’interface utilisateur d’ouverture de session. Par conséquent, votre organisation peut contrôler l’affichage des ouvertures de session, telles que les utilisateurs, les systèmes cibles pour l’ouverture de session, l’accès avant ouverture de session aux stratégies de verrouillage/déverrouillage de la station de travail et du réseau, via l’utilisation de fournisseurs d’informations d’identification personnalisés. Plusieurs fournisseurs d’informations d’identification peuvent coexister sur le même ordinateur.

Les fournisseurs d’authentification unique (SSO) peuvent être développés en tant que fournisseur d’informations d’identification standard ou en tant que fournisseur d’accès préalable à l’ouverture de session.

Chaque version de Windows contient un fournisseur d’informations d’identification par défaut et un fournisseur d’accès de pré-ouverture de session par défaut (PLAP), également appelé fournisseur SSO. Le fournisseur SSO permet aux utilisateurs d’établir une connexion à un réseau avant de se connecter à l’ordinateur local. Lorsque ce fournisseur est implémenté, le fournisseur n’énumère pas les vignettes dans l’interface utilisateur d’ouverture de session.

Un fournisseur SSO est destiné à être utilisé dans les scénarios suivants :

-   **L’authentification réseau et l’ouverture de session d’ordinateur sont gérées par différents fournisseurs d’informations d’identification.** Les variations de ce scénario sont les suivantes :

    -   Un utilisateur a la possibilité de se connecter à un réseau, tel qu’une connexion à un réseau privé virtuel (VPN), avant d’ouvrir une session sur l’ordinateur, mais il n’est pas nécessaire d’établir cette connexion.

    -   L’authentification réseau est nécessaire pour récupérer les informations utilisées pendant l’authentification interactive sur l’ordinateur local.

    -   Plusieurs authentifications réseau sont suivies de l’un des autres scénarios. Par exemple, un utilisateur s’authentifie auprès d’un fournisseur de services Internet (ISP), s’authentifie auprès d’un VPN, puis utilise les informations d’identification de son compte d’utilisateur pour ouvrir une session locale.

    -   Les informations d’identification mises en cache sont désactivées et une connexion des services d’accès à distance par le biais du VPN est requise avant l’ouverture de session locale pour authentifier l’utilisateur.

    -   Un utilisateur de domaine n’a pas de compte local configuré sur un ordinateur joint à un domaine et doit établir une connexion aux services d’accès à distance via une connexion VPN avant d’effectuer une ouverture de session interactive.

-   **L’authentification réseau et l’ouverture de session de l’ordinateur sont gérées par le même fournisseur d’informations d’identification.** Dans ce scénario, l’utilisateur doit se connecter au réseau avant d’ouvrir une session sur l’ordinateur.

**Énumération des vignettes de connexion**

Le fournisseur d’informations d’identification énumère les vignettes d’ouverture de session dans les cas suivants :

-   Pour les systèmes d’exploitation désignés dans la liste **s’applique à** au début de cette rubrique.

-   Le fournisseur d’informations d’identification énumère les vignettes pour l’ouverture de session de la station de travail. Le fournisseur d’informations d’identification sérialise généralement les informations d’identification pour l’authentification auprès de l’autorité de sécurité locale. Ce processus affiche des vignettes spécifiques à chaque utilisateur et spécifiques aux systèmes cibles de chaque utilisateur.

-   L’architecture d’ouverture de session et d’authentification permet à un utilisateur d’utiliser des vignettes énumérées par le fournisseur d’informations d’identification pour déverrouiller une station de travail. En règle générale, l’utilisateur actuellement connecté est la vignette par défaut, mais si plusieurs utilisateurs sont connectés, de nombreuses vignettes sont affichées.

-   Le fournisseur d’informations d’identification énumère les vignettes en réponse à une demande de l’utilisateur afin de modifier leur mot de passe ou d’autres informations privées, telles qu’un code confidentiel. En règle générale, l’utilisateur actuellement connecté est la vignette par défaut. Toutefois, si plusieurs utilisateurs sont connectés, de nombreuses vignettes s’affichent.

-   Le fournisseur d’informations d’identification énumère les vignettes en fonction des informations d’identification sérialisées à utiliser pour l’authentification sur les ordinateurs distants. L’interface utilisateur des informations d’identification n’utilise pas la même instance du fournisseur que l’interface utilisateur d’ouverture de session, déverrouiller la station de travail ou modifier le mot de passe. Par conséquent, les informations d’État ne peuvent pas être conservées dans le fournisseur entre les instances de l’interface utilisateur des informations d’identification. Cette structure génère une vignette pour chaque ouverture de session de l’ordinateur distant, en supposant que les informations d’identification ont été correctement sérialisées. Ce scénario est également utilisé dans le contrôle de compte d’utilisateur (UAC), qui peut aider à empêcher les modifications non autorisées sur un ordinateur en invitant l’utilisateur à avoir l’autorisation ou un mot de passe d’administrateur avant d’autoriser des actions susceptibles d’affecter le fonctionnement de l’ordinateur ou de modifier des paramètres qui affectent les autres utilisateurs de l’ordinateur.

Le diagramme suivant illustre le processus d’information d’identification pour les systèmes d’exploitation désignés dans la liste **s’applique à** au début de cette rubrique.

![Diagramme illustrant le processus d’informations d’identification pour les systèmes d’exploitation désignés dans la liste * * s’applique à * * au début de cette rubrique.](../media/credentials-processes-in-windows-authentication/AuthN_CredMan_CredProv.gif)

## <a name="credential-input-for-application-and-service-logon"></a><a name="BKMK_CredentialInputForApplicationAndServiceLogon"></a>Entrée des informations d’identification pour l’ouverture de session du service et de l’application
L’authentification Windows est conçue pour gérer les informations d’identification des applications ou des services qui ne nécessitent pas d’intervention de l’utilisateur. Les applications en mode utilisateur sont limitées en termes de ressources système auxquelles elles ont accès, tandis que les services peuvent avoir un accès illimité à la mémoire système et aux périphériques externes.

Les services système et les applications de niveau transport accèdent à un fournisseur SSP (Security Support Provider) par le biais de l’interface SSPI (Security Support Provider Interface) de Windows, qui fournit des fonctions pour énumérer les packages de sécurité disponibles sur un système, sélectionner un package et utiliser ce package pour obtenir une connexion authentifiée.

Lorsqu’une connexion client/serveur est authentifiée :

-   L’application côté client de la connexion envoie les informations d’identification au serveur à l’aide de la fonction SSPI `InitializeSecurityContext (General)`.

-   L’application côté serveur de la connexion répond avec la fonction SSPI `AcceptSecurityContext (General)`.

-   Les fonctions SSPI `InitializeSecurityContext (General)` et `AcceptSecurityContext (General)` sont répétées jusqu’à ce que tous les messages d’authentification nécessaires aient été échangés pour réussir ou échouer l’authentification.

-   Une fois la connexion authentifiée, le LSA sur le serveur utilise les informations du client pour créer le contexte de sécurité, qui contient un jeton d’accès.

-   Le serveur peut ensuite appeler la fonction SSPI `ImpersonateSecurityContext` pour attacher le jeton d’accès à un thread d’emprunt d’identité pour le service.

**Applications et mode utilisateur**

Le mode utilisateur dans Windows est composé de deux systèmes permettant de transmettre des demandes d’e/s aux pilotes en mode noyau appropriés : le système d’environnement, qui exécute des applications écrites pour de nombreux types différents de systèmes d’exploitation, et le système intégral, qui exploite des fonctions spécifiques au système pour le compte du système d’environnement.

Le système intégral gère les fonctions de system’specific d’exploitation pour le compte du système d’environnement et se compose d’un processus de système de sécurité (LSA), d’un service de station de travail et d’un service serveur. Le processus du système de sécurité traite des jetons de sécurité, accorde ou refuse des autorisations d’accès aux comptes d’utilisateur en fonction des autorisations de ressource, gère les demandes d’ouverture de session et lance l’authentification d’ouverture de session, et détermine les ressources système que le système d’exploitation doit auditer.

Les applications peuvent s’exécuter en mode utilisateur, où l’application peut s’exécuter comme n’importe quel principal, y compris dans le contexte de sécurité du système local (système). Les applications peuvent également s’exécuter en mode noyau, où l’application peut s’exécuter dans le contexte de sécurité du système local (système).

SSPI est disponible via le module secur32. dll, qui est une API utilisée pour obtenir des services de sécurité intégrés pour l’authentification, l’intégrité des messages et la confidentialité des messages. Il fournit une couche d’abstraction entre les protocoles de niveau application et les protocoles de sécurité. Étant donné que les applications différentes nécessitent différentes façons d’identifier ou d’authentifier les utilisateurs et les différentes méthodes de chiffrement des données lors de leur transit sur un réseau, SSPI offre un moyen d’accéder aux bibliothèques de liens dynamiques (dll) qui contiennent différentes fonctions d’authentification et de chiffrement. Ces dll sont appelées fournisseurs de support de sécurité (SSP).

Les comptes de service administrés et les comptes virtuels ont été introduits dans Windows Server 2008 R2 et Windows 7 pour fournir des applications cruciales, telles que Microsoft SQL Server et Internet Information Services (IIS), avec l’isolation de leurs propres comptes de domaine, tout en éliminant la nécessité pour un administrateur de gérer manuellement le nom de principal du service (SPN) et les informations d’identification de ces comptes. Pour plus d’informations sur ces fonctionnalités et leur rôle dans l’authentification, consultez [la documentation sur les comptes de service administrés pour Windows 7 et Windows Server 2008 R2](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx) et [vue d’ensemble des comptes de service administrés de groupe](../group-managed-service-accounts/group-managed-service-accounts-overview.md).

**Services et mode noyau**

Bien que la plupart des applications Windows s’exécutent dans le contexte de sécurité de l’utilisateur qui les démarre, ce n’est pas le cas des services. De nombreux services Windows, tels que les services réseau et d’impression, sont démarrés par le contrôleur de services lorsque l’utilisateur démarre l’ordinateur. Ces services peuvent s’exécuter en tant que service local ou système local et peuvent continuer à s’exécuter après la fermeture de la session de l’utilisateur humain.

> [!NOTE]
> Les services s’exécutent normalement dans des contextes de sécurité appelés système local (système), service réseau ou service local.  Windows Server 2008 R2 a introduit des services qui s’exécutent sous un compte de service administré, qui sont des principaux de domaine.

Avant de démarrer un service, le contrôleur de service se connecte à l’aide du compte désigné pour le service, puis présente les informations d’identification du service pour l’authentification par le LSA. Le service Windows implémente une interface de programmation que le gestionnaire de contrôleur de service peut utiliser pour contrôler le service. Un service Windows peut être démarré automatiquement lorsque le système est démarré ou manuellement à l’aide d’un programme de contrôle de service. Par exemple, lorsqu’un ordinateur client Windows joint un domaine, le service Messenger sur l’ordinateur se connecte à un contrôleur de domaine et y ouvre un canal sécurisé. Pour obtenir une connexion authentifiée, le service doit disposer d’informations d’identification approuvées par l’autorité de sécurité locale (LSA) de l’ordinateur distant. Lors de la communication avec d’autres ordinateurs du réseau, LSA utilise les informations d’identification du compte de domaine de l’ordinateur local, comme tous les autres services s’exécutant dans le contexte de sécurité du système local et du service réseau. Les services sur l’ordinateur local s’exécutent en tant que système, de sorte que les informations d’identification n’ont pas besoin d’être présentées à l’autorité LSA.

Le fichier KsecDD. sys gère et chiffre ces informations d’identification et utilise un appel de procédure locale dans le LSA. Le type de fichier est DRV (Driver) et est le fournisseur SSP (Security Support Provider) en mode noyau et, dans les versions indiquées dans la liste **s’applique à** au début de cette rubrique, est conforme à la norme FIPS 140-2 niveau 1.

Le mode noyau dispose d’un accès complet aux ressources matérielles et système de l’ordinateur. Le mode noyau arrête les services et applications en mode utilisateur pour accéder aux zones critiques du système d’exploitation auxquelles ils ne doivent pas avoir accès.

## <a name="local-security-authority"></a><a name="BKMK_LSA"></a>Autorité de sécurité locale
L’autorité de sécurité locale (LSA, Local Security Authority) est un processus système protégé qui authentifie et enregistre les utilisateurs sur l’ordinateur local. De plus, LSA gère les informations sur tous les aspects de la sécurité locale sur un ordinateur (ces aspects sont collectivement appelés stratégie de sécurité locale) et fournit plusieurs services pour la traduction entre les noms et les identificateurs de sécurité (SID). Le processus du système de sécurité, LSASS (local Security Authority Server Service), assure le suivi des stratégies de sécurité et des comptes qui sont en vigueur sur un système informatique.

Le LSA valide l’identité d’un utilisateur en fonction de laquelle des deux entités suivantes ont émis le compte de l’utilisateur :

-   **Autorité de sécurité locale.** L’autorité LSA peut valider les informations utilisateur en vérifiant la base de données du gestionnaire de comptes de sécurité (SAM) située sur le même ordinateur. N’importe quel poste de travail ou serveur membre peut stocker des informations sur les comptes d’utilisateurs locaux et sur les groupes locaux. Toutefois, ces comptes peuvent être utilisés pour accéder uniquement à cette station de travail ou à cet ordinateur.

-   **Autorité de sécurité pour le domaine local ou pour un domaine approuvé.** L’autorité LSA contacte l’entité qui a émis le compte et demande à la vérification que le compte est valide et que la demande provient du titulaire du compte.

Le service LSASS (Local Security Authority Subsystem Service) stocke en mémoire les informations d'identification pour le compte des utilisateurs avec les sessions Windows actives. Les informations d’identification stockées permettent aux utilisateurs d’accéder en toute transparence aux ressources réseau, telles que les partages de fichiers, les boîtes aux lettres Exchange Server et les sites SharePoint, sans entrer à nouveau leurs informations d’identification pour chaque service distant.

Le service LSASS peut stocker les informations d'identification sous des formes multiples, dont notamment :

-   Texte en clair chiffré réversible

-   Tickets Kerberos (Ticket-Granting tickets (TGT), tickets de service)

-   Hachage NT

-   Hachage LAN Manager (LM)

Si l’utilisateur ouvre une session Windows à l’aide d’une carte à puce, LSASS ne stocke pas de mot de passe en texte clair, mais stocke la valeur de hachage NT correspondante pour le compte et le code confidentiel en texte clair pour la carte à puce. Si l'attribut de compte est activé pour une carte à puce requise pour une ouverture de session interactive, une valeur de hachage NT aléatoire est générée automatiquement pour le compte à la place du hachage de mot de passe d'origine. Le hachage de mot de passe généré automatiquement quand l'attribut est défini ne change pas.

Si un utilisateur ouvre une session sur un ordinateur Windows avec un mot de passe compatible avec les hachages LAN Manager (LM), cet authentificateur est présent en mémoire.

Le stockage des informations d'identification en texte en clair dans la mémoire ne peut pas être désactivé, même si les fournisseurs d'informations d'identification qui en ont besoin sont désactivés.

Les informations d’identification stockées sont directement associées aux sessions d’ouverture de session service LSASS (Local Security Authority Subsystem Service) (LSASS) qui ont été démarrées après le dernier redémarrage et qui n’ont pas été fermées. Par exemple, des sessions LSA sont créées avec des informations d'identification LSA stockées lorsqu'un utilisateur effectue l'une des opérations suivantes :

-   Ouvre une session locale ou protocole RDP (Remote Desktop Protocol) (RDP) sur l’ordinateur.

-   Exécution d'une tâche en utilisant l'option **RunAs**

-   Exécution d'un service Windows actif sur l'ordinateur

-   Exécution d'une tâche planifiée ou d'un traitement par lots planifié

-   Exécution d'une tâche sur l'ordinateur local à l'aide d'un outil d'administration à distance

Dans certains cas, les secrets LSA, qui sont des éléments secrets de données accessibles uniquement aux processus de compte système, sont stockés sur le disque dur. Certains de ces secrets sont des informations d'identification qui doivent être conservées après le redémarrage. Ils sont stockés sous forme chiffrée sur le lecteur de disque dur. Les informations d'identification stockées en tant que secrets LSA peuvent inclure les éléments suivants :

-   Mot de passe du compte de Active Directory Domain Services (AD DS) de l’ordinateur

-   Mots de passe de compte utilisés pour les services Windows configurés sur l'ordinateur

-   Mots de passe de compte utilisés pour les tâches planifiées configurées

-   Mots de passe de compte utilisés pour les sites web et les pools d'applications IIS

-   Mots de passe pour les comptes Microsoft

Introduite dans Windows 8.1, le système d’exploitation client offre une protection supplémentaire pour l’autorité LSA afin d’empêcher la lecture de la mémoire et l’injection de code par des processus non protégés. Cette protection augmente la sécurité des informations d’identification stockées et gérées par l’autorité LSA.

Pour plus d’informations sur ces protections supplémentaires, consultez Configuration d’une [protection LSA supplémentaire](../credentials-protection-and-management/configuring-additional-lsa-protection.md).

## <a name="cached-credentials-and-validation"></a><a name="BKMK_CachedCredentialsAndValidation"></a>Validation et informations d’identification mises en cache
Les mécanismes de validation reposent sur la présentation des informations d’identification au moment de l’ouverture de session. Toutefois, lorsque l’ordinateur est déconnecté d’un contrôleur de domaine et que l’utilisateur présente des informations d’identification de domaine, Windows utilise le processus d’informations d’identification mises en cache dans le mécanisme de validation.

Chaque fois qu’un utilisateur se connecte à un domaine, Windows met en cache les informations d’identification fournies et les stocke dans la ruche sécurité dans le Registre du système d’exploitation.

Avec les informations d’identification mises en cache, l’utilisateur peut se connecter à un membre du domaine sans être connecté à un contrôleur de domaine au sein de ce domaine.


## <a name="credential-storage-and-validation"></a><a name="BKMK_CredentialStorageAndValidation"></a>Stockage et validation des informations d’identification
Il n’est pas toujours souhaitable d’utiliser un ensemble d’informations d’identification pour accéder à différentes ressources. Par exemple, un administrateur peut souhaiter utiliser des informations d’authentification plutôt que des informations d’identification de l’utilisateur lors de l’accès à un serveur distant. De même, si un utilisateur accède à des ressources externes, telles qu’un compte bancaire, il peut uniquement utiliser des informations d’identification différentes de celles de son domaine. Les sections suivantes décrivent les différences de gestion des informations d’identification entre les versions actuelles des systèmes d’exploitation Windows et les systèmes d’exploitation Windows Vista et Windows XP.

### <a name="remote-logon-credential-processes"></a>Processus d’informations d’identification d’ouverture de session à distance
Le protocole RDP (Remote Desktop Protocol) (RDP) gère les informations d’identification de l’utilisateur qui se connecte à un ordinateur distant à l’aide du client Bureau à distance, qui a été introduit dans Windows 8. Les informations d’identification sous forme de texte en clair sont envoyées à l’hôte cible sur lequel l’hôte tente d’exécuter le processus d’authentification et, en cas de réussite, connecte l’utilisateur aux ressources autorisées. Le protocole RDP ne stocke pas les informations d’identification sur le client, mais les informations d’identification de domaine de l’utilisateur sont stockées dans le processus LSASS.

Introduite dans Windows Server 2012 R2 et Windows 8.1, le mode administrateur restreint offre une sécurité supplémentaire aux scénarios de connexion à distance. Ce mode de Bureau à distance oblige l’application cliente à effectuer une demande d’ouverture de session réseau-réponse avec la fonction unidirectionnelle NT (NTOWF) ou utiliser un ticket de service Kerberos lors de l’authentification auprès de l’hôte distant. Une fois que l’administrateur est authentifié, l’administrateur n’a pas les informations d’identification de compte respectives dans LSASS, car elles n’ont pas été fournies à l’hôte distant. Au lieu de cela, l’administrateur a les informations d’identification du compte d’ordinateur pour la session. Les informations d’identification d’administrateur ne sont pas fournies à l’hôte distant, de sorte que les actions sont exécutées en tant que compte d’ordinateur. Les ressources sont également limitées au compte d’ordinateur, et l’administrateur ne peut pas accéder aux ressources avec son propre compte.

### <a name="automatic-restart-sign-on-credential-process"></a>Processus d’informations d’identification de l’authentification de redémarrage automatique
Lorsqu’un utilisateur se connecte sur un appareil Windows 8.1, LSA enregistre les informations d’identification de l’utilisateur dans la mémoire chiffrée qui sont accessibles uniquement par LSASS. exe. Lorsque Windows Update lance un redémarrage automatique sans présence de l’utilisateur, ces informations d’identification sont utilisées pour configurer l’ouverture de la connexion automatique pour l’utilisateur.

Au redémarrage, l’utilisateur est automatiquement connecté via le mécanisme d’ouverture de session automatique, puis l’ordinateur est verrouillé pour protéger la session de l’utilisateur. Le verrouillage est initié par Winlogon, tandis que la gestion des informations d’identification est effectuée par LSA. En se connectant et en verrouillant automatiquement la session de l’utilisateur sur la console, les applications de l’écran de verrouillage de l’utilisateur sont redémarrées et disponibles.

Pour plus d’informations sur connexion, consultez [connexion automatique de &#40;redémarrage connexion&#41;](winlogon-automatic-restart-sign-on-arso.md).

### <a name="stored-user-names-and-passwords-in-windows-vista-and-windows-xp"></a>Noms d’utilisateur et mots de passe stockés dans Windows Vista et Windows XP
Dans Windows Server 2008, Windows Server 2003, Windows Vista et Windows XP, les **noms d’utilisateur et mots de passe stockés** dans le panneau de configuration simplifient la gestion et l’utilisation de plusieurs ensembles d’informations d’identification d’ouverture de session, y compris les certificats X. 509 utilisés avec les cartes à puce et les informations d’identification Windows Live (désormais appelés compte Microsoft). Les informations d’identification, qui font partie du profil de l’utilisateur, sont stockées jusqu’à ce qu’elles soient nécessaires. Cette action peut augmenter la sécurité pour chaque ressource en s’assurant que, si un mot de passe est compromis, la sécurité n’est pas compromise.

Lorsqu’un utilisateur ouvre une session et tente d’accéder à des ressources supplémentaires protégées par mot de passe, telles qu’un partage sur un serveur, et si les informations d’identification d’ouverture de session par défaut de l’utilisateur ne sont pas suffisantes pour accéder à, les **noms d’utilisateur et les mots de passe stockés** sont interrogés. Si d’autres informations d’identification avec les informations d’ouverture de session correctes ont été enregistrées dans les **noms d’utilisateur et mots de passe stockés**, ces informations d’identification sont utilisées pour accéder à. Dans le cas contraire, l’utilisateur est invité à fournir de nouvelles informations d’identification, qui peuvent ensuite être enregistrées en vue de leur réutilisation, soit plus tard dans la session de connexion, soit lors d’une session suivante.

Les restrictions suivantes s’appliquent :

-   Si les **noms d’utilisateur et mots de passe stockés** contiennent des informations d’identification non valides ou incorrectes pour une ressource spécifique, l’accès à la ressource est refusé et la boîte de dialogue **noms et mots de passe d’utilisateur stockés** ne s’affiche pas.

-   Les **noms d’utilisateur et mots de passe stockés** stockent les informations d’identification uniquement pour l’authentification NTLM, le protocole Kerberos, le compte Microsoft (anciennement Windows Live ID) et l’authentification protocole SSL (SSL). Certaines versions d’Internet Explorer maintiennent leur propre cache pour l’authentification de base.

Ces informations d’identification deviennent une partie chiffrée du profil local d’un utilisateur dans le répertoire \Documents and Settings\Username\Application Data\Microsoft\Credentials. Par conséquent, ces informations d’identification peuvent être itinérantes avec l’utilisateur si la stratégie réseau de l’utilisateur prend en charge les profils utilisateur itinérants. Toutefois, si l’utilisateur a des copies de **noms d’utilisateurs et de mots de passe stockés** sur deux ordinateurs différents et modifie les informations d’identification associées à la ressource sur l’un de ces ordinateurs, la modification n’est pas propagée aux **noms d’utilisateur et mots de passe stockés** sur le deuxième ordinateur.

### <a name="windows-vault-and-credential-manager"></a>Coffre Windows et gestionnaire d’informations d’identification
Le gestionnaire des informations d’identification a été introduit dans Windows Server 2008 R2 et Windows 7 en tant que fonctionnalité du panneau de configuration pour stocker et gérer les noms d’utilisateur et les mots de passe. Le gestionnaire d’informations d’identification permet aux utilisateurs de stocker des informations d’identification pertinentes pour d’autres systèmes et sites Web dans le coffre Windows sécurisé. Certaines versions d’Internet Explorer utilisent cette fonctionnalité pour l’authentification auprès des sites Web.

La gestion des informations d’identification à l’aide du Gestionnaire d’informations d’identification est contrôlée par l’utilisateur sur l’ordinateur local. Les utilisateurs peuvent enregistrer et stocker des informations d’identification à partir des navigateurs et des applications Windows pris en charge de manière à ce qu’il leur soit plus simple de se connecter à ces ressources. Les informations d’identification sont enregistrées dans des dossiers chiffrés spéciaux sur l’ordinateur sous le profil de l’utilisateur. Les applications qui prennent en charge cette fonctionnalité (par le biais de l’utilisation des API du gestionnaire d’informations d’identification), telles que les navigateurs Web et les applications, peuvent présenter les informations d’identification correctes à d’autres ordinateurs et sites Web pendant le processus d’ouverture de session.

Lorsqu’un site Web, une application ou un autre ordinateur demande une authentification via NTLM ou le protocole Kerberos, une boîte de dialogue s’affiche dans laquelle vous activez la case à cocher **mettre à jour les informations d’identification par défaut** ou **enregistrer le mot de passe** . Cette boîte de dialogue qui permet à un utilisateur d’enregistrer des informations d’identification localement est générée par une application qui prend en charge les API du gestionnaire d’informations d’identification. Si l’utilisateur active la case à cocher **enregistrer le mot de passe** , le gestionnaire des informations d’identification effectue le suivi du nom d’utilisateur, du mot de passe et des informations associées pour le service d’authentification en cours d’utilisation.

La prochaine fois que le service est utilisé, Credential Manager fournit automatiquement les informations d’identification stockées dans le coffre Windows. Si ces informations sont refusées, l’utilisateur est invité à fournir les informations d’accès correctes. Si l’accès est accordé avec les nouvelles informations d’identification, le gestionnaire d’informations d’identification remplace les informations d’identification précédentes par le nouveau, puis stocke les nouvelles informations d’identification dans le coffre Windows.

## <a name="security-accounts-manager-database"></a><a name="BKMK_SAM"></a>Base de données du gestionnaire de comptes de sécurité
Le gestionnaire de comptes de sécurité (SAM, Security Accounts Manager) est une base de données qui stocke les groupes et comptes d’utilisateurs locaux. Il est présent dans tous les systèmes d’exploitation Windows ; Toutefois, lorsqu’un ordinateur est joint à un domaine, Active Directory gère les comptes de domaine dans des domaines Active Directory.

Par exemple, les ordinateurs clients qui exécutent un système d’exploitation Windows participent à un domaine de réseau en communiquant avec un contrôleur de domaine même si aucun utilisateur humain n’est connecté. Pour initier des communications, l’ordinateur doit disposer d’un compte actif dans le domaine. Avant d’accepter les communications à partir de l’ordinateur, l’autorité LSA sur le contrôleur de domaine authentifie l’identité de l’ordinateur, puis construit le contexte de sécurité de l’ordinateur de la même façon que pour un principal de sécurité humain. Ce contexte de sécurité définit l’identité et les fonctionnalités d’un utilisateur ou d’un service sur un ordinateur particulier ou un utilisateur, un service ou un ordinateur sur un réseau. Par exemple, le jeton d’accès contenu dans le contexte de sécurité définit les ressources (telles qu’un partage de fichiers ou une imprimante) auxquelles il est possible d’accéder et les actions (telles que la lecture, l’écriture ou la modification) qui peuvent être effectuées par ce principal (un utilisateur, un ordinateur ou un service sur cette ressource).

Le contexte de sécurité d’un utilisateur ou d’un ordinateur peut varier d’un ordinateur à un autre, par exemple lorsqu’un utilisateur se connecte à un serveur ou à une station de travail autre que la station de travail principale de l’utilisateur. Il peut également varier d’une session à l’autre, par exemple lorsqu’un administrateur modifie les droits et les autorisations de l’utilisateur. En outre, le contexte de sécurité est généralement différent lorsqu’un utilisateur ou un ordinateur fonctionne de façon autonome, dans un réseau ou dans le cadre d’un domaine de Active Directory.

## <a name="local-domains-and-trusted-domains"></a><a name="BKMK_LocalDomainsAndTrustedDomains"></a>Domaines locaux et domaines approuvés
Lorsqu’il existe une approbation entre deux domaines, les mécanismes d’authentification de chaque domaine s’appuient sur la validité des authentifications provenant de l’autre domaine. Les approbations permettent de fournir un accès contrôlé aux ressources partagées dans un domaine de ressource (domaine d’approbation) en vérifiant que les demandes d’authentification entrantes proviennent d’une autorité approuvée (le domaine approuvé). De cette façon, les approbations agissent comme des ponts qui permettent uniquement aux demandes d’authentification validées de circuler entre les domaines.

La façon dont une approbation spécifique passe les demandes d’authentification dépend de la façon dont elles sont configurées. Les relations d’approbation peuvent être unidirectionnelles, en fournissant l’accès à partir du domaine approuvé aux ressources du domaine d’approbation, ou bidirectionnel, en fournissant un accès à partir de chaque domaine aux ressources de l’autre domaine. Les approbations sont également non transitives, auquel cas une approbation existe uniquement entre les deux domaines de partenaire de confiance, ou transitive. dans ce cas, une approbation s’étend automatiquement à tous les autres domaines approuvés par l’un des partenaires.

Pour plus d’informations sur les relations d’approbation de domaine et de forêt concernant l’authentification, consultez [authentification déléguée et relations d’approbation](https://technet.microsoft.com/library/dn169022.aspx).

## <a name="certificates-in-windows-authentication"></a><a name="BKMK_CertificatesInWindowsAuthentication"></a>Certificats dans l’authentification Windows
Une infrastructure à clé publique (PKI) est une combinaison de logiciels, de technologies de chiffrement, de processus et de services qui permettent à une organisation de sécuriser ses communications et ses transactions commerciales. La capacité d’une infrastructure à clé publique à sécuriser les communications et les transactions métier est basée sur l’échange de certificats numériques entre les utilisateurs authentifiés et les ressources approuvées.

Un certificat numérique est un document électronique qui contient des informations sur l’entité à laquelle il appartient, l’entité sur laquelle il a été émis, un numéro de série unique ou d’autres dates d’identification, d’émission et d’expiration uniques, ainsi qu’une empreinte digitale numérique.

L’authentification est le processus qui consiste à déterminer si un hôte distant peut être approuvé. Pour établir sa fiabilité, l’hôte distant doit fournir un certificat d’authentification acceptable.

Les hôtes distants établissent leur fiabilité en obtenant un certificat auprès d’une autorité de certification. L’autorité de certification peut à son tour avoir la certification d’une autorité supérieure, qui crée une chaîne de confiance. Pour déterminer si un certificat est digne de confiance, une application doit déterminer l’identité de l’autorité de certification racine, puis déterminer s’il est digne de confiance.

De même, l’hôte distant ou l’ordinateur local doit déterminer si le certificat présenté par l’utilisateur ou l’application est authentique. Le certificat présenté par l’utilisateur via LSA et SSPI est évalué pour l’authenticité sur l’ordinateur local pour une ouverture de session locale, sur le réseau ou sur le domaine via les magasins de certificats dans Active Directory.

Pour produire un certificat, les données d’authentification passent par des algorithmes de hachage, tels que Secure Hash Algorithm 1 (SHA1), afin de produire un résumé de message. La synthèse du message est ensuite signée numériquement à l’aide de la clé privée de l’expéditeur pour prouver que le résumé du message a été généré par l’expéditeur.

> [!NOTE]
> SHA1 est la valeur par défaut dans Windows 7 et Windows Vista, mais elle a été remplacée par SHA2 dans Windows 8.

**Authentification par carte à puce**

La technologie de carte à puce est un exemple d’authentification basée sur les certificats. La connexion à un réseau à l’aide d’une carte à puce offre une forme fiable d’authentification, car elle utilise l’identification basée sur le chiffrement et la preuve de possession lors de l’authentification d’un utilisateur dans un domaine. Les services de certificats Active Directory (AD CS) fournissent l’identification basée sur le chiffrement par le biais de l’émission d’un certificat d’ouverture de session pour chaque carte à puce.

Pour plus d’informations sur l’authentification par carte à puce, consultez la [référence technique de la carte à puce Windows](https://technet.microsoft.com/library/ff404297.aspx).

La technologie de carte à puce virtuelle a été introduite dans Windows 8. Il stocke le certificat de la carte à puce sur le PC, puis le protège à l’aide de la puce de sécurité de l’Module de plateforme sécurisée (TPM) anti-falsification de l’appareil. De cette façon, l’ordinateur devient en fait la carte à puce qui doit recevoir le PIN de l’utilisateur pour être authentifié.

**Authentification distante et sans fil**

L’authentification réseau à distance et sans fil est une autre technologie qui utilise des certificats pour l’authentification. Le service d’authentification Internet (IAS) et les serveurs de réseau privé virtuel utilisent le protocole EAP-TLS (Extensible Authentication Protocol-Transport Level Security), le protocole PEAP (Protected Extensible Authentication Protocol) ou IPsec (Internet Protocol Security) pour effectuer une authentification basée sur les certificats pour de nombreux types d’accès réseau, y compris les réseaux privés virtuels (VPN) et sans fil.

Pour plus d’informations sur l’authentification basée sur les certificats dans la mise en réseau, consultez [authentification et certificats d’accès réseau](https://technet.microsoft.com/library/cc759575(WS.10).aspx).

## <a name="see-also"></a><a name="BKMK_SeeAlso"></a>Voir aussi
[Concepts de l’authentification Windows](https://docs.microsoft.com/windows-server/security/windows-authentication/windows-authentication-concepts)


