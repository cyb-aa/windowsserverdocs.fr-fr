---
title: Configurer les limites de notification
description: Cet article explique comment ajouter des limites de temps à différents types de notification
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: dba5b3b3c8b651935ec3c69695583d04087b7f2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826310"
---
# <a name="configure-notification-limits"></a>Configurer les limites de notification

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Pour réduire le nombre de notifications accumulées en raison de dépassements répétés d'un seuil de quota ou d'une tentative d’enregistrement de fichier non autorisé, le Gestionnaire de ressources du serveur de fichiers applique des limites de temps aux types de notification suivants :

-   Courrier électronique
-   Journal des événements
-   Command
-   Rapport

Chaque limite spécifie un délai avant qu'une autre notification du même type pour un problème identique ne soit générée.

Une limite de 60 minutes par défaut est définie pour chaque type de notification, mais vous pouvez modifier ces limites. La limite s’applique à toutes les notifications d’un type donné, qu'elles soient générées par des seuils de quotas ou par des événements de filtrage de fichiers.

## <a name="to-specify-a-standard-notification-limit-for-each-notification-type"></a>Pour spécifier une limite de notification standard pour chaque type de notification

1.  Dans l'arborescence de la console, cliquez avec le bouton droit sur **Gestionnaire de ressources du serveur de fichiers**, puis cliquez sur **Configurer les options**. La boîte de dialogue **Options du Gestionnaire de ressources du serveur de fichiers** s'affiche.

2.  Sous l'onglet **Limites de notification**, entrez une valeur en minutes pour chaque type de notification qui s’affiche.

3.  Cliquez sur **OK**.

> [!Note]
> Pour personnaliser les limites de temps associées aux notifications pour un quota spécifique ou un filtre de fichiers, vous pouvez utiliser les outils en ligne de commande du Gestionnaire de ressources du serveur de fichiers **Dirquota.exe** et **Filescrn.exe** ou les applets de commande du [Gestionnaire de ressources du serveur de fichiers](https://technet.microsoft.com/itpro/powershell/windows/fileserverresourcemanager/fileserverresourcemanager).

## <a name="see-also"></a>Voir aussi

-   [Options de paramètre File Server Resource Manager](setting-file-server-resource-manager-options.md)
-   [Outils de ligne de commande](command-line-tools.md)