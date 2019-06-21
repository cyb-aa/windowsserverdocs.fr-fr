---
title: Comprendre et configurer Azure Monitor
description: Le programme d’installation des informations détaillées sur Azure Monitor What ' s et comment configurer des alertes par e-mail et sms pour votre cluster des espaces de stockage direct dans Windows Server 2016 et 2019.
keywords: Espaces de stockage Direct, azure monitor, notifications, e-mail, sms
ms.assetid: ''
ms.prod: ''
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 3/26/2019
ms.localizationpriority: ''
ms.openlocfilehash: 6b229696e796f176fe89ab250ab48f1d9f0d5666
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280203"
---
---
# <a name="use-azure-monitor-to-send-emails-for-health-service-faults"></a>Utiliser Azure Monitor pour envoyer des courriers électroniques pour les erreurs de Service de contrôle d’intégrité

>S’applique à : Windows Server 2019, Windows Server 2016

Azure Monitor optimise la disponibilité et les performances de vos applications en fournissant une solution complète pour collecter, analyser et agir sur les données de télémétrie de votre cloud et des environnements locaux. Il vous permet de comprendre le fonctionnement de vos applications et identifie de façon proactive problèmes qui affectent les et les ressources qu’ils dépendent.

Cela est particulièrement utile pour votre cluster hyperconvergé d’en local. Avec Azure Monitor intégré, vous serez en mesure de configurer la messagerie, le texte (SMS) et autres alertes à la commande ping de lorsque quelque chose ne va pas avec votre cluster (ou lorsque vous souhaitez signaler une autre activité en fonction des données collectées). Ci-dessous, nous expliquerons brièvement comment Azure Monitor fonctionne, comment installer Azure Monitor et comment le configurer pour envoyer des notifications.


## <a name="understanding-azure-monitor"></a>Présentation d’Azure Monitor

Toutes les données collectées par Azure Monitor s’adapte à un des deux types fondamentaux : métriques et les journaux.

1. [Mesures](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection#metrics) sont des valeurs numériques qui décrivent certains aspects d’un système à un moment précis dans le temps. Elles sont légers et peut prendre en charge des scénarios en temps réel quasi. Vous pouvez voir les données collectées par le droit d’Azure Monitor dans leur page de vue d’ensemble dans le portail Azure.

![image de l’ingestion de métriques dans metrics explorer](media/configure-azure-monitor/metrics.png)

2. [Journaux](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection#logs) contiennent différents types de données organisées en enregistrements avec différents jeux de propriétés pour chaque type. Données de télémétrie comme événements et suivis sont stockés sous forme de journaux en outre aux données de performances afin qu’il peut tous être combinée pour l’analyse. Analyse des données de journal collectées par Azure Monitor avec [requêtes](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview) pour rapidement récupérer, consolider et analyser les données collectées. Vous pouvez créer et tester des requêtes à l’aide de [Analytique de journal](https://docs.microsoft.com/azure/azure-monitor/log-query/portals) dans le portail Azure, puis soit directement analyser les données à l’aide de ces outils ou enregistrer des requêtes pour une utilisation avec [visualisations](https://docs.microsoft.com/azure/azure-monitor/visualizations) ou [alerte règles](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview).

![image des journaux d’ingestion dans analytique de journal](media/configure-azure-monitor/logs.png)

Nous aurons plus de détails ci-dessous sur la façon de configurer ces alertes.

## <a name="configuring-health-service"></a>Configuration du Service de contrôle d’intégrité

La première chose que vous devez faire est de configurer votre cluster. Comme vous le savez peut-être, le [Service d’intégrité](../../failover-clustering/health-service-overview.md) améliore l’expérience opérationnelle pour les clusters exécutant des espaces de stockage Direct et surveillance quotidiennes. 

Comme nous l’avons vu ci-dessus, Azure Monitor collecte des journaux à partir de chaque nœud qu’elle s’exécute votre cluster. Par conséquent, nous devons configurer le Service de contrôle d’intégrité pour écrire dans un canal d’événement qui se trouve être :

```
Event Channel: Microsoft-Windows-Health/Operational
Event ID: 8465
```

Pour configurer le Service de contrôle d’intégrité, vous exécutez :

```PowerShell
get-storagesubsystem clus* | Set-StorageHealthSetting -Name "Platform.ETW.MasTypes" -Value "Microsoft.Health.EntityType.Subsystem,Microsoft.Health.EntityType.Server,Microsoft.Health.EntityType.PhysicalDisk,Microsoft.Health.EntityType.StoragePool,Microsoft.Health.EntityType.Volume,Microsoft.Health.EntityType.Cluster"
```

Lorsque vous exécutez l’applet de commande ci-dessus pour définir les paramètres de contrôle d’intégrité, vous entraîner les événements que nous souhaitons commencer en cours d’écriture pour le *Microsoft-Windows-contrôle d’intégrité/Operational* canal d’événement.

## <a name="configuring-log-analytics"></a>Configuration d’Analytique de journal

Maintenant que vous avez configuré la journalisation appropriés sur votre cluster, l’étape suivante consiste à configurer correctement le journal analytique.

Pour donner une vue d’ensemble, [Analytique de journal Azure](https://docs.microsoft.com/azure/azure-monitor/platform/agent-windows) peut collecter des données directement à partir de vos ordinateurs Windows physiques ou virtuels dans votre centre de données ou un autre environnement de cloud dans un référentiel unique pour une analyse détaillée et la corrélation.

Pour comprendre la configuration prise en charge, consultez [pris en charge les systèmes d’exploitation Windows](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems) et [configuration du pare-feu réseau](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#network-firewall-requirements).

Si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) avant de commencer.

### <a name="login-in-to-azure-portal"></a>Connectez-vous au portail Azure

Connectez-vous au portail Azure à [ https://portal.azure.com ](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

### <a name="create-a-workspace"></a>Créer un espace de travail

Pour plus d’informations sur les étapes répertoriées ci-dessous, consultez le [documentation Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/learn/quick-collect-windows-computer).

1. Dans le portail Azure, cliquez sur **tous les services**. Dans la liste des ressources, tapez **Analytique de journal**. Comme vous commencez à taper, les filtres de la liste en fonction de votre entrée. Sélectionnez **journal Analytique**.<br><br> 

   ![Portail Azure](media/configure-azure-monitor/azure-portal-01.png)<br><br>

2. Cliquez sur **créer**, puis sélectionnez des options pour les éléments suivants :

   * Fournissez un nom pour le nouveau **espace de travail Log Analytique**, tel que *DefaultLAWorkspace*. 
   * Sélectionnez un **abonnement** à lier à en sélectionnant dans la liste déroulante si la valeur par défaut sélectionnée n’est pas appropriée.
   * Pour **groupe de ressources**, sélectionnez un groupe de ressources existant qui contient un ou plusieurs machines virtuelles Azure. <br><br>

      ![Créer le panneau de ressources d’Analytique de journal](media/configure-azure-monitor/create-loganalytics-workspace-02.png) <br><br>  

3. Après avoir entré les informations requises dans le **espace de travail Log Analytique** volet, cliquez sur **OK**.  

Bien que les informations sont vérifiées et l’espace de travail est créé, vous pouvez suivre sa progression sous **Notifications** à partir du menu. 

### <a name="obtain-workspace-id-and-key"></a>Obtenir l’ID de l’espace de travail et la clé
Avant d’installer le Microsoft Monitoring Agent pour Windows, vous devez l’ID de l’espace de travail et la clé pour votre espace de travail Analytique de journal.  Cette information est requise par l’Assistant Installation pour configurer correctement l’agent et qu’il puisse communiquer avec l’Analytique de journal.  

1. Dans le portail Azure, cliquez sur **tous les services** trouvé dans le coin supérieur gauche. Dans la liste des ressources, tapez **Analytique de journal**. Comme vous commencez à taper, les filtres de la liste en fonction de votre entrée. Sélectionnez **journal Analytique**.
2. Dans votre liste d’espaces de travail Analytique de journal, sélectionnez *DefaultLAWorkspace* créé précédemment.
3. Sélectionnez **paramètres avancés**.<br><br> ![Paramètres du journal Analytique avancée](media/configure-azure-monitor/log-analytics-advanced-settings-01.png)<br><br>  
4. Sélectionnez **Sources connectées**, puis sélectionnez **les serveurs Windows**.   
5. La valeur à droite de **ID de l’espace de travail** et **clé primaire**. Faites des économies temporairement : copier et coller ces deux valeurs dans votre éditeur favori pour l’instant.   

## <a name="installing-the-agent-on-windows"></a>Installation de l’agent sur Windows
Les étapes suivantes, installer et configurer l’agent Microsoft Monitoring Agent. **Veillez à installer cet agent sur chaque serveur de votre cluster et indiquer que vous souhaitez que l’agent s’exécute au démarrage de Windows.**

1. Sur le **les serveurs Windows** page, sélectionnez l’option appropriée **télécharger l’Agent Windows** version à télécharger selon l’architecture de processeur du système d’exploitation Windows.
2. Exécutez le programme d’installation pour installer l’agent sur votre ordinateur.
2. Dans la page **Bienvenue**, cliquez sur **Suivant**.
3. Sur le **les termes du contrat de licence** page, lisez la licence, puis cliquez sur **J’accepte**.
4. Sur le **dossier de Destination** page, modifiez ou conservez le dossier d’installation par défaut, puis sur **suivant**.
5. Sur le **Options d’installation de l’Agent** page, choisir de connecter l’agent à Azure Log Analytique et puis cliquez sur **suivant**.   
6. Sur le **Analytique de journal Azure** page, procédez comme suit :
   1. Coller le **ID de l’espace de travail** et **clé d’espace de travail (clé primaire)** que vous avez copiée précédemment.    
    a. Si l’ordinateur doit communiquer via un serveur proxy pour le service d’Analytique de journal, cliquez sur **avancé** et indiquez l’URL et le numéro de port du serveur proxy.  Si votre serveur proxy requiert une authentification, tapez le nom d’utilisateur et le mot de passe pour s’authentifier auprès du serveur proxy, puis cliquez sur **suivant**.  
7. Cliquez sur **suivant** après avoir fourni les paramètres de configuration nécessaires.<br><br> ![Collez l’ID de l’espace de travail et la clé primaire](media/configure-azure-monitor/log-analytics-mma-setup-laworkspace.png)<br><br>
8. Sur le **prêt pour l’installation** page, passez en revue vos choix, puis sur **installer**.
9. Sur le **a été correctement configuré** , cliquez sur **Terminer**.

Lorsque vous avez terminé, le **Microsoft Monitoring Agent** apparaît dans **le panneau de configuration**. Vous pouvez vérifier votre configuration et vérifiez que l’agent est connecté au journal Analytique. Lorsque connecté, sous la **Analytique de journal Azure** , l’agent affiche un message indiquant : **L’agent Microsoft Monitoring Agent a correctement connecté au service d’Analytique de journal de Microsoft.** 

![État de la connexion MMA à Log Analytique](media/configure-azure-monitor/log-analytics-mma-laworkspace-status.png)

Pour comprendre la configuration prise en charge, consultez [pris en charge les systèmes d’exploitation Windows](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems) et [configuration du pare-feu réseau](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#network-firewall-requirements).

## <a name="collecting-event-and-performance-data"></a>Collecte des données de performances et d’événements

Analytique de journal peut collecter les événements à partir du journal des événements Windows et les compteurs de performances que vous spécifiez pour l’analyse à long terme et création de rapports et d’agir lorsqu’une condition particulière est détectée.  Suivez ces étapes pour configurer la collecte d’événements à partir du journal des événements Windows et plusieurs compteurs de performances courants pour commencer.  

1. Dans le portail Azure, cliquez sur **plus de services** trouvé sur le coin inférieur gauche. Dans la liste des ressources, tapez **Analytique de journal**. Comme vous commencez à taper, les filtres de la liste en fonction de votre entrée. Sélectionnez **journal Analytique**.
2. Sélectionnez **paramètres avancés**.<br><br> ![Paramètres du journal Analytique avancée](media/configure-azure-monitor/log-analytics-advanced-settings-01.png)<br><br> 
3. Sélectionnez **données**, puis sélectionnez **journaux des événements Windows**.  
4. Ajouter ici, le canal d’événement Service d’intégrité en tapant le nom de ci-dessous et cliquez sur le signe plus **+** .  
   ```
   Event Channel: Microsoft-Windows-Health/Operational
   ```
5. Dans la table, vérifiez les niveaux de gravité **erreur** et **avertissement**.   
6. Cliquez sur **enregistrer** en haut de la page pour enregistrer la configuration.
7. Sélectionnez **les compteurs de Performance Windows** pour activer la collecte des compteurs de performance sur un ordinateur Windows. 
8. Lorsque vous configurez initialement des compteurs de performances de Windows pour un espace de travail Analytique de journal, vous disposez de l’option pour créer rapidement plusieurs compteurs courants. Elles sont répertoriées avec une case à cocher en regard de chaque.<br> ![Compteurs de performances Windows par défaut sélectionnés](media/configure-azure-monitor/windows-perfcounters-default.png)<br> Cliquez sur **ajouter les compteurs de performances sélectionnés**.  Ils sont ajoutés et prédéfinis avec un intervalle d’échantillonnage collecte de dix secondes.  
9. Cliquez sur **enregistrer** en haut de la page pour enregistrer la configuration.

## <a name="creating-alerts-based-on-log-data"></a>Création d’alertes en fonction des données de journal

Si vous êtes arrivé présent, votre cluster doit envoyer vos journaux et les compteurs de performances pour l’Analytique de journal. L’étape suivante consiste à créer des règles d’alerte qui exécutent automatiquement des recherches dans les journaux à intervalles réguliers. Si les résultats de la recherche de journal correspondent aux critères particuliers, une alerte est déclenchée qui vous envoie une notification d’e-mail ou message texte. Nous allons étudier cette ci-dessous.

### <a name="create-a-query"></a>Créer une requête

Commencez par ouvrir le portail de recherche dans les journaux.   

1. Dans le portail Azure, cliquez sur **tous les services**. Dans la liste des ressources, tapez **moniteur**. Comme vous commencez à taper, les filtres de la liste en fonction de votre entrée. Sélectionnez **moniteur**.
2. Dans le menu de navigation analyse, sélectionnez **Analytique de journal** , puis sélectionnez un espace de travail.

Le moyen le plus rapide pour récupérer des données à utiliser avec est une requête simple qui retourne tous les enregistrements dans la table. Tapez les requêtes suivantes dans la zone de recherche, puis cliquez sur le bouton de recherche.  

```
Event
```

Données sont retournées dans l’affichage de liste par défaut, et vous pouvez voir le nombre total d’enregistrements retournés.

![Requête simple](media/configure-azure-monitor/log-analytics-portal-eventlist-01.png)

Sur le côté gauche de l’écran est le volet de filtre qui vous permet d’ajouter un filtrage à la requête sans modifier directement.  Plusieurs propriétés d’enregistrement sont affichées pour ce type d’enregistrement, et vous pouvez sélectionner une ou plusieurs valeurs de propriété pour affiner vos résultats de recherche.

Activez la case à cocher en regard **erreur** sous **EVENTLEVELNAME** ou tapez la commande suivante pour limiter les résultats aux événements d’erreur.

```
Event | where (EventLevelName == "Error")
```

![Filter](media/configure-azure-monitor/log-analytics-portal-eventlist-02.png)

Après avoir configuré les requêtes appropriées effectuées pour les événements qui que vous intéressent, enregistrez-les pour l’étape suivante.

### <a name="create-alerts"></a>Créer des alertes
Maintenant, nous allons étudier un exemple de création d’une alerte.

1. Dans le portail Azure, cliquez sur **tous les services**. Dans la liste des ressources, tapez **Analytique de journal**. Comme vous commencez à taper, les filtres de la liste en fonction de votre entrée. Sélectionnez **journal Analytique**.
2. Dans le volet gauche, sélectionnez **alertes** puis cliquez sur **nouvelle règle d’alerte** à partir du haut de la page pour créer une nouvelle alerte.<br><br> ![Créer la nouvelle règle d’alerte](media/configure-azure-monitor/alert-rule-02.png)<br>
3. Pour la première étape, sous la **créer une alerte** section, vous allez sélectionner votre espace de travail Analytique de journal en tant que la ressource, dans la mesure où il s’agit d’un signal d’alerte de journal en fonction.  Filtrer les résultats en choisissant le spécifique **abonnement** dans la liste déroulante si vous avez plusieurs, qui contient l’espace de travail Analytique de journal créée précédemment.  Filtre la **Type de ressource** en sélectionnant **Analytique de journal** dans la liste déroulante.  Enfin, sélectionnez le **ressource** **DefaultLAWorkspace** puis cliquez sur **fait**.<br><br> ![Créer des tâches d’alerte étape 1](media/configure-azure-monitor/alert-rule-03.png)<br>
4. Dans la section **critères d’alerte**, cliquez sur **ajouter des critères** sélectionner votre requête enregistrée, puis spécifier une logique qui suit la règle d’alerte.
5. Configurez l’alerte avec les informations suivantes :  
   a. À partir de la **selon** liste déroulante, sélectionnez **mesure métrique**.  Une mesure métrique créera une alerte pour chaque objet dans la requête avec une valeur qui dépasse notre seuil spécifié.  
   b. Pour le **Condition**, sélectionnez **supérieur** et spécifiez un thershold.  
   c. Ensuite, définissez quand déclencher l’alerte. Par exemple, vous pouvez sélectionner **violations consécutives** et dans la liste déroulante, sélectionnez **supérieur** une valeur de 3.  
   d. En cours d’évaluation en fonction de la section, vous devez modifier le **période** valeur **30** minutes et **fréquence** à 5. La règle exécute toutes les cinq minutes et renvoyer les enregistrements qui ont été créés au cours des trente dernières minutes de l’heure actuelle.  La définition de la période de temps à une fenêtre plus large des comptes pour le potentiel de la latence des données et garantit que la requête retourne des données pour éviter un faux négatif où l’alerte se déclenche jamais.  
6. Cliquez sur **fait** pour terminer la règle d’alerte.<br><br> ![Configurer le signal d’alerte](media/configure-azure-monitor/alert-signal-logic-02.png)<br> 
7. Maintenant passer à la deuxième étape, fournissez un nom de votre alerte dans le **nom de la règle d’alerte** champ, tels que **alerte sur tous les événements d’erreur**.  Spécifiez un **Description** détaillant les spécificités de l’alerte, puis sélectionnez **critique (gravité 0)** pour le **gravité** valeur parmi les options fournies.
8. Pour activer immédiatement la règle d’alerte lors de la création, acceptez la valeur par défaut **activer la règle lors de la création**.
9. Pour la troisième et dernière étape, vous spécifiez un **groupe d’actions**, ce qui garantit que les mêmes actions soient entreprises chaque fois une alerte est déclenchée et peut être utilisée pour chaque règle que vous définissez. Configurer un nouveau groupe d’actions avec les informations suivantes :  
   a. Sélectionnez **nouveau groupe d’actions** et **ajouter un groupe d’action** volet s’affiche.  
   b. Pour **nom groupe d’actions**, spécifiez un nom tel que **notifier des opérations informatiques -** et un **nom court** comme **itops-n**.  
   c. Vérifiez les valeurs par défaut **abonnement** et **groupe de ressources** sont corrects. Si ce n’est pas le cas, sélectionnez celui qui convient dans la liste déroulante.   
   d. Dans la section Actions, spécifiez un nom pour l’action, tel que **envoyer un message électronique** et sous **Type d’Action** sélectionnez **E-mail/SMS/Push, voix** dans la liste déroulante. Le **E-mail/SMS/Push, voix** volet Propriétés s’ouvre à droite afin de fournir des informations supplémentaires.  
   e. Sur le **E-mail/SMS/Push, voix** volet, sélectionner et configurer vos préférences. Par exemple, activer **E-mail** et fournir une adresse SMTP pour envoyer le message de messagerie valide.  
   f. Cliquez sur **OK** pour enregistrer vos modifications.<br><br> 

    ![Créer le nouveau groupe d’actions](media/configure-azure-monitor/action-group-properties-01.png)

10. Cliquez sur **OK** pour terminer le groupe d’actions. 
11. Cliquez sur **créer une règle d’alerte** pour terminer la règle d’alerte. Il commence à s’exécuter immédiatement.<br><br> ![Terminer la création de nouvelle règle d’alerte](media/configure-azure-monitor/alert-rule-01.png)<br> 

### <a name="test-alerts"></a>Alertes de test

Pour référence, il s’agit quoi ressemble un exemple d’alerte :

![Exemple d’e-mail d’alerte](media/configure-azure-monitor/warning.png)

## <a name="see-also"></a>Voir aussi

- [Vue d’ensemble Direct des espaces de stockage](storage-spaces-direct-overview.md)
- Pour plus d’informations, lisez le [documentation Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-viewdata).
