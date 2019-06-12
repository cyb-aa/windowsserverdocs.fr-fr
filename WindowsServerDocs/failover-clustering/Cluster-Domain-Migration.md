---
title: Migration de Cluster de domaine dans Windows Server 2016/2019 entre
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/18/2019
description: Cet article décrit le déplacement d’un cluster Windows Server 2019 d’un domaine vers un autre
ms.localizationpriority: medium
ms.openlocfilehash: 1054de942e807f00586903683faeaf695ec2f033
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452930"
---
# <a name="failover-cluster-domain-migration"></a>Migration de domaine de Cluster de basculement

> S’applique à : Windows Server 2019, Windows Server 2016

Cette rubrique fournit qu'une vue d’ensemble pour le déplacement de basculement Windows Server clusters à partir d’un domaine vers un autre.

## <a name="why-migrate-between-domains"></a>Pourquoi migrer entre des domaines

Il existe plusieurs scénarios où il est nécessaire de migration d’un cluster à partir d’un domaine à un autre.

- CompanyA fusionne avec la société Société_b et devez déplacer tous les clusters dans le domaine de CompanyA
- Les clusters sont créées dans le centre de données principal et expédiés en vers des emplacements distants
- Cluster a été créé comme un cluster de groupe de travail et doit maintenant faire partie d’un domaine
- Cluster a été créé comme un cluster de domaine et doit maintenant faire partie d’un groupe de travail
- Cluster est déplacé vers une zone de la société vers un autre et est un autre sous-domaine

Microsoft ne fournit pas prise en charge pour les administrateurs qui essaient de se déplacer des ressources d’un domaine vers un autre si l’opération d’application sous-jacente est non pris en charge. Par exemple, Microsoft ne fournit pas prise en charge pour les administrateurs qui essaient de se déplacer d’un serveur Microsoft Exchange d’un domaine vers un autre.

   > [!WARNING]
   > Nous vous recommandons d’effectuer une sauvegarde complète de stockage partagé dans le cluster avant de déplacer le cluster.

## <a name="windows-server-2016-and-earlier"></a>Windows Server 2016 et versions antérieur

Dans Windows Server 2016 et versions antérieures, le service de Cluster n’a pas avoir la capacité de déplacement d’un domaine vers un autre.  Ceci est dû à la dépendance accrue de Services de domaine Active Directory et les noms virtuels créés.   

## <a name="options"></a>Options

Pour effectuer ce déplacement, il existe deux options.

La première option implique la destruction du cluster et la reconstruction dans le nouveau domaine.

![Détruire et recréer](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-1.gif)

Comme le montre l’animation, cette option est destructive avec les étapes en cours :

1. Détruire le Cluster.
2. Modifier l’appartenance au domaine des nœuds dans le nouveau domaine.
3. Recréez le Cluster en tant que nouveau dans le domaine de mise à jour.  Cela impliquerait de devoir recréer toutes les ressources.

La deuxième option est moins destructrice, mais nécessite un matériel supplémentaire, car un nouveau cluster a besoin être généré dans le nouveau domaine.  Une fois que le cluster est dans le nouveau domaine, exécutez l’Assistant de Migration de Cluster pour migrer les ressources. Notez que cela ne migrer des données - vous devrez utiliser un autre outil pour migrer des données, tel que [Service de Migration de stockage](../storage/storage-migration-service/overview.md)(une fois que la prise en charge de cluster est ajouté).

![Créer et migrer](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-2.gif)

Comme le montre l’animation, cette option n’est pas destructif mais nécessite un matériel différent ou un nœud du cluster existant qu’a été supprimé.

1. Créer un nouveau CLUSTÉRINE le domaine tout en conservant l’ancien cluster disponible.
2. Utilisez le [Assistant de Migration de Cluster](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754481(v=ws.10)) pour migrer toutes les ressources vers le nouveau cluster. Pour rappel, il ne copie pas les données, sera donc pas à le faire séparément.
3. Désactiver ou supprimer l’ancien cluster.

Dans les deux options, le nouveau cluster doit avoir tous [applications adaptée aux clusters](https://technet.microsoft.com/aa369082(v=vs.90)) installé, la mise à jour, des pilotes et éventuellement tester pour s’assurer tout s’exécute correctement.  Il s’agit d’un processus beaucoup de temps si les données doivent également être déplacé.

## <a name="windows-server-2019"></a>Windows Server 2019

Dans Windows Server 2019, nous avons introduit des fonctionnalités de migration de domaine de cluster croisé.  Maintenant, les scénarios répertoriés ci-dessus peuvent facilement être effectuées et la nécessité de reconstruire n’est plus nécessaire.  

Déplacement d’un cluster d’un domaine est un processus simple. Pour ce faire, il existe deux nouvelles applets de commande PowerShell.

**Nouvelle ClusterNameAccount** – crée un compte du nom du Cluster dans Active Directory **Remove-ClusterNameAccount** – supprime les comptes de nom de Cluster à partir d’Active Directory

Le processus pour y parvenir consiste à modifier le cluster à partir d’un domaine vers un groupe de travail et retour vers le nouveau domaine.  La nécessité de supprimer un cluster, de reconstruire un cluster, d’installer des applications, etc. n’est pas obligatoire. Par exemple, elle ressemblerait à ceci :

![Migrer](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-3.gif)

## <a name="migrating-a-cluster-to-a-new-domain"></a>Migration d’un cluster vers un nouveau domaine

Dans les étapes suivantes, un cluster est déplacé à partir du domaine Contoso.com pour le nouveau domaine Fabrikam.com.  Le nom du cluster est *CLUSCLUS* et avec un rôle de serveur de fichiers appelé *FS-CLUSCLUS*.

1. Créer un compte d’administrateur local avec le même nom et mot de passe sur tous les serveurs du cluster.  Cela peut être nécessaire pour se connecter alors que les serveurs sont déplacement entre domaines.
2. Connectez-vous au premier serveur avec un compte d’utilisateur ou administrateur de domaine qui dispose des autorisations Active Directory pour le Cluster nom objet (CNO), objets d’ordinateur virtuel (VCO), a accès au Cluster et ouvrez PowerShell.
3. Vérifiez toutes les ressources de nom réseau du Cluster sont dans des état et de l’exécution en mode hors connexion la commande ci-dessous.  Cette commande supprime les objets Active Directory que le cluster peut avoir.

   ```PowerShell
   Remove-ClusterNameAccount -Cluster CLUSCLUS -DeleteComputerObjects
   ```
4. Utiliser Active Directory Users and Computers pour vous assurer de l’ordinateur CNO et VCO objets associés à tous les noms de cluster ont été supprimés.

   > [!NOTE]
   > Il est judicieux d’arrêter le service de Cluster sur tous les serveurs du cluster et défini le type de démarrage du service sur manuel afin que le service de Cluster ne démarre pas lorsque les serveurs sont le redémarrage lors de la modification des domaines.

   ```PowerShell
   Stop-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Manual
   ```

5. Modifier l’appartenance au domaine de serveurs à un groupe de travail, redémarrez les serveurs, joindre les serveurs vers le nouveau domaine, puis à nouveau.
6. Une fois que les serveurs sont dans le nouveau domaine, connectez-vous à un serveur avec un compte utilisateur ou un administrateur de domaine qui dispose des autorisations Active Directory pour créer des objets, a accès au Cluster, puis ouvrez PowerShell. Démarrer le Service de Cluster et définissez-le sur automatique.

   ```PowerShell
   Start-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Automatic
   ```
7. Mettre le nom du Cluster et tous les autres clusters des ressources de nom de réseau à un état connecté.

   ```PowerShell
   Start-ClusterResource -Name "Cluster Name"

   Start-ClusterResource -Name FS-CLUSCLUS
   ```

8. Modifiez le cluster pour faire partie du nouveau domaine avec des objets d’annuaire active directory associé. Pour ce faire, la commande figure ci-dessous et les ressources de nom de réseau doivent être dans un état en ligne.  Ce que fera cette commande est de recréer les objets de nom dans Active Directory.

   ```PowerShell
   New-ClusterNameAccount -Name CLUSTERNAME -Domain NEWDOMAINNAME.com -UpgradeVCOs
   ```

    REMARQUE : Si vous n’avez pas de tous les autres groupes dont le nom de réseau (par exemple, un Cluster Hyper-V avec uniquement des machines virtuelles), le commutateur de paramètre - UpgradeVCOs n’est pas nécessaire.

9. Utilisez Active Directory Users and Computers pour vérifiez le nouveau domaine et assurez-vous que les objets ordinateur associés ont été créés. S’ils ont, puis mettre les ressources restantes dans les groupes en ligne.

   ```PowerShell
   Start-ClusterGroup -Name "Cluster Group"

   Start-ClusterGroup -Name FS-CLUSCLUS
   ```

## <a name="known-issues"></a>Problèmes connus

Si vous utilisez la nouvelle fonctionnalité de témoin USB, il se peut que vous ne pourrez pas ajouter le cluster vers le nouveau domaine.  Le raisonnement est que le type de témoin de partage de fichier doit utiliser Kerberos pour l’authentification.  Modifier le témoin sur none avant d’ajouter le cluster au domaine.  Une fois celle-ci terminée, recréez le témoin USB.  Est de l’erreur que s’affiche :

```
New-ClusternameAccount : Cluster name account cannot be created.  This cluster contains a file share witness with invalid permissions for a cluster of type AdministrativeAccesssPoint ActiveDirectoryAndDns. To proceed, delete the file share witness.  After this you can create the cluster name account and recreate the file share witness.  The new file share witness will be automatically created with valid permissions.
```

