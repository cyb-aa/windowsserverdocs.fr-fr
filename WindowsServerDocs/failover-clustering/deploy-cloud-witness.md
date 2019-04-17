---
ms.assetid: 0cd1ac70-532c-416d-9de6-6f920a300a45
title: "Déployer un témoin de cloud pour un Cluster de basculement"
ms.prod: windows-server-threshold
manager: eldenc
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.topic: article
author: JasonGerend
ms.date: 10/2/2017
description: "Comment utiliser MicrosoftAzure pour héberger le témoin pour un Cluster de basculement Windows Server dans le cloud - aka comment déployer un témoin de Cloud."
ms.openlocfilehash: 564c6668fcc80a8bd1531c05c142996689de8154
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="deploy-a-cloud-witness-for-a-failover-cluster"></a>Déployer un témoin de Cloud pour un Cluster de basculement

> S’applique à: s’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Un témoin de cloud est un nouveau type de témoin de quorum du Cluster de basculement introduit dans Windows Server2016. Cette rubrique fournit une vue d’ensemble de la fonctionnalité de témoin de Cloud, les scénarios pris en charge et des instructions sur la façon de configurer un témoin de cloud pour un Cluster de basculement exécutant Windows Server2016.

## <a name="CloudWitnessOverview"></a>Vue d’ensemble du témoin de cloud

La figure1 illustre une configuration de quorum de Cluster de basculement multisite étendue avec Windows Server2016. Dans cet exemple de configuration (figure1), il existe 2nœuds dans des centres de 2données (référencés en tant que Sites). Notez qu’il est possible pour un cluster à couvrir les centres de données plus de 2. En outre, chaque centre de données peut avoir plus de 2nœuds. Une configuration de quorum de cluster classique dans ce programme d’installation (basculement automatique SLA) donne un vote à chaque nœud. Un vote supplémentaire est donné le témoin de quorum pour autoriser le cluster pour que celui en cours d’exécution même si l’option du centre de données subit une panne de courant. Le calcul est simple: il existe 5 nombre total de voix, et vous devez 3votes pour le cluster à le conserver en cours d’exécution.  

![Témoin de partage de fichiers dans un troisième distinct de site avec 2nœuds 2autres sites](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_1.png "témoin de partage de fichiers")  
**Figure1: Utilisation d’un témoin de partage de fichiers en tant que témoin de quorum**  

En cas de coupure de courant dans un centre de données, pour donner la possibilité d’égal pour le cluster dans un autre centre de données à le conserver en cours d’exécution, il est recommandé pour héberger le témoin de quorum dans un emplacement autre que les deux centres de données. Cela signifie nécessitant une troisième séparez généralement datacenter (site) pour héberger un serveur de fichiers qui sauvegarde le partage de fichiers qui est utilisé en tant que le témoin de quorum (témoin de partage de fichiers).  

La plupart des organisations n’ont pas un tiers de séparer le centre de données qui hébergera le serveur de fichiers sauvegarde le témoin de partage de fichiers. Cela signifie que les organisations hébergent principalement dans un des deux centres de données qui rend ce centre de données du centre de données principal par extension, le serveur de fichiers. Dans un scénario de cas de panne de courant dans le centre de données principal, le cluster est placée vers le bas, comme le centre de données autres n’auront que 2voix qui est inférieure à la majorité du quorum de 3voix nécessaire. Pour les clients qui ont troisième centre de données distinct pour héberger le serveur de fichiers, il est une surcharge de maintenir le serveur de fichiers hautement disponible sauvegarde le témoin de partage de fichiers. Hébergement de machines virtuelles dans le cloud public qui ont le serveur de fichiers pour un témoin de partage de fichiers en cours d’exécution dans le système d’exploitation invité est une surcharge importante en termes d’installation et de maintenance.  

Un témoin de cloud est un nouveau type de témoin de quorum de Cluster de basculement qui utilise MicrosoftAzure comme point d’arbitrage (figure2). Elle utilise le stockage d’objets Blob Azure en lecture/écriture un fichier objet blob qui est ensuite utilisé comme point d’arbitrage en cas de résolution de «split brain».  

Il existe importantes qui profite cette approche:
1. Tire parti de MicrosoftAzure (aucun besoin de centre de données troisième distinct).  
2. Utilise standard stockage d’objets Blob Azure disponibles (sans surcharge supplémentaires de maintenance de machines virtuelles hébergées dans le cloud public).  
3. Même compte de stockage Azure peut être utilisé pour plusieurs clusters (fichier d’un objet blob par cluster; id unique de cluster utilisé comme nom de fichier d’objet blob).  
4. Très faible $cost continue pour le compte de stockage (données très petites écrites par fichier blob, fichier blob mis à jour qu’une seule fois quand change état des nœuds de cluster).  
5. Type de ressource de témoin de Cloud intégré.  

![Diagramme illustrant un cluster étendu de plusieurs site avec un témoin de Cloud en tant que témoin de quorum](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_2.png)  
**Figure2: Multi-site étirée clusters avec un témoin de Cloud en tant que témoin de quorum**  

Comme indiqué dans la figure2, aucun tiers distinct site n’est requise. Comme n’importe quel autre témoin de quorum, témoin cloud Obtient un vote et peut participer, dans les calculs de quorum.  

## <a name="CloudWitnessSupportedScenarios"></a>Un témoin de cloud: Les scénarios pris en charge pour le type de témoin unique
Si vous avez un déploiement de Cluster de basculement, où tous les nœuds peuvent accéder à internet (par extension de Azure), il est recommandé de configurer un témoin de Cloud en tant que votre ressource de témoin de quorum.  

Certains scénarios pris en charge les utilisent de témoin de Cloud comme un témoin de quorum sont les suivantes:  
- Récupération d’urgence étiré clusters à plusieurs sites (voir figure2).  
- Clusters de basculement sans stockage partagé (SQL toujours sur etc.).  
- Clusters de basculement en cours d’exécution à l’intérieur du système d’exploitation invité hébergé dans un rôle d’ordinateur virtuel MicrosoftAzure (ou tout autre cloud public).  
- Clusters de basculement en cours d’exécution à l’intérieur du système d’exploitation des ordinateurs virtuels invités hébergés dans des clouds privés.
- Stockage des clusters avec ou sans stockage partagé, telles que les clusters de serveurs de fichiers avec montée en puissance parallèle.  
- Clusters de petites filiales (clusters de 2nœuds)  

À compter de Windows Server2012R2, il est recommandé de toujours configurer un témoin de tel que le cluster gère automatiquement le vote témoin et les nœuds de votent quorum dynamique.  

## <a name="CloudWitnessSetUp"></a>Configurer un témoin de Cloud pour un cluster
Pour configurer un témoin de Cloud en tant que témoin de quorum pour votre cluster, procédez comme suit:
1. Créer un compte de stockage Azure à utiliser comme un témoin de Cloud
2. Configurer le témoin de Cloud en tant que témoin de quorum pour votre cluster.

## <a name="create-an-azure-storage-account-to-use-as-a-cloud-witness"></a>Créer un compte de stockage Azure à utiliser comme un témoin de Cloud

Cette section décrit comment créer un point de terminaison compte et de vue et de copie du stockage des URL et d’accéder aux clés de ce compte.

Pour configurer un témoin de Cloud, vous devez disposer d’un compte de stockage Azure valide, qui peut être utilisé pour stocker le fichier de l’objet blob (utilisé pour l’arbitrage). Un témoin de cloud crée un conteneur connu **témoin de cloud msft** sous le compte de stockage Microsoft. Écritures de témoin cloud un fichier objet blob unique avec les clusters de l’ID unique utilisé comme nom de fichier du fichier sous cet objet blob **témoin de cloud msft** conteneur. Cela signifie que vous pouvez utiliser le même compte de stockage Azure de Microsoft pour configurer un témoin de Cloud pour plusieurs clusters différents.

Lorsque vous utilisez le même compte de stockage Azure pour la configuration d’un témoin de Cloud pour plusieurs différents clusters, un seul **témoin de cloud msft** conteneur est créé automatiquement. Ce conteneur contiendra le fichier un blob par cluster.

### <a name="to-create-an-azure-storage-account"></a>Pour créer un compte de stockage Azure

1. Connectez-vous à la [portail Azure](http://portal.azure.com).
2. Dans le menu Hub, sélectionnez New -> données + stockage -> compte de stockage.
3. Dans la création d’une page de compte de stockage, procédez comme suit:
    1. Entrez un nom pour votre compte de stockage.
    <br>Les noms de compte de stockage doivent être compris entre 3 et 24caractères et peuvent contenir des chiffres et lettres minuscules uniquement. Le nom de compte de stockage doit également être unique dans Azure.
        
    2. Pour **compte kind**, sélectionnez **usage général**.
    <br>Vous ne pouvez pas utiliser un compte de stockage Blob pour un témoin de Cloud.
    3. Pour **performances**, sélectionnez **Standard**.
    <br>Vous ne pouvez pas utiliser le stockage Premium Azure pour un témoin de Cloud.
    2. Pour **réplication**, sélectionnez **stockage redondants localement (LRS)**.
    <br>Le Clustering de basculement utilise le fichier de l’objet blob en tant que point d’arbitrage, ce qui nécessite une garantie de cohérence lors de la lecture des données. Pour celui-ci, vous devez sélectionner **stockage redondants localement** pour **réplication** type.

### <a name="view-and-copy-storage-access-keys-for-your-azure-storage-account"></a>Afficher et copier des touches d’accès de stockage pour votre compte de stockage Azure

Lorsque vous créez un compte de stockage MicrosoftAzure, il est associé à deux touches d’accès rapide qui sont générés automatiquement - clé d’accès principal et secondaire accès. Pour une première la création d’un témoin de Cloud, utilisez le **principal touche d’accès rapide**. Il n’existe aucune restriction concernant la touche à utiliser pour un témoin de Cloud.  

#### <a name="to-view-and-copy-storage-access-keys"></a>Pour afficher et copier les touches d’accès de stockage

Dans le portail Azure, accédez à votre compte de stockage, cliquez sur **tous les paramètres** puis cliquez sur **touches d’accès rapide** pour afficher, copier et régénérer les touches d’accès de votre compte. La lame de touches d’accès rapide inclut également des chaînes de connexion préconfiguré à l’aide de vos clés primaires et secondaires que vous pouvez copier pour utiliser dans vos applications (voir figure4).

![Instantané de la boîte de dialogue Gérer les touches d’accès rapide dans MicrosoftAzure](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_4.png)  
**La figure4: Touches d’accès de stockage**

### <a name="view-and-copy-endpoint-url-links"></a>Afficher et copier des liens URL de point de terminaison  
Lorsque vous créez un compte de stockage, les URL suivantes sont générés en utilisant le format: `https://<Storage Account Name>.<Storage Type>.<Endpoint>`  

Utilise toujours le témoin de cloud **Blob** comme type de stockage. Azure utilise **. core.windows.net** en tant que le point de terminaison. Lorsque vous configurez un témoin de Cloud, il est possible que vous avez configuré un autre point de terminaison en fonction de votre scénario (par exemple le centre de données MicrosoftAzure en Chine a un autre point de terminaison).  

> [!NOTE]  
> L’URL de point de terminaison est généré automatiquement par la ressource de témoin de Cloud et qu’il n’existe aucune étape supplémentaire de configuration nécessaires pour l’URL.  

#### <a name="to-view-and-copy-endpoint-url-links"></a>Pour afficher et copier des liens URL de point de terminaison
Dans le portail Azure, accédez à votre compte de stockage, cliquez sur **tous les paramètres** puis cliquez sur **propriétés** afficher et copier votre URL de point de terminaison (voir figure5).  

![Instantané des liens de point de terminaison de témoin de Cloud](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_5.png)  
**Figure5: Des liens URL du point de terminaison de témoin de Cloud**

Pour plus d’informations sur la création et la gestion des comptes de stockage Azure, voir [sur les comptes de stockage Azure](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/)

## <a name="configure-cloud-witness-as-a-quorum-witness-for-your-cluster"></a>Configurer un témoin de Cloud en tant que témoin de quorum pour votre cluster.
Configuration d’un témoin cloud est bien intégrée au sein de l’Assistant Configuration de Quorum existantes intégrée dans le Gestionnaire du Cluster de basculement.  

### <a name="to-configure-cloud-witness-as-a-quorum-witness"></a>Pour configurer un témoin de Cloud en tant que témoin de Quorum
1. Lancez le Gestionnaire du Cluster de basculement.
2. Avec le bouton droit, le cluster -> **autres Actions** -> **configurer les paramètres de quorum du Cluster** (voir figure6). Cette opération lance l’Assistant Configuration de quorum du Cluster.  
    ![Capture instantanée du chemin de menu pour les paramètres du Quorum du Cluster dans le Gestionnaire du Cluster de basculement UI Configue](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_7.png)**Figure6. Paramètres du Quorum du cluster**

3. Sur le **sélectionnez des Configurations de Quorum**, sélectionnez **sélectionner le témoin de quorum** (voir figure7).  

    ![Case d’instantané de «sélectionner le témoin quotrum» dans l’Assistant quorum du Cluster](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_8.png)  
    **La figure7. Sélectionnez la Configuration de Quorum**

4. Sur le **sélectionner le témoin de Quorum**, sélectionnez **configurer un témoin de cloud** (voir figure8).  

    ![Capture instantanée de la case d’option appropriée pour sélectionner un témoin de cloud](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_9.png)  
    **La figure8. Sélectionner le témoin de Quorum**  

5. Sur le **configurer le témoin de Cloud** page, entrez les informations suivantes:  
    1. (Paramètre requis) Nom du compte de stockage Azure.  
    2. (Paramètre requis) Touche d’accès rapide correspondant au compte de stockage.  
        1. Lorsque vous créez pour la première fois, utiliser une touche d’accès principal (voir figure5)  
        2. Lors de la rotation de la touche d’accès principal, utiliser une touche d’accès secondaire (voir figure5)  
    3. (Paramètre facultatif) Si vous prévoyez d’utiliser un point de terminaison du service Azure différent (par exemple, le service MicrosoftAzure en Chine), puis mettez à jour le nom du serveur point de terminaison.  

    ![Capture instantanée du volet configuration Cloud Witness dans l’Assistant quorum du Cluster](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_10.png)  
    **Figure9: Configurer le témoin de Cloud**

6. Lors de la configuration réussie de témoin de Cloud, vous pouvez afficher la ressource témoin nouvellement créé dans le Gestionnaire du Cluster de basculement logiciel enfichable (voir figure10).

    ![Configuration réussie de témoin de Cloud](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_11.png)  
    **Figure10: Réussir la configuration du témoin de Cloud**

### <a name="configuring-cloud-witness-using-powershell"></a>La configuration d’un témoin de Cloud à l’aide de PowerShell  
La commande PowerShell Set-ClusterQuorum existante a des nouveaux paramètres supplémentaires correspondant à un témoin de Cloud.  

Vous pouvez configurer un témoin de Cloud à l’aide de la [`Set-ClusterQuorum`](https://technet.microsoft.com/library/ee461013.aspx)commande PowerShell suivante:  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey>
```

En cas de besoin d’utiliser un autre point de terminaison (rare):  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> -Endpoint <servername>  
```

### <a name="azure-storage-account-considerations-with-cloud-witness"></a>Considérations en matière de compte de stockage Azure avec témoin de Cloud  
Lorsque vous configurez un témoin de Cloud en tant que témoin de quorum pour votre Cluster de basculement, procédez comme suit:
* Au lieu de stocker la clé d’accès, votre Cluster de basculement génère et le stocker en toute sécurité un jeton de sécurité de l’accès partagé (SAS).  
* Le jeton généré SAS est valide tant que la touche d’accès reste valide. Lors de la rotation de la touche d’accès principal, il est important pour tout d’abord mettre à jour le témoin de Cloud (sur tous vos clusters qui utilisent ce compte de stockage) avec la touche d’accès secondaire avant la régénération de la touche d’accès principal.  
* Un témoin de cloud utilise l’interface HTTPS reste du service de compte de stockage Azure. Cela signifie qu’elle requiert le port HTTPS être ouverts sur tous les nœuds de cluster.

### <a name="proxy-considerations-with-cloud-witness"></a>Considérations en matière de proxy avec témoin de Cloud  
Un témoin de cloud utilise le protocole HTTPS (port par défaut 443) pour établir la communication avec le service d’objets blob Azure. Assurez-vous que le port HTTPS est accessible via le réseau Proxy.

## <a name="see-also"></a>Voir aussi
- [Nouveautés du Clustering de basculement dans Windows Server](whats-new-in-failover-clustering.md)
