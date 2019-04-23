---
title: Assistant de nettoyage du serveur
description: Rubrique de Windows Server Update Service (WSUS) - l’utilisation de l’Assistant de nettoyage de serveur pour gérer l’espace disque
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7c351797-2716-4442-a668-60d5b4e77751
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: da87db95303ba7254e9f1f9dbc7219423e9e6b72
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884070"
---
# <a name="the-server-cleanup-wizard"></a>Assistant de nettoyage du serveur

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

L’Assistant Nettoyage du serveur est intégré dans l’interface utilisateur et peut être utilisé pour vous aider à gérer votre espace disque. Cet Assistant peut effectuer les opérations suivantes :

-   Supprimer des mises à jour inutilisées et supprimer les révisions de mise à jour toutes les anciennes mises à jour et révisions de mise à jour qui n’ont pas été approuvées.

-   Supprimer les ordinateurs ne contactant pas la suppression du serveur tous les ordinateurs clients qui n’ont pas contacté le serveur dans les trente jours ou plus.

-   Supprimer la suppression de fichiers de mise à jour inutiles que tout mettre à jour les fichiers qui ne sont pas nécessaires par les mises à jour ou par les serveurs en aval.

-   Refuser les mises à jour expirées refuser toutes les mises à jour qui ont été expirés par Microsoft.

-   Mises à jour de refus remplacée refuser toutes les mises à jour qui répondent à tous les critères suivants :

    -   La mise à jour remplacée n’est pas obligatoire

    -   La mise à jour remplacée a été sur le serveur pendant trente jours ou plus

    -   La mise à jour remplacée n’est pas actuellement signalée comme nécessaires par n’importe quel client

    -   La mise à jour remplacée n’a pas été explicitement déployé sur un groupe d’ordinateurs pendant 90 jours ou plus

    -   La mise à jour de remplacement doit être approuvée pour l’installation à un groupe d’ordinateurs

    >  [!WARNING]  
    >  Dans une hiérarchie WSUS, il est fortement recommandé que vous exécutez d’abord le processus de nettoyage sur le serveur WSUS de réplication en aval/plus bas et progresser ensuite dans la hiérarchie. Exécution incorrecte de nettoyage sur n’importe quel serveur en amont avant d’exécuter le nettoyage sur chaque serveur en aval peut entraîner une incohérence entre les données qui n’est présentes dans les bases de données en amont et en aval bases de données. Échecs de synchronisation peut entraîner l’incohérence des données entre les serveurs en amont et en aval. 

 >  [!IMPORTANT]  
    >  Si vous supprimez le contenu inutile à l’aide de l’Assistant de nettoyage du serveur, tous les fichiers de mise à jour privé que vous avez téléchargé à partir du site du catalogue Microsoft Update sont également supprimés. Vous devez réimporter ces fichiers après avoir exécuté l’Assistant de nettoyage du serveur. 

Si les mises à jour sont approuvées à l’aide d’une règle d’approbation automatique, ils peut-être toujours en cours de l’état « Approuvé » et ne seront pas supprimées par le nettoyage de serveur de l’Assistant. Pour supprimer automatiquement-mises à jour approuvées qui sont dans un état « approved », l’administrateur WSUS doit - au minimum - définir manuellement l’état d’approbation des mises à jour remplacées à « Non approuvés » afin qu’ils seront éligibles pour refus par l’Assistant Nettoyage du serveur. Le nettoyage de serveur qu'assure une mise à jour plus récente est approuvé et qu’aucun système client n’est toujours un état qui mettre à jour en fonction des besoins avant de marquer la mise à jour en tant que « Refusé ».




