---
title: Stratégies d'authentification et silos de stratégies d'authentification
description: Sécurité de Windows Server
ms.prod: windows-server
ms.technology: security-credential-protection
ms.topic: article
ms.assetid: 7eb0e640-033d-49b5-ab44-3959395ad567
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 48171657b42ec8b6ba09aa6a35a2f898d6775d07
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857082"
---
# <a name="authentication-policies-and-authentication-policy-silos"></a>Stratégies d'authentification et silos de stratégies d'authentification

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique destinée aux professionnels de l'informatique décrit les silos de stratégies d'authentification et les stratégies qui peuvent limiter des comptes à ces silos. Elle explique également comment utiliser des stratégies d'authentification pour restreindre l'étendue d'utilisation de certains comptes.

Les silos de stratégies d'authentification et les stratégies qui les accompagnent permettent de restreindre des informations d'identification dotées de privilèges élevés à des systèmes pertinents uniquement pour certains utilisateurs, ordinateurs ou services sélectionnés. Les silos peuvent être définis et gérés dans Active Directory Domain Services (AD DS) à l’aide de la Centre d’administration Active Directory et des applets de commande Windows PowerShell Active Directory.

Les silos de stratégies d'authentification sont des conteneurs dans lesquels les administrateurs peuvent affecter des comptes d'utilisateur, d'ordinateur et de service. Il est ensuite possible de gérer ces ensembles de comptes à l'aide des stratégies d'authentification qui ont été appliquées à ces conteneurs. Cela évite à l'administrateur d'avoir à effectuer le suivi de l'accès aux ressources pour les comptes individuels et contribue également à empêcher les utilisateurs malveillants d'accéder à d'autres ressources via le vol d'informations d'identification.

Les fonctionnalités introduites dans Windows Server 2012 R2 vous permettent de créer des silos de stratégies d’authentification, qui hébergent un ensemble d’utilisateurs à privilèges élevés. Vous pouvez ensuite affecter des stratégies d'authentification pour ce conteneur afin de limiter la zone du domaine où ces comptes privilégiés peuvent être utilisés. Lorsque les comptes appartiennent au groupe de sécurité Utilisateurs protégés, des contrôles supplémentaires sont appliqués, tels que l'utilisation exclusive du protocole Kerberos.

Ces fonctions permettent de restreindre l'utilisation des comptes importants aux hôtes importants. Par exemple, vous pouvez créer un nouveau silo Administrateurs de forêt contenant les administrateurs d'entreprise, de schéma et de domaine. Ensuite, vous pouvez configurer ce silo avec une stratégie d'authentification entraînant l'échec de l'authentification par mot de passe ou carte à puce à partir de systèmes autres que les contrôleurs de domaine et les consoles Administrateurs de domaine.

Pour plus d'informations sur la configuration de stratégies d'authentification et de silos de stratégies d'authentification, voir [Comment configurer des comptes protégés](how-to-configure-protected-accounts.md).

### <a name="about-authentication-policy-silos"></a>À propos des silos de stratégies d'authentification
Un silo de stratégies d'authentification détermine les comptes qui peuvent être restreints par le silo et définit les stratégies d'authentification à appliquer aux membres. Vous pouvez créer un silo en fonction des besoins de votre organisation. Les silos sont des objets Active Directory destinés aux utilisateurs, ordinateurs et services, tels que définis par le schéma exposé dans le tableau ci-dessous.

**Schéma de Active Directory pour les silos de stratégies d’authentification**

|Nom d’affichage|Description|
|--------|--------|
|Silo de stratégies d'authentification|Une instance de cette classe définit des stratégies d'authentification et des comportements associés pour les utilisateurs, les ordinateurs et les services affectés.|
|Silos de stratégies d'authentification|Un conteneur de cette classe peut obtenir des objets silos de stratégies d'authentification.|
|Silo de stratégies d'authentification appliqué|Spécifie si le silo de stratégies d'authentification est appliqué ou non.<p>S'il n'est pas appliqué, la stratégie est par défaut en mode audit. Des événements indiquant les succès ou échecs potentiels sont générés, mais aucune protection n'est appliquée au système.|
|Lien secondaire du silo de stratégies d'authentification affecté|Cet attribut correspond au lien secondaire pour le silo msDS-AssignedAuthNPolicySilo.|
|Membres du silo de stratégies d'authentification|Spécifie quels principaux sont affectés au silo AuthNPolicySilo.|
|Lien secondaire aux membres du silo de stratégies d'authentification|Cet attribut correspond au lien secondaire pour msDS-AuthNPolicySiloMembers.|

Les silos de stratégies d'authentification peuvent être configurés à l'aide de la console d'administration Active Directory ou de Windows PowerShell. Pour plus d'informations, voir [Comment configurer des comptes protégés](how-to-configure-protected-accounts.md).

### <a name="about-authentication-policies"></a>À propos des stratégies d'authentification
Une stratégie d'authentification définit les propriétés de durée de vie des tickets TGT du protocole Kerberos et les conditions de contrôle d'accès par authentification d'un type de compte. La stratégie repose sur et contrôle le conteneur de AD DS connu sous le nom de silo de stratégies d’authentification.

Les stratégies d'authentification contrôlent les éléments suivants :

-   La durée de vie du ticket TGT pour le compte, qui est définie comme non renouvelable.

-   Les critères que doivent respecter les comptes de périphériques pour se connecter avec un mot de passe ou un certificat.

-   Les critères que doivent respecter les utilisateurs et les périphériques pour s'authentifier auprès des services s'exécutant sous ce compte.

Le type de compte Active Directory détermine le rôle de l’appelant comme l’un des éléments suivants :

-   **Utilisateur**

    Les utilisateurs doivent toujours être membres du groupe de sécurité Utilisateurs protégés, lequel rejette par défaut les tentatives d'authentification utilisant NTLM.

    Les stratégies peuvent être configurées pour définir la durée de vie TGT d’un compte d’utilisateur sur une valeur plus petite ou limiter les appareils auxquels un compte d’utilisateur peut se connecter. Les expressions riches peuvent être configurées dans la stratégie d’authentification pour contrôler les critères que les utilisateurs et leurs appareils doivent respecter pour s’authentifier auprès du service.

    Pour plus d'informations, voir [Groupe de sécurité Utilisateurs protégés](protected-users-security-group.md).

-   **Services**

    Des comptes de service administrés autonomes, des comptes de service administrés de groupe ou un objet compte personnalisé dérivé de ces deux types de comptes de service sont utilisés. Les stratégies peuvent définir les conditions de contrôle d’accès d’un appareil, qui sont utilisées pour limiter les informations d’identification du compte de service administré à des appareils spécifiques avec une identité de Active Directory. Les services ne doivent jamais être membres du groupe de sécurité Utilisateurs protégés car toute authentification entrante échouera.

-   **Informatisé**

    L'objet compte d'ordinateur ou l'objet compte personnalisé qui est dérivé de l'objet compte d'ordinateur est utilisé. Des stratégies peuvent définir les conditions de contrôle d'accès requises pour autoriser l'authentification auprès du compte en fonction des propriétés propres à l'utilisateur et au périphérique. Les ordinateurs ne doivent jamais être membres du groupe de sécurité Utilisateurs protégés car toute authentification entrante échouera. Par défaut, les tentatives visant à utiliser l'authentification NTLM sont rejetées. La durée de vie des tickets TGT ne doit pas être configurée pour les comptes d'ordinateur.

> [!NOTE]
> Il est possible de définir une stratégie d'authentification sur un ensemble de comptes sans associer la stratégie à un silo de stratégies d'authentification. Vous pouvez utiliser cette stratégie quand vous n'avez qu'un seul compte à protéger.

**Schéma de Active Directory pour les stratégies d’authentification**

Les stratégies prévues pour les objets Active Directory relatifs aux utilisateurs, ordinateurs et services sont définies par le schéma présenté dans le tableau ci-dessous.

|Type|Nom d’affichage|Description|
|----|--------|--------|
|Stratégie|Stratégie d'authentification|Une instance de cette classe définit les comportements associés aux stratégies d'authentification pour les principaux affectés.|
|Stratégie|Stratégies d'authentification|Un conteneur de cette classe peut obtenir des objets de stratégie d'authentification.|
|Stratégie|Stratégie d'authentification appliquée|Spécifie si la stratégie d'authentification est appliquée ou non.<p>Si elle n'est pas appliquée, la stratégie est par défaut en mode audit et des événements indiquant les succès ou échecs potentiels sont générés, mais aucune protection n'est appliquée au système.|
|Stratégie|Lien secondaire de stratégie d'authentification affecté|Cet attribut correspond au lien secondaire pour msDS-AssignedAuthNPolicy.|
|Stratégie|Stratégie d'authentification affectée|Spécifie la stratégie AuthNPolicy qui doit être appliquée à ce principal.|
|Utilisateur|Stratégie d'authentification d'utilisateur|Spécifie la stratégie AuthNPolicy qui doit être appliquée aux utilisateurs affectés à cet objet silo.|
|Utilisateur|Lien secondaire de stratégie d'authentification d'utilisateur|Cet attribut correspond au lien secondaire pour msDS-UserAuthNPolicy.|
|Utilisateur|ms-DS-User-Allowed-To-Authenticate-To|Cet attribut permet de déterminer l'ensemble des principaux autorisés à s'authentifier auprès d'un service s'exécutant sous le compte d'utilisateur.|
|Utilisateur|ms-DS-User-Allowed-To-Authenticate-From|Cet attribut permet de déterminer l'ensemble des périphériques auxquels un compte d'utilisateur est autorisé à se connecter.|
|Utilisateur|Durée de vie de ticket TGT d'utilisateur|Spécifie l'âge maximal d'un ticket TGT Kerberos émis pour un utilisateur (exprimé en secondes). Les tickets TGT résultants sont non renouvelables.|
|Ordinateur|Stratégie d'authentification d'ordinateur|Spécifie la stratégie AuthNPolicy qui doit être appliquée aux ordinateurs affectés à cet objet silo.|
|Ordinateur|Lien secondaire de stratégie d'authentification d'ordinateur|Cet attribut correspond au lien secondaire pour msDS-ComputerAuthNPolicy.|
|Ordinateur|ms-DS-Computer-Allowed-To-Authenticate-To|Cet attribut permet de déterminer l'ensemble des principaux autorisés à s'authentifier auprès d'un service s'exécutant sous le compte d'ordinateur.|
|Ordinateur|Durée de vie de ticket TGT d'ordinateur|Spécifie l'âge maximal d'un ticket TGT Kerberos émis pour un ordinateur (exprimé en secondes). Il n'est pas conseillé de modifier ce paramètre.|
|Service|Stratégie d'authentification de service|Spécifie la stratégie AuthNPolicy qui doit être appliquée aux services affectés à cet objet silo.|
|Service|Lien secondaire de stratégie d'authentification de service|Cet attribut correspond au lien secondaire pour msDS-ServiceAuthNPolicy.|
|Service|ms-DS-Service-Allowed-To-Authenticate-To|Cet attribut permet de déterminer l'ensemble des principaux autorisés à s'authentifier auprès d'un service s'exécutant sous le compte de service.|
|Service|ms-DS-Service-Allowed-To-Authenticate-From|Cet attribut permet de déterminer l'ensemble des périphériques auxquels un compte de service est autorisé à se connecter.|
|Service|Durée de vie de ticket TGT de service|Spécifie l'âge maximal d'un ticket TGT Kerberos émis pour un service (exprimé en secondes).|

Des stratégies d'authentification peuvent être configurées pour chaque silo à l'aide de la console d'administration Active Directory ou de Windows PowerShell. Pour plus d'informations, voir [Comment configurer des comptes protégés](how-to-configure-protected-accounts.md).

## <a name="how-it-works"></a>Fonctionnement
Cette section décrit comment les stratégies d'authentification et les silos de stratégies d'authentification fonctionnent en association avec le groupe de sécurité Utilisateurs protégés et l'implémentation du protocole Kerberos dans Windows.

-   [Comment le protocole Kerberos est utilisé avec les silos d’authentification et les stratégies](#BKMK_HowKerbUsed)

-   [Comment limiter le fonctionnement de la connexion d’un utilisateur](#BKMK_HowRestrictingSignOn)

-   [Fonctionnement de la restriction de l’émission de tickets de service](#BKMK_HowRestrictingServiceTicket)

**Comptes protégés**

Le groupe de sécurité utilisateurs protégés déclenche une protection non configurable sur les appareils et ordinateurs hôtes exécutant Windows Server 2012 R2 et Windows 8.1, ainsi que sur les contrôleurs de domaine dans des domaines avec un contrôleur de domaine principal exécutant Windows Server 2012 R2. En fonction du niveau fonctionnel de domaine du compte, les membres du groupe de sécurité Utilisateurs protégés sont également protégés en raison des modifications apportées dans les méthodes d'authentification prises en charge dans Windows.

-   Le membre du groupe de sécurité Utilisateurs protégés ne peut pas s'authentifier en utilisant NTLM, l'authentification Digest ni la délégation d'informations d'identification par défaut CredSSP. Sur un appareil exécutant Windows 8.1 qui utilise l’un de ces fournisseurs de support de sécurité (SSP), l’authentification auprès d’un domaine échoue lorsque le compte est membre du groupe de sécurité utilisateurs protégés.

-   Le protocole Kerberos n'utilise pas les types de chiffrement DES et RC4 plus faibles dans le processus de pré-authentification. Cela signifie que le domaine doit être configuré pour prendre en charge au moins le type de chiffrement AES.

-   Le compte de l’utilisateur ne peut pas être délégué avec la délégation Kerberos contrainte ou non contrainte. Cela signifie que les connexions antérieures à d'autres systèmes peuvent échouer si l'utilisateur est membre du groupe de sécurité Utilisateurs protégés.

-   Le paramètre de durée de vie des tickets TGT Kerberos par défaut de quatre heures est configurable à l'aide des silos et stratégies d'authentification, auxquels il est possible d'accéder via le Centre d'administration Active Directory. Cela signifie qu'après quatre heures l'utilisateur doit s'authentifier de nouveau.

Pour plus d'informations sur ce groupe de sécurité, voir [Comment fonctionne le groupe Utilisateurs protégés](protected-users-security-group.md#BKMK_HowItWorks).

**Silos et stratégies d’authentification**

Les stratégies d'authentification et les silos de stratégies d'authentification tirent parti de l'infrastructure d'authentification Windows existante. L'utilisation du protocole NTLM est rejetée et le protocole Kerberos doté de types de chiffrement plus récents est utilisé. Les stratégies d'authentification offrent un complément au groupe de sécurité Utilisateurs protégés en permettant d'appliquer des limitations configurables aux comptes, en plus de limiter les comptes pour les services et les ordinateurs. Les stratégies d'authentification sont appliquées pendant l'échange du service d'authentification (AS) ou du service d'accord de tickets (TGS) du protocole Kerberos. Pour plus d'informations sur la façon dont Windows utilise le protocole Kerberos et sur les changements qui ont été effectués pour prendre en charge les stratégies d'authentification et les silos de stratégies d'authentification, voir :

-   [Fonctionnement du protocole d’authentification Kerberos version 5](https://technet.microsoft.com/library/cc772815(v=ws.10).aspx)

-   [Modifications apportées à l’authentification Kerberos](https://technet.microsoft.com/library/dd560670(v=ws.10).aspx) (windows Server 2008 R2 et Windows 7)

### <a name="how-the-kerberos-protocol-is-used-with-authentication-policy-silos-and-policies"></a><a name="BKMK_HowKerbUsed"></a>Comment le protocole Kerberos est utilisé avec les silos et stratégies de stratégie d’authentification
Quand un compte de domaine est lié à un silo de stratégies d'authentification et que l'utilisateur se connecte, le Gestionnaire des comptes de sécurité ajoute le type de revendication Silo de stratégies d'authentification qui inclut le silo comme valeur. Cette revendication sur le compte fournit l'accès au silo ciblé.

Quand une stratégie d'authentification est appliquée et que la demande du service d'authentification pour un compte de domaine est reçue sur le contrôleur de domaine, ce dernier retourne un ticket TGT non renouvelable, doté de la durée de vie configurée (à moins que la durée de vie des tickets TGT du domaine soit plus courte).

> [!NOTE]
> Le compte de domaine doit avoir une durée de vie de ticket TGT configurée et doit être directement lié à la stratégie ou indirectement lié via son appartenance au silo.

Quand une stratégie d'authentification est en mode audit et que la demande du service d'authentification pour un compte de domaine est reçue sur le contrôleur de domaine, ce dernier vérifie si l'authentification est autorisée pour le périphérique afin de pouvoir consigner un avertissement en cas d'échec. Une stratégie d'authentification auditée n'altère pas le processus, si bien que les demandes d'authentification n'échoueront pas si elles ne satisfont pas aux conditions requises de la stratégie.

> [!NOTE]
> Le compte de domaine doit être directement lié à la stratégie ou indirectement lié via son appartenance au silo.

Quand une stratégie d'authentification est appliquée et que le service d'authentification est blindé, la demande du service d'authentification pour un compte de domaine est reçue sur le contrôleur de domaine et ce dernier vérifie si l'authentification est autorisée pour le périphérique. En cas d'échec, le contrôleur de domaine retourne un message d'erreur et consigne un événement.

> [!NOTE]
> Le compte de domaine doit être directement lié à la stratégie ou indirectement lié via son appartenance au silo.

Lorsqu’une stratégie d’authentification est en mode audit et qu’une demande de service d’accord de ticket est reçue par le contrôleur de domaine pour un compte de domaine, le contrôleur de domaine vérifie si l’authentification est autorisée en fonction des données du certificat d’attribut de privilège de ticket (PAC) de la demande et enregistre un message d’avertissement en cas d’échec. Le certificat PAC contient divers types de données d'autorisation, dont notamment les groupes dont l'utilisateur est membre, les droits détenus par l'utilisateur et les stratégies qui s'appliquent à l'utilisateur. Ces informations sont utilisées pour générer le jeton d’accès de l’utilisateur. S’il s’agit d’une stratégie d’authentification appliquée qui autorise l’authentification auprès d’un utilisateur, d’un périphérique ou d’un service, le contrôleur de domaine vérifie si l’authentification est autorisée en fonction des données de la clé PAC du ticket de la demande. En cas d'échec, le contrôleur de domaine retourne un message d'erreur et consigne un événement.

> [!NOTE]
> Le compte de domaine doit être lié directement ou via son appartenance à un silo à une stratégie d'authentification auditée qui permet l'authentification auprès d'un utilisateur, d'un périphérique ou d'un service,

Vous pouvez utiliser une stratégie d'authentification unique pour tous les membres d'un silo ou des stratégies distinctes pour les utilisateurs, les ordinateurs et les comptes de service administrés.

Des stratégies d'authentification peuvent être configurées pour chaque silo à l'aide de la console d'administration Active Directory ou de Windows PowerShell. Pour plus d'informations, voir [Comment configurer des comptes protégés](how-to-configure-protected-accounts.md).

### <a name="how-restricting-a-user-sign-in-works"></a><a name="BKMK_HowRestrictingSignOn"></a>Comment limiter le fonctionnement de la connexion d’un utilisateur
Comme les stratégies d'authentification sont appliquées à un compte, la limitation s'applique aux comptes utilisés par les services. Si vous souhaitez limiter l'utilisation d'un mot de passe d'un service à des hôtes spécifiques, ce paramètre est utile. Par exemple, les comptes de service administrés de groupe sont configurés si les hôtes sont autorisés à récupérer le mot de passe à partir des services de domaine Active Directory. Toutefois, ce mot de passe peut être utilisé à partir de n'importe quel hôte pour l'authentification initiale. En appliquant une condition de contrôle d'accès, il est possible d'obtenir une couche de protection supplémentaire en limitant le mot de passe aux seuls hôtes qui peuvent le récupérer.

Lorsque des services qui s’exécutent en tant que système, service réseau ou autre identité de service local se connectent aux services réseau, ils utilisent le compte d’ordinateur de l’hôte. Les comptes d'ordinateur ne peuvent pas faire l'objet d'une limitation. Ainsi, même si le service utilise un compte d'ordinateur qui n'est pas destiné à un hôte Windows, le compte ne peut pas faire l'objet d'une limitation.

La restriction de la connexion des utilisateurs à des ordinateurs hôtes spécifiques nécessite que le contrôleur de domaine valide l’identité de l’hôte. Quand l'authentification Kerberos est utilisée avec le blindage Kerberos (qui fait partie du contrôle d'accès dynamique), le centre de distribution de clés reçoit le ticket TGT de l'hôte à partir duquel l'utilisateur s'authentifie. Le contenu de ce ticket TGT blindé sert à effectuer une vérification d'accès pour déterminer si l'hôte est autorisé.

Quand un utilisateur se connecte à Windows ou entre ses informations d'identification de domaine dans une invite de demande d'informations d'identification, par défaut, Windows envoie une demande AS-REQ non blindée au contrôleur de domaine. Si l’utilisateur envoie la demande à partir d’un ordinateur qui ne prend pas en charge le blindage, tel que les ordinateurs exécutant Windows 7 ou Windows Vista, la demande échoue.

La liste suivante décrit le processus :

-   Le contrôleur de domaine dans un domaine exécutant Windows Server 2012 R2 interroge le compte d’utilisateur et détermine s’il est configuré avec une stratégie d’authentification qui restreint l’authentification initiale nécessitant des demandes blindées.

-   Le contrôleur de domaine provoquera l'échec de la demande.

-   Étant donné que le blindage est requis, l’utilisateur peut tenter de se connecter à l’aide d’un ordinateur exécutant Windows 8.1 ou Windows 8, qui est activé pour prendre en charge le blindage Kerberos afin de retenter le processus de connexion.

-   Windows détecte que le domaine prend en charge le blindage Kerberos et envoie une demande AS-REQ blindée pour répéter la demande de connexion.

-   Le contrôleur de domaine effectue une vérification d’accès en utilisant les conditions de contrôle d’accès configurées et les informations d’identité du système d’exploitation client dans le ticket TGT utilisé pour Blinder la demande.

-   En cas d'échec de la vérification d'accès, le contrôleur de domaine rejette la demande.

Même quand les systèmes d'exploitation prennent en charge le blindage Kerberos, les conditions requises de contrôle d'accès peuvent être appliquées et doivent être satisfaites pour que l'accès soit accordé. Les utilisateurs se connectent à Windows ou entrent leurs informations d'identification de domaine dans une invite de demande d'informations d'identification pour une application. Par défaut, Windows envoie une demande AS-REQ non blindée au contrôleur de domaine. Si l’utilisateur envoie la demande à partir d’un ordinateur qui prend en charge le blindage, tel que Windows 8.1 ou Windows 8, les stratégies d’authentification sont évaluées comme suit :

1.  Le contrôleur de domaine dans un domaine exécutant Windows Server 2012 R2 interroge le compte d’utilisateur et détermine s’il est configuré avec une stratégie d’authentification qui restreint l’authentification initiale nécessitant des demandes blindées.

2.  Le contrôleur de domaine effectue une vérification d’accès en utilisant les conditions de contrôle d’accès configurées et les informations d’identité du système dans le ticket TGT utilisé pour Blinder la demande. La vérification d'accès réussit.

    > [!NOTE]
    > Si des limitations de groupe de travail héritées sont configurées, elles doivent également être respectées.

3.  Le contrôleur de domaine renvoie une réponse blindée (AS-REP) et l'authentification continue.

### <a name="how-restricting-service-ticket-issuance-works"></a><a name="BKMK_HowRestrictingServiceTicket"></a>Fonctionnement de la restriction de l’émission de tickets de service
Lorsqu’un compte n’est pas autorisé et qu’un utilisateur disposant d’un TGT tente de se connecter au service (par exemple, en ouvrant une application qui requiert l’authentification auprès d’un service identifié par le nom de principal du service (SPN) du service, la séquence suivante se produit :

1.  Dans sa tentative de connexion à SPN1 à partir de SPN, Windows envoie une demande TGS-REQ au contrôleur de domaine qui demande un ticket de service à SPN1.

2.  Le contrôleur de domaine dans un domaine exécutant Windows Server 2012 R2 recherche SPN1 pour trouver le compte Active Directory Domain Services pour le service et détermine que le compte est configuré avec une stratégie d’authentification qui limite l’émission du ticket de service.

3.  Le contrôleur de domaine effectue une vérification d’accès en utilisant les conditions de contrôle d’accès configurées et les informations d’identité de l’utilisateur dans le ticket TGT. La vérification d'accès échoue.

4.  Le contrôleur de domaine rejette la demande.

Lorsqu’un compte est autorisé parce que le compte remplit les conditions de contrôle d’accès définies par la stratégie d’authentification et qu’un utilisateur disposant d’un TGT tente de se connecter au service (par exemple, en ouvrant une application qui requiert l’authentification auprès d’un service identifié par le SPN du service), la séquence suivante se produit :

1.  Dans sa tentative de connexion à SPN1, Windows envoie une demande TGS-REQ au contrôleur de domaine qui demande un ticket de service à SPN1.

2.  Le contrôleur de domaine dans un domaine exécutant Windows Server 2012 R2 recherche SPN1 pour trouver le compte Active Directory Domain Services pour le service et détermine que le compte est configuré avec une stratégie d’authentification qui limite l’émission du ticket de service.

3.  Le contrôleur de domaine effectue une vérification d’accès en utilisant les conditions de contrôle d’accès configurées et les informations d’identité de l’utilisateur dans le ticket TGT. La vérification d'accès réussit.

4.  Le contrôleur de domaine répond à la demande avec une réponse de service d'accord de tickets (TGS-REP).

## <a name="associated-error-and-informational-event-messages"></a><a name="BKMK_ErrorandEvents"></a>Messages d’erreur et d’événement d’information associés
Le tableau ci-dessous décrit les événements associés au groupe de sécurité Utilisateurs protégés et les stratégies d'authentification appliquées aux silos de stratégies d'authentification.

Les événements sont enregistrés dans le journal Journaux des applications et des services, dans **Microsoft\Windows\Authentication**.

Pour connaître les étapes de dépannage qui utilisent ces événements, voir [Dépanner les stratégies d'authentification](how-to-configure-protected-accounts.md#troubleshoot-authentication-policies) et [Dépanner les événements associés aux Utilisateurs protégés](how-to-configure-protected-accounts.md#troubleshoot-events-related-to-protected-users).

|ID d'événement et journal|Description|
|----------|--------|
|101<p>**AuthenticationPolicyFailures-DomainController**|Raison : un échec de connexion NTLM se produit parce que la stratégie d’authentification est configurée.<p>Un événement est consigné dans le contrôleur de domaine pour indiquer que l'authentification NTLM a échoué parce que des limitations de contrôle d'accès sont requises et qu'elles ne peuvent pas être appliquées à NTLM.<p>Affiche les noms du compte, du périphérique, de la stratégie et du silo.|
|105<p>**AuthenticationPolicyFailures-DomainController**|Raison : un échec de restriction Kerberos se produit parce que l’authentification à partir d’un périphérique particulier n’était pas autorisée.<p>Un événement est consigné dans le contrôleur de domaine pour indiquer qu'un ticket TGT Kerberos a été refusé parce que le périphérique ne satisfaisait pas aux limitations de contrôle d'accès appliquées.<p>Affiche les noms du compte, du périphérique, de la stratégie et du silo, ainsi que la durée de vie du ticket TGT.|
|305<p>**AuthenticationPolicyFailures-DomainController**|Raison : un échec potentiel de restriction Kerberos peut se produire parce que l’authentification à partir d’un périphérique particulier n’était pas autorisée.<p>En mode audit, un événement d'information est consigné dans le contrôleur de domaine pour déterminer si un ticket TGT Kerberos sera refusé parce que le périphérique ne satisfaisait pas aux limitations de contrôle d'accès appliquées.<p>Affiche les noms du compte, du périphérique, de la stratégie et du silo, ainsi que la durée de vie du ticket TGT.|
|106<p>**AuthenticationPolicyFailures-DomainController**|Raison : un échec de restriction Kerberos se produit parce que l’utilisateur ou l’appareil n’a pas été autorisé à s’authentifier auprès du serveur.<p>Un événement est consigné dans le contrôleur de domaine pour indiquer qu'un ticket de service Kerberos a été refusé parce que l'utilisateur, le périphérique ou les deux ne satisfont pas aux limitations de contrôle d'accès appliquées.<p>Affiche les noms du périphérique, de la stratégie et du silo.|
|306<p>**AuthenticationPolicyFailures-DomainController**|Raison : un échec de restriction Kerberos peut se produire parce que l’utilisateur ou l’appareil n’a pas été autorisé à s’authentifier auprès du serveur.<p>En mode audit, un événement d'information est consigné sur le contrôleur de domaine pour indiquer qu'un ticket de service Kerberos sera refusé parce que l'utilisateur, le périphérique ou les deux ne satisfont pas aux limitations de contrôle d'accès.<p>Affiche les noms du périphérique, de la stratégie et du silo.|

## <a name="see-also"></a>Voir aussi
[Comment configurer des comptes protégés](how-to-configure-protected-accounts.md)

[Gestion et protection des informations d'identification](credentials-protection-and-management.md)

[Groupe de sécurité Utilisateurs protégés](protected-users-security-group.md)


