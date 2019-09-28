---
title: Assistant de nettoyage du serveur
description: Rubrique Windows Server Update Service (WSUS)-comment utiliser l’Assistant Nettoyage de serveur pour gérer l’espace disque
ms.prod: windows-server
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
ms.openlocfilehash: e285e59a27b6bf0ef1bf3b1ab0f78a96efa60c87
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361532"
---
# <a name="the-server-cleanup-wizard"></a>Assistant de nettoyage du serveur

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

L’Assistant Nettoyage de serveur est intégré à l’interface utilisateur et peut être utilisé pour vous aider à gérer l’espace disque. Cet Assistant peut effectuer les opérations suivantes :

- Supprimer les mises à jour inutilisées et mettre à jour les révisions supprimez toutes les mises à jour anciennes et les révisions de mise à jour qui n’ont pas été approuvées.

- Supprimer les ordinateurs qui ne contactent pas le serveur supprimez tous les ordinateurs clients qui n’ont pas contacté le serveur dans un délai de trente jours ou plus.

- Supprimer les fichiers de mise à jour inutiles supprimez tous les fichiers de mise à jour qui ne sont pas nécessaires pour les mises à jour ou les serveurs en aval.

- Les mises à jour refusées qui ont expiré refusent toutes les mises à jour qui ont expiré par Microsoft.

- Refuser les mises à jour remplacées décline toutes les mises à jour qui répondent à tous les critères suivants :

  -   La mise à jour remplacée n’est pas obligatoire

  -   La mise à jour remplacée a été sur le serveur pendant trente jours ou plus

  -   La mise à jour remplacée n’est actuellement pas signalée comme nécessaire par un client

  -   La mise à jour remplacée n’a pas été explicitement déployée sur un groupe d’ordinateurs pendant 90 jours ou plus

  -   La mise à jour de remplacement doit être approuvée pour l’installation sur un groupe d’ordinateurs

  > [!WARNING]
  >  Dans une hiérarchie WSUS, il est fortement recommandé d’abord d’exécuter le processus de nettoyage sur le serveur WSUS en aval/réplica le plus bas, puis de remonter la hiérarchie. Une exécution incorrecte du nettoyage sur un serveur en amont avant d’exécuter le nettoyage sur chaque serveur en aval peut entraîner une incohérence entre les données présentes dans les bases de données en amont et les bases de données en aval. L’incompatibilité des données peut entraîner des échecs de synchronisation entre les serveurs en aval et en amont. 
  > 
  > [!IMPORTANT]
  >  Si vous supprimez du contenu inutile à l’aide de l’Assistant de nettoyage de serveur, tous les fichiers de mise à jour privés que vous avez téléchargés à partir du site du catalogue Microsoft Update sont également supprimés. Vous devez réimporter ces fichiers après avoir exécuté l’Assistant Nettoyage du serveur. 

Si les mises à jour sont approuvées à l’aide d’une règle d’approbation automatique, elles sont peut-être toujours dans l’État « approuvé » et ne sont pas supprimées par l’Assistant Nettoyage du serveur. Pour supprimer les mises à jour approuvées automatiquement qui sont dans un État « approuvé », l’administrateur WSUS doit au minimum définir manuellement l’état d’approbation des mises à jour remplacées sur « non approuvé » afin qu’elles puissent être refusées par l’Assistant de nettoyage du serveur. L’Assistant Nettoyage du serveur s’assure qu’une mise à jour plus récente est approuvée et qu’aucun système client ne signale toujours cette mise à jour si nécessaire, avant de marquer la mise à jour comme « refusée ».




