---
title: Processus des informations d’identification dans l’authentification Windows
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 48c60816-fb8b-447c-9c8e-800c2e05b14f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 52d50a1bb6bfbe9d35146f362a2a5184df0a6504
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839860"
---
# <a name="credentials-processes-in-windows-authentication"></a>Processus des informations d’identification dans l’authentification Windows

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique de référence pour les professionnels de l’informatique décrit comment l’authentification Windows traite les informations d’identification.

Gestion des informations d’identification Windows est le processus par lequel le système d’exploitation reçoit les informations d’identification du service ou d’un utilisateur et permet de sécuriser ces informations pour la présentation ultérieure à la cible d’authentification. Dans le cas d’un ordinateur joint au domaine, la cible d’authentification est le contrôleur de domaine. Les informations d’identification utilisées dans l’authentification sont des documents numériques qui associent l’identité de l’utilisateur en une forme de preuve d’authenticité, comme un certificat, un mot de passe ou un code confidentiel.

Par défaut, les informations d’identification Windows sont validées par rapport à la base de données du Gestionnaire de comptes de sécurité (SAM) sur l’ordinateur local, ou par rapport à Active Directory sur un ordinateur joint au domaine, via le service Winlogon. Informations d’identification sont collectées via l’entrée d’utilisateur sur l’interface utilisateur d’ouverture de session ou par programme via l’interface de programmation d’application (API) qui sera présenté à la cible d’authentification.

Informations de sécurité local sont stockées dans le Registre sous **HKEY_LOCAL_MACHINE\SECURITY**. Informations stockées incluent des paramètres de stratégie, les valeurs de sécurité par défaut et les informations de compte, telles que des informations d’identification d’ouverture de session mis en cache. Une copie de la base de données SAM est également stockée ici, même s’il est protégé en écriture.

Le diagramme suivant illustre les composants requis et les chemins d’accès que les informations d’identification prennent via le système pour authentifier l’utilisateur ou le processus pour une ouverture de session réussie.

![Diagramme qui montre les composants requis et les chemins d’accès que les informations d’identification prennent via le système pour authentifier l’utilisateur ou le processus pour une ouverture de session réussie.](../media/credentials-processes-in-windows-authentication/AuthN_LSA_Architecture_Client.gif)

Le tableau suivant décrit chaque composant qui gère les informations d’identification dans le processus d’authentification au point d’ouverture de session.

**Composants d’authentification pour tous les systèmes**

|Component|Description|
|-------|--------|
|Ouverture de session de l’utilisateur|Winlogon.exe est le fichier exécutable chargé de gérer les interactions utilisateur sécurisée. Le service Winlogon lance le processus d’ouverture de session pour les systèmes d’exploitation Windows en passant les informations d’identification collectées par l’utilisateur sur le bureau sécurisé (l’interface utilisateur d’ouverture de session) à l’autorité de sécurité locale (LSA) via Secur32.dll.|
|Connexion de l’application|Application ou les ouvertures de session de service qui ne nécessitent pas d’ouverture de session interactive. La plupart des processus lancés par l’utilisateur est exécuté en mode utilisateur à l’aide de Secur32.dll tandis que le processus lancés au démarrage, tels que les services, s’exécutent en mode noyau à l’aide de Ksecdd.sys.<br /><br />Pour plus d’informations sur le mode utilisateur et mode noyau, voir les Applications et de Mode utilisateur ou de Services et de Mode noyau dans cette rubrique.|
|Secur32.dll|Plusieurs fournisseurs d’authentification qui constituent la base du processus d’authentification.|
|Lsasrv.dll|Le service serveur LSA, qui applique les stratégies de sécurité et agit comme le Gestionnaire de package de sécurité pour l’autorité LSA. L’autorité LSA contient la fonction Negotiate, qui sélectionne le protocole Kerberos ou NTLM après avoir déterminé quel protocole est réussisse.|
|Fournisseurs de sécurité|Un ensemble de fournisseurs qui peut appeler individuellement un ou plusieurs protocoles d’authentification. L’ensemble de fournisseurs par défaut peut changer à chaque version du système d’exploitation Windows et des fournisseurs personnalisés peuvent être écrits.|
|Netlogon.dll|Les services qui exécute le service Net Logon sont comme suit :<br /><br />-Gère le canal de l’ordinateur sécurisé (ne pas pour confondre avec Schannel) à un contrôleur de domaine.<br />-Transmet les informations d’identification de l’utilisateur via un canal sécurisé au contrôleur de domaine et retourne les identificateurs de sécurité (SID) du domaine et les droits d’utilisateur pour l’utilisateur.<br />-Publie des enregistrements de ressource de service dans le système DNS (Domain Name) et utilise le système DNS pour résoudre les noms aux adresses IP (Internet Protocol) des contrôleurs de domaine.<br />-Implémente le protocole de réplication basé sur l’appel de procédure distante (RPC) pour la synchronisation des contrôleurs de domaine principal (PDC) et contrôleurs de domaine de sauvegarde (BDC).|
|Samsrv.dll|Le Gestionnaire de SAM (Security Accounts), qui stocke les comptes de sécurité locaux, applique les stratégies stockées localement et prend en charge des API.|
|Registre|Le Registre contient une copie de la base de données SAM, les paramètres de stratégie de sécurité locale, valeurs de sécurité par défaut et les informations de compte qui sont uniquement accessibles dans le système de.|

Cette rubrique contient les sections suivantes :

-   [Entrée d’informations d’identification d’ouverture de session utilisateur](#BKMK_CrentialInputForUserLogon)

-   [Entrée d’informations d’identification pour l’application et de la connexion au service](#BKMK_CredentialInputForApplicationAndServiceLogon)

-   [Autorité de sécurité locale](#BKMK_LSA)

-   [Validation et les informations d’identification mises en cache](#BKMK_CachedCredentialsAndValidation)

-   [Stockage des informations d’identification et validation](#BKMK_CredentialStorageAndValidation)

-   [Base de données du Gestionnaire de comptes de sécurité](#BKMK_SAM)

-   [Les domaines locaux et des domaines approuvés](#BKMK_LocalDomainsAndTrustedDomains)

-   [Certificats de l’authentification Windows](#BKMK_CertificatesInWindowsAuthentication)

## <a name="BKMK_CrentialInputForUserLogon"></a>Entrée d’informations d’identification d’ouverture de session utilisateur
Dans Windows Server 2008 et Windows Vista, l’architecture Graphical Identification and Authentication (GINA) a été remplacée par un modèle de fournisseur d’informations d’identification, ce qui rendait possible d’énumérer des types différents d’ouverture de session via l’utilisation des vignettes de l’ouverture de session. Les deux modèles sont décrits ci-dessous.

**Identification et authentification graphique architecture**

L’architecture Graphical Identification and Authentication (GINA) s’applique aux systèmes d’exploitation Windows Server 2003, Microsoft Windows 2000 Server, Windows XP et Windows 2000 Professionnel. Dans ces systèmes, chaque ouverture de session interactive crée une instance distincte du service Winlogon. L’architecture de GINA est chargé dans l’espace de processus utilisé par Winlogon, reçoit et traite les informations d’identification et effectue les appels aux interfaces de l’authentification via LSALogonUser.

Les instances de Winlogon pour une ouverture de session interactive s’exécutent dans la Session 0. Services de système de hôtes de session 0 et autres processus essentiels, notamment le processus de l’autorité de sécurité locale (LSA).

Le diagramme suivant illustre le processus d’informations d’identification pour Windows Server 2003, Microsoft Windows 2000 Server, Windows XP et Microsoft Windows 2000 Professionnel.

![Diagramme illustrant le processus d’informations d’identification pour Windows Server 2003, Microsoft Windows 2000 Server, Windows XP et Microsoft Windows 2000 Professionnel](../media/credentials-processes-in-windows-authentication/AuthN_GINA_Architecture.gif)

**Architecture du fournisseur d’informations d’identification**

L’architecture du fournisseur d’informations d’identification s’applique à ces versions désignées dans la **s’applique à** figurant au début de cette rubrique. Dans ces systèmes, l’architecture d’entrée d’informations d’identification modifiée vers une conception extensible à l’aide de fournisseurs d’informations d’identification. Ces fournisseurs sont représentés par les vignettes de différents d’ouverture de session sur le bureau sécurisé qui permettent à n’importe quel nombre de scénarios d’ouverture de session - comptes différents pour le même utilisateur et les différentes méthodes d’authentification, telles que le mot de passe, carte à puce et la biométrie.

Avec l’architecture du fournisseur d’informations d’identification, Winlogon démarre toujours l’interface utilisateur d’ouverture de session après avoir reçu un événement de séquence de touches de sécurité. Interface utilisateur de connexion interroge chaque fournisseur d’informations d’identification pour le nombre de types différents d’informations d’identification que le fournisseur est configuré pour énumérer. Les fournisseurs d’informations d’identification ont la possibilité de spécifier une de ces vignettes en tant que la valeur par défaut. Une fois que tous les fournisseurs ont énumérés leurs vignettes, l’interface utilisateur d’ouverture de session les affiche à l’utilisateur. L’utilisateur interagit avec une vignette pour fournir leurs informations d’identification. L’interface utilisateur de l’ouverture de session soumet ces informations d’identification pour l’authentification.

Les fournisseurs d’informations d’identification ne sont pas des mécanismes de mise en œuvre. Ils sont utilisés pour collecter et sérialiser les informations d’identification. Les packages de l’autorité de sécurité locale et l’authentification appliquent la sécurité.

Les fournisseurs d’informations d’identification sont inscrits sur l’ordinateur et sont responsables pour les éléments suivants :

-   Décrivant les informations d’identification requises pour l’authentification.

-   Gestion des communications et la logique avec les autorités d’authentification externe.

-   Informations d’identification de l’empaquetage pour interactive et d’ouverture de session réseau.

Informations d’identification de l’empaquetage pour interactive et ouverture de session réseau inclut le processus de sérialisation. En sérialisant les informations d’identification de plusieurs mosaïques d’ouverture de session peuvent être affichés sur l’interface utilisateur d’ouverture de session. Par conséquent, votre organisation peut contrôler l’affichage d’ouverture de session tels que les utilisateurs, des systèmes cibles d’ouverture de session, d’ouverture de session préalable accéder aux réseau et la station de travail Verrouiller/déverrouiller stratégies - en utilisant des fournisseurs d’informations d’identification personnalisées. Plusieurs fournisseurs d’informations d’identification peuvent coexister sur le même ordinateur.

Uniques fournisseurs sign-on (SSO) peuvent être développés sous la forme d’un fournisseur d’informations d’identification standard ou un fournisseur de Pre-Logon-Access.

Chaque version de Windows contient le fournisseur d’informations d’identification d’une valeur par défaut et une valeur par défaut Pre-Logon-Access fournisseur (PLAP), également connu sous le fournisseur d’authentification unique. Le fournisseur d’authentification unique permet aux utilisateurs d’établir une connexion à un réseau avant de vous connecter à l’ordinateur local. Lorsque ce fournisseur est implémenté, le fournisseur n’énumère pas les vignettes sur l’interface utilisateur d’ouverture de session.

Un fournisseur d’authentification unique est destiné à être utilisée dans les scénarios suivants :

-   **Ouverture de session réseau l’authentification et l’ordinateur sont gérées par les fournisseurs d’informations d’identification différentes.** Variantes de ce scénario sont les suivantes :

    -   Un utilisateur a la possibilité de vous connecter à un réseau, telles que la connexion à un réseau privé virtuel (VPN), avant d’ouvrir une session sur l’ordinateur, mais n’est pas requis pour établir cette connexion.

    -   L’authentification réseau est nécessaire pour récupérer des informations utilisées pendant l’authentification interactive sur l’ordinateur local.

    -   Plusieurs authentifications de réseau sont suivies par l’un des autres scénarios. Par exemple, un utilisateur s’authentifie auprès d’un fournisseur de services Internet (ISP) s’authentifie auprès d’un réseau privé virtuel et utilise ensuite leurs informations d’identification du compte utilisateur pour se connecter localement.

    -   Les informations d’identification mises en cache sont désactivées, et une connexion de Services d’accès à distance via VPN est nécessaire avant l’ouverture de session locale pour authentifier l’utilisateur.

    -   Un utilisateur de domaine n’a pas un compte local sur un ordinateur joint au domaine et doit établir une connexion de Services d’accès à distance via une connexion VPN avant la fin d’ouverture de session interactive.

-   **Ouverture de session réseau l’authentification et l’ordinateur sont gérées par le même fournisseur d’informations d’identification.** Dans ce scénario, l’utilisateur est nécessaire pour se connecter au réseau avant d’ouvrir une session sur l’ordinateur.

**Énumération de vignette d’ouverture de session**

Le fournisseur d’informations d’identification énumère les vignettes d’ouverture de session dans les cas suivants :

-   Pour ces systèmes d’exploitation désignées dans la **s’applique à** figurant au début de cette rubrique.

-   Le fournisseur d’informations d’identification énumère les vignettes pour la connexion de la station de travail. Le fournisseur d’informations d’identification sérialise en général, les informations d’identification pour l’authentification à l’autorité de sécurité locale. Ce processus affiche des vignettes spécifiques pour chaque utilisateur et spécifiques aux systèmes de cible de chaque utilisateur.

-   L’architecture d’ouverture de session et authentification permet à un utilisateur de vignettes énumérées par le fournisseur d’informations d’identification permet de déverrouiller une station de travail. En règle générale, l’utilisateur actuellement connecté est la vignette par défaut, mais si plus d’un utilisateur est connecté, nombreuses vignettes sont affichés.

-   Le fournisseur d’informations d’identification énumère des vignettes en réponse à une demande de l’utilisateur à modifier son mot de passe ou d’autres informations privées, comme un code confidentiel. En règle générale, l’utilisateur actuellement connecté est la vignette par défaut ; Toutefois, si plus d’un utilisateur est connecté, nombreuses vignettes sont affichés.

-   Le fournisseur d’informations d’identification énumère les vignettes basées sur les informations d’identification sérialisées pour être utilisé pour l’authentification sur des ordinateurs distants. Informations d’identification de l’interface utilisateur n’utilisent pas la même instance du fournisseur en tant que l’interface utilisateur d’ouverture de session, déverrouillez la station de travail ou modifier le mot de passe. Par conséquent, les informations d’état ne peuvent pas être conservées dans le fournisseur entre des instances de l’interface utilisateur des informations d’identification. Cette structure entraîne une mosaïque pour chaque ordinateur distant d’ouverture de session, en supposant que les informations d’identification ont été correctement sérialisées. Ce scénario est également utilisé dans compte contrôle utilisateur (UAC), ce qui peut aider à empêcher les modifications non autorisées sur un ordinateur en invitant l’utilisateur pour l’autorisation ou un mot de passe administrateur avant d’autoriser des actions susceptibles d’affecter le fonctionnement de l’ordinateur ou qui peut modifier les paramètres qui affectent les autres utilisateurs de l’ordinateur.

Le diagramme suivant illustre le processus d’informations d’identification pour les systèmes d’exploitation désignée dans la **s’applique à** figurant au début de cette rubrique.

![Diagramme illustrant le processus d’informations d’identification pour les systèmes d’exploitation désignée dans la ** s’applique à ** figurant au début de cette rubrique](../media/credentials-processes-in-windows-authentication/AuthN_CredMan_CredProv.gif)

## <a name="BKMK_CredentialInputForApplicationAndServiceLogon"></a>Entrée d’informations d’identification pour l’application et de la connexion au service
L’authentification Windows est conçue pour gérer les informations d’identification pour les applications ou services qui ne nécessitent pas d’intervention de l’utilisateur. Applications en mode utilisateur sont limitées en termes de quelles ressources système qu’ils ont accès, tandis que les services peuvent avoir un accès illimité à la mémoire système et les périphériques externes.

Services système et des applications de niveau transport accéder à un fournisseur SSP (Security Support Provider) via la prise en charge Interface SSPI (Security Provider) dans Windows, qui fournit des fonctions pour énumérer les packages de sécurité disponibles sur un système, en sélectionnant un package et à l’aide de ce package pour obtenir une connexion authentifiée.

Lorsqu’une connexion client/serveur est authentifiée :

-   L’application côté client de la connexion envoie les informations d’identification au serveur en utilisant la fonction SSPI `InitializeSecurityContext (General)`.

-   L’application côté serveur de la connexion répond avec la fonction SSPI `AcceptSecurityContext (General)`.

-   Les fonctions SSPI `InitializeSecurityContext (General)` et `AcceptSecurityContext (General)` sont répétées jusqu'à ce que tous les messages d’authentification nécessaires ont été échangés pour la réussite ou l’échec d’authentification.

-   Une fois la connexion a été authentifiée, l’autorité LSA sur le serveur utilise les informations à partir du client pour générer le contexte de sécurité, qui contient un jeton d’accès.

-   Le serveur peut appeler ensuite la fonction SSPI `ImpersonateSecurityContext` pour joindre le jeton d’accès à un thread d’emprunt d’identité pour le service.

**Applications et le mode utilisateur**

Mode utilisateur dans Windows se compose de deux systèmes capables de transférer les demandes d’e/s pour les pilotes en mode noyau appropriée : le système de l’environnement, qui exécute des applications écrites pour différents types de systèmes d’exploitation, et le système intégraux, qui fonctionne fonctions spécifiques au système pour le compte du système de l’environnement.

Le système intégraux gère les fonctions system'specific d’exploitation pour le compte du système de l’environnement et se compose d’un processus de système de sécurité (l’autorité LSA), un service station de travail et un service de serveur. Le processus de système de sécurité porte sur les jetons de sécurité accorde ou refuse des autorisations pour accéder aux comptes d’utilisateur en fonction des autorisations de ressources, gère les demandes d’ouverture de session et lance l’authentification d’ouverture de session et détermine les ressources système du système d’exploitation Il faut auditer.

Applications peuvent s’exécuter en mode utilisateur où elle peut s’exécuter en tant que n’importe quel principal, y compris dans le contexte de sécurité du système (système Local). Applications peuvent également fonctionner en mode noyau où elle peut s’exécuter dans le contexte de sécurité du système (système Local).

SSPI est disponible via le module Secur32.dll, qui est une API utilisée pour obtenir des services de sécurité intégrée pour l’authentification, l’intégrité des messages et confidentialité des messages. Il fournit une couche d’abstraction entre les protocoles de sécurité et des protocoles au niveau de l’application. Différentes applications nécessitant des différentes façons d’identifier ou d’authentifier les utilisateurs et les différentes façons de crypter les données lorsqu’elles transitent sur un réseau, SSPI permet d’accéder aux bibliothèques de liens dynamiques (DLL) qui contiennent une authentification différents et les fonctions de chiffrement. Ces DLL est appelés fournisseurs de prise en charge de sécurité (SSP).

Comptes de service administrés et les comptes virtuels ont été introduits dans Windows Server 2008 R2 et Windows 7 pour fournir des applications cruciales, telles que Microsoft SQL Server et Internet Information Services (IIS), avec l’isolation de leurs propres comptes de domaine, tandis que éliminer le besoin d’un administrateur pour administrer manuellement le nom de principal du service (SPN) et les informations d’identification pour ces comptes. Pour plus d’informations sur ces fonctionnalités et leur rôle dans l’authentification, consultez [Managed Service comptes Documentation pour Windows 7 et Windows Server 2008 R2](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx) et [groupe Managed Service présentation des comptes](../group-managed-service-accounts/group-managed-service-accounts-overview.md).

**En mode noyau et les services**

Bien que la plupart des applications Windows s’exécutent dans le contexte de sécurité de l’utilisateur qui démarre les, cela n’est pas vrai des services. De nombreux services Windows, telles que le réseau et les services d’impression sont démarrés par le contrôleur de service lorsque l’utilisateur démarre l’ordinateur. Ces services peuvent s’exécuter en tant que Service Local ou système Local et peuvent continuer à exécuter après que le dernier utilisateur humain se déconnecte.

> [!NOTE]
> Services s’exécutent normalement dans les contextes de sécurité connus en tant que système Local (système), Service réseau ou Service Local.  Windows Server 2008 R2 a introduit les services qui s’exécutent sous un compte de service géré, et qui sont des entités de domaine.

Avant de démarrer un service, le service de contrôleur se connecte en utilisant le compte est désigné pour le service, et présente ensuite les informations d’identification du service d’authentification par l’autorité LSA. Le service Windows implémente une interface de programmation qui permet de contrôler le service par le Gestionnaire de contrôle de service. Un service Windows peut être démarré automatiquement lorsque le système est démarré ou manuellement avec un programme de contrôle de service. Par exemple, lorsqu’un ordinateur de client Windows rejoint un domaine, le service messenger sur l’ordinateur se connecte à un contrôleur de domaine et ouvre un canal sécurisé vers elle. Pour obtenir une connexion authentifiée, le service doit disposer des informations d’identification que les approbations de l’autorité de sécurité locale (LSA) de l’ordinateur distant. Lors de la communication avec d’autres ordinateurs du réseau, LSA utilise les informations d’identification pour le compte de domaine de l’ordinateur local, que tous les autres services s’exécutant dans le contexte de sécurité du système Local et Service réseau. Services sur l’ordinateur local s’exécutent en tant que système, informations d’identification n’avez donc pas à présenter à l’autorité LSA.

Le fichier Ksecdd.sys gère et de chiffre ces informations d’identification et utilise un appel de procédure locale dans l’autorité LSA. Le type de fichier est DRV (pilote) et qu’il est connu comme le fournisseur SSP (Security Support Provider) en mode noyau et, dans ces versions désignées dans la **s’applique à** liste au début de cette rubrique, est FIPS 140-2 niveau 1-conforme.

En mode noyau a un accès complet aux ressources système et de matériel de l’ordinateur. Le mode noyau arrête les services en mode utilisateur et les applications d’accéder à des zones critiques du système d’exploitation qu’ils ne doivent pas avoir accès à.

## <a name="BKMK_LSA"></a>Autorité de sécurité locale
L’autorité de sécurité locale (LSA) est un processus système protégé qui authentifie des utilisateurs et une session sur l’ordinateur local. En outre, LSA conserve les informations sur tous les aspects de la sécurité locale sur un ordinateur (ces aspects sont collectivement regroupés sous la stratégie de sécurité locale), et il fournit divers services de traduction entre les noms et les identificateurs de sécurité (SID). Le processus de système de sécurité, LSASS Local Security Authority Server Service (), effectue le suivi des stratégies de sécurité et les comptes qui sont en vigueur sur un système informatique.

L’autorité LSA valide l’identité d’un utilisateur en fonction duquel des deux entités suivantes émises le compte d’utilisateur :

-   **Autorité de sécurité locale.** L’autorité LSA peut valider les informations utilisateur en vérifiant la base de données du Gestionnaire de comptes de sécurité (SAM) situé sur le même ordinateur. Toute station de travail ou un serveur membre peut stocker les comptes d’utilisateurs locaux et des informations sur les groupes locaux. Toutefois, ces comptes peuvent être utilisés pour accéder à la seule cette station de travail ou un ordinateur.

-   **Autorité de sécurité pour le domaine local ou pour un domaine approuvé.** L’autorité LSA contacte l’entité qui a émis le compte et la vérification de demandes que le compte est valide et que la demande provient le titulaire du compte.

Le service LSASS (Local Security Authority Subsystem Service) stocke en mémoire les informations d'identification pour le compte des utilisateurs avec les sessions Windows actives. Les informations d’identification stockées permettent aux utilisateurs d’accéder facilement aux ressources réseau, telles que les partages de fichiers, les boîtes aux lettres Exchange Server et les sites SharePoint, sans entrer à nouveau leurs informations d’identification pour chaque service à distance.

Le service LSASS peut stocker les informations d'identification sous des formes multiples, dont notamment :

-   Texte en clair chiffré réversible

-   Tickets Kerberos (ticket-granting tickets TGT, tickets de service)

-   Hachage NT

-   Hachage LAN Manager (LM)

Si l’utilisateur ouvre une session Windows à l’aide d’une carte à puce, LSASS ne stocke pas d’un mot de passe en texte clair, mais elle stocke la valeur de hachage NT correspondante pour le compte et le texte en clair de code PIN pour la carte à puce. Si l'attribut de compte est activé pour une carte à puce requise pour une ouverture de session interactive, une valeur de hachage NT aléatoire est générée automatiquement pour le compte à la place du hachage de mot de passe d'origine. Le hachage de mot de passe généré automatiquement quand l'attribut est défini ne change pas.

Si un utilisateur ouvre une session un ordinateur Windows avec un mot de passe qui est compatible avec les hachages LAN Manager (LM), cet authentificateur est présent dans la mémoire.

Le stockage des informations d'identification en texte en clair dans la mémoire ne peut pas être désactivé, même si les fournisseurs d'informations d'identification qui en ont besoin sont désactivés.

Les informations d’identification stockées sont directement associées les sessions d’ouverture de session de sécurité autorité Service LSASS (Local Subsystem) qui ont été démarrées après le dernier redémarrage et n’ont pas été fermées. Par exemple, des sessions LSA sont créées avec des informations d'identification LSA stockées lorsqu'un utilisateur effectue l'une des opérations suivantes :

-   Se connecte à une session locale ou une session de protocole RDP (Remote Desktop) sur l’ordinateur

-   Exécution d'une tâche en utilisant l'option **RunAs**

-   Exécution d'un service Windows actif sur l'ordinateur

-   Exécution d'une tâche planifiée ou d'un traitement par lots planifié

-   Exécution d'une tâche sur l'ordinateur local à l'aide d'un outil d'administration à distance

Dans certains cas, les secrets LSA, qui sont des éléments secrets de données qui sont uniquement accessibles aux processus du compte système, sont stockés sur le lecteur de disque dur. Certains de ces secrets sont des informations d'identification qui doivent être conservées après le redémarrage. Ils sont stockés sous forme chiffrée sur le lecteur de disque dur. Les informations d'identification stockées en tant que secrets LSA peuvent inclure les éléments suivants :

-   Mot de passe de compte pour le compte de Services de domaine Active Directory (AD DS) de l’ordinateur

-   Mots de passe de compte utilisés pour les services Windows configurés sur l'ordinateur

-   Mots de passe de compte utilisés pour les tâches planifiées configurées

-   Mots de passe de compte utilisés pour les sites web et les pools d'applications IIS

-   Mots de passe pour les comptes Microsoft

Introduite dans Windows 8.1, le système d’exploitation client offre une protection supplémentaire pour l’autorité LSA empêcher la lecture de la mémoire et l’injection de code par des processus non protégés. Cette protection accroît la sécurité pour les informations d’identification que l’autorité LSA stocke et gère.

Pour plus d’informations sur ces protections supplémentaires, consultez [Configuring Additional LSA Protection](../credentials-protection-and-management/configuring-additional-lsa-protection.md).

## <a name="BKMK_CachedCredentialsAndValidation"></a>Validation et les informations d’identification mises en cache
Mécanismes de validation s’appuient sur la présentation des informations d’identification au moment de l’ouverture de session. Toutefois, lorsque l’ordinateur est déconnecté d’un contrôleur de domaine, et que l’utilisateur présente des informations d’identification de domaine, Windows utilise le processus d’informations d’identification mises en cache dans le mécanisme de validation.

Chaque fois qu’un utilisateur ouvre une session sur un domaine, Windows met en cache les informations d’identification fournies et les stocke dans la ruche de sécurité dans le Registre du système d’exploitation.

Avec les informations d’identification mises en cache, l’utilisateur pouvez vous connecter à un membre de domaine sans être connecté à un contrôleur de domaine dans ce domaine.


## <a name="BKMK_CredentialStorageAndValidation"></a>Stockage des informations d’identification et validation
Il n’est pas toujours souhaitable d’utiliser un ensemble d’informations d’identification pour l’accès à différentes ressources. Par exemple, un administrateur peut vouloir qu’à utiliser d’administration plutôt que des informations d’identification utilisateur lors de l’accès à un serveur distant. De même, si un utilisateur accède à des ressources externes, tel qu’un compte bancaire, il ou elle peut uniquement utiliser les informations d’identification différents de leurs informations d’identification de domaine. Les sections suivantes décrivent les différences dans la gestion des informations d’identification entre les versions actuelles des systèmes d’exploitation de Windows et les systèmes d’exploitation Windows Vista et Windows XP.

### <a name="remote-logon-credential-processes"></a>Processus des informations d’identification d’ouverture de session à distance
Le protocole RDP (Remote Desktop) gère les informations d’identification de l’utilisateur qui se connecte à un ordinateur distant à l’aide du Client Bureau à distance, ce qui a été introduite dans Windows 8. Les informations d’identification en texte clair sont envoyées à l’hôte cible où l’hôte tente d’effectuer le processus d’authentification et, en cas de réussite, l’utilisateur connecte aux ressources autorisées. RDP ne stocke pas les informations d’identification sur le client, mais les informations d’identification de domaine de l’utilisateur sont stockées dans le processus LSASS.

Introduits dans Windows Server 2012 R2 et Windows 8.1, le mode d’administration restreinte fournit une sécurité supplémentaire pour les scénarios d’ouverture de session à distance. Ce mode de bureau à distance, l’application cliente pour effectuer une réseau d’ouverture de session stimulation / réponse avec la fonction à sens unique NT (NTOWF) ou utiliser un ticket de service Kerberos pour l’authentification à l’hôte distant. Une fois que l’administrateur est authentifié, l’administrateur n’a pas les informations d’identification de compte respectifs dans le service LSASS, car ils n’ont pas été fournies à l’hôte distant. Au lieu de cela, l’administrateur a les informations d’identification du compte ordinateur pour la session. Informations d’identification d’administrateur ne sont pas fournies à l’hôte distant, actions sont effectuées en tant que le compte d’ordinateur. Ressources sont également limitées au compte d’ordinateur, et l’administrateur ne peut pas accéder aux ressources avec son propre compte.

### <a name="automatic-restart-sign-on-credential-process"></a>Processus de l’authentification d’informations d’identification de redémarrage automatique
Lorsqu’un utilisateur se connecte sur un appareil Windows 8.1, LSA enregistre les informations d’identification de l’utilisateur dans la mémoire chiffrée qui sont accessibles uniquement par LSASS.exe. Lors de la mise à jour Windows lance un redémarrage automatique sans la présence de l’utilisateur, ces informations d’identification sont utilisées pour configurer l’ouverture de session automatique pour l’utilisateur.

Lors du redémarrage, l’utilisateur est automatiquement connecté via le mécanisme d’ouverture de session automatique et l’ordinateur est en outre verrouillé pour protéger la session utilisateur. Le verrouillage est lancé via Winlogon, tandis que la gestion d’informations d’identification est effectuée par la LSA. En vous connectant automatiquement et le verrouillage de la session utilisateur sur la console, applications d’écran de verrouillage de l’utilisateur est redémarré et disponible.

Pour plus d’informations sur ARSO, consultez [Winlogon automatique redémarrer Sign-On &#40;ARSO&#41;](winlogon-automatic-restart-sign-on-arso.md).

### <a name="stored-user-names-and-passwords-in-windows-vista-and-windows-xp"></a>Noms d’utilisateurs et mots de passe dans Windows Vista et Windows XP
Dans Windows Server 2008, Windows Server 2003, Windows Vista et Windows XP, **stockées noms d’utilisateur et mots de passe** dans le panneau de configuration simplifie la gestion et l’utilisation de plusieurs jeux d’informations d’identification de l’ouverture de session, y compris les certificats X.509 utilisés avec les cartes à puce et les informations d’identification en direct Windows (maintenant appelées compte Microsoft). Les informations d’identification - partie du profil de l’utilisateur - sont stockées jusqu'à ce que nécessaire. Cette action permet de renforcer la sécurité sur une base par ressource en veillant à ce que si un mot de passe est compromis, il ne compromet pas toute la sécurité.

Après qu’un utilisateur se connecte et tente d’accéder aux ressources supplémentaires protégé par mot de passe, tel qu’un partage sur un serveur, et si les informations d’identification d’ouverture de session de l’utilisateur par défaut ne sont pas suffisantes pour y accéder, **stockées noms d’utilisateur et mots de passe** est interrogée . Si les autres informations d’identification avec les informations d’ouverture de session corrects ont été enregistrées dans **stockées noms d’utilisateur et mots de passe**, ces informations d’identification servent à accéder. Sinon, l’utilisateur est invité à fournir de nouvelles informations d’identification, qui peuvent ensuite être enregistrées pour une réutilisation, plus loin dans l’ouverture de session ou pendant une session suivante.

Les restrictions suivantes s’appliquent :

-   Si **stockées noms d’utilisateur et mots de passe** contient des informations d’identification non valides ou incorrectes pour une ressource spécifique, l’accès à la ressource est refusée et le **stockées noms d’utilisateur et mots de passe** n’est pas le cas de boîte de dialogue Apparaître.

-   **Stockées des noms d’utilisateur et mots de passe** stocke des informations d’identification uniquement pour NTLM, le protocole Kerberos, le compte Microsoft (anciennement Windows Live ID) et l’authentification Secure Sockets Layer (SSL). Certaines versions d’Internet Explorer conservent leur propre cache pour l’authentification de base.

Ces informations d’identification deviennent une partie chiffrée d’un profil d’utilisateur local dans le répertoire de se Data\Microsoft\Credentials \Documents and. Par conséquent, ces informations d’identification peuvent se déplacent avec l’utilisateur si la stratégie de réseau de l’utilisateur prend en charge les profils utilisateur itinérants. Toutefois, si l’utilisateur dispose des copies de **stockées noms d’utilisateur et mots de passe** sur deux ordinateurs différents et les modifications les informations d’identification qui sont associées à la ressource sur l’un de ces ordinateurs, la modification n’est pas propagée à  **Stockées des noms d’utilisateur et mots de passe** sur le deuxième ordinateur.

### <a name="windows-vault-and-credential-manager"></a>Coffre de Windows et le Gestionnaire d’informations d’identification
Le Gestionnaire d’informations d’identification a été introduit dans Windows Server 2008 R2 et Windows 7 comme une fonctionnalité le panneau de configuration pour stocker et gérer des noms d’utilisateur et mots de passe. Le Gestionnaire d’informations d’identification permet aux utilisateurs de stocker des informations d’identification appropriées à d’autres systèmes et les sites Web dans le coffre Windows sécurisé. Certaines versions d’Internet Explorer utilisent cette fonctionnalité pour l’authentification à des sites Web.

La gestion des informations d’identification à l’aide du Gestionnaire d’informations d’identification est contrôlée par l’utilisateur sur l’ordinateur local. Les utilisateurs peuvent enregistrer et stocker des informations d’identification à partir des navigateurs et des applications Windows pris en charge de manière à ce qu’il leur soit plus simple de se connecter à ces ressources. Informations d’identification sont enregistrées dans des dossiers chiffrés spéciaux sur l’ordinateur sous le profil utilisateur. Les applications qui prennent en charge cette fonctionnalité (via l’utilisation de l’API Credential Manager), telles que les navigateurs web et applications, peuvent présenter les informations d’identification correctes à d’autres ordinateurs et les sites Web pendant le processus d’ouverture de session.

Quand un site Web, une application ou un autre ordinateur demande une authentification via le protocole Kerberos ou NTLM, une boîte de dialogue s’affiche dans laquelle vous sélectionnez le **mise à jour les informations d’identification par défaut** ou **enregistrer le mot de passe**case à cocher. Cette boîte de dialogue qui permet à un utilisateur d’enregistrer les informations d’identification localement est générée par une application qui prend en charge les API du Gestionnaire d’informations d’identification. Si l’utilisateur sélectionne le **enregistrer le mot de passe** case à cocher, le Gestionnaire d’informations d’identification effectue le suivi de l’utilisateur nom d’utilisateur, mot de passe et des informations associées pour le service d’authentification qui est en cours d’utilisation.

La prochaine fois que le service est utilisé, le Gestionnaire d’informations d’identification fournit automatiquement les informations d’identification sont stockées dans le coffre Windows. Si ces informations sont refusées, l’utilisateur est invité à fournir les informations d’accès correctes. Si l’accès est accordé avec les nouvelles informations d’identification, le Gestionnaire d’informations d’identification remplace les informations précédentes par le nouveau, puis stocke les nouvelles informations d’identification dans le coffre Windows.

## <a name="BKMK_SAM"></a>Base de données du Gestionnaire de comptes de sécurité
Le Gestionnaire de comptes de sécurité (SAM) est une base de données qui stocke des groupes et comptes d’utilisateurs locaux. Il est présent dans chaque système d’exploitation Windows ; Toutefois, lorsqu’un ordinateur est joint à un domaine, Active Directory gère les comptes de domaine dans les domaines Active Directory.

Par exemple, les ordinateurs clients exécutant un système d’exploitation de Windows participent à un domaine de réseau en communiquant avec un contrôleur de domaine, même lorsque aucun utilisateur humain n’est connecté. Pour établir des communications, l’ordinateur doit avoir un compte actif dans le domaine. Avant d’accepter les communications à partir de l’ordinateur, l’autorité LSA sur le contrôleur de domaine authentifie l’identité de l’ordinateur et construit ensuite le contexte de sécurité de l’ordinateur comme il le fait pour un principal de sécurité humaine. Ce contexte de sécurité définit l’identité et les fonctionnalités d’un utilisateur ou un service sur un ordinateur particulier ou un utilisateur, un service ou un ordinateur sur un réseau. Par exemple, le jeton d’accès contenu dans le contexte de sécurité définit les ressources (par exemple, un partage de fichiers ou une imprimante) qui sont accessibles et les actions (par exemple, lecture, écriture ou de modification) qui peuvent être effectuées par ce principal - un utilisateur, un ordinateur ou un service sur ce ressource.

Le contexte de sécurité d’un utilisateur ou un ordinateur peut varier d’un ordinateur vers un autre, par exemple quand un utilisateur ouvre une session sur un serveur ou une station de travail autre que la station de travail principal de l’utilisateur. Elle peut également varier d’une session à un autre, par exemple quand un administrateur modifie les droits et autorisations de l’utilisateur. En outre, le contexte de sécurité est généralement différent lorsqu’un utilisateur ou ordinateur fonctionne de manière autonome, dans un réseau, ou en tant que partie d’un domaine Active Directory.

## <a name="BKMK_LocalDomainsAndTrustedDomains"></a>Les domaines locaux et des domaines approuvés
Lorsqu’il existe une relation d’approbation entre deux domaines, les mécanismes d’authentification pour chaque domaine s’appuient sur la validité des authentifications en provenance de l’autre domaine. Approuve aide afin de fournir un accès contrôlé aux ressources partagées dans un domaine de ressources (le domaine d’approbation) en vérifiant que l’authentification entrante demandes proviennent d’une autorité approuvée (le domaine approuvé). De cette façon, les approbations servent de ponts qui permettent uniquement validé le voyage de demandes d’authentification entre domaines.

Façon dont une approbation spécifique transmet les demandes d’authentification dépend de la façon dont elle est configurée. Relations d’approbation peuvent être unidirectionnelle, en fournissant un accès aux ressources du domaine d’approbation du domaine approuvé ou bidirectionnelle, en fournissant un accès aux ressources dans l’autre domaine de chaque domaine. Les approbations sont également non transitive, auquel cas une relation d’approbation existe uniquement entre les domaines de partenaires de confiance de deux, ou transitive, auquel cas une approbation étend automatiquement vers d’autres domaines qui fait confiance à celui des partenaires.

Pour plus d’informations sur les relations d’approbation de forêt et domaine concernant l’authentification, consultez [de l’authentification déléguée et de relations d’approbation](https://technet.microsoft.com/library/dn169022.aspx).

## <a name="BKMK_CertificatesInWindowsAuthentication"></a>Certificats de l’authentification Windows
Une infrastructure à clé publique (PKI) est la combinaison de logiciels, les technologies de chiffrement, les processus et les services qui permettent à une organisation de sécuriser ses communications et les transactions commerciales. La possibilité d’une infrastructure à clé publique pour sécuriser les communications et les transactions commerciales repose sur l’échange de certificats numériques entre des utilisateurs authentifiés et des ressources approuvées.

Un certificat numérique est un document électronique qui contient des informations sur l’entité à que laquelle il appartient, l’entité qu’il a été émis par, un numéro de série unique ou certaines autres identification unique, d’émission et dates d’expiration et une empreinte numérique.

L’authentification est le processus permettant de déterminer si un hôte distant est digne de confiance. Pour établir sa fiabilité, l’hôte distant doit fournir un certificat d’authentification acceptable.

Hôtes distants établissent leur fiabilité en obtenant un certificat auprès d’une autorité de certification (CA). L’autorité de certification peut, à son tour, dotées d’une autorité plus élevée, ce qui crée une chaîne d’approbation. Pour déterminer si un certificat est digne de confiance, une application doit déterminer l’identité de l’autorité de certification racine, puis déterminer si elle est digne de confiance.

De même, l’ordinateur local ou un hôte distant doit déterminer si le certificat présenté par l’utilisateur ou l’application est authentique. Le certificat présenté par l’utilisateur via l’autorité LSA et le SSPI est évalué pour authenticité sur l’ordinateur local pour l’ouverture de session locale, sur le réseau ou sur le domaine via les magasins de certificats dans Active Directory.

Pour produire un certificat, les données d’authentification passe par le biais des algorithmes de hachage, tels que Secure Hash Algorithm 1 (SHA1), pour produire une synthèse des messages. La synthèse du message est ensuite numériquement signée par à l’aide de la clé privée de l’expéditeur pour prouver que la synthèse du message a été produite par l’expéditeur.

> [!NOTE]
> SHA1 est la valeur par défaut dans Windows 7 et Windows Vista, mais a été remplacé par SHA2 dans Windows 8.

**Authentification de carte à puce**

Technologie de carte à puce est un exemple d’authentification par certificat. Ouvrir une session sur un réseau avec une carte à puce de fournit un mode d’authentification puissante, car elle utilise une identification basée sur le chiffrement et la preuve de possession lors de l’authentification d’un utilisateur à un domaine. Services de certificats Active Directory (AD CS) fournit l’identification en fonction du chiffrement via l’émission d’un certificat d’ouverture de session pour chaque carte à puce.

Pour plus d’informations sur l’authentification par carte à puce, consultez le [référence technique de carte à puce Windows](https://technet.microsoft.com/library/ff404297.aspx).

Technologie de carte à puce virtuelle a été introduite dans Windows 8. Il stocke le certificat de la carte à puce dans le PC et puis de le protéger à l’aide de puce de sécurité de l’appareil falsification du Module de plateforme sécurisée (TPM). De cette façon, le PC devient réellement la carte à puce qui doivent recevoir le code PIN de l’utilisateur afin d’être authentifié.

**Authentification à distance et sans fil**

L’authentification réseau à distance et sans fil est une autre technologie qui utilise des certificats pour l’authentification. Le Service d’authentification Internet (IAS) et les serveurs de réseau privé virtuel permet de Extensible Authentication Protocol-Transport Level Security (EAP-TLS), Extensible Authentication Protocol PEAP (Protected) ou Internet Protocol security (IPsec) effectuer l’authentification basée sur certificat pour de nombreux types d’accès réseau, y compris les connexions sans fil et réseau privé virtuel (VPN).

Pour plus d’informations sur l’authentification par certificat sur le réseau, consultez [réseau de l’authentification d’accès et les certificats](https://technet.microsoft.com/library/cc759575(WS.10).aspx).

## <a name="BKMK_SeeAlso"></a>Voir aussi
[Concepts de l’authentification Windows](https://technet.microsoft.com/library/d169018.aspx)


