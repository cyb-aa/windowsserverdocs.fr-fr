---
title: Mode de réplica WSUS en cours d’exécution
description: 'Rubrique Windows Server Update Service (WSUS)-Guide pratique pour configurer le mode de réplica '
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d218cd6b-3b6b-4429-913b-31d412ce3356
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b7da68fa9cbe71f8a67e74671d64d11908ae4654
ms.sourcegitcommit: 9687d3eb221b89061a48bf1e73fb3b25bee69f9a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2020
ms.locfileid: "78169559"
---
# <a name="running-wsus-replica-mode"></a>Mode de réplica WSUS en cours d’exécution

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un serveur WSUS qui s’exécute en mode réplica hérite des approbations de mise à jour et des groupes d’ordinateurs créés sur un serveur d’administration. Dans un scénario qui utilise le mode réplica, vous avez généralement un serveur d’administration unique et un ou plusieurs serveurs WSUS réplicas subordonnés répartis au sein de l’organisation, en fonction de la topographie d’un site ou d’une organisation. Vous approuvez les mises à jour et créez des groupes d’ordinateurs sur le serveur d’administration, que les serveurs en mode de réplica vont ensuite mettre en miroir. Les serveurs en mode de réplica peuvent être configurés uniquement lors de la configuration de WSUS. Si vous avez implémenté ce scénario, il est probable que les approbations de mise à jour et les groupes d’ordinateurs soient gérés de manière centralisée dans votre organisation.

Si votre serveur WSUS s’exécute en mode réplica, vous ne pouvez effectuer que des fonctionnalités d’administration limitées sur le serveur, qui se composent principalement des éléments suivants :

-   Ajout et suppression d’ordinateurs dans des groupes d’ordinateurs. L’appartenance au groupe d’ordinateurs n’est pas distribuée aux serveurs de réplication, mais uniquement les groupes d’ordinateurs eux-mêmes. Par conséquent, sur un serveur en mode réplica, vous héritez des groupes d’ordinateurs que vous avez créés sur le serveur d’administration. Toutefois, les groupes d’ordinateurs sont vides. Vous devez ensuite attribuer les ordinateurs clients qui se connectent au serveur de réplication pour les groupes d’ordinateurs.

-   Définition d’une planification de la synchronisation

-   Spécification des paramètres du serveur proxy

-   Spécification de la source de mise à jour. Il peut s’agir d’un serveur autre que le serveur d’administration

-   Affichage des mises à jour disponibles

-   Surveillance des paramètres de mise à jour, de synchronisation, d’état de l’ordinateur et WSUS sur le serveur

-   Exécution de tous les rapports WSUS standard disponibles sur les serveurs en mode réplica



