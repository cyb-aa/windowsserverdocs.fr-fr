---
title: Traitement des requêtes de connexion
description: Cette rubrique fournit une vue d’ensemble de demande de connexion du serveur de stratégie réseau de traitement dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 849d661a-42c1-4f93-b669-6009d52aad39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6ab08dced15cfadd5a0fc773efc2ddaa2da24c08
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829110"
---
# <a name="connection-request-processing"></a>Traitement des requêtes de connexion

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur la demande de connexion de traitement dans le serveur NPS dans Windows Server 2016.

>[!NOTE]
>Outre cette rubrique, la demande de connexion suivantes traitement documentation est disponible.
> - [Stratégies de demande de connexion](nps-crp-crpolicies.md)
> - [Noms de domaine](nps-crp-realm-names.md)
> - [Groupes de serveurs RADIUS distants](nps-crp-rrsg.md)

Vous pouvez utiliser le traitement des demandes de connexion pour spécifier où l’authentification des demandes de connexion est effectuée - sur l’ordinateur local ou sur un serveur RADIUS distant qui est membre du groupe de serveurs RADIUS distants. 

Si vous souhaitez que le serveur local exécutant le serveur NPS (Network Policy) pour effectuer l’authentification des demandes de connexion, vous pouvez utiliser la stratégie de demande de connexion par défaut sans configuration supplémentaire. En fonction de la stratégie par défaut, NPS authentifie les utilisateurs et les ordinateurs disposant d’un compte dans le domaine local et dans des domaines approuvés.

Si vous souhaitez transférer les demandes de connexion à un serveur NPS à distance ou un autre serveur RADIUS, créer un groupe de serveurs RADIUS distants, puis configurez une stratégie de demande de connexion qui transfère les requêtes à ce groupe de serveurs RADIUS distants. Avec cette configuration, NPS peut transférer les demandes d’authentification à n’importe quel serveur RADIUS et les utilisateurs disposant de comptes dans des domaines non approuvés peuvent être authentifiés.

L’illustration suivante montre le chemin d’accès d’un message de demande d’accès à partir d’un serveur d’accès réseau à un proxy RADIUS, puis à un serveur RADIUS dans un groupe de serveurs RADIUS distants. Sur le serveur proxy RADIUS, le serveur d’accès réseau est configuré comme client RADIUS ; et sur chaque serveur RADIUS, le proxy RADIUS est configuré comme client RADIUS.


![Traitement des demandes de connexion de serveur NPS](../../media/Nps-Connection-Request-Processing/Nps-Connection-Request-Processing.jpg)


>[!NOTE]
>Les serveurs d’accès réseau que vous utilisez avec le serveur NPS peuvent être des périphériques de passerelle qui sont compatibles avec le protocole RADIUS, tels que 802. 1 X des points d’accès sans fil et les commutateurs d’authentification, les serveurs exécutant l’accès à distance qui sont configurés en tant que VPN ou des serveurs d’accès à distance, ou autres périphériques compatibles RADIUS.

Si vous souhaitez que le serveur NPS pour traiter certaines demandes d’authentification localement lors du transfert d’autres demandes envoyées à un groupe de serveurs RADIUS distants, configurez plusieurs stratégies de demande de connexion.

Pour configurer une stratégie de demande de connexion qui spécifie quel groupe de serveur NPS ou RADIUS traite les demandes d’authentification, consultez les stratégies de demande de connexion.

Pour spécifier le serveur NPS ou autres serveurs RADIUS pour l’authentification des demandes sont transférées, consultez les groupes de serveurs RADIUS distants.

## <a name="nps-as-a-radius-server-connection-request-processing"></a>NPS en tant qu’un traitement de demande de connexion de serveur RADIUS

Lorsque vous utilisez NPS comme serveur RADIUS, les messages RADIUS fournissent l’authentification, autorisation et comptabilité pour les connexions d’accès réseau de la façon suivante :

1. Serveurs d’accès, telles que les serveurs d’accès réseau à distance, les serveurs VPN et les points d’accès sans fil, recevront des demandes de connexion à partir de clients d’accès. 

2. Le serveur d’accès, configuré pour utiliser RADIUS en tant que le protocole d’authentification, autorisation et comptabilité, crée un message de demande d’accès et l’envoie au serveur NPS. 

3. Le serveur NPS évalue le message de demande d’accès. 

4. Si nécessaire, le serveur NPS envoie un message de Challenge d’accès au serveur d’accès. Le serveur d’accès traite le challenge et envoie une demande d’accès mis à jour au serveur NPS. 

5. Les informations d’identification utilisateur sont vérifiées et les propriétés d’accès à distance du compte d’utilisateur sont obtenues à l’aide d’une connexion sécurisée à un contrôleur de domaine. 

6. La tentative de connexion est autorisée avec à la fois les propriétés de numérotation des stratégies de réseau et de compte d’utilisateur. 

7. Si la tentative de connexion est à la fois authentifiée et autorisée, le serveur NPS envoie un message d’acceptation d’accès au serveur d’accès. Si la tentative de connexion est pas authentifiée ou non autorisée, le serveur NPS envoie un message de refus d’accès au serveur d’accès. 

8. Le serveur d’accès termine le processus de connexion avec le client d’accès et envoie un message de demande de compte au serveur NPS, où le message est enregistré. 

9. Le serveur NPS envoie un message de réponse de compte au serveur d’accès. 

>[!NOTE]
>Le serveur d’accès envoie également des messages de demande de compte pendant le temps dans laquelle la connexion est établie lorsque la connexion d’accès client est fermée et lorsque le serveur d’accès est démarré et arrêté.

## <a name="nps-as-a-radius-proxy-connection-request-processing"></a>NPS en tant qu’un traitement de demande de connexion de proxy RADIUS

Lorsque NPS est utilisé en tant que proxy RADIUS entre un client RADIUS et un serveur RADIUS, les messages RADIUS pour le réseau d’accès connexion tentatives sont transférés de la façon suivante :

1. Serveurs d’accès, telles que les serveurs d’accès réseau à distance, les serveurs de réseau privé virtuel (VPN) et les points d’accès sans fil, recevront des demandes de connexion à partir de clients d’accès.

2. Le serveur d’accès, configuré pour utiliser RADIUS en tant que le protocole d’authentification, autorisation et comptabilité, crée un message de demande d’accès et l’envoie au serveur NPS qui est utilisé comme proxy RADIUS du serveur NPS.

3. Le proxy RADIUS NPS reçoit le message de demande d’accès et, selon les stratégies de demande de connexion configurées localement, détermine où transférer le message de demande d’accès.

4. Le proxy RADIUS NPS transfère le message de demande d’accès au serveur RADIUS approprié.

5. Le serveur RADIUS évalue le message de demande d’accès.

6. Si nécessaire, le serveur RADIUS envoie un message de Challenge d’accès au proxy RADIUS du serveur NPS, où il est transféré vers le serveur d’accès. Le serveur d’accès traite le challenge avec le client d’accès et envoie une demande d’accès mis à jour vers le proxy RADIUS du serveur NPS, où il est transféré vers le serveur RADIUS.

7. Le serveur RADIUS authentifie et autorise la tentative de connexion.

8. Si la tentative de connexion est à la fois authentifiée et autorisée, le serveur RADIUS envoie un message d’acceptation d’accès au proxy RADIUS du serveur NPS, où il est transféré vers le serveur d’accès. Si la tentative de connexion est pas authentifiée ou non autorisée, le serveur RADIUS envoie un message de refus d’accès au proxy RADIUS du serveur NPS, où il est transféré vers le serveur d’accès.

9. Le serveur d’accès termine le processus de connexion avec le client d’accès et envoie un message de demande de compte au proxy RADIUS du serveur NPS. Le proxy RADIUS NPS enregistre les données de comptabilité et transfère le message au serveur RADIUS.

10. Le serveur RADIUS envoie un message de réponse de compte au proxy RADIUS du serveur NPS, où il est transféré vers le serveur d’accès.

Pour plus d’informations sur le serveur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).
