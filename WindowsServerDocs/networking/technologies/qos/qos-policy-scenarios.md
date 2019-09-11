---
title: Scénarios de stratégie de QoS
description: Cette rubrique fournit des scénarios de stratégie de qualité de service (QoS) qui montrent comment utiliser stratégie de groupe pour hiérarchiser le trafic réseau d’applications et de services spécifiques dans Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c4306f06-a117-4f65-b78b-9fd0d1133f95
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e0968157532c0b3bd926acbaff4291e27a71de31
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871872"
---
# <a name="qos-policy-scenarios"></a>Scénarios de stratégie de QoS

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour passer en revue les scénarios hypothétiques qui illustrent comment, quand et pourquoi utiliser la stratégie QoS.

Les deux scénarios de cette rubrique sont les suivants :

1. Hiérarchiser le trafic réseau pour une application métier
2. Définir la priorité du trafic réseau pour une application serveur HTTP

>[!NOTE]
>Certaines sections de cette rubrique contiennent les étapes générales que vous pouvez suivre pour effectuer les actions décrites. Pour obtenir des instructions plus détaillées sur la gestion de la stratégie QoS, consultez [gérer la stratégie QoS](qos-policy-manage.md).

## <a name="scenario-1-prioritize-network-traffic-for-a-line-of-business-application"></a>Scénario 1 : Hiérarchiser le trafic réseau pour une application métier

Dans ce scénario, un service informatique a plusieurs objectifs qu’il peut accomplir à l’aide de la stratégie QoS :

- Améliorez les performances du réseau\-pour les applications stratégiques.
- Améliorez les performances du réseau pour un ensemble d’utilisateurs de clés lorsqu’ils utilisent une application spécifique.
- Veillez à ce\-que l’application de sauvegarde des données de l’entreprise n’entrave pas les performances du réseau en utilisant trop de bande passante en même temps.

Le département informatique décide de configurer la stratégie de qualité de service pour hiérarchiser les applications spécifiques en \(utilisant\) les valeurs DSCP du point de code du service de différenciation pour classifier le trafic réseau et configurer ses routeurs pour fournir des conditions préférentielles. traitement pour le trafic avec une priorité plus élevée. 

>[!NOTE]
>Pour plus d’informations sur DSCP, consultez la section **définir la priorité de QoS par le biais d’un point de code de services différenciés** dans la rubrique [stratégie de qualité de service (QoS)](qos-policy-top.md).

Outre les valeurs DSCP, les stratégies de QoS peuvent spécifier un taux d’accélération. La limitation a pour effet de limiter tout le trafic sortant qui correspond à la stratégie de QoS à un taux d’envoi spécifique.

### <a name="qos-policy-configuration"></a>Configuration de la stratégie QoS

Avec trois objectifs distincts à accomplir, l’administrateur informatique décide de créer trois stratégies de QoS différentes.

#### <a name="qos-policy-for-lob-app-servers"></a>Stratégie de QoS pour les serveurs d’applications LOB

La première\-application critique pour laquelle le service informatique crée une stratégie de QoS est\-une application ERP\) de planification \(des ressources de l’entreprise. L’application ERP est hébergée sur plusieurs ordinateurs qui exécutent tous Windows Server 2016. Dans Active Directory Domain Services, ces ordinateurs sont \(membres d’une UO\) d’unité d’organisation qui a été créée pour les \(serveurs d'\) applications métier métier. Le composant\-côté client de l’application ERP est installé sur les ordinateurs qui exécutent Windows 10 et Windows 8.1.

Dans stratégie de groupe, un administrateur sélectionne l’objet de stratégie \(de\) groupe objet stratégie de groupe sur lequel la stratégie QoS sera appliquée. À l’aide de l’Assistant stratégie de QoS, l’administrateur informatique crée une stratégie de QoS appelée « stratégie d’entreprise serveur\-» qui spécifie une valeur DSCP de priorité élevée de 44 pour toutes les applications, toute adresse IP, TCP et UDP et le numéro de port.

La stratégie QoS est appliquée uniquement aux serveurs LOB en liant l’objet de stratégie de groupe à l’unité d’organisation qui contient uniquement ces \(serveurs\) , via l’outil console de gestion des stratégies de groupe GPMC. Cette stratégie d’entreprise de serveur initiale applique\-la valeur DSCP haute priorité chaque fois que l’ordinateur envoie le trafic réseau. Cette stratégie de QoS peut être modifiée \(ultérieurement dans l’outil\) éditeur d’objets stratégie de groupe pour inclure les numéros de port de l’application ERP, ce qui limite la stratégie à s’appliquer uniquement lorsque le numéro de port spécifié est utilisé.

#### <a name="qos-policy-for-the-finance-group"></a>Stratégie QoS pour le groupe finance

Si de nombreux groupes au sein de l’entreprise accèdent à l’application ERP, le groupe finance dépend de cette application lors de la gestion des clients et le groupe nécessite des performances élevées de l’application.

Pour vous assurer que le groupe finance peut prendre en charge ses clients, la stratégie QoS doit classer le trafic de ces utilisateurs en priorité élevée. Toutefois, la stratégie ne doit pas s’appliquer lorsque les membres du groupe finance utilisent des applications autres que l’application ERP. 

Pour cette raison, le service informatique définit une deuxième stratégie de QoS appelée « stratégie d’LOB client » dans l’outil de l’éditeur d’objets stratégie de groupe qui applique une valeur DSCP de 60 lorsque le groupe d’utilisateurs finance exécute l’application ERP.

#### <a name="qos-policy-for-a-backup-app"></a>Stratégie de QoS pour une application de sauvegarde

Une application de sauvegarde distincte s’exécute sur tous les ordinateurs. Pour garantir que le trafic de l’application de sauvegarde n’utilise pas toutes les ressources réseau disponibles, le service informatique crée une stratégie de données de sauvegarde. Cette stratégie de sauvegarde spécifie une valeur DSCP de 1 basée sur le nom de l’exécutable pour l’application de sauvegarde, qui est **Backup. exe**. 

Un troisième objet de stratégie de groupe est créé et déployé pour tous les ordinateurs clients du domaine. Chaque fois que l’application de sauvegarde envoie des données, la valeur DSCP de faible priorité est appliquée, même si elle provient d’ordinateurs du service finance.
  
>[!NOTE]
>Le trafic réseau sans stratégie de QoS envoie avec une valeur DSCP égale à 0.

### <a name="scenario-policies"></a>Stratégies de scénario

Le tableau suivant récapitule les stratégies de QoS pour ce scénario.
  
|Nom de la stratégie|Valeur DSCP|Taux d’accélération|Appliqué aux unités d’organisation|Description|  
|-----------------|----------------|-------------------|-----------------------------------|-----------------|
|[Aucune stratégie]|0|Aucun|[Aucun déploiement]|Meilleur effort (par défaut) pour le trafic non classifié.|  
|Données de sauvegarde|1|Aucun|Tous les clients|Applique une valeur DSCP de priorité basse pour ces données en bloc.|  
|LOB serveur|44|Aucun|Unité d’organisation ordinateur pour les serveurs ERP|Applique le DSCP haute priorité pour le trafic du serveur ERP|  
|LOB client|60|Aucun|Groupe d’utilisateurs finance|Applique le DSCP haute priorité pour le trafic client ERP.|  

>[!NOTE]
>Les valeurs DSCP sont représentées sous forme décimale.

Avec les stratégies de qualité de service définies et appliquées à l’aide de stratégie de groupe, le trafic réseau sortant reçoit la valeur DSCP spécifiée par la stratégie. Les routeurs fournissent ensuite un traitement différentiel basé sur ces valeurs DSCP à l’aide de la mise en file d’attente. Pour ce service informatique, les routeurs sont configurés avec quatre files d’attente : High-Priority, Middle-Priority, Best-effort et basse priorité.

Lorsque le trafic arrive au routeur avec les valeurs DSCP de « stratégie d’entreprise de serveur » et « stratégie d’entreprise client », les données sont placées dans des files d’attente à priorité élevée. Le trafic avec une valeur DSCP égale à 0 reçoit le niveau de service le plus approprié. Les paquets avec une valeur DSCP de 1 (de l’application de sauvegarde) reçoivent un traitement de faible priorité.  
  
### <a name="prerequisites-for-prioritizing-a-line-of-business-application"></a>Conditions préalables pour définir la priorité d’une application métier

Pour effectuer cette tâche, assurez-vous que les conditions suivantes sont respectées :

- Les ordinateurs impliqués exécutent des\-systèmes d’exploitation compatibles QoS.

- Les ordinateurs impliqués sont membres d’un domaine \(Active Directory Domain Services\) AD DS afin de pouvoir être configurés à l’aide de stratégie de groupe.

- Les réseaux TCP/IP sont configurés avec des routeurs configurés pour\)le protocole DSCP \(RFC 2474. Pour plus d’informations, consultez la [RFC 2474](https://www.ietf.org/rfc/rfc2474.txt).

- Les exigences en matière d’informations d’identification d’administration sont satisfaites.

#### <a name="administrative-credentials"></a>Informations d’identification d’administration

Pour effectuer cette tâche, vous devez être en mesure de créer et de déployer des objets stratégie de groupe.
  
#### <a name="setting-up-the-test-environment-for-prioritizing-a-line-of-business-application"></a>Configuration de l’environnement de test pour définir la priorité d’une application métier

Pour configurer l’environnement de test, effectuez les tâches suivantes.

- Créez un domaine AD DS avec les clients et les utilisateurs regroupés en unités d’organisation. Pour obtenir des instructions sur le déploiement de AD DS, consultez le [Guide du réseau de base](https://docs.microsoft.com/windows-server/networking/core-network-guide/core-network-guide).

- Configurez les routeurs pour la file d’attente différentielle en fonction des valeurs DSCP. Par exemple, la valeur DSCP 44 entre dans une file d’attente « Platinum » et toutes les autres sont pondérées en file d’attente.

>[!NOTE]
> Vous pouvez afficher les valeurs DSCP à l’aide de captures réseau avec des outils tels que Moniteur réseau. Une fois que vous avez effectué une capture réseau, vous pouvez observer le champ OT dans les données capturées.

#### <a name="steps-for-prioritizing-a-line-of-business-application"></a>Étapes de la hiérarchisation d’une application métier

Pour classer par ordre de priorité une application métier, effectuez les tâches suivantes :

1. Créez et liez un objet de \(stratégie\) de groupe objet stratégie de groupe avec une stratégie QoS.

2. Configurez les routeurs pour traiter de façon différentielle une application métier (à l’aide de la mise en file d’attente) en fonction des valeurs DSCP sélectionnées. Les procédures de cette tâche varient en fonction du type de routeurs que vous avez.

## <a name="scenario-2-prioritize-network-traffic-for-an-http-server-application"></a>Scénario 2 : Définir la priorité du trafic réseau pour une application serveur HTTP

Dans Windows Server 2016, la QoS basée sur la stratégie comprend les stratégies basées sur l’URL de la fonctionnalité. Les stratégies d’URL vous permettent de gérer la bande passante pour les serveurs HTTP.

De nombreuses applications d’entreprise sont développées pour et hébergées\) sur des serveurs Web IIS Internet Information Services \(, et les applications Web sont accessibles à partir de navigateurs sur des ordinateurs clients.

Dans ce scénario, supposons que vous gérez un ensemble de serveurs IIS qui hébergent des vidéos de formation pour tous les employés de votre organisation. Votre objectif est de vous assurer que le trafic provenant de ces serveurs vidéo ne sature pas votre réseau et que le trafic vidéo est différencié du trafic vocal et de données sur le réseau. 

La tâche est semblable à la tâche du scénario 1. Vous allez concevoir et configurer les paramètres de gestion du trafic, tels que la valeur DSCP pour le trafic vidéo, et le taux de limitation comme vous le feriez pour les applications métier. Toutefois, lorsque vous spécifiez le trafic, au lieu de fournir le nom de l’application, vous entrez uniquement l’URL à laquelle votre application serveur HTTP https://hrweb/training répondra : par exemple,.
  
> [!NOTE]
>Vous ne pouvez pas utiliser les stratégies QoS basées sur l’URL pour hiérarchiser le trafic réseau pour les ordinateurs exécutant des systèmes d’exploitation Windows qui ont été publiés avant Windows 7 et Windows Server 2008 R2.

### <a name="precedence-rules-for-url-based-policies"></a>Règles de précédence pour les stratégies basées sur des URL

Toutes les URL suivantes sont valides et peuvent être spécifiées dans la stratégie QoS et appliquées simultanément à un ordinateur ou à un utilisateur :

- https://video

- https://training.hr.mycompany.com

- https://10.1.10.249:8080/tech  

- https://*/ebooks

Mais quel est l’ordre de priorité ? Les règles sont simples. Les stratégies basées sur des URL sont classées par ordre de priorité dans un ordre de lecture de gauche à droite. Ainsi, de la priorité la plus élevée à la priorité la plus basse, les champs d’URL sont les suivants :
  
[1. Schéma d’URL](#bkmk_QoS_UrlScheme)

[2. Hôte d’URL](#bkmk_QoS_UrlHost)

[3. Port de l’URL](#bkmk_QoS_UrlPort)

[4. Chemin de l’URL](#bkmk_QoS_UrlPath)

Les détails sont les suivants :

####  <a name="bkmk_QoS_UrlScheme"></a>1,0. Schéma d’URL

 `https://`a une priorité plus élevée `https://`que.

####  <a name="bkmk_QoS_UrlHost"></a>2. Hôte d’URL

 De la priorité la plus élevée à la plus basse, il s’agit des éléments suivants :

1. Hostname

2. Adresse IPv6

3. Adresse IPv4

4. Caractère générique

Dans le cas du nom d’hôte, un nom d’hôte avec davantage d’éléments en pointillés (plus de profondeur) a une priorité plus élevée qu’un nom d’hôte avec moins d’éléments en pointillés. Par exemple, parmi les noms d’hôte suivants :

- video.internal.training.hr.mycompany.com (profondeur = 6)
  
- selfguide.training.mycompany.com (profondeur = 4)
  
- apprentissage (profondeur = 1)
  
- bibliothèque (profondeur = 1)
  
  **Video.Internal.Training.hr.mycompany.com** a la priorité la plus élevée et **selfguide.Training.mycompany.com** a la priorité la plus élevée suivante. La **formation** et la **bibliothèque** partagent la même priorité la plus basse.  
  
####  <a name="bkmk_QoS_UrlPort"></a>1,3. Port de l’URL

Un numéro de port spécifique ou implicite a une priorité supérieure à celle d’un port générique.

####  <a name="bkmk_QoS_UrlPath"></a>4. Chemin de l’URL

À l’instar d’un nom d’hôte, un chemin d’URL peut être constitué de plusieurs éléments. Celui avec plus d’éléments a toujours une priorité plus élevée que celle avec moins. Par exemple, les chemins d’accès suivants sont répertoriés par priorité :  

1.  /ebooks/tech/windows/networking/qos

2.  /ebooks/tech/windows/

3.  /ebooks

4.  /

Si un utilisateur choisit d’inclure tous les sous-répertoires et fichiers qui suivent un chemin d’URL, ce chemin d’URL aura une priorité inférieure à celle qu’il aurait si le choix n’a pas été fait.

Un utilisateur peut également choisir de spécifier une adresse IP de destination dans une stratégie basée sur une URL. L’adresse IP de destination a une priorité inférieure à celle de l’un des quatre champs d’URL décrits précédemment.
  
### <a name="quintuple-policy"></a>Stratégie quintuple

Une stratégie quintuple est spécifiée par l’ID de protocole, l’adresse IP source, le port source, l’adresse IP de destination et le port de destination. Une stratégie quintuple a toujours une priorité plus élevée que toute stratégie basée sur une URL. 

Si une stratégie quintuple est déjà appliquée pour un utilisateur, une nouvelle stratégie basée sur une URL n’entraîne pas de conflits sur les ordinateurs clients de cet utilisateur.

Pour la rubrique suivante de ce guide, consultez [gérer la stratégie QoS](qos-policy-manage.md).

Pour obtenir la première rubrique de ce guide, consultez [stratégie de qualité de service (QoS)](qos-policy-top.md).
