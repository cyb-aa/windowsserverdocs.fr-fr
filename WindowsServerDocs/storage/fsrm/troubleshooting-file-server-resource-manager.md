---
title: Résolution des problèmes du Gestionnaire de ressources du serveur de fichiers
description: Cet article explique comment résoudre les problèmes courants lors de l’utilisation du Gestionnaire de ressources de serveur de fichiers
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 413cf51e5ceb1c4507b71fb77ee6005807a0ff13
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476179"
---
# <a name="troubleshooting-file-server-resource-manager"></a>Résolution des problèmes du Gestionnaire de ressources du serveur de fichiers

> S’applique à : Windows Server 2019, Windows Server 2016, Windows Server (canal semi-annuel), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Cette section répertorie les problèmes courants que vous pouvez rencontrer en utilisant le Gestionnaire de ressources de serveur de fichiers.

> [!Note]
> Une bonne option de résolution des problèmes consiste à rechercher les journaux des événements qui ont été générés par le Gestionnaire de ressources du serveur de fichiers. Toutes les entrées de journal des événements du Gestionnaire de ressources du serveur de fichiers se trouvent dans le journal des événements **Application** sous la source **SRMSVC**

## <a name="i-am-not-receiving-e-mail-notifications"></a>Je ne reçois pas de notifications par courrier électronique.

-   **Cause** : Les options de messagerie n’ont pas été configurées ou ont été configurées de manière incorrecte.

-   **Solution** : Sur le **Notifications par courrier électronique** sous l’onglet le **File Server Resource Manager Options** boîte de dialogue, vérifiez que le serveur SMTP et les destinataires du courrier électronique par défaut ont été spécifiés et sont valides. Envoyez un message électronique de test pour vérifier que les informations sont correctes et que le serveur SMTP utilisé pour envoyer les notifications fonctionne correctement. Pour plus d'informations, voir [Configurer les notifications par courrier électronique](configure-email-notifications.md).


## <a name="i-am-only-receiving-one-e-mail-notification-even-though-the-event-that-triggered-that-notification-happened-several-times-in-a-row"></a>Je ne reçois qu'une seule notification par courrier électronique, même si l’événement ayant déclenché la notification s'est produit plusieurs fois d'affilée.

-   **Cause** : Lorsqu’un utilisateur tente plusieurs fois pour enregistrer un fichier qui est bloqué ou pour enregistrer un fichier qui dépasse un seuil de quota, il est une notification par courrier électronique configurée pour ce fichier de filtrage ou événement de quota, uniquement un message électronique est envoyé à l’administrateur sur une période de 60 minutes par  par défaut. Cela évite une multitude de messages redondants dans le compte de messagerie de l’administrateur.

-   **Solution** : Sur le **limites de Notification** sous l’onglet le **File Server Resource Manager Options** boîte de dialogue, vous pouvez définir une limite de temps pour chacun des types de notification : courrier électronique, journal des événements, commande et rapport. Chaque limite spécifie un délai avant qu'une autre notification du même type ne soit générée pour un problème identique. Pour plus d'informations, voir [Configurer les limites de notification](configure-notification-limits.md).


## <a name="my-storage-reports-keep-failing-and-little-or-no-information-is-available-in-the-event-log-regarding-the-source-of-the-failure"></a>Mes rapports de stockage échouent à chaque fois et peu ou aucune information n’est disponible dans le journal des événements sur l’origine de l’échec.

-   **Cause** : Le volume où les rapports sont enregistrés est peut-être endommagé.
-   **Solution** : Exécutez **chkdsk** sur ce volume et réessayez de générer les rapports.

## <a name="my-file-screening-audit-reports-do-not-contain-any-information"></a>Mes rapports de vérification du filtrage des fichiers ne contiennent aucune information.

-   **Cause** : Un ou plusieurs des éléments suivants peuvent être la cause :
    -   La base de données de vérification n’est pas configurée pour enregistrer l’activité de filtrage de fichiers.
    -   La base de données de vérification est vide (autrement dit, aucune activité de filtrage de fichiers n’a été enregistrée).
    -   Les paramètres du rapport de vérification du filtrage des fichiers ne sélectionnent pas les données de la base de données de vérification.
    
-   **Solution** : Sur le **vérification du filtrage de fichier** sous l’onglet le **File Server Resource Manager Options** boîte de dialogue, vérifiez que le **enregistrer l’activité dans la base de données audit de filtrage de fichiers** vérifier est activée.
    -   Pour plus d’informations sur l’enregistrement d’activité de filtrage de fichiers, voir [Configurer la vérification du filtrage de fichiers](configure-file-screen-audit.md).
    -   Pour configurer les paramètres par défaut du rapport de vérification du filtrage de fichiers, voir [Configurer les rapports de stockage](configure-storage-reports.md).
    -   Pour modifier les paramètres de rapport pour les tâches de rapport planifiées ou les rapports à la demande, voir [Gestion des rapports de stockage](storage-reports-management.md).

## <a name="the-used-and-available-values-for-some-of-the-quotas-i-have-created-do-not-correspond-to-the-actual-limit-setting"></a>Les valeurs « Utilisées » et « Disponibles » de certains des quotas que j’ai créés ne correspondent pas au paramètre de « Limite » réel.

-   **Cause** : Vous pouvez avoir un quota imbriqué, où le quota d’un sous-dossier dérive une limite plus restrictive le quota de son dossier parent. Par exemple, il se peut qu'une limite de quota de 100 mégaoctets (Mo) soit appliquée à un dossier parent et qu'un quota de 200 Mo soit appliqué de façon distincte à chacun de ses sous-dossiers. Si le dossier parent contient un total de 50 Mo de données (somme des données stockées dans ses sous-dossiers), chacun des sous-dossiers répertorie seulement 50 Mo d’espace disque disponible.

-   **Solution** : Sous **gestion de Quota**, cliquez sur **Quotas**. Dans le volet **Résultats**, sélectionnez l’entrée de quota à traiter. Dans le volet **Actions**, cliquez sur **Afficher les quotas appliqués au dossier**, puis recherchez les quotas appliqués aux dossiers parents. Cela vous permettra d’identifier les quotas de dossier parent qui ont une limite de stockage inférieure au paramètre du quota sélectionné.

