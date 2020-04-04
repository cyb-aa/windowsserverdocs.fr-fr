---
title: Gestion des ordinateurs clients WSUS et des groupes d’ordinateurs WSUS
description: Rubrique Windows Server Update Service (WSUS)-procédure de gestion des ordinateurs clients et des groupes
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b1ea915-0f9f-4f0e-8913-a1dd460d07ab
author: coreyp-at-msft
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: 36ac4230ae84cae3a859e53e5faa2b83cb7412d5
ms.sourcegitcommit: 3c3dfee8ada0083f97a58997d22d218a5d73b9c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639823"
---
# <a name="managing-wsus-client-computers-and-wsus-computer-groups"></a>Gestion des ordinateurs clients WSUS et des groupes d’ordinateurs WSUS

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Le nœud ordinateurs est un point d’accès central dans la console d’administration WSUS pour la gestion des ordinateurs clients et des appareils WSUS. Sous ce nœud, vous pouvez trouver les différents groupes que vous avez configurés (plus le groupe par défaut, ordinateurs non attribués).

## <a name="managing-client-computers"></a>Gestion des ordinateurs clients
Si vous sélectionnez l’un des groupes d’ordinateurs dans le nœud **ordinateurs** sous **options** , les ordinateurs de ce groupe s’affichent dans le volet d’informations. Si un ordinateur est attribué à plusieurs groupes, il apparaît dans les listes des deux groupes. Si vous sélectionnez un ordinateur dans la liste, vous pouvez voir ses propriétés, qui incluent des détails généraux sur l’ordinateur et l’état des mises à jour pour celui-ci, telles que l’état de l’installation ou de la détection d’une mise à jour pour un ordinateur particulier. Vous pouvez filtrer la liste des ordinateurs sous un groupe d’ordinateurs donné par État. La valeur par défaut affiche uniquement les ordinateurs pour lesquels des mises à jour sont nécessaires ou qui ont subi des échecs d’installation. Toutefois, vous pouvez filtrer l’affichage par n’importe quel état. Cliquez sur **Actualiser** après avoir modifié le filtre d’État.

Vous pouvez également gérer des groupes d’ordinateurs sur la page ordinateurs, qui comprend la création de groupes et leur affectation d’ordinateurs. Pour plus d’informations sur la gestion des groupes d’ordinateurs, consultez Managing Computer groups dans la section suivante de ce guide et la section [1,5. Planifiez les groupes d’ordinateurs WSUS](../plan/plan-your-wsus-deployment.md#15-plan-wsus-computer-groups) à l’étape 1 : préparer votre déploiement de WSUS du Guide de déploiement de WSUS.

> [!NOTE]
> Vous devez d’abord configurer les ordinateurs clients pour qu’ils contactent le serveur WSUS avant de pouvoir les gérer à partir de ce serveur. Tant que vous n’aurez pas effectué cette tâche, votre serveur WSUS ne reconnaîtra pas vos ordinateurs clients et ils ne seront pas affichés dans la liste de la page ordinateurs. Pour plus d’informations sur la configuration des ordinateurs clients, consultez [1,5. Planifiez les groupes d’ordinateurs WSUS](../plan/plan-your-wsus-deployment.md#15-plan-wsus-computer-groups) de l’étape 1 : préparer votre déploiement WSUS et étape 3 : configurer WSUS dans le Guide de déploiement de WSUS.

## <a name="controlling-when-wsus-client-computers-install-updates"></a>Contrôle du moment où les ordinateurs clients WSUS installent les mises à jour
Il existe deux méthodes pour contrôler le moment où les ordinateurs clients WSUS installent les mises à jour :

-   Approbation avec échéances : les échéances s’appliquent strictement lorsqu’une mise à jour est installée

-   Stratégies de groupe WSUS : contrôle des stratégies de groupe lors de l’analyse et de l’installation des mises à jour par l’agent Windows Update

    Pour plus d’informations, consultez : [étape 5 : configurer les paramètres de stratégie de groupe pour mises à jour automatiques](../deploy/4-configure-group-policy-settings-for-automatic-updates.md)dans le Guide de déploiement de WSUS.

## <a name="managing-computer-groups"></a>Gestion des groupes d’ordinateurs
WSUS vous permet de cibler les mises à jour vers des groupes d’ordinateurs clients afin que vous puissiez vous assurer que des ordinateurs spécifiques obtiennent toujours les mises à jour adéquates au moment le plus approprié. Par exemple, si tous les ordinateurs d’un service (tel que l’équipe Comptabilité) ont une configuration spécifique, vous pouvez configurer un groupe pour cette équipe, décider des mises à jour dont leurs ordinateurs ont besoin et l’heure à laquelle elles doivent être installées, puis utiliser les rapports WSUS pour évaluer les mises à jour pour l’équipe.

Les ordinateurs sont toujours affectés au groupe **tous les ordinateurs** et restent affectés au groupe **ordinateurs non attribués** tant que vous ne les affectez pas à un autre groupe. Les ordinateurs peuvent appartenir à plusieurs groupes.

Les groupes d’ordinateurs peuvent être configurés en hiérarchies (par exemple, le groupe Traitement des paies et le groupe Compte fournisseurs sous le groupe Comptabilité). Les mises à jour approuvées pour un groupe supérieur sont automatiquement déployées vers des groupes inférieurs, ainsi que vers le groupe supérieur lui-même. Ainsi, si vous approuvez Update 1 pour le groupe comptabilité, la mise à jour sera déployée sur tous les ordinateurs du groupe comptabilité, tous les ordinateurs du groupe de paie et tous les ordinateurs du groupe comptes fournisseurs.

Dans la mesure où les ordinateurs peuvent être affectés à plusieurs groupes, il est possible pour une seule mise à jour d’être approuvée plusieurs fois pour le même ordinateur. Toutefois, la mise à jour ne sera déployée qu’une seule fois et tous les conflits seront résolus par le serveur WSUS. Pour poursuivre l’exemple ci-dessus, si ComputerA est affecté aux groupes Payroll et Accounts payable et que Update 1 est approuvé pour les deux groupes, il ne sera déployé qu’une seule fois.

Vous pouvez affecter les ordinateurs à des groupes d’ordinateurs en adoptant l’une des deux méthodes, à savoir le ciblage côté serveur ou le ciblage côté client. Avec le ciblage côté serveur, vous déplacez manuellement un ou plusieurs ordinateurs clients vers un groupe d’ordinateurs à la fois. Avec le ciblage côté client, vous utilisez la stratégie de groupe ou modifiez les paramètres du Registre sur les ordinateurs clients afin d’activer ces ordinateurs pour qu’ils soient automatiquement ajoutés dans les groupes d’ordinateurs précédemment créés. Ce processus peut être scripté et déployé sur plusieurs ordinateurs à la fois. Vous devez spécifier la méthode de ciblage que vous allez utiliser sur le serveur WSUS en sélectionnant l’une des deux options disponibles dans la section **ordinateurs** de la page **options** .

> [!NOTE]
> Si un serveur WSUS est exécuté en mode Réplica, il n’est pas possible de créer des groupes d’ordinateurs sur ce serveur. Tous les groupes d’ordinateurs nécessaires pour les clients du serveur de réplication doivent être créés sur le serveur WSUS qui est la racine de la hiérarchie du serveur WSUS. Pour plus d’informations sur le mode réplica, consultez [exécution du mode RÉPLICA WSUS](running-wsus-replica-mode.md) et pour plus d’informations sur le ciblage côté serveur et côté client, consultez la section [1,5. Planifiez les groupes d’ordinateurs WSUS](../plan/plan-your-wsus-deployment.md#15-plan-wsus-computer-groups) de l’étape 1 : préparer votre déploiement de WSUS dans le Guide de déploiement de WSUS.


