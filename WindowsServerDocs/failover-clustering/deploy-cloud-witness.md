---
ms.assetid: 0cd1ac70-532c-416d-9de6-6f920a300a45
title: Déployer un témoin de Cloud pour un cluster de basculement
ms.prod: windows-server
manager: eldenc
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.topic: article
author: JasonGerend
ms.date: 01/18/2019
description: Comment utiliser Microsoft Azure pour héberger le témoin d’un cluster de basculement Windows Server dans le Cloud-alias comment déployer un témoin Cloud.
ms.openlocfilehash: 1f38a1a436cfced8637b743817dc1b3d150f7fa6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369882"
---
# <a name="deploy-a-cloud-witness-for-a-failover-cluster"></a>Déployer un témoin de cloud pour un cluster de basculement

> S’applique à : Windows Server 2019, Windows Server 2016

Cloud Witness est un type de témoin de quorum de cluster de basculement qui utilise Microsoft Azure pour fournir un vote sur le quorum de cluster. Cette rubrique fournit une vue d’ensemble de la fonctionnalité de témoin de Cloud, les scénarios qu’elle prend en charge et des instructions sur la configuration d’un témoin de Cloud pour un cluster de basculement.

## <a name="CloudWitnessOverview"></a>Présentation du témoin Cloud

La figure 1 illustre une configuration de quorum de cluster de basculement étiré sur plusieurs sites avec Windows Server 2016. Dans cet exemple de configuration (figure 1), il existe 2 nœuds dans 2 centres de centres (appelés sites). Notez qu’il est possible qu’un cluster s’étende sur plus de 2 centres de informations. En outre, chaque centre de informations peut avoir plus de 2 nœuds. Une configuration de quorum de cluster classique dans cette installation (contrat SLA de basculement automatique) donne à chaque nœud un vote. Un vote supplémentaire est donné au témoin de quorum pour permettre au cluster de continuer à s’exécuter même si l’un des centres de données rencontre une panne d’alimentation. La mathématique est simple : il y a 5 votes au total et vous avez besoin de 3 votes pour que le cluster reste en cours d’exécution.  

![Témoin de partage de fichiers dans un troisième site distinct avec 2 nœuds dans 2 autres sites témoin de partage de](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_1.png "fichiers")  
**Figure 1 : Utilisation d’un témoin de partage de fichiers en tant que témoin de quorum @ no__t-0  

En cas de panne de courant dans un centre de donnes, afin d’offrir la même possibilité au cluster dans un autre centre de donnes de le maintenir en cours d’exécution, il est recommandé d’héberger le témoin de quorum dans un emplacement autre que les deux centres de donnes. Cela signifie généralement que vous avez besoin d’un troisième centre de centres (site) distinct pour héberger un serveur de fichiers qui sauvegarde le partage de fichiers utilisé comme témoin de quorum (témoin de partage de fichiers).  

La plupart des organisations ne disposent pas d’un troisième centre de fichiers distinct qui hébergera le serveur de fichiers témoin de partage de fichiers. Cela signifie que les organisations hébergent principalement le serveur de fichiers dans l’un des deux centres de donnes, qui, par extension, fait de ce centre de fichiers le centre de donnes principal. Dans un scénario où une panne d’alimentation se produisait dans le centre de donnes principal, le cluster se déplacerait, car l’autre centre de donnes n’aurait que 2 votes au-dessous de la majorité des 3 votes nécessaires. Pour les clients qui disposent d’un troisième centre de ressources distinct pour héberger le serveur de fichiers, il s’agit d’une surcharge pour gérer le serveur de fichiers à haut niveau de disponibilité qui sauvegarde le témoin de partage de fichiers. L’hébergement de machines virtuelles dans le cloud public avec le serveur de fichiers pour le témoin de partage de fichiers qui s’exécute dans le système d’exploitation invité représente une surcharge importante en termes de configuration & de maintenance.  

Cloud Witness est un nouveau type de témoin de quorum de cluster de basculement qui tire parti de Microsoft Azure comme point d’arbitrage (figure 2). Il utilise le stockage d’objets BLOB Azure pour lire/écrire un fichier BLOB qui est ensuite utilisé comme point d’arbitrage dans le cas d’une résolution split-brain.  

Cette approche présente des avantages significatifs :
1. Tire parti de Microsoft Azure (inutile de disposer d’un troisième centre de centres).  
2. Utilise le stockage d’objets BLOB Azure disponible standard (aucune surcharge de maintenance supplémentaire des machines virtuelles hébergées dans le cloud public).  
3. Le même compte de stockage Azure peut être utilisé pour plusieurs clusters (un fichier d’objets BLOB par cluster ; ID unique du cluster utilisé comme nom de fichier BLOB).  
4. $Cost très faible sur le compte de stockage (données très petites écrites par fichier BLOB, fichier BLOB mis à jour une seule fois lors de la modification de l’état des nœuds du cluster).  
5. Type de ressource témoin Cloud intégré.  

![Diagram illustrant un cluster étendu à plusieurs sites avec un témoin Cloud comme témoin de quorum @ no__t-1  
**Figure 2: Clusters étirés sur plusieurs sites avec un témoin Cloud comme témoin de quorum @ no__t-0  

Comme illustré à la figure 2, aucun autre site distinct n’est requis. Le témoin Cloud, comme tout autre témoin de quorum, obtient un vote et peut participer aux calculs de quorum.  

## <a name="CloudWitnessSupportedScenarios"></a>Témoin Cloud : Scénarios pris en charge pour un type de témoin unique
Si vous avez un déploiement de cluster de basculement, où tous les nœuds peuvent accéder à Internet (par extension d’Azure), il est recommandé de configurer un témoin de cloud comme ressource de témoin de quorum.  

Voici quelques-uns des scénarios pris en charge pour l’utilisation d’un témoin de Cloud en tant que témoin de quorum :  
- Clusters multisites étirés par la récupération d’urgence (voir figure 2).  
- Clusters de basculement sans stockage partagé (SQL Always On etc.).  
- Clusters de basculement s’exécutant dans le système d’exploitation invité hébergé dans Microsoft Azure rôle de machine virtuelle (ou tout autre cloud public).  
- Clusters de basculement s’exécutant dans le système d’exploitation invité des machines virtuelles hébergées dans des clouds privés.
- Les clusters de stockage avec ou sans stockage partagé, tels que les clusters de serveurs de fichiers avec montée en puissance parallèle.  
- Petits clusters de succursales (y compris des clusters à 2 nœuds)  

À compter de Windows Server 2012 R2, il est recommandé de toujours configurer un témoin, car le cluster gère automatiquement le vote témoin et les nœuds votent avec le quorum dynamique.  

## <a name="CloudWitnessSetUp"></a>Configurer un témoin de Cloud pour un cluster
Pour configurer un témoin de Cloud en tant que témoin de quorum pour votre cluster, procédez comme suit :
1. Créer un compte de stockage Azure à utiliser en tant que témoin Cloud
2. Configurez le témoin Cloud en tant que témoin de quorum pour votre cluster.

## <a name="create-an-azure-storage-account-to-use-as-a-cloud-witness"></a>Créer un compte de stockage Azure à utiliser en tant que témoin Cloud

Cette section décrit comment créer un compte de stockage et afficher et copier des URL de point de terminaison et des clés d’accès pour ce compte.

Pour configurer un témoin de Cloud, vous devez disposer d’un compte de stockage Azure valide qui peut être utilisé pour stocker le fichier BLOB (utilisé pour l’arbitrage). Cloud Witness crée un conteneur connu **msft-Cloud-Witness** dans le compte de stockage Microsoft. Le témoin Cloud écrit un seul fichier BLOB avec l’ID unique du cluster correspondant utilisé comme nom de fichier du fichier BLOB sous ce conteneur **msft-Cloud-Witness** . Cela signifie que vous pouvez utiliser le même compte de Stockage Microsoft Azure pour configurer un témoin de Cloud pour plusieurs clusters différents.

Lorsque vous utilisez le même compte de stockage Azure pour configurer le témoin de Cloud pour plusieurs clusters différents, un seul conteneur **msft-Cloud-Witness** est créé automatiquement. Ce conteneur contient un fichier d’objets BLOB par cluster.

### <a name="to-create-an-azure-storage-account"></a>Pour créer un compte de stockage Azure

1. Connectez-vous au [portail Azure](http://portal.azure.com).
2. Dans le menu Hub, sélectionnez Nouveau-> données + stockage-> compte de stockage.
3. Dans la page créer un compte de stockage, procédez comme suit :
    1. Entrez un nom pour votre compte de stockage.
    <br>Les noms de compte de stockage doivent comporter entre 3 et 24 caractères et ne peuvent contenir que des chiffres et des lettres minuscules. Le nom du compte de stockage doit également être unique dans Azure.
        
    2. Pour **type de compte**, sélectionnez **usage général**.
    <br>Vous ne pouvez pas utiliser un compte de stockage d’objets BLOB pour un témoin Cloud.
    3. Pour **performances**, sélectionnez **standard**.
    <br>Vous ne pouvez pas utiliser le stockage Premium Azure pour un témoin Cloud.
    2. Pour **la réplication**, sélectionnez **stockage localement redondant (LRS)** .
    <br>Le clustering de basculement utilise le fichier BLOB comme point d’arbitrage, ce qui nécessite des garanties de cohérence lors de la lecture des données. Par conséquent, vous devez sélectionner **le stockage localement redondant** pour le type de **réplication** .

### <a name="view-and-copy-storage-access-keys-for-your-azure-storage-account"></a>Afficher et copier les clés d’accès de stockage pour votre compte de stockage Azure

Lorsque vous créez un compte Stockage Microsoft Azure, il est associé à deux clés d’accès qui sont générées automatiquement-clé d’accès primaire et clé d’accès secondaire. Pour une première création de témoin de Cloud, utilisez la **clé d’accès primaire**. Il n’existe aucune restriction quant à la clé à utiliser pour le témoin Cloud.  

#### <a name="to-view-and-copy-storage-access-keys"></a>Pour afficher et copier les clés d’accès de stockage

Dans le portail Azure, accédez à votre compte de stockage, cliquez sur **tous les paramètres** , puis cliquez sur **clés d’accès** pour afficher, copier et régénérer les clés d’accès de votre compte. Le panneau clés d’accès comprend également des chaînes de connexion préconfigurées à l’aide de vos clés primaires et secondaires que vous pouvez copier pour les utiliser dans vos applications (voir figure 4).

![Snapshot de la boîte de dialogue gérer les clés d’accès dans Microsoft Azure @ no__t-1  
**Figure 4 : Clés d’accès de stockage @ no__t-0

### <a name="view-and-copy-endpoint-url-links"></a>Afficher et copier des liens URL de point de terminaison  
Lorsque vous créez un compte de stockage, les URL suivantes sont générées à l’aide du format : `https://<Storage Account Name>.<Storage Type>.<Endpoint>`  

Le témoin Cloud utilise toujours l' **objet BLOB** comme type de stockage. Azure utilise **. Core.Windows.net** comme point de terminaison. Lors de la configuration d’un témoin de Cloud, il est possible de le configurer avec un autre point de terminaison selon votre scénario (par exemple, le Microsoft Azure datacenter en Chine a un point de terminaison différent).  

> [!NOTE]  
> L’URL du point de terminaison est générée automatiquement par la ressource de témoin Cloud et aucune étape supplémentaire de configuration n’est nécessaire pour l’URL.  

#### <a name="to-view-and-copy-endpoint-url-links"></a>Pour afficher et copier des liens URL de point de terminaison
Dans le portail Azure, accédez à votre compte de stockage, cliquez sur **tous les paramètres** , puis cliquez sur **Propriétés** pour afficher et copier vos URL de point de terminaison (voir figure 5).  

![Snapshot des liens de point de terminaison du témoin de Cloud @ no__t-1  
@no__t 0Figure 5 : Liens URL du point de terminaison du témoin de Cloud @ no__t-0

Pour plus d’informations sur la création et la gestion des comptes de stockage Azure, consultez [à propos des comptes de stockage Azure](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/)

## <a name="configure-cloud-witness-as-a-quorum-witness-for-your-cluster"></a>Configurer un témoin de cloud comme témoin de quorum pour votre cluster
La configuration du témoin Cloud est bien intégrée dans l’Assistant Configuration de quorum existant intégré à la Gestionnaire du cluster de basculement.  

### <a name="to-configure-cloud-witness-as-a-quorum-witness"></a>Pour configurer le témoin de Cloud en tant que témoin de quorum
1. Lancez Gestionnaire du cluster de basculement.
2. Cliquez avec le bouton droit sur le cluster-> **plus d’Actions** ->  configurer les paramètres de**quorum du cluster** (voir figure 6). Cela lance l’Assistant Configuration du quorum du cluster.  
    ![Snapshot du chemin d’accès au menu pour configurer les paramètres de quorum du cluster dans l’interface utilisateur Gestionnaire du cluster de basculement @ no__t-1 **Figure 6. Paramètres de quorum du cluster @ no__t-0

3. Dans la page **Sélectionner les configurations de quorum** , sélectionnez **Sélectionner le témoin de quorum** (voir la figure 7).  

    ![Snapshot de la case d’option « Sélectionner le témoin quotrum » dans l’Assistant quorum de cluster @ no__t-1  
    @no__t 0Figure 7. Sélectionnez la configuration de quorum @ no__t-0

4. Dans la page **Sélectionner le témoin de quorum** , sélectionnez **configurer un témoin de Cloud** (voir figure 8).  

    ![Snapshot de la case d’option appropriée pour sélectionner un témoin de Cloud @ no__t-1  
    **Figure 8. Sélectionner le témoin de quorum @ no__t-0  

5. Dans la page **configurer le témoin Cloud** , entrez les informations suivantes :  
   1. (Paramètre obligatoire) Nom du compte de stockage Azure.  
   2. (Paramètre obligatoire) Clé d’accès correspondant au compte de stockage.  
       1. Lorsque vous créez pour la première fois, utilisez la clé d’accès primaire (voir figure 5)  
       2. Lors de la rotation de la clé d’accès primaire, utilisez la clé d’accès secondaire (voir figure 5)  
   3. (Paramètre facultatif) Si vous envisagez d’utiliser un autre point de terminaison de service Azure (par exemple, le service Microsoft Azure en Chine), mettez à jour le nom du serveur de point de terminaison.  

      ![Snapshot du volet de configuration du témoin de Cloud dans l’Assistant quorum de cluster @ no__t-1  
      **Figure 9 : Configurer votre témoin de Cloud @ no__t-0

6. Une fois la configuration du témoin Cloud terminée, vous pouvez afficher la ressource témoin nouvellement créée dans le composant logiciel enfichable Gestionnaire du cluster de basculement (voir figure 10).

    configuration de @no__t 0Successful du témoin de Cloud @ no__t-1  
    @no__t 0Figure 10 : Configuration réussie du témoin de Cloud @ no__t-0

### <a name="configuring-cloud-witness-using-powershell"></a>Configuration d’un témoin de Cloud à l’aide de PowerShell  
La commande PowerShell Set-ClusterQuorum existante possède de nouveaux paramètres supplémentaires correspondant au témoin Cloud.  

Vous pouvez configurer le témoin de Cloud à l’aide de la commande PowerShell [`Set-ClusterQuorum`](https://technet.microsoft.com/library/ee461013.aspx) suivante :  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey>
```

Si vous devez utiliser un point de terminaison différent (rare) :  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> -Endpoint <servername>  
```

### <a name="azure-storage-account-considerations-with-cloud-witness"></a>Considérations relatives au compte de stockage Azure avec un témoin Cloud  
Lors de la configuration d’un témoin de Cloud en tant que témoin de quorum pour votre cluster de basculement, tenez compte des éléments suivants :
* Au lieu de stocker la clé d’accès, votre cluster de basculement génère et stocke en toute sécurité un jeton de sécurité d’accès partagé (SAS).  
* Le jeton SAS généré est valide tant que la clé d’accès reste valide. Lors de la rotation de la clé d’accès primaire, il est important de commencer par mettre à jour le témoin Cloud (sur tous vos clusters qui utilisent ce compte de stockage) avec la clé d’accès secondaire avant de régénérer la clé d’accès primaire.  
* Cloud Witness utilise l’interface REST HTTPs du service de compte de stockage Azure. Cela signifie que le port HTTPs doit être ouvert sur tous les nœuds du cluster.

### <a name="proxy-considerations-with-cloud-witness"></a>Considérations relatives au proxy avec un témoin Cloud  
Le témoin Cloud utilise le protocole HTTPs (port par défaut 443) pour établir la communication avec le service BLOB Azure. Assurez-vous que le port HTTPs est accessible via le proxy réseau.

## <a name="see-also"></a>Voir aussi
- [Nouveautés du clustering de basculement dans Windows Server](whats-new-in-failover-clustering.md)
