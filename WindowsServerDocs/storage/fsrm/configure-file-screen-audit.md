---
title: "Configurer la vérification du filtrage de fichiers"
description: "Cet article explique comment configurer la vérification du filtrage de fichiers pour générer le rapport de vérification du filtrage des fichiers"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 89592a9e1f61374d2d909678a91dc4a06e0b1972
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="configure-file-screen-audit"></a>Configurer la vérification du filtrage de fichiers

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2

Le Gestionnaire de ressources du serveur de fichiers permet d'enregistrer l’activité de filtrage de fichiers dans une base de données de vérification. Les informations enregistrées dans cette base de données sont utilisées pour générer le rapport de vérification du filtrage des fichiers.

> [!Important]
> Si la case à cocher **Enregistrer l’activité de filtrage de fichiers dans la base de données de vérification** est désactivée, les rapports de vérification du filtrage des fichiers ne contiendront aucune information.

## <a name="to-configure-file-screen-audit"></a>Pour configurer la vérification du filtrage de fichiers

1.  Dans l'arborescence de la console, cliquez avec le bouton droit sur **Gestionnaire de ressources du serveur de fichiers**, puis cliquez sur **Configurer les options**. La boîte de dialogue **Options du Gestionnaire de ressources du serveur de fichiers** s'affiche.

2.  Sous l'onglet **Vérification du filtrage de fichiers**, cochez la case **Enregistrer l’activité de filtrage de fichiers dans la base de données de vérification**.

3.  Cliquez sur **OK**. Toute l'activité de filtrage de fichiers est à présent stockée dans la base de données de vérification. Vous pouvez la consulter en exécutant un rapport de vérification du filtrage des fichiers.

## <a name="see-also"></a>Articles associés

-   [Définition des options du Gestionnaire de ressources du serveur de fichiers](setting-file-server-resource-manager-options.md)
-   [Gestion des rapports de stockage](storage-reports-management.md)