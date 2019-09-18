---
title: Gérer les licences de votre déploiement Services Bureau à distance avec des licences d’accès client (CAL)
description: Vue d’ensemble de la gestion des licences dans Services Bureau à distance.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5be6546b-df16-4475-bcba-aa75aabef3e3
author: lizap
ms.author: elizapo
ms.date: 09/20/2018
manager: dongill
ms.openlocfilehash: 7783fd19c3dedde81514512f0c81f54eada33dc4
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871059"
---
# <a name="license-your-rds-deployment-with-client-access-licenses-cals"></a>Gérer les licences de votre déploiement Services Bureau à distance avec des licences d’accès client (CAL)

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Chaque utilisateur et appareil qui se connectent à un hôte de session Bureau à distance ont besoin de licences d’accès client (CAL). Vous utilisez le Gestionnaire de licences Bureau à distance pour installer, émettre et effectuer le suivi des licences d’accès client Services Bureau à distance.  

Lorsqu’un utilisateur ou un appareil se connecte à un serveur d’hôte de session Bureau à distance, ce dernier détermine si une licence d’accès client Services Bureau à distance est nécessaire. Le serveur d’hôte de session Bureau à distance demande ensuite une licence d’accès client au serveur de licences Bureau à distance. Si une licence d’accès client Services Bureau à distance adéquate est disponible sur un serveur de licences, elle est émise sur le client et celui-ci peut alors se connecter au serveur d’hôte de session Bureau à distance, et d’ici accéder au bureau ou aux applications qu’il tente d’utiliser.

Bien qu’aucun serveur de licences ne soit requis pendant la période de grâce du Gestionnaire de licences des services Bureau à distance, à l’expiration de cette période, les clients devront avoir une licence d’accès client Services Bureau à distance valide d’un serveur de licences pour pouvoir se connecter à un serveur d’hôte de session Bureau à distance.

Utilisez les informations suivantes pour en savoir plus sur le fonctionnement de la gestion des licences d’accès client dans Services Bureau à distance et sur le déploiement et la gestion de vos licences :

- [Gérer les licences de votre déploiement Services Bureau à distance avec des licences d’accès client (CAL)](#license-your-rds-deployment-with-client-access-licenses-cals)
  - [Comprendre le modèle des licences d’accès client](#understanding-the-cals-model)
  - [Remarque sur les versions de licence d’accès client](#note-about-cal-versions)

## <a name="understanding-the-cals-model"></a>Comprendre le modèle des licences d’accès client

Il existe deux types de licences d’accès client :

- Licences d’accès client Services Bureau à distance par appareil
- Licences d’accès client Services Bureau à distance par utilisateur

Le tableau suivant décrit les différences entre les deux types de licences d’accès client :

| Par périphérique                                                     | Par utilisateur                                                                         |
|----------------------------------------------------------------|----------------------------------------------------------------------------------|
| Les licences d’accès client sont physiquement affectées à chaque appareil.                   | Les licences d’accès client sont affectées à un utilisateur dans Active Directory.                                 |
| Les licences d’accès client sont suivies par le serveur de licences.                        | Les licences d’accès client sont suivies par le serveur de licences.                                          |
| Les licences d’accès client peuvent être suivies quelle que soit l’appartenance à Active Directory. | Les licences d’accès client ne peuvent pas être suivies dans un groupe de travail.                                       |
| Vous pouvez révoquer jusqu’à 20 % des licences d’accès client.                              | Vous ne pouvez pas révoquer de licences d’accès client.                                                      |
| Les licences d’accès client temporaires sont valides pendant 52 à 89 jours.                       | Les licences d’accès client ne sont pas disponibles.                                                |
| Les licences d’accès client ne peuvent pas être surutilisées.                                  | Les licences d’accès client peuvent être surutilisées (en violation de l’accord du Gestionnaire de licences des services Bureau à distance). |

Lorsque vous utilisez le modèle par appareil, une licence temporaire est émise la première fois qu’un appareil se connecte à l’hôte de session Bureau à distance. La deuxième fois que l’appareil se connecte, tant que le serveur de licences est activé et qu’il existe des licences d’accès client disponibles, le serveur de licences émet une licence permanente d’accès client par appareil Services Bureau à distance.

Lorsque vous utilisez le modèle par utilisateur, le Gestionnaire de licences n’est pas activé, et chaque utilisateur se voit accorder une licence pour se connecter à un hôte de session Bureau à distance à partir d’un nombre illimité d’appareils. Le serveur de licences émet des licences à partir du pool de licences d’accès client disponibles ou du pool de licences d’accès client surutilisées. Il est de votre responsabilité de vous assurer que tous les utilisateurs ont une licence valide et aucune licence d’accès client surutilisée, faute de quoi, vous êtes en violation des termes du contrat de licence Services Bureau à distance.

Pour assurer la conformité aux termes du contrat de licence Services Bureau à distance, suivez le nombre de licences par utilisateur Services Bureau à distance utilisées dans votre organisation et assurez-vous d’en avoir installé suffisamment sur le serveur de licences pour tous vos utilisateurs.

Vous pouvez utiliser le Gestionnaire de licences Bureau à distance pour effectuer le suivi et générer des rapports sur les licences d’accès client par utilisateur Services Bureau à distance.

## <a name="note-about-cal-versions"></a>Remarque sur les versions de licence d’accès client

La licence d’accès client utilisée par les utilisateurs ou appareils doit correspondre à la version de Windows Server à laquelle se connecte l’utilisateur ou l’appareil. Vous ne pouvez pas utiliser les anciennes licences d’accès client pour accéder aux versions plus récentes de Windows Server, mais vous pouvez utiliser des licences d’accès client plus récentes pour accéder aux versions antérieures de Windows Server.

Le tableau suivant présente les licences d’accès client compatibles sur les hôtes de session Bureau à distance et les hôtes de virtualisation Bureau à distance.

|                  |Licence d’accès client 2008 R2 et versions antérieures|Licence d’accès client 2012|Licence d’accès client 2016|Licence d’accès client 2019|
|---------------------------------|--------|--------|--------|--------|
| **Serveur de licences 2008, 2008 R2**| Oui    | Non     | Non     | Non     |
| **Serveur de licences 2012**         | Oui    | Oui    | Non     | Non     |
| **Serveur de licences 2012 R2**      | Oui    | Oui    | Non     | Non     |
| **Serveur de licences 2016**         | Oui    | Oui    | Oui    | Non     |
| **Serveur de licences 2019**         | Oui    | Oui    | Oui    | Oui    |

Tous les serveurs de licences Services Bureau à distance peuvent héberger des licences à partir de toutes les versions précédentes de Services Bureau à distance et de la version actuelle de Services Bureau à distance. Par exemple, un serveur de licences Services Bureau à distance Windows Server 2016 peut héberger des licences à partir de toutes les versions précédentes de Services Bureau à distance, tandis qu’un serveur de licences Services Bureau à distance Windows Server 2012 R2 peut héberger uniquement des licences jusqu’à la version Windows Server 2012 R2.
