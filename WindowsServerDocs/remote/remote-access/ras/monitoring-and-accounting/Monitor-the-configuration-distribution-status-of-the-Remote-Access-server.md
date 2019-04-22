---
title: Analyser l'état de distribution de la configuration du serveur d'accès à distance
description: Cette rubrique fait partie du guide de la surveillance de l’accès à distance et la gestion des comptes dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de285d13-9e54-4c46-88f0-607182e5e3dc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3171eb9e9c90d0688fa413b80d9dbbf162e77fe8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818420"
---
# <a name="monitor-the-configuration-distribution-status-of-the-remote-access-server"></a>Analyser l'état de distribution de la configuration du serveur d'accès à distance

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

**Remarque :** Windows Server 2012 associe DirectAccess et le Service d’accès à distance (RAS) en un seul rôle accès à distance.  
  
La console de gestion de l'accès distant compare les versions de la configuration de tous les serveurs analysés pour vérifier qu'ils disposent de la version la plus récente et qu'ils l'utilisent. Elle indique si la dernière version de la configuration (spécifiée dans les objets de stratégie de groupe) a été distribuée à tous les serveurs et si elle a correctement été appliquée sur les serveurs.  
  
### <a name="to-use-the-monitoring-dashboard-to-monitor-the-configuration-distribution"></a>Pour utiliser le tableau de bord d'analyse dans le but de vérifier la distribution de la configuration  
  
1.  Dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **Gestion de l'accès à distance**.  
  
2.  Cliquez sur **TABLEAU DE BORD** pour accéder à **Tableau de bord des accès distants** dans la **Console de gestion de l'accès distant**.  
  
3.  Dans le tableau de bord d'analyse, intéressez-vous à la vignette **État de configuration** située en haut au centre. Cette vignette indique l'état actuel de la distribution de la configuration  
  
Le tableau suivant montre les messages générés par la vignette **État de configuration**, leur signification et l'action d'administration requise (le cas échéant).  
  
|||||  
|-|-|-|-|  
|Sévérité|Message|Signification|Que faire ?|  
|Opération réussie|La configuration a été distribuée correctement.|La configuration spécifiée dans l'objet de stratégie de groupe a correctement été appliquée sur le serveur.|Aucune action nécessaire.|  
|Warning|La configuration pour le serveur [*nom du serveur*] n'a pas été récupérée auprès du contrôleur de domaine. L'objet de stratégie de groupe n'est pas lié.|La configuration spécifiée dans l'objet de stratégie de groupe n'a pas encore atteint le serveur. Peut-être parce que l'objet de stratégie de groupe n'est pas lié au serveur.|Liez l'objet de stratégie de groupe à une étendue de gestion qui est appliquée au serveur, ou dans un scénario d'objet de stratégie de groupe intermédiaire, exportez manuellement les paramètres de l'objet de stratégie de groupe intermédiaire et importez-les dans l'objet de stratégie de groupe de production. Pour plus d’informations sur la stratégie de groupe de mise en lots, consultez **la gestion de GPO des accès à distance avec des autorisations limitées** dans [Step-1-Plan-the-DirectAccess-Infrastructure](../../directaccess/single-server-advanced/Step-1-Plan-the-DirectAccess-Infrastructure.md). Pour connaître les étapes intermédiaires de GPO, consultez **configuration de GPO de l’accès à distance avec des autorisations limitées** dans [étape 1 : Configurer l’Infrastructure DirectAccess](../../directaccess/single-server-advanced/Step-1-Configuring-DirectAccess-Infrastructure.md).|  
|Warning|La configuration pour le serveur [*nom du serveur*] n'a pas encore été récupérée auprès du contrôleur de domaine.|La configuration spécifiée dans l'objet de stratégie de groupe n'a pas encore atteint le serveur.<br /><br />Il peut falloir jusqu'à 10 minutes pour propager une nouvelle configuration.|Laissez plus de temps aux stratégies pour se mettre à jour sur le serveur.|  
|Erreur|Impossible de récupérer la configuration pour le serveur [*nom du serveur*] auprès du contrôleur de domaine.|La configuration spécifiée dans l'objet de stratégie de groupe n'a pas atteint le serveur et plus de 10 minutes se sont écoulées depuis la modification de la configuration.|Cette erreur peut se produire dans l'un des scénarios suivants :<br /><br />-Le serveur ne dispose d’aucune connectivité au domaine pour mettre à jour les stratégies. Vous pouvez exécuter « gpupdate /force » sur le serveur pour forcer une mise à jour de la stratégie.<br />-Réplication de GPO peut être nécessaire pour récupérer la configuration mise à jour.<br />-Il n’existe aucun contrôleur de domaine accessible en écriture dans le site Active Directory du serveur d’accès à distance.<br /><br />Attendez que la réplication des objets de stratégie de groupe sur tous les contrôleurs de domaine soit terminée, puis utilisez l'applet de commande Windows PowerShell **Set-DAEntryPointDC** pour associer le point d'entrée à un contrôleur de domaine accessible en écriture dans Active Directory sur le serveur d'accès à distance.|  
|Warning|La configuration pour le serveur [*nom du serveur*] a été récupérée auprès du contrôleur de domaine mais n’est pas encore appliquée.|La configuration spécifiée dans l'objet de stratégie de groupe a atteint le serveur, mais n'a pas encore été appliquée.<br /><br />Il peut falloir jusqu'à 15 minutes pour appliquer la configuration.|Laissez plus de temps à la configuration pour être complètement appliquée au serveur.|  
|Erreur|La configuration pour le serveur [*nom du serveur*] récupérée auprès du contrôleur de domaine ne peut pas être appliquée.|La configuration spécifiée dans l'objet de stratégie de groupe a atteint le serveur, mais elle n'est pas correctement appliquée et plus de 15 minutes se sont écoulées depuis la modification de la configuration.|Cette erreur peut se produire dans l'un des scénarios suivants :<br /><br />1.  La configuration est en cours d'application. Ce scénario est considéré comme une erreur, car il aura fallu beaucoup de temps pour récupérer la configuration spécifiée dans l'objet de stratégie de groupe.<br />    Pour vérifier si cela est bien le cas, utilisez le **Planificateur de tâches** et accédez à Microsoft\Windows\RemoteAccess pour vérifier que **RAConfigTask** est en cours d'exécution.<br />2.  Si **RAConfigTask** n'est pas en cours d'exécution, l'application de la configuration sur le serveur a peut-être échoué.<br />    Vérifiez si des erreurs existent dans l'**Observateur d’événements** sous le canal des opérations d'accès à distance, situé dans \Journaux des applications et des services\Microsoft\Windows\RemoteAccess-RemoteAccessServer.<br />    Vérifiez si des erreurs existent dans **ÉTAT DES OPÉRATIONS** dans la console de gestion de l'accès distant. Pour plus d'informations, consultez [Analyser l'état de fonctionnement du serveur d'accès à distance et de ses composants](Monitor-the-operations-status-of-the-Remote-Access-server-and-its-components.md).|  
|Erreur|La configuration des serveurs multisites a été récupérée à partir du contrôleur de domaine, mais elle n'est pas la même pour tous les serveurs.|Il existe une incohérence entre les versions de la configuration des objets de stratégie de groupe du serveur dans le déploiement multisite.<br /><br />Idéalement, tous les objets de stratégie de groupe du serveur pour tous les points d'entrée ont la même configuration globale, mais, pour une raison ou une autre, ils ne sont pas synchronisés.|Cette erreur peut se produire quand une modification de la configuration a échoué et n'a pas été restaurée correctement.<br /><br />Vous devez restaurer les objets de stratégie de groupe du serveur à partir d'un état de sauvegarde dans lequel ils sont tous synchronisés. Pour plus d’informations sur le script que vous pouvez utiliser, consultez [Sauvegarder et restaurer la configuration de l’accès à distance](https://gallery.technet.microsoft.com/Back-up-and-Restore-Remote-e157e6a6).|  
  


