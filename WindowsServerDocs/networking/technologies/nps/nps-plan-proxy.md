---
title: Planifier un serveur NPS en tant que proxy RADIUS
description: Cette rubrique fournit des informations sur la planification du déploiement du proxy RADIUS du serveur de stratégie réseau dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ca77d64a-065b-4bf2-8252-3e75f71b7734
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d64fceaf7242b7fe44912f105229c132ef9ee3b3
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315755"
---
# <a name="plan-nps-as-a-radius-proxy"></a>Planifier un serveur NPS en tant que proxy RADIUS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Lorsque vous déployez un serveur NPS (Network Policy Server) en tant que protocole RADIUS (Remote Authentication Dial-In User Service) \(proxy\) RADIUS, NPS reçoit les demandes de connexion des clients RADIUS, tels que les serveurs d’accès réseau ou d’autres proxys RADIUS, puis transfère ces demandes de connexion aux serveurs qui exécutent NPS ou d’autres serveurs RADIUS. Vous pouvez utiliser ces instructions de planification pour simplifier votre déploiement RADIUS.

Ces instructions de planification n’incluent pas les circonstances dans lesquelles vous souhaitez déployer NPS en tant que serveur RADIUS. Lorsque vous déployez NPS en tant que serveur RADIUS, NPS effectue l’authentification, l’autorisation et la gestion des comptes pour les demandes de connexion pour le domaine local et pour les domaines qui approuvent le domaine local.

Avant de déployer NPS en tant que proxy RADIUS sur votre réseau, suivez les instructions ci-dessous pour planifier votre déploiement.

- Planifier la configuration du serveur NPS.

- Planifiez les clients RADIUS.

- Planifier des groupes de serveurs RADIUS distants.

- Planifiez les règles de manipulation des attributs pour le transfert des messages.

- Planifiez les stratégies de demande de connexion.

- Planifier la gestion des comptes NPS.

## <a name="plan-nps-configuration"></a>Planifier la configuration de NPS

Lorsque vous utilisez NPS en tant que proxy RADIUS, NPS transfère les demandes de connexion à un serveur NPS ou à d’autres serveurs RADIUS à des fins de traitement. Pour cette raison, l’appartenance au domaine du proxy NPS n’est pas pertinente. Le proxy n’a pas besoin d’être inscrit dans Active Directory Domain Services \(AD DS\), car il n’a pas besoin d’accéder aux propriétés de numérotation des comptes d’utilisateur. En outre, vous n’avez pas besoin de configurer des stratégies réseau sur un proxy NPS, car le proxy n’effectue pas d’autorisation pour les demandes de connexion. Le proxy NPS peut être membre d’un domaine ou il peut être un serveur autonome sans appartenance à un domaine.

NPS doit être configuré pour communiquer avec les clients RADIUS, également appelés serveurs d’accès réseau, à l’aide du protocole RADIUS. En outre, vous pouvez configurer les types d’événements que le serveur NPS enregistre dans le journal des événements et vous pouvez entrer une description pour le serveur.

### <a name="key-steps"></a>Étapes clés

Pendant la planification de la configuration du proxy NPS, vous pouvez suivre les étapes ci-dessous.

- Déterminez les ports RADIUS que le proxy NPS utilise pour recevoir des messages RADIUS des clients RADIUS et pour envoyer des messages RADIUS aux membres des groupes de serveurs RADIUS distants. Les ports UDP (User Datagram Protocol) par défaut sont 1812 et 1645 pour les messages d’authentification RADIUS et les ports UDP 1813 et 1646 pour les messages de gestion des comptes RADIUS.

- Si le proxy NPS est configuré avec plusieurs cartes réseau, déterminez les adaptateurs sur lesquels vous souhaitez autoriser le trafic RADIUS.

- Déterminez les types d’événements que vous souhaitez que le serveur NPS enregistre dans le journal des événements. Vous pouvez consigner les demandes de connexion rejetées, les demandes de connexion réussies ou les deux.

- Déterminez si vous déployez plusieurs proxys NPS. Pour fournir une tolérance de panne, utilisez au moins deux proxys NPS. Un proxy NPS est utilisé en tant que proxy RADIUS principal et l’autre en tant que sauvegarde. Chaque client RADIUS est ensuite configuré sur les deux proxys NPS. Si le proxy NPS principal devient indisponible, les clients RADIUS envoient des messages de demande d’accès à l’autre proxy NPS.

- Planifiez le script utilisé pour copier une configuration de proxy NPS vers d’autres proxys NPS afin de réduire la charge administrative et empêcher la configuration incorrecte d’un serveur. NPS fournit les commandes netsh qui vous permettent de copier la totalité ou une partie d’une configuration de proxy NPS pour l’importer sur un autre proxy NPS. Vous pouvez exécuter les commandes manuellement à l’invite netsh. Toutefois, si vous enregistrez votre séquence de commandes en tant que script, vous pouvez exécuter le script à une date ultérieure si vous décidez de modifier vos configurations de proxy.

## <a name="plan-radius-clients"></a>Planifier des clients RADIUS

Les clients RADIUS sont des serveurs d’accès réseau, tels que les points d’accès sans fil, les réseaux privés virtuels \(les serveurs\) VPN, les commutateurs 802.1 X et les serveurs d’accès à distance. Les proxys RADIUS, qui transfèrent les messages de demande de connexion aux serveurs RADIUS, sont également des clients RADIUS. NPS prend en charge tous les serveurs d’accès réseau et proxys RADIUS conformes au protocole RADIUS, comme décrit dans le document RFC 2865, « Remote Authentication Dial-in User Service \(RADIUS\)» et RFC 2866, « RADIUS Accounting ».

En outre, les commutateurs et les points d’accès sans fil doivent être en charge de l’authentification 802.1 X. Si vous souhaitez déployer le protocole EAP (Extensible Authentication Protocol) ou le protocole PEAP (Protected Extensible Authentication Protocol), les points d’accès et les commutateurs doivent prendre en charge l’utilisation d’EAP.

Pour tester l’interopérabilité de base pour les connexions PPP pour les points d’accès sans fil, configurez le point d’accès et le client d’accès pour utiliser le protocole PAP (Password Authentication Protocol). Utilisez des protocoles d’authentification PPP supplémentaires, tels que PEAP, jusqu’à ce que vous ayez testé ceux que vous envisagez d’utiliser pour l’accès au réseau.

### <a name="key-steps"></a>Étapes clés

Au cours de la planification des clients RADIUS, vous pouvez suivre les étapes ci-dessous.

- Documentez les attributs spécifiques au fournisseur (VSA) que vous devez configurer dans NPS. Si vos serveurs de fichiers requièrent VSA, consignez les informations VSA pour une utilisation ultérieure lorsque vous configurez vos stratégies réseau dans NPS.

- Documentez les adresses IP des clients RADIUS et votre proxy NPS pour simplifier la configuration de tous les appareils. Lorsque vous déployez vos clients RADIUS, vous devez les configurer pour qu’ils utilisent le protocole RADIUS, avec l’adresse IP du proxy NPS entrée en tant que serveur d’authentification. Et lorsque vous configurez NPS pour communiquer avec vos clients RADIUS, vous devez entrer les adresses IP du client RADIUS dans le composant logiciel enfichable NPS.

- Créer des secrets partagés pour la configuration sur les clients RADIUS et dans le composant logiciel enfichable NPS. Vous devez configurer les clients RADIUS avec un secret partagé, ou un mot de passe, que vous allez également entrer dans le composant logiciel enfichable NPS lors de la configuration des clients RADIUS dans NPS.

## <a name="plan-remote-radius-server-groups"></a>Planifier des groupes de serveurs RADIUS distants

Quand vous configurez un groupe de serveurs RADIUS distants sur un proxy NPS, vous indiquez au proxy NPS où envoyer certains ou tous les messages de demande de connexion qu’il reçoit des serveurs d’accès réseau et des proxys NPS ou d’autres proxys RADIUS.

Vous pouvez utiliser NPS en tant que proxy RADIUS pour transférer les demandes de connexion à un ou plusieurs groupes de serveurs RADIUS distants, et chaque groupe peut contenir un ou plusieurs serveurs RADIUS. Lorsque vous souhaitez que le proxy NPS transfère des messages à plusieurs groupes, configurez une stratégie de demande de connexion par groupe. La stratégie de demande de connexion contient des informations supplémentaires, telles que des règles de manipulation d’attribut, qui indiquent au proxy NPS les messages à envoyer au groupe de serveurs RADIUS distants spécifié dans la stratégie.

Vous pouvez configurer des groupes de serveurs RADIUS distants à l’aide des commandes netsh pour NPS, en configurant des groupes directement dans le composant logiciel enfichable NPS sous groupes de serveurs RADIUS distants ou en exécutant l’Assistant Nouvelle stratégie de demande de connexion.

### <a name="key-steps"></a>Étapes clés

Lors de la planification des groupes de serveurs RADIUS distants, vous pouvez suivre les étapes ci-dessous.

- Déterminez les domaines qui contiennent les serveurs RADIUS auxquels vous souhaitez que le proxy NPS transfère les demandes de connexion. Ces domaines contiennent les comptes d’utilisateur des utilisateurs qui se connectent au réseau via les clients RADIUS que vous déployez.

- Déterminez si vous devez ajouter de nouveaux serveurs RADIUS dans des domaines où RADIUS n’est pas déjà déployé.

- Documentez les adresses IP des serveurs RADIUS que vous souhaitez ajouter aux groupes de serveurs RADIUS distants.

- Déterminez le nombre de groupes de serveurs RADIUS distants que vous devez créer. Dans certains cas, il est préférable de créer un groupe de serveurs RADIUS distants par domaine, puis d’ajouter les serveurs RADIUS pour le domaine au groupe. Toutefois, il peut arriver que vous disposiez d’une grande quantité de ressources dans un domaine, notamment un grand nombre d’utilisateurs avec des comptes d’utilisateur dans le domaine, un grand nombre de contrôleurs de domaine et un grand nombre de serveurs RADIUS. Ou votre domaine peut couvrir une vaste zone géographique, ce qui vous permet d’avoir des serveurs d’accès réseau et des serveurs RADIUS situés à des emplacements éloignés les uns des autres. Dans ces cas, et éventuellement dans d’autres, vous pouvez créer plusieurs groupes de serveurs RADIUS distants par domaine.

- Créer des secrets partagés pour la configuration sur le proxy NPS et sur les serveurs RADIUS distants.

## <a name="plan-attribute-manipulation-rules-for-message-forwarding"></a>Planifier les règles de manipulation des attributs pour le transfert de messages

Les règles de manipulation d’attributs, qui sont configurées dans les stratégies de demande de connexion, vous permettent d’identifier les messages de demande d’accès que vous souhaitez transférer à un groupe de serveurs RADIUS distants spécifique.

Vous pouvez configurer NPS pour transférer toutes les demandes de connexion à un groupe de serveurs RADIUS distants sans utiliser de règles de manipulation d’attribut.

Toutefois, si vous avez plusieurs emplacements vers lesquels vous souhaitez transférer les demandes de connexion, vous devez créer une stratégie de demande de connexion pour chaque emplacement, puis configurer la stratégie avec le groupe de serveurs RADIUS distants vers lequel vous souhaitez transférer les messages, ainsi que avec les règles de manipulation d’attribut qui indiquent à NPS les messages à transférer.

Vous pouvez créer des règles pour les attributs suivants.

- Appelé-Station-ID. Numéro de téléphone du serveur d’accès réseau (NAS). La valeur de cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe de mise en correspondance de modèle pour spécifier des indicatifs de zone.

- Call-Station-ID. Numéro de téléphone utilisé par l’appelant. La valeur de cet attribut est une chaîne de caractères. Vous pouvez utiliser la syntaxe de mise en correspondance de modèle pour spécifier des indicatifs de zone.

- Nom d’utilisateur. Nom d’utilisateur fourni par le client d’accès et inclus par le NAS dans le message de demande d’accès RADIUS. La valeur de cet attribut est une chaîne de caractères qui contient généralement un nom de domaine et un nom de compte d’utilisateur.

Pour remplacer ou convertir correctement les noms de domaine dans le nom d’utilisateur d’une demande de connexion, vous devez configurer des règles de manipulation d’attributs pour l’attribut User-Name sur la stratégie de demande de connexion appropriée.

### <a name="key-steps"></a>Étapes clés

Pendant la planification des règles de manipulation d’attribut, vous pouvez utiliser les étapes suivantes.

- Planifiez le routage des messages à partir du NAS via le proxy vers les serveurs RADIUS distants pour vérifier que vous disposez d’un chemin logique avec lequel transférer les messages aux serveurs RADIUS.

- Déterminez un ou plusieurs attributs que vous souhaitez utiliser pour chaque stratégie de demande de connexion.

- Documentez les règles de manipulation d’attribut que vous prévoyez d’utiliser pour chaque stratégie de demande de connexion et associez-les aux règles du groupe de serveurs RADIUS distants vers lequel les messages sont transférés.

## <a name="plan-connection-request-policies"></a>Planifier les stratégies de demande de connexion

La stratégie de demande de connexion par défaut est configurée pour NPS lorsqu’elle est utilisée en tant que serveur RADIUS. Des stratégies de demande de connexion supplémentaires peuvent être utilisées pour définir des conditions plus spécifiques, créer des règles de manipulation d’attribut qui indiquent à NPS les messages à transférer aux groupes de serveurs RADIUS distants et pour spécifier des attributs avancés. Utilisez l’Assistant Nouvelle stratégie de demande de connexion pour créer des stratégies de demande de connexion communes ou personnalisées.

### <a name="key-steps"></a>Étapes clés

Lors de la planification des stratégies de demande de connexion, vous pouvez utiliser les étapes suivantes.

- Supprimez la stratégie de demande de connexion par défaut sur chaque serveur NPS qui fonctionne uniquement comme proxy RADIUS.

- Planifiez les conditions et paramètres supplémentaires requis pour chaque stratégie, en combinant ces informations avec le groupe de serveurs RADIUS distants et les règles de manipulation d’attribut planifiées pour la stratégie.

- Concevez le plan pour distribuer des stratégies de demande de connexion courantes à tous les proxys NPS. Créez des stratégies communes à plusieurs proxys NPS sur un serveur NPS, puis utilisez les commandes netsh pour NPS pour importer les stratégies de demande de connexion et la configuration du serveur sur tous les autres serveurs proxy.

## <a name="plan-nps-accounting"></a>Planifier la gestion des comptes NPS

Quand vous configurez NPS en tant que proxy RADIUS, vous pouvez le configurer pour effectuer la gestion de comptes RADIUS en utilisant les fichiers journaux au format NPS, les fichiers journaux au format compatible avec la base de données ou la journalisation des SQL Server NPS.

Vous pouvez également transférer des messages de gestion de comptes à un groupe de serveurs RADIUS distants qui effectue la gestion des comptes à l’aide de l’un de ces formats de journalisation.

### <a name="key-steps"></a>Étapes clés

Pendant la planification de la gestion des comptes NPS, vous pouvez utiliser les étapes suivantes.

- Déterminez si vous souhaitez que le proxy NPS effectue des services de gestion des comptes ou pour transférer des messages de gestion de comptes à un groupe de serveurs RADIUS distants pour la comptabilité.

- Planifiez la désactivation de la gestion de comptes proxy NPS locale si vous envisagez de transférer des messages de gestion vers d’autres serveurs.

- Planifiez les étapes de configuration de la stratégie de demande de connexion si vous envisagez de transférer des messages de gestion vers d’autres serveurs. Si vous désactivez la gestion de comptes locale pour le proxy NPS, vous devez activer et configurer correctement chaque stratégie de demande de connexion que vous configurez sur ce proxy.

- Déterminez le format de journalisation que vous souhaitez utiliser : les fichiers journaux au format IAS, les fichiers journaux au format compatible avec la base de données ou la journalisation NPS SQL Server.

Pour configurer l’équilibrage de charge pour NPS en tant que proxy RADIUS, consultez [équilibrage de charge du serveur proxy NPS](nps-manage-proxy-lb.md).

