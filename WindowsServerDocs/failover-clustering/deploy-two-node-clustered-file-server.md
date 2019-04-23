---
title: Déploiement d’un serveur de fichiers en cluster à deux nœuds
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 02/01/2019
description: Cet article décrit la création d’un cluster de serveurs de fichiers à deux nœuds
ms.openlocfilehash: fbfde60f60df64514a6a0f514cbabd005544af84
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846410"
---
# <a name="deploying-a-two-node-clustered-file-server"></a>Déploiement d’un serveur de fichiers en cluster à deux nœuds

> S'applique à : Windows Server 2019, Windows Server 2016

Un cluster de basculement est un groupe d'ordinateurs indépendants qui travaillent conjointement pour accroître la disponibilité des applications et des services. Les serveurs en cluster (appelés « nœuds ») sont connectés par des câbles physiques et par des logiciels. En cas de défaillance de l'un des nœuds, un autre prend sa place pour fournir le service (processus appelé « basculement »), garantissant ainsi des interruptions minimales de service pour les utilisateurs.

Ce guide décrit les étapes pour installer et configurer le cluster de basculement de serveur fichiers à usage général qui a deux nœuds. En créant la configuration dans ce guide, vous pouvez en savoir plus sur les clusters de basculement et vous familiariser avec l’interface de composant logiciel enfichable Gestion du Cluster de basculement dans Windows Server 2019 ou Windows Server 2016.

## <a name="overview-for-a-two-node-file-server-cluster"></a>Vue d'ensemble pour un cluster de serveur de fichiers à deux nœuds

Les serveurs dans un cluster de basculement peuvent fonctionner dans divers rôles, y compris les rôles de serveur de fichiers, de serveur Hyper-V ou de serveur de base de données et peuvent fournir une haute disponibilité pour une variété d’autres services et applications. Ce guide décrit comment configurer un cluster de serveur de fichiers à deux nœuds.

En règle générale, un cluster de basculement inclut une unité de stockage connectée physiquement à tous les serveurs du cluster, même si tout volume donné dans le stockage n'est accessible qu'à un serveur à la fois. Le diagramme suivant montre un cluster de basculement à deux nœuds connecté à une unité de stockage.

![Cluster à deux nœuds](media\Cluster-File-Server\Cluster-FS-Overview.png)

Les volumes de stockage ou numéros d'unités logiques exposés aux nœuds d'un cluster ne doivent pas être exposés à d'autres serveurs, y compris les serveurs d'un autre cluster. Le diagramme suivant illustre ce point.

![Numéros d’unités logiques de stockage](media\Cluster-File-Server\Cluster-FS-LUNs.png)

Notez que pour une disponibilité maximale des serveurs, il est important de suivre les meilleures pratiques en matière de gestion de serveurs, par exemple à travers la gestion soigneuse de l'environnement physique des serveurs, le test des modifications des logiciels avant leur implémentation complète, ainsi que le suivi attentif des mises à jour logicielles et des modifications de configuration sur tous les serveurs en cluster.

Le scénario suivant décrit comment un cluster de basculement de serveur de fichiers peut être configuré. Les fichiers qui sont partagés sont sur l'espace de stockage en cluster et l'un des serveurs en cluster peut jouer le rôle de serveur de fichiers qui les partage.

## <a name="shared-folders-in-a-failover-cluster"></a>Dossiers partagés dans un cluster de basculement

La liste suivante décrit les fonctionnalités de configuration de dossier partagé qui sont intégrée au clustering de basculement :

- Affichage est limité aux dossiers partagés en cluster uniquement (aucune association avec des dossiers partagés non cluster) : Quand un utilisateur affichez des dossiers partagés en spécifiant le chemin d’accès d’un serveur de fichiers en cluster, l’affichage inclut uniquement les dossiers partagés qui font partie du rôle de serveur de fichiers spécifiques. Il exclut les dossiers partagés non cluster et partie partages des rôles de serveur de fichiers distincts qui se trouvent sur un nœud du cluster.

- Énumération basée sur l’accès : Vous pouvez utiliser l'énumération basée sur l'accès pour masquer un dossier spécifique dans l'affichage de l'utilisateur. Au lieu de permettre aux utilisateurs de voir un dossier sans qu'ils puissent toutefois y accéder, vous pouvez choisir de les empêcher de voir le dossier. Vous pouvez configurer l’énumération basée sur l’accès pour un dossier partagé en cluster de la même façon que pour un dossier partagé en cluster.

- Accès hors connexion : Vous pouvez configurer l'accès hors connexion (mise en cache) pour un dossier partagé en cluster comme pour un dossier partagé qui n'est pas en cluster.

- Disques en cluster sont toujours reconnus comme faisant partie du cluster : Si vous utilisez l’interface de cluster de basculement, l’Explorateur Windows ou le composant logiciel enfichable Gestion du partage et le stockage, Windows reconnaît si un disque a été désigné comme étant dans le stockage de cluster. Si ce disque a déjà été configuré dans la gestion du Cluster de basculement dans le cadre d’un serveur de fichiers en cluster, vous pouvez ensuite utiliser une des interfaces mentionnées précédemment pour créer un partage sur le disque. Si ce disque n'a pas été configuré comme faisant partie d'un serveur de fichiers en cluster, vous ne pouvez pas créer de partage sur ce disque par inadvertance. En effet, le système afficherait alors un message d'erreur indiquant que, pour être partagé, le disque doit d'abord être configuré comme faisant partie d'un serveur de fichiers en cluster.

- Intégration des Services for Network File System : Le rôle de serveur de fichiers dans Windows Server inclut le service de rôle optionnel appelé Services pour NFS Network File System (). En installant le service de rôle et en configurant les dossiers partagés avec Services pour NFS, vous pouvez créer un serveur de fichiers en cluster qui prend en charge les clients basés sur UNIX.

## <a name="requirements-for-a-two-node-failover-cluster"></a>Configuration requise pour un cluster de basculement à deux nœuds

Pour un cluster de basculement dans Windows Server 2016 ou Windows Server 2019 pour être considéré comme une solution officiellement pris en charge par Microsoft, la solution doit respecter les critères suivants.

- Tous les composants matériels et logiciels doivent se conformer aux qualifications du logo approprié. Pour Windows Server 2016, il s’agit du logo « Certifié pour Windows Server 2016 ». Pour Windows Server 2019, il s’agit du logo « Certifié pour Windows Server 2019 ». Pour plus d’informations sur les systèmes matériels et logiciels ont été certifiés, visitez le site Microsoft [catalogue Windows Server](https://www.windowsservercatalog.com/default.aspx) site.

- La solution entièrement configurée (serveurs, réseau et stockage) doit réussir tous les tests dans l’Assistant de validation, qui fait partie du composant logiciel enfichable cluster de basculement.

Les éléments suivants sont nécessaires pour un cluster de basculement à deux nœuds.

- **serveurs :** Nous vous recommandons d’utiliser des ordinateurs correspondants avec des composants similaires ou identiques.  Les serveurs pour un cluster de basculement à deux nœuds doivent exécuter la même version de Windows Server. Ils doivent également avoir les mêmes mises à jour de logiciels (correctifs).

- **Cartes et câble réseau :** Le matériel réseau, comme d’autres composants de la solution de cluster de basculement, doit être compatible avec Windows Server 2016 ou Windows Server 2019. Si vous utilisez iSCSI, les cartes réseau doivent être dédiées à la communication de réseau ou iSCSI, pas les deux. Dans l’infrastructure réseau qui connecte vos nœuds de cluster, évitez d’avoir des points de défaillance uniques. Il existe plusieurs façons d'y parvenir. Vous pouvez connecter vos nœuds de cluster à l'aide de plusieurs réseaux distincts. Vous pouvez également connecter vos nœuds de cluster à l’aide d’un réseau construit avec des cartes réseau associées, des commutateurs redondants, des routeurs redondants ou tout autre matériel similaire permettant de supprimer les points de défaillance uniques.

   > [!NOTE]
   > Si les nœuds de cluster sont connectés à un réseau unique, le réseau transmet les exigences de redondance dans l’Assistant Validation d’une Configuration.  Toutefois, le rapport inclut un avertissement que le réseau ne doit pas avoir un point de défaillance unique.

- **Contrôleurs de périphériques ou adaptateurs appropriés pour le stockage :**
    - **Serial Attached SCSI ou Fibre Channel :** si vous utilisez SAS (Serial Attached SCSI) ou Fibre Channel, dans l'ensemble des serveurs en cluster, tous les composants de la pile de stockage doivent être identiques. Il est nécessaire que le logiciel à plusieurs chemins d’e/s (MPIO) et les composants logiciels de Module spécifique au périphérique (DSM) être identiques.  Il est recommandé que les contrôleurs de périphériques de stockage de masse, c’est-à-dire l’adaptateur de bus hôte (HBA), les pilotes HBA et le microprogramme HBA, qui sont attachés à l’espace de stockage en cluster soient identiques. Si vous utilisez des adaptateurs de bus hôte non similaires, vous devez vérifier auprès du fournisseur de stockage que vous respectez les configurations prises en charge ou recommandées.
    - **iSCSI :** Si vous utilisez iSCSI, chaque serveur en cluster doit avoir une ou plusieurs cartes réseau ou les adaptateurs de bus hôte dédiés au stockage ISCSI. Le réseau que vous utilisez pour iSCSI ne peut pas être utilisé pour la communication réseau. Dans tous les serveurs en cluster, les cartes réseau que vous utilisez pour la connexion à la cible de stockage iSCSI doivent être identiques ; en outre, il est recommandé d’utiliser des cartes Gigabit Ethernet ou une connexion plus rapide.  

- **Stockage :** Vous devez utiliser un stockage partagé est certifié pour Windows Server 2016 ou Windows Server 2019.
  
    Pour un cluster de basculement à deux nœuds, le stockage doit contenir au moins deux volumes distincts (numéros d’unités logiques) si vous utilisez un disque témoin pour le quorum. Le disque témoin est un disque de l'espace de stockage en cluster qui est conçu pour conserver une copie de la base de données de configuration du cluster. Pour cet exemple de cluster à deux nœuds, la configuration de quorum sera nœud et disque majoritaires. Nœud et disque majoritaires signifie que les nœuds et le disque témoin contiennent des copies de la configuration du cluster, et le cluster possède un quorum tant qu’une majorité (deux sur trois) de ces copies est disponible. L’autre volume (LUN) contiendra les fichiers qui sont partagés par les utilisateurs.

    Les conditions requises pour le stockage incluent les éléments suivants :

    - Pour utiliser la prise en charge de disque native incluse dans le clustering de basculement, utilisez des disques de base au lieu de disques dynamiques.
    - Il est recommandé de formater les partitions avec NTFS (pour le disque témoin, la partition doit être NTFS).
    - Pour le style de partition du disque, vous pouvez choisir entre une partition MBR (Master Boot Record) ou une partition GPT (table de partition GUID).
    - Le stockage doit répondre correctement aux commandes SCSI spécifiques, le stockage doit suivre le standard appelé 3 SCSI Primary Commands-(SPC-3). En particulier, le stockage doit prendre en charge les réservations persistantes telles que spécifiées dans le standard SPC-3. 
    -  Le pilote de miniport utilisé pour le stockage doit fonctionner avec le pilote de stockage Microsoft Storport.

## <a name="deploying-storage-area-networks-with-failover-clusters"></a>Déploiement de réseaux SAN avec des clusters de basculement

Lorsque vous déployez un réseau de stockage (SAN) avec un cluster de basculement, les instructions suivantes doivent être respectées.

- **Confirmer la certification du stockage :** À l’aide de la [catalogue Windows Server](https://www.windowsservercatalog.com/default.aspx) de site, vérifiez le stockage du fournisseur, y compris les pilotes, microprogrammes et logiciels est certifié pour Windows Server 2016 ou Windows Server 2019.

- **Isoler les périphériques de stockage, un cluster par périphérique :** les serveurs de clusters distincts ne doivent pas être en mesure d'accéder aux mêmes dispositifs de stockage. Dans la plupart des cas, un numéro d'unité logique utilisé pour un jeu de serveurs de cluster doit être isolé de tous les autres serveurs via le masquage ou la répartition en zones du numéro d'unité logique.

- **Envisagez d’utiliser le logiciel à plusieurs chemins d’e/s :** dans un ensemble fibre optique de stockage à haut niveau de disponibilité, vous pouvez déployer des clusters de basculement avec plusieurs adaptateurs de bus hôte à l'aide de logiciels MPIO (Multipath I/O). Cela fournit le niveau le plus élevé de redondance et de disponibilité. La solution multivoie doit être basée sur Microsoft Multipath i/o d’e/s (MPIO). Le fournisseur de matériel de stockage peut fournir un module spécifique au périphérique MPIO (DSM) pour votre matériel, même si Windows Server 2016 et Windows Server 2019 incluent un ou plusieurs modules DSM dans le cadre du système d’exploitation.

## <a name="network-infrastructure-and-domain-account-requirements"></a>Configuration requise de l'infrastructure et du compte de domaine

Pour un cluster de basculement à deux nœuds, vous avez besoin de l'infrastructure réseau suivante et d'un compte d'administration avec les autorisations de domaine suivantes :

- **Paramètres réseau et adresses IP :** lorsque vous utilisez des cartes réseau identiques pour un réseau, utilisez également des paramètres de communication identiques sur ces cartes (par exemple pour la vitesse, le mode duplex, le contrôle de flux et le type de média). En outre, comparez les paramètres de la carte réseau et du commutateur auquel elle est connectée, et vérifiez qu'aucun paramètre n'est en conflit.

    Si vous avez des réseaux privés qui ne sont pas routés vers le reste de votre infrastructure réseau, vérifiez que chacun de ces réseaux privés utilise un sous-réseau unique. Cela est nécessaire même si vous attribuez une adresse IP unique à chaque carte réseau. Par exemple, si vous avez un nœud de cluster dans un bureau central qui utilise un réseau physique, et un autre nœud dans une filiale qui utilise un autre réseau physique, ne spécifiez pas 10.0.0.0/24 pour les deux réseaux, même si vous attribuez une adresse IP unique à chaque carte adaptateur.

    Pour plus d’informations sur les cartes réseau, consultez Configuration matérielle requise pour un cluster de basculement à deux nœuds, plus haut dans ce guide.

- **DNS :** les serveurs du cluster doivent utiliser le système DNS (Domain Name System) pour la résolution de noms. Le protocole de mise à jour dynamique DNS peut être utilisé.

- **Rôle de domaine :** tous les serveurs du cluster doivent se trouver dans le même domaine Active Directory. Il est recommandé d'attribuer le même rôle de domaine à tous les serveurs en cluster (serveur membre ou contrôleur de domaine). Le rôle recommandé est celui de serveur membre.

- **Contrôleur de domaine :** il est recommandé de faire de vos serveurs en cluster des serveurs membres. Si tel est le cas, vous avez besoin d'un autre serveur qui joue le rôle de contrôleur de domaine dans le domaine qui contient votre cluster de basculement.

- **Clients :** Pour les besoins du test, vous pouvez connecter un ou plusieurs clients en réseau au cluster de basculement que vous créez et observer l'effet sur un client lorsque vous déplacez ou basculez le serveur de fichiers en cluster d'un nœud de cluster à l'autre.

- **Compte d’administration du cluster :** lorsque vous créez un cluster ou que vous y ajoutez des serveurs, vous devez avoir ouvert une session sur le domaine à l'aide d'un compte qui dispose de droits d'administrateur et d'autorisations pour tous les serveurs de ce cluster. Le compte n’a pas besoin d’être un compte Admins du domaine, mais peut être un compte Utilisateurs du domaine situé dans le groupe Administrateurs sur chaque serveur en cluster. En outre, si le compte n’est pas un compte Admins du domaine, le compte (ou le groupe dont le compte est membre) doit recevoir le **créer des objets ordinateur** et **lire toutes les propriétés** autorisations dans le domaine unité d’organisation (UO) qui est résidera dans.

## <a name="steps-for-installing-a-two-node-file-server-cluster"></a>Étapes pour l'installation d'un cluster de serveur de fichiers à deux nœuds

Vous devez effectuer les étapes suivantes pour installer un cluster de basculement de serveur de fichiers à deux nœuds.

Étape 1 : connecter les serveurs de cluster aux réseaux et au stockage

Étape 2 : installer la fonctionnalité de cluster de basculement

Étape 3 : valider la configuration du cluster

Étape 4 : créer le cluster

Si vous avez déjà installé les nœuds du cluster et que vous souhaitez configurer un cluster de basculement de serveur de fichiers, voir procédure de configuration d’un cluster de serveur de fichiers à deux nœuds, plus loin dans ce guide.

### <a name="step-1-connect-the-cluster-servers-to-the-networks-and-storage"></a>Étape 1 : connecter les serveurs de cluster aux réseaux et au stockage

Pour un réseau de cluster de basculement, évitez d'avoir des points de défaillance uniques. Il existe plusieurs façons d'y parvenir. Vous pouvez connecter vos nœuds de cluster à l'aide de plusieurs réseaux distincts. Vous pouvez également connecter vos nœuds de cluster à l'aide d'un réseau construit avec des cartes réseau associées, des commutateurs redondants, des routeurs redondants ou tout autre matériel similaire permettant de supprimer les points de défaillance uniques (si vous utilisez un réseau pour iSCSI, vous devez le créer en plus des autres réseaux).

Pour un cluster de serveur de fichiers à deux nœuds, lorsque vous connectez les serveurs à l'espace de stockage en cluster, vous devez exposer au moins deux volumes (numéros d'unités logiques). Vous pouvez exposer des volumes supplémentaires autant que nécessaire pour un test complet de votre configuration. N'exposez pas les volumes en cluster à des serveurs qui ne sont pas dans le cluster.

#### <a name="to-connect-the-cluster-servers-to-the-networks-and-storage"></a>Pour connecter les serveurs de cluster aux réseaux et au stockage

1. Passez en revue les détails sur les réseaux dans la configuration matérielle requise pour un cluster de basculement à deux nœuds et les exigences de compte des infrastructures et de domaine réseau pour un cluster de basculement à deux nœuds, plus haut dans ce guide.

2. Connectez et configurez les réseaux qui seront utilisés par les serveurs du cluster.

3. Si votre configuration de test inclut des clients ou un contrôleur de domaine non-membres d'un cluster, vérifiez que ces ordinateurs peuvent se connecter aux serveurs en cluster via au moins un réseau.

4. Suivez les instructions du fabricant pour connecter physiquement les serveurs au stockage.

5. Vérifiez que les disques (numéros d'unités logiques) que vous souhaitez utiliser dans le cluster sont exposés aux serveurs que vous mettrez en cluster (et uniquement ces serveurs). Vous pouvez utiliser les interfaces suivantes pour exposer des disques ou des numéros d'unités logiques :

    - Interface fournie par le fabricant du stockage.

    - Interface iSCSI appropriée, si vous utilisez iSCSI.

6. Si vous avez acheté un logiciel qui contrôle le format ou le fonctionnement du disque, suivez les instructions du fournisseur sur l’utilisation de ce logiciel avec Windows Server.

7. Sur l’un des serveurs que vous souhaitez un cluster, cliquez sur Démarrer, cliquez sur Outils d’administration, cliquez sur gestion de l’ordinateur, puis cliquez sur gestion des disques. (Si la boîte de dialogue contrôle de compte d’utilisateur s’affiche, vérifiez que l’action affichée est ce que vous voulez, puis cliquez sur Continuer.) Dans Gestion des disques, vérifiez que les disques de cluster sont visibles.

8. Si vous souhaitez avoir un volume de stockage de plus de 2 téraoctets, et si vous utilisez l'interface Windows pour contrôler le format du disque, convertissez ce dernier au style de partitionnement GPT (table de partition GUID). Pour ce faire, sauvegardez toutes les données sur le disque, supprimez tous les volumes sur le disque et puis, dans Gestion des disques, cliquez sur le disque (et non sur une partition) et cliquez sur Convertir en disque GPT.  Pour les volumes de moins de 2 téraoctets, au lieu d'utiliser le format GPT, vous pouvez recourir au style de partition MBR (enregistrement de démarrage principal).

9. Vérifiez le format des volumes ou numéros d’unités logiques exposés. Il est recommandé d'utiliser le format NTFS (pour le disque témoin, vous devez utiliser NTFS).

### <a name="step-2-install-the-file-server-role-and-failover-cluster-feature"></a>Étape 2 : Installez la fonctionnalité cluster fichier le rôle et le basculement du serveur

Dans cette étape, la fonctionnalité cluster fichier le rôle et le basculement du serveur sera installée. Les deux serveurs doivent exécuter Windows Server 2016 ou Windows Server 2019.

#### <a name="using-server-manager"></a>À l’aide du Gestionnaire de serveur

1. Ouvrez **le Gestionnaire de serveur** et, sous la **gérer** liste déroulante, sélectionnez **Ajout de rôles et fonctionnalités**.

   ![Ajouter la fonctionnalité](media\Cluster-File-Server\Cluster-FS-Add-Feature.png)

2. Si le **avant de commencer** fenêtre s’ouvre, choisissez **suivant**.

3. Pour le **Type d’Installation**, sélectionnez **installation en fonction du rôle ou une fonctionnalité** et **suivant**.

4. Vérifiez **sélectionner un serveur du pool de serveurs** est sélectionnée, le nom de l’ordinateur est mis en surbrillance, et **suivant**.

5. Pour le rôle de serveur, dans la liste des rôles, ouvrez **Services de fichiers**, sélectionnez **serveur de fichiers**, et **suivant**.

   ![Ajouter un rôle](media\Cluster-File-Server\Cluster-FS-Add-FS-Role-1.png)

6. Pour les fonctionnalités, à partir de la liste des fonctionnalités, sélectionnez **le Clustering de basculement**.  Affiche une boîte de dialogue contextuelle qui répertorie les outils d’administration également en cours d’installation.  Tout conserver sélectionné, choisissez **ajouter des fonctionnalités** et **suivant**.

   ![Ajouter la fonctionnalité](media\Cluster-File-Server\Cluster-FS-Add-WSFC-1.png)

7. Dans la page de Confirmation, sélectionnez Installer.

8. Une fois l’installation terminée, redémarrez l’ordinateur.

9. Répétez les étapes sur le deuxième ordinateur.

#### <a name="using-powershell"></a>À l'aide de PowerShell

1. Ouvrez une session PowerShell d’administration en double-cliquant sur le bouton Démarrer, puis en sélectionnant **Windows PowerShell (Admin)**.
2. Pour installer le rôle de serveur de fichiers, exécutez la commande :

    ```PowerShell
    Install-WindowsFeature -Name FS-FileServer
    ```

3. Pour installer la fonctionnalité de Clustering de basculement et de ses outils de gestion, exécutez la commande :

    ```PowerShell
    Install-WindowsFeature -Name Failover-Clustering -IncludeManagementTools
    ```

4. Une fois que ceux-ci ont terminé, vous pouvez vérifier qu’ils sont installés avec les commandes :

    ```PowerShell
    Get-WindowsFeature -Name FS-FileServer
    Get-WindowsFeature -Name Failover-Clustering
    ```

5. Une fois vérifié qu’ils sont installés, redémarrez l’ordinateur avec la commande :

    ```PowerShell
    Restart-Computer
    ```

6. Répétez les étapes sur le deuxième serveur.

### <a name="step-3-validate-the-cluster-configuration"></a>Étape 3 : valider la configuration du cluster

Avant de créer un cluster, il est fortement recommandé de valider votre configuration. La validation vous permet de vérifier que la configuration de vos serveurs, du réseau et du stockage répond à un ensemble de conditions requises spécifiques aux clusters de basculement.

#### <a name="using-failover-cluster-manager"></a>À l’aide du Gestionnaire du Cluster de basculement

1. À partir de **le Gestionnaire de serveur**, choisissez le **outils** vers le bas et sélectionnez **Gestionnaire du Cluster de basculement**.

2. Dans **Gestionnaire du Cluster de basculement**, accédez à la colonne du milieu sous **gestion** et choisissez **valider la Configuration**.

3. Si le **avant de commencer** fenêtre s’ouvre, choisissez **suivant**.

4. Dans le **sélectionner des serveurs ou un Cluster** fenêtre, ajouter dans les noms des deux ordinateurs qui seront les nœuds du cluster.  Par exemple, si les noms sont NODE1 et NODE2, entrez le nom et sélectionnez **ajouter**.  Vous pouvez également choisir le **Parcourir** bouton pour rechercher les noms Active Directory.  Une fois que les deux sont répertoriés sous **serveurs sélectionnés**, choisissez **suivant**.

5. Dans le **Options de test** fenêtre, sélectionnez **exécuter tous les tests (recommandés)**, et **suivant**.

6. Sur le **Confirmation** page, il vous donnera la liste de tous les tests, il vérifie.  Choisissez **suivant** et les tests commencera.

7. Une fois terminé, le **Résumé** page s’affiche après l’exécution des tests. Pour afficher les rubriques d'aide qui vous aideront à interpréter les résultats, cliquez sur **En savoir plus sur les tests de validation de cluster**.

8. Sur la page Résumé, cliquez sur Afficher le rapport et lisez les résultats de test. Apportez les modifications nécessaires dans la configuration et réexécutez les tests. <br>Pour afficher les résultats des tests après avoir fermé l’Assistant, consultez *date du rapport de SystemRoot\Cluster\Reports\Validation and time.html*.

9. Pour afficher les rubriques d’aide sur la validation de cluster après avoir fermé l’Assistant, dans la gestion du Cluster de basculement, cliquez sur aide, cliquez sur rubriques d’aide, cliquez sur l’onglet Sommaire, développez le contenu du cluster de basculement aide, puis cliquez sur validation d’une Configuration de Cluster de basculement .

#### <a name="using-powershell"></a>À l'aide de PowerShell

1. Ouvrez une session PowerShell d’administration en double-cliquant sur le bouton Démarrer, puis en sélectionnant **Windows PowerShell (Admin)**.

2. Pour valider les ordinateurs (par exemple, les noms d’ordinateur en cours de NODE1 et NODE2) pour le Clustering de basculement, exécutez la commande :

    ```PowerShell
    Test-Cluster -Node "NODE1","NODE2"
    ```
4. Pour afficher les résultats des tests après avoir fermé l’Assistant, consultez le fichier spécifié (dans SystemRoot\Cluster\Reports\), puis apportez les modifications nécessaires dans la configuration, réexécutez les tests.

Pour plus d’informations, consultez [validation d’une Configuration de Cluster de basculement](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v=ws.11)).

### <a name="step-4-create-the-cluster"></a>Étape 4 : Créer le Cluster

Ce qui suit crée un cluster de machines et de la configuration que vous avez.

#### <a name="using-failover-cluster-manager"></a>À l’aide du Gestionnaire du Cluster de basculement

1. À partir de **le Gestionnaire de serveur**, choisissez le **outils** vers le bas et sélectionnez **Gestionnaire du Cluster de basculement**.

2. Dans **Gestionnaire du Cluster de basculement**, accédez à la colonne du milieu sous **gestion** et choisissez **création d’un Cluster**.

3. Si le **avant de commencer** fenêtre s’ouvre, choisissez **suivant**.

4. Dans le **sélectionner des serveurs** fenêtre, ajouter dans les noms des deux ordinateurs qui seront les nœuds du cluster.  Par exemple, si les noms sont NODE1 et NODE2, entrez le nom et sélectionnez **ajouter**.  Vous pouvez également choisir le **Parcourir** bouton pour rechercher les noms Active Directory.  Une fois que les deux sont répertoriés sous **serveurs sélectionnés**, choisissez **suivant**.

5. Dans le **Point d’accès pour administrer le Cluster** fenêtre, entrez le nom du cluster que vous souhaitez utiliser.  Veuillez noter que cela n’est pas le nom que vous utiliserez pour vous connecter à vos partages de fichiers avec.  Il s’agit pour l’administration simplement le cluster.

   > [!NOTE]
   > Si vous utilisez des adresses IP statiques, vous devez sélectionner le réseau à utiliser et entrez l’adresse IP il utilisera pour le nom du cluster.  Si vous utilisez DHCP pour vos adresses IP, l’adresse IP est configurée automatiquement, pour vous.

6. Choisissez **suivant**.

7. Sur le **Confirmation** page, vérifiez que vous avez configuré et sélectionnez **suivant** pour créer le Cluster.

8. Sur le **Résumé** page, il vous donnera la configuration qu’il a créé.  Vous pouvez sélectionner Afficher le rapport pour afficher le rapport de la création.

#### <a name="using-powershell"></a>À l'aide de PowerShell

1. Ouvrez une session PowerShell d’administration en double-cliquant sur le bouton Démarrer, puis en sélectionnant **Windows PowerShell (Admin)**.

2. Exécutez la commande suivante pour créer le cluster si vous utilisez des adresses IP statiques.  Par exemple, les noms d’ordinateurs sont NODE1 et NODE2, le nom du cluster sera le CLUSTER et l’adresse IP sera 1.1.1.1.

   ```PowerShell
    New-Cluster -Name CLUSTER -Node "NODE1","NODE2" -StaticAddress 1.1.1.1
   ```

3. Exécutez la commande suivante pour créer le cluster si vous utilisez DHCP pour les adresses IP.  Par exemple, les noms d’ordinateurs sont NODE1 et NODE2, et le nom du cluster sera le CLUSTER.

   ```PowerShell    
   New-Cluster -Name CLUSTER -Node "NODE1","NODE2"
   ```

### <a name="steps-for-configuring-a-file-server-failover-cluster"></a>Étapes de configuration d’un cluster de basculement de serveur de fichiers

Pour configurer un cluster de basculement de serveur de fichiers, suivez les étapes ci-dessous.

1. À partir de **le Gestionnaire de serveur**, choisissez le **outils** vers le bas et sélectionnez **Gestionnaire du Cluster de basculement**.

2. Lorsque le Gestionnaire du Cluster de basculement s’ouvre, il doit mettre automatiquement le nom du cluster que vous avez créé.  Dans le cas contraire, accédez à la colonne du milieu sous **gestion** et choisissez **se connecter au Cluster**.  Entrez le nom du cluster que vous avez créé et **OK**.

3. Dans l’arborescence de la console, cliquez sur le « > » en regard du cluster que vous avez créé pour développer les éléments situés en dessous.

4. Cliquez sur le droit de la souris sur **rôles** et sélectionnez **configurer un rôle**.

5. Si le **avant de commencer** fenêtre s’ouvre, choisissez **suivant**.

6. Dans la liste des rôles, choisissez **serveur de fichiers** et **suivant**.

7. Pour le Type de serveur de fichiers, sélectionnez **serveur de fichiers pour une utilisation générale** et **suivant**.<br>Pour plus d’informations sur le serveur de fichiers avec montée en puissance, consultez [vue d’ensemble du serveur de fichiers avec montée en puissance](sofs-overview.md).

   ![Type de serveur de fichiers](media\Cluster-File-Server\Cluster-FS-File-Server-Type.png)

8. Dans le **Point d’accès Client** fenêtre, entrez le nom du serveur de fichiers que vous souhaitez utiliser.  Veuillez noter que cela n’est pas le nom du cluster.  Il s’agit de la connectivité de partage de fichier.  Par exemple, si je souhaite me connecter à \\SERVER, le nom d’entrée est serveur.

   > [!NOTE]
   > Si vous utilisez des adresses IP statiques, vous devez sélectionner le réseau à utiliser et entrez l’adresse IP il utilisera pour le nom du cluster.  Si vous utilisez DHCP pour vos adresses IP, l’adresse IP est configurée automatiquement, pour vous.

6. Choisissez **suivant**.

7. Dans le **sélectionner le stockage** fenêtre, sélectionnez le lecteur supplémentaire (pas le témoin) qui contiendra vos partages et **suivant**.

8. Sur le **Confirmation** page, vérifiez votre configuration, sélectionnez **suivant**.

9. Sur le **Résumé** page, il vous donnera la configuration qu’il a créé.  Vous pouvez sélectionner Afficher le rapport pour afficher le rapport de la création de rôle de serveur de fichiers.

10. Sous **rôles** dans l’arborescence de la console, vous verrez le nouveau rôle que vous avez créé répertoriés en tant que le nom que vous avez créé.  Avec elle mise en surbrillance, sous la **Actions** volet droit, choisissez **ajouter un partage de**.

11. Exécuter l’Assistant Partage de saisie de ce qui suit :

    - Type de partage, qu'il sera
    - Emplacement/chemin d’accès du dossier partagé seront
    - Le nom des utilisateurs partage se connecte au
    - Paramètres supplémentaires tels que l’énumération basée sur l’accès, la mise en cache, chiffrement, etc.
    - Autorisations au niveau de fichiers si elles seront autres que les valeurs par défaut

12. Sur le **Confirmation** page, vérifiez que vous avez configuré et sélectionnez **créer** pour créer le partage de serveur de fichiers.

13. Sur le **résultats** , sélectionnez Fermer s’il créé le partage.  Si elle ne peut pas créer le partage, il vous donnera les erreurs survenues.

14. Choisissez **fermer**.

15. Répétez ce processus pour tous les partages supplémentaires.