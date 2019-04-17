---
title: "Stratégies d’authentification et Silos de stratégies d’authentification"
description: "Sécurité de Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-credential-protection
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7eb0e640-033d-49b5-ab44-3959395ad567
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 460837c79c0e0d2c48331ddaaffcd118fd16ebc1
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="authentication-policies-and-authentication-policy-silos"></a>Stratégies d’authentification et Silos de stratégies d’authentification

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique destinée aux professionnels de l’informatique décrit les silos de stratégies d’authentification et les stratégies qui peuvent limiter des comptes à ces silos. Elle explique également comment les stratégies d’authentification permet de limiter la portée des comptes.

Silos de stratégies d’authentification et les stratégies qui les accompagnent permettent de contenir des informations d’identification à privilèges élevés sur les systèmes qui ne sont pertinentes pour les utilisateurs sélectionnés, les ordinateurs ou les services. Silos peuvent être définis et gérés dans les Services de domaine ActiveDirectory (ADDS) à l’aide du centre d’administration ActiveDirectory et les applets de commande ActiveDirectory Windows PowerShell.

Silos de stratégies d’authentification sont des conteneurs à laquelle les administrateurs peuvent affecter des comptes d’utilisateurs, les comptes d’ordinateurs et les comptes de service. Ensembles de comptes peuvent être gérés par les stratégies d’authentification qui ont été appliqués à ce conteneur. Cela réduit le besoin de l’administrateur de suivre l’accès aux ressources pour les comptes individuels et permet d’empêcher les utilisateurs malveillants d’accéder à d’autres ressources via le vol d’informations d’identification.

Les fonctions introduites dans Windows Server2012R2, vous autorisant à créer d’authentification silos de stratégies, qui hébergent un ensemble d’utilisateurs à privilèges élevés. Vous pouvez ensuite affecter des stratégies d’authentification pour ce conteneur afin de limiter l’où les comptes privilégiés peuvent être utilisés dans le domaine. Lorsque les comptes se trouvent dans le groupe de sécurité utilisateurs protégés, des contrôles supplémentaires sont appliqués, tels que l’utilisation exclusive du protocole Kerberos.

Grâce à ces fonctionnalités, vous pouvez limiter l’utilisation des comptes de valeur élevée aux hôtes importants. Par exemple, vous pouvez créer un silo administrateurs nouvelle forêt qui contient l’entreprise, schéma et administrateurs de domaine. Puis vous pouvez configurer ce silo avec une stratégie d’authentification afin que le mot de passe et l’authentification par carte à puce à partir des systèmes autres que les contrôleurs de domaine et les consoles d’administrateur de domaine échoue.

Pour plus d’informations sur la configuration des stratégies d’authentification et les silos d’authentification, voir [comment configurer des comptes protégés](how-to-configure-protected-accounts.md).

### <a name="about-authentication-policy-silos"></a>À propos des silos de stratégies d’authentification
Un silo de stratégies d’authentification détermine les comptes qui peuvent être restreints par le silo et définit les stratégies d’authentification à appliquer aux membres. Vous pouvez créer un silo en fonction des besoins de votre organisation. Les silos sont des objets ActiveDirectory pour les utilisateurs, ordinateurs et services, tel que défini par le schéma dans le tableau suivant.

**Schéma ActiveDirectory pour les silos de stratégies d’authentification**

|Nom d’affichage|Description|
|--------|--------|
|Silo de stratégies d’authentification|Une instance de cette classe définit les stratégies d’authentification et des comportements associés pour les utilisateurs, ordinateurs et services.|
|Silos de stratégies d’authentification|Un conteneur de cette classe peut contenir des objets silos de stratégies d’authentification.|
|Silo de stratégies d’authentification appliquée|Spécifie si le silo de stratégies d’authentification est appliqué.<br /><br />Une fois ne pas appliquées, la stratégie par défaut est en mode audit. Les événements qui indiquent le succès ou échecs potentiels sont générés, mais les protections ne sont pas appliquées au système.|
|Lien secondaire Silo de stratégie d’authentification|Cet attribut correspond au lien secondaire pour silo msDS-AssignedAuthNPolicySilo.|
|Membres du Silo de stratégies d’authentification|Spécifie quels principaux sont affectés au silo AuthNPolicySilo.|
|Lien secondaire membres du Silo de stratégie d’authentification|Cet attribut correspond au lien secondaire pour msDS-AuthNPolicySiloMembers.|

Silos de stratégies d’authentification peuvent être configurés à l’aide de la Console d’administration ActiveDirectory ou Windows PowerShell. Pour plus d’informations, voir [comment configurer des comptes protégés](how-to-configure-protected-accounts.md).

### <a name="about-authentication-policies"></a>À propos des stratégies d’authentification
Une stratégie d’authentification définit les propriétés de durée de vie du ticket (TGT ticket) ticket-granting protocole Kerberos et les conditions de contrôle d’accès d’authentification pour un type de compte. La stratégie s’appuie sur et contrôle le conteneur de domaine ActiveDirectory appelé le silo de stratégies d’authentification.

Stratégies d’authentification contrôlent les éléments suivants:

-   La durée de vie du ticket TGT pour le compte, qui est définie comme non renouvelable.

-   Les critères que doivent respecter pour se connecter avec un mot de passe ou un certificat les comptes d’appareil.

-   Les critères que doivent respecter pour s’authentifier auprès des services en cours d’exécution dans le cadre du compte d’utilisateurs et périphériques.

Le type de compte ActiveDirectory détermine le rôle de l’appelant en tant qu’une des opérations suivantes:

-   **Utilisateur**

    Les utilisateurs doivent toujours être membres du groupe de sécurité utilisateurs protégés, lequel rejette les tentatives d’authentification utilisant NTLM par défaut.

    Stratégies peuvent être configurées pour définir la durée de vie du ticket TGT d’un compte d’utilisateur à une valeur plus courte ou de limiter les périphériques auxquels un compte d’utilisateur peut se connecter. Expressions enrichi peuvent être configurées dans la stratégie d’authentification pour contrôler les critères que doivent respecter pour s’authentifier auprès du service les utilisateurs et leurs périphériques.

    Pour plus d’informations, voir [groupe de sécurité utilisateurs protégés](protected-users-security-group.md).

-   **Service**

    Autonome géré les comptes de service, comptes de service géré de groupe ou un objet compte personnalisé dérivé de ces deux types de service, les comptes sont utilisés. Stratégies peuvent définir l’accès d’un appareil, les conditions de contrôle, qui sont utilisées pour limiter les informations d’identification du compte de service administrés à des périphériques spécifiques avec une identité ActiveDirectory. Services ne doivent jamais être membres du groupe de sécurité utilisateurs protégés car toute authentification entrante échouera.

-   **Ordinateur**

    L’objet de compte d’ordinateur ou l’objet compte personnalisé dérivé de l’objet de compte d’ordinateur est utilisé. Stratégies peuvent définir l’accès à des conditions de contrôle qui sont nécessaires pour permettre l’authentification pour le compte basé sur l’utilisateur et les propriétés du périphérique. Les ordinateurs ne doivent jamais être membres du groupe de sécurité utilisateurs protégés car toute authentification entrante échouera. Par défaut, les tentatives d’utilisation de l’authentification NTLM sont rejetées. Durée de vie TGT ne doit pas être configurée pour les comptes d’ordinateur.

> [!NOTE]
> Il est possible de définir une stratégie d’authentification sur un ensemble de comptes sans associer la stratégie à un silo de stratégies d’authentification. Vous pouvez utiliser cette stratégie lorsque vous disposez d’un seul compte à protéger.

**Schéma ActiveDirectory pour les stratégies d’authentification**

Les stratégies pour les objets ActiveDirectory pour les utilisateurs, ordinateurs et services sont définies par le schéma dans le tableau suivant.

|Type|Nom d’affichage|Description|
|----|--------|--------|
|Stratégie|Stratégie d’authentification|Une instance de cette classe définit les comportements de stratégie d’authentification pour les principaux affectés.|
|Stratégie|Stratégies d’authentification|Un conteneur de cette classe peut contenir des objets de stratégie d’authentification.|
|Stratégie|Stratégie d’authentification appliquée|Spécifie si la stratégie d’authentification est appliquée.<br /><br />Une fois ne pas appliquées, la stratégie par défaut est en mode audit et les événements qui indiquent le succès ou échecs potentiels sont générés, mais protections ne sont pas appliquées au système.|
|Stratégie|Lien secondaire de stratégie d’authentification affecté|Cet attribut correspond au lien secondaire pour msDS-AssignedAuthNPolicy.|
|Stratégie|Stratégie d’authentification affecté|Spécifie la stratégie authnpolicy qui doit être appliquée à ce principal.|
|Utilisateur|Stratégie d’authentification utilisateur|Spécifie la stratégie authnpolicy qui doit être appliquée aux utilisateurs qui sont affectés à cet objet silo.|
|Utilisateur|Lien secondaire de stratégie utilisateur d’authentification|Cet attribut correspond au lien secondaire pour msDS-UserAuthNPolicy.|
|Utilisateur|Ms-DS - utilisateur - autorisé-à-authentifier-pour|Cet attribut est utilisé pour déterminer l’ensemble des principaux autorisés à s’authentifier auprès d’un service s’exécutant sous le compte d’utilisateur.|
|Utilisateur|Ms-DS - utilisateur - autorisé-à-authentifier-à partir de|Cet attribut est utilisé pour déterminer l’ensemble des périphériques auxquels un compte d’utilisateur est autorisé à se connecter.|
|Utilisateur|Durée de vie TGT utilisateur|Spécifie l’âge maximal d’un TGT Kerberos émis pour un utilisateur (exprimé en secondes). Les tickets TGT résultants sont non renouvelables.|
|Ordinateur|Stratégie d’authentification de l’ordinateur|Spécifie la stratégie authnpolicy qui doit être appliqué aux ordinateurs qui sont affectés à cet objet silo.|
|Ordinateur|Lien secondaire de stratégie ordinateur d’authentification|Cet attribut correspond au lien secondaire pour msDS-ComputerAuthNPolicy.|
|Ordinateur|Ms-DS - ordinateur - autorisé-à-authentifier-pour|Cet attribut est utilisé pour déterminer l’ensemble des principaux autorisés à s’authentifier auprès d’un service s’exécutant sous le compte d’ordinateur.|
|Ordinateur|Durée de vie TGT ordinateur|Spécifie l’âge maximal d’un TGT Kerberos émis pour un ordinateur (exprimé en secondes). Il n’est pas recommandé de modifier ce paramètre.|
|Service|Stratégie d’authentification de service|Spécifie la stratégie authnpolicy qui doit être appliquée à des services qui sont affectés à cet objet silo.|
|Service|Lien secondaire de stratégie service d’authentification|Cet attribut correspond au lien secondaire pour msDS-ServiceAuthNPolicy.|
|Service|Ms-DS - Service - autorisé-à-authentifier-pour|Cet attribut est utilisé pour déterminer l’ensemble des principaux autorisés à s’authentifier auprès d’un service s’exécutant sous le compte de service.|
|Service|Ms-DS - Service - autorisé-à-authentifier-à partir de|Cet attribut est utilisé pour déterminer l’ensemble des périphériques auxquels un compte de service est autorisé à se connecter.|
|Service|Durée de vie TGT de service|Spécifie l’âge maximal d’un TGT Kerberos émis pour un service (exprimé en secondes).|

Stratégies d’authentification peuvent être configurés pour chaque silo à l’aide de la Console d’administration ActiveDirectory ou Windows PowerShell. Pour plus d’informations, voir [comment configurer des comptes protégés](how-to-configure-protected-accounts.md).

## <a name="how-it-works"></a>Fonctionnement
Cette section décrit comment les stratégies d’authentification et les silos de fonctionnent en conjonction avec le groupe de sécurité utilisateurs protégés et l’implémentation du protocole Kerberos dans Windows.

-   [Comment le protocole Kerberos est utilisé avec les stratégies et silos d’authentification](#BKMK_HowKerbUsed)

-   [Comment limiter une connexion de l’utilisateur fonctionne.](#BKMK_HowRestrictingSignOn)

-   [Comment fonctionne la limitation d’émission de tickets de service](#BKMK_HowRestrictingServiceTicket)

**Comptes protégés**

Le groupe de sécurité utilisateurs protégés déclenche une protection non configurable sur les périphériques et les ordinateurs hôtes exécutant Windows Server2012R2 et Windows8.1 et sur les contrôleurs de domaine dans les domaines avec un contrôleur de domaine principal exécutant Windows Server2012R2. Selon le niveau fonctionnel du domaine du compte, les membres du groupe de sécurité utilisateurs protégés sont davantage protégés en raison de modifications dans les méthodes d’authentification qui sont pris en charge dans Windows.

-   Le membre du groupe de sécurité utilisateurs protégés ne peuvent pas s’authentifier à l’aide de la délégation d’informations d’identification CredSSP par défaut, l’authentification Digest ou NTLM. Sur un appareil exécutant Windows8.1 qui utilise l’un de ces fournisseurs de prise en charge de sécurité (SSP), l’authentification à un domaine échoue lorsque le compte est membre du groupe de sécurité utilisateurs protégés.

-   Le protocole Kerberos n’utilisera pas les types de chiffrement DES ou RC4 plus faibles dans le processus de pré-authentification. Cela signifie que le domaine doit être configuré pour prendre en charge au moins le type de chiffrement AES.

-   Le compte d’utilisateur ne peut pas être délégué avec Kerberos contrainte ou non contrainte délégation. Cela signifie que les connexions antérieures à d’autres systèmes peuvent échouer si l’utilisateur est membre du groupe de sécurité utilisateurs protégés.

-   Le paramètre de durée de vie des tickets TGT Kerberos par défaut de quatre heures est configurable à l’aide de stratégies d’authentification et silos, ce qui est accessible via le centre d’administration ActiveDirectory. Cela signifie que lorsque les quatre heures est dépassée, l’utilisateur doit s’authentifier à nouveau.

Pour plus d’informations sur ce groupe de sécurité, voir [comment les utilisateurs protégés groupe fonctionne](protected-users-security-group.md#BKMK_HowItWorks).

**Silos et stratégies d’authentification**

Stratégies d’authentification et les silos de tirer parti de l’infrastructure d’authentification Windows existante. L’utilisation du protocole NTLM est rejetée et le protocole Kerberos avec les nouveaux types de cryptage est utilisé. Stratégies d’authentification complètent le groupe de sécurité utilisateurs protégés en fournissant un moyen d’appliquer des restrictions configurables aux comptes, en plus de fournir des restrictions pour les comptes de services et les ordinateurs. Stratégies d’authentification sont appliquées pendant le service d’authentification Kerberos protocol (AS) ou le ticket-granting un échange de service (TGS). Pour plus d’informations sur la façon dont Windows utilise le protocole Kerberos, et quelles modifications ont été apportées pour prendre en charge les silos de stratégies d’authentification et les stratégies d’authentification, voir:

-   [Comment fonctionne le protocole d’authentification 5Kerberos Version](https://technet.microsoft.com/library/cc772815(v=ws.10).aspx)

-   [Modifications apportées à l’authentification Kerberos](https://technet.microsoft.com/library/dd560670(v=ws.10).aspx) (Windows Server2008R2 et Windows7)

### <a name="BKMK_HowKerbUsed"></a>Comment le protocole Kerberos est utilisé avec les stratégies et silos de stratégies d’authentification
Lorsqu’un compte de domaine est lié à un silo de stratégies d’authentification et l’utilisateur se connecte, le Gestionnaire de comptes de sécurité ajoute le type de revendication de Silo de stratégies d’authentification qui inclut le silo comme valeur. Cette revendication sur le compte fournit l’accès au silo ciblé.

Lorsqu’une stratégie d’authentification est appliquée et la demande de service d’authentification pour un compte de domaine est reçue sur le contrôleur de domaine, le contrôleur de domaine retourne un ticket TGT non renouvelable avec la durée de vie configurée (sauf si la durée de vie de ticket TGT de domaine est plus courte).

> [!NOTE]
> Le compte de domaine doit avoir une durée de vie TGT configurée et doit être directement lié à la stratégie ou indirectement lié via son appartenance au silo.

Lorsqu’une stratégie d’authentification est en mode audit et la demande de service d’authentification pour un compte de domaine est reçue sur le contrôleur de domaine, le contrôleur de domaine vérifie si l’authentification est autorisée pour le périphérique afin qu’il puisse consigner un avertissement en cas de panne. Une stratégie d’authentification auditée n’altère pas le processus, afin de demandes d’authentification n’échoueront pas si elles ne répondent pas aux exigences de la stratégie.

> [!NOTE]
> Le compte de domaine doit être directement lié à la stratégie ou indirectement lié via son appartenance au silo.

Lorsqu’une stratégie d’authentification est appliquée et que le service d’authentification est blindé, la demande de service d’authentification pour un compte de domaine est reçue sur le contrôleur de domaine, le contrôleur de domaine vérifie si l’authentification est autorisée pour le périphérique. En cas d’échec, le contrôleur de domaine renvoie un message d’erreur et consigne un événement.

> [!NOTE]
> Le compte de domaine doit être directement lié à la stratégie ou indirectement lié via son appartenance au silo.

Quand une stratégie d’authentification est en mode audit et une demande de service accord de tickets est reçue par le contrôleur de domaine pour un compte de domaine, le contrôleur de domaine vérifie si l’authentification est autorisée, selon ticket de la demande données Privilege Attribute Certificate certificat (), et il consigne un message d’avertissement en cas d’échec. Le certificat PAC contient divers types de données d’autorisation, y compris les groupes dont l’utilisateur est membre de droits de que l’utilisateur a, et les stratégies qui s’appliquent à l’utilisateur. Ces informations sont utilisées pour générer un jeton d’accès de l’utilisateur. S’il s’agit d’une stratégie d’authentification appliquée qui permet l’authentification à un utilisateur, un périphérique ou un service, les vérifications de contrôleur de domaine si l’authentification est autorisée basent sur le ticket de la demande données PAC. En cas d’échec, le contrôleur de domaine renvoie un message d’erreur et consigne un événement.

> [!NOTE]
> Le compte de domaine doit être directement lié ou lié via son appartenance silo à une stratégie d’authentification auditée qui permet l’authentification à un utilisateur, un périphérique ou un service

Vous pouvez utiliser une stratégie d’authentification unique pour tous les membres d’un silo, ou vous pouvez utiliser des stratégies distinctes pour les utilisateurs, ordinateurs et comptes de service administrés.

Stratégies d’authentification peuvent être configurés pour chaque silo à l’aide de la Console d’administration ActiveDirectory ou Windows PowerShell. Pour plus d’informations, voir [comment configurer des comptes protégés](how-to-configure-protected-accounts.md).

### <a name="BKMK_HowRestrictingSignOn"></a>Comment limiter une connexion de l’utilisateur fonctionne.
Étant donné que ces stratégies d’authentification sont appliquées à un compte, il s’applique également aux comptes qui sont utilisés par les services. Si vous souhaitez limiter l’utilisation d’un mot de passe pour un service à des hôtes spécifiques, ce paramètre est utile. Par exemple, groupe managed service les comptes sont configurés dans lequel les ordinateurs hôtes sont autorisés à récupérer le mot de passe à partir des Services de domaine ActiveDirectory. Toutefois, ce mot de passe peut servir à partir de n’importe quel hôte pour l’authentification initiale. En appliquant une condition de contrôle d’accès, une couche supplémentaire de protection peut être obtenue en limitant le mot de passe uniquement l’ensemble des ordinateurs hôtes qui peut récupérer le mot de passe.

Lorsque les services qui s’exécutent en tant que système, service réseau ou autres identité de service local se connectent aux services réseau, ils utilisent le compte d’ordinateur de l’ordinateur hôte. Comptes d’ordinateur ne peut pas être restreints. Par conséquent, même si le service utilise un compte d’ordinateur qui n’est pas pour un ordinateur hôte Windows, il ne peut pas être restreint.

Restriction de connexion de l’utilisateur à des hôtes spécifiques exige le contrôleur de domaine pour valider l’identité de l’ordinateur hôte. Lorsque vous utilisez l’authentification Kerberos avec le blindage Kerberos (qui fait partie du contrôle d’accès dynamique), le centre de Distribution de clés est fourni avec le ticket TGT de l’ordinateur hôte à partir de laquelle l’utilisateur s’authentifie. Le contenu de ce ticket TGT blindé sert à effectuer une vérification d’accès pour déterminer si l’ordinateur hôte est autorisé.

Lorsqu’un utilisateur se connecte à Windows ou entre leurs informations d’identification de domaine dans une invite d’informations d’identification pour une application, par défaut, Windows envoie une demande AS-REQ non blindée au contrôleur de domaine. Si l’utilisateur envoie la demande à partir d’un ordinateur ne prenant pas en charge le blindage, tels que des ordinateurs exécutant Windows7 ou WindowsVista, Échec de la demande.

La liste suivante décrit le processus:

-   Le contrôleur de domaine dans un domaine exécutant Windows Server2012R2 interroge pour le compte d’utilisateur et détermine si elle est configurée avec une stratégie d’authentification qui limite l’authentification initiale qui exige des demandes blindées.

-   Le contrôleur de domaine fait échouer la requête.

-   Comme un blindage est requis, l’utilisateur peut tenter de se connecter à l’aide d’un ordinateur exécutant Windows8.1 ou Windows8, qui est activé pour prendre en charge le blindage Kerberos pour recommencer le processus de connexion.

-   Windows détecte que le domaine prend en charge le blindage Kerberos et envoie une AS-REQ blindée pour réexécuter la demande de connexion.

-   Le contrôleur de domaine effectue une vérification d’accès en utilisant les conditions de contrôle d’accès configurées et informations d’identité du système d’exploitation client dans le ticket TGT qui a servi à blinder la demande.

-   Si la vérification d’accès échoue, le contrôleur de domaine rejette la demande.

Même lorsque les systèmes d’exploitation prennent en charge le blindage Kerberos, les exigences de contrôle d’accès peuvent être appliquées et doivent être remplies avant que l’accès est accordé. Les utilisateurs se connecter à Windows ou entrer leurs informations d’identification de domaine dans une invite d’informations d’identification pour une application. Par défaut, Windows envoie une demande AS-REQ non blindée au contrôleur de domaine. Si l’utilisateur envoie la demande à partir d’un ordinateur qui prend en charge le blindage, tels que Windows8.1 ou Windows8, les stratégies d’authentification sont évaluées comme suit:

1.  Le contrôleur de domaine dans un domaine exécutant Windows Server2012R2 interroge pour le compte d’utilisateur et détermine si elle est configurée avec une stratégie d’authentification qui limite l’authentification initiale qui exige des demandes blindées.

2.  Le contrôleur de domaine effectue une vérification d’accès en utilisant les conditions de contrôle d’accès configurées et informations d’identité du système dans le ticket TGT qui sert à blinder la demande. La vérification d’accès réussit.

    > [!NOTE]
    > Si les restrictions de groupe de travail héritées sont configurées, ceux doivent également être respectées.

3.  Le contrôleur de domaine répond avec une réponse blindée (AS-REP) et l’authentification continue.

### <a name="BKMK_HowRestrictingServiceTicket"></a>Comment fonctionne la limitation d’émission de tickets de service
Quand un compte n’est pas autorisé et un utilisateur disposant d’un ticket TGT essaie de se connecter au service (par exemple en ouvrant une application qui exige une authentification à un service qui est identifié par un nom principal de service du service (SPN), la séquence suivante se produit:

1.  Dans une tentative de connexion à SPN1 à partir de SPN, Windows envoie une demande TGS-REQ au contrôleur de domaine qui demande un ticket de service à SPN1.

2.  Le contrôleur de domaine dans un domaine exécutant Windows Server2012R2 recherche spn1 pour trouver le compte de Services de domaine ActiveDirectory pour le service et détermine que le compte est configuré avec une stratégie d’authentification qui limite l’émission de tickets de service.

3.  Le contrôleur de domaine effectue une vérification d’accès en utilisant les conditions de contrôle d’accès configurées et les informations d’identité de l’utilisateur dans le ticket TGT. La vérification d’accès échoue.

4.  Le contrôleur de domaine rejette la demande.

Lorsqu’un compte est autorisé, car le compte remplit les conditions de contrôle d’accès qui sont définies par la stratégie d’authentification et un utilisateur disposant d’un ticket TGT essaie de se connecter au service (par exemple en ouvrant une application qui exige une authentification à un service qui est identifié par le nom SPN du service), la séquence suivante se produit:

1.  Dans une tentative de connexion à SPN1, Windows envoie une demande TGS-REQ au contrôleur de domaine qui demande un ticket de service à SPN1.

2.  Le contrôleur de domaine dans un domaine exécutant Windows Server2012R2 recherche spn1 pour trouver le compte de Services de domaine ActiveDirectory pour le service et détermine que le compte est configuré avec une stratégie d’authentification qui limite l’émission de tickets de service.

3.  Le contrôleur de domaine effectue une vérification d’accès en utilisant les conditions de contrôle d’accès configurées et les informations d’identité de l’utilisateur dans le ticket TGT. La vérification d’accès réussit.

4.  Le contrôleur de domaine répond à la demande avec une réponse du service accord de tickets (TGS-REP).

## <a name="BKMK_ErrorandEvents"></a>Erreur associée et les messages d’événement d’information
Le tableau suivant décrit les événements qui sont associés au groupe de sécurité utilisateurs protégés et les stratégies d’authentification qui sont appliquées aux silos de stratégies d’authentification.

Les événements sont enregistrés dans les journaux des Applications et Services à **Microsoft\Windows\Authentication**.

Pour la résolution des problèmes des étapes qui utilisent ces événements, voir [dépanner les stratégies d’authentification](how-to-configure-protected-accounts.md#BKMK_TroubleshootAuthnPolicies) et [résoudre les événements associés aux utilisateurs protégés](how-to-configure-protected-accounts.md#BKMK_TrubleshootingEvents).

|ID d’événement et journal|Description|
|----------|--------|
|101<br /><br />**AuthenticationPolicyFailures-DomainController.**|Cause: Un échec de connexion NTLM se produit car la stratégie d’authentification est configurée.<br /><br />Un événement est consigné dans le contrôleur de domaine pour indiquer que l’authentification NTLM a échoué, car les restrictions de contrôle d’accès sont nécessaires, et ces restrictions ne peuvent pas être appliquées à NTLM.<br /><br />Affiche le compte d’appareil, stratégie et silo de noms.|
|105<br /><br />**AuthenticationPolicyFailures-DomainController.**|Cause: Un échec de limitation Kerberos se produit car l’authentification à partir d’un périphérique particulier n’était pas autorisée.<br /><br />Un événement est consigné dans le contrôleur de domaine pour indiquer qu’un TGT Kerberos a été refusé parce que le périphérique ne satisfaisait pas aux restrictions de contrôle d’accès appliquées.<br /><br />Affiche le compte, appareil, stratégie, les noms de silo et durée de vie TGT.|
|305<br /><br />**AuthenticationPolicyFailures-DomainController.**|Cause: Un échec de limitation Kerberos potentiel peut se produire parce que l’authentification à partir d’un périphérique particulier n’était pas autorisée.<br /><br />En mode audit, un événement d’information est consigné dans le contrôleur de domaine pour déterminer si un TGT Kerberos sera refusé parce que le périphérique ne satisfaisait pas aux restrictions de contrôle d’accès.<br /><br />Affiche le compte, appareil, stratégie, les noms de silo et durée de vie TGT.|
|106<br /><br />**AuthenticationPolicyFailures-DomainController.**|Cause: Un échec de limitation Kerberos se produit car l’utilisateur ou le périphérique n’était pas autorisé à s’authentifier auprès du serveur.<br /><br />Un événement est consigné dans le contrôleur de domaine pour indiquer qu’un ticket de service Kerberos a été refusé parce que l’utilisateur, le périphérique ou les deux ne répondent pas aux limitations de contrôle d’accès appliquées.<br /><br />Affiche le périphérique, la stratégie et silo de noms.|
|306<br /><br />**AuthenticationPolicyFailures-DomainController.**|Cause: Un échec de limitation Kerberos peut se produire parce que l’utilisateur ou le périphérique n’était pas autorisé à s’authentifier auprès du serveur.<br /><br />En mode audit, un événement d’information est consigné sur le contrôleur de domaine pour indiquer qu’un ticket de service Kerberos sera refusé parce que l’utilisateur, le périphérique ou les deux ne répondent pas aux restrictions de contrôle d’accès.<br /><br />Affiche le périphérique, la stratégie et silo de noms.|

## <a name="see-also"></a>Voir aussi
[Comment configurer des comptes protégés](how-to-configure-protected-accounts.md)

[Gestion et Protection des informations d’identification](credentials-protection-and-management.md)

[Groupe de sécurité utilisateurs protégés](protected-users-security-group.md)


