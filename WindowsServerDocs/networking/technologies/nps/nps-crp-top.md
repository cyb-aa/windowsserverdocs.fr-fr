---
title: Traitement des requêtes de connexion
description: Cette rubrique fournit une vue d’ensemble du traitement des demandes de connexion au serveur de stratégie réseau dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 849d661a-42c1-4f93-b669-6009d52aad39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 268a307336d9a4fae1be5621eec7c32a06a6204e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405394"
---
# <a name="connection-request-processing"></a>Traitement des requêtes de connexion

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur le traitement des demandes de connexion dans le serveur NPS (Network Policy Server) dans Windows Server 2016.

>[!NOTE]
>En plus de cette rubrique, la documentation suivante sur le traitement des demandes de connexion est disponible.
> - [Stratégies de demande de connexion](nps-crp-crpolicies.md)
> - [Noms de domaine](nps-crp-realm-names.md)
> - [Groupes de serveurs RADIUS distants](nps-crp-rrsg.md)

Vous pouvez utiliser le traitement des demandes de connexion pour spécifier l’emplacement d’exécution de l’authentification des demandes de connexion, sur l’ordinateur local ou sur un serveur RADIUS distant qui est membre d’un groupe de serveurs RADIUS distants. 

Si vous souhaitez que le serveur local exécutant NPS (Network Policy Server) effectue l’authentification pour les demandes de connexion, vous pouvez utiliser la stratégie de demande de connexion par défaut sans configuration supplémentaire. En fonction de la stratégie par défaut, NPS authentifie les utilisateurs et les ordinateurs qui ont un compte dans le domaine local et dans des domaines approuvés.

Si vous souhaitez transférer des demandes de connexion à un serveur NPS ou un autre serveur RADIUS distant, créez un groupe de serveurs RADIUS distants, puis configurez une stratégie de demande de connexion qui transmet les demandes à ce groupe de serveurs RADIUS distants. Avec cette configuration, NPS peut transférer les demandes d’authentification à n’importe quel serveur RADIUS, et les utilisateurs disposant de comptes dans des domaines non approuvés peuvent être authentifiés.

L’illustration suivante montre le chemin d’accès d’un message de demande d’accès d’un serveur d’accès réseau à un proxy RADIUS, puis à un serveur RADIUS situé dans un groupe de serveurs RADIUS distants. Sur le proxy RADIUS, le serveur d’accès réseau est configuré en tant que client RADIUS. et sur chaque serveur RADIUS, le proxy RADIUS est configuré en tant que client RADIUS.


![Traitement des demandes de connexion NPS](../../media/Nps-Connection-Request-Processing/Nps-Connection-Request-Processing.jpg)


>[!NOTE]
>Les serveurs d’accès réseau que vous utilisez avec NPS peuvent être des périphériques de passerelle conformes au protocole RADIUS, tels que les points d’accès sans fil 802.1 X et les commutateurs d’authentification, les serveurs exécutant l’accès à distance qui sont configurés en tant que serveurs VPN ou d’accès à distance, ou autres périphériques compatibles RADIUS.

Si vous souhaitez que NPS traite certaines demandes d’authentification localement tout en transférant d’autres demandes à un groupe de serveurs RADIUS distants, configurez plusieurs stratégies de demande de connexion.

Pour configurer une stratégie de demande de connexion qui spécifie le serveur NPS ou le groupe de serveurs RADIUS qui traite les demandes d’authentification, consultez stratégies de demande de connexion.

Pour spécifier les serveurs NPS ou d’autres serveurs RADIUS auxquels les demandes d’authentification sont transférées, consultez groupes de serveurs RADIUS distants.

## <a name="nps-as-a-radius-server-connection-request-processing"></a>Traitement de la demande de connexion du serveur RADIUS par NPS

Lorsque vous utilisez NPS en tant que serveur RADIUS, les messages RADIUS fournissent l’authentification, l’autorisation et la gestion des comptes pour les connexions d’accès réseau de la façon suivante :

1. Les serveurs d’accès, tels que les serveurs d’accès réseau à distance, les serveurs VPN et les points d’accès sans fil, reçoivent les demandes de connexion des clients d’accès. 

2. Le serveur d’accès, configuré pour utiliser RADIUS comme protocole d’authentification, d’autorisation et de gestion des comptes, crée un message de demande d’accès et l’envoie au serveur NPS. 

3. Le serveur NPS évalue le message de demande d’accès. 

4. Si nécessaire, le serveur NPS envoie un message de demande d’accès au serveur d’accès. Le serveur d’accès traite le défi et envoie une demande d’accès mise à jour au serveur NPS. 

5. Les informations d’identification de l’utilisateur sont vérifiées et les propriétés d’accès à distance du compte d’utilisateur sont obtenues à l’aide d’une connexion sécurisée à un contrôleur de domaine. 

6. La tentative de connexion est autorisée avec les propriétés de numérotation du compte d’utilisateur et les stratégies réseau. 

7. Si la tentative de connexion est authentifiée et autorisée, le serveur NPS envoie un message d’acceptation d’accès au serveur d’accès. Si la tentative de connexion n’est pas authentifiée ou n’est pas autorisée, le serveur NPS envoie un message de refus d’accès au serveur d’accès. 

8. Le serveur d’accès termine le processus de connexion avec le client d’accès et envoie un message de demande de compte au serveur NPS, où le message est enregistré. 

9. Le serveur NPS envoie un compte-réponse au serveur d’accès. 

>[!NOTE]
>Le serveur d’accès envoie également des messages de demande de comptes au moment où la connexion est établie, lors de la fermeture de la connexion cliente, et lors du démarrage et de l’arrêt du serveur d’accès.

## <a name="nps-as-a-radius-proxy-connection-request-processing"></a>Traitement des demandes de connexion du proxy RADIUS par NPS

Lorsque NPS est utilisé en tant que proxy RADIUS entre un client RADIUS et un serveur RADIUS, les messages RADIUS pour les tentatives de connexion d’accès réseau sont transférés de la façon suivante :

1. Les serveurs d’accès, tels que les serveurs d’accès réseau à distance, les serveurs de réseau privé virtuel (VPN) et les points d’accès sans fil, reçoivent les demandes de connexion des clients d’accès.

2. Le serveur d’accès, configuré pour utiliser RADIUS comme protocole d’authentification, d’autorisation et de gestion des comptes, crée un message de demande d’accès et l’envoie au serveur NPS utilisé comme proxy RADIUS NPS.

3. Le proxy RADIUS NPS reçoit le message de requête d’accès et, en fonction des stratégies de demande de connexion configurées localement, détermine où transférer le message de demande d’accès.

4. Le proxy RADIUS NPS transfère le message de demande d’accès au serveur RADIUS approprié.

5. Le serveur RADIUS évalue le message de demande d’accès.

6. Le cas échéant, le serveur RADIUS envoie un message de demande d’accès au proxy RADIUS NPS, où il est transféré vers le serveur d’accès. Le serveur d’accès traite le problème avec le client d’accès et envoie une demande d’accès mise à jour au proxy RADIUS NPS, où il est transféré vers le serveur RADIUS.

7. Le serveur RADIUS authentifie et autorise la tentative de connexion.

8. Si la tentative de connexion est authentifiée et autorisée, le serveur RADIUS envoie un message d’acceptation d’accès au proxy RADIUS NPS, où il est transféré vers le serveur d’accès. Si la tentative de connexion n’est pas authentifiée ou n’est pas autorisée, le serveur RADIUS envoie un message de refus d’accès au proxy RADIUS NPS, où il est transféré au serveur d’accès.

9. Le serveur d’accès termine le processus de connexion avec le client d’accès et envoie un message de demande de compte au proxy RADIUS NPS. Le proxy RADIUS NPS enregistre les données de gestion des comptes et transfère le message au serveur RADIUS.

10. Le serveur RADIUS envoie une réponse de gestion des comptes au proxy RADIUS NPS, où il est transféré vers le serveur d’accès.

Pour plus d’informations sur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).
