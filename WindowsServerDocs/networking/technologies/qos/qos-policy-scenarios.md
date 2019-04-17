---
title: Scénarios de stratégie de QoS
description: Cette rubrique fournit des scénarios de stratégie de qualité de Service (QoS), qui montrent comment utiliser la stratégie de groupe pour hiérarchiser le trafic réseau des applications spécifiques et des services dans Windows Server2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c4306f06-a117-4f65-b78b-9fd0d1133f95
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b635f73cc25552f4c1a05f8db6303f636eda3c54
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="qos-policy-scenarios"></a>Scénarios de stratégie de QoS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour passer en revue les scénarios hypothétiques qui montrent comment, quand et pourquoi utiliser la stratégie de QoS.

Les deux scénarios dans cette rubrique sont:

1. Hiérarchiser le trafic réseau pour une Application métier de
2. Hiérarchiser le trafic réseau pour une Application serveur HTTP

>[!NOTE]
>Certaines sections de cette rubrique contient les étapes générales à que suivre pour effectuer les actions décrites. Pour plus d’instructions sur la gestion de stratégie de QoS, voir [gérer la stratégie de QoS](qos-policy-manage.md).

## <a name="scenario-1-prioritize-network-traffic-for-a-line-of-business-application"></a>Scénario 1: Hiérarchiser le trafic réseau pour une Application métier de

Dans ce scénario, un service informatique a plusieurs objectifs pouvez accomplir à l’aide de stratégie de QoS:

- Fournir de meilleures performances de réseau pour les applications stratégiques mission\.
- Fournir de meilleures performances de réseau pour un jeu de clés d’utilisateurs pendant qu’ils utilisent une application spécifique.
- Assurez-vous que les données à l’échelle company\ application de sauvegarde n’entravent les performances du réseau en utilisant trop de bande passante en même temps.

Le service informatique décide de configurer la stratégie de QoS pour hiérarchiser les applications spécifiques à l’aide de valeurs \(DSCP\) Differentiation Service Code Point pour classer le trafic réseau et configurer ses routeurs pour fournir un traitement préférentiel pour le trafic de priorité plus élevé. 

>[!NOTE]
>Pour plus d’informations sur DSCP, consultez la section **définir la qualité de service priorité via un Differentiated Services Code Point** dans la rubrique [stratégie de qualité de Service (QoS)](qos-policy-top.md).

Outre les valeurs DSCP, les stratégies de QoS peuvent spécifier un taux d’accélération. La limitation a pour effet de limiter le trafic sortant correspond à la stratégie de QoS pour spécifiques à un taux d’envoi.

### <a name="qos-policy-configuration"></a>Configuration de la stratégie de QoS

Avec trois objectifs distinctes à accomplir, l’administrateur informatique décide de créer des trois différentes stratégies de qualité de service.

#### <a name="qos-policy-for-lob-app-servers"></a>Stratégie de QoS pour les serveurs d’applications cœur de métier

La première application mission\ critiques pour lequel le service informatique crée une stratégie de QoS est une ressource d’entreprise à l’échelle company\ \(ERP\) application de planification. L’application ERP est hébergée sur plusieurs ordinateurs qui exécutent tous Windows Server2016. Dans les Services de domaine ActiveDirectory, ces ordinateurs sont membres d’une unité d’organisation qui a été créé pour les serveurs d’applications métier de \(LOB\) \(OU\). Le composant côté client pour l’application ERP est installé sur les ordinateurs qui exécutent Windows10 et Windows8.1.

Dans la stratégie de groupe, un administrateur sélectionne le \(GPO\) objet de stratégie de groupe sur lequel la stratégie de qualité de service sera appliquée. À l’aide de l’Assistant Stratégie de QoS, l’administrateur crée une stratégie de QoS appelée «serveur métier stratégie» qui spécifie une valeur DSCP de priorité évolutifs de 44 pour toutes les applications, n’importe quelle adresse IP, TCP et UDP et numéro de port.

La stratégie de QoS s’applique uniquement aux serveurs métier en liant l’objet de stratégie de groupe à l’unité d’organisation qui contient uniquement ces serveurs, via l’outil \(GPMC\) Group Policy Management Console. Cet stratégie métier initiale du serveur s’applique la valeur DSCP de priorité évolutifs chaque fois que l’ordinateur envoie le trafic réseau. Cette stratégie de QoS peut ultérieurement être modifiée \ (dans l’éditeur d’objets de stratégie de groupe tool\) pour inclure les numéros de port de l’application ERP, qui limite la stratégie pour appliquer uniquement lorsque le numéro de port spécifié est utilisé.

#### <a name="qos-policy-for-the-finance-group"></a>Stratégie de QoS pour le groupe Finance

Tandis que de nombreux groupes au sein de la société accéder à l’application ERP, le groupe finance dépend de cette application lorsque des clients et le groupe nécessite des performances élevées à partir de l’application.

Pour vous assurer que le groupe finance peut prendre en charge leurs clients, la stratégie de QoS doive classer le trafic de ces utilisateurs en priorité élevée. Toutefois, la stratégie doit s’applique pas lorsque les membres du groupe finance utilisent des applications autres que l’application ERP. 

Pour cette raison, le service informatique définit une deuxième stratégie QoS appelée «Client métier stratégie» dans l’outil Éditeur d’objets de stratégie de groupe qui applique une valeur DSCP de 60 lorsque le groupe d’utilisateurs finance s’exécute l’application ERP.

#### <a name="qos-policy-for-a-backup-app"></a>Stratégie de QoS pour une application de sauvegarde

Une autre application de sauvegarde est en cours d’exécution sur tous les ordinateurs. Pour vous assurer que le trafic de l’application de sauvegarde n’utilise pas de toutes les ressources réseau disponibles, le service informatique crée une stratégie de données de sauvegarde. Cette stratégie de sauvegarde spécifie une valeur DSCP basé sur le nom du fichier exécutable pour l’application de sauvegarde, qui est de 1 **backup.exe **. 

Une troisième objet stratégie de groupe est créée et déployée pour tous les ordinateurs clients dans le domaine. Chaque fois que l’application de sauvegarde envoie des données, la valeur DSCP de faible priorité est appliquée, même si elle provenant d’ordinateurs du département finance.
  
>[!NOTE]
>Il envoie le trafic réseau sans une stratégie de QoS avec une valeur DSCP de 0.

### <a name="scenario-policies"></a>Stratégies de scénario

Le tableau suivant récapitule les stratégies de QoS pour ce scénario.
  
|Nom de la stratégie|Valeur DSCP|Taux d’accélération|Appliqués aux unités d’organisation|Description|  
|-----------------|----------------|-------------------|-----------------------------------|-----------------|
|[Aucune stratégie]|0|None|[Aucun déploiement]|Meilleure traitement effort (par défaut) pour le trafic non classée.|  
|Données de sauvegarde|1|None|Tous les clients|Applique une valeur DSCP de faible priorité pour ces données en bloc.|  
|Serveurs métier|44|None|Ordinateur unité d’organisation pour les serveurs ERP|S’applique DSCP haute priorité pour le trafic de serveur ERP|  
|Client cœur de métier|60|None|Groupe d’utilisateurs Finance|S’applique DSCP haute priorité pour le trafic de client ERP|  

>[!NOTE]
>Les valeurs DSCP sont représentées sous forme décimale.

Avec les stratégies de qualité de service défini et appliqué à l’aide de stratégie de groupe, le trafic réseau sortant reçoit la valeur DSCP stratégie spécifiée. Routeurs ensuite fournissant traitement différentielle basée sur ces valeurs DSCP à l’aide de queuing. Pour ce service informatique, les routeurs sont configurés avec quatre files d’attente: haute priorité, priorité intermédiaire, meilleur effort et faible priorité.

Lorsque le trafic arrive sur le routeur avec les valeurs DSCP à partir de «serveur métier stratégie» et «Client cœur de métier stratégie,» les données sont placées en file d’attente de haute priorité. Le trafic avec une valeur DSCP 0 reçoit un meilleur effort niveau de service. Les paquets avec une valeur DSCP de 1 (à partir de l’application de sauvegarde) bénéficient d’un traitement faible priorité.  
  
### <a name="prerequisites-for-prioritizing-a-line-of-business-application"></a>Configuration requise pour une application métier d’importance à accorder

Pour effectuer cette tâche, assurez-vous que les conditions suivantes:

- Les ordinateurs concernés sont en cours d’exécution QoS\ compatible avec les systèmes d’exploitation.

- Les ordinateurs concernés sont membres d’un domaine \(ADDS\) de Services de domaine ActiveDirectory afin qu’ils peuvent être configurés à l’aide de stratégie de groupe.

- Les réseaux TCP/IP sont configurés avec des routeurs configurés pour \(RFC2474\) DSCP. Pour plus d’informations, voir [RFC2474](http://www.ietf.org/rfc/rfc2474.txt).

- Configuration requise pour les informations d’identification d’administration sont remplies.

#### <a name="administrative-credentials"></a>Informations d’identification d’administration

Pour effectuer cette tâche, vous devez être en mesure de créer et déployer des objets de stratégie de groupe.
  
#### <a name="setting-up-the-test-environment-for-prioritizing-a-line-of-business-application"></a>Configuration de l’environnement de test pour une application métier d’importance à accorder

Pour configurer l’environnement de test, effectuez les tâches suivantes.

- Créer un domaine ADDS avec les clients et les utilisateurs regroupés dans des unités d’organisation. Pour obtenir des instructions sur le déploiement des services ADDS, voir la [Guide réseau de base ](https://docs.microsoft.com/windows-server/networking/core-network-guide/core-network-guide).

- Configurez les routeurs à différentielle file d’attente en fonction des valeurs DSCP. Par exemple, la valeur DSCP 44 entre une file d’attente de «Platinum» et tous les autres sont pondérées-équitable-en attente.

>[!NOTE]
> Vous pouvez afficher les valeurs DSCP à l’aide de captures réseau avec des outils comme le Moniteur réseau. Après avoir effectué une capture réseau, vous pouvez observer le champ TOS dans les données capturées.

#### <a name="steps-for-prioritizing-a-line-of-business-application"></a>Étapes pour une application métier d’importance à accorder

Pour une application métier de donner la priorité, effectuez les tâches suivantes:

1. Créer et lier un \(GPO\) objet de stratégie de groupe avec une stratégie de QoS.

2. Configurez les routeurs pour traiter différentielle line of business application (à l’aide queuing) basée sur les valeurs DSCP sélectionnés. Les procédures de cette tâche varie selon le type de routeurs que vous avez.

## <a name="scenario-2-prioritize-network-traffic-for-an-http-server-application"></a>Scénario 2: Hiérarchiser le trafic réseau pour une Application serveur HTTP

Dans Windows Server2016, la QoS basée sur la stratégie inclut la fonctionnalité stratégies basées sur une URL. Stratégies d’URL permettent de gérer la bande passante pour les serveurs HTTP.

De nombreuses applications d’entreprise sont développées pour hébergées sur les serveurs web Internet Information Services \(IIS\) et les applications Web sont accessibles à partir de navigateurs sur les ordinateurs clients.

Dans ce scénario, supposons que vous gérez un ensemble de serveurs IIS cet hôte des vidéos de formation pour les employés de l’ensemble de l’entreprise. Votre objectif est de vous assurer que le trafic à partir de ces serveurs vidéos ne surcharger votre réseau et vous assurer que le trafic vidéo se distingue de trafic de voix et des données sur le réseau. 

La tâche est similaire à la tâche dans le scénario 1. Vous concevez et configurer les paramètres de gestion du trafic, telles que la valeur DSCP pour le trafic vidéo, et la limitation de la même taux de comme vous le feriez pour les applications métier de. Mais lorsque vous spécifiez le trafic, au lieu de fournir le nom de l’application, vous entrez uniquement l’URL à laquelle votre application de serveur HTTP répondra: par exemple,https://hrweb/training.
  
> [!NOTE]
>Vous ne pouvez pas utiliser les stratégies de QoS basée sur une URL pour hiérarchiser le trafic réseau pour les ordinateurs exécutant des systèmes d’exploitation Windows antérieurs à Windows7 et Windows Server2008R2.

### <a name="precedence-rules-for-url-based-policies"></a>Règles de priorité pour les stratégies basées sur les URL

Toutes les URL suivantes sont valides et peuvent être spécifiés dans la stratégie de QoS et appliquées simultanément à un ordinateur ou un utilisateur:

- http://video

- https://training.hr.mycompany.com

- http://10.1.10.249:8080/tech  

- https://*/ebooks

Mais celui qui recevra la priorité? Les règles sont simples. Stratégies basées sur les URL sont prioritaires dans un ordre de lecture de gauche à droite. Par conséquent, à partir de la priorité la plus élevée à la priorité la plus basse, les champs d’URL sont:
  
[1. Schéma d’URL](#bkmk_QoS_UrlScheme)

[2. Hôte de l’URL](#bkmk_QoS_UrlHost)

[3. Port de l’URL](#bkmk_QoS_UrlPort)

[4. Chemin d’URL](#bkmk_QoS_UrlPath)

Détails sont les suivantes:

####  <a name="bkmk_QoS_UrlScheme"></a>1. schéma d’URL

 `https://` A une priorité supérieure à `http://`.

####  <a name="bkmk_QoS_UrlHost"></a>2. hôte de l’URL

 À partir de la priorité la plus élevée à la plus basse, ils sont:

1. Nom d’hôte

2. Adresse IPv6

3. Adresse IPv4

4. Caractère générique

Dans le cas du nom d’hôte, un nom d’hôte avec des éléments plus en pointillés (plus en détail) a une priorité supérieure à un nom d’hôte avec moins d’éléments en pointillés. Par exemple, entre les noms d’hôtes suivants:

- video.internal.training.hr.mycompany.com (profondeur = 6)
  
- selfguide.training.mycompany.com (profondeur = 4)
  
- Formation (profondeur = 1)
  
- Bibliothèque (profondeur = 1)
  
 **video.internal.training.hr.mycompany.com** a la priorité la plus élevée, et **selfguide.training.mycompany.com** a la priorité la plus élevée suivante. **Formation** et **bibliothèque** partagent la même priorité la plus basse.  
  
####  <a name="bkmk_QoS_UrlPort"></a>3. port de l’URL

Spécifique ou un numéro de port implicite a une priorité supérieure à un port de caractère générique.

####  <a name="bkmk_QoS_UrlPath"></a>4. chemin d’URL

Comme un nom d’hôte, un chemin d’accès de l’URL peut contenir plusieurs éléments. L’avec plus d’éléments toujours a une priorité plus élevée que celle avec moins. Par exemple, les chemins d’accès suivants sont répertoriés par priorité:  

1.  /eBooks/Tech/Windows/Networking/QoS

2.  / livres électroniques tech/windows /

3.  /ebooks

4.  /

Si un utilisateur choisit d’inclure tous les sous-répertoires et fichiers suivant un chemin d’URL, ce chemin d’accès de l’URL aura une priorité plus basse que cela aurait si le choix n’était pas réalisée.

Un utilisateur peut également spécifier une adresse IP de destination dans une stratégie basée sur une URL. L’adresse IP de destination a une priorité inférieure à l’un des quatre champs URL décrites précédemment.
  
### <a name="quintuple-policy"></a>Stratégie quintuple

Une stratégie Quintuple est spécifiée par l’ID de protocole, adresse IP source, port source, adresse IP de destination et port de destination. Une stratégie Quintuple a toujours une priorité plus élevée que toute stratégie basée sur une URL. 

Si une stratégie Quintuple est déjà appliquée pour un utilisateur, une nouvelle stratégie basée sur une URL n’entraîne pas conflits sur n’importe quel client de cet utilisateur ordinateurs.

Pour la rubrique suivante dans ce guide, voir [gérer la stratégie de QoS](qos-policy-manage.md).

Pour la première rubrique de ce guide, voir [stratégie de qualité de Service (QoS)](qos-policy-top.md).
