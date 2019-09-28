---
title: Configurer des rapports de stockage
description: Cet article explique comment configurer les paramètres par défaut des rapports de stockage
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: d3500f4ea4fc264f3cb663f17c3a50439b9cb454
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394261"
---
# <a name="configure-storage-reports"></a>Configurer des rapports de stockage

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Vous pouvez configurer les paramètres par défaut des rapports de stockage. Ces paramètres par défaut sont utilisés pour les rapports d’incidents générés par un événement de quota ou de filtrage de fichiers. Ils servent également aux rapports planifiés et à la demande. Vous pouvez remplacer les paramètres par défaut lorsque vous définissez les propriétés spécifiques de ces rapports.

> [!Important]
> Lorsque vous modifiez les paramètres par défaut pour un type de rapport, les modifications affectent tous les rapports d'incidents et toutes les tâches de rapport planifiées existantes qui utilisent les paramètres par défaut.

## <a name="to-configure-the-default-parameters-for-storage-reports"></a>Pour configurer les paramètres par défaut des rapports de stockage

1. Dans l'arborescence de la console, cliquez avec le bouton droit sur **Gestionnaire de ressources du serveur de fichiers**, puis cliquez sur **Configurer les options**. La boîte de dialogue **Options du Gestionnaire de ressources du serveur de fichiers** s'affiche.

2. Sous l'onglet **Rapports de stockage**, sous **Configurer les paramètres par défaut**, sélectionnez le type de rapport que vous souhaitez modifier.

3. Cliquez sur **Modifier les paramètres**.

4. Selon le type de rapport sélectionné, différents paramètres de rapport à modifier sont proposés. Effectuez toutes les modifications nécessaires, puis cliquez sur **OK** pour les enregistrer en tant que paramètres par défaut pour ce type de rapport.

5.  Répétez les étapes 2 à 4 pour chaque type de rapport à modifier.

6. Pour afficher la liste des paramètres par défaut pour tous les rapports, cliquez sur **Consulter les rapports**. Cliquez ensuite sur **Fermer**.

7.  Cliquez sur **OK**.

## <a name="see-also"></a>Voir aussi

-   [Définition des options du Gestionnaire de ressources du serveur de fichiers](setting-file-server-resource-manager-options.md)
-   [Gestion des rapports de stockage](storage-reports-management.md)