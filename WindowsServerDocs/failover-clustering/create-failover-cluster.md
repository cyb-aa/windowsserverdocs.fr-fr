---
title: Créer un cluster de basculement
description: Comment créer un cluster de basculement pour Windows Server 2012 R2, Windows Server 2012 et Windows Server 2016.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 11/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: f919e69488c4f2272ddd07e535ba4e2248ddf79c
ms.sourcegitcommit: 5cbaca9685720f11d896c4ca167c86e74c032feb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/06/2018
ms.locfileid: "6068977"
---
# Créer un cluster de basculement

>S’applique à: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Cette rubrique montre comment créer un cluster de basculement en utilisant le composant logiciel enfichable Gestionnaire du Cluster de basculement ou Windows PowerShell. Cette rubrique traite un déploiement classique, où les objets ordinateur pour le cluster et ses rôles en cluster associés sont créés dans Active Directory Domain Services (AD DS). Si vous déployez un cluster d’espaces de stockage Direct, au lieu de cela voir [Déployer des espaces de stockage Direct](../storage/storage-spaces/deploy-storage-spaces-direct.md).

Vous pouvez également déployer un cluster détaché d’Active Directory. Cette méthode de déploiement vous permet de créer un cluster de basculement sans les autorisations nécessaires pour créer des objets ordinateur dans AD DS ou la nécessité de demander cet ordinateur objets ont été prédéfinis dans AD DS. Cette option n’est disponible par le biais de Windows PowerShell et il est recommandée uniquement pour des scénarios spécifiques. Pour plus d’informations, voir [déployer un Cluster Active Directory-Detached](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

#### Liste de vérification: Créer un cluster de basculement

|État|Tâche|Référence|
|:---:|---|---|
|☐|Vérifiez les conditions préalables|[Vérifiez les conditions préalables](#verify-the-prerequisites)|
|☐|Installer la fonctionnalité Clustering avec basculement sur chaque serveur que vous souhaitez ajouter en tant qu’un nœud de cluster|[Installer la fonctionnalité Clustering avec basculement](#install-the-failover-clustering-feature)|
|☐|Exécutez l’Assistant de Validation de Cluster pour valider la configuration|[Valider la configuration](#validate-the-configuration)|
|☐|Exécutez l’Assistant Création d’un Cluster pour créer le cluster de basculement|[Créez le cluster de basculement](#create-the-failover-cluster)|
|☐|Créer des rôles en cluster aux charges de travail de cluster hôte|[Créer des rôles en cluster](#create-clustered-roles)|

## Vérifiez les conditions préalables

Avant de commencer, vérifiez les conditions préalables suivantes:

- Assurez-vous que tous les serveurs que vous souhaitez ajouter en tant que nœuds de cluster exécutent la même version de Windows Server.
- Passez en revue la configuration matérielle requise pour vous assurer que votre configuration est pris en charge. Pour plus d’informations, voir [configuration matérielle de Clustering de basculement et Options de stockage](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj612869(v%3dws.11)). Si vous créez un cluster d’espaces de stockage Direct, consultez les [espaces de stockage Direct configuration matérielle requise](../storage/storage-spaces/storage-spaces-direct-hardware-requirements.md).
- Pour ajouter le stockage en cluster lors de la création du cluster, assurez-vous que tous les serveurs peuvent accéder au stockage. (Vous pouvez également ajouter stockage en cluster après avoir créé le cluster.)
- Assurez-vous que tous les serveurs que vous souhaitez ajouter en tant que nœuds de cluster sont joints au même domaine Active Directory.
- (Facultatif) Créez une unité d’organisation (UO) et déplacer les comptes d’ordinateur pour les serveurs que vous souhaitez ajouter en tant que nœuds de cluster dans l’unité d’organisation. Comme meilleure pratique, nous vous recommandons de placer les clusters de basculement dans leur propre unité d’organisation dans AD DS. Cela peut vous aider à mieux contrôler les paramètres de stratégie de groupe ou les paramètres de modèle de sécurité sur les nœuds de cluster. En isolant les clusters dans leur propre unité d’organisation, il permet également d’empêcher contre la suppression accidentelle des objets ordinateur du cluster.

En outre, vérifiez les conditions de compte suivantes:

- Assurez-vous que le compte que vous souhaitez utiliser pour créer le cluster est un utilisateur de domaine disposant de droits d’administrateur sur tous les serveurs que vous souhaitez ajouter en tant que nœuds de cluster.
- Assurez-vous qu’une des opérations suivantes est vraie:
    - L’utilisateur qui crée le cluster a l’autorisation **d’objets de créer un ordinateur** à l’unité d’organisation ou le conteneur où se trouvent les serveurs qui forment le cluster.
    - Si l’utilisateur ne dispose pas de l’autorisation **des objets de créer un ordinateur** , demandez à un administrateur de domaine pour prédéfinir un objet ordinateur de cluster pour le cluster. Pour plus d’informations, voir [Prestage Cluster un objet ordinateur dans Active Directory Domain Services](prestage-cluster-adds.md).

>[!NOTE]
>Cette exigence ne s’applique pas si vous souhaitez créer un cluster détaché d’Active Directory dans Windows Server 2012 R2. Pour plus d’informations, voir [déployer un Cluster Active Directory-Detached](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

## Installer la fonctionnalité Clustering avec basculement

Vous devez installer la fonctionnalité Clustering avec basculement sur chaque serveur que vous souhaitez ajouter en tant qu’un nœud de cluster de basculement.

### Installer la fonctionnalité Clustering avec basculement

1. Démarrez le Gestionnaire de serveur.
2. Dans le menu **Gérer** , sélectionnez **Ajouter des rôles et fonctionnalités**.
3. Sur la page **avant de commencer** , cliquez sur **suivant**.
4. Sur la page **Sélectionner le type d’installation** , sélectionnez **l’installation en fonction du rôle ou une fonctionnalité**et sélectionnez **suivant**.
5. Sur la page **Sélectionner le serveur de destination** , sélectionnez le serveur où vous souhaitez installer la fonctionnalité et sélectionnez **suivant**.
6. Sur la page **Sélectionner des rôles de serveur** , cliquez sur **suivant**.
7. Sur la page **Sélectionner des fonctionnalités** , activez la case à cocher **Du Clustering de basculement** .
8. Pour installer les outils de gestion de cluster de basculement, sélectionnez **Ajouter des fonctionnalités**, puis sélectionnez **suivant**.
9. Sur la page des **sélections pour confirmer l’installation** , sélectionnez **l’installation**.
<br>Un redémarrage du serveur n’est pas requis pour la fonctionnalité Clustering avec basculement.

10. Lorsque l’installation est terminée, sélectionnez **Fermer**.
11. Répétez cette procédure sur chaque serveur que vous souhaitez ajouter en tant qu’un nœud de cluster de basculement.

>[!NOTE]
>Après avoir installé la fonctionnalité Clustering avec basculement, nous vous recommandons d’appliquer les dernières mises à jour à partir de Windows Update. En outre, pour un cluster de basculement Windows Server 2012, consultez l’article de Support Microsoft [des clusters de correctifs recommandés et des mises à jour pour le basculement basé sur Windows Server 2012](https://support.microsoft.com/help/2784261/recommended-hotfixes-and-updates-for-windows-server-2012-based-failove) et installer les mises à jour qui s’appliquent.

## Valider la configuration

Avant de créer le cluster de basculement, nous vous recommandons vivement de valider la configuration pour vous assurer que le matériel et les paramètres du matériel sont compatibles avec le clustering de basculement. Microsoft prend en charge une solution de cluster uniquement si la configuration complète est validé tous les tests et si tous les composants matériels sont validée pour la version de Windows Server qui exécutent les nœuds de cluster.

>[!NOTE]
>Vous devez disposer d’au moins deux nœuds à exécuter tous les tests. Si vous avez un seul nœud, la plupart des tests stockage critique n’exécutent pas.

### Exécuter des tests de validation de cluster

1. Sur un ordinateur qui présente les outils de gestion de Cluster de basculement installé à partir d’outils d’Administration de serveur distant ou sur un serveur où vous avez installé la fonctionnalité Clustering avec basculement, démarrez le Gestionnaire du Cluster de basculement. Pour ce faire sur un serveur, démarrez le Gestionnaire de serveur, puis dans le menu **Outils** , **Le Gestionnaire du Cluster de basculement**.
2. Dans le volet **Gestionnaire du Cluster de basculement** , sous **gestion**, sélectionnez **Valider la Configuration**.
3. Sur la page **Avant de commencer** , cliquez sur **suivant**.
4. Sur la page **Sélectionner des serveurs ou un Cluster** , dans la zone **Entrez le nom** , entrez le nom NetBIOS ou le nom de domaine complet d’un serveur que vous prévoyez d’ajouter en tant qu’un nœud de cluster de basculement, puis sélectionnez **Ajouter**. Répétez cette étape pour chaque serveur que vous souhaitez ajouter. Pour ajouter plusieurs serveurs en même temps, séparez-les par une virgule ou par des points-virgules. Par exemple, entrez les noms en utilisant le format *server1.contoso.com, server2.contoso.com*. Lorsque vous avez terminé, cliquez sur **suivant**.
5. Sur la page **Options de test** , sélectionnez **exécuter tous les tests (recommandés)** et sélectionnez **suivant**.
6. Sur la page de **Confirmation** , cliquez sur **suivant**.

    La page de validation affiche l’état des tests en cours d’exécution.
7. Sur la page **Résumé** , effectuez une des opérations suivantes:
    
      - Si les résultats indiquent que les tests s’est terminé correctement et la configuration est adaptée aux clusters, et vous souhaitez créer le cluster immédiatement, assurez-vous que la case à cocher **créer le cluster maintenant en utilisant les nœuds validés** est sélectionnée, puis Sélectionnez **Terminer**. Ensuite, passez à l’étape 4 de la procédure de [création du cluster de basculement](#create-the-failover-cluster) .
      - Si les résultats indiquent qu’il n’existait avertissement ou échec, sélectionnez **Afficher le rapport** pour afficher les détails et déterminer quels problèmes doivent être corrigées. Sachez qu’un message d’avertissement pour un test de validation particulière indique que cet aspect du cluster de basculement peut être pris en charge, mais peut ne pas convenir les meilleures pratiques recommandées.
        
        >[!NOTE]
        >Si vous recevez un message d’avertissement pour le test de validez la réservation persistante des espaces de stockage, consultez le blog [Avertissement de validation de Cluster de basculement Windows indique vos disques ne prennent pas en charge les réservations permanentes pour les espaces de stockage](https://blogs.msdn.microsoft.com/clustering/2013/05/24/validate-storage-spaces-persistent-reservation-test-results-with-warning/) pour plus d’informations.

Pour plus d’informations sur les tests de validation de matériel, voir [Valider le matériel pour un Cluster de basculement](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

## Créez le cluster de basculement

Pour effectuer cette étape, assurez-vous que le compte d’utilisateur que vous ouvrez une session en tant que répond aux exigences décrites dans la section [Vérifiez les conditions préalables](#verify-the-prerequisites) de cette rubrique.

1. Démarrez le Gestionnaire de serveur.
2. Dans le menu **Outils** , sélectionnez **Le Gestionnaire du Cluster de basculement**.
3. Dans le volet **Gestionnaire du Cluster de basculement** , sous **gestion**, sélectionnez **Créer un Cluster**.
    
    L’Assistant Création d’un Cluster s’ouvre.
4. Sur la page **Avant de commencer** , cliquez sur **suivant**.
5. Si la page **Sélectionner des serveurs** s’affiche, dans la zone **Entrez le nom** , entrez le nom NetBIOS ou le nom de domaine complet d’un serveur que vous prévoyez d’ajouter en tant qu’un nœud de cluster de basculement, puis sélectionnez **Ajouter**. Répétez cette étape pour chaque serveur que vous souhaitez ajouter. Pour ajouter plusieurs serveurs en même temps, séparez-les par une virgule ou un point-virgule. Par exemple, entrez les noms dans le format *server1.contoso.com; server2.contoso.com*. Lorsque vous avez terminé, cliquez sur **suivant**.
    
    >[!NOTE]
    >Si vous avez choisi créer le cluster immédiatement après l’exécution de validation dans la [procédure de validation de configuration](#validate-the-configuration), vous ne verrez pas la page **Sélectionner les serveurs** . Les nœuds qui ont été validés sont automatiquement ajoutés à l’Assistant Création d’un Cluster afin que vous n’êtes pas obligé de les entrer à nouveau.
6. Si vous avez ignoré validation précédemment, la page **d’Avertissement de Validation** s’affiche. Nous recommandons vivement de validation de cluster à exécuter. Seuls les clusters qui réussir tous les tests de validation sont pris en charge par Microsoft. Pour exécuter les tests de validation, sélectionnez **«Oui»** et sélectionnez **suivant**. Terminez l’Assistant valider un Configuration comme décrit dans la zone [valider la configuration](#validate-the-configuration).
7. Sur la page de **Point d’accès pour l’administration du Cluster** , procédez comme suit:
    
    1. Dans la zone **Nom du Cluster** , entrez le nom que vous voulez utiliser pour administrer le cluster. Avant de vous faire, passez en revue les informations suivantes:
        
          - Au cours de la création du cluster, ce nom est enregistré en tant que l’objet d’ordinateur de cluster (également connue sous *l’objet de nom de cluster* ou *CNO*) dans les services AD DS. Si vous spécifiez un nom NetBIOS pour le cluster, le CNO est créé dans le même emplacement dans lequel se trouvent les objets ordinateur pour les nœuds de cluster. Cela peut être le conteneur d’ordinateurs par défaut ou une unité d’organisation.
          - Pour spécifier un autre emplacement pour le CNO, vous pouvez entrer le nom unique d’une unité d’organisation dans la zone **Nom du Cluster** . Par exemple: *CN = ClusterName, unité d’organisation = Clusters, DC = Contoso, DC = com*.
          - Si un administrateur de domaine a prédéfini le CNO dans une autre unité d’organisation qu’où se trouvent les nœuds de cluster, spécifiez le nom unique qui fournit l’administrateur de domaine.
    2. Si le serveur ne dispose pas d’une carte réseau qui est configurée pour utiliser le protocole DHCP, vous devez configurer une ou plusieurs adresses IP statiques pour le cluster de basculement. Activez la case à cocher en regard de chaque réseau que vous souhaitez utiliser pour la gestion de cluster. Sélectionnez le champ **d’adresse** en regard d’un réseau sélectionné, puis entrez l’adresse IP que vous souhaitez attribuer au cluster. Cette adresse IP (ou les adresses) sera associées au nom de cluster dans système DNS (Domain Name).
    3. Lorsque vous avez terminé, cliquez sur **suivant**.
8. Sur la page de **Confirmation** , passez en revue les paramètres. Par défaut, la case à cocher **Ajouter tout le stockage éligible au cluster** est sélectionnée. Désactivez cette case à cocher si vous souhaitez effectuer une des opérations suivantes:
    
      - Vous souhaitez configurer le stockage plus tard.
      - Vous prévoyez de créer des espaces de stockage en cluster par le biais de gestionnaire du Cluster de basculement ou via les applets de commande PowerShell de Windows Clustering de basculement et n’avez pas encore créé les espaces de stockage dans les Services de fichiers et stockage. Pour plus d’informations, voir [Déployer des espaces de stockage en cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>).
9. Cliquez sur **suivant** pour créer le cluster de basculement.
10. Sur la page **Résumé** , vérifiez que le cluster de basculement a été créé avec succès. S’il s’agissait d’éventuels avertissements ou erreurs, permet d’afficher la sortie de synthèse ou sélectionnez **Afficher le rapport** pour afficher le rapport complet. Sélectionnez **Terminer**.
11. Pour confirmer que le cluster a été créé, vérifiez que le nom du cluster est répertorié sous **Le Gestionnaire du Cluster de basculement** dans l’arborescence de navigation. Vous pouvez développer le nom du cluster et ensuite sélectionner des éléments sous les **nœuds**, de **stockage** ou **les réseaux** pour afficher les ressources associées.
    
    Sachez qu’il peut prendre un certain temps pour le nom du cluster parvenir à répliquer dans DNS. Après l’inscription réussie de DNS et la réplication, si vous sélectionnez **Tous les serveurs** dans le Gestionnaire de serveur, le nom du cluster doit être répertorié en tant que serveur avec l’état **facilité de gestion** **en ligne**.

Une fois que le cluster est créé, vous pouvez faire choses telles que vérifier la configuration de quorum de cluster et si vous le souhaitez, créer des Volumes partagés de Cluster (CSV). Pour plus d’informations, voir [Quorum de comprendre dans les espaces de stockage Direct](../storage/storage-spaces/understand-quorum.md) et [Utilisation Volumes partagés du Cluster dans un cluster de basculement](failover-cluster-csvs.md).

## Créer des rôles en cluster

Après avoir créé le cluster de basculement, vous pouvez créer des rôles en cluster aux charges de travail de cluster hôte.

>[!NOTE]
>Pour les rôles en cluster qui nécessitent un point d’accès client, un objet ordinateur virtuel (VCO) est créé dans AD DS. Par défaut, tous les VCO pour le cluster sont créés dans le même conteneur ou unité d’organisation en tant que le CNO. Sachez qu’après avoir créé un cluster, vous pouvez déplacer le CNO vers une unité d’organisation.

Voici comment créer un rôle en cluster:

1. Utilisez le Gestionnaire de serveur ou Windows PowerShell pour installer le rôle ou une fonctionnalité qui est requise pour un rôle en cluster sur chaque nœud du cluster de basculement. Par exemple, si vous souhaitez créer un serveur de fichiers en cluster, installez le rôle de serveur de fichiers sur tous les nœuds de cluster.
    
    Le tableau suivant présente les rôles en cluster que vous pouvez configurer dans l’Assistant haute disponibilité et le rôle de serveur associé ou la fonctionnalité que vous devez installer en tant que condition préalable.
    
    <table>
    <thead>
    <tr class="header">
    <th>Rôle en cluster</th>
    <th>Rôle ou fonctionnalité prérequis</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>Serveur de Namespace DFS</td>
    <td>Espaces de noms DFS (partie du rôle de serveur de fichiers)</td>
    </tr>
    <tr class="even">
    <td>Serveur DHCP</td>
    <td>Rôle de serveur DHCP</td>
    </tr>
    <tr class="odd">
    <td>Distributed Transaction Coordinator (DTC)</td>
    <td>None</td>
    </tr>
    <tr class="even">
    <td>Serveur de fichiers</td>
    <td>Rôle de serveur de fichiers</td>
    </tr>
    <tr class="odd">
    <td>Application générique</td>
    <td>Non applicable</td>
    </tr>
    <tr class="even">
    <td>Script générique</td>
    <td>Non applicable</td>
    </tr>
    <tr class="odd">
    <td>Service générique</td>
    <td>Non applicable</td>
    </tr>
    <tr class="even">
    <td>Service Broker réplica Hyper-V</td>
    <td>Rôle Hyper-V</td>
    </tr>
    <tr class="odd">
    <td>Serveur cible iSCSI</td>
    <td>Serveur cible (partie du rôle de serveur de fichiers) iSCSI</td>
    </tr>
    <tr class="even">
    <td>Serveur iSNS</td>
    <td>fonctionnalité de Service serveur iSNS</td>
    </tr>
    <tr class="odd">
    <td>Message Queuing</td>
    <td>Message Queuing Services, nouvelle fonctionnalité</td>
    </tr>
    <tr class="even">
    <td>Autre serveur</td>
    <td>None</td>
    </tr>
    <tr class="odd">
    <td>Machine virtuelle</td>
    <td>Rôle Hyper-V</td>
    </tr>
    <tr class="even">
    <td>Serveur WINS</td>
    <td>Fonctionnalité du serveur WINS</td>
    </tr>
    </tbody>
    </table>
2. Dans le Gestionnaire de Cluster de basculement, développez le nom du cluster, avec le bouton droit de **rôles**et sélectionnez **Configurer un rôle**.
3. Suivez les étapes de l’Assistant haute disponibilité pour créer le rôle en cluster.
4. Pour vérifier que le rôle en cluster a été créé, dans le volet de **rôles** , assurez-vous que le rôle a le statut **en cours d’exécution**. Le volet rôles indique également le nœud propriétaire. Pour tester le basculement, le rôle d’avec le bouton droit, pointez sur **déplacer**et sélectionnez **Sélectionnez un nœud**. Dans la boîte de dialogue **Déplacer un rôle en cluster** , sélectionnez le nœud de cluster souhaité et sélectionnez **OK**. Dans la colonne de **Nœud propriétaire** , vérifiez que le nœud propriétaire changé.

## Créer un cluster de basculement à l’aide de Windows PowerShell

Les applets de commande Windows PowerShell suivantes effectuent les mêmes fonctions que les procédures précédentes de cette rubrique. Entrez chaque applet de commande sur une seule ligne, même si elles apparaissent avec retour automatique sur plusieurs lignes en raison des contraintes de mise en forme.

>[!NOTE]
>Vous devez utiliser Windows PowerShell pour créer un cluster détaché d’Active Directory dans Windows Server 2012 R2. Pour plus d’informations sur la syntaxe, consultez [déployer un Cluster Active Directory-Detached](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

L’exemple suivant installe la fonctionnalité Clustering avec basculement.

```PowerShell
Install-WindowsFeature –Name Failover-Clustering –IncludeManagementTools
```

L’exemple suivant exécute tous les tests de validation de cluster sur les ordinateurs qui sont nommés *Server1* et *Server2*.

```PowerShell
Test-Cluster –Node Server1, Server2
```

>[!NOTE]
>L’applet de commande **Test-Cluster** renvoie les résultats vers un fichier journal dans le répertoire de travail en cours. Par exemple: C:\Users\ < nom d’utilisateur > \AppData\Local\Temp.

L’exemple suivant crée un cluster de basculement qui est nommé *Mon_cluster* avec des nœuds *Server1* et *Server2*, affecte l' d’adresse IP statique *192.168.1.12*et ajoute la totalité du stockage éligible au cluster de basculement.

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12
```

L’exemple suivant crée le même cluster de basculement comme dans l’exemple précédent, mais il n’ajoute pas de stockage éligible au cluster de basculement.

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12 -NoStorage
```

L’exemple suivant crée un cluster nommé *Mon_cluster* dans le *Cluster* d’unité d’organisation du domaine *Contoso.com*.

```PowerShell
New-Cluster -Name CN=MyCluster,OU=Cluster,DC=Contoso,DC=com -Node Server1, Server2
```

Pour obtenir des exemples illustrant comment ajouter des rôles en cluster, consultez les rubriques telles que [Add-ClusterFileServerRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clusterfileserverrole?view=win10-ps) et [Add-ClusterGenericApplicationRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clustergenericapplicationrole?view=win10-ps).

## Informations supplémentaires

  - [Clustering de basculement](failover-clustering.md)
  - [Déployer un Cluster Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj863389(v%3dws.11)>)
  - [Serveur de fichiers avec montée en données d’Application](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831349(v%3dws.11)>)
  - [Déployer un Cluster détaché de répertoire actif](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))
  - [Utilisation du clustering invité pour une haute disponibilité](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)
  - [Mise à jour adaptée aux clusters](cluster-aware-updating.md)
  - [Nouveau Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/new-cluster?view=win10-ps)
  - [Test-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps)
