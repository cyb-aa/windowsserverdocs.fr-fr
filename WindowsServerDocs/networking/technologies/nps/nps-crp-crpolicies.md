---
title: Stratégies de requête de connexion
description: Cette rubrique fournit une vue d’ensemble des stratégies de demande de connexion au serveur de stratégie réseau dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 4ec45e0c-6b37-4dfb-8158-5f40677b0157
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 021cef4a220b183f6580bca75bc6db68aba893cf
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316235"
---
# <a name="connection-request-policies"></a>Stratégies de requête de connexion

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour apprendre à utiliser des stratégies de demande de connexion NPS pour configurer le serveur NPS en tant que serveur RADIUS, proxy RADIUS, ou les deux.

>[!NOTE]
>En plus de cette rubrique, la documentation suivante sur la stratégie de demande de connexion est disponible.
> - [Configurer des stratégies de demande de connexion](nps-crp-configure.md)
> - [Configurer des groupes de serveurs RADIUS distants](nps-crp-rrsg-configure.md)

Les stratégies de demande de connexion sont des ensembles de conditions et de paramètres qui permettent aux administrateurs réseau de désigner les serveurs protocole RADIUS (Remote Authentication Dial-In User Service) (RADIUS) qui effectuent l’authentification et l’autorisation des demandes de connexion que le le serveur exécutant le serveur NPS (Network Policy Server) reçoit des clients RADIUS. Les stratégies de demande de connexion peuvent être configurées pour désigner les serveurs RADIUS utilisés pour la gestion de comptes RADIUS.

Vous pouvez créer des stratégies de demande de connexion afin que certains messages de demande RADIUS envoyés à partir des clients RADIUS soient traités localement (NPS est utilisé en tant que serveur RADIUS) et que d’autres types de messages soient transférés vers un autre serveur RADIUS (NPS est utilisé en tant que proxy RADIUS).

Avec les stratégies de demande de connexion, vous pouvez utiliser NPS en tant que serveur RADIUS ou proxy RADIUS, en fonction de facteurs tels que les suivants : 

- Heure du jour et du jour de la semaine
- Nom de domaine dans la demande de connexion
- Type de connexion demandé
- L’adresse IP du client RADIUS

Accès RADIUS : les messages de demande sont traités ou transférés par NPS uniquement si les paramètres du message entrant correspondent au moins à l’une des stratégies de demande de connexion configurées sur le serveur NPS. 

Si les paramètres de stratégie correspondent et que la stratégie exige que le serveur NPS traite le message, il agit comme un serveur RADIUS, en authentifiant et en autorisant la demande de connexion. Si les paramètres de stratégie correspondent et que la stratégie requiert que le serveur NPS transfère le message, le serveur NPS agit comme un proxy RADIUS et transfère la demande de connexion à un serveur RADIUS distant pour traitement.

Si les paramètres d’un message de demande d’accès RADIUS entrant ne correspondent pas au moins à l’une des stratégies de demande de connexion, un message de refus d’accès est envoyé au client RADIUS et l’utilisateur ou l’ordinateur qui tente de se connecter au réseau se voit refuser l’accès.

## <a name="configuration-examples"></a>Exemples de configuration

Les exemples de configuration suivants montrent comment vous pouvez utiliser des stratégies de demande de connexion.

### <a name="nps-as-a-radius-server"></a>NPS comme serveur RADIUS

La stratégie de demande de connexion par défaut est la seule stratégie configurée. Dans cet exemple, le serveur NPS est configuré en tant que serveur RADIUS et toutes les demandes de connexion sont traitées par le serveur NPS local. Le serveur NPS peut authentifier et autoriser les utilisateurs dont les comptes se trouvent dans le domaine du domaine NPS et dans des domaines approuvés.


### <a name="nps-as-a-radius-proxy"></a>NPS en tant que proxy RADIUS

La stratégie de demande de connexion par défaut est supprimée et deux nouvelles stratégies de demande de connexion sont créées pour transférer les demandes vers deux domaines différents. Dans cet exemple, le serveur NPS est configuré en tant que proxy RADIUS. NPS ne traite pas les demandes de connexion sur le serveur local. Au lieu de cela, il transfère les demandes de connexion à des serveurs NPS ou autres qui sont configurés en tant que membres de groupes de serveurs RADIUS distants.


### <a name="nps-as-both-radius-server-and-radius-proxy"></a>NPS en tant que serveur RADIUS et proxy RADIUS

En plus de la stratégie de demande de connexion par défaut, une nouvelle stratégie de demande de connexion est créée, qui transfère les demandes de connexion à un serveur NPS ou à un autre serveur RADIUS dans un domaine non approuvé. Dans cet exemple, la stratégie de proxy apparaît en premier dans la liste ordonnée des stratégies. Si la demande de connexion correspond à la stratégie de proxy, la demande de connexion est transférée au serveur RADIUS dans le groupe de serveurs RADIUS distants. Si la demande de connexion ne correspond pas à la stratégie de proxy, mais qu’elle correspond à la stratégie de demande de connexion par défaut, NPS traite la demande de connexion sur le serveur local. Si la demande de connexion ne correspond à aucune stratégie, elle est ignorée.

### <a name="nps-as-radius-server-with-remote-accounting-servers"></a>NPS en tant que serveur RADIUS avec des serveurs de gestion distants

Dans cet exemple, le serveur NPS local n’est pas configuré pour effectuer la gestion des comptes et la stratégie de demande de connexion par défaut est révisée afin que les messages de gestion des comptes RADIUS soient transférés vers un serveur NPS ou un autre serveur RADIUS dans un groupe de serveurs RADIUS distants. Bien que les messages de gestion des comptes soient transférés, les messages d’authentification et d’autorisation ne sont pas transférés, et le serveur NPS local effectue ces fonctions pour le domaine local et tous les domaines approuvés.


### <a name="nps-with-remote-radius-to-windows-user-mapping"></a>Mappage d’utilisateur NPS avec RADIUS distant vers Windows

Dans cet exemple, NPS agit à la fois comme serveur RADIUS et proxy RADIUS pour chaque demande de connexion en transférant la demande d’authentification à un serveur RADIUS distant lors de l’utilisation d’un compte d’utilisateur Windows local pour l’autorisation. Cette configuration est implémentée en configurant l’attribut de mappage de l’utilisateur RADIUS distant sur Windows en tant que condition de la stratégie de demande de connexion. (En outre, un compte d’utilisateur doit être créé localement et portant le même nom que le compte d’utilisateur distant sur lequel l’authentification est effectuée par le serveur RADIUS distant.)

## <a name="connection-request-policy-conditions"></a>Conditions de la stratégie de demande de connexion

Les conditions de la stratégie de demande de connexion sont un ou plusieurs attributs RADIUS comparés aux attributs du message entrant de demande d’accès RADIUS. S’il existe plusieurs conditions, toutes les conditions du message de demande de connexion et de la stratégie de demande de connexion doivent correspondre pour que la stratégie soit appliquée par NPS.


Voici les attributs de condition disponibles que vous pouvez configurer dans les stratégies de demande de connexion.

### <a name="connection-properties-attribute-group"></a>Groupe d’attributs de propriétés de connexion

Le groupe d’attributs propriétés de connexion contient les attributs suivants.

- **Protocole tramé**. Utilisé pour désigner le type de tramage pour les paquets entrants. Les exemples sont protocole PPP (PPP), SLIP (Serial Line Internet Protocol), relais de trames et X. 25.
- **Type de service**. Utilisé pour désigner le type de service demandé. Par exemple, les connexions PPP et la connexion (par exemple, connexions Telnet) sont des exemples. Pour plus d’informations sur les types de service RADIUS, consultez RFC 2865, « Remote Authentication Dial-in User Service (RADIUS) ».
- **Type de tunnel**. Utilisé pour désigner le type de tunnel créé par le client demandeur. Les types de tunnels incluent le protocole PPTP (point-to-Point Tunneling Protocol) et le protocole L2TP (Layer Two Tunneling Protocol).

### <a name="day-and-time-restrictions-attribute-group"></a>Groupe d’attributs des restrictions de jour et d’heure 

Le groupe d’attributs des restrictions de jour et d’heure contient l’attribut des restrictions de jour et d’heure. Avec cet attribut, vous pouvez désigner le jour de la semaine et l’heure du jour de la tentative de connexion. Le jour et l’heure sont relatifs au jour et à l’heure du serveur NPS.

### <a name="gateway-attribute-group"></a>Groupe d’attributs de passerelle

Le groupe d’attributs de passerelle contient les attributs suivants.

- **ID de la station appelée**. Utilisé pour désigner le numéro de téléphone du serveur d’accès réseau. Cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe de mise en correspondance de modèle pour spécifier des indicatifs de zone.
- **Identificateur NAS**. Utilisé pour désigner le nom du serveur d’accès réseau. Cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe de mise en correspondance de modèle pour spécifier des identificateurs NAS.
- **Adresse IPv4 NAS**. Utilisé pour désigner l’adresse IPv4 (Internet Protocol version 4) du serveur d’accès au réseau (client RADIUS). Cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe de mise en correspondance de modèle pour spécifier des réseaux IP.
- **Adresse IPv6 NAS**. Utilisé pour désigner l’adresse IPv6 (Internet Protocol version 6) du serveur d’accès au réseau (client RADIUS). Cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe de mise en correspondance de modèle pour spécifier des réseaux IP.
- **Type de port NAS**. Utilisé pour désigner le type de média utilisé par le client d’accès. Les exemples sont les lignes téléphoniques analogiques (asynchrones), RNIS (réseau numérique à intégration de services), les tunnels ou les réseaux privés virtuels (VPN), les réseaux sans fil IEEE 802,11 et les commutateurs Ethernet.

### <a name="machine-identity-attribute-group"></a>Groupe d’attributs d’identité de l’ordinateur

Le groupe d’attributs d’identité de l’ordinateur contient l’attribut d’identité de l’ordinateur. À l’aide de cet attribut, vous pouvez spécifier la méthode avec laquelle les clients sont identifiés dans la stratégie.

### <a name="radius-client-properties-attribute-group"></a>Groupe d’attributs des propriétés du client RADIUS

Le groupe d’attributs propriétés du client RADIUS contient les attributs suivants.

- **ID de la station appelante**. Utilisé pour désigner le numéro de téléphone utilisé par l’appelant (le client d’accès). Cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe de mise en correspondance de modèle pour spécifier des indicatifs de zone.  Dans les authentifications 802.1 x, l’adresse MAC est généralement remplie et peut être mise en correspondance à partir du client.  Ce champ est généralement utilisé pour les scénarios de contournement des adresses Mac lorsque la stratégie de demande de connexion est configurée pour « accepter les utilisateurs sans valider les informations d’identification ».  
- **Nom convivial du client**. Utilisé pour désigner le nom de l’ordinateur client RADIUS qui demande l’authentification. Cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe de critères spéciaux pour spécifier les noms des clients.
- **Adresse IPv4 du client**. Utilisé pour désigner l’adresse IPv4 du serveur d’accès réseau (client RADIUS). Cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe de mise en correspondance de modèle pour spécifier des réseaux IP.
- **Adresse IPv6 du client**. Utilisé pour désigner l’adresse IPv6 du serveur d’accès réseau (client RADIUS). Cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe de mise en correspondance de modèle pour spécifier des réseaux IP.
- **Fournisseur du client**. Utilisé pour désigner le fournisseur du serveur d’accès réseau qui demande l’authentification. Un ordinateur exécutant le service de routage et d’accès à distance est le fabricant du serveur Microsoft NAS. Vous pouvez utiliser cet attribut pour configurer des stratégies distinctes pour différents fabricants d’accès réseau. Cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe de critères spéciaux.

### <a name="user-name-attribute-group"></a>Groupe d’attributs de nom d’utilisateur

Le groupe d’attributs nom d’utilisateur contient l’attribut nom d’utilisateur. À l’aide de cet attribut, vous pouvez désigner le nom d’utilisateur, ou une partie du nom d’utilisateur, qui doit correspondre au nom d’utilisateur fourni par le client d’accès dans le message RADIUS. Cet attribut est une chaîne de caractères qui contient généralement un nom de domaine et un nom de compte d’utilisateur. Vous pouvez utiliser la syntaxe de mise en correspondance de modèle pour spécifier des noms d’utilisateur.

## <a name="connection-request-policy-settings"></a>Paramètres de stratégie de demande de connexion

Les paramètres de stratégie de demande de connexion sont un ensemble de propriétés qui sont appliquées à un message RADIUS entrant. Les paramètres sont constitués des groupes de propriétés suivants.

- Authentification
- Gestion des comptes
- Manipulation d’attributs
- Transfert de la requête
- Avancé

Les sections suivantes fournissent des détails supplémentaires sur ces paramètres.

### <a name="authentication"></a>Authentification

En utilisant ce paramètre, vous pouvez remplacer les paramètres d’authentification qui sont configurés dans toutes les stratégies réseau. vous pouvez ainsi désigner les méthodes et les types d’authentification requis pour se connecter à votre réseau.

>[!IMPORTANT]
>Si vous configurez une méthode d’authentification dans la stratégie de demande de connexion qui est moins sécurisée que la méthode d’authentification que vous configurez dans la stratégie réseau, la méthode d’authentification la plus sécurisée que vous configurez dans la stratégie réseau est remplacée. Par exemple, si vous avez une stratégie réseau qui requiert l’utilisation du protocole EAP (Protected Extensible Authentication Protocol)-Microsoft Challenge Handshake Authentication Protocol version 2 \(PEAP-MS-CHAP v2\), qui est une méthode d’authentification par mot de passe pour une connexion sans fil sécurisée et vous configurez également une stratégie de demande de connexion pour autoriser l’accès non authentifié, le résultat est qu’aucun client ne doit s’authentifier à l’aide de PEAP-MS-CHAP v2. Dans cet exemple, tous les clients qui se connectent à votre réseau bénéficient d’un accès non authentifié.

### <a name="accounting"></a>Gestion des comptes

En utilisant ce paramètre, vous pouvez configurer la stratégie de demande de connexion pour transférer les informations de gestion de comptes à un serveur NPS ou à un autre serveur RADIUS dans un groupe de serveurs RADIUS distants afin que le groupe de serveurs RADIUS distants effectue la gestion des comptes.

>[!NOTE]
>Si vous avez plusieurs serveurs RADIUS et que vous souhaitez des informations de gestion de comptes pour tous les serveurs stockés dans une base de données de gestion de comptes RADIUS centrale, vous pouvez utiliser le paramètre de gestion des stratégies de demande de connexion dans une stratégie sur chaque serveur RADIUS à partir duquel transférer les données de gestion tous les serveurs d’un serveur NPS ou d’un autre serveur RADIUS désigné comme serveur de gestion des comptes.

Les paramètres de gestion des stratégies de demande de connexion fonctionnent indépendamment de la configuration de comptabilité du serveur NPS local. En d’autres termes, si vous configurez le serveur NPS local pour enregistrer les informations de gestion de comptes RADIUS dans un fichier local ou dans une base de données Microsoft SQL Server, il le fera, que vous configurez une stratégie de demande de connexion pour transférer les messages de gestion vers un RADIUS distant ou non. Groupe de serveurs.

Si vous souhaitez enregistrer les informations de comptabilité à distance, mais pas localement, vous devez configurer le serveur NPS local pour qu’il n’effectue pas de gestion des comptes, tout en configurant la gestion des comptes dans une stratégie de demande de connexion pour transférer les données de gestion vers un groupe de serveurs RADIUS distants.

### <a name="attribute-manipulation"></a>Manipulation d’attributs

Vous pouvez configurer un ensemble de règles de recherche et de remplacement qui manipulent les chaînes de texte de l’un des attributs suivants.

- Nom d'utilisateur
- ID de la station appelée
- ID de la station appelante

Le traitement des règles de recherche et de remplacement se produit pour l’un des attributs précédents avant que le message RADIUS ne soit soumis à des paramètres d’authentification et de gestion des comptes. Les règles de manipulation d’attribut s’appliquent uniquement à un seul attribut. Vous ne pouvez pas configurer des règles de manipulation d’attributs pour chaque attribut. En outre, la liste des attributs que vous pouvez manipuler est une liste statique. vous ne pouvez pas ajouter à la liste d’attributs disponibles pour la manipulation.

>[!NOTE]
>Si vous utilisez le protocole d’authentification MS-CHAP v2, vous ne pouvez pas manipuler l’attribut de nom d’utilisateur si la stratégie de demande de connexion est utilisée pour transférer le message RADIUS. La seule exception se produit lorsqu’une barre oblique inverse (caractère\) est utilisée et que la manipulation affecte uniquement les informations à gauche de celle-ci. Une barre oblique inverse est généralement utilisée pour indiquer un nom de domaine (les informations à gauche de la barre oblique inverse) et un nom de compte d’utilisateur dans le domaine (les informations à droite de la barre oblique inverse). Dans ce cas, seules les règles de manipulation d’attribut qui modifient ou remplacent le nom de domaine sont autorisées.

Pour obtenir des exemples de manipulation du nom de domaine dans l’attribut de nom d’utilisateur, consultez la section « Exemples de manipulation du nom de domaine dans l’attribut de nom d’utilisateur » dans la rubrique [utiliser des expressions régulières dans NPS](nps-crp-reg-expressions.md).

### <a name="forwarding-request"></a>Transfert de la requête

Vous pouvez définir les options de demande de transfert suivantes qui sont utilisées pour les messages de demande d’accès RADIUS :

- **Authentifier les demandes sur ce serveur**. En utilisant ce paramètre, NPS utilise un domaine Windows NT 4,0, Active Directory ou la base de données de comptes d’utilisateurs SAM (Security Accounts Manager) locale pour authentifier la demande de connexion. Ce paramètre spécifie également que la stratégie réseau correspondante configurée dans NPS, ainsi que les propriétés de numérotation du compte d’utilisateur, sont utilisées par NPS pour autoriser la demande de connexion. Dans ce cas, le serveur NPS est configuré pour s’exécuter en tant que serveur RADIUS.

- **Transférer les demandes au groupe de serveurs RADIUS distants suivant**. En utilisant ce paramètre, NPS transfère les demandes de connexion au groupe de serveurs RADIUS distants que vous spécifiez. Si le serveur NPS reçoit un message d’acceptation d’accès valide qui correspond au message de demande d’accès, la tentative de connexion est considérée comme authentifiée et autorisée. Dans ce cas, le serveur NPS agit comme un proxy RADIUS.

- **Accepter les utilisateurs sans valider les informations d’identification**. En utilisant ce paramètre, NPS ne vérifie pas l’identité de l’utilisateur qui tente de se connecter au réseau et le serveur NPS ne tente pas de vérifier que l’utilisateur ou l’ordinateur a le droit de se connecter au réseau. Lorsque NPS est configuré pour autoriser l’accès non authentifié et qu’il reçoit une demande de connexion, NPS envoie immédiatement un message d’acceptation d’accès au client RADIUS et l’utilisateur ou l’ordinateur se voit accorder un accès réseau. Ce paramètre est utilisé pour certains types de tunneling obligatoire où le client d’accès est tunnelé avant que les informations d’identification de l’utilisateur soient authentifiées.

>[!NOTE]
>Cette option d’authentification ne peut pas être utilisée lorsque le protocole d’authentification du client d’accès est MS-CHAP v2 ou EAP-TLS (Extensible Authentication Protocol-Transport Layer Security), tous deux fournissant une authentification mutuelle. Dans l’authentification mutuelle, le client d’accès prouve qu’il s’agit d’un client d’accès valide au serveur d’authentification (le serveur NPS), et le serveur d’authentification prouve qu’il s’agit d’un serveur d’authentification valide pour le client d’accès. Lorsque cette option d’authentification est utilisée, le message d’acceptation d’accès est retourné. Toutefois, le serveur d’authentification ne fournit pas de validation au client d’accès et l’authentification mutuelle échoue.

Pour obtenir des exemples d’utilisation d’expressions régulières pour créer des règles de routage qui transfèrent les messages RADIUS avec un nom de domaine spécifié vers un groupe de serveurs RADIUS distants, consultez la section « exemple de transfert de messages RADIUS par un serveur proxy » dans la rubrique [utiliser des expressions régulières dans NPS](nps-crp-reg-expressions.md).

### <a name="advanced"></a>Avancé

Vous pouvez définir des propriétés avancées pour spécifier la série d’attributs RADIUS :

- Ajouté au message de réponse RADIUS lorsque le serveur NPS est utilisé en tant que serveur d’authentification ou de gestion des comptes RADIUS. Lorsqu’il existe des attributs spécifiés à la fois sur une stratégie réseau et sur la stratégie de demande de connexion, les attributs qui sont envoyés dans le message de réponse RADIUS sont la combinaison des deux ensembles d’attributs.
- Ajouté au message RADIUS lorsque le serveur NPS est utilisé en tant que proxy d’authentification ou de gestion des comptes RADIUS. Si l’attribut existe déjà dans le message qui est transféré, il est remplacé par la valeur de l’attribut spécifié dans la stratégie de demande de connexion. 

En outre, certains attributs qui sont disponibles pour la configuration sous l’onglet **paramètres** de stratégie de demande de connexion dans la catégorie **avancé** fournissent des fonctionnalités spécialisées. Par exemple, vous pouvez configurer l’attribut de mappage de l' **utilisateur RADIUS distant vers Windows** lorsque vous souhaitez fractionner l’authentification et l’autorisation d’une demande de connexion entre deux bases de données de comptes d’utilisateur.

L’attribut de **mappage d’utilisateur RADIUS distant vers Windows** spécifie que l’autorisation Windows se produit pour les utilisateurs authentifiés par un serveur RADIUS distant. En d’autres termes, un serveur RADIUS distant effectue une authentification par rapport à un compte d’utilisateur dans une base de données de comptes d’utilisateurs distants, mais le serveur NPS local autorise la demande de connexion par rapport à un compte d’utilisateur dans une base de données de comptes d’utilisateurs locaux. Cela est utile lorsque vous souhaitez autoriser les visiteurs à accéder à votre réseau.

Par exemple, les visiteurs des organisations partenaires peuvent être authentifiés par leur propre serveur RADIUS d’organisation partenaire, puis utiliser un compte d’utilisateur Windows au niveau de votre organisation pour accéder à un réseau local (LAN) invité sur votre réseau.

Les autres attributs qui fournissent des fonctionnalités spécialisées sont les suivants :

- **MS-Quarantine-IPFilter et MS-Quarantine-Session-Timeout**. Ces attributs sont utilisés lorsque vous déployez le contrôle de quarantaine d’accès réseau (NAQC) avec votre déploiement VPN de routage et d’accès distant.
- **Passport-mappage utilisateur-UPN-suffixe**. Cet attribut vous permet d’authentifier les demandes de connexion avec les informations d’identification du compte d’utilisateur Windows Live™ ID.
- **Tunnel-tag**. Cet attribut désigne le numéro d’ID de réseau local virtuel auquel la connexion doit être affectée par le NAS quand vous déployez des réseaux locaux virtuels (VLAN).

## <a name="default-connection-request-policy"></a>Stratégie de demande de connexion par défaut

Une stratégie de demande de connexion par défaut est créée lorsque vous installez NPS. Cette stratégie a la configuration suivante.

- L’authentification n’est pas configurée.
- La gestion des comptes n’est pas configurée pour transmettre les informations de gestion à un groupe de serveurs RADIUS distants.
- L’attribut n’est pas configuré avec des règles de manipulation d’attribut qui transfèrent les demandes de connexion à des groupes de serveurs RADIUS distants.
- La demande de transfert est configurée de sorte que les demandes de connexion soient authentifiées et autorisées sur le serveur NPS local.
- Les attributs avancés ne sont pas configurés.

La stratégie de demande de connexion par défaut utilise NPS en tant que serveur RADIUS. Pour configurer un serveur exécutant NPS en tant que proxy RADIUS, vous devez également configurer un groupe de serveurs RADIUS distants. Vous pouvez créer un groupe de serveurs RADIUS distants lorsque vous créez une stratégie de demande de connexion à l’aide de l’Assistant Nouvelle stratégie de demande de connexion. Vous pouvez soit supprimer la stratégie de demande de connexion par défaut, soit vérifier que la stratégie de demande de connexion par défaut est la dernière stratégie traitée par NPS en la plaçant en dernier dans la liste triée des stratégies.

>[!NOTE]
>Si NPS et le service d’accès à distance sont installés sur le même ordinateur et que le service d’accès à distance est configuré pour l’authentification et la gestion des comptes Windows, il est possible que les demandes d’authentification d’accès à distance et de gestion des comptes soient transmises à un serveur RADIUS. . Cela peut se produire lorsque les demandes d’authentification et de gestion de l’accès à distance correspondent à une stratégie de demande de connexion qui est configurée pour les transférer vers un groupe de serveurs RADIUS distants.
