---
title: Configurer le réplica Hyper-V
ms.technology: compute-hyper-v
description: Fournit des instructions pour la configuration du réplica, le test de basculement et la création d’une première réplication.
ms.prod: windows-server
manager: dongill
ms.topic: article
ms.assetid: eea9e996-bfec-4065-b70b-d8f66e7134ac
author: kbdazure
ms.author: kathydav
ms.date: 10/10/2016
ms.openlocfilehash: b2730c04e4a4cba4a8c79e600e18bfdba1cfd0c4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859032"
---
# <a name="set-up-hyper-v-replica"></a>Configurer le réplica Hyper-V

>S’applique à Windows Server 2016

La réplication Hyper-V fait partie intégrante du rôle Hyper-V. Elle contribue à votre stratégie de récupération d’urgence en répliquant les machines virtuelles d’un serveur hôte Hyper-V vers un autre pour maintenir les charges de travail disponibles.  Le réplica Hyper-V crée une copie d’un ordinateur virtuel actif sur un ordinateur virtuel hors connexion de réplica. Notez les points suivants :  

-   **Ordinateurs hôtes Hyper-V**: les serveurs hôtes principaux et secondaires peuvent être physiquement colocalisés ou se trouver dans des emplacements géographiques distincts avec la réplication via une liaison WAN. Les hôtes Hyper-V peuvent être autonomes, en cluster ou un mélange des deux. Il n’existe aucune dépendance de Active Directory entre les serveurs et ils n’ont pas besoin d’être membres du domaine.  

-   **Réplication et suivi des modifications**: quand vous activez le réplica Hyper-V pour un ordinateur virtuel spécifique, la réplication initiale crée un ordinateur virtuel de réplication identique sur un serveur hôte secondaire. Après cela, le suivi des modifications de réplica Hyper-V crée et gère un fichier journal qui capture les modifications sur un disque dur virtuel de machine virtuelle. Le fichier journal est lu dans l’ordre inverse du disque dur virtuel de réplication en fonction des paramètres de fréquence de réplication. Cela signifie que les modifications les plus récentes sont stockées et répliquées de façon asynchrone. La réplication peut être sur HTTP ou HTTPs.  

-   **Réplication étendue (chaînée)** : cette fonctionnalité vous permet de répliquer un ordinateur virtuel à partir d’un hôte principal vers un hôte secondaire, puis de répliquer l’hôte secondaire sur un troisième hôte. Notez que vous ne pouvez pas répliquer à partir de l’hôte principal directement vers le deuxième et le troisième.  

    Cette fonctionnalité rend le réplica Hyper-V plus robuste pour la récupération d’urgence, car si une panne se produit, vous pouvez récupérer à partir du réplica principal et du réplica étendu.  Vous pouvez basculer vers le réplica étendu si vos emplacements principaux et secondaires diminuent. Notez que le réplica étendu ne prend pas en charge la réplication cohérente avec les applications et doit utiliser les mêmes disques durs virtuels que ceux utilisés par le réplica secondaire.  

-   **Basculement**: si une panne se produit dans votre emplacement principal (ou secondaire en cas d’extension), vous pouvez lancer manuellement un basculement de test, planifié ou non planifié.  

    ||Test|Planifié|Non prévu|  
    |-|--------|-----------|-------------|  
    |Quand dois-je exécuter ?|Vérifier qu’un ordinateur virtuel peut basculer et démarrer sur le site secondaire<p>Utile pour le test et la formation|Pendant les interruptions planifiées et les pannes|Pendant des événements inattendus|  
    |Un ordinateur virtuel en double a-t-il été créé ?|Oui|Non|Non|  
    |Où est-il initié ?|Sur la machine virtuelle de réplication|Lancée sur l’ordinateur principal, puis se termine sur l’ordinateur secondaire|Sur la machine virtuelle de réplication|  
    |À quelle fréquence dois-je exécuter ?|Nous vous recommandons d’utiliser une fois par mois pour les tests|Une fois tous les six mois ou conformément aux exigences de conformité|Uniquement en cas de sinistre lorsque la machine virtuelle principale n’est pas disponible|  
    |L’ordinateur virtuel principal continue-t-il de répliquer ?|Oui|Oui. Lorsque la panne est résolue, la réplication inverse réplique les modifications sur le site principal afin que les données primaires et secondaires soient synchronisées.|Non|  
    |Y a-t-il une perte de données ?|Aucune|None. Après le basculement, le réplica Hyper-V réplique le dernier ensemble de modifications suivies sur le serveur principal pour garantir une perte de données nulle.|Dépend de l’événement et des points de récupération|  
    |Y a-t-il des temps d’arrêt ?|None. Il n’a aucun impact sur votre environnement de production. Il crée une machine virtuelle de test en double lors du basculement. Une fois le basculement terminé, vous sélectionnez le **basculement** sur la machine virtuelle de réplication et elle est automatiquement nettoyée et supprimée.|Durée de la panne planifiée|Durée de l’interruption non planifiée|  

-   **Points de récupération**: lorsque vous configurez les paramètres de réplication d’une machine virtuelle, vous spécifiez les points de récupération à partir desquels vous souhaitez les stocker. Les points de récupération représentent un instantané dans le temps à partir duquel vous pouvez récupérer un ordinateur virtuel. Évidemment, moins de données sont perdues si vous récupérez à partir d’un point de récupération très récent. Vous pouvez accéder aux points de récupération jusqu’à 24 heures.  

## <a name="deployment-prerequisites"></a>Configuration requise pour le déploiement  
Voici ce que vous devez vérifier avant de commencer :  

-   **Déterminez les disques durs virtuels qui doivent être répliqués**. En particulier, les disques durs virtuels qui contiennent des données qui changent rapidement et qui ne sont pas utilisées par le serveur de réplication après le basculement, comme les disques de fichiers d’échange, doivent être exclus de la réplication pour économiser la bande passante réseau. Notez les disques durs virtuels qui peuvent être exclus.  

-   **Déterminez la fréquence à laquelle vous devez synchroniser les données**: les données sur le serveur de réplication sont synchronisées en fonction de la fréquence de réplication que vous configurez (30 secondes, 5 minutes ou 15 minutes). La fréquence choisie doit prendre en compte les éléments suivants : les machines virtuelles exécutent-elles des données critiques avec un RPO faible ? Quelles sont les considérations relatives à la bande passante ?  Vos machines virtuelles hautement critiques nécessitent évidemment une réplication plus fréquente.  

-   **Décider comment récupérer des données**: par défaut, le réplica Hyper-V stocke uniquement un point de récupération unique qui sera la dernière réplication envoyée du serveur principal au serveur secondaire. Toutefois, si vous souhaitez pouvoir récupérer des données à un point antérieur dans le temps, vous pouvez spécifier que des points de récupération supplémentaires doivent être stockés (jusqu’à un maximum de 24 points). Si vous avez besoin de points de récupération supplémentaires, vous devez noter que cela nécessite une surcharge supplémentaire du traitement et des ressources de stockage.  

-   Déterminer **les charges de travail que vous allez répliquer**: la réplication de réplica Hyper-V standard maintient la cohérence de l’état du système d’exploitation de la machine virtuelle après un basculement, mais pas de l’état des applications qui s’exécutent sur la machine virtuelle. Si vous souhaitez être en mesure de récupérer l’état de votre charge de travail, vous pouvez créer des points de récupération cohérents avec les applications. Notez que la récupération de cohérence des applications n’est pas disponible sur le site de réplication étendu si vous utilisez la réplication étendue (chaînée).  

-   **Décider comment effectuer la réplication initiale des données de l’ordinateur virtuel**: la réplication commence par le transfert des besoins pour transférer l’état actuel des machines virtuelles. Cet état initial peut être transmis directement via le réseau existant, soit immédiatement soit à un moment ultérieur que vous configurez. Vous pouvez également utiliser un ordinateur virtuel restauré existant (par exemple, si vous avez restauré une sauvegarde antérieure de l’ordinateur virtuel sur le serveur de réplication) comme copie initiale. Vous pouvez également économiser la bande passante réseau en copiant la copie initiale sur des supports externes, puis en remettant physiquement les supports sur le site de réplication.  Si vous souhaitez utiliser une machine virtuelle préexistante, supprimez toutes les captures instantanées précédentes qui lui sont associées.  

## <a name="deployment-steps"></a>Étapes de déploiement  

### <a name="step-1-set-up-the-hyper-v-hosts"></a>Étape 1 : configurer les ordinateurs hôtes Hyper-V  
Vous aurez besoin d’au moins deux ordinateurs hôtes Hyper-V avec une ou plusieurs machines virtuelles sur chaque serveur. [Prise en main d’Hyper-V](../get-started/Get-started-with-Hyper-V-on-Windows.md). Le serveur hôte sur lequel vous allez répliquer des machines virtuelles doit être configuré en tant que serveur de réplication.  

1.  Dans les paramètres Hyper-V du serveur vers lequel vous allez répliquer des machines virtuelles, dans **configuration de la réplication**, sélectionnez **activer cet ordinateur en tant que serveur**de réplication.  

2.  Vous pouvez répliquer sur HTTP ou HTTPs chiffré. Sélectionnez **utiliser Kerberos (http)** ou **utiliser l’authentification basée sur les certificats (https**). Par défaut, HTTP 80 et HTTPs 443 sont activés en tant qu’exceptions de pare-feu sur le serveur Hyper-V de réplication. Si vous modifiez les paramètres de port par défaut, vous devrez également modifier l’exception de pare-feu. Si vous effectuez une réplication via HTTPs, vous devez sélectionner un certificat et configurer l’authentification par certificat.  

3.  Pour autorisation, sélectionnez **autoriser la réplication à partir de n’importe quel serveur authentifié** pour autoriser le serveur de réplication à accepter le trafic de réplication de l’ordinateur virtuel à partir de n’importe quel serveur principal qui s’authentifie correctement. Sélectionnez **autoriser la réplication à partir des serveurs spécifiés** pour accepter le trafic uniquement à partir des serveurs principaux que vous sélectionnez spécifiquement.  

    Pour les deux options, vous pouvez spécifier l’emplacement de stockage des disques durs virtuels répliqués sur le serveur Hyper-V de réplication.  

4.  Cliquez sur **OK** ou **Appliquer**.  

### <a name="step-2-set-up-the-firewall"></a>Étape 2 : configurer le pare-feu  
Pour autoriser la réplication entre les serveurs principaux et secondaires, le trafic doit traverser le pare-feu Windows (ou tout autre pare-feu tiers). Lorsque vous avez installé le rôle Hyper-V sur les serveurs par défaut, les exceptions pour HTTP (80) et HTTPs (443) sont créées. Si vous utilisez ces ports standard, il vous suffit d’activer les règles :  

-  Pour activer les règles sur un serveur hôte autonome :  

    1. Ouvrez le pare-feu Windows avec la sécurité avancée, puis cliquez sur **règles de trafic entrant**.  

    2. Pour activer l’authentification HTTP (Kerberos), cliquez avec le bouton droit sur **écouteur HTTP de réplica Hyper-V (TCP-in)**  >**activer la règle.** Pour activer l’authentification par certificat HTTPs, cliquez avec le bouton droit sur l' **écouteur https de réplica Hyper-V (TCP-entrant)** >**règle**d’activation.  

-  Pour activer les règles sur un cluster Hyper-V, ouvrez une session Windows PowerShell à l’aide de **exécuter en tant qu’administrateur**, puis exécutez l’une des commandes suivantes :  

    -   Pour HTTP : `get-clusternode | ForEach-Object  {Invoke-command -computername $_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}`  

    -   Pour HTTPs : `get-clusternode | ForEach-Object  {Invoke-command -computername $_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}`  

### <a name="enable-virtual-machine-replication"></a>Activation de la réplication de machine virtuelle  
Procédez comme suit sur chaque machine virtuelle que vous souhaitez répliquer :  

1.  Dans le volet d' **informations** du Gestionnaire Hyper-V, sélectionnez une machine virtuelle en cliquant dessus.  
    Cliquez avec le bouton droit sur l’ordinateur virtuel sélectionné, puis cliquez sur **activer la réplication** pour ouvrir l’Assistant Activation de la réplication.  

2.  Dans la page **Avant de commencer**, cliquez sur **Suivant**.  

3.  Dans la page **spécifier le serveur de réplication** , dans la zone serveur de réplication, entrez le nom NetBIOS ou le nom de domaine complet du serveur de réplication. Si le serveur de réplication fait partie d’un cluster de basculement, entrez le nom du Service Broker de réplication Hyper-V. Cliquez sur **Suivant**.  

4.  Sur la page **spécifier les paramètres de connexion** , le réplica Hyper-V récupère automatiquement les paramètres d’authentification et de port que vous avez configurés pour le serveur de réplication. Si les valeurs ne sont pas récupérées, vérifiez que le serveur est configuré en tant que serveur de réplication et qu’il est inscrit dans DNS. Si nécessaire, tapez le paramètre manuellement.  

5.  Dans la page **choisir les disques durs virtuels de réplication** , assurez-vous que les disques durs virtuels que vous souhaitez répliquer sont sélectionnés, et désactivez les cases à cocher de tous les disques durs virtuels que vous souhaitez exclure de la réplication. Ensuite, cliquez sur **Suivant**.  

6.  Dans la page **configurer la fréquence de réplication** , spécifiez la fréquence à laquelle les modifications doivent être synchronisées du serveur principal au serveur secondaire. Ensuite, cliquez sur **Suivant**.  

7.  Sur la page **configurer des points de récupération supplémentaires** , indiquez si vous souhaitez conserver uniquement le dernier point de récupération ou créer des points supplémentaires.    Si vous souhaitez récupérer de manière cohérente des applications et des charges de travail qui ont leurs propres enregistreurs VSS, nous vous recommandons de sélectionner **service VSS (VSS) FREQUENC**y et de spécifier la fréquence de création des captures instantanées de cohérence des applications. Notez que le service de demandeur VMM Hyper-V doit être en cours d’exécution sur les serveurs Hyper-V principal et secondaire. Ensuite, cliquez sur **Suivant**.  

8.  Dans la page **choisir la réplication initiale** , sélectionnez la méthode de réplication initiale à utiliser.  Le paramètre par défaut pour envoyer la copie initiale sur le réseau copie le fichier de configuration d’ordinateur virtuel principal (VMCX) et les fichiers de disque dur virtuel (VHDX et VHD) que vous avez sélectionnés sur votre connexion réseau. Vérifiez la disponibilité de la bande passante réseau si vous envisagez d’utiliser cette option. Si la machine virtuelle principale est déjà configurée sur le site secondaire comme machine virtuelle de réplication, il peut être utile de sélectionner **utiliser un ordinateur virtuel existant sur le serveur de réplication comme copie initiale**. Vous pouvez utiliser l’exportation Hyper-V pour exporter l’ordinateur virtuel principal et l’importer en tant qu’ordinateur virtuel de réplication sur le serveur secondaire. Pour les machines virtuelles plus volumineuses ou la bande passante limitée, vous pouvez choisir de faire en sorte que la réplication initiale sur le réseau se produise plus tard, puis de configurer les heures creuses ou d’envoyer les informations de réplication initiale en tant que média hors connexion.  

    Si vous effectuez une réplication hors connexion, vous transportez la copie initiale sur le serveur secondaire à l’aide d’un support de stockage externe tel qu’un disque dur ou un lecteur USB. Pour ce faire, vous devez connecter le stockage externe au serveur principal (ou au nœud propriétaire dans un cluster), puis, lorsque vous sélectionnez Envoyer la copie initiale à l’aide d’un support externe, vous pouvez spécifier un emplacement local ou sur votre support externe où la copie initiale peut être stockée.  Un ordinateur virtuel d’espace réservé est créé sur le site de réplication. Une fois la réplication initiale terminée, le stockage externe peut être livré au site de réplication. Vous connecterez le média externe au serveur secondaire ou au nœud propriétaire du cluster secondaire. Ensuite, vous allez importer le réplica initial à un emplacement spécifié et le fusionner dans la machine virtuelle d’espace réservé.  

9. Dans la page **fin de l’activation de la réplication** , passez en revue les informations du résumé, puis cliquez sur **Terminer.** Les données de la machine virtuelle seront transférées conformément aux paramètres que vous avez choisis. une boîte de dialogue s’affiche et indique que la réplication a été correctement activée.  

10. Si vous souhaitez configurer la réplication étendue (chaînée), ouvrez le serveur de réplication, puis cliquez avec le bouton droit sur la machine virtuelle que vous souhaitez répliquer. Cliquez sur **réplication** > **étendre la réplication** et spécifiez les paramètres de réplication.  

## <a name="run-a-failover"></a>Exécuter un basculement  
À l’issue de ces étapes de déploiement, votre environnement répliqué est opérationnel. Vous pouvez maintenant exécuter les basculements nécessaires.  

**Test de basculement**: Si vous souhaitez exécuter un test de basculement, cliquez avec le bouton droit sur l’ordinateur virtuel principal et sélectionnez **réplication** > **test de basculement**. Choisissez le point de récupération le plus récent ou un autre, s’il est configuré. Un nouvel ordinateur virtuel de test sera créé et démarré sur le site secondaire. Une fois les tests terminés, sélectionnez **arrêter le test de basculement** sur la machine virtuelle de réplication pour la nettoyer. Notez que, pour une machine virtuelle, vous ne pouvez exécuter qu’un test de basculement à la fois. [En savoir plus](https://blogs.technet.com/b/virtualization/archive/2012/07/26/types-of-failover-operations-in-hyper-v-replica.aspx).  

**Basculement planifié**: pour exécuter un basculement planifié, cliquez avec le bouton droit sur l’ordinateur virtuel principal et sélectionnez **réplication** > le **basculement planifié**. Le basculement planifié effectue des vérifications de la configuration requise pour garantir l’absence de perte de données. Il vérifie que la machine virtuelle principale est arrêtée avant de commencer le basculement. Une fois la machine virtuelle basculée, elle commence à répliquer les modifications sur le site principal lorsqu’elle est disponible. Notez que pour que cela fonctionne, le serveur principal doit être configuré pour recevoir la réplication à partir du serveur secondaire ou du Service Broker de réplication Hyper-V dans le cas d’un cluster principal. Le basculement planifié envoie le dernier ensemble de modifications suivies. [En savoir plus](https://blogs.technet.com/b/virtualization/archive/2012/07/31/types-of-failover-operations-in-hyper-v-replica-part-ii-planned-failover.aspx).  

**Basculement non planifié**: pour exécuter un basculement non planifié, cliquez avec le bouton droit sur l’ordinateur virtuel de réplication, puis sélectionnez **réplication** > **basculement non planifié** à partir du Gestionnaire Hyper-V ou du gestionnaire de clustering de basculement. Si cette option est activée, vous pouvez récupérer à partir du point de récupération le plus récent ou des points de récupération précédents. Après le basculement, vérifiez que tout fonctionne comme prévu sur la machine virtuelle basculée, puis cliquez sur **Terminer** sur la machine virtuelle de réplication. [En savoir plus](https://blogs.technet.com/b/virtualization/archive/2012/08/08/types-of-failover-operations-in-hyper-v-replica-part-iii-unplanned-failover.aspx).  
