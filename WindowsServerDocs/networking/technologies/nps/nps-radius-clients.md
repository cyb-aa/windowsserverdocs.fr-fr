---
title: Clients RADIUS
description: Cette rubrique fournit une vue d’ensemble de Clients RADIUS pour le serveur NPS dans Windows Server2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d3a09ac9-75f8-4f57-aab4-b0fdfe110118
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a970dabcc6f3f4fc4d8ed5a0dedd01b5a7dee71a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="radius-clients"></a>Clients RADIUS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Un serveur d’accès réseau \(NAS\) est un périphérique qui fournit un certain niveau d’accès à un réseau plus large. Un NAS à l’aide d’une infrastructure RADIUS est également un client RADIUS, envoie des demandes de connexion et des messages de gestion des comptes à un serveur RADIUS pour l’authentification, d’autorisation et la gestion de comptes.

>[!NOTE]
>Les ordinateurs clients, tels que les ordinateurs portables et les autres ordinateurs qui exécutent les systèmes d’exploitation clients, ne sont pas des clients RADIUS. Clients RADIUS sont des serveurs d’accès réseau - tels que les points d’accès sans fil, les commutateurs d’authentification 802. 1 X, les serveurs \(VPN\) de réseau privé virtuel et les serveurs d’accès à distance - car ils utilisent le protocole RADIUS pour communiquer avec les serveurs RADIUS tels que serveur de stratégie réseau \(NPS\).

Pour déployer NPS en tant que serveur RADIUS ou un proxy RADIUS, vous devez configurer les clients RADIUS dans NPS.

## <a name="radius-client-examples"></a>Exemples de clients RADIUS

Sont des exemples de serveurs d’accès réseau:

- Serveurs d’accès réseau qui assurent la connectivité d’accès à distance à un réseau d’entreprise ou à Internet. Un ordinateur exécutant le système d’exploitation Windows Server2016 et le service d’accès à distance qui fournit des soit traditionnel accès à distance ou les services d’accès à distance de réseau privé virtuel (VPN) à un intranet d’entreprise est un exemple.
- Points d’accès sans fil qui fournissent un accès de couche physique à un réseau d’entreprise à l’aide de technologies de transmission et de réception sans fil.
- Commutateurs qui fournissent l’accès à la couche physique au réseau d’une organisation, à l’aide de technologies LAN traditionnelles, telles que Ethernet.
- Proxy RADIUS qui transfère les demandes de connexion à des serveurs RADIUS qui sont membres du groupe de serveurs RADIUS distants qui est configuré sur le proxy RADIUS.

## <a name="radius-access-request-messages"></a>Messages de demande d’accès RADIUS

Les clients RADIUS soit créent des messages de demande d’accès RADIUS et les transfèrent vers un proxy RADIUS ou d’un serveur RADIUS, soit ils transfèrent les messages de demande d’accès à un serveur RADIUS qui ils ont reçu à partir d’un autre client RADIUS mais que vous n’ont pas créé eux-mêmes.

Les clients RADIUS ne pas traitent les messages de demande d’accès en effectuant l’authentification, l’autorisation et de gestion des comptes. Seuls les serveurs RADIUS exécutent ces fonctions.

NPS, toutefois, peuvent être configurées en tant que proxy RADIUS et un serveur RADIUS simultanément, afin qu’il traite certains messages de demande d’accès et transfère les autres messages.

## <a name="nps-as-a-radius-client"></a>NPS comme client RADIUS

NPS se comporte comme un client RADIUS lorsque vous le configurez en tant que proxy RADIUS pour transférer des messages de demande d’accès à d’autres serveurs RADIUS pour le traitement. Lorsque vous utilisez NPS en tant que proxy RADIUS, les étapes de configuration générales suivantes sont requises:

1. Serveurs d’accès réseau, tels que les points d’accès sans fil et les serveurs VPN, sont configurés avec l’adresse IP du proxy NPS en tant que le serveur RADIUS désigné ou un serveur d’authentification. Ainsi, les serveurs d’accès réseau, qui créent des messages de demande d’accès en fonction des informations que provenant de clients d’accès, pour transférer des messages au proxy NPS.

2. Le proxy NPS est configuré en ajoutant chaque serveur d’accès réseau comme un client RADIUS. Cette étape de configuration permet au proxy NPS pour recevoir des messages des serveurs d’accès réseau et de communiquer avec eux tout au long de l’authentification. En outre, les stratégies de demande de connexion sur le proxy NPS sont configurés pour spécifier que les messages de demande d’accès à un ou plusieurs serveurs RADIUS. Ces stratégies sont également configurés avec un groupe de serveurs RADIUS distants, qui indique à NPS où envoyer les messages qu’il reçoit des serveurs d’accès réseau.

3. Le serveur NPS ou autres serveurs RADIUS qui sont membres du groupe de serveurs RADIUS distants sur le proxy NPS sont configurés pour recevoir des messages à partir du proxy NPS. Cette opération est effectuée en configurant le proxy NPS comme client RADIUS.

## <a name="radius-client-properties"></a>Propriétés du client RADIUS

Lorsque vous ajoutez un client RADIUS à la configuration NPS par le biais de la console NPS, ou via les commandes netsh pour NPS ou les commandes Windows PowerShell, vous configurez NPS pour recevoir des messages de demande d’accès RADIUS à partir d’un serveur d’accès réseau ou un proxy RADIUS.

Lorsque vous configurez un client RADIUS dans NPS, vous pouvez spécifier les propriétés suivantes:

### <a name="client-name"></a>Nom du client

 Un nom convivial pour le client RADIUS, ce qui rend plus facile d’identifier quand vous utilisez les serveur NPS enfichable ou les commandes netsh pour NPS.

### <a name="ip-address"></a>Adresse IP

L’adresse de \(IPv4\) Internet Protocol version4 ou le nom du système de nom de domaine \(DNS\) du client RADIUS.

### <a name="client-vendor"></a>Fournisseur du client

Le fournisseur du client RADIUS. Dans le cas contraire, vous pouvez utiliser la valeur RADIUS standard pour fournisseur de Client.

### <a name="shared-secret"></a>Secret partagé

Une chaîne de texte qui est utilisée comme un mot de passe entre les clients RADIUS, les serveurs RADIUS et proxy RADIUS. Lors de l’attribut de l’authentificateur de Message est utilisé, le secret partagé est également utilisé comme clé pour chiffrer les messages RADIUS. Cette chaîne doit être configurée sur le client RADIUS et dans le composant logiciel enfichable serveur NPS.

### <a name="message-authenticator-attribute"></a>Attribut de l’authentificateur de message

Décrit dans la RFC2869, «RADIUS Extensions», un hachage de Message Digest 5 \(MD5\) de l’intégralité du message RADIUS. Si l’attribut de l’authentificateur de Message RADIUS est présent, il est vérifié. Si la vérification échoue, le message RADIUS est ignoré. Si les paramètres du client exigent l’attribut de l’authentificateur de Message et il n’est pas présent, le message RADIUS est ignoré. Utilisation de l’attribut de l’authentificateur de Message est recommandée.

>[!NOTE]
>L’attribut de l’authentificateur de Message est requis et activée par défaut lorsque vous utilisez l’authentification Extensible Authentication Protocol \(EAP\). 

Pour plus d’informations sur le serveur NPS, voir [serveur NPS (Network Policy)](nps-top.md).

