---
title: Migration de cluster inter-domaines dans Windows Server 2016/2019
description: Cet article décrit le déplacement d’un cluster Windows Server 2019 d’un domaine à un autre
ms.prod: windows-server
manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.author: johnmar
ms.date: 01/18/2019
ms.localizationpriority: medium
ms.openlocfilehash: ba556b5a00f3932e2049135b177a7ad8bbceec9c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828292"
---
# <a name="failover-cluster-domain-migration"></a>Migration d’un domaine de cluster de basculement

> S’applique à : Windows Server 2019, Windows Server 2016

Cette rubrique fournit une vue d’ensemble du déplacement de clusters de basculement Windows Server d’un domaine à un autre.

## <a name="why-migrate-between-domains"></a>Pourquoi migrer entre des domaines

Il existe plusieurs scénarios dans lesquels la migration d’un cluster d’un autre à un autre est nécessaire.

- La sociétéa fusionne avec CompanyB et doit déplacer tous les clusters dans le domaine de l’entreprise
- Les clusters sont créés dans le centre de documentation principal et expédiés vers des emplacements distants
- Le cluster a été créé en tant que cluster de groupe de travail et doit maintenant faire partie d’un domaine
- Le cluster a été créé en tant que cluster de domaine et doit maintenant faire partie d’un groupe de travail
- Le cluster est déplacé vers une zone de la société vers une autre et est un sous-domaine différent

Microsoft ne fournit pas de support aux administrateurs qui essaient de déplacer des ressources d’un domaine à un autre si l’opération d’application sous-jacente n’est pas prise en charge. Par exemple, Microsoft ne fournit pas de support technique aux administrateurs qui essaient de déplacer un serveur Microsoft Exchange d’un domaine vers un autre.

   > [!WARNING]
   > Nous vous recommandons d’effectuer une sauvegarde complète de l’ensemble du stockage partagé dans le cluster avant de déplacer le cluster.

## <a name="windows-server-2016-and-earlier"></a>Windows Server 2016 et versions antérieures

Dans Windows Server 2016 et les versions antérieures, les service de cluster n’ont pas la possibilité de passer d’un domaine à un autre.  Cela était dû à une dépendance accrue sur les Active Directory Domain Services et les noms virtuels créés.   

## <a name="options"></a>Options

Pour effectuer ce déplacement, il existe deux options.

La première option consiste à détruire le cluster et à le reconstruire dans le nouveau domaine.

![Détruire et régénérer](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-1.gif)

Comme l’indique l’animation, cette option est destructrice avec les étapes suivantes :

1. Détruisez le cluster.
2. Modifiez l’appartenance au domaine des nœuds dans le nouveau domaine.
3. Recréez le cluster en tant que nouveau dans le domaine mis à jour.  Cela entraînerait la recréation de toutes les ressources.

La deuxième option est moins destructrice, mais nécessite un matériel supplémentaire, car un nouveau cluster doit être créé dans le nouveau domaine.  Une fois le cluster dans le nouveau domaine, exécutez l’Assistant Migration de cluster pour migrer les ressources. Notez que cela ne migre pas les données. vous devez utiliser un autre outil pour migrer des données, tel que le [service de migration du stockage](../storage/storage-migration-service/overview.md)(une fois la prise en charge du cluster ajoutée).

![Créer et migrer](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-2.gif)

Comme l’indique l’animation, cette option n’est pas destructrice, mais nécessite un matériel différent ou un nœud du cluster existant qui n’a pas été supprimé.

1. Créez un nouveau cluster dans le nouveau domaine tout en gardant l’ancien cluster disponible.
2. Utilisez l' [Assistant Migration de cluster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754481(v=ws.10)) pour migrer toutes les ressources vers le nouveau cluster. Rappel : les données ne sont pas copiées. vous devez donc effectuer séparément.
3. Désaffecter ou détruire l’ancien cluster.

Dans les deux options, le nouveau cluster doit disposer de toutes les [applications prenant en charge les clusters](https://technet.microsoft.com/aa369082(v=vs.90)) , des pilotes à jour et éventuellement des tests pour s’assurer que tout fonctionne correctement.  Il s’agit d’un processus qui prend du temps si des données doivent également être déplacées.

## <a name="windows-server-2019"></a>Windows Server 2019

Dans Windows Server 2019, nous avons introduit des fonctionnalités de migration de domaine entre clusters.  Ainsi, les scénarios ci-dessus peuvent être facilement effectués et le besoin de reconstruction n’est plus nécessaire.  

Le déplacement d’un cluster à partir d’un domaine est un processus simple. Pour ce faire, il existe deux nouveaux applets PowerShell.

**New-ClusterNameAccount** : crée un compte de nom de cluster dans Active Directory **Remove-ClusterNameAccount** – supprime les comptes de nom de cluster de Active Directory

Pour ce faire, vous pouvez modifier le cluster d’un domaine à un groupe de travail et revenir au nouveau domaine.  La nécessité de détruire un cluster, de reconstruire un cluster, d’installer des applications, etc. n’est pas obligatoire. Par exemple, il ressemble à ceci :

![Migrer](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-3.gif)

## <a name="migrating-a-cluster-to-a-new-domain"></a>Migration d’un cluster vers un nouveau domaine

Dans les étapes suivantes, un cluster est déplacé du domaine Contoso.com vers le nouveau domaine Fabrikam.com.  Le nom du cluster est *CLUSCLUS* et avec un rôle de serveur de fichiers nommé *FS-CLUSCLUS*.

1. Créez un compte d’administrateur local avec le même nom et le même mot de passe sur tous les serveurs du cluster.  Cela peut être nécessaire pour se connecter pendant que les serveurs se déplacent entre les domaines.
2. Connectez-vous au premier serveur avec un compte d’utilisateur de domaine ou un compte administrateur disposant des autorisations Active Directory sur l’objet nom de cluster (CNO), les objets ordinateur virtuel (VCO), l’accès au cluster et l’ouverture de PowerShell.
3. Vérifiez que toutes les ressources de nom réseau de cluster sont dans un État hors connexion et exécutez la commande ci-dessous.  Cette commande supprime les objets Active Directory que le cluster peut avoir.

   ```PowerShell
   Remove-ClusterNameAccount -Cluster CLUSCLUS -DeleteComputerObjects
   ```
4. Utilisez Active Directory les utilisateurs et les ordinateurs pour vous assurer que les objets ordinateur CNO et VCO associés à tous les noms en cluster ont été supprimés.

   > [!NOTE]
   > Il est conseillé d’arrêter la service de cluster sur tous les serveurs du cluster et de définir le type de démarrage du service sur manuel afin que le service de cluster ne démarre pas lorsque les serveurs redémarrent au cours de la modification des domaines.

   ```PowerShell
   Stop-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Manual
   ```

5. Modifiez l’appartenance de domaine des serveurs à un groupe de travail, redémarrez les serveurs, joignez les serveurs au nouveau domaine, puis redémarrez.
6. Une fois que les serveurs se trouvent dans le nouveau domaine, connectez-vous à un serveur avec un compte d’utilisateur de domaine ou d’administrateur disposant des autorisations Active Directory pour créer des objets, avoir accès au cluster et ouvrir PowerShell. Démarrez le service de cluster, puis redéfinissez-le sur automatique.

   ```PowerShell
   Start-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Automatic
   ```
7. Mettez le nom du cluster et toutes les autres ressources de nom de réseau de cluster à un État en ligne.

   ```PowerShell
   Start-ClusterResource -Name "Cluster Name"

   Start-ClusterResource -Name FS-CLUSCLUS
   ```

8. Modifiez le cluster pour qu’il fasse partie du nouveau domaine avec des objets Active Directory associés. Pour ce faire, la commande est en dessous et les ressources de nom de réseau doivent être dans un État en ligne.  Cette commande permet de recréer les objets Name dans Active Directory.

   ```PowerShell
   New-ClusterNameAccount -Name CLUSTERNAME -Domain NEWDOMAINNAME.com -UpgradeVCOs
   ```

    Remarque : Si vous n’avez pas de groupes supplémentaires avec des noms de réseau (par exemple, un cluster Hyper-V avec des machines virtuelles uniquement), le commutateur de paramètre-UpgradeVCOs n’est pas nécessaire.

9. Utilisez Active Directory utilisateurs et ordinateurs pour vérifier le nouveau domaine et vérifier que les objets ordinateur associés ont été créés. Si tel est le cas, mettez les ressources restantes dans les groupes en ligne.

   ```PowerShell
   Start-ClusterGroup -Name "Cluster Group"

   Start-ClusterGroup -Name FS-CLUSCLUS
   ```

## <a name="known-issues"></a>Problèmes connus

Si vous utilisez la nouvelle fonctionnalité de témoin USB, vous ne pourrez pas ajouter le cluster au nouveau domaine.  Le raisonnement est que le type de témoin de partage de fichiers doit utiliser Kerberos pour l’authentification.  Remplacez le témoin par aucun avant d’ajouter le cluster au domaine.  Une fois l’opération terminée, recréez le témoin USB.  L’erreur que vous verrez est la suivante :

```
New-ClusternameAccount : Cluster name account cannot be created.  This cluster contains a file share witness with invalid permissions for a cluster of type AdministrativeAccesssPoint ActiveDirectoryAndDns. To proceed, delete the file share witness.  After this you can create the cluster name account and recreate the file share witness.  The new file share witness will be automatically created with valid permissions.
```

