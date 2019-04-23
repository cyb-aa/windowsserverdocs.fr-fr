---
title: Planifier un serveur NPS en tant que proxy RADIUS
description: Cette rubrique fournit des informations sur la planification dans Windows Server 2016 du déploiement de proxy RADIUS du serveur stratégie réseau.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ca77d64a-065b-4bf2-8252-3e75f71b7734
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 83fbe57ee62480439190dcc53428e02a4f8e6897
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829560"
---
# <a name="plan-nps-as-a-radius-proxy"></a>Planifier un serveur NPS en tant que proxy RADIUS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Lorsque vous déployez le serveur NPS (Network Policy) comme un Remote Authentication Dial-In User Service \(RADIUS\) proxy, NPS reçoit les demandes de connexion des clients RADIUS, tels que les serveurs d’accès réseau ou d’autres proxys RADIUS, puis transfère ces demandes de connexion aux serveurs exécutant NPS ou autres serveurs RADIUS. Vous pouvez utiliser ces instructions de planification pour simplifier votre déploiement de RADIUS.

Ces instructions de planification n’incluent pas les circonstances dans lesquelles vous souhaitez déployer NPS comme serveur RADIUS. Lorsque vous déployez le serveur NPS comme serveur RADIUS, NPS effectue l’authentification, autorisation et comptabilité pour les demandes de connexion pour le domaine local et pour les domaines qui approuvent le domaine local.

Avant de déployer NPS en tant que proxy RADIUS sur votre réseau, utilisez les instructions suivantes pour planifier votre déploiement.

- Planifier la configuration de serveur NPS.

- Planifier des clients RADIUS.

- Planifier des groupes de serveurs RADIUS distants.

- Planifier les règles de manipulation d’attribut pour le transfert des messages.

- Planifier les stratégies de demande de connexion.

- Planifier la gestion des comptes NPS.

## <a name="plan-nps-configuration"></a>Planifier la configuration du serveur NPS

Lorsque vous utilisez NPS en tant que proxy RADIUS, NPS transfère les demandes de connexion à un serveur NPS ou d’autres serveurs RADIUS pour le traitement. Pour cette raison, l’appartenance au domaine du proxy NPS est sans importance. Le proxy ne doive pas être inscrits dans les Services de domaine Active Directory \(AD DS\) , car il n’a pas besoin d’accéder aux propriétés d’accès à distance des comptes d’utilisateur. En outre, vous n’avez pas besoin de configurer des stratégies de réseau sur un proxy de serveur NPS, car le proxy n’effectue pas d’autorisation des demandes de connexion. Le proxy de serveur NPS peut être un membre du domaine, ou il peut être un serveur autonome avec aucune appartenance au domaine.

NPS doit être configuré pour communiquer avec les clients RADIUS, également appelés serveurs d’accès réseau, en utilisant le protocole RADIUS. En outre, configurables les types d’événements qui enregistre NPS dans le journal et vous pouvez entrer une description pour le serveur des événements.

### <a name="key-steps"></a>Étapes clés

Lors de la planification de configuration du proxy NPS, vous pouvez utiliser les étapes suivantes.

- Déterminer les ports RADIUS par proxy NPS pour recevoir des messages RADIUS à partir de clients RADIUS et envoyer des messages RADIUS aux membres des groupes de serveurs RADIUS distants. Les ports de protocole UDP (User Datagram) par défaut sont 1812 et 1645 pour les messages d’authentification RADIUS et les ports UDP 1813 et 1646 pour les messages de gestion des comptes RADIUS.

- Si le proxy de serveur NPS est configuré avec plusieurs cartes réseau, déterminez les adaptateurs sur laquelle vous souhaitez le trafic RADIUS pour être autorisée.

- Déterminer les types d’événements que NPS enregistre dans le journal des événements. Vous pouvez consigner les demandes de connexion rejetées, les demandes de connexion réussie ou les deux.

- Déterminer si vous déployez plusieurs proxy NPS. Pour fournir une tolérance de panne, utilisez au moins deux serveurs proxy de serveur NPS. Un proxy de serveur NPS est utilisé en tant que proxy RADIUS principal et l’autre est utilisée en tant que sauvegarde. Chaque client RADIUS est ensuite configuré sur les deux serveurs proxy de serveur NPS. Si le proxy NPS principal devient indisponible, les clients RADIUS envoient des messages de demande d’accès au proxy NPS autre.

- Planifier le script utilisé pour copier une configuration de proxy NPS auprès d’autres proxys de serveur NPS pour économiser sur les coûts d’administration et pour empêcher une configuration incorrecte d’un serveur. NPS fournit les commandes Netsh qui vous permettent de copier tout ou partie d’une configuration de proxy de serveur NPS pour les importer sur un autre proxy NPS. Vous pouvez exécuter les commandes manuellement à l’invite Netsh. Toutefois, si vous enregistrez votre séquence de commandes sous forme de script, vous pouvez exécuter le script à une date ultérieure si vous décidez de modifier vos configurations de proxy.

## <a name="plan-radius-clients"></a>Planifier des clients RADIUS

Les clients RADIUS sont des serveurs d’accès réseau, tels que les points d’accès sans fil, réseau privé virtuel \(VPN\) serveurs, 802.1 commutateurs compatibles X et les serveurs d’accès à distance. Les proxys RADIUS, qui transfèrent les messages de demande pour les serveurs RADIUS de la connexion, sont également des clients RADIUS. NPS prend en charge tous les serveurs d’accès réseau et les proxys RADIUS qui sont conformes avec le protocole RADIUS, comme décrit dans la RFC 2865, « Remote Authentication Dial-in User Service \(RADIUS\), » et RFC 2866, « Gestion des comptes RADIUS ».

En outre, les points d’accès sans fil et commutateurs doivent être capables d’authentification de 802. 1 X. Si vous souhaitez déployer le protocole EAP (Extensible Authentication) ou Extensible Authentication Protocol PEAP (Protected), les commutateurs et les points d’accès doivent prendre en charge l’utilisation du protocole EAP.

Pour tester une interopérabilité de base pour les connexions PPP pour les points d’accès sans fil, configurez le point d’accès et le client d’accès à utiliser le protocole PAP (Password Authentication Protocol). Utiliser des protocoles d’authentification PPP supplémentaires, telles que PEAP, jusqu'à ce que vous avez testé ceux que vous souhaitez utiliser pour l’accès réseau.

### <a name="key-steps"></a>Étapes clés

Lors de la planification pour les clients RADIUS, vous pouvez utiliser les étapes suivantes.

- Documentez les attributs spécifiques au fournisseur (VSA) vous devez configurer dans NPS. Si votre NAS requièrent VSA, consigner les informations de VSA pour une utilisation ultérieure lorsque vous configurez vos stratégies réseau dans NPS.

- Documentez les adresses IP de clients RADIUS et de votre proxy NPS pour simplifier la configuration de tous les périphériques. Lorsque vous déployez vos clients RADIUS, vous devez les configurer pour utiliser le protocole RADIUS, avec l’adresse IP de proxy NPS entré en tant que le serveur d’authentification. Et lorsque vous configurez le serveur NPS pour communiquer avec vos clients RADIUS, vous devez entrer les adresses IP du client RADIUS dans le composant logiciel enfichable NPS.

- Créer des secrets partagés pour la configuration sur les clients RADIUS et dans le composant logiciel enfichable NPS. Vous devez configurer les clients RADIUS avec un secret partagé ou le mot de passe, que vous entrerez également dans le composant logiciel enfichable NPS lors de la configuration des clients RADIUS dans NPS.

## <a name="plan-remote-radius-server-groups"></a>Planifier des groupes de serveurs RADIUS distants

Lorsque vous configurez un groupe de serveurs RADIUS distants sur un proxy de serveur NPS, vous indiquez le proxy de serveur NPS où envoyer connexion certains ou tous les messages de demande qu’il reçoit à partir des serveurs d’accès réseau et proxys de serveur NPS ou autres proxys RADIUS.

Vous pouvez utiliser le serveur NPS comme proxy RADIUS pour transférer la connexion demande à un ou des groupes de serveurs RADIUS distants de plus et chaque groupe peuvent contenir un ou plusieurs serveurs RADIUS. Lorsque vous souhaitez que le proxy de serveur NPS pour transférer des messages à plusieurs groupes, configurer une stratégie de demande de connexion par groupe. La stratégie de demande de connexion contient des informations supplémentaires, telles que les règles de manipulation d’attribut, qui indiquent le proxy NPS les messages à envoyer au groupe de serveurs RADIUS distant spécifié dans la stratégie.

Vous pouvez configurer des groupes de serveurs RADIUS distants en utilisant les commandes Netsh pour NPS, en configurant des groupes directement dans le composant logiciel enfichable NPS sous les groupes de serveurs RADIUS distants ou en exécutant l’Assistant Nouvelle stratégie de demande de connexion.

### <a name="key-steps"></a>Étapes clés

Lors de la planification pour les groupes de serveurs RADIUS distants, vous pouvez utiliser les étapes suivantes.

- Déterminer les domaines qui contiennent les serveurs RADIUS auquel vous souhaitez que le proxy de serveur NPS pour transférer les demandes de connexion. Ces domaines contiennent les comptes d’utilisateur pour les utilisateurs qui se connectent au réseau via les clients RADIUS que vous déployez.

- Déterminer s’il faut ajouter les nouveaux serveurs RADIUS dans des domaines où RADIUS n’est pas déjà déployé.

- Documentez les adresses IP des serveurs RADIUS que vous souhaitez ajouter à des groupes de serveurs RADIUS distants.

- Déterminez combien groupes de serveurs RADIUS distants vous devez créer. Dans certains cas, il est préférable de créer un groupe de serveurs RADIUS distants par domaine, puis ajoutez les serveurs RADIUS pour le domaine au groupe. Toutefois, il peut arriver dans lequel vous avez une grande quantité de ressources dans un domaine, y compris un grand nombre d’utilisateurs avec des comptes d’utilisateur dans le domaine, un grand nombre de contrôleurs de domaine et un grand nombre de serveurs RADIUS. Ou votre domaine peut-être couvrir une large zone géographique, à l’origine que vous disposiez de serveurs d’accès réseau et les serveurs RADIUS dans les emplacements sont distants les uns des autres. Dans ces et éventuellement autres cas, vous pouvez créer plusieurs groupes de serveurs RADIUS distants par domaine.

- Créer des secrets partagés pour la configuration sur le proxy de serveur NPS et sur les serveurs RADIUS distants.

## <a name="plan-attribute-manipulation-rules-for-message-forwarding"></a>Planifier les règles de manipulation d’attribut pour le transfert des messages

Les règles de manipulation d’attribut, qui sont configurés dans les stratégies de demande de connexion, permettent d’identifier les messages de demande d’accès que vous souhaitez transférer à un groupe de serveurs RADIUS distant spécifique.

Vous pouvez configurer NPS pour transférer toutes les demandes de connexion à un groupe de serveurs RADIUS distants sans utiliser les règles de manipulation d’attribut.

Si vous avez plus d’un emplacement vers lequel vous souhaitez transférer les demandes de connexion, toutefois, vous devez créer une stratégie de demande de connexion pour chaque emplacement, puis configurez la stratégie avec le groupe de serveurs RADIUS distant auquel vous souhaitez transférer les messages, ainsi que avec les règles de manipulation d’attribut qui indiquent à NPS les messages à transférer.

Vous pouvez créer des règles pour les attributs suivants.

- Called-Station-ID. Le numéro de téléphone du serveur d’accès réseau (NAS). La valeur de cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe des critères spéciaux pour spécifier les indicatifs.

- Appel-Station-ID. Le numéro de téléphone utilisé par l’appelant. La valeur de cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe des critères spéciaux pour spécifier les indicatifs.

- Nom d’utilisateur. Le nom d’utilisateur qui est fourni par le client d’accès et qui est inclus par le serveur NAS dans le message de demande d’accès RADIUS. La valeur de cet attribut est une chaîne de caractères qui contient généralement un nom de domaine et un nom de compte d’utilisateur.

Pour remplacer ou convertir des noms de domaine dans le nom d’utilisateur d’une demande de connexion correctement, vous devez configurer des règles de manipulation d’attribut pour l’attribut de nom d’utilisateur sur la stratégie de demande de connexion appropriée.

### <a name="key-steps"></a>Étapes clés

Lors de la planification pour les règles de manipulation d’attribut, vous pouvez utiliser les étapes suivantes.

- Planifier le routage des messages à partir de l’accès réseau via le proxy pour les serveurs RADIUS distants pour vérifier que vous disposez d’un chemin d’accès logique auquel transférer les messages sur les serveurs RADIUS.

- Déterminer un ou plusieurs attributs que vous souhaitez utiliser pour chaque stratégie de demande de connexion.

- Documentez les règles de manipulation d’attribut que vous projetez d’utiliser pour chaque stratégie de demande de connexion et correspond aux règles au groupe de serveurs RADIUS distant auquel les messages sont transférés.

## <a name="plan-connection-request-policies"></a>Planifier les stratégies de demande de connexion

La stratégie de demande de connexion par défaut est configurée pour NPS lorsqu’il est utilisé comme un serveur RADIUS. Stratégies de demande de connexion supplémentaires peuvent être utilisés pour définir des conditions plus spécifiques, de créer des règles de manipulation qui indiquent à NPS les messages à transférer vers les groupes de serveurs RADIUS distants et pour spécifier des attributs avancés d’attribut. Utilisez l’Assistant Nouvelle stratégie de demande de connexion pour créer des stratégies de demande de connexion courantes ou personnalisées.

### <a name="key-steps"></a>Étapes clés

Lors de la planification pour les stratégies de demande de connexion, vous pouvez utiliser les étapes suivantes.

- Supprimer la stratégie de demande de connexion par défaut sur chaque serveur exécutant NPS qui fonctionne uniquement en tant que proxy RADIUS.

- Planifiez des conditions supplémentaires et les paramètres requis pour chaque stratégie, combinaison de ces informations avec le groupe de serveurs RADIUS distants et les règles de manipulation d’attribut prévues pour la stratégie.

- Concevez le plan pour distribuer des stratégies de demande de connexion commune à tous les proxys de serveur NPS. Créer des stratégies communes à plusieurs serveurs proxy de serveur NPS sur un serveur NPS, puis utilisez les commandes Netsh pour NPS pour importer la connexion demande stratégies et le serveur de configuration sur tous les autres serveurs proxy.

## <a name="plan-nps-accounting"></a>Planifier la gestion des comptes NPS

Lorsque vous configurez le serveur NPS en tant que proxy RADIUS, vous pouvez le configurer pour effectuer la gestion de comptes RADIUS à l’aide des fichiers journaux au format NPS, les fichiers journaux au format de base de données compatibles ou NPS SQL Server journalisation.

Vous pouvez également transférer les messages de comptabilité à un groupe de serveurs RADIUS distant qui effectue la gestion des comptes en utilisant l’une de ces formats de journalisation.

### <a name="key-steps"></a>Étapes clés

Lors de la planification pour la gestion des comptes NPS, vous pouvez utiliser les étapes suivantes.

- Déterminez si vous souhaitez que le proxy de serveur NPS pour effectuer des services de gestion des comptes ou pour transférer les messages de comptabilité à un groupe de serveurs RADIUS distant pour la comptabilité.

- Envisagez de désactiver le proxy de serveur NPS local comptabilité si vous envisagez de transférer les messages de comptabilité à d’autres serveurs.

- Planifier les étapes de configuration de stratégie de demande connexion si vous envisagez de transférer les messages de comptabilité à d’autres serveurs. Si vous désactivez la gestion des comptes locale pour le proxy de serveur NPS, chaque stratégie de demande de connexion que vous configurez sur ce proxy doit avoir le transfert de messages de comptabilité activée et configurée correctement.

- Déterminer le format de journalisation que vous souhaitez utiliser : Fichiers de journaux au format IAS, fichiers journaux au format de base de données compatibles ou journalisation NPS SQL Server.

Pour configurer l’équilibrage de charge pour le serveur NPS en tant que proxy RADIUS, consultez [NPS Proxy Server l’équilibrage de charge](nps-manage-proxy-lb.md).

