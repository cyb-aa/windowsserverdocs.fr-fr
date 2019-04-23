---
title: Stratégies de requête de connexion
description: Cette rubrique fournit une vue d’ensemble de stratégies de demande de connexion de serveur NPS dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4ec45e0c-6b37-4dfb-8158-5f40677b0157
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 21330b3838064c7b31e78ef70bdc04f0db0f50db
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872940"
---
# <a name="connection-request-policies"></a>Stratégies de requête de connexion

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour savoir comment utiliser des stratégies de demande de connexion de serveur NPS pour configurer le serveur NPS comme serveur RADIUS, proxy RADIUS ou les deux.

>[!NOTE]
>Outre cette rubrique, la documentation de stratégie de demande connexion suivante est disponible.
> - [Configurer des stratégies de demande de connexion](nps-crp-configure.md)
> - [Configurer des groupes de serveurs RADIUS distants](nps-crp-rrsg-configure.md)

Les stratégies de demande de connexion sont des ensembles de conditions et paramètres qui permettent aux administrateurs réseau de désigner les serveurs d’authentification Dial-In Service RADIUS (Remote User), effectuez l’authentification et autorisation de connexion demande que le serveur exécutant le serveur NPS (Network Policy Server) reçoit des clients RADIUS. Stratégies de demande de connexion peuvent être configurées pour désigner les serveurs RADIUS sont utilisés pour la gestion des comptes RADIUS.

Vous pouvez créer connexion de stratégies de demande afin que certains messages de requête RADIUS envoyées à partir de clients RADIUS sont traités localement (NPS est utilisé comme un serveur RADIUS) et autres types de messages sont transférés vers un autre serveur RADIUS (NPS est utilisé en tant que proxy RADIUS).

Avec les stratégies de demande de connexion, vous pouvez utiliser NPS comme serveur RADIUS ou en tant que proxy RADIUS, en fonction de facteurs tels que les éléments suivants : 

- L’heure et du jour de la semaine
- Le nom de domaine dans la demande de connexion
- Le type de connexion demandée
- L’adresse IP du client RADIUS

Messages de demande d’accès RADIUS sont traitées ou transmises par le serveur NPS uniquement si les paramètres du message entrant correspond au moins une des stratégies de demande de connexion configurées sur le serveur NPS. 

Si les paramètres de stratégie correspond à, et la stratégie exige que le serveur NPS traite le message, NPS se comporte comme un serveur RADIUS, authentification et autorisation de la demande de connexion. Si les paramètres de stratégie correspond à, et la stratégie exige que le serveur NPS transfère le message, NPS agit comme un proxy RADIUS et transfère la demande de connexion à un serveur RADIUS à distance pour le traitement.

Si les paramètres d’un message de demande d’accès RADIUS entrant ne correspondent pas au moins une des stratégies de demande de connexion, un message de refus d’accès est envoyé au client RADIUS et l’utilisateur ou l’ordinateur tente de se connecter au réseau de l’accès est refusé.

## <a name="configuration-examples"></a>Exemples de configuration

Les exemples de configuration suivants montrent comment vous pouvez utiliser les stratégies de demande de connexion.

### <a name="nps-as-a-radius-server"></a>NPS comme serveur RADIUS

La stratégie de demande de connexion par défaut est la seule stratégie configurée. Dans cet exemple, NPS est configuré comme un serveur RADIUS et toutes les demandes de connexion sont traitées par le serveur NPS local. Le serveur NPS peut authentifier et autoriser les utilisateurs dont les comptes sont dans le domaine du domaine du serveur NPS et dans des domaines approuvés.


### <a name="nps-as-a-radius-proxy"></a>NPS en tant que proxy RADIUS

La stratégie de demande de connexion par défaut est supprimée, et deux nouvelles stratégies de demande de connexion sont créés pour transférer les requêtes vers deux domaines différents. Dans cet exemple, NPS est configuré en tant que proxy RADIUS. NPS ne traite pas les demandes de connexion sur le serveur local. Au lieu de cela, il transfère les demandes de connexion à NPS ou d’autres serveurs RADIUS qui sont configurés en tant que membres de groupes de serveurs RADIUS distants.


### <a name="nps-as-both-radius-server-and-radius-proxy"></a>NPS en tant que serveur RADIUS et proxy RADIUS

En plus de la stratégie de demande de connexion par défaut, une nouvelle stratégie de demande de connexion est créée qui transfère les demandes de connexion à un serveur NPS ou un autre serveur RADIUS dans un domaine non approuvé. Dans cet exemple, la stratégie proxy apparaît en premier dans la liste ordonnée des stratégies. Si la demande de connexion correspond à la stratégie de proxy, la demande de connexion est transférée au serveur RADIUS dans le groupe de serveurs RADIUS distants. Si la demande de connexion ne correspond pas à la stratégie de proxy, mais ne correspond pas à la stratégie de demande de connexion par défaut, le serveur NPS traite la demande de connexion sur le serveur local. Si la demande de connexion ne correspond pas à une stratégie, elle est ignorée.

### <a name="nps-as-radius-server-with-remote-accounting-servers"></a>NPS en tant que serveur RADIUS avec des serveurs de gestion à distance

Dans cet exemple, le serveur NPS local n’est pas configuré pour effectuer la gestion des comptes et la stratégie de demande de connexion par défaut est révisée afin que les messages de gestion des comptes RADIUS sont transférés vers un serveur NPS ou un autre serveur RADIUS dans un groupe de serveurs RADIUS distants. Bien que les messages de comptabilité sont transférés, messages d’authentification et d’autorisation ne sont pas transférés et le serveur NPS local exécute ces fonctions pour le domaine local et tous les domaines approuvés.


### <a name="nps-with-remote-radius-to-windows-user-mapping"></a>NPS avec RADIUS à distance au mappage d’utilisateur Windows

Dans cet exemple, NPS sert à la fois un serveur RADIUS et proxy RADIUS pour chaque demande de connexion en transférant la demande d’authentification à un serveur RADIUS à distance lors de l’utilisation d’un compte d’utilisateur Windows local pour l’autorisation. Cette configuration est implémentée en configurant le rayon à distance à l’attribut de mappage de l’utilisateur Windows en tant que condition de la stratégie de demande de connexion. (En outre, un compte d’utilisateur doit être créé localement qui a le même nom que le compte d’utilisateur distant sur lequel l’authentification est effectuée par le serveur RADIUS à distance.)

## <a name="connection-request-policy-conditions"></a>Conditions de stratégie de demande de connexion

Conditions de stratégie de demande de connexion sont un ou plusieurs attributs RADIUS qui sont comparés aux attributs du message de demande d’accès RADIUS entrant. S’il existe plusieurs conditions, toutes les conditions dans la connexion de message de demande, puis dans la demande de connexion stratégie doit correspondre pour la stratégie soit appliquée par NPS.


Voici les attributs disponibles que vous pouvez configurer dans les stratégies de demande de connexion.

### <a name="connection-properties-attribute-group"></a>Groupe d’attributs de propriétés de connexion

Le groupe d’attributs de propriétés de connexion contient les attributs suivants.

- **Protocole de trames**. Utilisé pour indiquer que le type de trame pour les paquets entrants. X.25, Frame Relay, protocole PPP (point) et SLIP Serial Line Internet Protocol () sont des exemples.
- **Type de service**. Utilisé pour indiquer que le type de service demandé. Exemples encadrée (par exemple, les connexions PPP) et ouverture de session (par exemple, les connexions Telnet). Pour plus d’informations sur les types de service RADIUS, consultez la RFC 2865, « Remote Authentication Dial-in User Service (RADIUS) ».
- **Type de tunnel**. Utilisé pour désigner le type de tunnel est créé par le client demandeur. Types de tunnel incluent le protocole de Tunneling de point à point (PPTP) et le Layer Two Tunneling Protocol (L2TP).

### <a name="day-and-time-restrictions-attribute-group"></a>Groupe d’attributs jour et les Restrictions de temps 

Le groupe d’attributs jour et les Restrictions de temps contient l’attribut de jour et les Restrictions de temps. Avec cet attribut, vous pouvez désigner le jour de la semaine et l’heure du jour de la tentative de connexion. Le jour et l’heure est relatif à la journée et l’heure du serveur NPS.

### <a name="gateway-attribute-group"></a>Groupe d’attributs de passerelle

Le groupe d’attributs de passerelle contient les attributs suivants.

- **Appelé ID de Station**. Utilisé pour désigner le numéro de téléphone du serveur d’accès réseau. Cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe des critères spéciaux pour spécifier les indicatifs.
- **Identificateur NAS**. Utilisé pour désigner le nom du serveur d’accès réseau. Cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe des critères spéciaux pour spécifier les identificateurs NAS.
- **Adresse IPv4 NAS**. Utilisé pour désigner l’Internet Protocol version 4 (IPv4) adresse du serveur d’accès réseau (le client RADIUS). Cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe des critères spéciaux pour spécifier des réseaux IP.
- **Adresse IPv6 NAS**. Utilisé pour désigner l’Internet Protocol version 6 (IPv6) adresse du serveur d’accès réseau (le client RADIUS). Cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe des critères spéciaux pour spécifier des réseaux IP.
- **Type de Port NAS**. Utilisé pour désigner le type de média utilisé par le client d’accès. Les lignes téléphoniques analogiques (appelés async), réseau numérique à intégration de Services (RNIS), les tunnels ou réseaux privés virtuels (VPN), IEEE 802.11 sans fil, sont des exemples et les commutateurs Ethernet.

### <a name="machine-identity-attribute-group"></a>Groupe d’attributs identité machine

Groupe d’attributs de l’identité de l’ordinateur contient l’attribut de l’identité de l’ordinateur. À l’aide de cet attribut, vous pouvez spécifier la méthode avec laquelle les clients sont identifiés dans la stratégie.

### <a name="radius-client-properties-attribute-group"></a>Groupe d’attributs des propriétés de Client RADIUS

Le groupe d’attributs de propriétés du Client RADIUS contient les attributs suivants.

- **ID de la Station appelante**. Utilisé pour désigner le numéro de téléphone utilisé par l’appelant (le client d’accès). Cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe des critères spéciaux pour spécifier les indicatifs.  Dans les authentifications de 802. 1 x, l’adresse MAC est généralement créé et peut être mis en correspondance à partir du client.  Ce champ est généralement utilisé pour les scénarios de contournement d’adresse Mac lorsque la stratégie de demande de connexion est configurée pour accepter les utilisateurs sans valider les informations d’identification.  
- **Nom convivial du client**. Utilisé pour désigner le nom de l’ordinateur client RADIUS qui demande une authentification. Cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe des critères spéciaux pour spécifier les noms de client.
- **Adresse IPv4 du client**. Utilisé pour désigner l’adresse IPv4 du serveur d’accès réseau (le client RADIUS). Cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe des critères spéciaux pour spécifier des réseaux IP.
- **Adresse IPv6 du client**. Utilisé pour désigner l’adresse IPv6 du serveur d’accès réseau (le client RADIUS). Cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe des critères spéciaux pour spécifier des réseaux IP.
- **Fournisseur de client**. Utilisé pour spécifier le fournisseur du serveur d’accès réseau qui demande une authentification. Un ordinateur exécutant le service Routage et accès distant est le fabricant NAS Microsoft. Vous pouvez utiliser cet attribut pour configurer des stratégies distinctes pour les différents constructeurs NAS. Cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe des critères spéciaux.

### <a name="user-name-attribute-group"></a>Groupe d’attributs nom utilisateur

Le groupe d’attributs de nom d’utilisateur contient l’attribut de nom d’utilisateur. À l’aide de cet attribut, vous pouvez désigner le nom d’utilisateur, ou une partie du nom d’utilisateur, qui doit correspondre au nom d’utilisateur fourni par le client d’accès dans le message RADIUS. Cet attribut est une chaîne de caractères qui contient généralement un nom de domaine et un nom de compte d’utilisateur. Vous pouvez utiliser la syntaxe des critères spéciaux pour spécifier des noms d’utilisateur.

## <a name="connection-request-policy-settings"></a>Paramètres de stratégie de demande de connexion

Paramètres de stratégie de demande de connexion sont un ensemble de propriétés qui sont appliqués à un message RADIUS entrant. Paramètres correspondent à des groupes de propriétés suivants.

- Authentification
- Gestion des comptes
- Manipulation d’attribut
- Demande de transfert
- Avancé

Les sections suivantes fournissent des détails supplémentaires sur ces paramètres.

### <a name="authentication"></a>Authentification

À l’aide de ce paramètre, vous pouvez remplacer les paramètres d’authentification qui sont configurés dans toutes les stratégies réseau et vous pouvez désigner les méthodes d’authentification et les types qui sont requises pour se connecter à votre réseau.

>[!IMPORTANT]
>Si vous configurez une méthode d’authentification dans la stratégie de demande de connexion qui est moins sécurisée que la méthode d’authentification que vous configurez dans la stratégie réseau, la méthode d’authentification plus sécurisée que vous configurez dans la stratégie de réseau est remplacée. Par exemple, si vous avez une stratégie de réseau qui nécessite l’utilisation de Protected Extensible Authentication Protocol-Microsoft Challenge Handshake Authentication Protocol version 2 \(PEAP-MS-CHAP v2\), qui est basée sur un mot de passe méthode d’authentification sans fil sécurisé et également configurer une stratégie de demande de connexion pour autoriser l’accès non authentifié, le résultat est qu’aucun client n’est nécessaires pour s’authentifier à l’aide de PEAP-MS-CHAP v2. Dans cet exemple, tous les clients qui se connectent à votre réseau sont accordées tout accès non authentifié.

### <a name="accounting"></a>Gestion des comptes

À l’aide de ce paramètre, vous pouvez configurer la stratégie de demande de connexion pour transférer les informations de comptabilité à un serveur NPS ou un autre serveur RADIUS dans un groupe de serveurs RADIUS distants afin que le groupe de serveurs RADIUS distants effectue la gestion des comptes.

>[!NOTE]
>Si vous avez plusieurs serveurs RADIUS et que vous souhaitez que les informations de gestion pour tous les serveurs sont stockées dans une base de gestion des comptes RADIUS centrale, vous pouvez utiliser le paramètre comptable de stratégie de demande de connexion dans une stratégie sur chaque serveur RADIUS pour transférer des données de gestion des comptes à partir de tous les serveurs à un serveur NPS ou tout autre serveur RADIUS qui est désigné comme un serveur de gestion des comptes.

Demande de connexion paramètres de stratégie de gestion des comptes fonctionnent indépendamment de la configuration de gestion de serveur NPS local. En d’autres termes, si vous configurez le serveur NPS local pour enregistrer les informations de gestion des comptes RADIUS vers un fichier local ou à une base de données Microsoft SQL Server, il procédera ainsi même si vous configurez une stratégie de demande de connexion pour transférer les messages de gestion des comptes RADIUS à distance groupe de serveurs.

Si vous souhaitez des informations de comptabilité consignées à distance mais pas localement, vous devez configurer le serveur NPS local pour ne pas effectuer de comptabilité, mais aussi configurer la comptabilité dans une stratégie de demande de connexion pour transférer des données de comptabilité à un groupe de serveurs RADIUS distants.

### <a name="attribute-manipulation"></a>Manipulation d’attribut

Vous pouvez configurer un ensemble de règles de recherche et de remplacement qui manipulent les chaînes de texte d’un des attributs suivants.

- Nom d'utilisateur
- ID de la Station appelée
- ID de la Station appelante

Le traitement des règles de recherche et remplacement se produit pour l’un des attributs précédents avant que le message RADIUS est soumis aux paramètres d’authentification et de comptabilité. Règles de manipulation d’attribut s’appliquent uniquement à un seul attribut. Vous ne pouvez pas configurer des règles de manipulation d’attribut pour chaque attribut. En outre, la liste des attributs que vous pouvez manipuler est une liste statique ; Vous ne pouvez pas ajouter à la liste des attributs disponibles pour la manipulation.

>[!NOTE]
>Si vous utilisez le protocole d’authentification MS-CHAP v2, vous ne pouvez pas manipuler l’attribut de nom d’utilisateur si la stratégie de demande de connexion est utilisée pour transférer le message RADIUS. La seule exception se produit lorsqu’une barre oblique inverse (\) caractère est utilisé et la manipulation affecte uniquement les informations à gauche de celui-ci. Une barre oblique inverse est généralement utilisée pour indiquer un nom de domaine (les informations à gauche du caractère barre oblique inverse) et un nom de compte d’utilisateur au sein du domaine (les informations à droite du caractère barre oblique inverse). Dans ce cas, uniquement les règles de manipulation d’attribut qui modifient ou remplacent le nom de domaine sont autorisées.

Pour obtenir des exemples montrant comment manipuler le nom de domaine dans l’attribut de nom d’utilisateur, consultez la section « Exemples pour la manipulation du nom de domaine dans l’attribut de nom d’utilisateur » dans la rubrique [utiliser des Expressions régulières dans NPS](nps-crp-reg-expressions.md).

### <a name="forwarding-request"></a>Demande de transfert

Vous pouvez définir ce qui suit les options de requête qui sont utilisées pour les messages de demande d’accès RADIUS de transfert :

- **Authentifier les demandes sur ce serveur**. À l’aide de ce paramètre, NPS utilise un domaine Windows NT 4.0, Active Directory ou la base de données de comptes d’utilisateur Gestionnaire de comptes de sécurité (SAM) local pour authentifier la demande de connexion. Ce paramètre spécifie également que la stratégie réseau correspondante configurée dans NPS, ainsi que les propriétés d’accès à distance du compte d’utilisateur, sont utilisées par NPS pour autoriser la demande de connexion. Dans ce cas, le serveur NPS est configuré pour fonctionner comme un serveur RADIUS.

- **Transférer les demandes vers le groupe de serveurs RADIUS distant suivant**. À l’aide de ce paramètre, NPS transfère les demandes de connexion au groupe de serveurs RADIUS distant que vous spécifiez. Si le serveur NPS reçoit un message d’acceptation d’accès valide qui correspond au message de demande d’accès, la tentative de connexion est considéré comme authentifié et autorisé. Dans ce cas, le serveur NPS joue le rôle proxy RADIUS.

- **Accepter les utilisateurs sans valider les informations d’identification**. À l’aide de ce paramètre, NPS ne vérifie pas l’identité de l’utilisateur tente de se connecter au réseau et NPS ne tente pas de vérifier que l’utilisateur ou l’ordinateur a le droit de se connecter au réseau. Lorsque NPS est configuré pour autoriser l’accès non authentifié et qu’il reçoit une demande de connexion, NPS envoie immédiatement qu'un message d’acceptation d’accès du rayon de client et l’utilisateur ou l’ordinateur est accordé l’accès au réseau. Ce paramètre est utilisé pour certains types de tunneling obligatoire où le client d’accès tunnel avant que les informations d’identification de l’utilisateur sont authentifiées.

>[!NOTE]
>Cette option d’authentification ne peut pas être utilisée lorsque le protocole d’authentification du client d’accès est MS-CHAP v2 ou Extensible Authentication Protocol-Transport Layer Security (EAP-TLS), qui fournissent l’authentification mutuelle. Dans l’authentification mutuelle, le client d’accès prouve qu’il est un client d’accès valide pour le serveur d’authentification (le serveur NPS) et le serveur d’authentification prouve qu’il est un serveur d’authentification valid pour le client d’accès. Lorsque cette option d’authentification est utilisée, le message d’acceptation d’accès est retourné. Toutefois, le serveur d’authentification ne fournit pas de validation au client d’accès, et l’authentification mutuelle échoue.

Pour obtenir des exemples montrant comment utiliser des expressions régulières pour créer des règles de routage qui transfèrent les messages RADIUS avec un nom de domaine Kerberos spécifié à un groupe de serveurs RADIUS distants, consultez la section « Exemple pour le transfert des messages RADIUS par un serveur proxy » dans la rubrique [utiliser Les Expressions régulières dans NPS](nps-crp-reg-expressions.md).

### <a name="advanced"></a>Avancé

Vous pouvez définir des propriétés avancées pour spécifier la série d’attributs RADIUS qui sont :

- Ajouté au message de réponse RADIUS lorsque le serveur NPS est utilisé en tant que l’authentification RADIUS ou serveur de gestion des comptes. Lorsqu’il existe des attributs spécifiés dans une stratégie de réseau et la stratégie de demande de connexion, les attributs qui sont envoyés dans le message de réponse RADIUS sont la combinaison des deux ensembles d’attributs.
- Ajouté au message RADIUS lorsque le serveur NPS est utilisé en tant que l’authentification RADIUS ou gestion des comptes proxy. Si l’attribut existe déjà dans le message qui est transmis, elle est remplacée par la valeur de l’attribut spécifié dans la stratégie de demande de connexion. 

En outre, certains attributs qui sont disponibles pour la configuration de la connexion demandent une stratégie **paramètres** onglet dans le **avancé** catégorie fournissent des fonctionnalités spécialisées. Par exemple, vous pouvez configurer le **RADIUS distant vers le mappage de l’utilisateur Windows** attribut lorsque vous souhaitez fractionner l’authentification et l’autorisation d’une demande de connexion entre deux comptes de bases de données.

Le **RADIUS distant vers le mappage de l’utilisateur Windows** attribut spécifie que l’autorisation Windows se produit pour les utilisateurs sont authentifiés par un serveur RADIUS à distance. En d’autres termes, un serveur RADIUS à distance effectue l’authentification auprès d’un compte d’utilisateur dans une base de données de comptes d’utilisateur distant, mais le serveur NPS local autorise la demande de connexion par rapport à un compte d’utilisateur dans une base de données de comptes d’utilisateur local. Cela est utile lorsque vous souhaitez autoriser les visiteurs de l’accès à votre réseau.

Par exemple, les visiteurs d’organisations partenaires peuvent être authentifiés par leur propre serveur RADIUS organisation partenaire et puis utilisent un compte d’utilisateur Windows dans votre organisation à accéder à un invité réseau local (LAN) sur votre réseau.

Autres attributs qui fournissent des fonctionnalités spécialisées sont :

- **MS-Quarantine-IPFilter et MS-Quarantine-Session-Timeout**. Ces attributs sont utilisés lorsque vous déployez Network Access Quarantine contrôle d ' avec votre déploiement de routage et accès à distance VPN.
- **Passport-utilisateur-mappage--suffixe UPN**. Cet attribut vous permet d’authentifier les demandes de connexion avec informations d’identification du compte Windows Live™ ID utilisateur.
- **Balise de tunnel**. Cet attribut désigne le numéro d’ID de VLAN à laquelle la connexion doit être affectée par le serveur NAS lorsque vous déployez des réseaux locaux virtuels (VLAN).

## <a name="default-connection-request-policy"></a>Stratégie de demande de connexion par défaut

Une stratégie de demande de connexion par défaut est créée lorsque vous installez NPS. Cette stratégie a la configuration suivante.

- L’authentification n’est pas configurée.
- Comptabilité n’est pas configurée pour transférer les informations de comptabilité à un groupe de serveurs RADIUS distants.
- Attribut n’est pas configuré avec des règles de manipulation d’attribut qui transfère les demandes de connexion à des groupes de serveurs RADIUS distants.
- Demande de transfert est configuré afin que les demandes de connexion sont authentifiées et autorisées sur le serveur NPS local.
- Les attributs avancés ne sont pas configurés.

La stratégie de demande de connexion par défaut utilise NPS comme serveur RADIUS. Pour configurer un serveur qui exécute NPS pour agir en tant que proxy RADIUS, vous devez également configurer un groupe de serveurs RADIUS distants. Vous pouvez créer un nouveau groupe de serveurs RADIUS distant pendant la création d’une nouvelle stratégie de demande de connexion à l’aide de l’Assistant Nouvelle stratégie de demande de connexion. Vous pouvez supprimer la stratégie de demande de connexion par défaut ou vérifiez que la stratégie de demande de connexion par défaut est la dernière stratégie traitée par le serveur NPS en plaçant dernière dans la liste ordonnée des stratégies.

>[!NOTE]
>Si NPS et le service d’accès à distance sont installés sur le même ordinateur et l’accès à distance le service est configuré pour l’authentification Windows et de comptabilité, il est possible pour l’authentification de l’accès à distance et des demandes de compte peut être envoyé à un serveur RADIUS . Cela peut se produire lorsque l’authentification d’accès à distance et les demandes de gestion correspond à une stratégie de demande de connexion qui est configurée pour les transférer vers un groupe de serveurs RADIUS distants.
