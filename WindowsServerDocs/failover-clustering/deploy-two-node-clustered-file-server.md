---
title: Déploiement d’un serveur de fichiers en cluster à deux nœuds
ms.prod: windows-server
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 02/01/2019
description: Cet article décrit la création d’un cluster de serveurs de fichiers à deux nœuds.
ms.openlocfilehash: 03e78495b3fc85449d3d383706fb82541dd10372
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361304"
---
# <a name="deploying-a-two-node-clustered-file-server"></a>Déploiement d’un serveur de fichiers en cluster à deux nœuds

> S’applique à : Windows Server 2019, Windows Server 2016

Un cluster de basculement est un groupe d'ordinateurs indépendants qui travaillent conjointement pour accroître la disponibilité des applications et des services. Les serveurs en cluster (appelés « nœuds ») sont connectés par des câbles physiques et par des logiciels. En cas de défaillance de l'un des nœuds, un autre prend sa place pour fournir le service (processus appelé « basculement »), garantissant ainsi des interruptions minimales de service pour les utilisateurs.

Ce guide décrit les étapes d’installation et de configuration d’un cluster de basculement de serveur de fichiers à usage général qui a deux nœuds. En créant la configuration dans ce guide, vous pouvez en savoir plus sur les clusters de basculement et vous familiariser avec l’interface du composant logiciel enfichable Gestion du cluster de basculement dans Windows Server 2019 ou Windows Server 2016.

## <a name="overview-for-a-two-node-file-server-cluster"></a>Vue d'ensemble pour un cluster de serveur de fichiers à deux nœuds

Les serveurs dans un cluster de basculement peuvent fonctionner dans divers rôles, y compris les rôles de serveur de fichiers, de serveur Hyper-V ou de serveur de base de données, et peuvent fournir une haute disponibilité pour un large éventail d’autres services et applications. Ce guide décrit comment configurer un cluster de serveur de fichiers à deux nœuds.

En règle générale, un cluster de basculement inclut une unité de stockage connectée physiquement à tous les serveurs du cluster, même si tout volume donné dans le stockage n'est accessible qu'à un serveur à la fois. Le diagramme suivant montre un cluster de basculement à deux nœuds connecté à une unité de stockage.

![Cluster à deux nœuds](media/Cluster-File-Server/Cluster-FS-Overview.png)

Les volumes de stockage ou numéros d'unités logiques exposés aux nœuds d'un cluster ne doivent pas être exposés à d'autres serveurs, y compris les serveurs d'un autre cluster. Le diagramme suivant illustre ce point.

![Numéros d’unités logiques dans le stockage](media/Cluster-File-Server/Cluster-FS-LUNs.png)

Notez que pour une disponibilité maximale des serveurs, il est important de suivre les meilleures pratiques en matière de gestion de serveurs, par exemple à travers la gestion soigneuse de l'environnement physique des serveurs, le test des modifications des logiciels avant leur implémentation complète, ainsi que le suivi attentif des mises à jour logicielles et des modifications de configuration sur tous les serveurs en cluster.

Le scénario suivant décrit comment un cluster de basculement de serveur de fichiers peut être configuré. Les fichiers qui sont partagés sont sur l'espace de stockage en cluster et l'un des serveurs en cluster peut jouer le rôle de serveur de fichiers qui les partage.

## <a name="shared-folders-in-a-failover-cluster"></a>Dossiers partagés dans un cluster de basculement

La liste suivante décrit les fonctionnalités de configuration de dossiers partagés intégrées au clustering de basculement :

- L’affichage est limité aux dossiers partagés en cluster uniquement (pas de combinaison avec des dossiers partagés non cluster) : Quand un utilisateur affiche des dossiers partagés en spécifiant le chemin d’accès à un serveur de fichiers en cluster, l’affichage inclut uniquement les dossiers partagés qui font partie du rôle de serveur de fichiers spécifique. Il exclut les dossiers partagés non cluster et partage une partie des rôles de serveur de fichiers distincts qui se trouvent sur un nœud du cluster.

- Énumération basée sur l’accès : Vous pouvez utiliser l'énumération basée sur l'accès pour masquer un dossier spécifique dans l'affichage de l'utilisateur. Au lieu de permettre aux utilisateurs de voir un dossier sans qu'ils puissent toutefois y accéder, vous pouvez choisir de les empêcher de voir le dossier. Vous pouvez configurer l’énumération basée sur l’accès pour un dossier partagé en cluster de la même façon que pour un dossier partagé non cluster.

- Accès hors connexion : Vous pouvez configurer l'accès hors connexion (mise en cache) pour un dossier partagé en cluster comme pour un dossier partagé qui n'est pas en cluster.

- Les disques en cluster sont toujours reconnus comme faisant partie du cluster : Que vous utilisiez l’interface de cluster de basculement, l’Explorateur Windows ou le composant logiciel enfichable Gestion du partage et du stockage, Windows reconnaît si un disque a été désigné comme étant dans le stockage en cluster. Si un disque de ce type a déjà été configuré dans la gestion du cluster de basculement dans le cadre d’un serveur de fichiers en cluster, vous pouvez utiliser l’une des interfaces mentionnées précédemment pour créer un partage sur le disque. Si ce disque n'a pas été configuré comme faisant partie d'un serveur de fichiers en cluster, vous ne pouvez pas créer de partage sur ce disque par inadvertance. En effet, le système afficherait alors un message d'erreur indiquant que, pour être partagé, le disque doit d'abord être configuré comme faisant partie d'un serveur de fichiers en cluster.

- Intégration des services pour le système de fichiers réseau : Le rôle de serveur de fichiers dans Windows Server comprend le service de rôle facultatif appelé services pour NFS (Network File System). En installant le service de rôle et en configurant les dossiers partagés avec Services pour NFS, vous pouvez créer un serveur de fichiers en cluster qui prend en charge les clients basés sur UNIX.

## <a name="requirements-for-a-two-node-failover-cluster"></a>Configuration requise pour un cluster de basculement à deux nœuds

Pour qu’un cluster de basculement dans Windows Server 2016 ou Windows Server 2019 soit considéré comme une solution officiellement prise en charge par Microsoft, la solution doit répondre aux critères suivants.

- Tous les composants matériels et logiciels doivent se conformer aux qualifications du logo approprié. Pour Windows Server 2016, il s’agit du logo « certifié pour Windows Server 2016 ». Pour Windows Server 2019, il s’agit du logo « certifié pour Windows Server 2019 ». Pour plus d’informations sur les systèmes matériels et logiciels certifiés, consultez le site du [catalogue Microsoft Windows Server](https://www.windowsservercatalog.com/default.aspx) .

- La solution entièrement configurée (serveurs, réseau et stockage) doit réussir tous les tests de l’Assistant validation, qui fait partie du composant logiciel enfichable cluster de basculement.

Les éléments suivants seront nécessaires pour un cluster de basculement à deux nœuds.

- **Serveurs** Nous vous recommandons d’utiliser des ordinateurs correspondants avec des composants identiques ou similaires.  Les serveurs d’un cluster de basculement à deux nœuds doivent exécuter la même version de Windows Server. Ils doivent également avoir les mêmes mises à jour logicielles (correctifs).

- **Cartes réseau et câble :** Le matériel réseau, comme les autres composants de la solution de cluster de basculement, doit être compatible avec Windows Server 2016 ou Windows Server 2019. Si vous utilisez iSCSI, les cartes réseau doivent être dédiées à la communication réseau ou à iSCSI, et non aux deux. Dans l’infrastructure réseau qui connecte vos nœuds de cluster, évitez d’avoir des points de défaillance uniques. Il existe plusieurs façons d'y parvenir. Vous pouvez connecter vos nœuds de cluster à l'aide de plusieurs réseaux distincts. Vous pouvez également connecter vos nœuds de cluster à l’aide d’un réseau construit avec des cartes réseau associées, des commutateurs redondants, des routeurs redondants ou tout autre matériel similaire permettant de supprimer les points de défaillance uniques.

   > [!NOTE]
   > Si les nœuds de cluster sont connectés avec un seul réseau, le réseau passe l’exigence de redondance dans l’Assistant validation d’une configuration.  Toutefois, le rapport inclut un avertissement indiquant que le réseau ne doit pas avoir un point de défaillance unique.

- **Contrôleurs d’appareil ou adaptateurs appropriés pour le stockage :**
    - **Serial Attached SCSI ou Fibre Channel :** si vous utilisez SAS (Serial Attached SCSI) ou Fibre Channel, dans l'ensemble des serveurs en cluster, tous les composants de la pile de stockage doivent être identiques. Il est nécessaire que les composants logiciels MPIO (Multipath I/O) et DSM (Device specific module) soient identiques.  Il est recommandé que les contrôleurs de périphériques de stockage de masse, c’est-à-dire l’adaptateur de bus hôte (HBA), les pilotes HBA et le microprogramme HBA, qui sont attachés à l’espace de stockage en cluster soient identiques. Si vous utilisez des adaptateurs de bus hôte non similaires, vous devez vérifier auprès du fournisseur de stockage que vous respectez les configurations prises en charge ou recommandées.
    - **Protocole** Si vous utilisez iSCSI, chaque serveur en cluster doit avoir une ou plusieurs cartes réseau ou adaptateurs de bus hôte dédiés au stockage ISCSI. Le réseau que vous utilisez pour iSCSI ne peut pas être utilisé pour la communication réseau. Dans tous les serveurs en cluster, les cartes réseau que vous utilisez pour la connexion à la cible de stockage iSCSI doivent être identiques ; en outre, il est recommandé d’utiliser des cartes Gigabit Ethernet ou une connexion plus rapide.  

- **Rangement** Vous devez utiliser un stockage partagé certifié pour Windows Server 2016 ou Windows Server 2019.
  
    Pour un cluster de basculement à deux nœuds, le stockage doit contenir au moins deux volumes (numéros d’unités logiques) distincts si vous utilisez un disque témoin pour le quorum. Le disque témoin est un disque de l'espace de stockage en cluster qui est conçu pour conserver une copie de la base de données de configuration du cluster. Pour cet exemple de cluster à deux nœuds, la configuration de quorum sera node et Disk majoritaire. Nœud et disque majoritaires signifie que les nœuds et le disque témoin contiennent chacun des copies de la configuration du cluster, et que le cluster a un quorum tant qu’une majorité (deux sur trois) de ces copies sont disponibles. L’autre volume (LUN) contient les fichiers qui sont partagés avec les utilisateurs.

    Les conditions requises pour le stockage incluent les éléments suivants :

    - Pour utiliser la prise en charge de disque native incluse dans le clustering de basculement, utilisez des disques de base au lieu de disques dynamiques.
    - Il est recommandé de formater les partitions avec NTFS (pour le disque témoin, la partition doit être NTFS).
    - Pour le style de partition du disque, vous pouvez choisir entre une partition MBR (Master Boot Record) ou une partition GPT (table de partition GUID).
    - Le stockage doit répondre correctement à des commandes SCSI spécifiques, le stockage doit suivre la norme nommée SCSI Primary Commands-3 (SPC-3). En particulier, le stockage doit prendre en charge les réservations persistantes telles que spécifiées dans le standard SPC-3. 
    -  Le pilote de miniport utilisé pour le stockage doit fonctionner avec le pilote de stockage Microsoft Storport.

## <a name="deploying-storage-area-networks-with-failover-clusters"></a>Déploiement de réseaux SAN avec des clusters de basculement

Lors du déploiement d’un réseau de zone de stockage (SAN) avec un cluster de basculement, les instructions suivantes doivent être respectées.

- **Confirmez la certification du stockage :** À l’aide du site du [Catalogue Windows Server](https://www.windowsservercatalog.com/default.aspx) , vérifiez que le stockage du fournisseur, y compris les pilotes, les microprogrammes et les logiciels, est certifié pour windows server 2016 ou windows server 2019.

- **Isoler les périphériques de stockage, un cluster par appareil :** les serveurs de clusters distincts ne doivent pas être en mesure d'accéder aux mêmes dispositifs de stockage. Dans la plupart des cas, un numéro d'unité logique utilisé pour un jeu de serveurs de cluster doit être isolé de tous les autres serveurs via le masquage ou la répartition en zones du numéro d'unité logique.

- **Envisagez d’utiliser le logiciel MPIO (Multipath I/O) :** dans un ensemble fibre optique de stockage à haut niveau de disponibilité, vous pouvez déployer des clusters de basculement avec plusieurs adaptateurs de bus hôte à l'aide de logiciels MPIO (Multipath I/O). Cela fournit le niveau le plus élevé de redondance et de disponibilité. La solution multivoie doit être basée sur Microsoft MPIO (Multipath I/O). Le fabricant de matériel de stockage peut fournir un module spécifique au périphérique (DSM) MPIO pour votre matériel, bien que Windows Server 2016 et Windows Server 2019 incluent un ou plusieurs modules DSM dans le cadre du système d’exploitation.

## <a name="network-infrastructure-and-domain-account-requirements"></a>Configuration requise de l'infrastructure et du compte de domaine

Pour un cluster de basculement à deux nœuds, vous avez besoin de l'infrastructure réseau suivante et d'un compte d'administration avec les autorisations de domaine suivantes :

- **Paramètres réseau et adresses IP :** lorsque vous utilisez des cartes réseau identiques pour un réseau, utilisez également des paramètres de communication identiques sur ces cartes (par exemple pour la vitesse, le mode duplex, le contrôle de flux et le type de média). En outre, comparez les paramètres de la carte réseau et du commutateur auquel elle est connectée, et vérifiez qu'aucun paramètre n'est en conflit.

    Si vous avez des réseaux privés qui ne sont pas routés vers le reste de votre infrastructure réseau, vérifiez que chacun de ces réseaux privés utilise un sous-réseau unique. Cela est nécessaire même si vous attribuez une adresse IP unique à chaque carte réseau. Par exemple, si vous avez un nœud de cluster dans un bureau central qui utilise un réseau physique, et un autre nœud dans une filiale qui utilise un autre réseau physique, ne spécifiez pas 10.0.0.0/24 pour les deux réseaux, même si vous attribuez une adresse IP unique à chaque carte adaptateur.

    Pour plus d’informations sur les cartes réseau, consultez Configuration matérielle requise pour un cluster de basculement à deux nœuds, plus haut dans ce guide.

- **DN** les serveurs du cluster doivent utiliser le système DNS (Domain Name System) pour la résolution de noms. Le protocole de mise à jour dynamique DNS peut être utilisé.

- **Rôle de domaine :** tous les serveurs du cluster doivent se trouver dans le même domaine Active Directory. Il est recommandé d'attribuer le même rôle de domaine à tous les serveurs en cluster (serveur membre ou contrôleur de domaine). Le rôle recommandé est celui de serveur membre.

- **Contrôleur de domaine :** il est recommandé de faire de vos serveurs en cluster des serveurs membres. Si tel est le cas, vous avez besoin d'un autre serveur qui joue le rôle de contrôleur de domaine dans le domaine qui contient votre cluster de basculement.

- **Les** Pour les besoins du test, vous pouvez connecter un ou plusieurs clients en réseau au cluster de basculement que vous créez et observer l'effet sur un client lorsque vous déplacez ou basculez le serveur de fichiers en cluster d'un nœud de cluster à l'autre.

- **Compte d’administration du cluster :** lorsque vous créez un cluster ou que vous y ajoutez des serveurs, vous devez avoir ouvert une session sur le domaine à l'aide d'un compte qui dispose de droits d'administrateur et d'autorisations pour tous les serveurs de ce cluster. Le compte n’a pas besoin d’être un compte Admins du domaine, mais peut être un compte Utilisateurs du domaine situé dans le groupe Administrateurs sur chaque serveur en cluster. En outre, si le compte n’est pas un compte admins du domaine, le compte (ou le groupe dont le compte est membre) doit disposer des autorisations **créer des objets ordinateur** et **Lire toutes les propriétés** dans l’unité d’organisation de domaine (UO) qui est résident dans.

## <a name="steps-for-installing-a-two-node-file-server-cluster"></a>Étapes pour l'installation d'un cluster de serveur de fichiers à deux nœuds

Vous devez effectuer les étapes suivantes pour installer un cluster de basculement de serveur de fichiers à deux nœuds.

Étape 1 : connecter les serveurs de cluster aux réseaux et au stockage

Étape 2 : installer la fonctionnalité de cluster de basculement

Étape 3 : valider la configuration du cluster

Étape 4 : créer le cluster

Si vous avez déjà installé les nœuds de cluster et souhaitez configurer un cluster de basculement de serveur de fichiers, consultez étapes de configuration d’un cluster de serveurs de fichiers à deux nœuds, plus loin dans ce guide.

### <a name="step-1-connect-the-cluster-servers-to-the-networks-and-storage"></a>Étape 1 : connecter les serveurs de cluster aux réseaux et au stockage

Pour un réseau de cluster de basculement, évitez d'avoir des points de défaillance uniques. Il existe plusieurs façons d'y parvenir. Vous pouvez connecter vos nœuds de cluster à l'aide de plusieurs réseaux distincts. Vous pouvez également connecter vos nœuds de cluster à l'aide d'un réseau construit avec des cartes réseau associées, des commutateurs redondants, des routeurs redondants ou tout autre matériel similaire permettant de supprimer les points de défaillance uniques (si vous utilisez un réseau pour iSCSI, vous devez le créer en plus des autres réseaux).

Pour un cluster de serveur de fichiers à deux nœuds, lorsque vous connectez les serveurs à l'espace de stockage en cluster, vous devez exposer au moins deux volumes (numéros d'unités logiques). Vous pouvez exposer des volumes supplémentaires autant que nécessaire pour un test complet de votre configuration. N'exposez pas les volumes en cluster à des serveurs qui ne sont pas dans le cluster.

#### <a name="to-connect-the-cluster-servers-to-the-networks-and-storage"></a>Pour connecter les serveurs de cluster aux réseaux et au stockage

1. Passez en revue les détails sur les réseaux dans Configuration matérielle requise pour un cluster de basculement à deux nœuds et les exigences en matière d’infrastructure réseau et de compte de domaine pour un cluster de basculement à deux nœuds, plus haut dans ce guide.

2. Connectez et configurez les réseaux qui seront utilisés par les serveurs du cluster.

3. Si votre configuration de test inclut des clients ou un contrôleur de domaine non-membres d'un cluster, vérifiez que ces ordinateurs peuvent se connecter aux serveurs en cluster via au moins un réseau.

4. Suivez les instructions du fabricant pour connecter physiquement les serveurs au stockage.

5. Vérifiez que les disques (numéros d'unités logiques) que vous souhaitez utiliser dans le cluster sont exposés aux serveurs que vous mettrez en cluster (et uniquement ces serveurs). Vous pouvez utiliser les interfaces suivantes pour exposer des disques ou des numéros d'unités logiques :

    - Interface fournie par le fabricant du stockage.

    - Interface iSCSI appropriée, si vous utilisez iSCSI.

6. Si vous avez acheté un logiciel qui contrôle le format ou le fonctionnement du disque, suivez les instructions du fournisseur sur l’utilisation de ce logiciel avec Windows Server.

7. Sur l’un des serveurs que vous souhaitez Clusterer, cliquez sur Démarrer, sur outils d’administration, sur gestion de l’ordinateur, puis sur gestion des disques. (Si la boîte de dialogue contrôle de compte d’utilisateur s’affiche, vérifiez que l’action affichée est celle que vous souhaitez, puis cliquez sur continuer.) Dans gestion des disques, vérifiez que les disques de cluster sont visibles.

8. Si vous souhaitez avoir un volume de stockage de plus de 2 téraoctets, et si vous utilisez l'interface Windows pour contrôler le format du disque, convertissez ce dernier au style de partitionnement GPT (table de partition GUID). Pour ce faire, sauvegardez toutes les données sur le disque, supprimez tous les volumes sur le disque, puis, dans gestion des disques, cliquez avec le bouton droit sur le disque (et non sur une partition), puis cliquez sur convertir en disque GPT.  Pour les volumes de moins de 2 téraoctets, au lieu d'utiliser le format GPT, vous pouvez recourir au style de partition MBR (enregistrement de démarrage principal).

9. Vérifiez le format des volumes ou numéros d’unités logiques exposés. Il est recommandé d'utiliser le format NTFS (pour le disque témoin, vous devez utiliser NTFS).

### <a name="step-2-install-the-file-server-role-and-failover-cluster-feature"></a>Étape 2 : Installer le rôle de serveur de fichiers et la fonctionnalité de cluster de basculement

Dans cette étape, le rôle de serveur de fichiers et la fonctionnalité de cluster de basculement seront installés. Les deux serveurs doivent exécuter Windows Server 2016 ou Windows Server 2019.

#### <a name="using-server-manager"></a>Utilisation de Gestionnaire de serveur

1. Ouvrez **Gestionnaire de serveur** et sous la liste déroulante **gérer** , sélectionnez **Ajouter des rôles et des fonctionnalités**.

   ![Ajouter une fonctionnalité](media/Cluster-File-Server/Cluster-FS-Add-Feature.png)

2. Si la fenêtre **avant de commencer** s’ouvre, cliquez sur **suivant**.

3. Pour le **type d’installation**, sélectionnez installation basée sur **un rôle ou une fonctionnalité** , puis sur **suivant**.

4. Vérifiez que l’option **Sélectionner un serveur du pool de serveurs** est sélectionnée, que le nom de la machine est mis en surbrillance, puis **suivant**.

5. Pour le rôle serveur, dans la liste des rôles, ouvrez **services de fichiers**, sélectionnez **serveur de fichiers**, puis **suivant**.

   ![Ajouter un rôle](media/Cluster-File-Server/Cluster-FS-Add-FS-Role-1.png)

6. Pour les fonctionnalités, dans la liste des fonctionnalités, sélectionnez **clustering de basculement**.  Une boîte de dialogue contextuelle s’affiche et répertorie les outils d’administration également en cours d’installation.  Conserver tous les éléments sélectionnés, choisissez **Ajouter des fonctionnalités** et **suivant**.

   ![Ajouter une fonctionnalité](media/Cluster-File-Server/Cluster-FS-Add-WSFC-1.png)

7. Dans la page confirmation, sélectionnez Installer.

8. Une fois l’installation terminée, redémarrez l’ordinateur.

9. Répétez les étapes sur le deuxième ordinateur.

#### <a name="using-powershell"></a>À l'aide de PowerShell

1. Ouvrez une session PowerShell d’administration en cliquant avec le bouton droit sur le bouton Démarrer, puis en sélectionnant **Windows PowerShell (admin)** .
2. Pour installer le rôle de serveur de fichiers, exécutez la commande suivante :

    ```PowerShell
    Install-WindowsFeature -Name FS-FileServer
    ```

3. Pour installer la fonctionnalité de clustering de basculement et ses outils d’administration, exécutez la commande suivante :

    ```PowerShell
    Install-WindowsFeature -Name Failover-Clustering -IncludeManagementTools
    ```

4. Une fois qu’elles sont terminées, vous pouvez vérifier qu’elles sont installées à l’aide des commandes suivantes :

    ```PowerShell
    Get-WindowsFeature -Name FS-FileServer
    Get-WindowsFeature -Name Failover-Clustering
    ```

5. Une fois que vous avez vérifié qu’ils sont installés, redémarrez l’ordinateur à l’aide de la commande :

    ```PowerShell
    Restart-Computer
    ```

6. Répétez les étapes sur le second serveur.

### <a name="step-3-validate-the-cluster-configuration"></a>Étape 3 : valider la configuration du cluster

Avant de créer un cluster, il est fortement recommandé de valider votre configuration. La validation vous permet de vérifier que la configuration de vos serveurs, du réseau et du stockage répond à un ensemble de conditions requises spécifiques aux clusters de basculement.

#### <a name="using-failover-cluster-manager"></a>Utilisation de Gestionnaire du cluster de basculement

1. Dans **Gestionnaire de serveur**, choisissez la liste déroulante **Outils** , puis sélectionnez **Gestionnaire du cluster de basculement**.

2. Dans **Gestionnaire du cluster de basculement**, accédez à la colonne du milieu sous **gestion** et choisissez **valider la configuration**.

3. Si la fenêtre **avant de commencer** s’ouvre, cliquez sur **suivant**.

4. Dans la fenêtre **Sélectionner des serveurs ou un cluster** , ajoutez les noms des deux ordinateurs qui seront les nœuds du cluster.  Par exemple, si les noms sont NODE1 et NODE2, entrez le nom et sélectionnez **Ajouter**.  Vous pouvez également cliquer sur le bouton **Parcourir** pour rechercher les noms dans Active Directory.  Une fois les deux répertoriés sous **serveurs sélectionnés**, choisissez **suivant**.

5. Dans la fenêtre **options de test** , sélectionnez **exécuter tous les tests (recommandé)** , puis **suivant**.

6. Dans la page **confirmation** , vous obtiendrez la liste de tous les tests qu’il vérifiera.  Choisissez **suivant** pour démarrer les tests.

7. Une fois l’opération terminée, la page **Résumé** s’affiche après l’exécution des tests. Pour afficher les rubriques d'aide qui vous aideront à interpréter les résultats, cliquez sur **En savoir plus sur les tests de validation de cluster**.

8. Toujours dans la page Résumé, cliquez sur Afficher le rapport et lisez les résultats des tests. Apportez les modifications nécessaires à la configuration et réexécutez les tests. <br>Pour afficher les résultats des tests après avoir fermé l’Assistant, consultez *SystemRoot\Cluster\Reports\Validation Report Date and Time. html*.

9. Pour afficher les rubriques d’aide sur la validation de cluster après avoir fermé l’Assistant, dans gestion du cluster de basculement, cliquez sur aide, sur rubriques d’aide, sur l’onglet Sommaire, développez le contenu de l’aide sur le cluster de basculement, puis cliquez sur validation d’une configuration de cluster de basculement. .

#### <a name="using-powershell"></a>À l'aide de PowerShell

1. Ouvrez une session PowerShell d’administration en cliquant avec le bouton droit sur le bouton Démarrer, puis en sélectionnant **Windows PowerShell (admin)** .

2. Pour valider les ordinateurs (par exemple, les noms d’ordinateur NODE1 et NODE2) pour le clustering de basculement, exécutez la commande suivante :

    ```PowerShell
    Test-Cluster -Node "NODE1","NODE2"
    ```
4. Pour afficher les résultats des tests après avoir fermé l’Assistant, consultez le fichier spécifié (dans SystemRoot\Cluster\Reports @ no__t-0, apportez les modifications nécessaires à la configuration et réexécutez les tests.

Pour plus d’informations, consultez [validation d’une configuration de cluster de basculement](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v=ws.11)).

### <a name="step-4-create-the-cluster"></a>Étape 4 : Créer le cluster

La commande suivante permet de créer un cluster à partir des ordinateurs et de la configuration que vous avez.

#### <a name="using-failover-cluster-manager"></a>Utilisation de Gestionnaire du cluster de basculement

1. Dans **Gestionnaire de serveur**, choisissez la liste déroulante **Outils** , puis sélectionnez **Gestionnaire du cluster de basculement**.

2. Dans **Gestionnaire du cluster de basculement**, accédez à la colonne du milieu sous **gestion** et choisissez **créer un cluster**.

3. Si la fenêtre **avant de commencer** s’ouvre, cliquez sur **suivant**.

4. Dans la fenêtre **Sélectionner les serveurs** , ajoutez les noms des deux ordinateurs qui seront les nœuds du cluster.  Par exemple, si les noms sont NODE1 et NODE2, entrez le nom et sélectionnez **Ajouter**.  Vous pouvez également cliquer sur le bouton **Parcourir** pour rechercher les noms dans Active Directory.  Une fois les deux répertoriés sous **serveurs sélectionnés**, choisissez **suivant**.

5. Dans la fenêtre **point d’accès pour l’administration du cluster** , entrez le nom du cluster que vous allez utiliser.  Notez qu’il ne s’agit pas du nom que vous utiliserez pour vous connecter à vos partages de fichiers avec.  Il s’agit simplement d’administrer le cluster.

   > [!NOTE]
   > Si vous utilisez des adresses IP statiques, vous devrez sélectionner le réseau à utiliser et entrer l’adresse IP qu’elle utilisera pour le nom du cluster.  Si vous utilisez DHCP pour vos adresses IP, l’adresse IP est configurée automatiquement pour vous.

6. Choisissez **suivant**.

7. Dans la page **confirmation** , vérifiez ce que vous avez configuré et sélectionnez **suivant** pour créer le cluster.

8. Dans la page **Résumé** , vous obtiendrez la configuration qu’il a créée.  Vous pouvez sélectionner Afficher le rapport pour afficher le rapport de la création.

#### <a name="using-powershell"></a>À l'aide de PowerShell

1. Ouvrez une session PowerShell d’administration en cliquant avec le bouton droit sur le bouton Démarrer, puis en sélectionnant **Windows PowerShell (admin)** .

2. Exécutez la commande suivante pour créer le cluster si vous utilisez des adresses IP statiques.  Par exemple, les noms d’ordinateur sont NODE1 et NODE2, le nom du cluster est CLUSTER et l’adresse IP est 1.1.1.1.

   ```PowerShell
    New-Cluster -Name CLUSTER -Node "NODE1","NODE2" -StaticAddress 1.1.1.1
   ```

3. Exécutez la commande suivante pour créer le cluster si vous utilisez DHCP pour les adresses IP.  Par exemple, les noms d’ordinateur sont NODE1 et NODE2, et le nom du cluster est CLUSTER.

   ```PowerShell    
   New-Cluster -Name CLUSTER -Node "NODE1","NODE2"
   ```

### <a name="steps-for-configuring-a-file-server-failover-cluster"></a>Étapes de configuration d’un cluster de basculement de serveur de fichiers

Pour configurer un cluster de basculement de serveur de fichiers, suivez les étapes ci-dessous.

1. Dans **Gestionnaire de serveur**, choisissez la liste déroulante **Outils** , puis sélectionnez **Gestionnaire du cluster de basculement**.

2. Lorsque Gestionnaire du cluster de basculement s’ouvre, le nom du cluster que vous avez créé doit être automatiquement mis en place.  Si ce n’est pas le cas, accédez à la colonne centrale sous **gestion** et choisissez **se connecter au cluster**.  Entrez le nom du cluster que vous avez créé, puis **OK**.

3. Dans l’arborescence de la console, cliquez sur le signe « > » en regard du cluster que vous avez créé pour développer les éléments situés sous celui-ci.

4. Cliquez avec le bouton droit sur **rôles** et sélectionnez **configurer le rôle**.

5. Si la fenêtre **avant de commencer** s’ouvre, cliquez sur **suivant**.

6. Dans la liste des rôles, choisissez **serveur de fichiers** et **suivant**.

7. Pour le type de serveur de fichiers, sélectionnez **serveur de fichiers pour utilisation générale** et **suivant**.<br>Pour plus d’informations sur les Serveur de fichiers avec montée en puissance parallèle, consultez [vue d’ensemble des serveur de fichiers avec montée en puissance parallèle](sofs-overview.md).

   ![Type de serveur de fichiers](media/Cluster-File-Server/Cluster-FS-File-Server-Type.png)

8. Dans la fenêtre **point d’accès client** , entrez le nom du serveur de fichiers que vous allez utiliser.  Notez qu’il ne s’agit pas du nom du cluster.  Il s’agit de la connectivité de partage de fichiers.  Par exemple, si je souhaite me connecter à \\SERVER, le nom entré est SERVER.

   > [!NOTE]
   > Si vous utilisez des adresses IP statiques, vous devrez sélectionner le réseau à utiliser et entrer l’adresse IP qu’elle utilisera pour le nom du cluster.  Si vous utilisez DHCP pour vos adresses IP, l’adresse IP est configurée automatiquement pour vous.

6. Choisissez **suivant**.

7. Dans la fenêtre **Sélectionner le stockage** , sélectionnez le lecteur supplémentaire (pas le témoin) qui contiendra vos partages et **ensuite**.

8. Dans la page **confirmation** , vérifiez votre configuration, puis sélectionnez **suivant**.

9. Dans la page **Résumé** , vous obtiendrez la configuration qu’il a créée.  Vous pouvez sélectionner Afficher le rapport pour afficher le rapport de création du rôle de serveur de fichiers.

10. Sous **rôles** dans l’arborescence de la console, vous verrez le nouveau rôle que vous avez créé sous le nom que vous avez créé.  Lorsqu’il est mis en surbrillance, sous le volet **actions** à droite, choisissez **Ajouter un partage**.

11. Exécutez l’Assistant partage en entrant les éléments suivants :

    - Type de partage
    - Emplacement/chemin d’accès le dossier partagé sera
    - Nom du partage auquel les utilisateurs se connectent
    - Paramètres supplémentaires tels que l’énumération basée sur l’accès, la mise en cache, le chiffrement, etc.
    - Autorisations au niveau du fichier si elles sont autres que les valeurs par défaut

12. Sur la page **confirmation** , vérifiez ce que vous avez configuré et sélectionnez **créer** pour créer le partage de serveur de fichiers.

13. Dans la page **résultats** , sélectionnez Fermer si le partage a été créé.  S’il n’a pas pu créer le partage, vous obtiendrez les erreurs générées.

14. Choisissez **Fermer**.

15. Répétez ce processus pour tous les partages supplémentaires.