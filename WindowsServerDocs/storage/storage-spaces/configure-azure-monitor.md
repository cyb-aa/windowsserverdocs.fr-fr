---
title: Comprendre et configurer Azure Monitor
description: Informations d’installation détaillées sur les Azure Monitor et la configuration des alertes par courrier électronique et SMS pour votre cluster d’espaces de stockage direct dans Windows Server 2016 et 2019.
keywords: Espaces de stockage direct, Azure Monitor, notifications, e-mail, SMS
ms.assetid: ''
ms.prod: windows-server-threshold
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/10/2020
ms.openlocfilehash: 933a22dad76f80b8ff76f604089bfd7c9bf3e207
ms.sourcegitcommit: 76469d1b7465800315eaca3e0c7f0438fc3939ed
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2020
ms.locfileid: "75919977"
---
# <a name="use-azure-monitor-to-send-emails-for-health-service-faults"></a>Utiliser Azure Monitor pour envoyer des e-mails pour les erreurs de Service de contrôle d’intégrité

>S’applique à : Windows Server 2019, Windows Server 2016

Azure Monitor optimise la disponibilité et les performances de vos applications en fournissant une solution complète pour collecter, analyser et agir sur les données de télémétrie de vos environnements cloud et locaux. Il vous aide à comprendre le fonctionnement de vos applications et identifie de façon proactive les problèmes qui les affectent ainsi que les ressources dont elles dépendent.

Cela s’avère particulièrement utile pour votre cluster hyper-convergé local. Avec Azure Monitor intégré, vous serez en mesure de configurer la messagerie électronique, le texte (SMS) et d’autres alertes en cas de problème avec votre cluster (ou lorsque vous souhaitez marquer une autre activité en fonction des données collectées). Ci-dessous, nous expliquons brièvement comment Azure Monitor fonctionne, comment installer Azure Monitor et comment le configurer pour vous envoyer des notifications.

Si vous utilisez System Center, consultez le pack d' [administration espaces de stockage direct](https://www.microsoft.com/download/details.aspx?id=100782) qui surveille les clusters windows server 2019 et windows server 2016 espaces de stockage direct.

Ce pack d’administration comprend les éléments suivants :

* Analyse des performances et de l’intégrité du disque physique
* Analyse des performances et de l’intégrité des nœuds de stockage
* Analyse des performances et de l’intégrité du pool de stockage
* État du type et de la réplication de la résilience du volume

## <a name="understanding-azure-monitor"></a>Compréhension des Azure Monitor

Toutes les données collectées par Azure Monitor font partie d’un des deux types fondamentaux : métriques et journaux.

1. Les [métriques](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection#metrics) sont des valeurs numériques décrivant un aspect d’un système à un moment précis dans le temps. Elles sont légères et capables de prendre en charge des scénarios en quasi-temps réel. Vous verrez les données collectées par Azure Monitor à droite dans la page vue d’ensemble de la Portail Azure.

![image des mesures ingérées dans Metrics Explorer](media/configure-azure-monitor/metrics.png)

2. Les [journaux d’activité](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection#logs) contiennent différents types de données organisées en enregistrements, avec différents jeux de propriétés pour chaque type. Les données de télémétrie, comme les événements et les traces, sont stockées sous forme de journaux d’activité en plus des données de performances, afin qu’elles puissent être combinées pour analyse. Les données de journal collectées par Azure Monitor peuvent être analysées à l’aide de [requêtes](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview) qui permettent de récupérer, consolider et analyser rapidement les données collectées. Vous pouvez créer et tester des requêtes à l’aide de [Log Analytics](https://docs.microsoft.com/azure/azure-monitor/log-query/portals) dans le Portail Azure, avant d’analyser directement les données à l’aide de ces outils ou d’enregistrer les requêtes pour les utiliser pour les [visualisations](https://docs.microsoft.com/azure/azure-monitor/visualizations) ou les [règles d’alerte](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview).

![image des journaux ingérés dans log Analytics](media/configure-azure-monitor/logs.png)

Nous verrons plus de détails ci-dessous sur la configuration de ces alertes.

## <a name="onboarding-your-cluster-using-windows-admin-center"></a>Intégration de votre cluster à l’aide du centre d’administration Windows

À l’aide du centre d’administration Windows, vous pouvez intégrer votre cluster à Azure Monitor.

![GIF de cluster d’intégration à Azure Monitor»](media/configure-azure-monitor/onboarding.gif)

Pendant ce processus d’intégration, les étapes ci-dessous se produisent en coulisses. Nous expliquons comment les configurer en détail au cas où vous souhaiteriez configurer manuellement votre cluster. 

### <a name="configuring-health-service"></a>Configuration de Service de contrôle d’intégrité

La première chose à faire est de configurer votre cluster. Comme vous le savez peut-être, le [service de contrôle d’intégrité](../../failover-clustering/health-service-overview.md) améliore la surveillance quotidienne et l’expérience opérationnelle pour les clusters exécutant espaces de stockage direct. 

Comme nous l’avons vu plus haut, Azure Monitor collecte les journaux de chaque nœud sur lequel il s’exécute dans votre cluster. Nous devons donc configurer le Service de contrôle d’intégrité pour écrire dans un canal d’événement, qui se présente comme suit :

```
Event Channel: Microsoft-Windows-Health/Operational
Event ID: 8465
```

Pour configurer l’Service de contrôle d’intégrité, vous exécutez :

```PowerShell
get-storagesubsystem clus* | Set-StorageHealthSetting -Name "Platform.ETW.MasTypes" -Value "Microsoft.Health.EntityType.Subsystem,Microsoft.Health.EntityType.Server,Microsoft.Health.EntityType.PhysicalDisk,Microsoft.Health.EntityType.StoragePool,Microsoft.Health.EntityType.Volume,Microsoft.Health.EntityType.Cluster"
```

Lorsque vous exécutez l’applet de commande ci-dessus pour définir les paramètres d’intégrité, vous provoquez l’écriture des événements sur le canal d’événements *Microsoft-Windows-Health/Operational* .

### <a name="configuring-log-analytics"></a>Configuration de Log Analytics

Maintenant que vous avez configuré la journalisation appropriée sur votre cluster, l’étape suivante consiste à configurer correctement log Analytics.

Pour obtenir une vue d’ensemble, [Azure log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/agent-windows) peut collecter des données directement à partir de vos ordinateurs physiques ou virtuels Windows dans votre centre de données ou dans un autre environnement Cloud dans un référentiel unique pour une analyse et une corrélation détaillées.

Pour comprendre la configuration prise en charge, consultez les pages [Prise en charge des systèmes d’exploitation Windows](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems) et [Configuration du pare-feu réseau](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#network-firewall-requirements).

Si vous n’avez pas d’abonnement Azure, créez un [compte gratuit](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) avant de commencer.

#### <a name="login-in-to-azure-portal"></a>Connectez-vous au portail Azure

Connectez-vous au portail Azure à l’adresse [https://portal.azure.com](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

#### <a name="create-a-workspace"></a>Créer un espace de travail

Pour plus d’informations sur les étapes indiquées ci-dessous, consultez la [documentation Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/learn/quick-collect-windows-computer).

1. Dans le portail Azure, cliquez sur **Tous les services**. Dans la liste de ressources, saisissez **Log Analytics**. À mesure que tapez, la liste s'affine en fonction des informations saisies. Sélectionnez **Log Analytics**.<br><br> 

   ![Portail Azure](media/configure-azure-monitor/azure-portal-01.png)<br><br>

2. Cliquez sur **Créer**, puis sélectionnez des options pour les éléments suivants :

   * Attribuez un nom au nouvel **Espace de travail Log Analytics**, tel que *DefaultLAWorkspace*. 
   * Dans la liste déroulante **Abonnement**, sélectionnez un abonnement à lier si la valeur par défaut sélectionnée n’est pas appropriée.
   * Pour **Groupe de ressources**, sélectionnez un groupe de ressources existant qui contient une ou plusieurs machines virtuelles Azure. <br><br>

      ![Créer le panneau de ressources Log Analytics](media/configure-azure-monitor/create-loganalytics-workspace-02.png) <br><br>  

3. Après avoir entré les informations requises dans le volet **Espace de travail Log Analytics**, cliquez sur **OK**.  

Pendant que les informations sont vérifiées et l’espace de travail créé, vous pouvez suivre la progression sous **Notifications** dans le menu. 

#### <a name="obtain-workspace-id-and-key"></a>Obtenir l’ID et la clé d’espace de travail
Avant d’installer Microsoft Monitoring Agent pour Windows, vous devez disposer de l’ID et de la clé d’espace de travail pour votre espace de travail Log Analytics.  L’Assistant Installation a besoin de ces informations pour configurer correctement l’agent et s’assurer qu’il peut communiquer avec Log Analytics.  

1. Dans le portail Azure, cliquez sur **Tous les services** en haut à gauche. Dans la liste de ressources, saisissez **Log Analytics**. À mesure que tapez, la liste s'affine en fonction des informations saisies. Sélectionnez **Log Analytics**.
2. Dans votre liste d’espaces de travail Log Analytics, sélectionnez *DefaultLAWorkspace* créé précédemment.
3. Sélectionnez **Paramètres avancés**.<br><br> ![Paramètres avancés de Log Analytics](media/configure-azure-monitor/log-analytics-advanced-settings-01.png)<br><br>  
4. Sélectionnez **Sources connectées**, puis **Serveurs Windows**.   
5. Des valeurs figurent à droite d’**ID de l’espace de travail** et de **Clé primaire**. Enregistrez les copies temporaires et collez-les dans votre éditeur favori pour le moment.   

### <a name="installing-the-agent-on-windows"></a>Installation de l’agent sur Windows
Les étapes suivantes installent et configurent le Microsoft Monitoring Agent. **Veillez à installer cet agent sur chaque serveur de votre cluster et indiquez que vous souhaitez que l’agent s’exécute au démarrage de Windows.**

1. Dans la page **Serveurs Windows**, sélectionnez l’option **Télécharger l’agent Windows** correspondant à la version appropriée selon l’architecture du processeur du système d’exploitation Windows.
2. Exécutez le programme d’installation pour installer l’agent sur votre ordinateur.
2. Dans la page **Bienvenue**, cliquez sur **Suivant**.
3. Dans la page **Termes du contrat de licence**, lisez les conditions de licence, puis cliquez sur **J’accepte**.
4. Dans la page **Dossier de destination**, modifiez ou conservez le dossier d’installation par défaut, puis cliquez sur **Suivant**.
5. Sur la page **Options d’installation de l’agent**, choisissez de connecter l’agent à Azure Log Analytics, puis cliquez sur **Suivant**.   
6. Dans la page **Azure Log Analytics**, procédez comme suit :
   1. Collez l’**ID de l’espace de travail** et la **Clé d’espace de travail (Clé primaire)** que vous avez copiés précédemment.    
    a. Si l’ordinateur a besoin de communiquer via un serveur proxy avec le service Log Analytics, cliquez sur **Avancé**, puis indiquez l’URL et le numéro de port du serveur proxy.  Si votre serveur proxy requiert une authentification, tapez le nom d’utilisateur et un mot de passe pour vous authentifier auprès du serveur proxy, puis cliquez sur **Suivant**.  
7. Cliquez sur **Suivant** après avoir fourni les paramètres de configuration nécessaires.<br><br> ![Coller l’ID et la clé primaire de l’espace de travail](media/configure-azure-monitor/log-analytics-mma-setup-laworkspace.png)<br><br>
8. Dans la page **Prêt pour l’installation**, passez en revue vos choix, puis cliquez sur **Installer**.
9. Dans la page **Configuration effectuée**, cliquez sur **Terminer**.

À la fin de ce processus, l’ **agent Microsoft Monitoring Agent** apparaît dans le **Panneau de configuration**. Vous pouvez examiner votre configuration et vérifier que l’agent est connecté à Log Analytics. Une fois connecté, sous l’onglet **Azure Log Analytics**, l’agent affiche un message indiquant que **Microsoft Monitoring Agent s’est correctement connecté au service Microsoft Log Analytics**. 

![État de la connexion MMA à Log Analytics](media/configure-azure-monitor/log-analytics-mma-laworkspace-status.png)

Pour comprendre la configuration prise en charge, consultez les pages [Prise en charge des systèmes d’exploitation Windows](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems) et [Configuration du pare-feu réseau](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#network-firewall-requirements).

## <a name="setting-up-alerts-using-windows-admin-center"></a>Configuration des alertes à l’aide du centre d’administration Windows

Dans le centre d’administration Windows, vous pouvez configurer des alertes par défaut qui s’appliquent à tous les serveurs de votre Log Analytics espace de travail. 

![GIF de configuration des alertes»](media/configure-azure-monitor/setup1.gif)

Il s’agit des alertes et de leurs conditions par défaut que vous pouvez choisir :

| Nom de l'alerte                | Condition par défaut                                  |
|---------------------------|----------------------------------------------------|
| Utilisation du processeur           | Plus de 85% pendant 10 minutes                            |
| Utilisation de la capacité du disque | Plus de 85% pendant 10 minutes                            |
| Utilisation de la mémoire        | Mémoire disponible inférieure à 100 Mo pendant 10 minutes   |
| Pulsation                 | Moins de 2 temps pendant 5 minutes                   |
| Erreur système critique     | Toute alerte critique dans le journal des événements du système de cluster |
| Alerte du service de contrôle d’intégrité      | Toute erreur du service de contrôle d’intégrité sur le cluster            |

Une fois que vous avez configuré les alertes dans le centre d’administration Windows, vous pouvez voir les alertes dans votre espace de travail log Analytics dans Azure.

![GIF de configuration des alertes»](media/configure-azure-monitor/setup2.gif)

Pendant ce processus d’intégration, les étapes ci-dessous se produisent en coulisses. Nous expliquons comment les configurer en détail au cas où vous souhaiteriez configurer manuellement votre cluster. 

### <a name="collecting-event-and-performance-data"></a>Collecte des données d’événement et de performances

Log Analytics est capable de collecter les événements des journaux des événements Windows ainsi que des compteurs de performances que vous spécifiez dans l’optique de procéder à une analyse à long terme et de générer des rapports afin de réagir dès qu’une condition particulière est détectée.  Pour configurer la collecte d’événements à partir du journal des événements Windows, ainsi que plusieurs compteurs de performances courants avec lesquels commencer, procédez comme suit.  

1. Dans le portail Azure, cliquez sur **Plus de services** dans l’angle inférieur gauche. Dans la liste de ressources, saisissez **Log Analytics**. À mesure que tapez, la liste s'affine en fonction des informations saisies. Sélectionnez **Log Analytics**.
2. Sélectionnez **Paramètres avancés**.<br><br> ![Paramètres avancés de Log Analytics](media/configure-azure-monitor/log-analytics-advanced-settings-01.png)<br><br> 
3. Sélectionnez **Données**, puis **Journaux des événements Windows**.  
4. Ici, ajoutez le Service de contrôle d’intégrité canal d’événements en tapant le nom ci-dessous, puis cliquez sur le signe plus (+) **+** .  
   ```
   Event Channel: Microsoft-Windows-Health/Operational
   ```
5. Dans le tableau, vérifiez les niveaux de gravité **Erreur** et **Avertissement**.   
6. Cliquez sur **Enregistrer** en haut de la page pour enregistrer la configuration.
7. Sélectionnez **Compteurs de performances Windows** pour activer la collecte des compteurs de performances sur un ordinateur Windows. 
8. Quand vous procédez à la configuration initiale des compteurs de performances Windows pour un nouvel espace de travail Log Analytics, la possibilité vous est offerte de créer rapidement plusieurs compteurs courants. Ils s’affichent avec une case à cocher en regard.<br> ![Compteurs de performances Windows par défaut sélectionnés](media/configure-azure-monitor/windows-perfcounters-default.png)<br> Cliquez sur **Ajouter les compteurs de performances sélectionnés**.  Ils sont ajoutés et prédéfinis avec un intervalle d’échantillonnage de collecte de dix secondes.  
9. Cliquez sur **Enregistrer** en haut de la page pour enregistrer la configuration.

## <a name="creating-alerts-based-on-log-data"></a>Création d’alertes basées sur des données de journal

Si vous avez déjà effectué cette opération, votre cluster doit envoyer vos journaux et compteurs de performance à Log Analytics. L’étape suivante consiste à créer des règles d’alerte qui exécutent automatiquement des recherches dans les journaux à intervalles réguliers. Si les résultats de la recherche dans les journaux correspondent à des critères spécifiques, une alerte est déclenchée pour vous envoyer une notification par courrier électronique ou texte. Étudions ce qui suit.

### <a name="create-a-query"></a>Créer une requête

Commencez par ouvrir le portail Recherche dans les journaux.   

1. Dans le portail Azure, cliquez sur **Tous les services**. Dans la liste des ressources, tapez **Moniteur**. À mesure que tapez, la liste s'affine en fonction des informations saisies. Sélectionnez **Moniteur**.
2. Dans le menu de navigation Moniteur, sélectionnez **Log Analytics**, puis sélectionnez un espace de travail.

Le moyen le plus rapide de récupérer des données à utiliser consiste à faire appel à une requête simple qui retourne tous les enregistrements d’une table. Tapez les requêtes suivantes dans la zone de recherche, puis cliquez sur le bouton Rechercher.  

```
Event
```

Les données sont retournées dans la vue Liste par défaut, et vous pouvez voir le nombre total d’enregistrements retournés.

![Requête simple](media/configure-azure-monitor/log-analytics-portal-eventlist-01.png)

Sur le côté gauche de l’écran se trouve le volet de filtre, qui vous permet d’ajouter un filtrage à la requête sans la modifier directement.  Plusieurs propriétés d’enregistrement s’affichent pour ce type d’enregistrement, et vous pouvez sélectionner une ou plusieurs valeurs de propriété pour affiner vos résultats de recherche.

Cochez la case en regard de **erreur** sous **valeur eventlevelname** ou tapez la commande suivante pour limiter les résultats aux événements d’erreur.

```
Event | where (EventLevelName == "Error")
```

![Filter](media/configure-azure-monitor/log-analytics-portal-eventlist-02.png)

Une fois les requêtes appropriées effectuées pour les événements qui vous intéressent, enregistrez-les pour l’étape suivante.

### <a name="create-alerts"></a>Créer des alertes
À présent, examinons un exemple de création d’une alerte.

1. Dans le portail Azure, cliquez sur **Tous les services**. Dans la liste de ressources, saisissez **Log Analytics**. À mesure que tapez, la liste s'affine en fonction des informations saisies. Sélectionnez **Log Analytics**.
2. Dans le volet gauche, sélectionnez **Alertes** puis cliquez sur **Nouvelle règle d’alerte** en haut de la page pour créer une nouvelle alerte.<br><br> ![Créer une nouvelle règle d’alerte](media/configure-azure-monitor/alert-rule-02.png)<br>
3. Pour la première étape, dans la section **Créer une alerte**, sélectionnez votre espace de travail Log Analytics en tant que ressource, car il s’agit d’un signal d’alerte basé sur des journaux.  Filtrez les résultats en choisissant l' **abonnement** spécifique dans la liste déroulante si vous en avez plusieurs, qui contient log Analytics espace de travail créé précédemment.  Filtrez le **Type de ressource** en sélectionnant **Log Analytics** dans la liste déroulante.  Enfin, sélectionnez la **ressource** **DefaultLAWorkspace** , puis cliquez sur **terminé**.<br><br> ![Créer une tâche d’alerte étape 1](media/configure-azure-monitor/alert-rule-03.png)<br>
4. Sous la section **critères d’alerte**, cliquez sur **Ajouter des critères** pour sélectionner votre requête enregistrée, puis spécifiez la logique que la règle d’alerte suit.
5. Configurez l’alerte avec les informations suivantes :  
   a. Dans la liste déroulante **Basé sur**, sélectionnez **Mesure des métriques**.  Une mesure des métriques créera une alerte pour chaque objet de la requête dont la valeur dépasse notre seuil spécifié.  
   b. Pour la **condition**, sélectionnez **supérieur à** et spécifiez un thershold.  
   c. Ensuite, définissez quand déclencher l’alerte. Par exemple, vous pouvez sélectionner des **violations consécutives** et, dans la liste déroulante, sélectionner **supérieur à** la valeur 3.  
   d. Sous évaluation basée sur la section, modifiez la valeur de **période** à **30** minutes et la **fréquence** à 5. La règle s’exécute toutes les cinq minutes et renvoie les enregistrements qui ont été créés dans les trente minutes précédant l’heure actuelle.  Paramétrer la période de temps sur une durée plus longue compense la latence potentielle des données et garantit que la requête retourne des données pour éviter un faux positif où l’alerte ne se déclenche jamais.  
6. Cliquez sur **Terminé** pour terminer la règle d’alerte.<br><br> ![Configurer un signal d’alerte](media/configure-azure-monitor/alert-signal-logic-02.png)<br> 
7. Maintenant, passez à la deuxième étape, fournissez un nom de votre alerte dans le champ nom de la **règle d’alerte** , par exemple **alerte sur tous les événements d’erreur**.  Spécifiez une **Description** détaillant les spécificités de l’alerte, puis sélectionnez **Critique (gravité 0)** pour la valeur de **Gravité** parmi les options fournies.
8. Pour activer immédiatement la règle d’alerte lors de la création, acceptez la valeur par défaut pour l’option **Activer la règle lors de sa création**.
9. Pour la troisième et dernière étape, vous spécifiez un **Groupe d’actions**, ce qui garantit que les mêmes actions soient entreprises chaque fois qu’une alerte est déclenchée et peuvent être utilisées pour chaque règle que vous définissez. Configurez un nouveau groupe d’actions avec les informations suivantes :  
   a. Sélectionnez **Nouveau groupe d’actions** et le volet **Ajouter un groupe action** s’affiche.  
   b. Pour le **Nom du groupe d’actions**, spécifiez un nom tel que **Opérations informatiques - Notifier** et un **Nom court** comme **itops-n**.  
   c. Vérifiez que les valeurs par défaut de l’**Abonnement** et du **Groupe de ressources** sont correctes. Si ce n’est pas le cas, sélectionnez la valeur correcte dans la liste déroulante.   
   d. Dans la section Actions, spécifiez un nom pour l’action, tel que **Envoyer un message électronique**, et pour le **Type d’action**, sélectionnez **E-mail/SMS/Push/Voix** dans la liste déroulante. Le volet Propriétés **E-mail/SMS/Push/Voix** s’ouvre sur la droite afin de fournir des informations supplémentaires.  
   e. Dans le volet **e-mail/SMS/Push/Voice** , sélectionnez et configurez votre préférence. Par exemple, activez la **messagerie électronique** et fournissez une adresse SMTP de messagerie valide pour remettre le message à.  
   f. Cliquez sur **OK** pour enregistrer vos modifications.<br><br> 

    ![Créer un nouveau groupe d’action](media/configure-azure-monitor/action-group-properties-01.png)

10. Cliquez sur **OK** pour créer le groupe d’actions. 
11. Cliquez sur **Créer une règle d’alerte** pour terminer la règle d’alerte. Son exécution démarre immédiatement.<br><br> ![Terminer la création d’une nouvelle règle d’alerte](media/configure-azure-monitor/alert-rule-01.png)<br> 

### <a name="example-alert"></a>Exemple d’alerte

Pour référence, voici à quoi ressemble un exemple d’alerte dans Azure.

![GIF d’alerte dans Azure](media/configure-azure-monitor/alert.gif)

Voici un exemple de l’e-mail que vous allez envoyer par Azure Monitor :

![Exemple d’e-mail d’alerte](media/configure-azure-monitor/warning.png)

## <a name="see-also"></a>Articles associés

- [Présentation de espaces de stockage direct](storage-spaces-direct-overview.md)
- Pour plus d’informations, consultez la [documentation Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-viewdata).
- Lisez cet article pour obtenir une vue d’ensemble de la façon de [se connecter à d’autres services hybrides Azure](../../manage/windows-admin-center/azure/index.md).
