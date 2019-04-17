---
title: "Liste de vérification: appliquer un filtre de fichiers à un volume ou un dossier"
description: "Cet article explique comment appliquer un filtre de fichiers à un volume ou un dossier"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: f4c6953d27361ab2e3210a1574a5988e434f701f
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="checklist---apply-a-file-screen-to-a-volume-or-folder"></a>Liste de vérification: appliquer un filtre de fichiers à un volume ou un dossier

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2

Pour appliquer un filtre de fichiers à un volume ou un dossier, utilisez la liste suivante:
1. Configurez les paramètres de messagerie si vous prévoyez d’envoyer des notifications de filtrage des fichiers ou des rapports de stockage par courrier électronique. Pour ce faire, suivez les instructions dans [Configurer les notifications par courrier électronique](configure-email-notifications.md).

2. Activez l’enregistrement des événements de filtrage des fichiers dans la base de données de vérification si vous prévoyez de générer des rapports de vérification du filtrage des fichiers.
[Configurez la vérification du filtrage de fichiers](configure-file-screen-audit.md)

3. Évaluez les types de fichiers stockés qui constituent des candidats aux règles de filtrage. Vous pouvez utiliser des rapports au niveau du nœud **Gestion des rapports de stockage** pour fournir des données. (Par exemple, exécutez un rapport Fichiers par groupe de fichiers ou un rapport Fichiers volumineux à la demande pour identifier les fichiers qui occupent beaucoup d’espace disque.) [Générez des rapports à la demande](generate-reports-on-demand.md) 

4. Passez en revue les groupes de fichiers préconfigurés ou créez un nouveau groupe de fichiers pour appliquer une stratégie de filtrage spécifique dans votre organisation. [Définissez des groupes de fichiers pour le filtrage](define-file-groups-for-screening.md)  

5. Passez en revue les propriétés des modèles de filtres de fichiers disponibles. (Dans **Gestion du filtrage de fichiers**, cliquez sur le nœud **Modèles de filtres de fichiers**.) [Modifiez les propriétés du modèle de filtre de fichiers](edit-file-screen-template-properties.md) 
<br />
 - Ou -
 <br /> Créez un nouveau modèle de filtre de fichiers pour appliquer une stratégie de stockage dans votre organisation.  [Créez un modèle de filtre de fichiers](create-file-screen-template.md) 

6. Créez un filtre de fichiers basé sur le modèle sur un volume ou un dossier. 
 [Créez un filtre de fichiers](create-file-screen.md)
 
7. Configurez des exceptions de filtre de fichiers dans les sous-dossiers du dossier ou du volume. [Créez une exception au filtre de fichiers](create-file-screen-exception.md) 

8. Planifiez une tâche de rapport contenant un rapport de vérification du filtrage des fichiers pour surveiller l’activité de filtrage régulièrement.
  [Planifiez un ensemble de rapports](schedule-set-of-reports.md)


> [!NOTE]
> Pour limiter le stockage sur un volume ou un dossier, voir [Liste de vérification: appliquer un quota à un volume ou un dossier](checklist-apply-file-screen-to-volume-or-folder.md)