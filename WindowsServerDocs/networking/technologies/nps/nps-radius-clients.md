---
title: Clients RADIUS
description: Cette rubrique fournit une vue d’ensemble de Clients RADIUS pour le serveur NPS dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d3a09ac9-75f8-4f57-aab4-b0fdfe110118
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fdca45237d971c1b2e08443112b63d3ce77e4a2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874310"
---
# <a name="radius-clients"></a>Clients RADIUS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Un serveur d’accès réseau \(NAS\) est un périphérique qui fournit un certain niveau d’accès à un réseau plus large. Un NAS à l’aide d’une infrastructure RADIUS est également un client RADIUS, envoie des demandes de connexion et des messages de comptabilité à un serveur RADIUS pour l’authentification, autorisation et la comptabilité.

>[!NOTE]
>Les ordinateurs clients, tels que les ordinateurs portables et les autres ordinateurs qui exécutent les systèmes d’exploitation clients, ne sont pas des clients RADIUS. Les clients RADIUS sont des serveurs d’accès réseau - tels que les points d’accès sans fil, les commutateurs de l’authentification 802. 1 X, réseau privé virtuel \(VPN\) serveurs et les serveurs d’accès à distance - car ils utilisent le protocole RADIUS pour communiquer avec RADIUS serveurs, tels que serveur de stratégie réseau \(NPS\) serveurs.

Pour déployer NPS en tant que serveur RADIUS ou proxy RADIUS, vous devez configurer les clients RADIUS dans NPS.

## <a name="radius-client-examples"></a>Exemples de clients RADIUS

Sont des exemples de serveurs d’accès réseau :

- Serveurs d’accès réseau qui assurent la connectivité d’accès à distance à un réseau d’entreprise ou à Internet. Un exemple est un ordinateur exécutant le système d’exploitation Windows Server 2016 et le service d’accès à distance qui fournit des soit traditionnel accès à distance ou les services d’accès à distance de réseau privé virtuel (VPN) à un intranet d’entreprise.
- Points d’accès sans fil qui fournissent un accès de couche physique à un réseau d’entreprise à l’aide des technologies de transmission et de réception sans fil.
- Commutateurs qui fournissent un accès de couche physique à un réseau d’entreprise, à l’aide des technologies LAN traditionnelles, telles que Ethernet.
- Les proxys RADIUS qui transfèrent les demandes de connexion pour les serveurs RADIUS qui sont membres du groupe de serveurs RADIUS distants qui est configuré sur le proxy RADIUS.

## <a name="radius-access-request-messages"></a>Messages de demande d’accès RADIUS

Clients RADIUS créent les messages de demande d’accès RADIUS et les transfèrent vers un proxy RADIUS ou d’un serveur RADIUS, ou ils transfèrent les messages de demande d’accès à un serveur RADIUS qui ils ont reçu à partir d’un autre client RADIUS mais que vous n’ont pas créé eux-mêmes.

Clients RADIUS ne traitent pas les messages de demande d’accès par authentification, autorisation et la comptabilité. Seuls les serveurs RADIUS exécutent ces fonctions.

NPS, toutefois, peuvent être configurées en tant que proxy RADIUS et un serveur RADIUS simultanément, afin qu’il traite certains messages de demande d’accès et transfère les autres messages.

## <a name="nps-as-a-radius-client"></a>NPS en tant que client RADIUS

NPS se comporte comme un client RADIUS lorsque vous le configurez en tant que proxy RADIUS pour transférer les messages de demande d’accès à d’autres serveurs RADIUS pour le traitement. Lorsque vous utilisez NPS en tant que proxy RADIUS, les étapes générales suivantes sont nécessaires :

1. Serveurs d’accès réseau, tels que les points d’accès sans fil et les serveurs VPN, sont configurés avec l’adresse IP du proxy NPS en tant que le serveur RADIUS désigné ou un serveur d’authentification. Ainsi, les serveurs d’accès réseau, qui créent des messages de la demande d’accès en fonction des informations qu’ils reçoivent à partir de clients d’accès à transférer les messages vers le proxy de serveur NPS.

2. Le proxy de serveur NPS est configuré en ajoutant chaque serveur d’accès réseau comme un client RADIUS. Cette étape de configuration permet au proxy NPS pour recevoir des messages des serveurs d’accès réseau et de communiquer avec eux tout au long de l’authentification. En outre, les stratégies de demande de connexion sur le proxy NPS sont configurées pour spécifier les messages de demande d’accès à transférer à un ou plusieurs serveurs RADIUS. Ces stratégies sont également configurés avec un groupe de serveurs RADIUS distant, ce qui indique à NPS où envoyer les messages qu’il reçoit des serveurs d’accès réseau.

3. Le serveur NPS ou autres serveurs RADIUS qui sont membres du groupe de serveurs RADIUS distants sur le proxy NPS sont configurés pour recevoir des messages à partir du proxy de serveur NPS. Cela s’effectue en configurant le proxy NPS en tant que client RADIUS.

## <a name="radius-client-properties"></a>Propriétés du client RADIUS

Lorsque vous ajoutez un client RADIUS à la configuration NPS via la console NPS, ou en utilisant les commandes netsh pour NPS ou les commandes Windows PowerShell, vous configurez le serveur NPS pour recevoir des messages de demande d’accès RADIUS à partir d’un serveur d’accès de réseau ou un Proxy RADIUS.

Lorsque vous configurez un client RADIUS dans NPS, vous pouvez désigner les propriétés suivantes :

### <a name="client-name"></a>Nom du client

 Un nom convivial pour le client RADIUS, ce qui le rend plus facile à identifier lors de l’utilisation du serveur NPS enfichable ou les commandes netsh pour NPS.

### <a name="ip-address"></a>Adresse IP

Protocole Internet version 4 \(IPv4\) adresse ou le système de nom de domaine \(DNS\) nom du client RADIUS.

### <a name="client-vendor"></a>Client-fournisseur

Le fournisseur du client RADIUS. Sinon, vous pouvez utiliser la valeur RADIUS standard pour fournisseur de Client.

### <a name="shared-secret"></a>Secret partagé

Une chaîne de texte qui est utilisée comme un mot de passe entre les clients RADIUS, les serveurs RADIUS et les proxys RADIUS. Lorsque l’attribut de l’authentificateur de Message est utilisé, le secret partagé est également utilisé comme clé pour chiffrer les messages RADIUS. Cette chaîne doit être configurée sur le client RADIUS et dans le composant logiciel enfichable NPS.

### <a name="message-authenticator-attribute"></a>Attribut de l’authentificateur de message

Décrit dans RFC 2869, « RADIUS Extensions », un Message Digest 5 \(MD5\) hachage de l’intégralité du message RADIUS. Si l’attribut de l’authentificateur de Message RADIUS est présent, il est vérifié. Si la vérification échoue, le message RADIUS est ignoré. Si les paramètres du client exigent l’attribut de l’authentificateur de Message et il n’est pas présent, le message RADIUS est ignoré. Utilisation de l’attribut de l’authentificateur de Message est recommandée.

>[!NOTE]
>L’attribut de l’authentificateur de Message est requis et activé par défaut lorsque vous utilisez le protocole d’authentification Extensible \(EAP\) l’authentification. 

Pour plus d’informations sur le serveur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).

