---
title: Configurer la vérification du filtrage de fichiers
description: Cet article explique comment configurer la vérification du filtrage de fichiers pour générer le rapport de vérification du filtrage des fichiers
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: a9444e158a935f4e931aa7a5d634d970c5acc88e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394565"
---
# <a name="configure-file-screen-audit"></a>Configurer la vérification du filtrage de fichiers

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Le Gestionnaire de ressources du serveur de fichiers permet d'enregistrer l’activité de filtrage de fichiers dans une base de données de vérification. Les informations enregistrées dans cette base de données sont utilisées pour générer le rapport de vérification du filtrage des fichiers.

> [!Important]
> Si la case à cocher **Enregistrer l’activité de filtrage de fichiers dans la base de données de vérification** est désactivée, les rapports de vérification du filtrage des fichiers ne contiendront aucune information.

## <a name="to-configure-file-screen-audit"></a>Pour configurer la vérification du filtrage de fichiers

1.  Dans l'arborescence de la console, cliquez avec le bouton droit sur **Gestionnaire de ressources du serveur de fichiers**, puis cliquez sur **Configurer les options**. La boîte de dialogue **Options du Gestionnaire de ressources du serveur de fichiers** s'affiche.

2.  Sous l'onglet **Vérification du filtrage de fichiers**, cochez la case **Enregistrer l’activité de filtrage de fichiers dans la base de données de vérification**.

3.  Cliquez sur **OK**. Toute l'activité de filtrage de fichiers est à présent stockée dans la base de données de vérification. Vous pouvez la consulter en exécutant un rapport de vérification du filtrage des fichiers.

## <a name="see-also"></a>Voir aussi

-   [Définition des options du Gestionnaire de ressources du serveur de fichiers](setting-file-server-resource-manager-options.md)
-   [Gestion des rapports de stockage](storage-reports-management.md)