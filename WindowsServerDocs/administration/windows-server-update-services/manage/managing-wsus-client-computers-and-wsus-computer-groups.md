---
title: Gestion des ordinateurs clients WSUS et des groupes d’ordinateurs WSUS
description: Rubrique de Windows Server Update Service (WSUS) - comment gérer les ordinateurs clients et les groupes
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e63aa5d53f01fbff3029c6896556d92c9c99aa50
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857680"
---
# <a name="managing-wsus-client-computers-and-wsus-computer-groups"></a>Gestion des ordinateurs clients WSUS et des groupes d’ordinateurs WSUS

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le nœud des ordinateurs est un point d’accès central dans la console d’administration WSUS pour gérer les ordinateurs clients WSUS et les appareils. Sous ce nœud, vous pouvez rechercher les différents groupes que vous avez configuré (ainsi que le groupe par défaut, les ordinateurs non attribués).

## <a name="managing-client-computers"></a>Gestion des ordinateurs clients
En sélectionnant un des groupes d’ordinateurs dans le **ordinateurs** nœud sous **Options** entraîne les ordinateurs de ce groupe à afficher dans le volet détails. Si un ordinateur est affecté à plusieurs groupes, il apparaît dans les listes des deux groupes. Si vous sélectionnez un ordinateur dans la liste, vous pouvez voir ses propriétés, qui incluent des informations générales sur l’ordinateur et l’état des mises à jour, telles que l’installation ou l’état de la détection d’une mise à jour pour un ordinateur particulier. Vous pouvez filtrer la liste des ordinateurs sous un groupe d’ordinateurs donné par état. La valeur par défaut montre uniquement les ordinateurs pour les mises à jour sont nécessaires ou qui ont eu des échecs d’installation ; Toutefois, vous pouvez filtrer l’affichage par n’importe quel état. Cliquez sur **Actualiser** après avoir modifié le filtre d’état.

Vous pouvez également gérer des groupes d’ordinateurs dans la page ordinateurs, ce qui inclut la création des groupes et en leur attribuant des ordinateurs. Pour plus d’informations sur la gestion des groupes d’ordinateurs, consultez Gestion des groupes d’ordinateurs dans la prochaine section de ce guide et [1.5. Planifier des groupes d’ordinateurs WSUS](../plan/plan-your-wsus-deployment.md#BKMK_1.5) à l’étape 1 : Préparer votre déploiement de WSUS du guide de déploiement de WSUS.

> [!NOTE]
> Vous devez d’abord configurer des ordinateurs clients contactent le serveur WSUS avant de pouvoir les gérer à partir de ce serveur. Jusqu'à ce que vous effectuez cette tâche, votre serveur WSUS ne reconnaîtra pas vos ordinateurs clients et ils seront affichera pas dans la liste dans la page ordinateurs. Pour plus d’informations sur la configuration des ordinateurs clients, consultez [1.5. Planifier des groupes d’ordinateurs WSUS](../plan/plan-your-wsus-deployment.md#BKMK_1.5) de l’étape 1 : Préparer votre déploiement de WSUS et l’étape 3 : Configurer WSUS, dans le guide de déploiement de WSUS.

## <a name="controlling-when-wsus-client-computers-install-updates"></a>Contrôle lorsque les ordinateurs clients WSUS installer les mises à jour
Il existe deux méthodes pour contrôler quand les ordinateurs clients WSUS installent les mises à jour :

-   Approbation dont les échéances : Strictement les échéances lorsqu’une mise à jour est installé.

-   Stratégies de groupe WSUS : Contrôlent les stratégies de groupe lors de l’analyse de l’Agent de mise à jour Windows et installe les mises à jour

    Pour plus d’informations, consultez : [Étape 5 : Configurer les paramètres de stratégie de groupe pour les mises à jour automatiques](../deploy/4-configure-group-policy-settings-for-automatic-updates.md), dans le Guide de déploiement de WSUS.

## <a name="managing-computer-groups"></a>La gestion des groupes d’ordinateurs
WSUS vous permet de cibler les mises à jour vers des groupes d’ordinateurs clients afin que vous puissiez vous assurer que des ordinateurs spécifiques obtiennent toujours les mises à jour adéquates au moment le plus approprié. Par exemple, si tous les ordinateurs d’un service (tel que l’équipe Comptabilité) ont une configuration spécifique, vous pouvez configurer un groupe pour cette équipe, décider des mises à jour dont leurs ordinateurs ont besoin et l’heure à laquelle elles doivent être installées, puis utiliser les rapports WSUS pour évaluer les mises à jour pour l’équipe.

Les ordinateurs sont toujours affectés à la **tous les ordinateurs** de groupe et restent affectés au **ordinateurs non assignés** groupe tant que vous les affectez à un autre groupe. Les ordinateurs peuvent appartenir à plusieurs groupes.

Les groupes d’ordinateurs peuvent être configurés en hiérarchies (par exemple, le groupe Traitement des paies et le groupe Compte fournisseurs sous le groupe Comptabilité). Mises à jour sont approuvées pour un groupe supérieur seront automatiquement déployées vers les groupes inférieurs, ainsi que le groupe plus élevé. Par conséquent, si vous approuvez Update1 pour le groupe comptabilité, la mise à jour sera déployé pour tous les ordinateurs dans le groupe comptabilité, tous les ordinateurs dans le groupe traitement des paies et tous les ordinateurs dans le groupe compte fournisseurs.

Dans la mesure où les ordinateurs peuvent être affectés à plusieurs groupes, il est possible pour une seule mise à jour d’être approuvée plusieurs fois pour le même ordinateur. Toutefois, la mise à jour ne sera déployée qu’une seule fois et tous les conflits seront résolus par le serveur WSUS. Pour continuer avec l’exemple ci-dessus, si computerA est affecté à la gestion de la paie et les groupes de comptes fournisseurs, et si Update1 est approuvée pour les deux groupes, il sera déployé qu’une seule fois.

Vous pouvez affecter les ordinateurs à des groupes d’ordinateurs en adoptant l’une des deux méthodes, à savoir le ciblage côté serveur ou le ciblage côté client. Avec le ciblage côté serveur, vous déplacez manuellement un ou plusieurs ordinateurs clients à un groupe d’ordinateurs à la fois. Avec le ciblage côté client, vous utilisez la stratégie de groupe ou modifiez les paramètres du Registre sur les ordinateurs clients afin d’activer ces ordinateurs pour qu’ils soient automatiquement ajoutés dans les groupes d’ordinateurs précédemment créés. Ce processus peut être scripté et déployé sur de nombreux ordinateurs à la fois. Vous devez spécifier la méthode de ciblage que vous utiliserez sur le serveur WSUS en sélectionnant une des deux options sur la **ordinateurs** section de la **Options** page.

> [!NOTE]
> Si un serveur WSUS est exécuté en mode Réplica, il n’est pas possible de créer des groupes d’ordinateurs sur ce serveur. Tous les groupes d’ordinateurs nécessaires pour les clients du serveur réplica doivent être créés sur le serveur WSUS qui est la racine de l’arborescence du serveur WSUS. Pour plus d’informations sur le mode réplica, consultez [mode réplica de WSUS en cours d’exécution](running-wsus-replica-mode.md) et pour plus d’informations sur le ciblage de côté serveur et côté client, consultez la section [1.5. Planifier des groupes d’ordinateurs WSUS](../plan/plan-your-wsus-deployment.md#BKMK_1.5) de l’étape 1 : Préparer votre déploiement de WSUS dans le guide de déploiement de WSUS.


