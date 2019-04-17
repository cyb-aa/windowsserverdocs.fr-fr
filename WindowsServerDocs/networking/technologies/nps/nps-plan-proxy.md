---
title: Planifier le serveur NPS en tant que proxy RADIUS
description: Cette rubrique fournit des informations sur le déploiement de proxy RADIUS du serveur stratégie réseau planification dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ca77d64a-065b-4bf2-8252-3e75f71b7734
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 966e555ebcac6c7daf4a26b322f0d29f023f8539
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="plan-nps-as-a-radius-proxy"></a>Planifier le serveur NPS en tant que proxy RADIUS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Lorsque vous déployez le serveur NPS (Network Policy) en tant que \(RADIUS\) Remote Authentication Dial-In User Service proxy, NPS reçoit les demandes de connexion des clients RADIUS, tels que les serveurs d’accès réseau ou d’autres proxys RADIUS et transfère ensuite ces demandes de connexion à des serveurs exécutant NPS ou autres serveurs RADIUS. Vous pouvez utiliser ces recommandations en matière de planification pour simplifier le déploiement de RADIUS.

Ces recommandations en matière de planification n’incluent pas les circonstances dans lesquelles vous souhaitez déployer un serveur NPS comme serveur RADIUS. Lorsque vous déployez NPS comme serveur RADIUS, NPS effectue l’authentification, l’autorisation et gestion des comptes pour les demandes de connexion pour le domaine local et pour les domaines qui approuvent le domaine local.

Avant de déployer NPS en tant que proxy RADIUS sur votre réseau, suivez les indications ci-dessous pour planifier votre déploiement.

- Planifier la configuration du serveur NPS.

- Planifier des clients RADIUS.

- Planifier des groupes de serveurs RADIUS distants.

- Planifier les règles de manipulation d’attribut pour le transfert de message.

- Planifier les stratégies de demande de connexion.

- Planifier la gestion des comptes NPS.

## <a name="plan-nps-server-configuration"></a>Planifier la configuration du serveur NPS

Lorsque vous utilisez NPS en tant que proxy RADIUS, NPS transfère les demandes de connexion à un serveur NPS ou d’autres serveurs RADIUS pour le traitement. Pour cette raison, l’appartenance au domaine du proxy NPS n’est pas pertinente. Le serveur proxy n’a pas besoin être enregistré dans les Services de domaine Active Directory \(AD DS\) car il n’a pas besoin d’accéder aux propriétés d’accès à des comptes d’utilisateur. En outre, vous n’avez pas besoin de configurer des stratégies de réseau sur un serveur proxy NPS, car le serveur proxy n’effectue pas d’autorisation pour les demandes de connexion. Le proxy de serveur NPS peut être un membre du domaine ou il peut être un serveur autonome avec aucun l’appartenance au domaine.

NPS doit être configuré pour communiquer avec les clients RADIUS, également appelés serveurs d’accès réseau, en utilisant le protocole RADIUS. Vous pouvez en outre, configurer aux types d’événements qui enregistre NPS dans le journal et vous pouvez entrer une description pour le serveur des événements.

### <a name="key-steps"></a>Décrit les principales étapes

Lors de la planification de la configuration du proxy NPS, vous pouvez utiliser les étapes suivantes.

- Déterminer les ports RADIUS proxy NPS utilise pour recevoir les messages RADIUS à partir de clients RADIUS et envoyer des messages RADIUS pour les membres de groupes de serveurs RADIUS distants. Les ports de protocole UDP (User Datagram) par défaut sont 1812 et 1645 pour les messages d’authentification RADIUS et les ports UDP 1813 et 1646 pour les messages de gestion de comptes RADIUS.

- Si le proxy NPS est configuré avec plusieurs cartes réseau, déterminez les cartes sur laquelle vous souhaitez que le trafic RADIUS à autoriser.

- Déterminer les types d’événements que vous souhaitez enregistrer dans le journal des événements NPS. Vous pouvez connecter des demandes de connexion rejetés, les demandes de connexion réussie ou les deux.

- Déterminer si vous déployez plusieurs proxy NPS. Pour fournir une tolérance de panne, utilisez au moins deux serveurs proxy de serveur NPS. Un proxy NPS est utilisé en tant que proxy RADIUS principal et l’autre est utilisé en tant que sauvegarde. Chaque client RADIUS est ensuite configurée sur les deux serveurs proxy NPS. Si le proxy NPS principal devient indisponible, les clients RADIUS envoient des messages de demande d’accès à un autre proxy NPS.

- Planifier le script utilisé pour copier une configuration de proxy NPS à d’autres proxys NPS pour enregistrer sur la charge administrative et éviter une configuration incorrecte d’un serveur. NPS fournit les commandes Netsh que vous permettent de copier tout ou partie d’une configuration de proxy NPS pour les importer sur un autre proxy NPS. Vous pouvez exécuter les commandes manuellement à l’invite de commandes Netsh. Toutefois, si vous enregistrez votre séquence de commandes en tant que script, vous pouvez exécuter le script à une date ultérieure si vous décidez de modifier vos configurations de proxy.

## <a name="plan-radius-clients"></a>Planifier des clients RADIUS

Clients RADIUS sont des serveurs d’accès réseau, tels que les points d’accès sans fil, serveurs \(VPN\) de réseau privé virtuel, 802.1 commutateurs compatibles X et les serveurs d’accès à distance. Proxy RADIUS, ce qui permet de transférer des messages demande aux serveurs RADIUS de connexion, est également des clients RADIUS. NPS prend en charge tous les serveurs d’accès réseau et des proxys RADIUS conformes avec le protocole RADIUS, comme décrit dans la RFC 2865, «distant Authentication Dial-in User Service \(RADIUS\),» et la RFC 2866, «Gestion de comptes RADIUS».

En outre, les points d’accès sans fil et commutateurs doivent être capables d’authentification de 802. 1 X. Si vous souhaitez déployer le protocole EAP (Extensible Authentication) ou Extensible Authentication Protocol PEAP (Protected), les commutateurs et les points d’accès doivent prendre en charge l’utilisation du protocole EAP.

Pour tester une interopérabilité de base pour les connexions PPP pour les points d’accès sans fil, configurez le point d’accès et le client d’accès à utiliser le protocole PAP (Password Authentication Protocol). Utilisez des protocoles d’authentification PPP supplémentaires, telles que PEAP, jusqu'à ce que vous avez testé ceux que vous prévoyez d’utiliser pour l’accès réseau.

### <a name="key-steps"></a>Décrit les principales étapes

Lors de la planification pour les clients RADIUS, vous pouvez utiliser les étapes suivantes.

- Document spécifique au fournisseur attributs au que vous devez configurer dans NPS. Si votre NAS requièrent au, ouvrez une session les informations VSA pour une utilisation ultérieure lorsque vous configurez vos stratégies réseau dans NPS.

- Les adresses IP des clients RADIUS et de votre proxy NPS pour simplifier la configuration de tous les périphériques de numérisation de document. Lorsque vous déployez vos clients RADIUS, vous devez les configurer pour utiliser le protocole RADIUS, avec l’adresse IP de proxy NPS entré en tant que le serveur d’authentification. Et lorsque vous configurez le serveur NPS pour communiquer avec vos clients RADIUS, vous devez entrer les adresses IP du client RADIUS dans le composant logiciel enfichable NPS.

- Créer des secrets partagés pour la configuration sur les clients RADIUS et dans le composant logiciel enfichable serveur NPS. Vous devez configurer les clients RADIUS avec un secret partagé, ou un mot de passe, vous devrez également entrer dans le composant logiciel enfichable NPS lors de la configuration de clients RADIUS dans NPS.

## <a name="plan-remote-radius-server-groups"></a>Planifier des groupes de serveurs RADIUS distants

Lorsque vous configurez un groupe de serveurs RADIUS distants sur un serveur proxy NPS, vous indiquez le proxy de serveur NPS où envoyer connexion certains ou tous les messages de demande qu’il reçoit à partir des serveurs d’accès réseau et proxys NPS ou autres proxys RADIUS.

Vous pouvez utiliser le serveur NPS comme un proxy RADIUS pour transférer la connexion demande à un ou des groupes de serveurs RADIUS distants plus et chaque groupe peuvent contenir un ou plusieurs serveurs RADIUS. Lorsque vous souhaitez que le proxy NPS pour transférer des messages à plusieurs groupes, configurer une stratégie de demande de connexion par groupe. La stratégie de demande de connexion contient des informations supplémentaires, telles que les règles de manipulation d’attribut, qui indiquent le proxy NPS les messages à envoyer à un groupe de serveurs RADIUS distants spécifié dans la stratégie.

Vous pouvez configurer des groupes de serveurs RADIUS distants à l’aide des commandes Netsh pour NPS, en configurant des groupes directement dans le composant logiciel enfichable serveur NPS sous les groupes de serveurs RADIUS distants ou en exécutant l’Assistant Nouvelle stratégie de demande de connexion.

### <a name="key-steps"></a>Décrit les principales étapes

Lors de la planification de groupes de serveurs RADIUS distants, vous pouvez utiliser les étapes suivantes.

- Déterminez les domaines qui contiennent les serveurs RADIUS auquel vous souhaitez que le proxy NPS pour transférer les demandes de connexion. Ces domaines contiennent les comptes d’utilisateur pour les utilisateurs qui se connectent au réseau via les clients RADIUS que vous déployez.

- Déterminer si vous devez ajouter de nouveaux serveurs RADIUS dans les domaines où RADIUS n’est pas déjà déployé.

- Documentez les adresses IP des serveurs RADIUS que vous souhaitez ajouter à des groupes de serveurs RADIUS distants.

- Déterminez combien groupes de serveurs RADIUS distants vous devez créer. Dans certains cas, il est préférable créer un groupe de serveurs RADIUS distants par domaine, puis ajoutez les serveurs RADIUS pour le domaine au groupe. Toutefois, il peut arriver dans lequel vous avez une grande quantité de ressources dans un domaine, y compris un grand nombre d’utilisateurs avec des comptes d’utilisateur dans le domaine, un grand nombre de contrôleurs de domaine et un grand nombre de serveurs RADIUS. Ou votre domaine peut-être couvrir une zone géographique volumineuse, vous devez disposer de serveurs d’accès réseau et les serveurs RADIUS dans des emplacements distants entre eux à l’origine. Dans ces et éventuellement autres cas, vous pouvez créer plusieurs groupes de serveurs RADIUS distants par domaine.

- Créer des secrets partagés pour la configuration sur le proxy NPS et sur les serveurs RADIUS distants.

## <a name="plan-attribute-manipulation-rules-for-message-forwarding"></a>Planifier les règles de manipulation d’attribut pour le transfert de message

Règles de manipulation d’attribut, qui sont configurés dans les stratégies de demande de connexion, permettent d’identifier les messages de demande d’accès que vous souhaitez transférer à un groupe de serveurs RADIUS distants spécifique.

Vous pouvez configurer NPS pour transférer toutes les demandes de connexion à un groupe de serveurs RADIUS distants sans l’aide de règles de manipulation d’attribut.

Si vous avez plus d’un emplacement auquel vous souhaitez transférer les demandes de connexion, toutefois, vous devez créer une stratégie de demande de connexion pour chaque emplacement, puis configurer la stratégie avec le groupe de serveurs RADIUS distants pour lesquels vous souhaitez transférer des messages, ainsi qu’avec les règles de manipulation d’attribut qui indiquent NPS les messages à transférer.

Vous pouvez créer des règles pour les attributs suivants.

- ID de Station appelée Le numéro de téléphone du serveur d’accès réseau (NAS). La valeur de cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe des critères spéciaux pour spécifier des codes de zone.

- ID de Station. appelant Le numéro de téléphone utilisé par l’appelant. La valeur de cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe des critères spéciaux pour spécifier des codes de zone.

- Nom d’utilisateur. Le nom d’utilisateur qui est fourni par le client d’accès et qui est inclus par le NAS dans le message de demande d’accès RADIUS. La valeur de cet attribut est une chaîne de caractères qui contient généralement un nom de domaine et un nom de compte d’utilisateur.

Pour remplacer ou convertir les noms de domaine dans le nom d’utilisateur d’une demande de connexion correctement, vous devez configurer des règles de manipulation d’attribut pour l’attribut de nom d’utilisateur sur la stratégie de demande de connexion appropriée.

### <a name="key-steps"></a>Décrit les principales étapes

Lors de la planification pour les règles de manipulation d’attribut, vous pouvez utiliser les étapes suivantes.

- Planifier le routage des messages à partir de la NAS via le proxy pour les serveurs RADIUS distants pour vérifier que vous disposez d’un chemin d’accès logique permettant de transférer les messages sur les serveurs RADIUS.

- Déterminer un ou plusieurs attributs que vous souhaitez utiliser pour chaque stratégie de demande de connexion.

- Les règles de manipulation d’attribut que vous prévoyez d’utiliser pour chaque stratégie de demande de connexion de documents et correspondent aux règles au groupe de serveurs RADIUS distant auquel les messages sont transférés.

## <a name="plan-connection-request-policies"></a>Planifier les stratégies de demande de connexion

La stratégie de demande de connexion par défaut est configurée pour NPS lorsqu’il est utilisé comme serveur RADIUS. Stratégies de demande de connexion supplémentaires peuvent être utilisées pour définir des conditions plus spécifiques, de créer des règles de manipulation qui indiquent que les messages à transférer à des groupes de serveurs RADIUS distants et pour spécifier les attributs avancés à NPS attribut. Utilisez l’Assistant Nouvelle stratégie de demande de connexion pour créer des stratégies de demande de connexion courantes ou personnalisées.

### <a name="key-steps"></a>Décrit les principales étapes

Lors de la planification de stratégies de demande de connexion, vous pouvez utiliser les étapes suivantes.

- Supprimer la stratégie de demande de connexion par défaut sur chaque serveur NPS qui fonctionne uniquement en tant que proxy RADIUS.

- Planifier des conditions supplémentaires et les paramètres qui sont requis pour chaque stratégie, la combinaison de ces informations avec le groupe de serveurs RADIUS distants et les règles de manipulation d’attribut planifiées pour la stratégie.

- Créer le plan pour distribuer des stratégies de demande de connexion commun à tous les serveurs proxy NPS. Créer des stratégies communs à plusieurs proxys NPS sur un serveur NPS, puis utilisez les commandes Netsh pour NPS pour importer la connexion demande des stratégies et le serveur de configuration sur tous les autres serveurs proxy de fédération.

## <a name="plan-nps-accounting"></a>Planifier la gestion des comptes NPS

Lorsque vous configurez le serveur NPS en tant que proxy RADIUS, vous pouvez le configurer pour effectuer la gestion de comptes RADIUS à l’aide de fichiers journaux, fichiers journaux de format compatible avec la base de données ou NPS SQL Server NPS journalisation.

Vous pouvez également transférer des messages de la gestion des comptes à un groupe de serveurs RADIUS distants qui effectue la gestion des comptes à l’aide d’un de ces formats de journalisation.

### <a name="key-steps"></a>Décrit les principales étapes

Lors de la planification pour la gestion des comptes NPS, vous pouvez utiliser les étapes suivantes.

- Déterminez si vous souhaitez que le proxy NPS pour effectuer des services de comptabilité ou de transférer des messages de comptabilité à un groupe de serveurs RADIUS distant pour la gestion des comptes.

- Envisagez de désactiver le proxy de serveur NPS local comptabilité si vous envisagez de transférer les messages de comptabilité à d’autres serveurs.

- Planifier les étapes de configuration de stratégie de demande connexion si vous envisagez de transférer les messages de comptabilité à d’autres serveurs. Si vous désactivez la gestion des comptes locale pour le proxy NPS, chaque stratégie de demande de connexion que vous configurez sur ce serveur proxy doit disposer de transfert des messages comptabilité activée et configurée correctement.

- Déterminer le format de journal que vous souhaitez utiliser: les fichiers journaux au format IAS, fichiers journaux de format compatible avec la base de données ou la journalisation NPS SQL Server.

Pour configurer l’équilibrage de charge pour le serveur NPS en tant que proxy RADIUS, consultez [NPS Proxy Server l’équilibrage de charge](nps-manage-proxy-lb.md).

