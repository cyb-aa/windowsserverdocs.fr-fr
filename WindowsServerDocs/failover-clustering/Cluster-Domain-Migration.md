---
title: Dépassez la Migration de domaine de Cluster dans Windows Server 2016/2019
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/18/2019
description: Cet article décrit le déplacement d’un cluster Windows Server 2019 d’un domaine à un autre
ms.localizationpriority: medium
ms.openlocfilehash: bcfd458c94d33820f434cde3313dc069fc42ffd9
ms.sourcegitcommit: 21677706eb85cb1396c1f40bf443146c09ef1b0d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/31/2019
ms.locfileid: "9042033"
---
# Migration de domaine de Cluster de basculement

> S’applique à: Windows Server 2019, Windows Server 2016

Cette rubrique fournit qu'une vue d’ensemble pour déplacer le basculement de Windows Server clusters d’un domaine à l’autre.

## Pourquoi migrer entre les domaines

Il existe plusieurs scénarios où il est nécessaire de migration d’un cluster à partir d’un domaine à un autre.

- CompanyA fusionne avec la société Société_b et devez déplacer tous les clusters dans le domaine CompanyA
- Les clusters sont intégrées dans le centre de données principal et fournies vers des emplacements distants
- Cluster a été conçu en tant qu’un cluster de groupe de travail et désormais doit faire partie d’un domaine
- Cluster a été conçu en tant que domaine cluster et désormais doit faire partie d’un groupe de travail
- Cluster est déplacé vers une zone de la société à l’autre et est un sous-domaine différents

Microsoft ne prend pas en charge pour les administrateurs qui tentez de déplacer des ressources à partir d’un domaine à l’autre si l’opération d’application sous-jacente est non pris en charge. Par exemple, Microsoft ne prend pas en charge pour les administrateurs qui tentez de déplacer un serveur Microsoft Exchange à partir d’un domaine à l’autre.

   > [!WARNING]
   > Nous vous conseillons d’effectuer une sauvegarde complète de stockage tous partagé du cluster avant de déplacer le cluster.

## Windows Server 2016 et versions antérieur

Dans Windows Server 2016 et versions antérieures, le service de Cluster n’a pas ont la possibilité de déplacement d’un domaine à l’autre.  Il s’agissait en raison de la dépendance accrue sur les Services de domaine Active Directory et les noms virtuels créés.   

## Options

Pour ce faire une telle opération, il existe deux options.

La première option implique la destruction du cluster et recréer dans le nouveau domaine.

![Détruire et régénérer](media\Cross-Domain-Cluster-Migration\Cross-Cluster-Domain-Migration-1.gif)

Comme indiqué dans l’animation, cette option est destructrice en suivant les étapes en cours:

1. Détruire le Cluster.
2. Modifier l’appartenance au domaine des nœuds dans le nouveau domaine.
3. Recréer le Cluster en tant que nouvelle dans le domaine mis à jour.  Cela entraînerait avoir à recréer toutes les ressources.

La deuxième option est moins destructrice, mais nécessite un matériel supplémentaire comme un nouveau cluster doivent être généré dans le nouveau domaine.  Une fois que le cluster se trouve dans le nouveau domaine, exécutez l’Assistant de Migration de Cluster à migrer les ressources. Notez que cela ne prend pas migrer les données, vous devez utiliser un autre outil de migration des données, par exemple, le [Service de Migration du stockage](../storage/storage-migration-service/overview.md)(une fois que la prise en charge du cluster est ajouté).

![Créer et migrer](media\Cross-Domain-Cluster-Migration\Cross-Cluster-Domain-Migration-2.gif)

Comme indiqué dans l’animation, cette option n’est pas destructrice, mais nécessite un matériel différent ou un nœud du cluster existant qu’a été supprimé.

1. Créez un nouveau CLUSTÉRINE le nouveau domaine tout en conservant l’ancien cluster disponible.
2. Utilisez l' [Assistant de Migration de Cluster](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754481(v=ws.10)) à migrer toutes les ressources vers le nouveau cluster. Rappel, cela ne copie pas les données, de manière devra être effectuée séparément.
3. Retirer ou supprimer l’ancien cluster.

Dans les deux options, le nouveau cluster doivent disposer de toutes les [applications adaptée aux clusters](https://technet.microsoft.com/aa369082(v=vs.90)) installé, la mise à jour, pilotes, et éventuellement de test pour vous assurer que tous les s’exécutera correctement.  Il s’agit d’un processus beaucoup de temps si les données doivent également être déplacés.

## Windows Server2019

Dans Windows Server 2019, nous avons introduit des capacités de migration de domaine de cluster croisée.  Maintenant, les scénarios ci-dessus peuvent facilement être effectuées et la nécessité de reconstruction n’est plus nécessaire.  

Déplacement d’un cluster d’un domaine est un processus simple. Pour ce faire, il existe deux nouvelles applets de commande PowerShell.

**New-ClusterNameAccount** – crée un compte de nom de Cluster dans Active Directory **Remove-ClusterNameAccount** – supprime les comptes de nom de Cluster dans Active Directory

Le processus pour effectuer cette opération consiste à modifier le cluster à partir d’un domaine à un groupe de travail et vers le nouveau domaine.  La nécessité de détruire un cluster, générez à nouveau un cluster, installer des applications, etc. n’est pas obligatoire. Par exemple, cela se présenterait comme suit:

![Migrer](media\Cross-Domain-Cluster-Migration\Cross-Cluster-Domain-Migration-3.gif)

## Migration d’un cluster vers un nouveau domaine

Dans les étapes suivantes, un cluster est déplacé à partir du domaine Contoso.com vers le nouveau domaine Fabrikam.com.  Le nom du cluster est *CLUSCLUS* et avec un rôle de serveur de fichiers appelé *FS-CLUSCLUS*.

1. Créez un compte administrateur local avec le même nom et mot de passe sur tous les serveurs du cluster.  Cela peut être nécessaire pour se connecter, tandis que les serveurs sont déplacement entre des domaines.
2. Connectez-vous au premier serveur avec un compte d’utilisateur ou administrateur de domaine disposant des autorisations Active Directory pour le Cluster nom objet (CNO), les objets d’ordinateur virtuel (VCO), a accès au Cluster et ouvrez PowerShell.
3. Vérifier que toutes les ressources de nom réseau de Cluster sont dans l’état et s’exécute en mode hors connexion la ci-dessous commande.  Cette commande supprime les objets Active Directory que le cluster peut avoir.

   ```PowerShell
   Remove-ClusterNameAccount -Cluster CLUSCLUS -DeleteComputerObjects
   ```
4. Utilisez Active Directory utilisateurs et ordinateurs pour vous assurer de l’ordinateur CNO et VCO associés à tous les noms en cluster ont été supprimés.

   > [!NOTE]
   > Il est judicieux de les arrêter le service de Cluster sur tous les serveurs du cluster et définissez le type de démarrage du service manuel afin que le service de Cluster ne démarre pas lorsque le redémarrage automatique des serveurs lors de la modification des domaines.

   ```PowerShell
   Stop-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Manual
   ```

5. Modifier l’appartenance au domaine de serveurs à un groupe de travail, redémarrer les serveurs, joindre les serveurs vers le nouveau domaine et relancez.
6. Une fois que les serveurs sont dans le nouveau domaine, se connecter à un serveur avec un compte d’utilisateur ou administrateur de domaine qui a des autorisations Active Directory pour créer des objets, qui a accès au Cluster et ouvrez PowerShell. Démarrez le Service de Cluster et définissez-le sur automatique.

   ```PowerShell
   Start-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Automatic
   ```
7. Importer le nom du Cluster et tous les autres cluster réseau nommer des ressources dans un état en ligne.

   ```PowerShell
   Start-ClusterResource -Name "Cluster Name"

   Start-ClusterResource -Name FS-CLUSCLUS
   ```

8. Modifier le cluster pour composer le nouveau domaine avec des objets active directory associés. Pour ce faire, la commande est ci-dessous et les ressources de nom réseau doivent être dans un état en ligne.  Cette commande fera est recréer les nom des objets dans Active Directory.

   ```PowerShell
   New-ClusterNameAccount -Name CLUSTERNAME -Domain NEWDOMAINNAME.com -UpgradeVCOs
   ```

    Remarque: Si vous ne disposez pas des groupes supplémentaires avec des noms de réseau (autrement dit, un Cluster Hyper-V avec uniquement des ordinateurs virtuels), le commutateur de paramètre - UpgradeVCOs n’est pas nécessaire.

9. Utilisez Active Directory utilisateurs et ordinateurs pour vérifier le nouveau domaine et vérifiez que les objets ordinateur associé ont été créés. S’ils disposent, puis placez les autres ressources dans les groupes en ligne.

   ```PowerShell
   Start-ClusterGroup -Name "Cluster Group"

   Start-ClusterGroup -Name FS-CLUSCLUS
   ```

## Problèmes connus

Si vous utilisez la nouvelle fonctionnalité de témoin USB, vous ne pourrez pas ajouter le cluster vers le nouveau domaine.  La logique est que le type de témoin de partage de fichier doit utiliser Kerberos pour l’authentification.  Modifier le rappel None avant d’ajouter le cluster au domaine.  Une fois qu’elle est terminée, recréez le témoin USB.  L’erreur que vous voyez est le suivant:

```
New-ClusternameAccount : Cluster name account cannot be created.  This cluster contains a file share witness with invalid permissions for a cluster of type AdministrativeAccesssPoint ActiveDirectoryAndDns. To proceed, delete the file share witness.  After this you can create the cluster name account and recreate the file share witness.  The new file share witness will be automatically created with valid permissions.
```

