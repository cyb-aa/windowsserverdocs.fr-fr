---
title: Scénarios de stratégie de QoS
description: Cette rubrique fournit des scénarios de stratégie de qualité de Service (QoS), qui montrent comment utiliser la stratégie de groupe pour hiérarchiser le trafic réseau des applications spécifiques et des services dans Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c4306f06-a117-4f65-b78b-9fd0d1133f95
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2617c897ed2ea173d29fc7c4a87e52557154d463
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446992"
---
# <a name="qos-policy-scenarios"></a>Scénarios de stratégie de QoS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour passer en revue les scénarios hypothétiques qui illustrent comment, quand et pourquoi utiliser la stratégie de QoS.

Les deux scénarios dans cette rubrique sont :

1. Hiérarchiser le trafic réseau pour une Application Line-of-Business
2. Hiérarchiser le trafic réseau pour une Application de serveur HTTP

>[!NOTE]
>Certaines sections de cette rubrique contient les étapes générales à que suivre pour effectuer les actions décrites. Pour obtenir des instructions sur la gestion de stratégie de QoS, consultez [gérer la stratégie de QoS](qos-policy-manage.md).

## <a name="scenario-1-prioritize-network-traffic-for-a-line-of-business-application"></a>Scénario 1 : Hiérarchiser le trafic réseau pour une Application Line-of-Business

Dans ce scénario, un service informatique a plusieurs objectifs qu’ils peuvent accomplir en utilisant une stratégie de QoS :

- Offrir de meilleures performances réseau des missions\-applications critiques.
- Offrir de meilleures performances de réseau pour un jeu de clés des utilisateurs quand ils utilisent une application spécifique.
- Vérifiez que la société\-données à l’échelle des applications de sauvegarde n’entravent pas les performances du réseau en utilisant trop de bande passante en même temps.

Le service informatique décide de configurer la stratégie de QoS pour hiérarchiser les applications spécifiques à l’aide de Point de Code de Service de différenciation \(DSCP\) valeurs pour classer le trafic réseau et de configurer ses routeurs pour fournir préférentiel traitement pour le trafic de priorité plus élevé. 

>[!NOTE]
>Pour plus d’informations sur DSCP, consultez la section **définir QoS priorité via un différenciées Services Code Point** dans la rubrique [stratégie de qualité de Service (QoS)](qos-policy-top.md).

En plus des valeurs DSCP, les stratégies de qualité de service peuvent spécifier un taux d’accélération. La limitation a pour effet de limiter le trafic sortant correspond à la stratégie de QoS pour spécifiques à un taux d’envoi.

### <a name="qos-policy-configuration"></a>Configuration de la stratégie de QoS

Avec trois objectifs distincts pour accomplir, l’administrateur informatique décide de créer des trois différentes stratégies de qualité de service.

#### <a name="qos-policy-for-lob-app-servers"></a>Stratégie de QoS pour les serveurs d’applications métier

La première mission\-application critique pour lequel le département informatique crée une stratégie de QoS est une société\-large ERP \(ERP\) application. L’application ERP est hébergée sur plusieurs ordinateurs qui exécutent tous Windows Server 2016. Dans les Services de domaine Active Directory, ces ordinateurs sont membres d’une unité d’organisation \(unité d’organisation\) qui a été créé pour line-of-business \(LOB\) serveurs d’applications. Le client\-composant de côté pour l’application ERP est installé sur les ordinateurs qui exécutent Windows 10 et Windows 8.1.

Dans la stratégie de groupe, un administrateur sélectionne l’objet de stratégie de groupe \(GPO\) sur lequel la stratégie de QoS s’appliqueront. À l’aide de l’Assistant Stratégie de QoS, l’administrateur IT crée une stratégie de QoS appelée « serveur LOB stratégie » qui spécifie une valeur haute\-priorité valeur DSCP de 44 pour toutes les applications, n’importe quelle adresse IP, TCP et UDP et numéro de port.

La stratégie QoS est appliquée uniquement aux serveurs métier en liant l’objet de stratégie de groupe à l’unité d’organisation qui contient uniquement ces serveurs, via la Console de gestion de stratégie de groupe \(GPMC\) outil. Ce serveur initial LOB stratégie s’applique à la haute\-priorité DSCP valeur chaque fois que l’ordinateur envoie le trafic réseau. Cette stratégie de QoS ultérieurement modifiables \(dans l’outil Éditeur d’objets de stratégie de groupe\) pour inclure les numéros de port de l’application ERP, ce qui limite la stratégie pour appliquer uniquement lorsque le numéro de port spécifié est utilisé.

#### <a name="qos-policy-for-the-finance-group"></a>Stratégie de QoS pour le groupe Finance

Alors que de nombreux groupes au sein de la société accéder à l’application ERP, le groupe finance dépend de cette application lorsque vous traitez des clients, et le groupe requiert des performances élevées à partir de l’application.

Pour vous assurer que le groupe finance peut prendre en charge leurs clients, la stratégie de QoS doit classifier le trafic de ces utilisateurs avec une priorité élevée. Toutefois, la stratégie doit s’applique pas lorsque les membres du groupe finance utiliser les applications autres que l’application ERP. 

Pour cette raison, le département informatique définit une deuxième stratégie de qualité de service appelée « Client LOB stratégie » dans l’outil Éditeur d’objets de stratégie de groupe qui applique une valeur DSCP de 60 lorsque le groupe d’utilisateurs de finance s’exécute l’application ERP.

#### <a name="qos-policy-for-a-backup-app"></a>Stratégie de QoS pour une application de sauvegarde

Une application de sauvegarde distincte est en cours d’exécution sur tous les ordinateurs. Pour garantir que le trafic de l’application de sauvegarde n’utilise pas toutes les ressources réseau disponibles, le département informatique crée une stratégie de sauvegarde de données. Cette stratégie de sauvegarde spécifie une valeur DSCP basé sur le nom de l’exécutable de l’application de sauvegarde, ce qui est de 1 **backup.exe**. 

Un troisième objet stratégie de groupe est créé et déployé pour tous les ordinateurs clients dans le domaine. Chaque fois que l’application de sauvegarde envoie des données, la valeur DSCP de faible priorité est appliquée, même s’il provient d’ordinateurs du département finance.
  
>[!NOTE]
>Il envoie le trafic réseau sans une stratégie de QoS avec une valeur DSCP 0.

### <a name="scenario-policies"></a>Stratégies de scénario

Le tableau suivant récapitule les stratégies de QoS pour ce scénario.
  
|Nom de la stratégie|Valeur DSCP|Taux d’accélération|Appliqués aux unités d’organisation|Description|  
|-----------------|----------------|-------------------|-----------------------------------|-----------------|
|[Aucune stratégie]|0|Aucune|[Aucun déploiement]|Meilleur traitement d’effort (valeur par défaut) pour le trafic non classifié.|  
|Données de sauvegarde|1|Aucune|Tous les clients|Applique une valeur DSCP de faible priorité pour ces données en bloc.|  
|Serveur LOB|44|Aucune|Ordinateur unité d’organisation pour les serveurs d’ERP|S’applique DSCP de haute priorité pour le trafic du serveur ERP|  
|Client LOB|60|Aucune|Groupe d’utilisateurs de Finance|S’applique DSCP de haute priorité pour le trafic de client ERP|  

>[!NOTE]
>Les valeurs DSCP sont représentées sous forme décimale.

Des stratégies QoS sont définies et appliquées à l’aide de stratégie de groupe, le trafic réseau sortant reçoit la valeur DSCP stratégie spécifiée. Traitement différencié en fonction de ces valeurs DSCP à l’aide de queuing ensuite fournissant des routeurs. Pour ce service informatique, les routeurs sont configurés avec quatre files d’attente : haute priorité, middle-priorité, meilleur effort et une priorité basse.

Lorsque le trafic arrive sur le routeur avec les valeurs DSCP de « serveur LOB stratégie » et « Client LOB stratégie, « les données sont placées en file d’attente de priorité élevée. Le trafic avec une valeur DSCP 0 reçoit un meilleur effort niveau de service. Les paquets avec une valeur DSCP de 1 (à partir de l’application de sauvegarde) reçoivent un traitement de faible priorité.  
  
### <a name="prerequisites-for-prioritizing-a-line-of-business-application"></a>Conditions préalables pour la hiérarchisation d’une application line-of-business

Pour effectuer cette tâche, assurez-vous que les conditions suivantes :

- Les ordinateurs concernés sont en cours d’exécution QoS\-des systèmes d’exploitation compatibles.

- Les ordinateurs impliqués sont membres d’un Active Directory Domain Services \(AD DS\) domaine afin qu’ils peuvent être configurés à l’aide de stratégie de groupe.

- Les réseaux TCP/IP sont configurés avec des routeurs configurés pour DSCP \(RFC 2474\). Pour plus d’informations, consultez [RFC 2474](https://www.ietf.org/rfc/rfc2474.txt).

- Informations d’identification administratives sont satisfaites.

#### <a name="administrative-credentials"></a>Informations d’identification d’administration

Pour effectuer cette tâche, vous devez être en mesure de créer et déployer des objets de stratégie de groupe.
  
#### <a name="setting-up-the-test-environment-for-prioritizing-a-line-of-business-application"></a>Configuration de l’environnement de test pour la hiérarchisation d’une application line-of-business

Pour configurer l’environnement de test, effectuez les tâches suivantes.

- Créer un domaine AD DS avec les clients et utilisateurs regroupés dans des unités d’organisation. Pour obtenir des instructions sur le déploiement d’AD DS, voir la [Guide du réseau](https://docs.microsoft.com/windows-server/networking/core-network-guide/core-network-guide).

- Configurez les routeurs à différentielle file d’attente en fonction des valeurs DSCP. Par exemple, la valeur DSCP 44 entre dans une file d’attente de « Platinum » et tous les autres sont pondérées-équitable-en file d’attente.

>[!NOTE]
> Vous pouvez afficher les valeurs DSCP à l’aide de captures réseau avec des outils comme le Moniteur réseau. Une fois que vous effectuez une capture réseau, vous pouvez observer le champ TOS dans les données capturées.

#### <a name="steps-for-prioritizing-a-line-of-business-application"></a>Étapes pour la hiérarchisation d’une application line-of-business

Pour hiérarchiser une application line of business, effectuez les tâches suivantes :

1. Créer et lier un objet de stratégie de groupe \(GPO\) avec une stratégie de QoS.

2. Configurer les routeurs pour traiter de façon différentielle line of business application (en utilisant queuing) basée sur les valeurs DSCP sélectionnées. Les procédures de cette tâche varie en fonction du type de routeurs que vous avez.

## <a name="scenario-2-prioritize-network-traffic-for-an-http-server-application"></a>Scénario 2 : Hiérarchiser le trafic réseau pour une Application de serveur HTTP

Dans Windows Server 2016, QoS basée sur la stratégie inclut la fonctionnalité de stratégies basées sur des URL. URL de stratégies permettent de gérer la bande passante pour les serveurs HTTP.

De nombreuses applications d’entreprise sont développées pour et hébergées sur Internet Information Services \(IIS\) serveurs web et les applications Web sont accessibles à partir des navigateurs sur les ordinateurs clients.

Dans ce scénario, supposons que gérer un ensemble de serveurs IIS de cet hôte des vidéos de formation pour les employés de l’ensemble de l’entreprise. Votre objectif est de vous assurer que le trafic à partir de ces serveurs vidéo ne sont pas surcharger votre réseau et vous assurer que le trafic vidéo se différencie du trafic voix et de données sur le réseau. 

La tâche est similaire à la tâche dans le scénario 1. Vous allez concevoir et configurer les paramètres de gestion du trafic, telles que la valeur DSCP pour le trafic de vidéo, et le taux de régulation le même comme vous le feriez pour les applications line-of-business. Mais lorsque vous spécifiez le trafic, au lieu de fournir le nom de l’application, vous entrez uniquement l’URL à laquelle votre application de serveur HTTP répondra : par exemple, https://hrweb/training.
  
> [!NOTE]
>Vous ne pouvez pas utiliser des stratégies de QoS basé sur l’URL pour hiérarchiser le trafic réseau pour les ordinateurs exécutant des systèmes d’exploitation Windows antérieurs à Windows 7 et Windows Server 2008 R2.

### <a name="precedence-rules-for-url-based-policies"></a>Règles de priorité pour les stratégies basées sur des URL

Toutes les URL suivantes sont valides et peuvent être spécifiés dans la stratégie de QoS et appliquées simultanément sur un ordinateur ou un utilisateur :

- https://video

- https://training.hr.mycompany.com

- https://10.1.10.249:8080/tech  

- https://*/ebooks

Mais celle qui reçoit la priorité ? Les règles sont simples. Stratégies basées sur les URL sont hiérarchisées dans un ordre de lecture de gauche à droite. Par conséquent, à partir de la priorité la plus élevée à la priorité la plus basse, les champs URL sont :
  
[1. Schéma d’URL](#bkmk_QoS_UrlScheme)

[2. Hôte de l’URL](#bkmk_QoS_UrlHost)

[3. Port de l’URL](#bkmk_QoS_UrlPort)

[4. Chemin d’URL](#bkmk_QoS_UrlPath)

Détails sont les suivants :

####  <a name="bkmk_QoS_UrlScheme"></a> 1. Schéma d’URL

 `https://` a une priorité plus élevée que `https://`.

####  <a name="bkmk_QoS_UrlHost"></a> 2. Hôte de l’URL

 À partir de la priorité la plus élevée à la plus basse, elles sont :

1. Hostname

2. Adresse IPv6

3. Adresse IPv4

4. Caractère générique

Dans le cas du nom d’hôte, un nom d’hôte avec des éléments plus en pointillé (plus de détails) a une priorité supérieure à un nom d’hôte avec moins d’éléments en pointillés. Par exemple, parmi les noms d’hôtes suivants :

- video.internal.training.hr.mycompany.com (depth = 6)
  
- selfguide.training.mycompany.com (depth = 4)
  
- formation (profondeur = 1)
  
- bibliothèque (profondeur = 1)
  
  **Video.Internal.Training.hr.mycompany.com** a la priorité la plus élevée, et **selfguide.training.mycompany.com** a la priorité la plus élevée suivante. **Formation** et **bibliothèque** partagent la même priorité la plus basse.  
  
####  <a name="bkmk_QoS_UrlPort"></a> 3. Port de l’URL

Un ou un numéro de port implicite a une priorité supérieure à un port de caractère générique.

####  <a name="bkmk_QoS_UrlPath"></a> 4. Chemin d’URL

Comme un nom d’hôte, un chemin d’URL peut se composer de plusieurs éléments. Celui avec plus d’éléments a toujours une priorité plus élevée que celle d’inférieure. Par exemple, les chemins d’accès suivants sont répertoriés par priorité :  

1.  /ebooks/tech/windows/networking/qos

2.  /ebooks/tech/windows/

3.  /eBooks

4.  /

Si un utilisateur choisit d’inclure tous les sous-répertoires et fichiers de la suite d’un chemin d’URL, ce chemin d’accès de l’URL a une priorité plus faible que vous avez si le choix n’ont pas été apportée.

Un utilisateur peut également choisir de spécifier une adresse IP de destination dans une stratégie basée sur une URL. L’adresse IP de destination a une priorité plus faible que l’un des quatre champs URL décrits précédemment.
  
### <a name="quintuple-policy"></a>Stratégie quintuple

Une stratégie Quintuple est spécifiée par l’ID de protocole, adresse IP source, port source, adresse IP de destination et port de destination. Une stratégie Quintuple a toujours une priorité plus élevée que toute stratégie basé sur l’URL. 

Si une stratégie Quintuple est déjà appliquée à un utilisateur, une nouvelle stratégie basé sur l’URL ne sera pas provoquer des conflits sur n’importe quel client de l’utilisateur les ordinateurs.

Pour la rubrique suivante de ce guide, consultez [gérer la stratégie de QoS](qos-policy-manage.md).

Pour la première rubrique de ce guide, consultez [stratégie de qualité de Service (QoS)](qos-policy-top.md).
