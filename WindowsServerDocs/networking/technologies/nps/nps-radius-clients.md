---
title: Clients RADIUS
description: Cette rubrique fournit une vue d’ensemble des clients RADIUS pour le serveur NPS (Network Policy Server) dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d3a09ac9-75f8-4f57-aab4-b0fdfe110118
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1dfc1bb71d2800a8a9587e54147950dfd7fb371f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395997"
---
# <a name="radius-clients"></a>Clients RADIUS

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Un serveur d’accès réseau \(NAS @ no__t-1 est un appareil qui fournit un certain niveau d’accès à un réseau plus large. Un NAS utilisant une infrastructure RADIUS est également un client RADIUS, qui envoie des demandes de connexion et des messages de gestion de comptes à un serveur RADIUS pour l’authentification, l’autorisation et la gestion des comptes.

>[!NOTE]
>Les ordinateurs clients, tels que les ordinateurs portables et les autres ordinateurs exécutant des systèmes d’exploitation clients, ne sont pas des clients RADIUS. Les clients RADIUS sont des serveurs d’accès réseau, tels que les points d’accès sans fil, les commutateurs d’authentification 802.1 X, les serveurs de réseau privé virtuel \(VPN @ no__t-1 et les serveurs d’accès à distance, car ils utilisent le protocole RADIUS pour communiquer avec les serveurs RADIUS. As Network Policy Server \(NPS @ no__t-3 Servers.

Pour déployer NPS en tant que serveur RADIUS ou proxy RADIUS, vous devez configurer les clients RADIUS dans NPS.

## <a name="radius-client-examples"></a>Exemples de clients RADIUS

Voici quelques exemples de serveurs d’accès réseau :

- Serveurs d’accès réseau qui fournissent une connectivité d’accès à distance à un réseau d’entreprise ou à Internet. Par exemple, un ordinateur exécutant le système d’exploitation Windows Server 2016 et le service d’accès à distance, qui fournit des services d’accès à distance traditionnels ou de réseau privé virtuel (VPN) à un intranet d’entreprise.
- Des points d’accès sans fil qui fournissent un accès à la couche physique à un réseau d’entreprise à l’aide de technologies de transmission et de réception sans fil.
- Commutateurs qui fournissent un accès de couche physique au réseau d’une organisation, à l’aide de technologies LAN traditionnelles, telles que Ethernet.
- Proxys RADIUS qui transfèrent les demandes de connexion aux serveurs RADIUS qui sont membres d’un groupe de serveurs RADIUS distants configuré sur le proxy RADIUS.

## <a name="radius-access-request-messages"></a>Accès RADIUS-messages de demande

Les clients RADIUS créent des messages de demande d’accès RADIUS et les transfèrent à un proxy RADIUS ou à un serveur RADIUS, ou ils transfèrent les messages de demande d’accès à un serveur RADIUS qu’ils ont reçus d’un autre client RADIUS mais n’ont pas été créés eux-mêmes.

Les clients RADIUS ne traitent pas les messages de demande d’accès en effectuant l’authentification, l’autorisation et la gestion des comptes. Seuls les serveurs RADIUS effectuent ces fonctions.

Toutefois, le serveur NPS peut être configuré à la fois en tant que proxy RADIUS et serveur RADIUS simultanément, afin de traiter certains messages de demande d’accès et de transférer d’autres messages.

## <a name="nps-as-a-radius-client"></a>NPS en tant que client RADIUS

NPS agit comme un client RADIUS lorsque vous le configurez en tant que proxy RADIUS pour transférer des messages de demande d’accès à d’autres serveurs RADIUS à des fins de traitement. Lorsque vous utilisez NPS en tant que proxy RADIUS, les étapes de configuration générales suivantes sont requises :

1. Les serveurs d’accès réseau, tels que les points d’accès sans fil et les serveurs VPN, sont configurés avec l’adresse IP du proxy NPS en tant que serveur RADIUS ou serveur d’authentification désigné. Cela permet aux serveurs d’accès réseau, qui créent des messages de demande d’accès en fonction des informations qu’ils reçoivent des clients d’accès, de transférer les messages au proxy NPS.

2. Le proxy NPS est configuré en ajoutant chaque serveur d’accès réseau en tant que client RADIUS. Cette étape de configuration permet au proxy NPS de recevoir des messages des serveurs d’accès réseau et de communiquer avec eux via l’authentification. En outre, les stratégies de demande de connexion sur le proxy NPS sont configurées pour spécifier les messages de demande d’accès à transférer à un ou plusieurs serveurs RADIUS. Ces stratégies sont également configurées avec un groupe de serveurs RADIUS distants, qui indique à NPS où envoyer les messages qu’il reçoit des serveurs d’accès réseau.

3. Le serveur NPS ou d’autres serveurs RADIUS membres du groupe de serveurs RADIUS distants sur le proxy NPS sont configurés pour recevoir des messages du proxy NPS. Pour ce faire, vous devez configurer le proxy NPS en tant que client RADIUS.

## <a name="radius-client-properties"></a>Propriétés du client RADIUS

Lorsque vous ajoutez un client RADIUS à la configuration du serveur NPS via la console NPS ou à l’aide des commandes netsh pour les commandes NPS ou Windows PowerShell, vous configurez NPS pour recevoir des messages de demande d’accès RADIUS à partir d’un serveur d’accès réseau ou d’un Proxy RADIUS.

Quand vous configurez un client RADIUS dans NPS, vous pouvez désigner les propriétés suivantes :

### <a name="client-name"></a>Nom du client

 Nom convivial du client RADIUS, qui facilite l’identification de l’utilisation du composant logiciel enfichable NPS ou des commandes netsh pour NPS.

### <a name="ip-address"></a>Adresse IP

L’adresse IP (Internet Protocol version 4) \(IPv4 @ no__t-1 ou le nom de domaine System \(DNS @ no__t-3 nom du client RADIUS.

### <a name="client-vendor"></a>Client-fournisseur

Fournisseur du client RADIUS. Dans le cas contraire, vous pouvez utiliser la valeur RADIUS standard pour client-Vendor.

### <a name="shared-secret"></a>Secret partagé

Chaîne de texte utilisée comme mot de passe entre les clients RADIUS, les serveurs RADIUS et les proxys RADIUS. Lorsque l’attribut de l’authentificateur de message est utilisé, le secret partagé est également utilisé comme clé pour chiffrer les messages RADIUS. Cette chaîne doit être configurée sur le client RADIUS et dans le composant logiciel enfichable NPS.

### <a name="message-authenticator-attribute"></a>Attribut de l’authentificateur de message

Décrit dans le document RFC 2869, « extensions RADIUS », un message Digest 5 \(MD5 @ no__t-1 hash de l’ensemble du message RADIUS. Si l’attribut de l’authentificateur de message RADIUS est présent, il est vérifié. En cas d’échec de la vérification, le message RADIUS est ignoré. Si les paramètres du client requièrent l’attribut de l’authentificateur du message et qu’il n’est pas présent, le message RADIUS est ignoré. L’utilisation de l’attribut de l’authentificateur de message est recommandée.

>[!NOTE]
>L’attribut de l’authentificateur de message est obligatoire et activé par défaut lorsque vous utilisez l’authentification \(EAP @ no__t-1. 

Pour plus d’informations sur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).

