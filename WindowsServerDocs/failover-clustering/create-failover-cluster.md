---
title: créer un cluster de basculement
description: Comment créer un cluster de basculement pour Windows Server 2012 R2, Windows Server 2012, Windows Server 2016 et Windows Server 2019.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 11/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 4122375a48cae17e5f3ebcd7e9f3ce1fad28a105
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222493"
---
# <a name="create-a-failover-cluster"></a>créer un cluster de basculement

>S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Cette rubrique montre comment créer un cluster de basculement à l'aide du composant logiciel enfichable Gestionnaire du cluster de basculement ou de Windows PowerShell. Elle s'appuie sur un déploiement classique dans lequel les objets ordinateur du cluster et ses rôles en cluster associés sont créés dans les services de domaine Active Directory (AD DS). Si vous déployez un cluster d’espaces de stockage Direct, reportez-vous à la place [déployer des espaces de stockage Direct](../storage/storage-spaces/deploy-storage-spaces-direct.md).

Vous pouvez également déployer un cluster détaché d’Active Directory. Cette méthode de déploiement vous permet de créer un cluster de basculement sans les autorisations nécessaires pour créer des objets ordinateur dans AD DS ni le besoin de demander à ce que les objets ordinateur soient prédéfinis dans AD DS. Cette option est disponible uniquement via Windows PowerShell, et elle n'est recommandée que pour certains scénarios spécifiques. Pour plus d’informations, voir [Déployer un cluster détaché d’Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

#### <a name="checklist-create-a-failover-cluster"></a>Liste de vérification : créer un cluster de basculement

|État|Tâche|Référence|
|:---:|---|---|
|☐|Vérifier les conditions préalables|[Vérifier les conditions préalables](#verify-the-prerequisites)|
|☐|Installer la fonctionnalité de clustering de basculement sur chaque serveur à ajouter comme nœud de cluster|[Installer la fonctionnalité Clustering avec basculement](#install-the-failover-clustering-feature)|
|☐|Exécuter l'Assistant Validation de cluster pour valider la configuration|[Valider la configuration](#validate-the-configuration)|
|☐|Exécuter l'Assistant Création d'un cluster pour créer le cluster de basculement|[Créer le cluster de basculement](#create-the-failover-cluster)|
|☐|Créer des rôles en cluster pour héberger des charges de travail de cluster|[Créer des rôles en cluster](#create-clustered-roles)|

## <a name="verify-the-prerequisites"></a>Vérifier les conditions préalables

Avant de commencer, vérifiez les conditions préalables suivantes :

- Assurez-vous que l'ensemble des serveurs que vous voulez ajouter en tant que nœuds de cluster exécutent la même version de Windows Server.
- Examinez la configuration matérielle requise pour vous assurer que votre configuration est prise en charge. Pour plus d'informations, voir [Failover Clustering Hardware Requirements and Storage Options](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj612869(v%3dws.11)). Si vous créez un cluster d’espaces de stockage Direct, consultez [espaces de stockage Direct configuration matérielle requise](../storage/storage-spaces/storage-spaces-direct-hardware-requirements.md).
- Pour ajouter le stockage en cluster pendant la création du cluster, assurez-vous que tous les serveurs peuvent accéder au stockage. (Vous pouvez aussi ajouter un stockage en cluster après avoir créé le cluster.)
- Vérifiez que l'ensemble des serveurs à ajouter comme nœuds de cluster sont membres du même domaine Active Directory.
- (Facultatif) Créez une unité d'organisation (UO) et déplacez les comptes d'ordinateurs pour les serveurs à ajouter comme nœuds de cluster dans l'UO. Nous vous recommandons, à titre de meilleure pratique, de placer les clusters de basculement dans leur propre UO dans AD DS. Cela peut vous aider à mieux repérer les paramètres de stratégie de groupe ou les paramètres de modèle de sécurité qui affectent les nœuds de cluster. Le fait d'isoler les clusters dans leur propre UO vous aide aussi à vous prémunir contre les risques de suppression accidentelle d'objets ordinateur de cluster.

Par ailleurs, vérifiez les conditions requises en matière de comptes :

- Assurez-vous que le compte que vous envisagez d'utiliser pour créer le cluster est un utilisateur de domaine qui dispose de droits d'administrateur sur tous les serveurs que vous voulez ajouter en tant que nœuds de cluster.
- Assurez-vous que l'une ou l'autre des conditions suivantes est vraie :
    - L'utilisateur qui crée le cluster dispose de l'autorisation de **création d'objets ordinateur** sur l'UO ou le conteneur où résident les serveurs qui constitueront le cluster.
    - Si l'utilisateur ne dispose pas de l'autorisation de **création d'objets ordinateur** , demandez à un administrateur de domaine de prédéfinir un objet ordinateur de cluster pour le cluster. Pour plus d'informations, voir [Prestage Cluster Computer Objects in Active Directory Domain Services](prestage-cluster-adds.md).

>[!NOTE]
>Cette exigence ne s’applique pas si vous souhaitez créer un cluster détaché d’Active Directory dans Windows Server 2012 R2. Pour plus d’informations, voir [Déployer un cluster détaché d’Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

## <a name="install-the-failover-clustering-feature"></a>Installer la fonctionnalité de clustering de basculement

Vous devez installer la fonctionnalité de clustering de basculement sur chaque serveur à ajouter comme nœud de cluster.

### <a name="install-the-failover-clustering-feature"></a>Installer la fonctionnalité de clustering de basculement

1. Démarrez le Gestionnaire de serveur.
2. Sur le **gérer** menu, sélectionnez **Ajout de rôles et fonctionnalités**.
3. Sur le **avant de commencer** page, sélectionnez **suivant**.
4. Sur le **sélectionner type d’installation** , sélectionnez **installation en fonction du rôle ou une fonctionnalité**, puis sélectionnez **suivant**.
5. Sur le **server de sélectionner la destination** page, sélectionnez le serveur où vous souhaitez installer la fonctionnalité, puis sélectionnez **suivant**.
6. Sur le **sélectionner des rôles de serveur** , sélectionnez **suivant**.
7. Dans la page **Sélectionner des fonctionnalités**, cochez la case **Clustering de basculement**.
8. Pour installer les outils de gestion de cluster de basculement, sélectionnez **ajouter des fonctionnalités**, puis sélectionnez **suivant**.
9. Sur le **confirmer les sélections d’installation** page, sélectionnez **installer**.
<br>La fonctionnalité de clustering de basculement ne nécessite pas le redémarrage du serveur.

10. Lorsque l’installation est terminée, sélectionnez **fermer**.
11. Répétez cette procédure sur chaque serveur à ajouter comme nœud de cluster.

>[!NOTE]
>Après avoir installé la fonctionnalité de clustering de basculement, nous vous recommandons d'appliquer les dernières mises à jour de Windows Update. En outre, pour un cluster de basculement Windows Server 2012, passez en revue la [correctifs et mises à jour pour les clusters de basculement Windows Server 2012 recommandés](https://support.microsoft.com/help/2784261/recommended-hotfixes-and-updates-for-windows-server-2012-based-failove) Support Microsoft l’article et installer les mises à jour qui s’appliquent.

## <a name="validate-the-configuration"></a>Valider la configuration

Avant de créer le cluster de basculement, nous vous recommandons vivement de valider la configuration en vérifiant que les paramètres matériels et logiciels sont compatibles avec le clustering de basculement. Microsoft ne prend en charge une solution de cluster que si elle réussit tous les tests de validation et si tous les composants matériels sont certifiés pour la version de Windows Server exécutée sur les nœuds de cluster.

>[!NOTE]
>Pour exécuter tous les tests, vous devez disposer d'au moins deux nœuds. Si vous n'en avez qu'un, la plupart des tests de stockage importants ne s'exécuteront pas.

### <a name="run-cluster-validation-tests"></a>Exécuter des tests de validation de cluster

1. Sur un ordinateur doté des outils de gestion du cluster de basculement installés à partir des outils d'administration de serveur distant, ou sur un serveur sur lequel vous avez installé la fonctionnalité de clustering de basculement, démarrez le Gestionnaire du cluster de basculement. Pour ce faire, sur un serveur, démarrez le Gestionnaire de serveur, puis, dans le **outils** menu, sélectionnez **Gestionnaire du Cluster de basculement**.
2. Dans le **Gestionnaire du Cluster de basculement** volet, sous **gestion**, sélectionnez **valider la Configuration**.
3. Sur le **avant de commencer** page, sélectionnez **suivant**.
4. Sur le **sélectionner des serveurs ou un Cluster** page, dans le **entrer le nom du** zone, entrez le nom NetBIOS ou le nom de domaine complet d’un serveur que vous souhaitez ajouter en tant qu’un nœud de cluster de basculement et sélectionnez **Ajouter**. Répétez cette étape pour chaque serveur à ajouter. Pour ajouter plusieurs serveurs à la fois, séparez les noms d’une virgule ou d’un point-virgule. Par exemple, entrez les noms au format *server1.contoso.com, server2.contoso.com*. Lorsque vous avez terminé, sélectionnez **suivant**.
5. Sur le **Options de test** page, sélectionnez **exécuter tous les tests (recommandés)** , puis sélectionnez **suivant**.
6. Sur le **Confirmation** page, sélectionnez **suivant**.

    La page de validation affiche l'état des tests en cours d'exécution.
7. Dans la page **Résumé**, procédez de l'une ou l'autre des façons suivantes :
    
      - Si les résultats indiquent que les tests ont réussi et la configuration est adaptée au clustering, et que vous souhaitez créer le cluster immédiatement, assurez-vous que le **créer le cluster maintenant en utilisant les nœuds validés** vérifier zone est sélectionnée, puis sélectionnez **Terminer**. Ensuite, passez à l'étape 4 de la procédure [Créer le cluster de basculement](#create-the-failover-cluster).
      - Si les résultats indiquent la présence d’avertissements ou échecs, sélectionnez **afficher le rapport** pour afficher les détails et de déterminer quels problèmes doivent être corrigées. Notez qu'un avertissement dans le cadre d'un test de validation indique que l'aspect en question du cluster de basculement peut être pris en charge, mais qu'il n'est peut-être pas conforme aux meilleures pratiques.
        
        >[!NOTE]
        >Si vous recevez un avertissement pour le test Valider la réservation persistante des espaces de stockage, voir le billet de blog qui explique qu’ [un avertissement de validation du cluster de basculement Windows indique que vos disques ne prennent pas en charge les réservations persistantes pour les espaces de stockage](https://blogs.msdn.microsoft.com/clustering/2013/05/24/validate-storage-spaces-persistent-reservation-test-results-with-warning/) pour plus d’informations.

Pour plus d’informations sur les tests de validation matériels, voir [Validate Hardware for a Failover Cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

## <a name="create-the-failover-cluster"></a>Créer le cluster de basculement

Pour effectuer cette étape, assurez-vous que le compte d'utilisateur avec lequel vous ouvrez la session remplit les conditions décrites dans la section [Vérifier les conditions préalables](#verify-the-prerequisites) de cette rubrique.

1. Démarrez le Gestionnaire de serveur.
2. Sur le **outils** menu, sélectionnez **Gestionnaire du Cluster de basculement**.
3. Dans le **Gestionnaire du Cluster de basculement** volet, sous **gestion**, sélectionnez **création d’un Cluster**.
    
    L'Assistant Création d'un cluster s'ouvre.
4. Sur le **avant de commencer** page, sélectionnez **suivant**.
5. Si le **sélectionner des serveurs** page s’affiche, dans le **entrer le nom du** zone, entrez le nom NetBIOS ou le nom de domaine complet d’un serveur que vous souhaitez ajouter en tant qu’un nœud de cluster de basculement et sélectionnez **Ajouter**. Répétez cette étape pour chaque serveur à ajouter. Pour ajouter plusieurs serveurs à la fois, séparez les noms d’une virgule ou d’un point-virgule. Par exemple, entrez les noms au format *server1.contoso.com; server2.contoso.com*. Lorsque vous avez terminé, sélectionnez **suivant**.
    
    >[!NOTE]
    >Si vous avez choisi de créer le cluster immédiatement après l’exécution de la validation le [configuration validation procédure](#validate-the-configuration), vous ne verrez pas le **sélectionner des serveurs** page. Les nœuds qui ont été validés sont ajoutés automatiquement à l'Assistant Création d'un cluster, si bien que vous n'avez pas besoin de les entrer à nouveau.
6. Si vous avez passé l'étape de validation antérieure, la page **Avertissement de validation** s'affiche. Nous vous recommandons vivement d'exécuter la validation du cluster. Microsoft assure uniquement la prise en charge des clusters qui ont réussi tous les tests de validation. Pour exécuter les tests de validation, sélectionnez **Oui**, puis sélectionnez **suivant**. Terminez l’Assistant Validation d’une Configuration comme décrit dans [valider la configuration](#validate-the-configuration).
7. Dans la page **Point d'accès pour l'administration du cluster**, procédez comme suit :
    
    1. Dans la zone **Nom du cluster** , entrez le nom que vous voulez utiliser pour administrer le cluster. Avant cela, prenez connaissance des informations suivantes :
        
          - Pendant la création du cluster, ce nom est inscrit en tant qu'objet ordinateur de cluster (aussi appelé *objet nom de cluster* ou *CNO*) dans AD DS. Si vous spécifiez un nom NetBIOS pour le cluster, le CNO est créé à l'emplacement où résident les objets ordinateur des nœuds du cluster. Il peut s'agir soit du conteneur Ordinateurs par défaut, soit d'une UO.
          - Pour spécifier un autre emplacement pour le CNO, vous pouvez entrer le nom unique d'une UO dans la zone **Nom du cluster** . Exemple : *CN=ClusterName, OU=Clusters, DC=Contoso, DC=com*.
          - Si un administrateur du domaine a prédéfini le CNO dans une UO différente de celle où résident les nœuds du cluster, spécifiez le nom unique fourni par l'administrateur du domaine.
    2. Si la carte réseau du serveur n'a pas été configurée pour utiliser DHCP, vous devez configurer une ou plusieurs adresses IP statiques pour le cluster de basculement. Cochez la case correspondant à chaque réseau que vous voulez utiliser pour la gestion du cluster. Sélectionnez le **adresse** champ en regard d’un réseau sélectionné, puis entrez l’adresse IP que vous souhaitez affecter au cluster. Cette adresse IP (et les autres éventuelles) est associée au nom de cluster dans le système DNS (Domain Name System).
    3. Lorsque vous avez terminé, sélectionnez **suivant**.
8. Dans la page **Confirmation** , examinez les paramètres. Par défaut, la case **Ajouter la totalité du stockage disponible au cluster** est cochée. Décochez cette case dans l'un des cas suivants :
    
      - Vous souhaitez configurer le stockage à un moment ultérieur.
      - Vous envisagez de créer des espaces de stockage en cluster via le Gestionnaire du cluster de basculement ou les applets de commande de clustering de basculement Windows PowerShell, et vous n'avez pas encore créé d'espaces de stockage dans Services de fichiers et de stockage. Pour plus d'informations, voir [Deploy Clustered Storage Spaces](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>).
9. Sélectionnez **suivant** pour créer le cluster de basculement.
10. Dans la page **Résumé**, vérifiez que le cluster de basculement a bien été créé. En présence d’avertissements ou d’erreurs, examinez le résumé généré ou sélectionnez **afficher le rapport** pour afficher le rapport complet. Sélectionnez **Terminer**.
11. Pour vérifier que le cluster a été créé, assurez-vous que le nom du cluster figure bien sous **Gestionnaire du cluster de basculement** dans l'arborescence de navigation. Vous pouvez développer le nom du cluster, puis sélectionnez les éléments sous **nœuds**, **stockage** ou **réseaux** pour afficher les ressources associées.
    
    Notez que la réplication du nom du cluster dans DNS peut prendre un certain temps. Après l’inscription DNS réussie et la réplication, si vous sélectionnez **tous les serveurs** dans le Gestionnaire de serveur, le nom du cluster doit être répertorié en tant que serveur avec un **la facilité de gestion** état de **en ligne** .

Après avoir créé le cluster, vous pouvez effectuer certaines opérations comme vérifier la configuration de quorum du cluster et créer éventuellement des volumes partagés de cluster. Pour plus d’informations, consultez [Quorum de présentation dans les espaces de stockage Direct](../storage/storage-spaces/understand-quorum.md) et [Use Cluster Shared Volumes dans un cluster de basculement](failover-cluster-csvs.md).

## <a name="create-clustered-roles"></a>Créer des rôles en cluster

Après avoir créé le cluster de basculement, vous pouvez créer des rôles en cluster pour héberger des charges de travail de cluster.

>[!NOTE]
>Pour les rôles en cluster qui nécessitent un point d'accès client, un objet ordinateur virtuel est créé dans AD DS. Par défaut, tous les objets ordinateur virtuel du cluster sont créés dans le même conteneur ou la même UO en tant que CNO. Notez qu'après avoir créé un cluster, vous pouvez déplacer le CNO vers n'importe quelle UO.

Voici comment créer un rôle en cluster :

1. Utilisez le Gestionnaire de serveur ou Windows PowerShell pour installer le rôle ou la fonctionnalité nécessaire à un rôle en cluster sur chaque nœud de cluster de basculement. Par exemple, si vous voulez créer un serveur de fichiers en cluster, installez le rôle de serveur de fichiers sur tous les nœuds du cluster.
    
    Le tableau suivant présente les rôles en cluster que vous pouvez configurer dans l'Assistant Haute disponibilité, ainsi que le rôle serveur associé ou la fonctionnalité que vous devez installer comme condition préalable.
    

|Rôle en cluster  |Rôle ou fonctionnalité prérequis  |
|---------|---------|
|Serveur de Namespace     |   Espaces de noms (partie du rôle de serveur de fichiers)       |
|Serveur d'espace de noms DFS     |  Rôle de serveur DHCP       |
|Coordinateur de transactions distribuées (DTC)     | Aucune        |
|Serveur de fichiers     |  Rôle de serveur de fichiers       |
|Application générique     |  Non applicable       |
|Script générique     |   Non applicable      |
|Service générique     |   Non applicable      |
|Service Broker de réplication Hyper-V     |   Rôle Hyper-V      |
|iSCSI Target Server     |    Serveur cible iSCSI (partie du rôle de serveur de fichiers)     |
|Serveur iSNS     |  Fonctionnalité Service serveur iSNS       |
|Message Queuing     |  Fonctionnalité Services Message Queuing       |
|Autre serveur     |  Aucune       |
|Ordinateur virtuel     |  Rôle Hyper-V       |
|Serveur WINS     |   Fonctionnalité Serveur WINS      |

2. Dans le Gestionnaire de Cluster de basculement, développez le nom du cluster, cliquez sur **rôles**, puis sélectionnez **configurer un rôle**.
3. Suivez les étapes de l'Assistant Haute disponibilité pour créer le rôle en cluster.
4. Pour vérifier que le rôle en cluster a été créé, dans le volet **Rôles**, assurez-vous que l'état du rôle est **Exécution en cours**. Le volet Rôles indique aussi le nœud propriétaire. Pour tester le basculement, cliquez sur le rôle, pointez sur **déplacer**, puis sélectionnez **sélectionner un nœud**. Dans le **déplacer le rôle en cluster** boîte de dialogue, sélectionnez le nœud de cluster souhaité, puis **OK**. Dans la colonne **Nœud propriétaire** , vérifiez que le nœud propriétaire a changé.

## <a name="create-a-failover-cluster-by-using-windows-powershell"></a>Créer un cluster de basculement à l'aide de Windows PowerShell

Les applets de commande Windows PowerShell suivantes effectuer les mêmes fonctions que les procédures précédentes de cette rubrique. Entrez chaque applet de commande sur une seule ligne, même si elles tiennent sur plusieurs lignes du fait de contraintes de mise en forme.

>[!NOTE]
>Vous devez utiliser Windows PowerShell pour créer un cluster détaché d’Active Directory dans Windows Server 2012 R2. Pour plus d’informations sur la syntaxe, voir [Deploy an Active Directory-Detached Cluster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

L'exemple suivant installe la fonctionnalité de clustering de basculement.

```PowerShell
Install-WindowsFeature –Name Failover-Clustering –IncludeManagementTools
```

L'exemple suivant exécute tous les tests de validation de cluster sur les ordinateurs nommés *Server1* et *Server2*.

```PowerShell
Test-Cluster –Node Server1, Server2
```

>[!NOTE]
>Le **Test-Cluster** applet de commande renvoie les résultats vers un fichier journal dans le répertoire de travail actuel. Exemple : C:\Users\<nom d’utilisateur > \AppData\Local\Temp.

L'exemple suivant crée un cluster de basculement nommé *MyCluster* avec les nœuds *Server1* et *Server2*, attribue l'adresse IP statique *192.168.1.12*, puis ajoute la totalité du stockage disponible au cluster de basculement.

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12
```

L'exemple suivant crée le même cluster de basculement que dans l'exemple précédent, mais il n'ajoute pas le stockage disponible au cluster de basculement.

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12 -NoStorage
```

L'exemple suivant crée un cluster nommé *MyCluster* dans l'UO *Cluster* du domaine *Contoso.com*.

```PowerShell
New-Cluster -Name CN=MyCluster,OU=Cluster,DC=Contoso,DC=com -Node Server1, Server2
```

Pour obtenir des exemples d’ajout de rôles en cluster, voir les rubriques [Add-ClusterFileServerRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clusterfileserverrole?view=win10-ps) et [Add-ClusterGenericApplicationRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clustergenericapplicationrole?view=win10-ps).

## <a name="more-information"></a>Informations supplémentaires

  - [Clustering de basculement](failover-clustering.md)
  - [Déployer un Cluster Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj863389(v%3dws.11)>)
  - [Serveur de fichiers de montée en puissance pour les données d’Application](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831349(v%3dws.11)>)
  - [Déployer un Cluster détaché d’annuaire Active](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))
  - [À l’aide du Clustering invité pour la haute disponibilité](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)
  - [ Mise à jour adaptée aux clusters](cluster-aware-updating.md)
  - [Nouveau Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/new-cluster?view=win10-ps)
  - [Cluster de test](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps)
