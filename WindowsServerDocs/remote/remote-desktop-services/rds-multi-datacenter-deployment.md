---
title: Centres de données RDS géo-redondants dans Azure
description: Découvrez comment créer un déploiement RDS utilisant plusieurs centres de données pour fournir une haute disponibilité dans différents emplacements géographiques.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 61c36528-cf47-4af0-83c1-a883f79a73a5
author: haley-rowland
ms.author: elizapo
ms.date: 06/14/2017
manager: dongill
ms.openlocfilehash: 5c0f5d6937a79f36df264597400fe71af3f3779b
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80855592"
---
# <a name="create-a-geo-redundant-multi-data-center-rds-deployment-for-disaster-recovery"></a>Créer un déploiement RDS avec plusieurs centres de données géo-redondants pour la récupération d’urgence

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Vous pouvez activer la récupération d’urgence pour votre déploiement des services Bureau à distance en exploitant plusieurs centres de données dans Azure. Contrairement à un déploiement RDS standard à haute disponibilité (comme indiqué dans l’[architecture des services Bureau à distance](desktop-hosting-logical-architecture.md)), qui utilise des centres de données dans une seule région Azure (par exemple, l’Europe de l’ouest), un déploiement de plusieurs centres de données utilise des centres de données sur plusieurs emplacements géographiques, accroissant ainsi la disponibilité de votre déploiement : un centre de données Azure peut ne pas être disponible, mais il est peu probable que plusieurs régions soient affectées en même temps. En déployant une architecture RDS géo-redondante, vous pouvez activer le basculement en cas de défaillance catastrophique d’une région entière.

Vous pouvez utiliser les instructions ci-dessous pour exploiter les services d’infrastructure Azure et RDS pour proposer des services d’hébergement de postes de travail géo-redondants et des licences d’accès SAL à plusieurs locataires via le programme [Microsoft SPLA (Service Provider License Agreement)](https://www.microsoft.com/hosting/licensing/splabenefits.aspx). Vous pouvez également utiliser les étapes ci-dessous pour créer un service d’hébergement géo-redondant pour vos employés à l’aide des [droits étendus des CAL utilisateur RDS via Software Assurance](https://download.microsoft.com/download/6/B/A/6BA3215A-C8B5-4AD1-AA8E-6C93606A4CFB/Windows_Server_2012_R2_Remote_Desktop_Services_Licensing_Datasheet.pdf).

## <a name="logical-architecture-for-high-availability---single-and-multiple-regions"></a>Architecture logique pour une haute disponibilité - région unique ou multiples
L’illustration suivante montre l’architecture pour un déploiement à haute disponibilité dans une seule région Azure :

![Un déploiement à haute disponibilité dans une seule région Azure](media/rds-ha-single-region.png)

Le déploiement se compose de trois couches :

- Services Azure - les interfaces de gestion Azure, y compris le portail Azure, les API et les services de mise en réseau public, tels que le DNS et l’adressage IP public.
- Service d’hébergement de poste de travail - les machines virtuelles, les réseaux, le stockage, les services Azure et les services de rôle Windows Server
- Azure Fabric - les systèmes d’exploitation Windows qui exécutent le rôle Hyper-V, utilisé pour virtualiser des serveurs physiques, des unités de stockage, des commutateurs réseau et des routeurs. L’utilisation d’Azure Fabric vous permet de créer des machines virtuelles, des réseaux, du stockage et des applications indépendants du matériel sous-jacent.


En comparaison, voici l’architecture d’un déploiement utilisant plusieurs centres de données Azure :

![un déploiement RDS qui utilise plusieurs régions Azure](media/rds-ha-multi-region.png)

Tout le déploiement RDS est répliqué dans une deuxième région Azure pour créer un déploiement géo-redondant. Cette architecture utilise un modèle actif/passif avec l’exécution d’un seul déploiement RDS à la fois. Une connexion de réseau virtuel à réseau virtuel permet à deux environnements de communiquer entre eux. Les déploiements RDS sont basés sur une seule forêt/un seul domaine Active Directory, et les serveurs AD se répliquent entre les deux déploiements, ce qui signifie que les utilisateurs peuvent se connecter à un déploiement en utilisant les mêmes informations d’identification. Les paramètres utilisateur et les données stockées dans des disques de profil utilisateur (UPD) sont stockés sur un serveur de fichiers avec montée en puissance parallèle d’espaces de stockage direct avec un cluster à deux nœuds (SOFS). Un deuxième cluster d’espaces de stockage direct identique est déployé au sein de la deuxième région (passive), et le réplica de stockage est utilisé pour répliquer les profils utilisateur à partir du déploiement actif vers passif. Azure Traffic Manager est utilisé pour diriger automatiquement les utilisateurs finaux vers le déploiement actuellement actif. Du point de vue de l’utilisateur final, ils accèdent au déploiement à l’aide d’une URL unique et ne savent pas quelle région ils utilisent.


Vous *pouvez* créer un déploiement RDS qui n’est pas hautement disponible dans chaque région, mais même si une seule machine virtuelle est redémarrée dans une région, un basculement se produit, ce qui augmente les probabilités de basculement, avec l’impact que cela a sur les performances.

## <a name="deployment-steps"></a>Étapes de déploiement
Créez les ressources suivantes dans Azure pour créer un déploiement RDS avec plusieurs centres de données géo-redondants :

1. Deux groupes de ressources dans deux régions Azure séparées. Par exemple, RG A (le déploiement actif, RG signifie « resource group », groupe de ressources) et RG B (le déploiement passif).
2. Un déploiement Active Directory hautement disponible dans RG A. Vous pouvez utiliser le [nouveau modèle de domaine Active Directory avec 2 contrôleurs de domaine](https://azure.microsoft.com/resources/templates/active-directory-new-domain-ha-2-dc/) pour créer le déploiement.
3. Un déploiement RDS hautement disponible dans RG A. Utilisez le modèle [Déploiement de batterie RDS utilisant un annuaire Active Directory existant](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/) pour créer le déploiement RDS de base, puis suivez les informations contenues de l’article [Services Bureau à distance - Disponibilité élevée](rds-plan-high-availability.md) pour configurer les autres composants RDS pour une haute disponibilité.
4. Un réseau virtuel dans RG B - Vérifiez que vous utilisez un espace d’adressage qui ne chevauche pas le déploiement dans RG A.
5. Une [connexion de réseau virtuel à réseau virtuel](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps) entre les deux groupes de ressources.
6. Deux machines virtuelles AD au sein d’un groupe de disponibilité dans RG B - Assurez-vous que les noms des machines virtuelles sont différents de celles présentes dans RG A. Déployez deux machines virtuelles Windows Server 2016 dans un groupe de disponibilité unique, installez le rôle AD DS, promouvez-les au contrôleur de domaine au sein du domaine créé à l’étape 1.
7. Un deuxième déploiement RDS à haute disponibilité dans RG B. 
   1. Utilisez à nouveau le modèle [Déploiement de batterie RDS à l’aide d’un annuaire Active Directory existant](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/), en y apportant les modifications suivantes. (Pour personnaliser le modèle, sélectionnez-le dans la galerie, cliquez sur **Déployer sur Azure** puis sur **Modifier le modèle**.)
      1. Ajuster l’espace d’adressage de l’adresse IP privée du serveur DNS pour qu’il corresponde au réseau virtuel dans R GB. 
      
         Recherchez « dnsServerPrivateIp » dans des variables. Modifier l’adresse IP par défaut (10.0.0.4) pour qu’elle corresponde à l’espace d’adressage défini dans le réseau virtuel dans RG B.
   
      2. Modifier les noms d’ordinateurs afin qu’ils ne soient pas en conflit avec ceux du déploiement dans RG A.
      
         Recherchez les machines virtuelles dans la section **Ressources** du modèle. Modifier le champ **computerName** sous **osProfile**. Par exemple, « gateway » peut devenir « gateway **-b**» ; « [concat ('rdsh-', copyIndex())] » peut devenir « [concat ('rdsh - b-', copyIndex())] » et « broker » peut devenir « broker **-b** ».
      
         (Vous pouvez également modifier les noms des machines virtuelles manuellement après avoir exécuté le modèle.)
   2. Comme à l’étape 3 ci-dessus, utilisez les informations dans [Services Bureau à distance - Haute disponibilité](rds-plan-high-availability.md) afin de configurer les autres composants RDS pour une haute disponibilité.
8. Un serveur de fichiers avec montée en puissance d’espaces de stockage direct avec le réplica de stockage entre les deux déploiements. Utilisez le [script PowerShell](https://github.com/robotechredmond/301-s2d-sr-dr-md/tree/master/scripts) pour déployer le [modèle](https://github.com/robotechredmond/301-s2d-sr-dr-md) entre les groupes de ressources.

   > [!NOTE]
   > Vous pouvez configurer le stockage manuellement (au lieu d’utiliser le script et le modèle PowerShell) : 
   >1. Déployer un [SOFS d’espaces de stockage direct à deux nœuds](rds-storage-spaces-direct-deployment.md) dans RG A pour stocker vos disques de profil utilisateur (UPD).
   >2. Déployez un SOFS d’espaces de stockage direct identique dans RG B. Veillez à utiliser la même quantité de stockage dans chaque cluster.
   >3. Configurer le [réplica de stockage avec la réplication asynchrone](../../storage/storage-replica/cluster-to-cluster-storage-replication.md) entre les deux.

### <a name="enable-upds"></a>Activer des UPD
Le réplica de stockage réplique des données à partir d’un volume source (associé avec le déploiement principal/actif) vers un volume de destination (associé avec le déploiement secondaire/passif). Par conception, le cluster de destination s’affiche comme **En ligne (aucun accès)** : le réplica de stockage démonte les volumes de destination et leurs points de montage ou lettres de lecteur. Cela signifie que l’activation des UPD pour le déploiement secondaire en fournissant le chemin de partage de fichiers échouera, car le volume n’est pas monté. 

Vous souhaitez en savoir plus sur la gestion de la réplication ? Consultez [Réplication du stockage de cluster à cluster](../../storage/storage-replica/cluster-to-cluster-storage-replication.md).

Pour activer les UPD sur les deux déploiements, procédez comme suit :

1. Exécutez [l’applet de commande Set-RDSessionCollectionConfiguration](https://docs.microsoft.com/powershell/module/remotedesktop/set-rdsessioncollectionconfiguration) pour activer les disques de profil utilisateur pour le déploiement principal  (actif). Fournissez un chemin d’accès au partage de fichiers sur le volume source (créé à l’étape 7 dans les étapes de déploiement).
2. Inverser le sens du réplica de stockage afin que le volume de destination devienne le volume source (le volume est monté et rendu accessible par le déploiement secondaire). Vous pouvez exécuter l’applet de commande **Set-SRPartnership** pour le faire. Par exemple :

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-b-s2d-c" -SourceRGName "cluster-b-s2d-c" -DestinationComputerName "cluster-a-s2d-c" -DestinationRGName "cluster-a-s2d-c"
   ```
3. Activez les disques de profil utilisateur dans le déploiement secondaire (passif). Utilisez les mêmes étapes que celles prévues pour le déploiement principal, à l’étape 1.
4. Inverser de nouveau le sens du réplica de stockage, le volume source d’origine redevient le volume source dans le partenariat SR et le déploiement principal peut accéder au partage de fichiers. Par exemple :

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-a-s2d-c" -SourceRGName "cluster-a-s2d-c" -DestinationComputerName "cluster-b-s2d-c" -DestinationRGName "cluster-b-s2d-c"
   ```


### <a name="azure-traffic-manager"></a>Azure Traffic Manager 

Créez un profil [Azure Traffic Manager](/azure/traffic-manager/traffic-manager-overview) et veillez à sélectionner la méthode de routage **Priorité**. Définissez les deux points de terminaison sur les adresses IP publiques de chaque déploiement. Sous **Configuration**, passez au protocole HTTPS (au lieu de HTTP) et utilisez le port 443 (au lieu de 80). Prenez note de la **durée de vie DNS**et configurez comme il convient pour les besoins de votre basculement. 

Notez que Traffic Manager exige que les points de terminaison retournent 200 « OK » en réponse à une demande GET pour être marqués comme « sain ». L’objet publicIP créé à partir des modèles RDS fonctionnera, mais n’ajoutez pas d’addenda de chemin d’accès. Au lieu de cela, vous pouvez donner aux utilisateurs finaux l’URL de Traffic Manager avec « /RDWeb » à sa suite, par exemple : ```http://deployment.trafficmanager.net/RDWeb```

En déployant Azure Traffic Manager avec la méthode de routage par priorité, vous empêchez les utilisateurs finaux d’accéder au déploiement passif alors que le déploiement actif fonctionne. Si les utilisateurs finaux accèdent au déploiement passif et que le sens du réplica de stockage n’a pas été modifié pour le basculement, la connexion de l’utilisateur se bloque car le déploiement tente et ne parvient pas à accéder au partage de fichiers sur le cluster d’espaces de stockage direct passif. Le déploiement échoue finalement et donne à l’utilisateur un profil temporaire.  

### <a name="deallocate-vms-to-save-resources"></a>Libérer des machines virtuelles pour économiser les ressources 
Après avoir configuré les deux déploiements, vous pouvez arrêter et libérer les machines virtuelles RDSH et de l’infrastructure RDS secondaire pour réduire les coûts sur ces machines virtuelles. Les machines virtuelles du serveur AD et SOFS des espaces de stockage direct doivent toujours rester en exécution dans le déploiement passif/secondaire pour activer la synchronisation du compte et du profil utilisateur.  

En cas de basculement, vous devez démarrer les machines virtuelles libérées. Cette configuration de déploiement présente l’avantage d’être peu coûteuse, au détriment du temps de basculement. Si une défaillance irrémédiable se produit dans le déploiement actif, vous devrez démarrer manuellement le déploiement passif, ou vous aurez besoin d’un script d’automatisation pour détecter la défaillance et démarrer automatiquement le déploiement passif. Dans les deux cas, plusieurs minutes peuvent être nécessaires pour que le déploiement passif s’exécute et soit disponible pour que les utilisateurs se connectent, ce qui entraîne un temps d’arrêt pour le service. Ce temps d’arrêt dépend du temps nécessaire pour démarrer l’infrastructure RDS et les machines virtuelles RDSH (généralement entre 2 et 4 minutes, si les machines virtuelles sont démarrées en parallèle plutôt qu’en série) et le temps de mettre le cluster passif en ligne (qui dépend de la taille du cluster, généralement 2 à 4 minutes pour un cluster à 2 nœuds avec 2 disques par nœud). 

### <a name="active-directory"></a>Active Directory 
Les serveurs Active Directory dans chaque déploiement sont des réplicas dans le même domaine/la même forêt. Active Directory possède un protocole de synchronisation incorporée pour synchroniser les quatre contrôleurs de domaine. Toutefois, il peut y avoir un décalage si un nouvel utilisateur est ajouté à un seul serveur AD, du temps peut être nécessaire pour le répliquer sur tous les serveurs AD dans les deux déploiements. Par conséquent, avertissez les utilisateurs de ne pas essayer de se connecter immédiatement après leur ajout au domaine. 

### <a name="rd-license-server"></a>Serveur de licence RD 
Fournissez un [CAL RD par utilisateur](rds-client-access-license.md) pour chaque utilisateur nommé et autorisé à accéder au déploiement géo-redondant. Distribuez uniformément les CAL par utilisateur entre les deux serveurs de licences Bureau à distance dans le déploiement actif. Ensuite, dupliquez ces CAL pour les deux serveurs de licences Bureau à distance dans le déploiement de passif. Étant donné que les CAL sont dupliquées entre les déploiements actif et passif, à n’importe quel moment, un seul déploiement peut être actif avec les utilisateurs qui se connectent, ou vous violez le contrat de licence.  

### <a name="image-management"></a>Gestion d’image 
Quand vous mettez à jour vos images RDSH pour fournir des mises à jour logicielles ou de nouvelles applications, vous devez mettre à jour séparément les collections RDSH dans chaque déploiement afin de maintenir une expérience utilisateur commune entre les deux déploiements. Vous pouvez utiliser le [modèle Mise à jour de collection RDSH](https://azure.microsoft.com/resources/templates/rds-update-rdsh-collection/), mais notez que l’infrastructure RDS et les machines virtuelles RDSH du déploiement passif doivent être en cours d’exécution pour exécuter le modèle. 

## <a name="failover"></a>Basculement

Dans le cas du déploiement actif / passif, un basculement vous demande de démarrer les machines virtuelles du déploiement secondaire. Procéder manuellement ou avec un script d’automatisation. En cas de basculement catastrophique du SOFS d’espaces de stockage direct, modifiez le sens du partenariat du réplica de stockage, afin que le volume de destination devienne le volume source. Par exemple :

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-b-s2d-c" -SourceRGName "cluster-b-s2d-c" -DestinationComputerName "cluster-a-s2d-c" -DestinationRGName "cluster-a-s2d-c"
   ```

Vous pouvez en savoir plus avec l’article [Réplication du stockage de cluster à cluster](../../storage/storage-replica/cluster-to-cluster-storage-replication.md).

Azure Traffic Manager reconnaît automatiquement que le déploiement principal a échoué et que le déploiement secondaire est sain (dans la passerelle RD, les machines virtuelles ont été démarrées dans RG B) et dirige le trafic utilisateur vers le déploiement secondaire. Les utilisateurs peuvent utiliser la même URL de Traffic Manager pour continuer à travailler sur leurs ressources distantes, bénéficiant d’une expérience cohérente. Notez que le cache DNS client ne met pas à jour l’enregistrement pour la durée de vie définie dans la configuration d’Azure Traffic Manager.

### <a name="test-failover"></a>Test de basculement
Dans un partenariat de réplica de stockage, un seul volume (source) peut être actif à la fois. Cela signifie que lorsque vous changez le sens du partenariat du réplica de stockage, le volume dans le déploiement principal (RG A) devient la destination de réplication et est donc masqué. Par conséquent, les utilisateurs qui se connectent à RG A n’auront plus accès à leurs disques de profil utilisateur stockés sur le serveur SOFS dans RG A. 

Pour tester le basculement tout en permettant aux utilisateurs de se connecter :
1. Démarrer les machines virtuelles d’infrastructure et les machines virtuelles RDSH dans RG B.
2. Basculer le sens du partenariat du réplica de stockage (cluster-b-s2d-c devient le volume source).
3. [Désactivez le point de terminaison](/azure/traffic-manager/traffic-manager-manage-endpoints#to-disable-an-endpoint) de RG A dans le profil Azure Traffic Manager pour forcer l’ATM à diriger le trafic vers RG B. Vous pouvez également utiliser un script PowerShell :

   ```powershell
   Disable-AzureRmTrafficManagerEndpoint -Name publicIpA -Type AzureEndpoints -ProfileName MyTrafficManagerProfile -ResourceGroupName RGA -Force
   ```

RG B est désormais le déploiement principal actif. Pour redéfinir RG A comme déploiement principal :

1. Basculer le sens du partenariat du réplica de stockage (cluster a s2d c devient le volume source) :

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-a-s2d-c" -SourceRGName "cluster-a-s2d-c" -DestinationComputerName "cluster-b-s2d-c" -DestinationRGName "cluster-b-s2d-c"
   ```
2. Réactiver le point de terminaison de RG A dans le profil Azure Traffic Manager :

   ```powershell
   Enable-AzureRmTrafficManagerEndpoint -Name publicIpA -Type AzureEndpoints -ProfileName MyTrafficManagerProfile -ResourceGroupName RGA 
   ```

## <a name="considerations-for-on-premises-deployments"></a>Considérations pour les déploiements locaux

Tandis qu’un déploiement local n’a pas pu utiliser les modèles de démarrage rapide Azure référencés dans cet article, vous pouvez implémenter manuellement tous les rôles d’infrastructure. Dans un déploiement sur site où le coût n’est pas déterminé par la consommation Azure, envisagez d’utiliser un modèle actif/actif pour le basculement plus rapide.

Vous pouvez utiliser Azure Traffic Manager avec des points de terminaison locaux, mais cela nécessite un abonnement Azure. Vous pouvez également, pour le DNS fourni aux utilisateurs finaux, leur donner un enregistrement CNAME qui dirige simplement les utilisateurs vers le déploiement principal. En cas de basculement, modifiez l’enregistrement CNAME du DNS pour rediriger vers le déploiement secondaire. De cette façon, l’utilisateur final utilise une URL unique, comme avec Azure Traffic Manager, qui dirige l’utilisateur vers le déploiement approprié. 

Si vous êtes intéressé par la création d’un modèle local-vers-Azure, envisagez d’utiliser [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview).
