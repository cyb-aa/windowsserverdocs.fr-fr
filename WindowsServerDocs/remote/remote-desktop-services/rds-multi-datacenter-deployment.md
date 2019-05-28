---
title: Géo-redondant centres de données des services Bureau à distance dans Azure
description: Découvrez comment créer un déploiement services Bureau à distance qui utilise plusieurs centres de données pour fournir une haute disponibilité dans différents emplacements géographiques.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 61c36528-cf47-4af0-83c1-a883f79a73a5
author: haley-rowland
ms.author: elizapo
ms.date: 06/14/2017
manager: dongill
ms.openlocfilehash: 2d12062f302c28a8124e0aa49af7f441e77ffe33
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222792"
---
# <a name="create-a-geo-redundant-multi-data-center-rds-deployment-for-disaster-recovery"></a>Créer un centre de géo-redondant, donnée multi déploiement des services Bureau à distance pour la récupération d’urgence

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016

Vous pouvez activer la récupération d’urgence pour votre déploiement des Services Bureau à distance en exploitant plusieurs centres de données dans Azure. Contrairement à un déploiement services Bureau à distance hautement disponible standard (comme indiqué dans le [architecture des Services Bureau à distance](desktop-hosting-logical-architecture.md)), qui utilise des centres de données dans une seule région Azure (par exemple, Europe de l’ouest), un déploiement de plusieurs centres de données utilise des données centres dans plusieurs emplacements géographiques, accroître la disponibilité de votre déploiement : un Azure centre de données peuvent être indisponibles, mais il est peu probable que plusieurs régions seraient s’arrêtent en même temps. En déployant une architecture de services Bureau à distance géo-redondant, vous pouvez activer le basculement en cas de défaillance catastrophique d’une région entière.

Vous pouvez utiliser les instructions ci-dessous pour tirer parti des services d’infrastructure Microsoft Azure et des services Bureau à distance pour fournir des services d’hébergement et les licences d’accès abonné (SAL) de bureau géo-redondant à plusieurs clients via la [fournisseur de services Microsoft Programme de licence SPLA (Agreement)](https://www.microsoft.com/hosting/licensing/splabenefits.aspx). Vous pouvez également utiliser les étapes ci-dessous pour créer un service d’hébergement géo-redondant pour vos employés à l’aide de [utilisateur licences droits via Software Assurance étendus](https://download.microsoft.com/download/6/B/A/6BA3215A-C8B5-4AD1-AA8E-6C93606A4CFB/Windows_Server_2012_R2_Remote_Desktop_Services_Licensing_Datasheet.pdf).

## <a name="logical-architecture-for-high-availability---single-and-multiple-regions"></a>Architecture logique pour la haute disponibilité - unique et plusieurs régions
L’illustration suivante montre l’architecture pour un déploiement à haute disponibilité dans une seule région Azure :

![Un déploiement à haute disponibilité dans une seule région Azure](media/rds-ha-single-region.png)

Le déploiement se compose de trois couches :

- Services Azure - les interfaces de gestion Azure, y compris le portail Azure et les API et les services de mise en réseau publics, tels que DNS et l’adressage IP public.
- Hébergement de service - machines virtuelles, réseaux, stockage, services Azure et les services de rôle Windows Server du bureau
- Azure Fabric - systèmes d’exploitation Windows qui exécute le rôle Hyper-V, permet de virtualiser les serveurs physiques, les unités de stockage, les commutateurs réseau et les routeurs. À l’aide d’Azure Fabric vous permet de créer des machines virtuelles, réseaux, stockage et les applications indépendantes du matériel sous-jacent.


En comparaison, voici l’architecture pour un déploiement qui utilise plusieurs centres de données Azure :

![Un déploiement services Bureau à distance qui utilise plusieurs régions Azure](media/rds-ha-multi-region.png)

Tout le déploiement des services Bureau à distance est répliqué dans une seconde région Azure pour créer un déploiement géo-redondant. Cette architecture utilise un modèle actif / passif, où un seul déploiement de services Bureau à distance est en cours d’exécution à la fois. Une connexion de réseau virtuel à réseau virtuel permet les deux environnements de communiquer entre eux. Les déploiements de services Bureau à distance sont basés sur un seul domaine Active Directory forêt / et les serveurs AD répliquent entre les deux déploiements, les utilisateurs de signification peuvent se connecter à un déploiement en utilisant les informations d’identification. Paramètres utilisateur et données stockées dans des disques de profil utilisateur (UPD) sont stockées sur un serveur de fichiers de cluster à deux nœuds montée espaces de stockage Direct (SOFS). Un deuxième cluster d’espaces de stockage Direct identique est déployé dans la seconde région (passive), et le réplica de stockage est utilisé pour répliquer les profils utilisateur à partir de l’actif au déploiement passif. Azure Traffic Manager est utilisé pour diriger automatiquement les utilisateurs finaux à quelle que soit la déploiement est actuellement actif - à partir de la perspective de l’utilisateur final, ils accéder au déploiement à l’aide d’une URL unique et ne sont pas conscients de la région dans laquelle ils finissent par utiliser.


Vous *pourrait* créer un déploiement services Bureau à distance non hautement disponible dans chaque région, mais si même une seule machine virtuelle est redémarrée dans une région, un basculement se produit, ce qui augmente les probabilités de basculement qui se produisent avec associé impacts sur les performances.

## <a name="deployment-steps"></a>Étapes de déploiement
Dans Azure pour créer un déploiement de RDS géo-redondant plusieurs centres de données, créez les ressources suivantes :

1. Séparent les deux groupes de ressources dans deux régions Azure. Par exemple, A RG (le déploiement actif, RG signifie « groupe de ressources ») et B RG (le déploiement passif).
2. Un déploiement Active Directory hautement disponible dans RG A. Vous pouvez utiliser la [nouveau domaine Active Directory avec modèle de contrôleurs de domaine 2](https://azure.microsoft.com/resources/templates/active-directory-new-domain-ha-2-dc/) pour créer le déploiement.
3. Un déploiement de RDS hautement disponible dans RG A. Use le [services Bureau à distance à l’aide d’active directory existant de déploiement de batterie de serveurs](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/) modèle pour créer le déploiement de services Bureau à distance de base, puis suivez les informations contenues dans [des Services Bureau à distance - élevé disponibilité](rds-plan-high-availability.md) pour configurer les autres composants de services Bureau à distance pour la haute disponibilité.
4. Un réseau virtuel dans RG B - Vérifiez que vous utilisez un espace d’adressage qui ne chevauche pas le déploiement dans RG A.
5. Un [connexion de réseau virtuel à réseau virtuel](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps) entre les groupes de ressources de deux.
6. Deux machines virtuelles de AD dans un groupe de disponibilité définie dans le groupe de ressources B - Assurez-vous que les noms de machine virtuelle sont différents des machines virtuelles AD dans RG A. déployer deux machines virtuelles Windows Server 2016 dans un groupe de disponibilité unique définie, installez le rôle Services de domaine Active Directory, puis de les promouvoir à la suite de domaine rouleau de peinture dans le domaine que vous avez créé à l’étape 1.
7. Un deuxième déploiement RDS hautement disponible dans RG B. 
   1. Utilisez le [services Bureau à distance à l’aide d’active directory existant de déploiement de batterie de serveurs](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/) à nouveau le modèle, mais cette fois apporter les modifications suivantes. (Pour personnaliser le modèle, sélectionnez-le dans la galerie, cliquez sur **déployer sur Azure** , puis **modifier le modèle**.)
      1. Ajuster l’espace d’adressage de l’IP privée du serveur DNS pour qu’elles correspondent au réseau virtuel dans RG B. 
      
         Recherchez « dnsServerPrivateIp » dans des variables. Modifier l’adresse IP par défaut (10.0.0.4) correspondant à l’espace d’adressage que vous avez défini dans le réseau virtuel dans RG B.
   
      2. Modifier les noms d’ordinateurs afin qu’ils ne sont en conflit avec ceux du déploiement dans RG A.
      
         Recherchez les machines virtuelles dans le **ressources** section du modèle. Modifier le **computerName** champ sous **osProfile**. Par exemple, « passerelle » peut devenir « passerelle **-b**» ; « [concat ('rdsh-', copyIndex())] » peut devenir « [concat ('rdsh - b-', copyIndex())] » et « broker » peut devenir « broker **-b**».
      
         (Vous pouvez également modifier les noms des machines virtuelles manuellement après avoir exécuté le modèle.)
   2. Comme à l’étape 3 ci-dessus, utilisez les informations de [des Services Bureau à distance - haute disponibilité](rds-plan-high-availability.md) pour configurer les autres composants de services Bureau à distance pour la haute disponibilité.
8. Un serveur de fichiers de montée en puissance espaces de stockage Direct avec le réplica de stockage entre les deux déploiements. Utilisez le [script PowerShell](https://github.com/robotechredmond/301-s2d-sr-dr-md/tree/master/scripts) pour déployer le [modèle](https://github.com/robotechredmond/301-s2d-sr-dr-md) entre les groupes de ressources.

   > [!NOTE]
   > Vous pouvez configurer le stockage manuellement (au lieu d’utiliser le script PowerShell et le modèle) : 
   >1. Déployer un [SOFS directe des espaces de stockage deux nœuds](rds-storage-spaces-direct-deployment.md) dans le groupe de ressources A pour stocker vos disques de profil utilisateur (UPD).
   >2. Déploiement d’un deuxième, identiques stockage sofs avec espaces Direct dans RG B : veillez à utiliser la même quantité de stockage dans chaque cluster.
   >3. Configurer [le réplica de stockage avec la réplication asynchrone](../../storage/storage-replica/cluster-to-cluster-storage-replication.md) entre les deux.

### <a name="enable-upds"></a>Activer les UPD
Le réplica de stockage réplique des données à partir d’un volume source (associé avec le déploiement principal/actif) dans un volume de destination (associé avec le déploiement de la base de données secondaire/passif). Par conception, le cluster de destination s’affiche en tant que **en ligne (aucun accès)** -réplica de stockage démonte les volumes de destination et leurs points de montage ou lettres de lecteur. Cela signifie que l’activation de disques de profil utilisateur pour le déploiement secondaire en fournissant le chemin de partage de fichiers échouera, car le volume n’est pas monté. 

Vous souhaitez en savoir plus sur la gestion de la réplication ? Découvrez [Cluster à cluster de réplication du stockage](../../storage/storage-replica/cluster-to-cluster-storage-replication.md).

Pour activer les UPD sur les deux déploiements, procédez comme suit :

1. Exécutez le [applet de commande Set-RDSessionCollectionConfiguration](https://docs.microsoft.com/powershell/module/remotedesktop/set-rdsessioncollectionconfiguration) pour activer les disques de profil utilisateur pour le déploiement (actif) principal - fournir un chemin d’accès au partage de fichiers sur le volume source (que vous avez créé à l’étape 7 dans les étapes de déploiement).
2. Inverser la direction de réplica de stockage afin que le volume de destination devient le volume source (cela monte le volume et le rend accessible par le déploiement secondaire). Vous pouvez exécuter **Set-SRPartnership** applet de commande pour ce faire. Exemple :

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-b-s2d-c" -SourceRGName "cluster-b-s2d-c" -DestinationComputerName "cluster-a-s2d-c" -DestinationRGName "cluster-a-s2d-c"
   ```
3. Activer les disques de profil utilisateur dans le déploiement secondaire (passif). Utilisez les mêmes étapes comme vous l’avez fait pour le déploiement principal, à l’étape 1.
4. Inverser le sens de réplica de stockage à nouveau, par conséquent, le volume source d’origine est à nouveau le volume source dans le partenariat SR et le déploiement principal peut accéder au partage de fichier. Exemple :

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-a-s2d-c" -SourceRGName "cluster-a-s2d-c" -DestinationComputerName "cluster-b-s2d-c" -DestinationRGName "cluster-b-s2d-c"
   ```


### <a name="azure-traffic-manager"></a>Azure Traffic Manager 

Créer un [Azure Traffic Manager](/azure/traffic-manager/traffic-manager-overview) Profiler et veillez à sélectionner le **priorité** méthode de routage. Définissez deux points de terminaison sur les adresses IP publiques de chaque déploiement. Sous **Configuration**, remplacez le protocole HTTPS (au lieu de HTTP) et le port 443 (au lieu de 80). Prenez note de la **durée de vie DNS**et le configurer comme il convient pour les besoins de votre basculement. 

Notez que Traffic Manager nécessite des points de terminaison à retourner 200 OK en réponse à une demande GET pour être marqué comme « sain ». L’objet d’adresse IP publique créée à partir des modèles de services Bureau à distance fonctionnera, mais n’ajoutez pas d’un addenda de chemin d’accès. Au lieu de cela, vous pouvez donner aux utilisateurs finaux l’URL de Traffic Manager avec « / RDWeb » est ajouté, par exemple : ```http://deployment.trafficmanager.net/RDWeb```

En déployant Azure Traffic Manager avec la méthode de routage de priorité, vous empêcher les utilisateurs finaux d’accéder à du déploiement passif alors que le déploiement actif est fonctionnel. Si les utilisateurs finaux accéder au déploiement passif et la direction de réplica de stockage n’a pas été basculée pour le basculement, la connexion de l’utilisateur se bloque que le déploiement tente et ne parvient pas à accéder au partage de fichiers sur le cluster d’espaces de stockage Direct passif - finalement le déploiement sera renoncer et donner à l’utilisateur un profil temporaire.  

### <a name="deallocate-vms-to-save-resources"></a>Libérer les machines virtuelles pour économiser les ressources 
Après avoir configuré les deux déploiements, vous pouvez éventuellement arrêter et libérer de l’infrastructure de services Bureau à distance et des machines virtuelles RDSH pour réduire les coûts sur ces machines virtuelles secondaire. Le machines virtuelles du serveur SOFS directe des espaces de stockage et AD doive rester toujours en cours d’exécution dans le déploiement de base de données secondaire/passif pour activer la synchronisation de compte et profil utilisateur.  

En cas de basculement, vous devez démarrer les machines virtuelles libérées. Cette configuration de déploiement présente l’avantage d’être un coût inférieur, mais au détriment du temps de basculement. Si une défaillance irrémédiable se produit dans le déploiement actif, vous devrez démarrer manuellement le déploiement passif, ou vous aurez besoin d’un script d’automatisation pour détecter la défaillance et de démarrer le déploiement passif automatiquement. Dans les deux cas, il peut prendre plusieurs minutes pour obtenir le déploiement passif en cours d’exécution et disponible pour les utilisateurs à se connecter, ce qui entraîne un temps d’arrêt pour le service. Ce temps d’arrêt dépend de la quantité de temps il prend pour démarrer l’infrastructure des services Bureau à distance et RDSH machines virtuelles (généralement 2 à 4 minutes, si les machines virtuelles sont démarrées en parallèle plutôt qu’en série) et le temps de mettre le cluster passif en ligne (qui dépend de la taille du cluster généralement 2 à 4 minutes pour un cluster à 2 nœuds avec 2 disques par nœud). 

### <a name="active-directory"></a>Active Directory 
Les serveurs Active Directory dans chaque déploiement sont des réplicas dans le même domaine/forêt. Active Directory a un protocole de synchronisation incorporée pour synchroniser les contrôleurs de domaine de quatre. Toutefois, il peut y avoir certains décalage afin que si un nouvel utilisateur est ajouté à un seul serveur AD, il peut prendre un certain temps pour répliquer sur tous les serveurs AD dans les deux déploiements. Par conséquent, veillez à avertir les utilisateurs à ne pas essayer de se connecter immédiatement après leur ajout au domaine. 

### <a name="rd-license-server"></a>Serveur de licences des services Bureau à distance 
Fournir un [CAL de bureau à distance par utilisateur](rds-client-access-license.md) pour chaque utilisateur nommé qui est autorisé à accéder au déploiement géo-redondant. Distribuer le par utilisateur CAL uniformément entre les deux serveurs de licences bureau à distance dans le déploiement actif. Puis, le dupliquer ces licences d’accès client pour les deux serveurs de licences bureau à distance dans le déploiement de passif. Étant donné que les licences d’accès client sont dupliquées entre le déploiement actif et passif, à un moment donné, un seul déploiement peut être actif avec les utilisateurs qui se connectent ; Sinon, vous viole le contrat de licence.  

### <a name="image-management"></a>Gestion des images 
Quand vous mettez à jour vos images RDSH pour fournir des mises à jour logicielles ou nouvelles applications, vous devez mettre à jour séparément les collections RDSH dans chaque déploiement de maintenir une expérience utilisateur commune entre les deux déploiements. Vous pouvez utiliser la [modèle de collection de mise à jour RDSH](https://azure.microsoft.com/resources/templates/rds-update-rdsh-collection/), mais notez que le déploiement passif infrastructure RDS et machines virtuelles RDSH doivent être en cours d’exécution pour exécuter le modèle. 

## <a name="failover"></a>Basculement

Dans le cas le déploiement actif / passif, basculement, vous devez démarrer les machines virtuelles du déploiement secondaire. Procéder manuellement ou avec un script d’automatisation. En cas de basculement catastrophique de le sofs avec Direct des espaces de stockage, modifier la direction de partenariat de réplica de stockage, afin que le volume de destination devient le volume source. Exemple :

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-b-s2d-c" -SourceRGName "cluster-b-s2d-c" -DestinationComputerName "cluster-a-s2d-c" -DestinationRGName "cluster-a-s2d-c"
   ```

Vous pouvez en savoir plus, consultez [Cluster à cluster de réplication du stockage](../../storage/storage-replica/cluster-to-cluster-storage-replication.md).

Azure Traffic Manager reconnaît automatiquement que le déploiement principal a échoué et que le déploiement secondaire est sain (dans les machines virtuelles de passerelle Bureau à distance ont été démarrées dans RG B) et dirige le trafic utilisateur vers le déploiement secondaire. Utilisateurs peuvent utiliser la même URL de Traffic Manager pour continuer à travailler sur leurs ressources distantes, bénéficiant d’une expérience cohérente. Notez que le cache DNS client ne met pas à jour l’enregistrement pour la durée de la durée de vie définie dans la configuration d’Azure Traffic Manager.

### <a name="test-failover"></a>Test de basculement
Dans un partenariat de réplica de stockage, qu’un seul volume (source) peut être actif à la fois. Cela signifie que lorsque vous changez la direction SR partenariat, le volume dans le déploiement principal (RG A) devient la destination de réplication et est donc masqué. Par conséquent, les utilisateurs qui se connectent à RG A n’auront plus accès à leurs disques de profil utilisateur stockés sur le serveur SOFS dans RG A. 

Pour tester le basculement tout en permettant aux utilisateurs de poursuivre la connexion :
1. Démarrer les machines virtuelles d’infrastructure et les machines virtuelles RDSH dans RG B.
2. Basculer la direction du partenariat de SR (cluster-b-s2d-c devient le volume source).
3. [Désactiver le point de terminaison](/azure/traffic-manager/traffic-manager-manage-endpoints#to-disable-an-endpoint) de groupe de ressources A dans le profil Azure Traffic Manager pour forcer l’ATM pour diriger le trafic vers RG B. vous pouvez également cliquer, utiliser un script PowerShell :

   ```powershell
   Disable-AzureRmTrafficManagerEndpoint -Name publicIpA -Type AzureEndpoints -ProfileName MyTrafficManagerProfile -ResourceGroupName RGA -Force
   ```

RG B est désormais le déploiement principal actif. Pour revenir à RG A en tant que déploiement principal :

1. Basculer la direction du partenariat de SR (cluster a s2d c devient le volume source) :

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-a-s2d-c" -SourceRGName "cluster-a-s2d-c" -DestinationComputerName "cluster-b-s2d-c" -DestinationRGName "cluster-b-s2d-c"
   ```
2. Réactiver le point de terminaison de RG A dans le profil Azure Traffic Manager :

   ```powershell
   Enable-AzureRmTrafficManagerEndpoint -Name publicIpA -Type AzureEndpoints -ProfileName MyTrafficManagerProfile -ResourceGroupName RGA 
   ```

## <a name="considerations-for-on-premises-deployments"></a>Considérations pour les déploiements locaux

Un déploiement local n’a pas pu utiliser les modèles de démarrage rapide Azure référencés dans cet article, vous pouvez implémenter manuellement tous les rôles d’infrastructure. Dans un déploiement sur site où le coût n’est pas déterminé par la consommation Azure, envisagez d’utiliser un modèle actif / actif pour le basculement plus rapide.

Vous pouvez utiliser Azure Traffic Manager avec les points de terminaison en local, mais il nécessite un abonnement Azure. Vous pouvez également, pour le DNS fourni aux utilisateurs finaux, leur donner un enregistrement CNAME qui dirige simplement les utilisateurs vers le déploiement principal. En cas de basculement, modifiez l’enregistrement CNAME DNS pour rediriger vers le déploiement secondaire. De cette façon, l’utilisateur final utilise une URL unique, tout comme avec Azure Traffic Manager, qui dirige l’utilisateur vers le déploiement approprié. 

Si vous êtes intéressé de la création d’un modèle sur-site-à-site Azure, envisagez d’utiliser [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview).
