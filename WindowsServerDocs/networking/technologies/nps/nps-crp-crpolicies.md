---
title: Stratégies de demande de connexion
description: Cette rubrique fournit une vue d’ensemble des stratégies de demande de connexion de serveur de stratégie réseau dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 4ec45e0c-6b37-4dfb-8158-5f40677b0157
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 496ba87597b0be6cad1109269d2f72f52de017f1
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="connection-request-policies"></a>Stratégies de demande de connexion

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour savoir comment utiliser des stratégies de demande de connexion de serveur NPS pour configurer le serveur NPS en tant que serveur RADIUS, proxy RADIUS ou les deux.

>[!NOTE]
>Outre cette rubrique, la documentation de stratégie de demande connexion suivante est disponible.
> - [Configurer des stratégies de demande de connexion](nps-crp-configure.md)
> - [Configurer des groupes de serveurs RADIUS distants](nps-crp-rrsg-configure.md)

Stratégies de demande de connexion sont des ensembles de conditions et les paramètres qui permettent aux administrateurs réseau de désigner les serveurs Authentication Dial-In utilisateur RADIUS (Remote Service) qui effectuent l’authentification et l’autorisation des demandes de connexion que le serveur exécutant le serveur NPS (Network Policy) reçoit des clients RADIUS. Stratégies de demande de connexion peuvent être configurés pour désigner les serveurs RADIUS sont utilisés pour la gestion de comptes RADIUS.

Vous pouvez créer connexion de stratégies de demande de sorte que certains messages de demande RADIUS envoyés à partir de clients RADIUS sont traitées localement (NPS est utilisé comme serveur RADIUS) et d’autres types de messages sont transférés vers un autre serveur RADIUS (NPS est utilisé en tant que proxy RADIUS).

Avec les stratégies de demande de connexion, vous pouvez utiliser NPS comme serveur RADIUS ou en tant que proxy RADIUS, en fonction de facteurs tels que les éléments suivants: 

- L’heure de la journée et jour de la semaine
- Le nom de domaine dans la demande de connexion
- Le type de connexion demandé
- L’adresse IP du client RADIUS

Messages de demande d’accès RADIUS sont traitées ou transmises par le serveur NPS uniquement si les paramètres du message entrant correspondent au moins une des stratégies de demande de connexion configurées sur le serveur NPS. 

Si les paramètres de stratégie correspond à et la stratégie requiert que le serveur NPS de traiter le message, NPS se comporte comme un serveur RADIUS, authentification et autorisation de la demande de connexion. Si les paramètres de stratégie correspond à et la stratégie requiert que le serveur NPS transfère le message, NPS agit comme un proxy RADIUS et transfère la demande de connexion à un serveur RADIUS à distance pour le traitement.

Si les paramètres d’un message de demande d’accès RADIUS entrant ne correspondent pas au moins une des stratégies de demande de connexion, un message de refus d’accès est envoyé au client RADIUS et l’utilisateur ou l’ordinateur tente de se connecter au réseau accès est refusé.

## <a name="configuration-examples"></a>Exemples de configuration

Les exemples de configuration suivants montrent comment vous pouvez utiliser des stratégies de demande de connexion.

### <a name="nps-as-a-radius-server"></a>NPS comme serveur RADIUS

La stratégie de demande de connexion par défaut est la seule stratégie configurée. Dans cet exemple, NPS est configuré comme serveur RADIUS et toutes les demandes de connexion sont traitées par le serveur NPS local. Le serveur NPS peut authentifier et autoriser les utilisateurs dont les comptes sont dans le domaine du domaine du serveur NPS et dans les domaines approuvés.


### <a name="nps-as-a-radius-proxy"></a>NPS en tant que proxy RADIUS

La stratégie de demande de connexion par défaut est supprimée, et deux nouvelles stratégies de demande de connexion sont créés pour transférer les demandes vers deux domaines différents. Dans cet exemple, le serveur NPS est configuré en tant que proxy RADIUS. NPS ne traite pas les demandes de connexion sur le serveur local. Au lieu de cela, il transfère les demandes de connexion au serveur NPS ou d’autres serveurs RADIUS qui sont configurés en tant que membres de groupes de serveurs RADIUS distants.


### <a name="nps-as-both-radius-server-and-radius-proxy"></a>NPS en tant que serveur RADIUS et proxy RADIUS

En plus de la stratégie de demande de connexion par défaut, une nouvelle stratégie de demande de connexion est créée qui transfère les demandes de connexion à un serveur NPS ou tout autre serveur RADIUS dans un domaine non approuvé. Dans cet exemple, la stratégie de proxy apparaît en premier dans la liste de stratégies. Si la demande de connexion correspond à la stratégie de proxy, la demande de connexion est transférée vers le serveur RADIUS dans le groupe de serveurs RADIUS distants. Si la demande de connexion ne correspond pas à la stratégie de proxy, mais ne correspond pas à la stratégie de demande de connexion par défaut, NPS traite la demande de connexion sur le serveur local. Si la demande de connexion ne correspond pas à une stratégie, il est supprimé.

### <a name="nps-as-radius-server-with-remote-accounting-servers"></a>NPS en tant que serveur RADIUS avec les serveurs de gestion à distance

Dans cet exemple, le serveur NPS local n’est pas configuré pour effectuer la gestion des comptes et la stratégie de demande de connexion par défaut est mis à jour afin que les messages de gestion de comptes RADIUS sont transférés à un serveur NPS ou tout autre serveur RADIUS dans un groupe de serveurs RADIUS distants. Bien que les messages de la gestion des comptes sont transférés, les messages d’authentification et d’autorisation ne sont pas transférés et le serveur NPS local effectue ces fonctions pour le domaine local et tous les domaines approuvés.


### <a name="nps-with-remote-radius-to-windows-user-mapping"></a>NPS avec RADIUS distant vers le mappage utilisateur Windows

Dans cet exemple, NPS, se comporte comme un serveur RADIUS et en tant que proxy RADIUS pour chaque demande de connexion en transférant la demande d’authentification à un serveur RADIUS à distance lors de l’utilisation d’un compte d’utilisateur Windows local pour l’autorisation. Cette configuration est implémentée en configurant le rayon à distance à l’attribut de mappage de l’utilisateur Windows en tant que condition de la stratégie de demande de connexion. (En outre, un compte d’utilisateur doit être créé localement qui a le même nom que le compte d’utilisateur distant sur lequel l’authentification est effectuée par le serveur RADIUS à distance.)

## <a name="connection-request-policy-conditions"></a>Conditions de stratégie de demande de connexion

Conditions de stratégie de demande de connexion sont un ou plusieurs attributs RADIUS qui sont comparés aux attributs du message de demande d’accès RADIUS entrant. S’il existe plusieurs conditions, toutes les conditions dans la connexion de message de demande, puis dans la demande de connexion stratégie doit correspondre dans l’ordre de la stratégie appliquée par le serveur NPS.

Voici les attributs disponibles que vous pouvez configurer dans les stratégies de demande de connexion.

### <a name="connection-properties-attribute-group"></a>Groupe d’attributs de propriétés de connexion

Le groupe d’attributs de propriétés de connexion contient les attributs suivants.

- **Protocole de trames**. Permet de spécifier le type de trame pour les paquets entrants. Protocole PPP (point), série ligne SLIP (Internet Protocol), relais de trame et X.25 sont des exemples.
- **Type de service**. Utilisé pour désigner le type de service demandé. Exemples encadré (par exemple, les connexions PPP) et de la connexion (par exemple, les connexions Telnet). Pour plus d’informations sur les types de services RADIUS, consultez la RFC 2865, «Remote Authentication Dial-in User Service (RADIUS)».
- **Type de tunnel**. Utilisé pour désigner le type de tunnel est créé par le client demandeur. Types de tunnel incluent le protocole (PPTP) et Layer Two Tunneling Protocol (L2TP).

### <a name="day-and-time-restrictions-attribute-group"></a>Groupe d’attributs jour et les Restrictions de temps 

Le groupe d’attributs jour et les Restrictions temporelles contient l’attribut jour et les Restrictions de temps. Avec cet attribut, vous pouvez désigner le jour de la semaine et l’heure du jour de la tentative de connexion. Le jour et l’heure est relative à la journée et l’heure du serveur NPS.

### <a name="gateway-attribute-group"></a>Groupe d’attributs de passerelle

Le groupe d’attributs passerelle contient les attributs suivants.

- **Appelée ID de la Station**. Utilisé pour désigner le numéro de téléphone du serveur d’accès réseau. Cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe des critères spéciaux pour spécifier des codes de zone.
- **Identificateur NAS**. Utilisé pour désigner le nom du serveur d’accès réseau. Cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe des critères spéciaux pour spécifier les identificateurs NAS.
- **Adresse IPv4 NAS**. Permet de désigner Internet Protocol version 4 (IPv4) adresse du serveur d’accès réseau (le client RADIUS). Cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe des critères spéciaux pour spécifier des réseaux IP.
- **Adresse IPv6 NAS**. Permet de désigner Internet Protocol version 6 (IPv6) adresse du serveur d’accès réseau (le client RADIUS). Cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe des critères spéciaux pour spécifier des réseaux IP.
- **Type de Port NAS**. Utilisé pour désigner le type de média utilisé par le client d’accès. Les lignes téléphoniques analogiques (appelé async), Services réseau RNIS (numérique), les tunnels ou réseaux privés virtuels (VPN), IEEE 802.11 sans fil, sont des exemples et les commutateurs Ethernet.

### <a name="machine-identity-attribute-group"></a>Groupe d’attributs identité de l’ordinateur

Le groupe d’attributs identité de l’ordinateur contient l’attribut de l’identité de l’ordinateur. À l’aide de cet attribut, vous pouvez spécifier la méthode avec laquelle les clients sont identifiés dans la stratégie.

### <a name="radius-client-properties-attribute-group"></a>Groupe d’attributs propriétés du Client RADIUS

Le groupe d’attributs de propriétés du Client RADIUS contient les attributs suivants.

- **ID de la Station appelante**. Utilisé pour désigner le numéro de téléphone utilisé par l’appelant (le client d’accès). Cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe des critères spéciaux pour spécifier des codes de zone.
- **Nom convivial du client**. Utilisé pour désigner le nom de l’ordinateur client RADIUS qui demande l’authentification. Cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe des critères spéciaux pour spécifier les noms de client.
- **Adresse IPv4 du client**. Utilisé pour désigner l’adresse IPv4 du serveur d’accès réseau (le client RADIUS). Cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe des critères spéciaux pour spécifier des réseaux IP.
- **Adresse IPv6 du client**. Utilisé pour désigner l’adresse IPv6 du serveur d’accès réseau (le client RADIUS). Cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe des critères spéciaux pour spécifier des réseaux IP.
- **Fournisseur du client**. Utilisé pour désigner le fournisseur du serveur d’accès réseau qui demande l’authentification. Un ordinateur exécutant le service Routage et accès distant est le fabricant NAS Microsoft. Vous pouvez utiliser cet attribut pour configurer des stratégies distinctes pour les différents fabricants NAS. Cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe des critères spéciaux.

### <a name="user-name-attribute-group"></a>Groupe d’attribut nom de l’utilisateur

Le groupe d’attributs de nom d’utilisateur contient l’attribut de nom d’utilisateur. À l’aide de cet attribut, vous pouvez désigner le nom d’utilisateur, ou une partie du nom d’utilisateur, qui doit correspondre au nom d’utilisateur fourni par le client d’accès dans le message RADIUS. Cet attribut est une chaîne de caractères qui contient généralement un nom de domaine et un nom de compte d’utilisateur. Vous pouvez utiliser la syntaxe des critères spéciaux pour spécifier les noms d’utilisateur.

## <a name="connection-request-policy-settings"></a>Paramètres de stratégie de demande de connexion

Paramètres de stratégie de demande de connexion sont un ensemble de propriétés qui sont appliqués à un message RADIUS entrant. Paramètres correspondent à des groupes suivants des propriétés.

- Authentification
- Gestion des comptes
- Manipulation d’attribut
- Transfert de la demande
- Avancé

Les sections suivantes fournissent des informations supplémentaires sur ces paramètres.

### <a name="authentication"></a>Authentification

À l’aide de ce paramètre, vous pouvez remplacer les paramètres d’authentification qui sont configurés dans toutes les stratégies de réseau et vous pouvez désigner les méthodes d’authentification et les types qui sont requises pour se connecter à votre réseau.

>[!IMPORTANT]
>Si vous configurez une méthode d’authentification dans la stratégie de demande de connexion qui est moins sécurisée que la méthode d’authentification que vous configurez dans la stratégie de réseau, la méthode d’authentification plus sécurisée que vous configurez dans la stratégie de réseau est remplacée. Par exemple, si vous disposez d’une stratégie de réseau qui nécessite l’utilisation de protégé Extensible Authentication Protocol-Microsoft Challenge Handshake Authentication Protocol version 2 \ (PEAP-MS-CHAP v2\), qui est une méthode d’authentification par mot de passe pour sans fil sécurisé, et vous également configurer une stratégie de demande de connexion pour autoriser l’accès non authentifié, le résultat est qu’aucun client n’est nécessaires pour s’authentifier à l’aide de PEAP-MS-CHAP v2. Dans cet exemple, tous les clients qui se connectent à votre réseau sont accordés l’accès non authentifié.

### <a name="accounting"></a>Gestion des comptes

À l’aide de ce paramètre, vous pouvez configurer la stratégie de demande de connexion pour transférer des informations de gestion à un serveur NPS ou tout autre serveur RADIUS dans un groupe de serveurs RADIUS distants afin que le groupe de serveurs RADIUS distants effectue la gestion des comptes.

>[!NOTE]
>Si vous avez plusieurs serveurs RADIUS et que vous souhaitez que les informations de gestion pour tous les serveurs sont stockées dans une base de gestion de comptes RADIUS centrale, vous pouvez utiliser le paramètre gestion des comptes de stratégie de demande de connexion dans une stratégie sur chaque serveur RADIUS pour transférer des données de gestion des comptes à partir de tous les serveurs à un serveur NPS ou tout autre serveur RADIUS qui est désignée comme un serveur de gestion des comptes.

Paramètres gestion des comptes de la stratégie de demande connexion fonctionnent indépendamment de la configuration de gestion du serveur NPS local. En d’autres termes, si vous configurez le serveur NPS local pour enregistrer les informations de gestion de comptes RADIUS dans un fichier local ou à une base de données Microsoft SQL Server, il fera même si vous configurez une stratégie de demande de connexion pour transférer des messages de la gestion des comptes à un groupe de serveurs RADIUS distants.

Si vous souhaitez que les informations de gestion ouvert une session à distance, mais pas localement, vous devez configurer le serveur NPS local pour ne pas effectuer la gestion des comptes, mais aussi configurer la gestion des comptes dans une stratégie de demande de connexion pour transférer des données de gestion des comptes à un groupe de serveurs RADIUS distants.

### <a name="attribute-manipulation"></a>Manipulation d’attribut

Vous pouvez configurer un ensemble de règles de recherche et remplacement qui manipulent les chaînes de texte de l’un des attributs suivants.

- Nom d’utilisateur
- ID de la Station appelée
- ID de la Station appelante

Traitement des règles de recherche et remplacement se produit pour un des attributs précédents avant que le message RADIUS est soumis à des paramètres d’authentification et de gestion des comptes. Règles de manipulation d’attribut s’appliquent uniquement à un attribut unique. Vous ne pouvez pas configurer les règles de manipulation d’attribut pour chaque attribut. En outre, la liste des attributs que vous pouvez manipuler est une liste statique; Vous ne pouvez pas ajouter à la liste des attributs disponibles pour la manipulation.

>[!NOTE]
>Si vous utilisez le protocole d’authentification MS-CHAP v2, vous ne pouvez pas manipuler l’attribut de nom d’utilisateur si la stratégie de demande de connexion est utilisée pour transférer le message RADIUS. La seule exception se produit lorsqu’une barre oblique inverse (\) est utilisée et la manipulation affecte uniquement les informations à gauche de celui-ci. Une barre oblique inverse est généralement utilisée pour indiquer un nom de domaine (les informations à gauche de la barre oblique inverse) et un nom de compte d’utilisateur au sein du domaine (les informations à droite de la barre oblique inverse). Dans ce cas, uniquement les règles de manipulation d’attribut modifient ou de remplacer le nom de domaine sont autorisées.

Des exemples illustrant comment manipuler le nom de domaine dans l’attribut de nom d’utilisateur, voir la section «Exemples de manipulation du nom de domaine dans l’attribut de nom d’utilisateur» dans la rubrique [utiliser des Expressions régulières dans NPS](nps-crp-reg-expressions.md).

### <a name="forwarding-request"></a>Transfert de la demande

Vous pouvez définir les éléments suivants des options de requête qui sont utilisées pour les messages de demande d’accès RADIUS de transfert:

- **Authentifier les demandes sur ce serveur**. À l’aide de ce paramètre, NPS utilise un domaine Windows NT 4.0, Active Directory ou la base de données de local gestionnaire de comptes de sécurité (SAM) utilisateur comptes pour authentifier la demande de connexion. Ce paramètre indique également que la stratégie réseau correspondante configurée dans NPS, ainsi que les propriétés de numérotation du compte d’utilisateur, sont utilisées par NPS pour autoriser la demande de connexion. Dans ce cas, le serveur NPS est configuré pour exécuter en tant que serveur RADIUS.

- **Transférer les demandes au groupe de serveurs RADIUS distant suivant**. À l’aide de ce paramètre, NPS transfère les demandes de connexion pour le groupe de serveurs RADIUS distants que vous spécifiez. Si le serveur NPS reçoit un message d’acceptation d’accès valide qui correspond au message de demande d’accès, la tentative de connexion est considéré comme authentifiés et autorisés. Dans ce cas, le serveur NPS agit comme un proxy RADIUS.

- **Accepter les utilisateurs sans valider les informations d’identification**. À l’aide de ce paramètre, NPS ne vérifie pas l’identité de l’utilisateur tente de se connecter au réseau et serveur NPS n’essaie pas de vérifier que l’utilisateur ou l’ordinateur a le droit de se connecter au réseau. Lorsque le serveur NPS est configuré pour autoriser les accès non authentifié et qu’il reçoit une demande de connexion, NPS envoie immédiatement qu'un message d’acceptation d’accès au client et que l’utilisateur ou ordinateur rayon est accordé l’accès au réseau. Ce paramètre est utilisé pour certains types de tunneling obligatoire où le client d’accès tunnel avant que les informations d’identification de l’utilisateur sont authentifiées.

>[!NOTE]
>Cette option d’authentification ne peut pas être utilisée lorsque le protocole d’authentification du client d’accès est MS-CHAP v2 ou Extensible Authentication Protocol-Transport Layer Security (EAP-TLS), qui fournissent une authentification mutuelle. Dans l’authentification mutuelle, le client d’accès prouve qu’il s’agit d’un client d’accès valide pour le serveur d’authentification (le serveur NPS) et le serveur d’authentification prouve qu’il est un serveur d’authentification valide pour le client d’accès. Lorsque cette option d’authentification est utilisée, le message d’acceptation d’accès est renvoyé. Toutefois, le serveur d’authentification ne fournit pas de validation au client d’accès, et l’authentification mutuelle échoue.

Pour obtenir des exemples d’utiliser des expressions régulières pour créer des règles de routage de transférer des messages RADIUS avec un nom de domaine Kerberos spécifié à un groupe de serveurs RADIUS distants, voir la section «Exemple de transfert des messages par un serveur proxy RADIUS» dans la rubrique [utiliser des Expressions régulières dans NPS](nps-crp-reg-expressions.md).

### <a name="advanced"></a>Avancé

Vous pouvez définir des propriétés avancées pour spécifier la série d’attributs RADIUS qui sont:

- Ajouté au message de réponse RADIUS lorsque le serveur NPS est utilisé comme un serveur de gestion des comptes ou de l’authentification RADIUS. Lorsqu’il existe des attributs spécifiés sur une stratégie de réseau et la stratégie de demande de connexion, les attributs qui sont envoyées dans le message de réponse RADIUS sont la combinaison des deux jeux d’attributs.
- Ajouté au message RADIUS lorsque le serveur NPS est utilisé comme un proxy de gestion des comptes ou d’authentification RADIUS. Si l’attribut existe déjà dans le message qui est transféré, il est remplacé par la valeur de l’attribut spécifié dans la stratégie de demande de connexion. 

En outre, certains attributs qui sont disponibles pour la configuration de la connexion de stratégie de demande **paramètres** onglet dans le **avancé** catégorie fournissent des fonctionnalités spécialisées. Par exemple, vous pouvez configurer le **RADIUS distant vers le mappage de l’utilisateur Windows** lorsque vous souhaitez diviser l’authentification et l’autorisation d’une demande de connexion entre deux utilisateur comptes des bases de données d’attribut.

Le **RADIUS distant vers le mappage de l’utilisateur Windows** attribut spécifie que l’autorisation Windows se produit pour les utilisateurs sont authentifiés par un serveur RADIUS à distance. En d’autres termes, un serveur RADIUS effectue une authentification par rapport à un compte d’utilisateur dans une base de données de comptes utilisateur à distance, mais le serveur NPS local autorise la demande de connexion par rapport à un compte d’utilisateur dans une base de données de comptes utilisateur local. Cela est utile lorsque vous souhaitez autoriser l’accès à votre réseau les visiteurs.

Par exemple, les visiteurs d’organisations partenaires peuvent être authentifiés par leur propre serveur RADIUS organisation partenaire et ensuite utilisent un compte d’utilisateur Windows de votre organisation pour accéder à un invité réseau local (LAN) sur votre réseau.

D’autres attributs qui fournissent des fonctionnalités spécialisées sont:

- **MS-mise en quarantaine-IPFilter et MS-mise en quarantaine-Session-Timeout**. Ces attributs sont utilisés lorsque vous déployez un réseau accès quarantaine contrôle d ' avec votre déploiement de routage et accès distant VPN.
- **Suffixe du UPN mappage Passport utilisateur**. Cet attribut vous permet d’authentifier les demandes de connexion avec les informations d’identification du compte Windows Live™ ID utilisateur.
- **Balise de tunnel**. Cet attribut désigne le numéro d’ID de réseau local virtuel auquel la connexion doit être affectée par le NAS lorsque vous déployez des réseaux locaux virtuels (VLAN).

## <a name="default-connection-request-policy"></a>Stratégie de demande de connexion par défaut

Une stratégie de demande de connexion par défaut est créée lorsque vous installez le serveur NPS. Cette stratégie possède la configuration suivante.

- L’authentification n’est pas configurée.
- Gestion des comptes ne sont pas configuré pour transférer les informations de gestion des comptes à un groupe de serveurs RADIUS distants.
- Attribut n’est pas configuré avec les règles de manipulation d’attribut qui transfèrent des demandes de connexion à des groupes de serveurs RADIUS distants.
- Demande de transfert est configuré afin que les demandes de connexion sont authentifiés et autorisés sur le serveur NPS local.
- Les attributs avancés ne sont pas configurés.

La stratégie de demande de connexion par défaut utilise NPS comme serveur RADIUS. Pour configurer un serveur qui exécute NPS pour agir en tant que proxy RADIUS, vous devez également configurer un groupe de serveurs RADIUS distants. Vous pouvez créer un nouveau groupe de serveurs RADIUS distant pendant la création d’une nouvelle stratégie de demande de connexion à l’aide de l’Assistant Nouvelle stratégie de demande de connexion. Vous pouvez supprimer la stratégie de demande de connexion par défaut ou vérifiez que la stratégie de demande de connexion par défaut est la dernière stratégie traitée par le serveur NPS en le plaçant dernière dans la liste de stratégies.

>[!NOTE]
>Si NPS et le service d’accès à distance sont installés sur le même ordinateur et l’accès à distance le service est configuré pour l’authentification Windows et gestion des comptes, il est possible pour l’authentification d’accès à distance et les demandes de gestion des comptes à transmettre à un serveur RADIUS. Cela peut se produire lorsque l’authentification d’accès à distance et les demandes de gestion correspondent à une stratégie de demande de connexion qui est configurée pour les transférer vers un groupe de serveurs RADIUS distants.