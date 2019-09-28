---
title: Appliquer un quota à un volume ou un dossier
description: Cet article explique comment appliquer un quota de stockage à un volume ou un dossier
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 44eea3c3802e261239db9bf8350777e1b83fb9c4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394333"
---
# <a name="checklist-apply-a-quota-to-a-volume-or-folder"></a>Liste de vérification : Appliquer un quota à un volume ou à un dossier

> S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server (canal semi-annuel)

1. Configurez les paramètres de messagerie si vous prévoyez d’envoyer des notifications de seuil ou des rapports de stockage par courrier électronique. [Configurer les notifications par courrier électronique](configure-email-notifications.md)

2. Évaluez les besoins de stockage sur le volume ou le dossier. Vous pouvez utiliser des rapports sur le nœud **Gestion des rapports de stockage** pour fournir des données. (Par exemple, exécutez une rapport sur les fichiers par propriétaire rapport à la demande pour identifier les utilisateurs qui utilisent de grandes quantités d’espace disque.) [Générer des rapports à la demande](generate-reports-on-demand.md)

3. Passez en revue les modèles de quotas préconfigurés disponibles. (Dans **gestion de quota**, cliquez sur le nœud **modèles de quota** .) [Modifier les propriétés du modèle de Quota](edit-quota-template-properties.md) 
<br />\- Ou - <br /> Créez un nouveau modèle de quota pour appliquer une stratégie de stockage dans votre organisation. [Créer un modèle de quota](create-quota-template.md)

4. Créez un quota basé sur le modèle sur le volume ou le dossier.  
 [Créer un quota](create-quota.md) <br /> \- Ou - <br /> Créez un quota automatique pour générer automatiquement des quotas pour les sous-dossiers sur le volume ou le dossier. [Créer un quota automatique](create-auto-apply-quota.md)

6. Planifiez une tâche de rapport contenant un rapport d’utilisation du quota pour surveiller l’utilisation du quota régulièrement. [Planifier un ensemble de rapports](schedule-set-of-reports.md)

> [!Note]
> Si vous souhaitez filtrer des fichiers sur un volume ou un dossier, voir [Checklist : Appliquez un écran de fichier à un volume ou à un dossier @ no__t-0.











