---
title: Licence de votre déploiement des services Bureau à distance avec des licences d’accès client (CAL)
description: Vue d’ensemble du client Gestionnaire de licences des Services Bureau à distance.
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
ms.openlocfilehash: 6648a52bb4d09725935a2197d6ce6fa6d8cc74a8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853440"
---
# <a name="license-your-rds-deployment-with-client-access-licenses-cals"></a>Licence de votre déploiement des services Bureau à distance avec des licences d’accès client (CAL)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Chaque utilisateur et périphérique se connecte à un hôte de Session Bureau à distance doivent un licences d’accès client (CAL). Utiliser le Gestionnaire de licences bureau à distance pour installer, émettre et effectuer le suivi des licences d’accès client RDS.  

Lorsqu’un utilisateur ou un périphérique se connecte à un serveur hôte de Session Bureau à distance, le serveur hôte de Session Bureau à distance détermine si une CAL RDS est nécessaire. Le serveur hôte de Session Bureau à distance demande ensuite une CAL RDS à partir du serveur de licences bureau à distance. Si une licence est disponible à partir d’un serveur de licences, la CAL RDS est émise pour le client et le client est en mesure de se connecter au serveur hôte de Session Bureau à distance et à partir de là vers le bureau ou les applications qu’ils vous essayez d’utiliser.

Bien qu’il existe une période de grâce pendant l’aucune licence serveur est nécessaire, après que la période de grâce terminée, les clients doit être une CAL RDS valide émis par un serveur de licences avant de se connecter à un serveur hôte de Session Bureau à distance.

Utilisez les informations suivantes pour en savoir plus sur comment fonctionnent les licences d’accès client des Services Bureau à distance et à déployer et gérer vos licences :

- [Comprendre le modèle de licences d’accès client](#understanding-the-cals-model)
- [Activer le serveur de licences](rds-activate-license-server.md)
- [Installer des licences d’accès client RDS sur le serveur de licences](rds-install-cals.md)
- [Suivre les CAL utilisées dans votre déploiement](rds-track-cals.md)

## <a name="understanding-the-cals-model"></a>Présentation du modèle de licences d’accès client

Il existe deux types de licences d’accès client :

- Services Bureau à distance par les licences d’accès client de périphérique
- Services Bureau à distance par des CAL utilisateur

Le tableau suivant décrit les différences entre les deux types de licences d’accès client :

| Par périphérique                                                     | Par utilisateur                                                                         |
|----------------------------------------------------------------|----------------------------------------------------------------------------------|
| Licences d’accès client sont physiquement affectés à chaque appareil.                   | Licences d’accès client sont affectés à un utilisateur dans Active Directory.                                 |
| Licences d’accès client sont suivies par le serveur de licences.                        | Licences d’accès client sont suivies par le serveur de licences.                                          |
| Licences d’accès client peuvent être suivis, quel que soit l’appartenance à Active Directory. | Licences d’accès client ne peut pas être suivies dans un groupe de travail.                                       |
| Vous pouvez révoquer jusqu'à 20 % des licences d’accès client.                              | Vous ne pouvez pas révoquer les licences d’accès client.                                                      |
| Les CAL temporaires sont valides pendant 52 : 89 jours.                       | Les CAL temporaires ne sont pas disponibles.                                                |
| Licences d’accès client ne peut pas être surutilisées.                                  | Licences d’accès client peuvent être surutilisées (en violation de l’accord de licence de bureau à distance). |

Lorsque vous utilisez le modèle par périphérique, une licence temporaire est émise à la première fois qu'un appareil se connecte à l’hôte de Session Bureau à distance. La deuxième fois que l’appareil se connecte, tant que le serveur de licences est activé et il existe des licences d’accès client disponibles, les problèmes de serveur de licences une CAL permanente de services Bureau à distance par appareil.

Lorsque vous utilisez le modèle par utilisateur, Gestionnaire de licences n’est pas appliquée, et chaque utilisateur se voit accorder une licence pour se connecter à un hôte de Session Bureau à distance à partir de n’importe quel nombre d’appareils. Le serveur de licences émet des licences à partir du pool de licence d’accès client disponible ou le pool de licence d’accès client Over-Used. Il vous incombe pour vous assurer que tous vos utilisateurs ont une licence valide et Over-Used CAL de zéro, sinon, vous êtes en violation des termes du contrat de licence des Services Bureau à distance.

Pour assurer la conformité avec les termes du contrat de licence des Services Bureau à distance, suivre le nombre de licences par utilisateur utilisés dans votre organisation et assurez-vous de disposer d’un suffisamment des CAL par utilisateur installée sur le serveur de licences pour tous vos utilisateurs.

Vous pouvez utiliser le Gestionnaire de licences bureau à distance pour effectuer le suivi et générer des rapports sur les services Bureau à distance des CAL par utilisateur.

## <a name="note-about-cal-versions"></a>Remarque sur les versions de licence d’accès client

La licence d’accès client utilisée par les utilisateurs ou appareils doit correspondre à la version de Windows Server auquel se connecte l’utilisateur ou l’appareil. Vous ne pouvez pas utiliser les anciennes licences d’accès client pour accéder aux versions plus récentes de Windows Server, mais vous pouvez utiliser des licences d’accès client plus récent pour accéder aux versions antérieures de Windows Server.

Le tableau suivant présente les CAL sont compatibles sur les hôtes de Session Bureau à distance et les hôtes de virtualisation des services Bureau à distance.

|                  |2008 R2 et les licences d’accès client antérieures|2012 CAL|2016 CAL|2019 CAL|
|---------------------------------|--------|--------|--------|--------|
| **2008, 2008 R2 server sous licence**| Oui    | Non     | Non     | Non     |
| **serveur de licences 2012**         | Oui    | Oui    | Non     | Non     |
| **Serveur de licences de 2012 R2**      | Oui    | Oui    | Non     | Non     |
| **serveur de licences 2016**         | Oui    | Oui    | Oui    | Non     |
| **serveur de licences 2019**         | Oui    | Oui    | Oui    | Oui    |

N’importe quel serveur de licences des services Bureau à distance peut héberger des licences à partir de toutes les versions précédentes des Services Bureau à distance et la version actuelle des Services Bureau à distance. Par exemple, un serveur de licences des services Bureau à distance de Windows Server 2016 peut héberger des licences à partir de toutes les versions précédentes des services Bureau à distance, tandis qu’un serveur de licences des services Bureau à distance de Windows Server 2012 R2 peut héberger uniquement des licences à Windows Server 2012 R2.
