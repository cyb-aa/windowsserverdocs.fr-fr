---
title: "Se connecter à un ordinateur distant"
description: "Cet article explique comment se connecter à un ordinateur distant pour gérer les ressources de stockage à partir du Gestionnaire de ressources du serveur de fichiers"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 93d2be926437b65ed8eb84a828ea0d7da6a51086
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="connect-to-a-remote-computer"></a>Se connecter à un ordinateur distant 

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2

Pour gérer les ressources de stockage sur un ordinateur distant, vous pouvez vous connecter à l'ordinateur distant à partir du Gestionnaire de ressources du serveur de fichiers. Lorsque vous êtes connecté, le Gestionnaire de ressources du serveur de fichiers vous permet de gérer les quotas, de filtrer les fichiers, de gérer les classifications, de planifier les tâches de gestion de fichiers et de générer des rapports avec ces ressources distantes.

> [!Note]
> Le Gestionnaire de ressources du serveur de fichiers peut gérer des ressources soit sur l’ordinateur local, soit sur un ordinateur distant, mais pas les deux en même temps.

## <a name="to-connect-to-a-remote-computer-from-file-server-resource-manager"></a>Pour se connecter à un ordinateur distant à partir du Gestionnaire de ressources du serveur de fichiers

1.  Dans **Outils d'administration**, cliquez sur **Gestionnaire de ressources du serveur de fichiers**.

2.  Dans l'arborescence de la console, cliquez avec le bouton droit sur **Gestionnaire de ressources du serveur de fichiers**, puis cliquez sur **Se connecter à un autre ordinateur**.

3.  Dans la boîte de dialogue **Se connecter à un autre ordinateur**, cliquez sur **Autre ordinateur**. Puis tapez le nom du serveur auquel vous souhaitez vous connecter (ou cliquez sur **Parcourir** pour rechercher un ordinateur distant).

4.  Cliquez sur **OK**.

> [!Important]
> La commande **Se connecter à un autre ordinateur** est disponible uniquement si vous ouvrez le Gestionnaire de ressources de serveur de fichiers à partir des **Outils d’administration**. Si vous accédez au Gestionnaire de ressources de serveur de fichiers à partir du Gestionnaire de serveur, la commande n’est pas disponible.

## <a name="additional-considerations"></a>Autres éléments à prendre en considération

Pour gérer des ressources distantes à l'aide du Gestionnaire de ressources du serveur de fichiers:

-   Vous devez être connecté à l'ordinateur local avec un compte de domaine membre du groupe **Administrateurs** sur l'ordinateur distant.
-   L’ordinateur distant doit exécuter Windows Server et le Gestionnaire de ressources du serveur de fichiers doit être installé.
-   L'exception **Gestion de ressources du serveur de fichiers à distance** doit être activée sur l’ordinateur distant. Activez cette exception à l’aide du pare-feu Windows dans le panneau de configuration.

## <a name="see-also"></a>Articles associés

-   [Gestion des ressources de stockage distant](managing-remote-storage-resources.md)