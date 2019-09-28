---
title: Outils en ligne de commande des outils de gestion de ressources pour serveur de fichiers
description: Cet article décrit les outils en ligne de commande de Windows Server 2016
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 78c054c5b0c3de19d1f3acd825335eab2f140541
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394297"
---
# <a name="file-server-resource-manager-command-line-tools"></a>Outils en ligne de commande des outils de gestion de ressources pour serveur de fichiers

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Le Gestionnaire de ressources du serveur de fichiers installe les applets de commande PowerShell [FileServerResourceManager](https://technet.microsoft.com/itpro/powershell/windows/fileserverresourcemanager/fileserverresourcemanager), ainsi que les outils en ligne de commande suivants :

-   **Dirquota.exe**. Permet de créer et de gérer des quotas, des quotas automatiques et des modèles de quotas.
-   **Filescrn.exe**. Permet de créer et de gérer des filtres de fichiers, des modèles de filtres de fichiers, des exceptions de filtre de fichiers et des groupes de fichiers.
-   **Storrept.exe**. Permet de configurer des paramètres de rapport et de générer des rapports de stockage à la demande. Permet également de créer des tâches de rapport, que vous pouvez planifier à l’aide de **schtasks.exe**.

Vous pouvez utiliser ces outils pour gérer les ressources de stockage sur un ordinateur local ou distant. Pour plus d'informations ces outils en ligne de commande, consultez les références suivantes :

-   **Dirquota**: <https://go.microsoft.com/fwlink/?LinkId=92741>
-   **Filescrn**: <https://go.microsoft.com/fwlink/?LinkId=92742>
-   **StorRept**: <https://go.microsoft.com/fwlink/?LinkId=92743>


> [!Note]
> Pour afficher la syntaxe de commande et les paramètres disponibles pour une commande, exécutez la commande avec le paramètre <strong>/?</strong> .


## <a name="remote-management-using-the-command-line-tools"></a>Administration à distance à l'aide des outils en ligne de commande

Chaque outil dispose de plusieurs options pour effectuer des actions semblables à celles disponibles dans le composant logiciel enfichable MMC Gestionnaire de ressources du serveur de fichiers. Pour qu'une commande exécute une action sur un ordinateur distant au lieu de l’ordinateur local, utilisez le paramètre **/remote**:*Nom_Ordinateur*.

Par exemple,**Dirquota.exe** inclut un paramètre **template export** pour écrire des paramètres de modèle de quota dans un fichier XML et un paramètre **template import** pour importer les paramètres du modèle à partir du fichier XML. Lorsque vous ajoutez le paramètre **/remote**:*Nom_Ordinateur* à la commande **Dirquota.exe template import**, les modèles du fichier XML sur l’ordinateur local sont importés vers l’ordinateur distant.

> [!Note]
> Lorsque vous exécutez les outils en ligne de commande avec le paramètre **/remote**:<em>Nom_Ordinateur</em> pour exporter (ou importer) un modèle sur un ordinateur distant, les modèles sont écrits sur (ou copiés depuis) un fichier XML sur l’ordinateur local.

<br />

## <a name="additional-considerations"></a>Considérations supplémentaires 

Pour gérer les ressources distantes avec les outils en ligne de commande :

-   Vous devez être connecté avec un compte de domaine membre du groupe **Administrateurs** sur l'ordinateur local et sur l'ordinateur distant.
-   Vous devez exécuter les outils en ligne de commande à partir d’une fenêtre d'invite de commandes. Pour ouvrir une fenêtre d’invite de commandes avec élévation de privilèges, pointez sur **Démarrer**, **Tous les programmes**, cliquez sur **Accessoires**, cliquez avec le bouton droit sur **Invite de commandes**, puis cliquez sur **Exécuter en tant qu’administrateur**.
-   L’ordinateur distant doit exécuter Windows Server et le Gestionnaire de ressources du serveur de fichiers doit être installé.
-   L'exception **Gestion de ressources du serveur de fichiers à distance** doit être activée sur l’ordinateur distant. Activez cette exception à l’aide du pare-feu Windows dans le panneau de configuration.


## <a name="see-also"></a>Voir aussi

-   [Gestion des ressources de stockage distant](managing-remote-storage-resources.md)