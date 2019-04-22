---
title: Configurer la vérification du filtrage de fichiers
description: Cet article explique comment configurer la vérification du filtrage de fichiers pour générer le rapport de vérification du filtrage des fichiers
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 89592a9e1f61374d2d909678a91dc4a06e0b1972
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824470"
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

-   [Options de paramètre File Server Resource Manager](setting-file-server-resource-manager-options.md)
-   [Gestion des rapports de stockage](storage-reports-management.md)