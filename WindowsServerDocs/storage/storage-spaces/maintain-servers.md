---
title: Mise hors connexion d'un serveur d'espaces de stockage direct pour la maintenance
ms.prod: windows-server
ms.author: eldenc
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: eldenchristensen
ms.date: 10/08/2018
Keywords: Espaces de stockage directs, S2D, maintenance
ms.assetid: 73dd8f9c-dcdb-4b25-8540-1d8707e9a148
ms.localizationpriority: medium
ms.openlocfilehash: 20439a06c255a73f20a297f765e6ed11abfde6f2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402831"
---
# <a name="taking-a-storage-spaces-direct-server-offline-for-maintenance"></a>Mise hors connexion d'un serveur d'espaces de stockage direct pour la maintenance

> S’applique à : Windows Server 2019, Windows Server 2016

Cette rubrique fournit des conseils sur le redémarrage ou l'arrêt correct de serveurs avec [espaces de stockage direct](storage-spaces-direct-overview.md).

Avec les espaces de stockage direct, la mise hors connexion d'un serveur (son extinction) signifie également la mise hors connexion de parties du stockage qui est partagé entre tous les serveurs du cluster. Cela nécessite la mise en pause (interruption) du serveur à mettre hors connexion, le déplacement des rôles vers d’autres serveurs du cluster et la vérification que toutes les données sont disponibles sur les autres serveurs du cluster afin que les données soient protégées et accessibles pendant la maintenance.

Utilisez les procédures suivantes pour suspendre correctement un serveur dans un cluster d’espaces de stockage direct avant la mise hors connexion. 

   > [!IMPORTANT]
   > Pour installer des mises à jour sur un cluster d’espaces de stockage direct, utilisez Mise à jour adaptée aux clusters, qui effectue automatiquement les procédures décrites dans cette rubrique à votre place lors de l’installation des mises à jour. Pour plus d’informations, voir [Mise à jour adaptée aux clusters](https://technet.microsoft.com/library/hh831694.aspx).

## <a name="verifying-its-safe-to-take-the-server-offline"></a>Vérification que la mise hors connexion du serveur est sûre

Avant de mettre un serveur hors connexion pour maintenance, vérifiez que tous vos volumes sont sains.

Pour ce faire, ouvrez une session PowerShell avec des autorisations Administrateur, puis exécutez la commande suivante pour afficher le statut du volume :

```PowerShell
Get-VirtualDisk 
```

Voici un exemple de ce à quoi peut ressembler la sortie :
```
FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                OK                Healthy      True           1 TB
MyVolume2    Mirror                OK                Healthy      True           1 TB
MyVolume3    Mirror                OK                Healthy      True           1 TB
```

Vérifiez que la propriété **HealthStatus** de chaque volume (disque virtuel) est **Sain**.

Pour ce faire dans le Gestionnaire du cluster de basculement, accédez à **Stockage** > **Disques**.

Vérifiez que la colonne  **Statut**pour chaque volume (disque virtuel) indique **En ligne**.

## <a name="pausing-and-draining-the-server"></a>Mise en pause et drainage du serveur

Avant le redémarrage ou l’arrêt du serveur, la mise en pause et le drainage (déplacement) de tous les rôles, tels que les ordinateurs virtuels en cours d’exécution sur ce dernier. Cela permet également aux espaces de stockage direct de vider et valider des données de la façon appropriée pour vous assurer que l’arrêt est transparent pour toutes les applications en cours d’exécution sur ce serveur.

   > [!IMPORTANT]
   > Mettez toujours en pause et drainez toujours les serveurs en cluster avant de les redémarrer ou de les arrêter.

Dans PowerShell, exécutez l’applet de commande suivante (en tant qu’administrateur) pour mettre en pause et drainer.

```PowerShell
Suspend-ClusterNode -Drain
```

Pour ce faire dans le Gestionnaire du cluster de basculement, accédez à **Nœuds**, cliquez avec le bouton droit sur le nœud, puis sélectionnez **Pause** > **Drainer les rôles**.

![Mettre le drainage en pause](media/maintain-servers/pause-drain.png)

Tous les ordinateurs virtuels vont commencer la migration dynamique vers d’autres serveurs du cluster. Ce processus peut prendre quelques minutes.

   > [!NOTE]
   > Lorsque vous mettez en pause et drainez le nœud de cluster correctement, Windows effectue une vérification de sécurité automatique pour s'assurer que la procédure peut se poursuivre en toute sécurité. Si des volumes ne sont pas intègres, il arrête et vous avertit qu’il n’est pas sûr de continuer.

![Vérification de sécurité](media/maintain-servers/safety-check.png)

## <a name="shutting-down-the-server"></a>Arrêt du serveur

Une fois que le serveur a terminé le drainage, il est indiqué comme **En pause** dans le Gestionnaire du cluster de basculement et PowerShell.

![Suspendu](media/maintain-servers/paused.png)

Vous pouvez maintenant le redémarrer ou l'arrêter en toute sécurité, comme vous le feriez normalement (par exemple, en utilisant les applets de commande PowerShell Restart-Computer ou Stop-Computer).

```PowerShell
Get-VirtualDisk 

FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                Incomplete        Warning      True           1 TB
MyVolume2    Mirror                Incomplete        Warning      True           1 TB
MyVolume3    Mirror                Incomplete        Warning      True           1 TB
```

L’état opérationnel incomplet ou détérioré est normal lorsque les nœuds sont en cours d’arrêt ou de démarrage/arrêt du service de cluster sur un nœud et ne doivent pas poser de problème. Tous vos volumes restent en ligne et accessibles.

## <a name="resuming-the-server"></a>Reprise du serveur

Lorsque le serveur est prêt à recommencer à héberger des charges de travail, faites-le reprendre.

Dans PowerShell, exécutez l’applet de commande suivante (en tant qu’administrateur) pour reprendre.

```PowerShell
Resume-ClusterNode
```

Pour redéplacer les rôles exécutés précédemment sur ce serveur, utilisez l’indicateur **-Restauration automatique** facultatif.

```PowerShell
Resume-ClusterNode –Failback Immediate
```

Pour ce faire dans le Gestionnaire du cluster de basculement, accédez à **Nœuds**, cliquez avec le bouton droit sur le nœud, puis sélectionnez **Reprendre** > **Restaurer les rôles**.

![Reprendre-Restauration automatique](media/maintain-servers/resume-failback.png)

## <a name="waiting-for-storage-to-resync"></a>En attente de resynchronisation du stockage

Lorsque le serveur reprend, toute nouvelle écriture qui s’est produite alors qu’il n’était pas disponible doit être resynchronisée. Cela se fait de manière automatique. Grâce au suivi des modifications intelligent, il n’est pas nécessaire d'analyse ou de synchroniser *toutes les* données, mais uniquement les modifications. Ce processus est limité pour atténuer l’impact sur les charges de travail de production. En fonction de la durée pendant laquelle le serveur a été mis en pause et de la quantité de nouvelles données écrites, plusieurs minutes peuvent être nécessaires.

Vous devez attendre la fin de la resynchronisation avant de mettre d’autres serveurs du cluster hors connexion.

Dans PowerShell, exécutez l’applet de commande suivante (en tant qu’administrateur) pour surveiller la progression.

```PowerShell
Get-StorageJob
```
Voici quelques exemples de sortie indiquant les tâches de resynchronisation (réparation) :
```
Name   IsBackgroundTask ElapsedTime JobState  PercentComplete BytesProcessed BytesTotal
----   ---------------- ----------- --------  --------------- -------------- ----------
Repair True             00:06:23    Running   65              11477975040    17448304640
Repair True             00:06:40    Running   66              15987900416    23890755584
Repair True             00:06:52    Running   68              20104802841    22104819713
```

**BytesTotal** montre la quantité de stockage devant être resynchronisé. **PercentComplete** affiche la progression.

   > [!WARNING]
   > Il est déconseillé de mettre un autre serveur hors connexion tant que ces tâches de réparation ne sont pas terminées.

Pendant ce temps, vos volumes sont toujours marqués comme **Avertissement**, ce qui est normal. 

Par exemple, si vous utilisez l'applet de commande `Get-VirtualDisk`, vous pouvez voir la sortie suivante :
```
FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                InService         Warning      True           1 TB
MyVolume2    Mirror                InService         Warning      True           1 TB
MyVolume3    Mirror                InService         Warning      True           1 TB
```

Une fois les tâches terminées, vérifiez que les volumes sont de nouveau marqués comme **Sains** à l’aide de l'applet de commande `Get-VirtualDisk`. Voici quelques exemples de sortie :

```
FriendlyName ResiliencySettingName OperationalStatus HealthStatus IsManualAttach Size
------------ --------------------- ----------------- ------------ -------------- ----
MyVolume1    Mirror                OK                Healthy      True           1 TB
MyVolume2    Mirror                OK                Healthy      True           1 TB
MyVolume3    Mirror                OK                Healthy      True           1 TB
```

Vous pouvez maintenant mettre en pause et redémarrer les autres serveurs du cluster en toute sécurité.

## <a name="how-to-update-storage-spaces-direct-nodes-offline"></a>Mise à jour des nœuds espaces de stockage direct hors connexion
Suivez les étapes ci-dessous pour suivre rapidement votre espaces de stockage direct système. Cela implique la planification d’une fenêtre de maintenance et la mise en service du système pour la mise à jour corrective. Si vous avez besoin d’appliquer rapidement une mise à jour de sécurité critique ou si vous devez vous assurer que la mise à jour corrective est terminée dans votre fenêtre de maintenance, cette méthode peut vous être utile. Ce processus met en avant le cluster espaces de stockage direct, le corrige, puis le réintègre. Le compromis est le temps d’arrêt des ressources hébergées.

1. Planifiez votre fenêtre de maintenance.
2. Mettre les disques virtuels hors connexion.
3. Arrêtez le cluster pour mettre le pool de stockage hors connexion. Exécutez l’applet de commande **Stop-cluster** ou utilisez gestionnaire du cluster de basculement pour arrêter le cluster.
4. Définissez le service de cluster sur **désactivé** dans services. msc sur chaque nœud. Cela empêche le service de cluster de démarrer pendant la mise à jour corrective.
5. Appliquez la mise à jour cumulative de Windows Server et toutes les mises à jour de pile de maintenance requises pour tous les nœuds. (Vous pouvez mettre à jour tous les nœuds en même temps, sans attendre que le cluster ne soit en cours).  
6. Redémarrez les nœuds et assurez-vous que tout fonctionne correctement.
7. Réactivez le service de cluster sur **automatique** sur chaque nœud.
8. Démarrez le cluster. Exécutez l’applet de commande **Start-Cluster** ou utilisez gestionnaire du cluster de basculement. 

   Veuillez le faire quelques minutes.  Assurez-vous que le pool de stockage est sain.
9. Remettez les disques virtuels en ligne.
10. Surveillez l’état des disques virtuels en exécutant les applets de commande **« obtient-volume »** et **« VirtualDisk »** .


## <a name="see-also"></a>Voir aussi

- [Présentation de espaces de stockage direct](storage-spaces-direct-overview.md)
- [Mise à jour adaptée aux clusters](https://technet.microsoft.com/library/hh831694.aspx)
