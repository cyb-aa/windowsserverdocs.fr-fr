---
title: Mode de réplica WSUS en cours d’exécution
description: 'Rubrique de Windows Server Update Service (WSUS) - comment configurer le mode de réplica '
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3b4139354a3f0f7b1f1a97107d2f6b28db2b02c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878740"
---
# <a name="running-wsus-replica-mode"></a>Mode de réplica WSUS en cours d’exécution

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un serveur WSUS en cours d’exécution en mode réplica hérite les approbations de mise à jour et les groupes d’ordinateurs créés sur un serveur d’administration. Dans un scénario qui utilise le mode réplica, vous devez en général un seul serveur d’administration et un ou plusieurs serveurs WSUS réplica subordonnés répartis dans toute l’organisation, basée sur site ou de la topographie d’organisation. Approuver les mises à jour et de créer des groupes d’ordinateurs sur le serveur d’administration, puis répercute les serveurs en mode réplica. Serveurs en mode réplica peuvent être configurés uniquement lors de l’installation de WSUS, et si vous avez implémenté ce scénario, il est probable, car il est important de votre organisation qui mettent à jour les approbations et les groupes d’ordinateurs sont gérés de manière centralisée.

Si votre serveur WSUS s’exécute en mode réplica, vous serez en mesure d’effectuer uniquement les fonctionnalités d’administration limités sur le serveur, ce qui se compose principalement de :

-   Ajout et suppression d’ordinateurs à partir de groupes d’ordinateurs. Groupes d’ordinateurs ne sont pas distribué à des serveurs de réplication, seul l’ordinateur groupes eux-mêmes. Par conséquent, sur un serveur en mode réplica, vous héritera les groupes d’ordinateurs que vous avez créé sur le serveur d’administration. Toutefois, les groupes d’ordinateurs sera vides. Vous devez puis attribuez les ordinateurs clients qui se connectent au serveur de réplica pour les groupes d’ordinateurs.

-   Définition d’une planification de la synchronisation

-   Spécification des paramètres de serveur proxy

-   Spécification de la source de mise à jour. Cela peut être un serveur autre que le serveur d’administration

-   Affichage des mises à jour disponibles

-   Mise à jour, la synchronisation, état de l’ordinateur et les paramètres WSUS sur le serveur de surveillance

-   Exécutant WSUS standards tous les rapports disponibles sur les serveurs en mode réplica



