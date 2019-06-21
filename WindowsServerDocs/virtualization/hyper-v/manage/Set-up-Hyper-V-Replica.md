---
title: Configurer le réplica Hyper-V
ms.technology: compute-hyper-v
description: Fournit des instructions pour la configuration réplica, test de basculement et une réplication en premier.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.topic: article
ms.assetid: eea9e996-bfec-4065-b70b-d8f66e7134ac
author: KBDAzure
ms.author: kathydav
ms.date: 10/10/2016
ms.openlocfilehash: b8dcf23946d99509aafba0f8af58bf633bedd069
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280235"
---
# <a name="set-up-hyper-v-replica"></a>Configurer le réplica Hyper-V

>S'applique à : Windows Server 2016

La réplication Hyper-V fait partie intégrante du rôle Hyper-V. Il contribue à votre stratégie de récupération d’urgence en répliquant des machines virtuelles à partir d’un serveur hôte Hyper-V à un autre pour conserver vos charges de travail disponibles.  Réplica Hyper-V crée une copie d’une machine virtuelle en direct à un ordinateur virtuel hors connexion. Notez les points suivants :  

-   **Hôtes Hyper-V**: Serveurs hôtes principaux et secondaires peuvent être physiquement colocalisés ou dans des emplacements géographiques distincts avec la réplication via un WAN lien. Hôtes Hyper-V peuvent être autonome, en cluster, ou un mélange des deux. Il n’existe aucune dépendance de Active Directory entre les serveurs, et ils ne doivent être membres du domaine.  

-   **Réplication et le suivi des modifications**: Lorsque vous activez la réplication Hyper-V pour un ordinateur virtuel spécifique, la réplication initiale crée une machine virtuelle de réplica identiques sur un serveur hôte secondaire. Une fois que cela se produit, le suivi des modifications de réplica Hyper-V crée et gère un fichier journal qui capture les modifications sur un disque dur virtuel de machine virtuelle. Le fichier journal est lu dans l’ordre inverse pour le réplica sur disque dur virtuel en fonction des paramètres de fréquence de réplication. Cela signifie que les modifications les plus récentes sont stockées et répliquées de manière asynchrone. La réplication peut être via HTTP ou HTTPS.  

-   **Étendue de réplication (chaînée)** : Cela vous permet de répliquer une machine virtuelle à partir d’un hôte principal vers un hôte secondaire, puis de répliquer la base de données secondaire héberger pour héberger une troisième. Notez que vous ne pouvez pas répliquer à partir de l’hôte principal directement à la deuxième et la troisième.  

    Cette fonctionnalité rend le réplica Hyper-V plus robuste pour la récupération d’urgence, car si une panne se produit, vous pouvez récupérer à partir du réplica principal et étendu.  Vous pouvez basculer sur le réplica étendu si vos emplacements principaux et secondaires tombent en panne. Notez que le réplica étendu ne prend pas en charge la réplication cohérente avec les applications et qu’il doit utiliser les disques durs virtuels mêmes qui utilise le réplica secondaire.  

-   **Basculement**: Si une panne se produit dans votre serveur principal (ou l’étendue de base de données secondaire en cas de) emplacement, vous pouvez lancer manuellement un test, planifié ou le basculement non planifié.  

    ||Tester|Planifié|Non planifié|  
    |-|--------|-----------|-------------|  
    |Quand dois-je exécuter ?|Vérifiez qu’une machine virtuelle peut basculer et démarrer dans le site secondaire<br /><br />Utile pour l’apprentissage et de test|Pendant les pannes et les temps d’arrêt planifié|Lors d’événements inattendus|  
    |Une machine virtuelle en double est créée ?|Oui|Non|Non|  
    |Où est lancée ?|Sur la machine virtuelle de réplica|Lancée sur l’ordinateur principal, puis se termine sur l’ordinateur secondaire|Sur la machine virtuelle de réplica|  
    |À quelle fréquence dois-je exécuter ?|Nous vous recommandons une fois par mois pour le test|Une fois tous les six mois ou conformément aux exigences de conformité|Uniquement en cas de sinistre, lorsque l’ordinateur virtuel principal n’est pas disponible|  
    |La machine virtuelle principale continuent d’être répliqués ?|Oui|Oui. Une fois la panne résoudre réplique de la réplication inverse les modifications sur le site principal afin que les principaux et secondaires sont synchronisées.|Non|  
    |Existe-t-il une perte de données ?|Aucune|Aucun. Après le basculement réplica Hyper-V est répliquée sur le dernier jeu de suivi des modifications vers le réplica principal pour vous assurer de ne perdre aucune donnée.|Dépend de l’événement et points de récupération|  
    |Y a-t-il des temps d’arrêt ?|Aucun. Il n’affecte pas votre environnement de production. Il crée une machine virtuelle de test en double pendant le basculement. Après basculement vous sélectionnez **basculement** sur le réplica de machine virtuelle et il a automatiquement nettoyé et supprimé.|La durée de la panne planifiée|La durée de la panne non planifiée|  

-   **Points de récupération**: Lorsque vous configurez les paramètres de réplication pour une machine virtuelle, vous spécifiez les points de récupération que vous souhaitez stocker à partir de celui-ci. Points de récupération représentent un instantané dans le temps à partir de laquelle vous pouvez récupérer une machine virtuelle. Évidemment, moins de données seront perdues si vous récupérez à partir d’un point de récupération très récente. Vous pouvez accéder à des points de récupération remonte jusqu'à 24 heures.  

## <a name="deployment-prerequisites"></a>Configuration requise pour le déploiement  
Voici ce que vous devez vérifier avant de commencer :  

-   **Déterminer quels disques durs virtuels devoir être répliqués**. En particulier, disques durs virtuels qui contiennent des données qui changent rapidement et pas utilisé par le serveur de réplication après que basculement, tels que des disques de fichier d’échange, doit être exclu de la réplication pour économiser la bande passante réseau. Prenez note des disques durs virtuels peuvent être exclus.  

-   **Déterminer la fréquence à laquelle il est nécessaire synchroniser les données**:  Les données sur le serveur de réplica sont synchronisées mis à jour en fonction de la fréquence de réplication que vous configurez (30 secondes, 5 minutes ou 15 minutes). La fréquence choisie doit prendre en compte les éléments suivants : Les machines virtuelles exécutent des données critiques avec un RPO faible ? Ce que vous êtes considérations relatives à la bande passante ?  Vos machines virtuelles hautement critiques devez évidemment une réplication plus fréquente.  

-   **Décider comment récupérer des données**:  Par défaut, le réplica Hyper-V stocke uniquement un point de récupération unique qui sera la dernière réplication envoyée depuis le serveur principal vers le serveur secondaire. Toutefois si vous souhaitez que l’option récupérer les données à un point antérieur dans le temps vous pouvez spécifier que les points de récupération supplémentaires doivent être stockées (avec un maximum de 24 points toutes les heures). Si vous avez besoin de points de récupération supplémentaires, vous devez noter que cela nécessite davantage de ressources stockage et le traitement sur une surcharge.  

-   **Déterminer quelles charges de travail que vous allez répliquer les**: Réplication Hyper-V Replica standard maintient la cohérence dans l’état du système d’exploitation machine virtuelle après un basculement, mais pas l’état des applications qui s’exécute sur l’ordinateur virtuel. Si vous souhaitez être en mesure de pointe de l’état de votre charge de travail, vous pouvez créer de récupération cohérent d’application de récupération. Notez que récupération cohérents d’application n’est pas disponible sur le site de réplica étendu si vous utilisez la réplication étendue (chaînée).  

-   **Décider comment effectuer la réplication initiale des données de la machine virtuelle**: La réplication démarre en transférant les besoins pour transférer l’état actuel des machines virtuelles. Cet état initial peut être transmis directement via le réseau existant, soit immédiatement soit à un moment ultérieur que vous configurez. Vous pouvez également utiliser une machine virtuelle restaurée préexistante (par exemple, si vous avez restauré une sauvegarde antérieure de la machine virtuelle sur le serveur réplica) comme copie initiale. Vous pouvez également économiser la bande passante réseau en copiant la copie initiale sur des supports externes, puis en remettant physiquement les supports sur le site de réplication.  Si vous souhaitez utiliser un préexistant machine virtuelle supprimer toutes les captures instantanées associées.  

## <a name="deployment-steps"></a>Étapes de déploiement  

### <a name="step-1-set-up-the-hyper-v-hosts"></a>Étape 1 : Configurer les hôtes Hyper-V  
Vous aurez besoin d’au moins deux hôtes Hyper-V avec un ou plusieurs machines virtuelles sur chaque serveur. [Bien démarrer avec Hyper-V](../get-started/Get-started-with-Hyper-V-on-Windows.md). Le serveur hôte que vous allez répliquer les machines virtuelles devez être configuré comme serveur de réplication.  

1.  Dans les paramètres Hyper-V pour le serveur vous allez répliquer des machines virtuelles au, **Configuration de la réplication**, sélectionnez **activer cet ordinateur comme serveur réplica**.  

2.  Vous pouvez répliquer via HTTP ou HTTPS chiffré. Sélectionnez **utiliser Kerberos (HTTP)** ou **utiliser l’authentification par certificat (HTTPS**). Par défaut, HTTP 80 et HTTPS 443 sont activés en tant qu’exceptions de pare-feu sur le serveur d’Hyper-V réplica. Si vous modifiez les paramètres de port par défaut, vous devez également modifier l’exception de pare-feu. Si vous répliquez sur HTTPS, vous devez sélectionner un certificat et vous devez avoir configuré l’authentification de certificat.  

3.  Pour l’autorisation, sélectionnez **autoriser la réplication à partir de n’importe quel serveur authentifié** pour permettre au serveur de réplica accepter le trafic de réplication de machine virtuelle à partir de n’importe quel serveur principal est correctement authentifié. Sélectionnez **autoriser la réplication des serveurs spécifiés** à accepter le trafic uniquement à partir de serveurs principaux en particulier, vous sélectionnez.  

    Pour les deux options, vous pouvez spécifier où les disques durs virtuels répliquées doivent être stockées sur le serveur d’Hyper-V réplica.  

4.  Cliquez sur **OK** ou **Appliquer**.  

### <a name="step-2-set-up-the-firewall"></a>Étape 2 : Configuration du pare-feu  
Pour autoriser la réplication entre les serveurs primaires et secondaire, le trafic doit obtenir via le pare-feu Windows (ou d’autres pare-feux tiers). Lorsque vous avez installé le rôle Hyper-V sur les serveurs par des exceptions par défaut pour le protocole HTTP (80) et HTTPS (443) sont créés. Si vous utilisez ces ports standards, vous devez simplement activer les règles :  

-  Pour activer les règles sur un serveur hôte autonome :  

    1. Ouvrez les pare-feu Windows avec fonctions avancées de sécurité et cliquez sur **règles de trafic entrant**.  

    2. Pour activer l’authentification HTTP (Kerberos), avec le bouton droit **écouteur HTTP de réplica Hyper-V (TCP-entrant)**  >**activer la règle.** Pour activer l’authentification par certificat HTTPS, avec le bouton droit **écouteur HTTPS de réplica Hyper-V (TCP-entrant)** > E**acti règle**.  

-  Pour activer les règles sur un cluster Hyper-V, ouvrez une session Windows PowerShell avec **exécuter en tant qu’administrateur**, puis exécutez une de ces commandes :  

    -   Pour HTTP :  `get-clusternode | ForEach-Object  {Invoke-command -computername $_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}`  

    -   Pour le protocole HTTPS : `get-clusternode | ForEach-Object  {Invoke-command -computername $_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}`  

### <a name="enable-virtual-machine-replication"></a>Activation de la réplication de machine virtuelle  
Procédez comme suit sur chaque machine virtuelle à répliquer :  

1.  Dans le **détails** volet du Gestionnaire Hyper-V, sélectionnez une machine virtuelle en cliquant dessus.  
    Avec le bouton droit de la machine virtuelle sélectionnée et cliquez sur **activer la réplication** pour ouvrir l’Assistant Activation de la réplication.  

2.  Dans la page **Avant de commencer**, cliquez sur **Suivant**.  

3.  Sur le **spécifier le serveur réplica** page, dans la zone serveur réplica, entrez le nom NetBIOS ou le nom de domaine complet du serveur réplica. Si le serveur de réplication fait partie d’un cluster de basculement, entrez le nom de service Broker de la réplication Hyper-V. Cliquez sur **Suivant**.  

4.  Sur le **spécifier les paramètres de connexion** page, Hyper-V Replica récupère automatiquement les paramètres d’authentification et le port configurés pour le serveur réplica. Si les valeurs ne sont pas en cours récupérées Vérifiez que le serveur est configuré comme serveur réplica, et il est enregistré dans DNS. Si le type dans le paramètre requis manuellement.  

5.  Sur le **choisir les disques durs virtuels de réplication** , vérifiez que les disques durs virtuels que vous souhaitez répliquer sont cochées, puis désactivez les cases à cocher pour les disques durs virtuels que vous souhaitez exclure de la réplication. Ensuite, cliquez sur **Suivant**.  

6.  Sur le **configurer la fréquence de réplication** , spécifiez la fréquence à laquelle modifications doivent être synchronisées du site principal vers le site secondaire. Ensuite, cliquez sur **Suivant**.  

7.  Sur le **configurer les Points de récupération supplémentaires** page, indiquez si vous souhaitez conserver le dernier point de récupération ou pour créer des points supplémentaires.    Si vous souhaitez récupérer régulièrement les applications et charges de travail qui ont leurs propres enregistreurs VSS nous vous recommandons de sélectionner **Volume Shadow Copy Service (VSS) frequenc**y et spécifiez la fréquence de création des instantanés cohérents au niveau de l’application. Notez que le Service demandeur de VMM Hyper-V doit être en cours d’exécution sur les serveurs Hyper-V principaux et secondaires. Ensuite, cliquez sur **Suivant**.  

8.  Sur le **choisir la réplication initiale** , sélectionnez la méthode de réplication initiale à utiliser.  Le paramètre par défaut pour envoyer une copie initiale sur le réseau copiera le fichier de configuration de machine virtuelle principale (VMCX) et les fichiers de disque dur virtuel (VHDX et disque dur virtuel) que vous avez sélectionné sur votre connexion réseau. Vérifiez la disponibilité de la bande passante réseau si vous prévoyez d’utiliser cette option. Si la machine virtuelle principale est déjà configurée sur le site secondaire comme une machine virtuelle de réplication, il peut être utile sélectionner **utiliser un ordinateur virtuel existant sur le serveur de réplication comme copie initiale**. Vous pouvez utiliser Hyper-V exporter pour exporter l’ordinateur virtuel principal et l’importer comme une machine virtuelle de réplica sur le serveur secondaire. Pour les machines virtuelles plus grandes ou une bande passante limitée vous pouvez choisir la réplication initiale sur le réseau à une heure ultérieure, puis configurez les heures creuses, ou pour envoyer les informations de la réplication initiale en tant que média hors connexion.  

    Si vous effectuez une réplication hors connexion, vous allez transport la copie initiale sur le serveur secondaire à l’aide d’un support de stockage externe tel qu’un disque dur ou un lecteur USB. Pour cela de que vous devez connecter l’externe stockage au serveur principal (ou nœud de propriétaire d’un cluster), puis lorsque vous sélectionnez envoient la copie initiale à l’aide de supports externes, que vous pouvez spécifier un emplacement local ou sur un support externe où la copie initiale peut être stockée.  Une machine virtuelle d’espace réservé est créée sur le site de réplication. Une fois terminée la réplication initiale du stockage externe peut être envoyé vers le site de réplication. Il vous vous connectez à un support externe vers le serveur secondaire ou sur le nœud propriétaire du cluster secondaire. Ensuite, vous allez importer le réplica initial vers un emplacement spécifié et fusionner dans la machine virtuelle espace réservé.  

9. Sur le **fin de l’activation de la réplication** page, passez en revue les informations contenues dans le résumé, puis sur **terminer.** . Les données de la machine virtuelle seront transférées selon les paramètres choisis. et une boîte de dialogue s’affiche indiquant que la réplication a été activée avec succès.  

10. Si vous souhaitez configurer la réplication étendue (chaînée), ouvrez le serveur de réplication et avec le bouton droit de la machine virtuelle que vous souhaitez répliquer. Cliquez sur **réplication** > **étendre la réplication** et spécifiez les paramètres de réplication.  

## <a name="run-a-failover"></a>Exécuter un basculement  
Après avoir effectué ces étapes de déploiement votre environnement répliqué est en cours d’exécution. Vous pouvez maintenant exécuter des basculements en fonction des besoins.  

**Test de basculement**:  Si vous souhaitez exécuter un basculement de test clic droit de la machine virtuelle principale et sélectionnez **réplication** > **le Test de basculement**. Choisissez le point de récupération plus récente ou autres si configuré. Une machine virtuelle de test est créée et démarrée sur le site secondaire. Une fois que vous avez terminé le test, sélectionnez **arrêter le Test du basculement** sur la machine virtuelle de réplica pour les nettoyer. Notez que pour une machine virtuelle que vous ne pouvez exécuter un test de basculement à la fois. [En savoir plus](https://blogs.technet.com/b/virtualization/archive/2012/07/26/types-of-failover-operations-in-hyper-v-replica.aspx).  

**Basculement planifié**: Pour exécuter un clic droit basculement planifié de l’ordinateur virtuel principal et sélectionnez **réplication** > **basculement planifié**. Basculement planifié effectue des vérifications de conditions préalables pour vous assurer de ne perdre aucune donnée. Il vérifie que la machine virtuelle principale est arrêtée avant de commencer le basculement. Une fois que l’ordinateur virtuel est basculé, il démarre la réplication des modifications vers le site principal lorsqu’il est disponible. Notez que pour ce faire le serveur principal doit être configuré pour la réplication de réception à partir du serveur secondaire ou dans le service Broker de réplication Hyper-V dans le cas d’un cluster principal. Planifié envoie de basculement le dernier ensemble de modifications. [En savoir plus](https://blogs.technet.com/b/virtualization/archive/2012/07/31/types-of-failover-operations-in-hyper-v-replica-part-ii-planned-failover.aspx).  

**Basculement non planifié**: Pour exécuter un basculement non planifié, avec le bouton droit sur l’ordinateur virtuel réplica, puis sélectionnez **réplication** > **basculement non planifié** depuis le Gestionnaire Hyper-V ou de gestionnaire du Clustering de basculement. Si cette option est activée, vous pouvez récupérer à partir du dernier point de récupération ou à partir de points de récupération précédents. Après le basculement, vérifiez que tout fonctionne comme prévu sur l’échec de la machine virtuelle basculée, puis cliquez sur **Terminer** sur l’ordinateur virtuel réplica. [En savoir plus](https://blogs.technet.com/b/virtualization/archive/2012/08/08/types-of-failover-operations-in-hyper-v-replica-part-iii-unplanned-failover.aspx).  
