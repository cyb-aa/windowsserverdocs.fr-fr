---
title: "Processus des informations d’identification dans l’authentification Windows"
description: "Sécurité de Windows Server"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="credentials-processes-in-windows-authentication"></a>Processus des informations d’identification dans l’authentification Windows

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique de référence pour les professionnels de l’informatique décrit la façon dont l’authentification Windows traite les informations d’identification.

Gestion des informations d’identification Windows est le processus par lequel le système d’exploitation reçoit les informations d’identification du service ou l’utilisateur et permet de sécuriser ces informations pour les future présentation à la cible d’authentification. Dans le cas d’un ordinateur joint au domaine, la cible d’authentification est le contrôleur de domaine. Les informations d’identification utilisées dans l’authentification sont des documents numériques qui associent l’identité de l’utilisateur en une forme de preuve d’authenticité, par exemple, un certificat, un mot de passe ou un code confidentiel.

Par défaut, les informations d’identification Windows sont validées par rapport à la base de données du Gestionnaire de comptes de sécurité (SAM) sur l’ordinateur local ou dans ActiveDirectory sur un ordinateur joint au domaine, via le service Winlogon. Informations d’identification sont collectées par le biais de l’entrée d’utilisateur sur l’interface utilisateur d’ouverture de session ou par programmation via l’interface de programmation d’application (API) de façon à être présentés à la cible d’authentification.

Informations de sécurité locale sont stockées dans le Registre sous **HKEY_LOCAL_MACHINE\SECURITY**. Les informations stockées incluent des paramètres de stratégie, les valeurs de sécurité par défaut et les informations de compte, telles que les informations d’identification mises en cache d’ouverture de session. Une copie de la base de données SAM est également stockée ici, même s’il est protégé.

Le diagramme suivant illustre les composants requis et les chemins d’accès que les informations d’identification prendront via le système pour authentifier l’utilisateur ou le processus pour une ouverture de session réussie.

![Diagramme illustre les composants requis et les chemins d’accès que les informations d’identification prendront via le système pour authentifier l’utilisateur ou le processus pour une ouverture de session réussie.](../media/credentials-processes-in-windows-authentication/AuthN_LSA_Architecture_Client.gif)

Le tableau suivant décrit chaque composant qui gère les informations d’identification dans le processus d’authentification au point d’ouverture de session.

**Composants d’authentification pour tous les systèmes**

|Composant|Description|
|-------|--------|
|Ouverture de session utilisateur|Winlogon.exe est le fichier exécutable chargé de gérer les interactions utilisateur sécurisé. Le service Winlogon lance le processus d’ouverture de session pour les systèmes d’exploitation Windows en transmettant les informations d’identification collectées par action de l’utilisateur sur le bureau sécurisé (l’interface utilisateur d’ouverture de session) pour l’autorité de sécurité locale (LSA) par le biais de Secur32.dll.|
|Connexion de l’application|L’application ou service ouvertures de session qui ne nécessitent pas l’ouverture de session interactive. La plupart des processus lancé par l’utilisateur à l’aide de Secur32.dll tandis que le processus lancées au démarrage, tels que les services, s’exécutent en mode noyau à l’aide de Ksecdd.sys d’exécution en mode utilisateur.<br /><br />Pour plus d’informations sur le mode utilisateur et en mode noyau, voir les Applications et Mode utilisateur ou de Services et en Mode noyau dans cette rubrique.|
|Secur32.dll|Plusieurs fournisseurs d’authentification qui traitent de la base de l’authentification.|
|Lsasrv.dll|Le service serveur LSA, qui agit comme le Gestionnaire de package de sécurité pour l’autorité LSA et applique les stratégies de sécurité. L’autorité LSA contient la fonction Negotiate, qui sélectionne le protocole Kerberos ou NTLM après avoir déterminé le protocole est aboutisse.|
|Prise en charge les fournisseurs de sécurité|Un ensemble de fournisseurs qui peuvent appeler individuellement un ou plusieurs protocoles d’authentification. L’ensemble de fournisseurs par défaut permettre modifier avec chaque version du système d’exploitation Windows et des fournisseurs personnalisés peuvent être écrits.|
|Netlogon.dll|Les services qui exécute le service Net Logon sont les suivantes:<br /><br />-Gère le canal l’ordinateur sécurisé (afin de ne pas confondre avec Schannel) à un contrôleur de domaine.<br />-Transmet les informations d’identification de l’utilisateur via un canal sécurisé au contrôleur de domaine et retourne les identificateurs de sécurité (SID) du domaine et les droits d’utilisateur pour l’utilisateur.<br />-Publie des enregistrements de ressource de service dans le système DNS (Domain Name) et utilise DNS pour résoudre les noms pour les adresses IP (Internet Protocol) des contrôleurs de domaine.<br />-Implémente le protocole de la réplication basé sur l’appel de procédure distante (RPC) pour la synchronisation des contrôleurs de domaine principal (PDC) et les contrôleurs de domaine (CSD).|
|Samsrv.dll|Le Gestionnaire de SAM (Security Accounts), qui stocke les comptes de sécurité locaux, applique les stratégies stockées localement et prend en charge des API.|
|Registre|Le Registre contient une copie des SAM de base de données, paramètres de stratégie de sécurité locale, les valeurs de sécurité par défaut et les informations de compte ne sont pas accessibles au système.|

Cette rubrique contient les sections suivantes:

-   [Entrée d’informations d’identification d’ouverture de session utilisateur](#BKMK_CrentialInputForUserLogon)

-   [Entrée d’informations d’identification d’ouverture de session de service et d’application](#BKMK_CredentialInputForApplicationAndServiceLogon)

-   [Autorité de sécurité locale](#BKMK_LSA)

-   [Validation et des informations d’identification mises en cache](#BKMK_CachedCredentialsAndValidation)

-   [Stockage d’informations d’identification et la validation](#BKMK_CredentialStorageAndValidation)

-   [Base de données du Gestionnaire de comptes de sécurité](#BKMK_SAM)

-   [Les domaines locaux et des domaines approuvés](#BKMK_LocalDomainsAndTrustedDomains)

-   [Certificats de l’authentification Windows](#BKMK_CertificatesInWindowsAuthentication)

## <a name="BKMK_CrentialInputForUserLogon"></a>Entrée d’informations d’identification d’ouverture de session utilisateur
Dans Windows Server2008 et WindowsVista, l’architecture Graphical Identification and Authentication (GINA) a été remplacé par un modèle de fournisseur d’informations d’identification, ce qui rend possible énumérer les types d’ouverture de session différents par le biais de vignettes d’ouverture de session. Les deux modèles sont décrites ci-dessous.

**Architecture d’authentification et d’Identification graphique**

L’architecture d’authentification (GINA) Graphical Identification and s’applique pour les systèmes d’exploitation Windows Server2003, MicrosoftWindows2000 Server, WindowsXP et Windows2000 Professionnel. Dans ces systèmes, toutes les sessions d’ouverture de session interactive crée une instance distincte du service Winlogon. L’architecture GINA est chargé dans l’espace de processus utilisé par Winlogon, reçoit et traite les informations d’identification et effectue les appels aux interfaces de l’authentification par le biais de LSALogonUser.

Les instances de Winlogon pour une ouverture de session interactive s’exécuter dans la Session 0. Les services système session 0hôtes et les autres processus critiques, y compris le processus de l’autorité de sécurité locale (LSA).

Le diagramme suivant illustre le processus d’informations d’identification pour MicrosoftWindows2000 Professionnel, WindowsXP, Windows Server2003 et MicrosoftWindows2000 Server.

![Diagramme montrant le processus d’informations d’identification pour MicrosoftWindows2000 Professionnel, WindowsXP, Windows Server2003 et MicrosoftWindows2000 Server](../media/credentials-processes-in-windows-authentication/AuthN_GINA_Architecture.gif)

**Architecture du fournisseur d’informations d’identification**

L’architecture du fournisseur d’informations d’identification s’applique à ces versions désignées dans la **s’applique à** figurant au début de cette rubrique. Dans ces systèmes, l’architecture d’entrée d’informations d’identification modifié sur une conception extensible à l’aide des fournisseurs d’informations d’identification. Ces fournisseurs sont représentés par les vignettes d’ouverture de session différente sur le bureau sécurisé qui autorise un nombre quelconque de scénarios d’ouverture de session - comptes différents pour le même utilisateur et les différentes méthodes d’authentification, telles que le mot de passe, carte à puce et la biométrie.

Avec l’architecture du fournisseur d’informations d’identification, Winlogon démarre toujours l’interface utilisateur d’ouverture de session après avoir reçu un événement de séquence de touches de sécurité. L’interface utilisateur d’ouverture de session interroge chaque fournisseur d’informations d’identification pour le nombre de types d’informations d’identification différentes que le fournisseur est configuré pour énumérer. Les fournisseurs d’informations d’identification ont la possibilité de spécifier une des ces vignettes en tant que la valeur par défaut. Une fois que tous les fournisseurs ont énumérés leurs vignettes, l’interface utilisateur d’ouverture de session les affiche à l’utilisateur. L’utilisateur interagit avec une vignette à fournir leurs informations d’identification. L’interface utilisateur d’ouverture de session envoie ces informations d’identification pour l’authentification.

Les fournisseurs d’informations d’identification ne sont pas les mécanismes de contrainte. Ils sont utilisés pour collecter et de sérialiser les informations d’identification. Les packages de l’autorité de sécurité locale et l’authentification appliquent la sécurité.

Les fournisseurs d’informations d’identification sont enregistrés sur l’ordinateur et sont chargés pour les éléments suivants:

-   Décrivant les informations d’identification requises pour l’authentification.

-   Communication de gestion et la logique avec les autorités d’authentification externe.

-   Informations d’identification de la création de packages pour interactive et d’ouverture de session réseau.

Informations d’identification de la création de packages pour interactive et ouverture de session réseau inclut le processus de sérialisation. Lors de la sérialisation des informations d’identification plusieurs vignettes d’ouverture de session peuvent être affichés sur l’interface utilisateur d’ouverture de session. Par conséquent, votre organisation peut contrôler l’affichage d’ouverture de session, telles que les utilisateurs, des systèmes cibles d’ouverture de session, d’ouverture de session préalable accès aux réseau et la station de travail verrouillez/déverrouillez stratégies - par le biais de fournisseurs d’informations d’identification personnalisées. Plusieurs fournisseurs d’informations d’identification peuvent coexister sur le même ordinateur.

Fournisseurs (d’authentification unique SSO) à authentification uniques peuvent être développés sous la forme d’un fournisseur d’informations d’identification standard ou un fournisseur Pre-Logon-Access.

Chaque version de Windows contient le fournisseur d’informations d’identification par défaut et un seul par défaut Pre-Logon-Access fournisseur (PLAP), également connu sous le fournisseur d’authentification unique. Le fournisseur d’authentification unique permet aux utilisateurs d’établir une connexion à un réseau avant de vous connecter à l’ordinateur local. Lorsque ce fournisseur est implémenté, le fournisseur n’énumère pas les vignettes sur l’interface utilisateur d’ouverture de session.

Un fournisseur d’authentification unique est destiné à être utilisé dans les scénarios suivants:

-   **Ouverture de session d’authentification et l’ordinateur réseau sont gérées par les fournisseurs d’informations d’identification différentes.** Variantes de ce scénario sont les suivantes:

    -   Un utilisateur a la possibilité de se connecter à un réseau, telles que la connexion à un réseau privé virtuel (VPN), avant d’ouvrir une session sur l’ordinateur mais n’est pas requis pour effectuer cette connexion.

    -   L’authentification réseau est nécessaire pour extraire les informations utilisées lors de l’authentification interactive sur l’ordinateur local.

    -   Plusieurs tentatives d’authentification réseau sont suivies par l’un des autres scénarios. Par exemple, un utilisateur s’authentifie auprès d’un fournisseur de services Internet (ISP) s’authentifie auprès d’un réseau privé virtuel et utilise ensuite leurs informations d’identification du compte utilisateur pour ouvrir une session localement.

    -   Les informations d’identification mises en cache sont désactivées, et une connexion d’accès à distance via VPN est requise avant l’ouverture de session locale pour authentifier l’utilisateur.

    -   Un utilisateur de domaine ne dispose pas d’un compte local sur un ordinateur joint au domaine et doit établir une connexion d’accès à distance par le biais de connexion VPN avant d’effectuer l’ouverture de session interactive.

-   **Ouverture de session d’authentification et l’ordinateur réseau sont gérées par le même fournisseur d’informations d’identification.** Dans ce scénario, l’utilisateur est nécessaire pour se connecter au réseau avant d’ouvrir une session sur l’ordinateur.

**Énumération de vignette d’ouverture de session**

Le fournisseur d’informations d’identification énumère les vignettes d’ouverture de session dans les cas suivants:

-   Pour ces systèmes d’exploitation désignées dans la **s’applique aux** figurant au début de cette rubrique.

-   Le fournisseur d’informations d’identification énumère les vignettes pour l’ouverture de session. Le fournisseur d’informations d’identification sérialise généralement des informations d’identification pour l’authentification à l’autorité de sécurité locale. Ce processus affiche des vignettes spécifiques pour chaque utilisateur et spécifiques aux systèmes de cible de chaque utilisateur.

-   L’architecture d’ouverture de session et authentification permet à un utilisateur vignettes énumérés par le fournisseur d’informations d’identification permet de déverrouiller une station de travail. En règle générale, l’utilisateur actuellement connecté est la vignette par défaut, mais si plus d’un utilisateur est connecté, les nombreuses vignettes sont affichées.

-   Le fournisseur d’informations d’identification énumère les vignettes en réponse à une demande de l’utilisateur à modifier leur mot de passe ou d’autres informations privées, par exemple, un code confidentiel. En règle générale, l’utilisateur actuellement connecté est la vignette par défaut; toutefois, si plusieurs utilisateurs est connecté, nombreuses vignettes sont affichés.

-   Le fournisseur d’informations d’identification énumère les vignettes basés sur les informations d’identification sérialisées à utiliser pour l’authentification sur des ordinateurs distants. Informations d’identification de l’interface utilisateur n’utilisent pas la même instance du fournisseur de l’interface utilisateur d’ouverture de session, déverrouiller la station de travail ou modifier le mot de passe. Par conséquent, les informations d’état ne peuvent pas être conservées dans le fournisseur entre les instances de l’interface utilisateur d’informations d’identification. Cette structure se traduit par une vignette pour chaque ordinateur distant d’ouverture de session, en supposant que les informations d’identification ont été correctement sérialisées. Ce scénario est également utilisé dans compte contrôle utilisateur (UAC), qui peuvent vous aider à empêcher les modifications non autorisées à un ordinateur en invitant l’utilisateur pour l’autorisation ou un mot de passe administrateur avant d’autoriser des actions susceptibles d’affecter le fonctionnement de l’ordinateur ou qui peut modifier les paramètres qui affectent les autres utilisateurs de l’ordinateur.

Le diagramme suivant illustre le processus d’informations d’identification pour les systèmes d’exploitation désignée dans la **s’applique à** figurant au début de cette rubrique.

![Diagramme illustrant le processus d’informations d’identification pour les systèmes d’exploitation désignée dans la ** s’applique à ** figurant au début de cette rubrique](../media/credentials-processes-in-windows-authentication/AuthN_CredMan_CredProv.gif)

## <a name="BKMK_CredentialInputForApplicationAndServiceLogon"></a>Entrée d’informations d’identification d’ouverture de session de service et d’application
L’authentification Windows est conçue pour gérer les informations d’identification pour les applications ou services qui ne nécessitent pas d’interaction utilisateur. Les applications en mode utilisateur sont limitées en termes de quelles ressources système qu’ils ont accès, tandis que les services peuvent avoir un accès illimité à la mémoire système et les périphériques externes.

Les services système et des applications de niveau transport accéder à un fournisseur SSP (Security Support Provider) via le fournisseur d’Interface SSPI (Security Support) dans Windows, qui fournit des fonctions pour énumérer les packages de sécurité disponibles sur un système, en sélectionnant un package et à l’aide de ce package à obtenir une connexion authentifiée.

Lorsqu’une connexion client/serveur est authentifiée:

-   L’application sur le côté client de la connexion envoie les informations d’identification pour le serveur à l’aide de la fonction SSPI `InitializeSecurityContext (General)`.

-   L’application sur le côté serveur de la connexion répond avec la fonction SSPI `AcceptSecurityContext (General)`.

-   Les fonctions SSPI `InitializeSecurityContext (General)`et `AcceptSecurityContext (General)`sont répété jusqu'à ce que tous les messages d’authentification nécessaires ont été échangés pour la réussite ou l’échec d’authentification.

-   Une fois la connexion a été authentifiée, l’autorité LSA sur le serveur utilise les informations à partir du client pour créer le contexte de sécurité, qui contient un jeton d’accès.

-   Le serveur peut ensuite appeler la fonction SSPI `ImpersonateSecurityContext`pour joindre le jeton d’accès à un thread d’emprunt d’identité pour le service.

**Applications et le mode utilisateur**

Mode utilisateur dans Windows est composé de deux systèmes capables de transférer les demandes d’e/s pour les pilotes en mode noyau appropriée: le système de l’environnement, qui s’exécute les applications écrites pour différents types de systèmes d’exploitation, et le système intégré, ce qui fonctionne de fonctions spécifiques du système pour le compte du système de l’environnement.

Le système gère les fonctions system'specific pour le compte du système de l’environnement et se compose d’un processus de système de sécurité (l’autorité LSA), un service station de travail et un service de serveur. Le processus de système de sécurité traite des jetons de sécurité accorde ou refuse les autorisations d’accès aux comptes d’utilisateur en fonction des autorisations de ressource, gère les demandes d’ouverture de session et lance l’authentification d’ouverture de session et détermine les ressources système du système d’exploitation doit vérifier.

Applications peuvent s’exécuter en mode utilisateur sur lequel l’application peut s’exécuter en tant que n’importe quelle entité, y compris dans le contexte de sécurité du système Local (système). Les applications peuvent également exécuter en mode noyau où l’application peut s’exécuter dans le contexte de sécurité du système Local (système).

L’interface SSPI est disponible via le module Secur32.dll, qui est une API utilisée pour obtenir des services de sécurité intégrée pour l’authentification, l’intégrité des messages et la confidentialité des messages. Il fournit une couche d’abstraction entre les protocoles de sécurité et des protocoles au niveau de l’application. Étant donné que différentes applications nécessitent différentes façons d’identification ou l’authentification des utilisateurs et des méthodes différentes pour le chiffrement des données lors de leur transit sur le réseau, SSPI fournit un moyen d’accéder aux bibliothèques de liens dynamiques (DLL) qui contiennent différente fonctions d’authentification et chiffrement. Ces DLL est appelés fournisseurs de prise en charge de sécurité (SSP).

Service géré comptes et les comptes virtuels ont été introduits dans Windows Server2008R2 et Windows7des applications cruciales, telles que MicrosoftSQLServer et Internet Information Services (IIS), d’isoler leurs propres comptes de domaine, alors qu’en éliminant la nécessité pour un administrateur manuellement administrer le nom principal de service (SPN) et les informations d’identification de ces comptes. Pour plus d’informations sur ces fonctionnalités et leur rôle dans l’authentification, voir [Managed Service comptes Documentation pour Windows7 et Windows Server2008R2](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx) et [groupe Managed Service Accounts Overview](../group-managed-service-accounts/group-managed-service-accounts-overview.md).

**En mode noyau et de services**

Même si la plupart des applications Windows s’exécute dans le contexte de sécurité de l’utilisateur qui les démarre, cela n’est pas vrai pour les services. Nombreux services Windows, tels que les services d’impression et réseau sont démarrés par le contrôleur de service lorsque l’utilisateur démarre l’ordinateur. Ces services peuvent s’exécuter en tant que Service Local ou système Local et peuvent continuer à exécuter après que le dernier utilisateur humain se déconnecte.

> [!NOTE]
> Services s’exécutent normalement dans les contextes de sécurité connus en tant que système Local (système), Service réseau ou Service Local.  Windows Server2008R2 a introduit les services qui s’exécutent sous un compte de service administré, et qui sont des entités de domaine.

Avant de démarrer un service, le contrôleur de service ouvre une session en utilisant le compte qui est désigné pour le service et présente ensuite les informations d’identification du service pour l’authentification par l’autorité LSA. Le service Windows implémente une interface de programmation que le Gestionnaire de contrôle de service permettre utiliser pour contrôler le service. Un service Windows peut être démarré automatiquement lorsque le système est démarré ou manuellement à un programme de contrôle de service. Par exemple, lorsqu’un ordinateur client Windows joint à un domaine, le service messenger sur l’ordinateur se connecte à un contrôleur de domaine et ouvre un canal sécurisé. Pour obtenir une connexion authentifiée, le service doit disposer des informations d’identification qui l’approuve de l’autorité de sécurité locale (LSA) de l’ordinateur distant. Lors de la communication avec d’autres ordinateurs du réseau, LSA utilise les informations d’identification de compte de domaine de l’ordinateur local, comme tous les autres services en cours d’exécution dans le contexte de sécurité du système Local et Service réseau. Les services sur l’ordinateur local s’exécuter en tant que système informations d’identification n’avez pas besoin d’être présentée à l’autorité LSA.

Le fichier Ksecdd.sys gère et chiffre ces informations d’identification et utilise un appel de procédure local dans l’autorité LSA. Le type de fichier est DRV (pilote) et est connu en tant que le fournisseur SSP (Security Support Provider) en mode noyau et, dans les versions désignées dans la **s’applique à** liste au début de cette rubrique, est FIPS 140-2 niveau 1 conforme.

En mode noyau a un accès complet aux ressources système et de matériel de l’ordinateur. Le mode noyau arrête les services en mode utilisateur et les applications d’accéder à des zones critiques du système d’exploitation qu’ils ne doivent pas avoir accès à.

## <a name="BKMK_LSA"></a>Autorité de sécurité locale
L’autorité de sécurité locale (LSA) est un processus système protégé qui authentifie et se connecte aux utilisateurs une session sur l’ordinateur local. En outre, LSA conserve les informations sur tous les aspects de sécurité locale sur un ordinateur (ces aspects sont collectivement regroupés sous la stratégie de sécurité locale), et il fournit divers services de traduction de noms et des identificateurs de sécurité (SID). Le processus du système de sécurité, autorité serveur Service LSASS (Local Security), effectue le suivi des stratégies de sécurité et les comptes qui sont en vigueur sur un ordinateur.

L’autorité LSA valide l’identité d’un utilisateur basée sur lequel des deux entités suivantes émises le compte d’utilisateur:

-   **Autorité de sécurité locale.** L’autorité LSA peut valider les informations de l’utilisateur en vérifiant la base de données du Gestionnaire de comptes de sécurité (SAM) situé sur le même ordinateur. N’importe quel poste de travail ou un serveur membre peut stocker les comptes d’utilisateurs locaux et des informations sur les groupes locaux. Toutefois, ces comptes peuvent être utilisés pour accéder à la seule cette station de travail ou ordinateur.

-   **Autorité de sécurité pour le domaine local ou un domaine approuvé.** L’autorité LSA contacte l’entité qui a émis le compte et la vérification de demandes que le compte est valide et que la demande provient du titulaire du compte.

Le Service LSASS (Local sécurité autorité sous-système Service) stocke des informations d’identification dans la mémoire pour les utilisateurs avec les sessions Windows actives. Les informations d’identification stockées permettent aux utilisateurs d’accéder facilement aux ressources réseau, telles que les partages de fichiers, boîtes aux lettres Exchange Server et les sites SharePoint, sans devoir entrer leurs informations d’identification pour chaque service distant.

LSASS peut stocker des informations d’identification sous des formes multiples, y compris:

-   Texte en clair chiffré réversible

-   Tickets Kerberos (ticket-granting tickets TGT, tickets de service)

-   Hachage NT

-   Hachage de LAN Manager (LM)

Si l’utilisateur ouvre une session sur Windows à l’aide d’une carte à puce, LSASS ne stocke pas d’un mot de passe en texte brut, mais elle stocke la valeur de hachage NT correspondante pour le compte et le code confidentiel en texte en clair pour la carte à puce. Si l’attribut de compte est activé pour une carte à puce est requise pour l’ouverture de session interactive, une valeur de hachage NT aléatoire est automatiquement générée pour le compte au lieu du hachage de mot de passe d’origine. Le hachage de mot de passe est généré automatiquement lorsque l’attribut est défini ne change pas.

Si un utilisateur ouvre une session un ordinateur basé sur Windows avec un mot de passe est compatible avec les hachages LAN Manager (LM), cet authentificateur est présent dans la mémoire.

Le stockage des informations d’identification en texte en clair dans la mémoire ne peut pas être désactivé, même si les fournisseurs d’informations d’identification qui en ont besoin sont désactivés.

Les informations d’identification stockées sont directement associées les sessions d’ouverture de session locale LSASS Security Authority Subsystem Service () qui ont été démarrées après le dernier redémarrage et n’ont pas été fermées. Par exemple, les sessions LSA avec les informations d’identification LSA stockées sont créées lorsqu’un utilisateur effectue les opérations suivantes:

-   Ouvre une session sur une session locale ou une session de protocole RDP (Remote Desktop) sur l’ordinateur

-   Exécution d’une tâche à l’aide de la **RunAs** option

-   Exécute un service Windows actif sur l’ordinateur

-   Exécute une tâche planifiée de tâche ou de commandes

-   Exécution d’une tâche sur l’ordinateur local à l’aide d’un outil d’administration à distance

Dans certains cas, les secrets LSA, qui sont des éléments secrètes de données qui sont accessibles uniquement aux processus du compte système, sont stockés sur le lecteur de disque dur. Certains de ces secrets sont des informations d’identification qui doivent être conservées après le redémarrage, et ils sont stockés sous forme chiffrée sur le lecteur de disque dur. Informations d’identification stockées en tant que secrets LSA peuvent inclure:

-   Mot de passe de compte pour le compte de Services de domaine ActiveDirectory (ADDS) de l’ordinateur

-   Mots de passe pour les services Windows qui sont configurés sur l’ordinateur

-   Mots de passe pour les tâches planifiées configurées

-   Mots de passe pour les sites Web et les pools d’applications IIS

-   Mots de passe pour les comptes Microsoft

Introduite dans Windows8.1, le système d’exploitation client fournit une protection supplémentaire pour l’autorité LSA empêcher la lecture de la mémoire et une injection de code par des processus non protégés. Cette protection accroît la sécurité pour les informations d’identification qui l’autorité LSA stocke et gère.

Pour plus d’informations sur ces protections supplémentaires, voir [Configuring Additional LSA Protection](../credentials-protection-and-management/configuring-additional-lsa-protection.md).

## <a name="BKMK_CachedCredentialsAndValidation"></a>Validation et des informations d’identification mises en cache
Mécanismes de validation s’appuient sur la présentation des informations d’identification au moment de l’ouverture de session. Toutefois, lorsque l’ordinateur est déconnecté à partir d’un contrôleur de domaine et que l’utilisateur est présentant les informations d’identification de domaine, Windows utilise le processus d’informations d’identification mises en cache dans le mécanisme de validation.

Chaque fois qu'un utilisateur ouvre une session sur un domaine, Windowsmet en cache les informations d’identification fournies et les stocke dans la ruche de sécurité dans le Registre du système d’exploitation.

Avec les informations d’identification mises en cache, l’utilisateur peut se connecter vers un membre de domaine sans être connecté à un contrôleur de domaine dans ce domaine.


## <a name="BKMK_CredentialStorageAndValidation"></a>Stockage d’informations d’identification et la validation
Il n’est pas toujours souhaitable d’utiliser un ensemble d’informations d’identification pour accéder à différentes ressources. Par exemple, un administrateur peut souhaitez utiliser administration plutôt que des informations d’identification utilisateur lors de l’accès à un serveur distant. De même, si un utilisateur accède à des ressources externes, par exemple, un compte bancaire, il ou elle peut uniquement utiliser les informations d’identification autres que ceux que leurs informations d’identification de domaine. Les sections suivantes décrivent les différences de gestion des informations d’identification entre les versions actuelles de systèmes d’exploitation Windows et les systèmes d’exploitation WindowsVista et WindowsXP.

### <a name="remote-logon-credential-processes"></a>Processus des informations d’identification d’ouverture de session à distance
Le protocole RDP (Remote Desktop) gère les informations d’identification de l’utilisateur qui se connecte à un ordinateur distant à l’aide du Client Bureau à distance, ce qui a été introduite dans Windows8. Les informations d’identification en texte clair sont envoyées à l’ordinateur hôte cible sur lequel l’hôte tente d’effectuer le processus d’authentification et, en cas de réussite, l’utilisateur connecte aux ressources autorisées. RDP ne stocke pas les informations d’identification sur le client, mais les informations d’identification de domaine de l’utilisateur sont stockées dans le processus LSASS.

Introduite dans Windows Server2012R2 et Windows8.1, le mode administrateur restreint offre une sécurité supplémentaire aux scénarios d’ouverture de session à distance. Ce mode de bureau à distance, l’application cliente à effectuer une réseau d’ouverture de session stimulation-réponse avec la fonction à sens unique NT (NTOWF) ou utiliser un ticket de service Kerberos lors de l’authentification à l’hôte distant. Une fois que l’administrateur est authentifié, l’administrateur n’a pas les informations d’identification de compte respectifs dans LSASS, car ils n’ont pas été fournis à l’hôte distant. Au lieu de cela, l’administrateur possède les informations d’identification du compte ordinateur pour la session. Informations d’identification d’administrateur ne sont pas fournies à l’hôte distant, afin que les actions sont exécutées en tant que le compte d’ordinateur. Ressources sont également limités pour le compte d’ordinateur, et l’administrateur ne peut pas accéder aux ressources avec son propre compte.

### <a name="automatic-restart-sign-on-credential-process"></a>Processus d’ouverture de session d’informations d’identification de redémarrage automatique
Lorsqu’un utilisateur se connecte sur un appareil Windows8.1, LSA enregistre les informations d’identification de l’utilisateur dans la mémoire chiffrée qui sont accessibles uniquement par LSASS.exe. Lorsque Windows Update initie un redémarrage automatique sans la présence de l’utilisateur, ces informations d’identification sont utilisées pour configurer l’ouverture de session automatique pour l’utilisateur.

Lors du redémarrage, l’utilisateur est automatiquement connecté via le mécanisme d’ouverture de session automatique, puis l’ordinateur est en outre verrouillé afin de protéger la session utilisateur. Le verrouillage est lancée via Winlogon, tandis que la gestion des informations d’identification est effectuée par l’autorité LSA. En vous connectant automatiquement et le verrouillage de la session utilisateur sur la console, applications d’écran de verrouillage de l’utilisateur est redémarré et disponible.

Pour plus d’informations sur ARSO, voir [Winlogon automatique redémarrer Sign-On et #40; aRSO et #41;](winlogon-automatic-restart-sign-on-arso.md).

### <a name="stored-user-names-and-passwords-in-windows-vista-and-windows-xp"></a>Noms d’utilisateurs et mots de passe dans WindowsVista et WindowsXP
Dans Windows Server2008, Windows Server2003, WindowsVista et WindowsXP, **stockées noms d’utilisateur et mots de passe** dans le panneau de configuration simplifie la gestion et l’utilisation de plusieurs jeux d’informations d’identification d’ouverture de session, y compris les certificats X.509utilisés avec des cartes à puce et les informations d’identification Windows Live a (maintenant appelées compte Microsoft). Les informations d’identification - partie du profil de l’utilisateur - sont stockées en tant que nécessaire. Cette action permet de renforcer la sécurité en fonction de chaque ressource en vous assurant que si un mot de passe est compromis, il ne compromet pas toute la sécurité.

Une fois un utilisateur se connecte et tente d’accéder aux ressources supplémentaires protégé par mot de passe, tel qu’un partage sur un serveur, et si les informations d’identification d’ouverture de session de l’utilisateur par défaut ne sont pas suffisantes pour accéder, **stockées noms d’utilisateur et mots de passe** est interrogé. Si les autres informations d’identification avec les informations d’ouverture de session approprié ont été enregistrées dans **stockées noms d’utilisateur et mots de passe**, ces informations d’identification sont utilisées pour y accéder. Dans le cas contraire, l’utilisateur est invité à fournir de nouvelles informations d’identification, qui peuvent ensuite être enregistrées pour une réutilisation, plus loin dans l’ouverture de session ou lors d’une session ultérieure.

Les restrictions suivantes s’appliquent:

-   Si **stockées noms d’utilisateur et mots de passe** contient des informations d’identification non valides ou incorrectes pour une ressource spécifique, l’accès à la ressource est refusée et le **stockées noms d’utilisateur et mots de passe** boîte de dialogue n’apparaît pas.

-   **Stockées des noms d’utilisateur et mots de passe** stocke des informations d’identification uniquement pour NTLM, le protocole Kerberos, le compte Microsoft (anciennement WindowsLiveID) et l’authentification Secure Sockets Layer (SSL). Certaines versions d’Internet Explorer conservent leur propre cache pour l’authentification de base.

Ces informations d’identification devient une partie d’un profil d’utilisateur local dans le répertoire Data\Microsoft\Credentials se \Documents and chiffrée. Par conséquent, ces informations d’identification peuvent se déplacent avec l’utilisateur si la stratégie de réseau de l’utilisateur prend en charge les profils utilisateur itinérants. Toutefois, si l’utilisateur possède des copies du **stockées noms d’utilisateur et mots de passe** sur deux ordinateurs différents et les modifications les informations d’identification qui sont associées à la ressource sur un de ces ordinateurs, la modification n’est pas propagée vers **stockées noms d’utilisateur et mots de passe** sur le deuxième ordinateur.

### <a name="windows-vault-and-credential-manager"></a>Coffre Windows et le Gestionnaire d’informations d’identification
Le Gestionnaire d’informations d’identification a été introduit dans Windows Server2008R2 et Windows7 en tant que le panneau de configuration fonctionnalité pour stocker et gérer les noms d’utilisateur et mots de passe. Le Gestionnaire d’informations d’identification permet aux utilisateurs de stocker des informations d’identification appropriées à d’autres systèmes et les sites Web dans le coffre Windows sécurisé. Certaines versions d’Internet Explorer utilisent cette fonctionnalité pour l’authentification aux sites Web.

Gestion des informations d’identification à l’aide du Gestionnaire d’informations d’identification est contrôlée par l’utilisateur sur l’ordinateur local. Les utilisateurs peuvent enregistrer et stocker des informations d’identification à partir de navigateurs pris en charge et les applications Windows pour le rendre pratique lorsqu’ils ont besoin pour se connecter à ces ressources. Informations d’identification sont enregistrées dans des dossiers chiffrés spéciaux sur l’ordinateur sous le profil utilisateur. Les applications qui prennent en charge cette fonctionnalité (via les API du Gestionnaire d’informations d’identification), tels que les navigateurs web et applications, peuvent présenter les informations d’identification correctes à d’autres ordinateurs et les sites Web au cours du processus d’ouverture de session.

Lorsqu’un site Web, une application ou un autre ordinateur demande une authentification via NTLM ou le protocole Kerberos, une boîte de dialogue s’affiche dans laquelle vous sélectionnez le **mise à jour les informations d’identification par défaut** ou **enregistrer le mot de passe** case à cocher. Cette boîte de dialogue qui permet à un utilisateur d’enregistrer les informations d’identification localement est générée par une application qui prend en charge les API du Gestionnaire d’informations d’identification. Si l’utilisateur sélectionne le **enregistrer le mot de passe** case à cocher, le Gestionnaire d’informations d’identification conserve une trace de nom d’utilisateur de l’utilisateur, mot de passe, et des informations pour le service d’authentification qui est en cours d’utilisation.

La prochaine fois que le service est utilisé, le Gestionnaire d’informations d’identification fournit automatiquement les informations d’identification qui sont stockée dans le coffre de sauvegarde de Windows. Si elle n’est pas accepté, l’utilisateur est invité pour les informations d’accès correctes. Si l’accès est accordé avec les nouvelles informations d’identification, le Gestionnaire d’informations d’identification remplace les informations précédentes par le nouveau, puis stocke les nouvelles informations d’identification dans le coffre de sauvegarde de Windows.

## <a name="BKMK_SAM"></a>Base de données du Gestionnaire de comptes de sécurité
Le Gestionnaire de comptes de sécurité (SAM) est une base de données qui stocke les groupes et comptes d’utilisateurs locaux. Il est présent dans tous les systèmes d’exploitation Windows; toutefois, lorsqu’un ordinateur est joint à un domaine, ActiveDirectory gère les comptes de domaine dans les domaines ActiveDirectory.

Par exemple, les ordinateurs clients exécutant un système d’exploitation Windows participent à un domaine de réseau en communiquant avec un contrôleur de domaine, même lorsque aucun utilisateur humaine est connecté. Pour établir des communications, l’ordinateur doit avoir un compte actif dans le domaine. Avant d’accepter les communications à partir de l’ordinateur, l’autorité LSA sur le contrôleur de domaine authentifie l’identité de l’ordinateur et crée ensuite le contexte de sécurité de l’ordinateur comme pour un principal de sécurité humaine. Ce contexte de sécurité définit l’identité et les fonctionnalités d’un utilisateur ou un service sur un ordinateur particulier ou un utilisateur, un service ou un ordinateur sur un réseau. Par exemple, le jeton d’accès contenu dans le contexte de sécurité définit les ressources (par exemple, un partage de fichiers ou une imprimante) qui sont accessibles et les actions (par exemple, lecture, écriture ou de modification) qui peuvent être effectuées par cette entité - un utilisateur, ordinateur ou service qui ressource.

Le contexte de sécurité d’un utilisateur ou un ordinateur peut varier d’un ordinateur vers un autre, par exemple, lorsqu’un utilisateur ouvre une session sur un serveur ou une station de travail autres que la station de travail de l’utilisateur principal. Il peut également varier d’une session à l’autre, par exemple, lorsqu’un administrateur modifie les droits et les autorisations de l’utilisateur. En outre, le contexte de sécurité est généralement différent lorsqu’un utilisateur ou ordinateur fonctionne de manière autonome, dans un réseau, ou en tant que partie d’un domaine ActiveDirectory.

## <a name="BKMK_LocalDomainsAndTrustedDomains"></a>Les domaines locaux et des domaines approuvés
Lorsqu’il existe une relation d’approbation entre deux domaines, les mécanismes d’authentification pour chaque domaine s’appuient sur la validité des authentifications en provenance de l’autre domaine. Approbations aide à fournir un accès contrôlé aux ressources partagées dans un domaine de ressources (le domaine d’approbation) en vérifiant que l’authentification entrante demandes proviennent d’une autorité approuvée (le domaine approuvé). De cette façon, les approbations servent de ponts qui permettent uniquement validé voyage de demandes d’authentification entre domaines.

Comment une relation d’approbation spécifique transmet les demandes d’authentification dépend de la façon dont il est configuré. Relations d’approbation peuvent être à sens unique, en fournissant un accès aux ressources du domaine d’approbation du domaine approuvé ou bidirectionnelle, en fournissant un accès aux ressources de l’autre domaine de chaque domaine. Approbations sont également non transitives, auquel cas il existe une approbation uniquement entre les domaines de partenaire de deux confiance, ou transitives, auquel cas une relation d’approbation étend automatiquement vers d’autres domaines d’un des partenaires approuve.

Pour plus d’informations sur les relations d’approbation de domaine et de forêt concernant l’authentification, consultez [de l’authentification déléguée et les relations d’approbation](https://technet.microsoft.com/library/dn169022.aspx).

## <a name="BKMK_CertificatesInWindowsAuthentication"></a>Certificats de l’authentification Windows
Une infrastructure à clé publique (PKI) est la combinaison des logiciels, des technologies de chiffrement, des processus et des services qui permettent à une organisation de sécuriser ses communications et les transactions commerciales. La possibilité de sécuriser les communications et les transactions commerciales d’une infrastructure à clé publique est basée sur l’échange de certificats numériques entre les utilisateurs authentifiés et des ressources approuvées.

Un certificat numérique est un document électronique qui contient des informations sur l’entité qu’il appartient à l’entité qu’il a été émis par, un numéro de série unique ou certaines autres identification unique, d’émission et dates d’expiration et une empreinte digitale.

L’authentification est le processus permettant de déterminer si un hôte distant peut être approuvé. Pour établir sa fiabilité, l’hôte distant doit fournir un certificat d’authentification acceptable.

Hôtes distants établissent leur fiabilité en obtenant un certificat auprès d’une autorité de certification (CA). L’autorité de certification peut, à son tour, dotées d’une autorité supérieure, ce qui crée une chaîne d’approbation. Pour déterminer si un certificat est digne de confiance, une application doit déterminer l’identité de l’autorité de certification racine, puis déterminer si elle est digne de confiance.

De même, l’ordinateur local ou un hôte distant doit déterminer si le certificat présenté par l’utilisateur ou l’application est authentique. Le certificat présenté par l’utilisateur par le biais de l’autorité LSA et SSPI est évalué pour authenticité sur l’ordinateur local pour l’ouverture de session locale, sur le réseau ou sur le domaine via les magasins de certificats dans ActiveDirectory.

Pour générer un certificat, les données d’authentification passe par le biais d’algorithmes de hachage, telles que Secure Hash Algorithm 1 (SHA1), pour produire un résumé du message. Le résumé du message est ensuite signé à l’aide de la clé privée de l’expéditeur pour prouver que le résumé du message a été créé par l’expéditeur.

> [!NOTE]
> SHA1 est la valeur par défaut dans Windows7 et WindowsVista, mais il a été remplacé par SHA2 dans Windows8.

**Authentification par carte à puce**

Technologie de carte à puce est un exemple d’authentification par certificat. Connexion à un réseau avec une carte à puce fournit d’authentification puissante, car elle utilise une identification basée sur le chiffrement et preuve de possession lors de l’authentification d’un utilisateur à un domaine. Services de certificats ActiveDirectory (ADCS) fournit la chiffrement basé d’identification par le biais de l’émission d’un certificat d’ouverture de session pour chaque carte à puce.

Pour plus d’informations sur l’authentification par carte à puce, consultez le [référence technique de carte à puce Windows](https://technet.microsoft.com/library/ff404297.aspx).

Technologie de carte à puce virtuelle a été introduite dans Windows8. Il stocke le certificat de la carte à puce dans le PC, puis de le protéger à l’aide de puce de sécurité de Module de plateforme sécurisée (TPM) de falsification de l’appareil. De cette façon, le PC devient réellement la carte à puce qui doivent recevoir le code confidentiel de l’utilisateur afin d’être authentifié.

**Authentification à distance et sans fil**

L’authentification réseau à distance et sans fil est une autre technologie qui utilise des certificats pour l’authentification. Le Service d’authentification Internet (IAS) et les serveurs de réseau privé virtuel utilisent Extensible Authentication Protocol-Transport Level Security (EAP-TLS), Extensible Authentication Protocol PEAP (Protected) ou Internet Protocol security (IPsec) pour effectuer l’authentification par certificat pour de nombreux types d’accès réseau, y compris les connexions sans fil et réseau privé virtuel (VPN).

Pour plus d’informations sur l’authentification basée sur les certificats dans la mise en réseau, consultez [réseau de l’authentification d’accès et les certificats](https://technet.microsoft.com/library/cc759575(WS.10).aspx).

## <a name="BKMK_SeeAlso"></a>Voir aussi
[Concepts de l’authentification Windows](https://technet.microsoft.com/library/d169018.aspx)


