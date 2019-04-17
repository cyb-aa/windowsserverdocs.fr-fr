---
title: "Appliquer un quota à un volume ou un dossier"
description: "Cet article explique comment appliquer un quota de stockage à un volume ou un dossier"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 62910af666fb16db5c2e7a30b49eedfa8c12cacb
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="checklist-apply-a-quota-to-a-volume-or-folder"></a>Liste de vérification: appliquer un quota à un volume ou un dossier

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2

1. Configurez les paramètres de messagerie si vous prévoyez d’envoyer des notifications de seuil ou des rapports de stockage par courrier électronique. [Configurez les notifications par courrier électronique](configure-email-notifications.md)

2. Évaluez les besoins de stockage sur le volume ou le dossier. Vous pouvez utiliser des rapports sur le nœud **Gestion des rapports de stockage** pour fournir des données. (Par exemple, exécutez un rapport Fichiers par propriétaire à la demande pour identifier les utilisateurs qui utilisent un espace disque important.) [Générez des rapports à la demande](generate-reports-on-demand.md)

3. Passez en revue les modèles de quotas préconfigurés disponibles. (Dans **Gestion de quota**, cliquez sur le nœud **Modèles de quotas**.) [Modifiez les propriétés du modèle de quota](edit-quota-template-properties.md) 
<br />- Ou - <br /> Créez un nouveau modèle de quota pour appliquer une stratégie de stockage dans votre organisation. [Créez un modèle de quota](create-quota-template.md)

4. Créez un quota basé sur le modèle sur le volume ou le dossier.  
 [Créez un quota](create-quota.md) <br /> - Ou - <br /> Créez un quota automatique pour générer automatiquement des quotas pour les sous-dossiers sur le volume ou le dossier. [Créez un quota automatique](create-auto-apply-quota.md)

6. Planifiez une tâche de rapport contenant un rapport d’utilisation du quota pour surveiller l’utilisation du quota régulièrement. [Planifiez un ensemble de rapports](schedule-set-of-reports.md)

> [!Note]
> Si vous souhaitez filtrer les fichiers sur un volume ou un dossier, consultez [Liste de vérification: appliquer un filtre de fichiers à un volume ou un dossier](checklist-apply-file-screen-to-volume-or-folder.md).











