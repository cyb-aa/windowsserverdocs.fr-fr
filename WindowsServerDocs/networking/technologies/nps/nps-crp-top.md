---
title: Traitement des demandes de connexion
description: Cette rubrique fournit une vue d’ensemble de la demande de connexion du serveur de stratégie réseau dans Windows Server2016 de traitement.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 849d661a-42c1-4f93-b669-6009d52aad39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6756c89dadbd01998ffef6a6d785c079076f2532
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="connection-request-processing"></a>Traitement des demandes de connexion

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour en savoir plus sur la demande de connexion de traitement dans le serveur NPS dans Windows Server2016.

>[!NOTE]
>Outre cette rubrique, la demande de connexion suivantes traitement documentation est disponible.
> - [Stratégies de demande de connexion](nps-crp-crpolicies.md)
> - [Noms de domaine](nps-crp-realm-names.md)
> - [Groupes de serveurs RADIUS distants](nps-crp-rrsg.md)

Vous pouvez utiliser le traitement des demandes de connexion pour spécifier où l’authentification des demandes de connexion est effectuée, sur l’ordinateur local ou sur un serveur RADIUS distant qui est membre du groupe de serveurs RADIUS distants. 

Si vous souhaitez que le serveur local exécutant le serveur NPS (Network Policy) pour effectuer l’authentification pour les demandes de connexion, vous pouvez utiliser la stratégie de demande de connexion par défaut sans configuration supplémentaire. En fonction de la stratégie par défaut, NPS authentifie les utilisateurs et les ordinateurs qui ont un compte dans le domaine local et dans les domaines approuvés.

Si vous souhaitez transférer les demandes de connexion à un serveur NPS à distance ou un autre serveur RADIUS, créer un groupe de serveurs RADIUS distants, puis configurez une stratégie de demande de connexion qui transfère les requêtes à ce groupe de serveurs RADIUS distants. Avec cette configuration, NPS peut transférer les demandes d’authentification à un serveur RADIUS et les utilisateurs avec des comptes dans des domaines non approuvés peuvent être authentifiés.

L’illustration suivante montre le chemin d’accès d’un message de demande d’accès à partir d’un serveur d’accès réseau vers un proxy RADIUS, puis une session sur un serveur RADIUS dans un groupe de serveurs RADIUS distants. Sur le serveur proxy RADIUS, le serveur d’accès réseau est configuré comme client RADIUS; et sur chaque serveur RADIUS, le proxy RADIUS est configuré comme client RADIUS.


![Traitement des demandes de connexion du serveur NPS](../../media/Nps-Connection-Request-Processing/Nps-Connection-Request-Processing.jpg)


>[!NOTE]
>Les serveurs d’accès réseau que vous utilisez avec le serveur NPS peuvent être des périphériques de passerelle qui sont compatibles avec le protocole RADIUS, tels que les points de l’accès sans fil 802. 1 X et les commutateurs d’authentification, les serveurs d’accès à distance en cours d’exécution qui sont configurés en tant que VPN ou les serveurs d’accès à distance, ou d’autres appareils compatibles RADIUS.

Si vous souhaitez que NPS pour traiter les demandes d’authentification localement lors du transfert d’autres demandes envoyées à un groupe de serveurs RADIUS distants, configurer plusieurs stratégies de demande de connexion.

Pour configurer une stratégie de demande de connexion qui spécifie quel groupe de serveur NPS ou RADIUS traite les demandes d’authentification, consultez les stratégies de demande de connexion.

Pour spécifier le serveur NPS ou autres serveurs RADIUS de demandes d’authentification sont transférées, voir groupes de serveurs RADIUS distants.

## <a name="nps-as-a-radius-server-connection-request-processing"></a>NPS en tant qu’un traitement de demande de connexion de serveur RADIUS

Lorsque vous utilisez NPS comme serveur RADIUS, les messages RADIUS fournissent l’authentification, l’autorisation et la gestion des comptes pour les connexions d’accès réseau de la manière suivante:

1. Serveurs d’accès, tels que les serveurs d’accès réseau à distance, les serveurs VPN et les points d’accès sans fil, recevoir des demandes de connexion à partir de clients d’accès. 

2. Le serveur d’accès, configuré pour utiliser RADIUS en tant que le protocole d’authentification, d’autorisation et gestion des comptes, crée un message de demande d’accès et l’envoie au serveur NPS. 

3. Le serveur NPS évalue le message de demande d’accès. 

4. Si nécessaire, le serveur NPS envoie un message de demande de l’accès au serveur d’accès. Le serveur d’accès traite la demande et envoie une demande d’accès mis à jour sur le serveur NPS. 

5. Les informations d’identification utilisateur sont vérifiées et les propriétés de numérotation du compte d’utilisateur sont obtenues à l’aide d’une connexion sécurisée à un contrôleur de domaine. 

6. La tentative de connexion est autorisée avec les deux propriétés de numérotation des stratégies réseau et le compte d’utilisateur. 

7. Si la tentative de connexion est authentifiée et autorisée, le serveur NPS envoie un message d’acceptation d’accès au serveur d’accès. Si la tentative de connexion est pas authentifiée ou non autorisée, le serveur NPS envoie un message de refus d’accès au serveur d’accès. 

8. Le serveur d’accès termine le processus de connexion avec le client d’accès et envoie un message de demande de compte sur le serveur NPS, où le message est enregistré. 

9. Le serveur NPS envoie une réponse de compte au serveur d’accès. 

>[!NOTE]
>Le serveur d’accès envoie également des messages de demande de compte au cours de l’heure dans lequel la connexion est établie, lorsque la connexion d’accès client est fermée, lorsque le serveur d’accès est démarré et arrêté.

## <a name="nps-as-a-radius-proxy-connection-request-processing"></a>NPS en tant qu’un traitement de demande de connexion proxy RADIUS

Lorsque le serveur NPS est utilisé en tant que proxy RADIUS entre un client RADIUS et un serveur RADIUS, messages RADIUS pour le réseau d’accès connexion tentatives sont transférés de la manière suivante:

1. Serveurs d’accès, tels que les serveurs d’accès réseau à distance, les serveurs de réseau privé virtuel (VPN) et les points d’accès sans fil, recevoir des demandes de connexion à partir de clients d’accès.

2. Le serveur d’accès, configuré pour utiliser RADIUS en tant que le protocole d’authentification, d’autorisation et gestion des comptes, crée un message de demande d’accès et l’envoie au serveur NPS qui est utilisé en tant que proxy RADIUS NPS.

3. Le proxy RADIUS NPS reçoit le message de demande d’accès et, selon les stratégies de demande de connexion configurées localement, détermine où transférer le message de demande d’accès.

4. Le proxy RADIUS du serveur NPS transfère le message de demande d’accès au serveur RADIUS approprié.

5. Le serveur RADIUS évalue le message de demande d’accès.

6. Si nécessaire, le serveur RADIUS envoie un message de Challenge d’accès au proxy RADIUS du serveur NPS, où il est transféré vers le serveur d’accès. Le serveur d’accès traite le challenge avec le client d’accès et envoie une demande d’accès mis à jour pour le proxy RADIUS du serveur NPS, où il est transféré vers le serveur RADIUS.

7. Le serveur RADIUS authentifie et autorise la tentative de connexion.

8. Si la tentative de connexion est authentifiée et autorisée, le serveur RADIUS envoie un message d’acceptation d’accès au proxy RADIUS du serveur NPS, où il est transféré vers le serveur d’accès. Si la tentative de connexion est pas authentifiée ou non autorisée, le serveur RADIUS envoie un message de refus d’accès au proxy RADIUS du serveur NPS, où il est transféré vers le serveur d’accès.

9. Le serveur d’accès termine le processus de connexion avec le client d’accès et envoie un message de demande de compte pour le proxy RADIUS du serveur NPS. Le proxy RADIUS NPS enregistre les données de gestion et transfère le message vers le serveur RADIUS.

10. Le serveur RADIUS envoie une réponse de compte pour le proxy RADIUS du serveur NPS, où il est transféré vers le serveur d’accès.

Pour plus d’informations sur le serveur NPS, voir [serveur NPS (Network Policy)](nps-top.md).
