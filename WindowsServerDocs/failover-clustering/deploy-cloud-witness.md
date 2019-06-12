---
ms.assetid: 0cd1ac70-532c-416d-9de6-6f920a300a45
title: Déployer un témoin cloud pour un Cluster de basculement
ms.prod: windows-server-threshold
manager: eldenc
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.topic: article
author: JasonGerend
ms.date: 01/18/2019
description: Comment utiliser Microsoft Azure pour héberger le témoin d’un Cluster de basculement Windows Server dans le cloud - également appelé comment déployer un témoin Cloud.
ms.openlocfilehash: 64fd39a37c63d24f8fc0eb4f45c8a7e9f6089013
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439781"
---
# <a name="deploy-a-cloud-witness-for-a-failover-cluster"></a>Déployer un témoin de cloud pour un cluster de basculement

> S’applique à : Windows Server 2019, Windows Server 2016

Témoin de cloud est un type de témoin de quorum de Cluster de basculement qui utilise Microsoft Azure pour fournir un vote du quorum de cluster. Cette rubrique fournit une vue d’ensemble de la fonctionnalité de témoin de Cloud, les scénarios pris en charge et des instructions sur la façon de configurer un témoin cloud pour un Cluster de basculement.

## <a name="CloudWitnessOverview"></a>Vue d’ensemble du témoin de cloud

La figure 1 illustre une configuration de quorum de Cluster de basculement multisite étirée avec Windows Server 2016. Dans cet exemple de configuration (figure 1), il existe 2 nœuds dans 2 centres de données (appelés Sites). Notez, il est possible pour un cluster de s’étendre sur plus de 2 centres de données. En outre, chaque centre de données peut avoir plus de 2 nœuds. Une configuration de quorum de cluster typique dans ce programme d’installation (contrat SLA du basculement automatique) donne à chaque nœud un vote. Vote supplémentaire est donné au témoin de quorum pour autoriser le cluster à conserver en cours d’exécution même si celui du centre de données subit une panne d’alimentation. Le calcul est simple : il existe 5 nombre total de votes et vous avez besoin des 3 votes pour le cluster garantir son fonctionnement.  

![Témoin de partage de fichiers dans un troisième distinct de site avec 2 nœuds dans les 2 autres sites](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_1.png "témoin de partage de fichiers")  
**Figure 1 : À l’aide d’un témoin de partage de fichiers en tant que témoin de quorum**  

En cas de panne d’alimentation dans un centre de données, pour donner la possibilité d’égal pour le cluster dans un autre centre de données pour qu’il soit en cours d’exécution, il est recommandé d’héberger le témoin de quorum dans un emplacement autre que les deux centres de données. Cela signifie que de nécessiter une troisième sépare généralement centre de données (site) pour héberger un serveur de fichiers qui sauvegarde le partage de fichiers qui est utilisé en tant que le témoin de quorum (témoin de partage de fichiers).  

La plupart des organisations n’ont pas de séparer le centre de données qui hébergera le serveur de fichiers témoin de partage de fichiers de stockage une troisième. Cela signifie que les organisations hébergent principalement le serveur de fichiers dans un des deux centres de données, rendant par extension, ce centre de données du centre de données principal. Dans un scénario de cas de panne d’alimentation dans le centre de données principal, le cluster irait vers le bas comme le centre de données autres n’auront que 2 voix qui est inférieur à la majorité du quorum de 3 votes nécessaires. Pour les clients qui ont le troisième centre de données distinct pour héberger le serveur de fichiers, il existe une surcharge pour maintenir le serveur de fichiers hautement disponible, sauvegarde le témoin de partage de fichiers. Hébergement des machines virtuelles dans le cloud public ayant le serveur de fichiers pour le témoin de partage de fichiers en cours d’exécution dans le système d’exploitation invité est une surcharge significative en termes d’installation et de maintenance.  

Témoin de cloud est un nouveau type de témoin de quorum de Cluster de basculement qui s’appuie sur Microsoft Azure comme point d’arbitrage (figure 2). Il utilise le stockage Blob Azure pour lire/écrire un fichier blob qui est ensuite utilisé comme point d’arbitrage en cas de résolution « split brain ».  

Sont importantes qui profite cette approche :
1. S’appuie sur Microsoft Azure (aucun besoin de centre de données troisième distinct).  
2. Utilise le standard disponible le stockage Blob Azure (aucune surcharge de maintenance supplémentaire de machines virtuelles hébergées dans le cloud public).  
3. Même compte de stockage Azure peut être utilisé pour plusieurs clusters (fichier d’un objet blob par cluster ; id unique de cluster utilisé comme nom de fichier blob).  
4. $Cost continu très faible au compte de stockage (très petites données écrites par fichier blob, fichier blob mis à jour une seule fois lorsque l’état des nœuds de cluster passe).  
5. Type de ressource témoin Cloud intégré.  

![Diagramme illustrant un cluster étendu de plusieurs sites avec un témoin de Cloud en tant que témoin de quorum](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_2.png)  
**Figure 2 : Clusters à plusieurs sites étirés avec témoin de Cloud en tant que témoin de quorum**  

Comme indiqué dans la figure 2, aucun tiers distinct site n’est requise. Témoin de cloud, comme n’importe quel autre témoin de quorum, obtient un vote et peut participer à des calculs de quorum.  

## <a name="CloudWitnessSupportedScenarios"></a>Témoin de cloud : Scénarios pris en charge pour le type de rappel unique
Si vous avez un déploiement de Cluster de basculement, où tous les nœuds peuvent accéder à internet (par extension d’Azure), il est recommandé de configurer un témoin de Cloud en tant que votre ressource de témoin de quorum.  

Certains des scénarios pris en charge utilisent de témoin de Cloud comme un témoin de quorum sont les suivantes :  
- Récupération d’urgence étiré clusters multisites (voir figure 2).  
- Clusters de basculement sans stockage partagé (SQL toujours sur etc.).  
- Clusters de basculement en cours d’exécution à l’intérieur du système d’exploitation invités hébergés dans le rôle de Machine virtuelle Microsoft Azure (ou n’importe quel autre cloud public).  
- Clusters de basculement en cours d’exécution à l’intérieur d’invité du système d’exploitation de Machines virtuelles hébergées dans des clouds privés.
- Stockage des clusters avec ou sans stockage partagé, telles que les clusters de serveur de fichiers avec montée en puissance.  
- Clusters de petites succursales (2 nœuds de clusters)  

À compter de Windows Server 2012 R2, il est recommandé de toujours configurer un témoin que le cluster gère automatiquement le vote témoin et les nœuds de votent avec Quorum dynamique.  

## <a name="CloudWitnessSetUp"></a> Configurer un témoin Cloud pour un cluster
Pour configurer un témoin de Cloud en tant que témoin de quorum pour votre cluster, procédez comme suit :
1. Créer un compte de stockage Azure à utiliser comme un témoin Cloud
2. Configurer le témoin de Cloud en tant que témoin de quorum pour votre cluster.

## <a name="create-an-azure-storage-account-to-use-as-a-cloud-witness"></a>Créer un compte de stockage Azure à utiliser comme un témoin Cloud

Cette section décrit comment créer un point de terminaison compte et afficher et copier du stockage des URL et clés d’accès pour ce compte.

Pour configurer un témoin de Cloud, vous devez disposer d’un compte de stockage Azure valide qui peut être utilisé pour stocker le fichier blob (utilisé pour l’arbitrage). Témoin de cloud crée un conteneur connu **témoin de cloud msft** sous le compte de stockage de Microsoft. Témoin de cloud écrit un fichier blob unique par correspondante de cluster de l’ID unique utilisé comme nom de fichier du fichier blob sous ce **témoin de cloud msft** conteneur. Cela signifie que vous pouvez utiliser le même compte de stockage Microsoft Azure pour configurer un témoin Cloud pour plusieurs clusters différents.

Lorsque vous utilisez le même compte de stockage Azure pour la configuration du témoin de Cloud pour plusieurs différents clusters, un seul **témoin de cloud msft** conteneur est créé automatiquement. Ce conteneur contiendra le fichier blob d’un par cluster.

### <a name="to-create-an-azure-storage-account"></a>Pour créer un compte de stockage Azure

1. Se connecter à la [Azure Portal](http://portal.azure.com).
2. Dans le menu Hub, sélectionnez Nouveau -> données + stockage -> compte de stockage.
3. Dans la création d’une page de compte de stockage, procédez comme suit :
    1. Entrez un nom pour votre compte de stockage.
    <br>Les noms de compte de stockage doivent être comprise entre 3 et 24 caractères et peuvent contenir uniquement des lettres minuscules et chiffres. Le nom de compte de stockage doit également être unique au sein d’Azure.
        
    2. Pour **type de compte**, sélectionnez **à usage général**.
    <br>Vous ne pouvez pas utiliser un compte de stockage d’objets Blob pour un témoin de Cloud.
    3. Pour **performances**, sélectionnez **Standard**.
    <br>Vous ne pouvez pas utiliser le stockage Premium Azure pour un témoin de Cloud.
    2. Pour **réplication**, sélectionnez **stockage localement redondant (LRS)** .
    <br>Le Clustering de basculement utilise le fichier blob en tant que point d’arbitrage, ce qui nécessite des garanties de cohérence lors de la lecture des données. Par conséquent, vous devez sélectionner **stockage localement redondant** pour **réplication** type.

### <a name="view-and-copy-storage-access-keys-for-your-azure-storage-account"></a>Afficher et copier les clés d’accès de stockage pour votre compte de stockage Azure

Lorsque vous créez un compte de stockage Microsoft Azure, il est associé à deux clés d’accès qui sont générés automatiquement - clé d’accès primaire et clé d’accès secondaire. Pour la première fois la création d’un témoin de Cloud, utilisez le **clé d’accès primaire**. Il n’existe aucune restriction concernant la clé à utiliser pour le témoin de Cloud.  

#### <a name="to-view-and-copy-storage-access-keys"></a>Pour afficher et copier les clés d’accès de stockage

Dans le portail Azure, accédez à votre compte de stockage, cliquez sur **tous les paramètres** puis cliquez sur **clés d’accès** pour afficher, copier et régénérer les clés d’accès de votre compte. Le panneau clés d’accès inclut également des chaînes de connexion préconfigurées à l’aide de vos clés primaires et secondaires que vous pouvez copier pour l’utiliser dans vos applications (voir figure 4).

![Instantané de la boîte de dialogue Gérer les clés d’accès dans Microsoft Azure](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_4.png)  
**Figure 4 : Clés d’accès de stockage**

### <a name="view-and-copy-endpoint-url-links"></a>Afficher et copier des liens URL de point de terminaison  
Lorsque vous créez un compte de stockage, les URL suivantes sont générées en utilisant le format : `https://<Storage Account Name>.<Storage Type>.<Endpoint>`  

Témoin de cloud utilise toujours **Blob** comme type de stockage. Azure utilise **. core.windows.net** comme point de terminaison. Lorsque vous configurez un témoin de Cloud, il est possible que vous avez configuré un point de terminaison différents conformément à votre scénario (par exemple le centre de données Microsoft Azure en Chine a un autre point de terminaison).  

> [!NOTE]  
> L’URL de point de terminaison est généré automatiquement par la ressource de témoin de Cloud et s’il n’existe aucune étape supplémentaire de configuration nécessaires pour l’URL.  

#### <a name="to-view-and-copy-endpoint-url-links"></a>Pour afficher et copier des liens URL de point de terminaison
Dans le portail Azure, accédez à votre compte de stockage, cliquez sur **tous les paramètres** puis cliquez sur **propriétés** pour afficher et copier votre URL de point de terminaison (voir figure 5).  

![Instantané des liens de point de terminaison de témoin de Cloud](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_5.png)  
**Figure 5 : Des liens URL de point de terminaison de témoin de cloud**

Pour plus d’informations sur la création et la gestion des comptes de stockage Azure, consultez [propos des comptes de stockage Azure](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/)

## <a name="configure-cloud-witness-as-a-quorum-witness-for-your-cluster"></a>Configurer un témoin de Cloud en tant que témoin de quorum pour votre cluster
Configuration d’un témoin cloud est très bien intégrée dans l’Assistant de Configuration de Quorum existant créé dans le Gestionnaire du Cluster de basculement.  

### <a name="to-configure-cloud-witness-as-a-quorum-witness"></a>Pour configurer un témoin de Cloud en tant que témoin de Quorum
1. Lancez le Gestionnaire de Cluster de basculement.
2. Avec le bouton droit, le cluster -> **autres Actions** -> **configurer les paramètres de Cluster Quorum** (voir figure 6). Cette opération lance l’Assistant Configuration de Quorum de Cluster.  
    ![Instantané du chemin d’accès de menu pour configurer les paramètres de Quorum de Cluster dans le Gestionnaire du Cluster de basculement UI](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_7.png) **Figure 6. Paramètres du Quorum du cluster**

3. Sur le **sélectionner les Configurations de Quorum** , sélectionnez **sélectionner le témoin de quorum** (voir figure 7).  

    ![Instantané de « sélectionner le témoin quotrum » case d’option dans l’Assistant Quorum du Cluster](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_8.png)  
    **Figure 7. Sélectionnez la Configuration de Quorum**

4. Sur le **sélectionner le témoin de Quorum** , sélectionnez **configurer un témoin cloud** (voir figure 8).  

    ![Instantané de la case d’option appropriée pour sélectionner un témoin cloud](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_9.png)  
    **Figure 8. Sélectionner le témoin de Quorum**  

5. Sur le **configurer le témoin de Cloud** page, entrez les informations suivantes :  
   1. (Paramètre obligatoire) Nom du compte de stockage Azure.  
   2. (Paramètre obligatoire) Clé d’accès correspondant au compte de stockage.  
       1. Lorsque vous créez pour la première fois, utilisez la clé d’accès primaire (voir figure 5)  
       2. Lors de la rotation de la clé d’accès primaire, utilisez la clé d’accès secondaire (voir figure 5)  
   3. (Paramètre facultatif) Si vous envisagez d’utiliser un point de terminaison de service Azure différentes (par exemple, le service Microsoft Azure en Chine), puis mettez à jour le nom de serveur de point de terminaison.  

      ![Instantané du volet de configuration Cloud Witness dans l’Assistant Quorum du Cluster](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_10.png)  
      **Figure 9 : Configurez pas votre témoin de Cloud**

6. Après la configuration d’un témoin de Cloud, vous pouvez afficher la ressource témoin nouvellement créé dans le Gestionnaire du Cluster de basculement-composant logiciel enfichable (voir figure 10).

    ![Configuration réussie de témoin de Cloud](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_11.png)  
    **Figure 10 : Configuration réussie de témoin de Cloud**

### <a name="configuring-cloud-witness-using-powershell"></a>Configuration du témoin de Cloud à l’aide de PowerShell  
La commande Set-ClusterQuorum PowerShell existante a des nouveaux paramètres supplémentaires correspondant à un témoin de Cloud.  

Vous pouvez configurer un témoin de Cloud à l’aide de la [ `Set-ClusterQuorum` ](https://technet.microsoft.com/library/ee461013.aspx) la commande PowerShell suivante :  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey>
```

Au cas où vous devez utiliser un autre point de terminaison (rare) :  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> -Endpoint <servername>  
```

### <a name="azure-storage-account-considerations-with-cloud-witness"></a>Considérations sur le compte de stockage Azure avec un témoin de Cloud  
Lorsque vous configurez un témoin de Cloud en tant que témoin de quorum pour votre Cluster de basculement, procédez comme suit :
* Au lieu de stocker la clé d’accès, votre Cluster de basculement génère et stocker en toute sécurité un jeton de sécurité d’accès partagé (SAP).  
* Le jeton SAP généré est valide aussi longtemps que la clé d’accès est valide. Lors de la rotation de la clé d’accès primaire, il est important de tout d’abord mettre à jour le témoin de Cloud (sur tous vos clusters qui utilisent ce compte de stockage) avec la clé d’accès secondaire avant la régénération de la clé d’accès primaire.  
* Témoin de cloud utilise l’interface HTTPS REST du service de compte de stockage Azure. Cela signifie qu’il nécessite le port HTTPS soient ouverts sur tous les nœuds de cluster.

### <a name="proxy-considerations-with-cloud-witness"></a>Considérations relatives aux proxy avec témoin de Cloud  
Témoin de cloud utilise le protocole HTTPS (port 443 par défaut) pour établir la communication avec le service blob Azure. Vérifiez que le port HTTPS est accessible via le Proxy de réseau.

## <a name="see-also"></a>Voir aussi
- [Nouveautés du Clustering de basculement dans Windows Server](whats-new-in-failover-clustering.md)
