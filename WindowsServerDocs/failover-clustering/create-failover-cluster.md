---
title: créer un cluster de basculement
description: Comment créer un cluster de basculement pour Windows Server 2012 R2, Windows Server 2012, Windows Server 2016 et Windows Server 2019.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
manager: lizross
ms.technology: storage-failover-clustering
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: 03e5f875b19a9d903f7cefa9d2174b6e60b44c13
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80827812"
---
# <a name="create-a-failover-cluster"></a>créer un cluster de basculement

>S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Cette rubrique montre comment créer un cluster de basculement à l'aide du composant logiciel enfichable Gestionnaire du cluster de basculement ou de Windows PowerShell. Elle s'appuie sur un déploiement classique dans lequel les objets ordinateur du cluster et ses rôles en cluster associés sont créés dans les services de domaine Active Directory (AD DS). Si vous déployez un cluster espaces de stockage direct, consultez déployer des [espaces de stockage direct](../storage/storage-spaces/deploy-storage-spaces-direct.md).

Vous pouvez également déployer un cluster détaché Active Directory. Cette méthode de déploiement vous permet de créer un cluster de basculement sans les autorisations nécessaires pour créer des objets ordinateur dans AD DS ni le besoin de demander à ce que les objets ordinateur soient prédéfinis dans AD DS. Cette option est disponible uniquement via Windows PowerShell, et elle n'est recommandée que pour certains scénarios spécifiques. Pour plus d’informations, voir [Déployer un cluster détaché d’Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

#### <a name="checklist-create-a-failover-cluster"></a>Liste de vérification : créer un cluster de basculement

| Statut | Tâche | Référence |
| ---    | ---  | ---       |
| ☐    | Vérifier les conditions préalables | [Vérifier les conditions préalables](#verify-the-prerequisites) |
| ☐    | Installer la fonctionnalité de clustering de basculement sur chaque serveur à ajouter comme nœud de cluster | [Installer la fonctionnalité de clustering de basculement](#install-the-failover-clustering-feature) |
| ☐    | Exécuter l'Assistant Validation de cluster pour valider la configuration | [Valider la configuration](#validate-the-configuration) |
| ☐ | Exécuter l'Assistant Création d'un cluster pour créer le cluster de basculement | [Créer le cluster de basculement](#create-the-failover-cluster) |
| ☐ | Créer des rôles en cluster pour héberger des charges de travail de cluster | [Créer des rôles en cluster](#create-clustered-roles) |

## <a name="verify-the-prerequisites"></a>Vérifier les conditions préalables

Avant de commencer, vérifiez les conditions préalables suivantes :

- Assurez-vous que l'ensemble des serveurs que vous voulez ajouter en tant que nœuds de cluster exécutent la même version de Windows Server.
- Examinez la configuration matérielle requise pour vous assurer que votre configuration est prise en charge. Pour plus d'informations, voir [Failover Clustering Hardware Requirements and Storage Options](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj612869(v%3dws.11)). Si vous créez un cluster espaces de stockage direct, consultez [espaces de stockage direct configuration matérielle requise](../storage/storage-spaces/storage-spaces-direct-hardware-requirements.md).
- Pour ajouter un stockage en cluster lors de la création du cluster, assurez-vous que tous les serveurs peuvent accéder au stockage. (Vous pouvez aussi ajouter un stockage en cluster après avoir créé le cluster.)
- Vérifiez que l'ensemble des serveurs à ajouter comme nœuds de cluster sont membres du même domaine Active Directory.
- (Facultatif) Créez une unité d'organisation (UO) et déplacez les comptes d'ordinateurs pour les serveurs à ajouter comme nœuds de cluster dans l'UO. Nous vous recommandons, à titre de meilleure pratique, de placer les clusters de basculement dans leur propre UO dans AD DS. Cela peut vous aider à mieux repérer les paramètres de stratégie de groupe ou les paramètres de modèle de sécurité qui affectent les nœuds de cluster. Le fait d'isoler les clusters dans leur propre UO vous aide aussi à vous prémunir contre les risques de suppression accidentelle d'objets ordinateur de cluster.

Par ailleurs, vérifiez les conditions requises en matière de comptes :

- Assurez-vous que le compte que vous envisagez d'utiliser pour créer le cluster est un utilisateur de domaine qui dispose de droits d'administrateur sur tous les serveurs que vous voulez ajouter en tant que nœuds de cluster.
- Assurez-vous que l'une ou l'autre des conditions suivantes est vraie :
    - L'utilisateur qui crée le cluster dispose de l'autorisation de **création d'objets ordinateur** sur l'UO ou le conteneur où résident les serveurs qui constitueront le cluster.
    - Si l'utilisateur ne dispose pas de l'autorisation de **création d'objets ordinateur**, demandez à un administrateur de domaine de prédéfinir un objet ordinateur de cluster pour le cluster. Pour plus d'informations, voir [Prédéfinir des objets ordinateur de cluster dans les services de domaine Active Directory](prestage-cluster-adds.md).

> [!NOTE]
> Cette condition ne s’applique pas si vous souhaitez créer un cluster Active Directory détaché dans Windows Server 2012 R2. Pour plus d’informations, voir [Déployer un cluster détaché d’Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

## <a name="install-the-failover-clustering-feature"></a>Installer la fonctionnalité de clustering de basculement

Vous devez installer la fonctionnalité de clustering de basculement sur chaque serveur à ajouter comme nœud de cluster.

### <a name="install-the-failover-clustering-feature"></a>Installer la fonctionnalité de clustering de basculement

1. Démarrez le Gestionnaire de serveur.
2. Dans le menu **gérer** , sélectionnez **Ajouter des rôles et des fonctionnalités**.
3. Dans la page **avant de commencer** , sélectionnez **suivant**.
4. Dans la page **Sélectionner le type d’installation** , sélectionnez installation basée sur **un rôle ou une fonctionnalité**, puis sélectionnez **suivant**.
5. Dans la page **Sélectionner le serveur de destination** , sélectionnez le serveur sur lequel vous voulez installer la fonctionnalité, puis sélectionnez **suivant**.
6. Dans la page **Sélectionner des rôles serveurs**, sélectionnez **Suivant**.
7. Dans la page **Sélectionner des fonctionnalités**, cochez la case **Clustering de basculement**.
8. Pour installer les outils de gestion du cluster de basculement, sélectionnez **Ajouter des fonctionnalités**, puis cliquez sur **suivant**.
9. Dans la page **confirmer les sélections d’installation** , sélectionnez **installer**.
<br>La fonctionnalité de clustering de basculement ne nécessite pas le redémarrage du serveur.

10. Une fois l’installation terminée, sélectionnez **Fermer**.
11. Répétez cette procédure sur chaque serveur à ajouter comme nœud de cluster.

> [!NOTE]
> Après avoir installé la fonctionnalité de clustering de basculement, nous vous recommandons d'appliquer les dernières mises à jour de Windows Update. En outre, pour un cluster de basculement basé sur Windows Server 2012, passez en revue les [correctifs et mises à jour recommandés pour les clusters de basculement basés sur Windows server 2012](https://support.microsoft.com/help/2784261/recommended-hotfixes-and-updates-for-windows-server-2012-based-failove) support Microsoft article et installez les mises à jour qui s’appliquent.

## <a name="validate-the-configuration"></a>Valider la configuration

Avant de créer le cluster de basculement, nous vous recommandons vivement de valider la configuration en vérifiant que les paramètres matériels et logiciels sont compatibles avec le clustering de basculement. Microsoft ne prend en charge une solution de cluster que si elle réussit tous les tests de validation et si tous les composants matériels sont certifiés pour la version de Windows Server exécutée sur les nœuds de cluster.

> [!NOTE]
> Pour exécuter tous les tests, vous devez disposer d'au moins deux nœuds. Si vous n'en avez qu'un, la plupart des tests de stockage importants ne s'exécuteront pas.

### <a name="run-cluster-validation-tests"></a>Exécuter les tests de validation de cluster

1. Sur un ordinateur doté des outils de gestion du cluster de basculement installés à partir des outils d'administration de serveur distant, ou sur un serveur sur lequel vous avez installé la fonctionnalité de clustering de basculement, démarrez le Gestionnaire du cluster de basculement. Pour effectuer cette opération sur un serveur, démarrez Gestionnaire de serveur, puis, dans le menu **Outils** , sélectionnez **Gestionnaire du cluster de basculement**.
2. Dans le volet **Gestionnaire du cluster de basculement** , sous **gestion**, sélectionnez **valider la configuration**.
3. Dans la page **avant de commencer** , sélectionnez **suivant**.
4. Dans la page **Sélectionner des serveurs ou un cluster** , dans la zone **entrer le nom** , entrez le nom NetBIOS ou le nom de domaine complet d’un serveur que vous prévoyez d’ajouter en tant que nœud de cluster de basculement, puis sélectionnez **Ajouter**. Répétez cette étape pour chaque serveur à ajouter. Pour ajouter plusieurs serveurs à la fois, séparez les noms d’une virgule ou d’un point-virgule. Par exemple, entrez les noms au format `server1.contoso.com, server2.contoso.com`. Lorsque vous avez terminé, sélectionnez **suivant**.
5. Dans la page **options de test** , sélectionnez **exécuter tous les tests (recommandé)** , puis cliquez sur **suivant**.
6. Dans la page **confirmation** , sélectionnez **suivant**.

    La page de validation affiche l'état des tests en cours d'exécution.
7. Dans la page **Résumé**, procédez de l'une ou l'autre des façons suivantes :
    
      - Si les résultats indiquent que les tests se sont terminés correctement et que la configuration est adaptée au clustering, et que vous souhaitez créer le cluster immédiatement, assurez-vous que la case à cocher **créer le cluster maintenant en utilisant les nœuds validés** est activée, puis sélectionnez **Terminer**. Ensuite, passez à l'étape 4 de la procédure [Créer le cluster de basculement](#create-the-failover-cluster).
      - Si les résultats indiquent que des avertissements ou des échecs se sont produits, sélectionnez **afficher le rapport** pour afficher les détails et déterminer les problèmes qui doivent être corrigés. Notez qu'un avertissement dans le cadre d'un test de validation indique que l'aspect en question du cluster de basculement peut être pris en charge, mais qu'il n'est peut-être pas conforme aux meilleures pratiques.
        
        > [!NOTE]
        > Si vous recevez un avertissement pour le test Valider la réservation persistante des espaces de stockage, voir le billet de blog qui explique qu’ [un avertissement de validation du cluster de basculement Windows indique que vos disques ne prennent pas en charge les réservations persistantes pour les espaces de stockage](https://blogs.msdn.microsoft.com/clustering/2013/05/24/validate-storage-spaces-persistent-reservation-test-results-with-warning/) pour plus d’informations.

Pour plus d'informations sur les tests de validation du matériel, voir [Valider le matériel pour un cluster de basculement](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

## <a name="create-the-failover-cluster"></a>Créer le cluster de basculement

Pour effectuer cette étape, assurez-vous que le compte d'utilisateur avec lequel vous ouvrez la session remplit les conditions décrites dans la section [Vérifier les conditions préalables](#verify-the-prerequisites) de cette rubrique.

1. Démarrez le Gestionnaire de serveur.
2. Dans le menu **Outils** , sélectionnez **Gestionnaire du cluster de basculement**.
3. Dans le volet **Gestionnaire du cluster de basculement** , sous **gestion**, sélectionnez **créer un cluster**.
    
    L'Assistant Création d'un cluster s'ouvre.
4. Dans la page **avant de commencer** , sélectionnez **suivant**.
5. Si la page **Sélectionner les serveurs** s’affiche, dans la zone entrer le **nom** , entrez le nom NetBIOS ou le nom de domaine complet d’un serveur que vous envisagez d’ajouter en tant que nœud de cluster de basculement, puis sélectionnez **Ajouter**. Répétez cette étape pour chaque serveur à ajouter. Pour ajouter plusieurs serveurs à la fois, séparez les noms d’une virgule ou d’un point-virgule. Par exemple, entrez les noms au format *server1.contoso.com; server2.contoso.com*. Lorsque vous avez terminé, sélectionnez **suivant**.
    
    > [!NOTE]
    > Si vous avez choisi de créer le cluster immédiatement après l’exécution de la validation dans la [procédure de validation](#validate-the-configuration)de la configuration, la page **Sélectionner les serveurs** ne s’affiche pas. Les nœuds qui ont été validés sont ajoutés automatiquement à l'Assistant Création d'un cluster, si bien que vous n'avez pas besoin de les entrer à nouveau.
6. Si vous avez passé l'étape de validation antérieure, la page **Avertissement de validation** s'affiche. Nous vous recommandons vivement d'exécuter la validation du cluster. Microsoft assure uniquement la prise en charge des clusters qui ont réussi tous les tests de validation. Pour exécuter les tests de validation, sélectionnez **Oui**, puis cliquez sur **suivant**. Exécutez l’Assistant validation d’une configuration comme décrit dans [valider la configuration](#validate-the-configuration).
7. Dans la page **Point d'accès pour l'administration du cluster**, procédez comme suit :
    
    1. Dans la zone **Nom du cluster**, entrez le nom que vous voulez utiliser pour administrer le cluster. Avant cela, prenez connaissance des informations suivantes :
        
          - Pendant la création du cluster, ce nom est inscrit en tant qu'objet ordinateur de cluster (aussi appelé *objet nom de cluster* ou *CNO*) dans AD DS. Si vous spécifiez un nom NetBIOS pour le cluster, le CNO est créé à l'emplacement où résident les objets ordinateur des nœuds du cluster. Il peut s'agir soit du conteneur Ordinateurs par défaut, soit d'une UO.
          - Pour spécifier un autre emplacement pour le CNO, vous pouvez entrer le nom unique d'une UO dans la zone **Nom du cluster**. Par exemple : *CN=ClusterName, OU=Clusters, DC=Contoso, DC=com*.
          - Si un administrateur du domaine a prédéfini le CNO dans une UO différente de celle où résident les nœuds du cluster, spécifiez le nom unique fourni par l'administrateur du domaine.
    2. Si la carte réseau du serveur n'a pas été configurée pour utiliser DHCP, vous devez configurer une ou plusieurs adresses IP statiques pour le cluster de basculement. Cochez la case correspondant à chaque réseau que vous voulez utiliser pour la gestion du cluster. Sélectionnez le champ **adresse** en regard d’un réseau sélectionné, puis entrez l’adresse IP que vous souhaitez affecter au cluster. Cette adresse IP (et les autres éventuelles) est associée au nom de cluster dans le système DNS (Domain Name System).
    
      >[!NOTE]
      > Si vous utilisez Windows Server 2019, vous avez la possibilité d’utiliser un nom de réseau distribué pour le cluster. Un nom de réseau distribué utilise les adresses IP des serveurs membres au lieu de demander une adresse IP dédiée pour le cluster. Par défaut, Windows utilise un nom de réseau distribué s’il détecte que vous créez le cluster dans Azure (vous n’êtes donc pas obligé de créer un équilibreur de charge interne pour le cluster) ou une adresse statique ou IP normale si vous exécutez en local. Pour plus d’informations, consultez [nom de réseau distribué](https://blogs.windows.com/windowsexperience/2018/08/14/announcing-windows-server-2019-insider-preview-build-17733/#W0YAxO8BfwBRbkzG.97).
    
    3. Lorsque vous avez terminé, sélectionnez **suivant**.
8. Dans la page **Confirmation**, examinez les paramètres. Par défaut, la case **Ajouter la totalité du stockage disponible au cluster** est cochée. Décochez cette case dans l'un des cas suivants :
    
      - Vous souhaitez configurer le stockage à un moment ultérieur.
      - Vous envisagez de créer des espaces de stockage en cluster via le Gestionnaire du cluster de basculement ou les applets de commande de clustering de basculement Windows PowerShell, et vous n'avez pas encore créé d'espaces de stockage dans Services de fichiers et de stockage. Pour plus d'informations, voir [Deploy Clustered Storage Spaces](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>).
9. Sélectionnez **suivant** pour créer le cluster de basculement.
10. Dans la page **Résumé**, vérifiez que le cluster de basculement a bien été créé. Si des avertissements ou des erreurs se sont produits, affichez le résumé de la sortie ou sélectionnez **afficher le rapport** pour afficher le rapport complet. Sélectionnez **Terminer**.
11. Pour vérifier que le cluster a été créé, assurez-vous que le nom du cluster figure bien sous **Gestionnaire du cluster de basculement** dans l'arborescence de navigation. Vous pouvez développer le nom du cluster, puis sélectionner des éléments sous **nœuds**, **stockage** ou **réseaux** pour afficher les ressources associées.
    
    Notez que la réplication du nom du cluster dans DNS peut prendre un certain temps. Une fois l’inscription et la réplication DNS réussies, si vous sélectionnez **tous les serveurs** dans Gestionnaire de serveur, le nom du cluster doit être indiqué en tant que serveur avec l’état de **facilité de gestion** **en ligne**.

Après avoir créé le cluster, vous pouvez effectuer certaines opérations comme vérifier la configuration de quorum du cluster et créer éventuellement des volumes partagés de cluster. Pour plus d’informations, consultez [comprendre le quorum dans espaces de stockage direct](../storage/storage-spaces/understand-quorum.md) et [utiliser des volumes partagés de cluster dans un cluster de basculement](failover-cluster-csvs.md).

## <a name="create-clustered-roles"></a>Créer des rôles en cluster

Après avoir créé le cluster de basculement, vous pouvez créer des rôles en cluster pour héberger des charges de travail de cluster.

>[!NOTE]
>Pour les rôles en cluster qui nécessitent un point d'accès client, un objet ordinateur virtuel est créé dans AD DS. Par défaut, tous les objets ordinateur virtuel du cluster sont créés dans le même conteneur ou la même UO en tant que CNO. Notez qu'après avoir créé un cluster, vous pouvez déplacer le CNO vers n'importe quelle UO.

Voici comment créer un rôle en cluster :

1. Utilisez le Gestionnaire de serveur ou Windows PowerShell pour installer le rôle ou la fonctionnalité nécessaire à un rôle en cluster sur chaque nœud de cluster de basculement. Par exemple, si vous voulez créer un serveur de fichiers en cluster, installez le rôle de serveur de fichiers sur tous les nœuds du cluster.
    
    Le tableau suivant présente les rôles en cluster que vous pouvez configurer dans l'Assistant Haute disponibilité, ainsi que le rôle serveur associé ou la fonctionnalité que vous devez installer comme condition préalable.

   | Rôle en cluster  | Rôle ou fonctionnalité prérequis  |
   | ---------       | ---------                    |
   | Serveur d’espaces de noms     |   Espaces de noms (composant du rôle de serveur de fichiers)       |
   | Serveur d'espace de noms DFS     |  Rôle de serveur DHCP       |
   | DTC (Distributed Transaction Coordinator)     | Aucune        |
   | Serveur de fichiers     |  Rôle de serveur de fichiers       |
   | Application générique     |  Non applicable       |
   | Script générique     |   Non applicable      |
   | Service générique     |   Non applicable      |
   | Service Broker de réplication Hyper-V     |   Rôle Hyper-V      |
   | Serveur cible iSCSI     |    Serveur cible iSCSI (partie du rôle de serveur de fichiers)     |
   | Serveur iSNS     |  Fonctionnalité Service serveur iSNS       |
   | Message Queuing (page pouvant être en anglais)     |  Fonctionnalité Services Message Queuing       |
   | Autre serveur     |  Aucune       |
   | Virtual Machine     |  Rôle Hyper-V       |
   | Serveur WINS     |   Fonctionnalité Serveur WINS      |

2. Dans Gestionnaire du cluster de basculement, développez le nom du cluster, cliquez avec le bouton droit sur **rôles**, puis sélectionnez **configurer le rôle**.
3. Suivez les étapes de l'Assistant Haute disponibilité pour créer le rôle en cluster.
4. Pour vérifier que le rôle en cluster a été créé, dans le volet **Rôles**, assurez-vous que l'état du rôle est **Exécution en cours**. Le volet Rôles indique aussi le nœud propriétaire. Pour tester le basculement, cliquez avec le bouton droit sur le rôle, pointez sur **déplacer**, puis sélectionnez **Sélectionner un nœud**. Dans la boîte de dialogue **déplacer le rôle en cluster** , sélectionnez le nœud de cluster souhaité, puis cliquez sur **OK**. Dans la colonne **Nœud propriétaire**, vérifiez que le nœud propriétaire a changé.

## <a name="create-a-failover-cluster-by-using-windows-powershell"></a>Créer un cluster de basculement à l'aide de Windows PowerShell

Les applets de commande Windows PowerShell suivantes effectuent les mêmes fonctions que les procédures précédentes de cette rubrique. Entrez chaque applet de commande sur une seule ligne, même si elles tiennent sur plusieurs lignes du fait de contraintes de mise en forme.

> [!NOTE]
> Vous devez utiliser Windows PowerShell pour créer un cluster Active Directory détaché dans Windows Server 2012 R2. Pour plus d'informations sur la syntaxe, voir [Déployer un cluster détaché d'Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

L'exemple suivant installe la fonctionnalité de clustering de basculement.

```PowerShell
Install-WindowsFeature –Name Failover-Clustering –IncludeManagementTools
```

L'exemple suivant exécute tous les tests de validation de cluster sur les ordinateurs nommés *Server1* et *Server2*.

```PowerShell
Test-Cluster –Node Server1, Server2
```

> [!NOTE]
> L’applet de commande **test-cluster** génère les résultats dans un fichier journal dans le répertoire de travail actuel. Par exemple : C:\Users\<nom d’utilisateur > \AppData\Local\Temp.

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

  - [Clustering avec basculement](failover-clustering.md)
  - [Déployer un cluster Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj863389(v%3dws.11)>)
  - [Serveur de fichiers avec montée en puissance parallèle pour les données d’application](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831349(v%3dws.11)>)
  - [Déployer un cluster détaché Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))
  - [Utilisation du clustering invité pour la haute disponibilité](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)
  - [ Mise à jour adaptée aux clusters](cluster-aware-updating.md)
  - [Nouveau cluster](https://docs.microsoft.com/powershell/module/failoverclusters/new-cluster?view=win10-ps)
  - [Test-cluster](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps)
