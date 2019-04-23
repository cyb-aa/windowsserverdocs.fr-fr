---
title: Guide de l’utilisateur de Server Performance Advisor
description: Guide de l’utilisateur de Server Performance Advisor
ms.assetid: be99a5a9-b9c0-436b-912c-e5c5839a533d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: manage
ms.openlocfilehash: 97fab1a36db2e903b3a909bee9e2f1748f7841fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834140"
---
# <a name="server-performance-advisor-users-guide"></a>Guide de l’utilisateur de Server Performance Advisor

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Windows 8

Ce guide de l’utilisateur pour Microsoft Server Performance Advisor (SPA) fournit des instructions sur l’utilisation de SPA pour identifier les goulots d’étranglement dans les systèmes déployés dans les différents rôles de serveur.

SPA peut vous aider avec les opérations suivantes :

* Gérer les performances de votre serveur et résoudre les problèmes de performances de serveur.

* Fournir des rapports de données et des recommandations sur la configuration commune et des problèmes de performances.

* Recommandations best Practice locaux basés sur les données collectées.

> [!NOTE]
> La Console de SPA ne fait pas de toutes les modifications vers les serveurs.

Pour plus d’informations sur le développement de packs d’Advisor de SPA, consultez [Server Performance Advisor Pack Development Guide](server-performance-advisor-pack-development-guide.md).

## <a name="server-performance-advisor-overview"></a>Vue d’ensemble de Server Performance Advisor


Cette section explique l’historique de SPA décrit son public cible et les scénarios et présente une vue d’ensemble architecturale de l’outil.

### <a name="history-of-spa"></a>Historique de SPA

La version d’origine de Microsoft Server Performance Advisor (SPA) a été publiée en 2005-2006. La solution destinée principalement Windows Server 2003, y compris le système d’exploitation principal et les rôles de serveur tels que Internet Information Services (IIS) et active directory. L’objectif de l’outil est pour vous aider à évaluer les performances de votre serveur et de résoudre les problèmes de performances de serveur.

SPA fournit des rapports de données et des recommandations pour les administrateurs système sur les problèmes de performances et de configuration courante. SPA regroupe les données liées aux performances de diverses sources sur les serveurs, par exemple, les compteurs de performances, clés de Registre, WMI requêtes, les fichiers de configuration et suivi d’événements pour Windows (ETW). Selon les données de performances du serveur qu’il collecte, SPA peut fournir des explications approfondies sur la situation de performances de serveur actuelle et des recommandations de problème sur ce qui peut être amélioré.

Il y a eu des téléchargements plus d’un million de l’outil SPA d’origine, et cela a permis à de nombreux administrateurs système de gérer les performances de leurs serveurs. Il a été un outil de résolution des problèmes de performances très réussie.

Depuis la version de l’application SPA d’origine, il existe plusieurs modifications majeures dans le monde de performances du serveur. Plus particulièrement, la version de Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008, avec les nouvelles versions correspondantes des rôles de serveur tels que les Services Internet (IIS) 8.5. Les versions plus récentes de Server Performance Advisor étendent la fonctionnalité de l’application SPA d’origine pour les systèmes d’exploitation de serveur récents et les rôles de serveur.

Bien que SPA 3.1 partage les mêmes objectifs de conception et d’un paradigme d’analyse de performances en tant que l’outil d’origine, il a été réécrit avec les dernières technologies. Il présente également les améliorations clées suivantes :

* Peut effectuer une analyse les serveurs exécutant Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008.

* Prend en charge la fonctionnalité d’analyse à distance, qui permet de 3.1 SPA collecter et analyser des données à partir d’une console centrale, sans avoir besoin d’installer du code sur les serveurs à analyser.

* Utilise Microsoft SQL Server pour l’analyse de données et de stockage, ce qui permet de traiter et de stocker de grandes quantités de données.

* L’outil sépare les packs d’Advisor. Logique d’analyse de performances pour chaque rôle de serveur est publiée sous la forme d’un pack d’advisor, qui peut être publié ou mis à jour séparément à partir de l’outil.

* Fournit les packs d’Advisor sont développées dans les scripts SQL avec une architecture ouverte. Les parties non-Microsoft peuvent développer des packs d’advisor ou étendre les packs d’advisor existant à partir de Microsoft pour couvrir les besoins particuliers.

* Prend en charge les nouvelles fonctionnalités telles que les rapports de comparaison de côte à côte, tendance historique les graphiques et à vous aider à trouver les anomalies.

* Fournit des fonctionnalités telles que la modification, importation et exportation des seuils pour vous aider à bien paramétrer les rapports et les notifications et de partagent ces réglages avec d’autres utilisateurs SPA.

* Prend en charge de plusieurs projets, ce qui peuvent être utilisées pour regrouper les serveurs cibles.

* Fournit périodique collecte des données à partir de la console de SPA.

* Permet des requêtes personnalisées et génération de rapports à l’aide de Microsoft SQL Server (pour les utilisateurs expérimentés).

* Applets de commande Windows PowerShell personnalisées sont disponibles à utiliser avec SPA

* Compatibilité descendante pour l’importation et affichage des rapports à partir d’une base de données SPA 3.0

### <a name="target-audience"></a>Public visé

L’outil SPA est principalement conçu pour les administrateurs système qui gèrent moins de 100 serveurs dans différents rôles de serveur. Il peut également utilisé par les ingénieurs du support technique pour collecter des données de performances et à résoudre les problèmes de performances pour les clients.

SPA propose des recommandations pour vous aider à résoudre les problèmes de performances ; Toutefois, il est basé sur des hypothèses qui ne sont pas applicables à votre environnement de serveur spécifique. Les recommandations qui sont offerts par SPA doivent être considérées des suggestions. Nous pensons que les utilisateurs SPA avoir une bonne compréhension de la configuration de système, cas d’usage et l’impact du réglage du système sur le comportement de l’ensemble du système. Les administrateurs système doivent décider si les recommandations de SPA doivent être appliquées à leur environnement.

### <a href="" id="what-s-new-in-spa-3-1-"></a>Nouveautés nouveautées de SPA 3.1 ?

Les fonctionnalités et améliorations suivantes ont été ajoutées dans SPA 3.1 :

* Active directory module Advisor vous aide à analyser les performances générales du rôle de serveur pour les Services de domaine active directory server s.

* Prise en charge aux développeurs d’ajouter l’alerte de notifications d’événements ETW perdue dans les packs d’advisor et le fait de rendre visible l’alerte quand un rapport a été généré et les événements ont été perdus en raison de la journalisation de fréquence élevée sur lente ou lourdement conflits disque

### <a name="target-scenarios"></a>Scénarios cibles

Voici les scénarios cibles pour SPA :

* **Environnement de serveur**

    SPA est conçu pour configurer et maintenir facilement. Il s’applique aux environnements de serveur qui utilisent des serveurs de 1 à 100. SPA n’évolue pas bien si vous tentez de gérer les performances de plus de 100 serveurs. Pour les environnements plus grand ou plus sophistiquées, vous devez envisager d’utiliser System Center Operations Manager.

* **Résolution des problèmes de performances**

    Vous pouvez utiliser SPA comme un outil de résolution des problèmes de performances. Permet de collecter des données de performances de haut niveau, il effectue une traitement sur les données pour vous aider à mieux comprendre leur comportement de l’ensemble du système de publication approfondie, et il signale toute anomalie. Lorsque vous suspectez un problème de performances par un client, vous pouvez utiliser SPA pour collecter et analyser les données de performances à partir du serveur.

    SPA génère un rapport que vous pouvez afficher. Rapports SPA fournissent une liste de notification qui met en évidence les problèmes potentiels et une section de données qui inclut des différents index de performances, les configurations et les paramètres pour le serveur. Vous pouvez utiliser ce rapport pour identifier le problème de performances et les recommandations pour trouver des solutions pour le problème. Rapports peuvent être comparées avec d’autres rapports qui ont été générées à un autre moment ou par un autre serveur. À l’aide de cette comparaison côte à côte, vous pouvez déterminer les différences entre la ligne de base normale par rapport à un comportement anormal.

    **Remarque** SPA n’est pas conçu pour être un débogage ou l’outil de contrôle. En outre, du point de vue des serveurs, il peut être considéré comme un outil en lecture seule, et elle ne modifie pas les configurations de serveurs.

     

* **Analyse des performances des index**

    Vous pouvez utiliser SPA pour surveiller l’index de performances des serveurs. Vous pouvez choisir d’exécuter régulièrement des SPA sur les serveurs pour collecter des données de performances, puis exécutez un graphique de tendances ou un graphique d’historique pour détecter les anomalies. Vous pouvez afficher le rapport pour une analyse particulière, découvrez plus d’informations sur le problème de performances, puis utiliser les recommandations ou autres données de rapport pour résoudre le problème.

### <a name="spa-architecture"></a>Architecture SPA

La logique de collecte de données SPA repose sur un protocole dans Windows appelé des journaux de performances et alertes (PLA). PLA permet aux programmes de collecter des données de performances à partir de serveurs locaux ou distants, tels que les compteurs de performances, le requêtes WMI, traces ETW, clés de Registre et les fichiers de configuration. Lors de l’application à page unique s’exécute l’analyse des performances sur un serveur cible, il crée un ensemble de collecteurs de données PLA basé sur le Pack de conseiller SPA spécifique que vous avez sélectionné. Le pack de l’Assistant contient la source de données à collecter et le partage de fichiers où les journaux doivent être stockés. Collecte des données SPA stocke uniquement un seul compte d’utilisateur ; le même compte d’utilisateur utilisé par PLA est également utilisé pour écrire les journaux.

SPA utilise une base de données SQL Server pour stocker les journaux de performances sont collectées à partir de serveurs cibles. SPA importe tous les journaux de performances à partir du partage de fichiers à la base de données, puis utilise la logique d’analyse de données à l’intérieur de chaque module advisor pour traiter les données et génèrent les rapports. Un pack d’advisor analyse les données de performances qui a été collecté à partir des serveurs cibles et génère les rapports SPA. Pour plus d’informations sur la création d’un pack d’advisor, consultez [Server Performance Advisor Pack Development Guide](server-performance-advisor-pack-development-guide.md).

Les interfaces utilisateur de console SPA et les interactions sont générées dans le cadre de la SPAConsole.exe. Vous pouvez utiliser la console pour créer une base de données, ajouter ou supprimer des packs d’advisor, gérer les serveurs cibles, exécuter l’analyse des performances et afficher des rapports de performances.

La console SPA permettre s’exécuter sur les systèmes d’exploitation suivants :

* Windows 8.1

* Windows 8

* Windows 7

* Windows Server 2012 R2

* Windows Server 2012

* Windows Server 2008 R2

* Windows Server 2008

Dans une application métier classique, il existe trois niveaux : la couche de présentation, la couche de logique métier et la couche de stockage. SPA est conçu comme un deux niveau produit de la console et la base de données. La console constitue une couche de présentation avec une certaine logique liés au processus inclus et la base de données sert de la couche de stockage et de la couche de logique métier. La console capture l’entrée de l’utilisateur, et qu’il contrôle les étapes de collecte de données, de traitement des données et de génération de rapports. SPA ne dépend pas de services de système de Windows.

Le diagramme suivant illustre l’architecture de haut niveau du système SPA. Le processus est :

1.  À partir de la console de l’application à page unique, vous exécutez l’analyse des performances sur les serveurs spécifiés.

2.  Lorsque les données de performances sont collectées, PLA sur les serveurs cibles écrit les journaux pour le partage de fichiers spécifié par l’ensemble de collecteurs de données.

3.  Une fois que la collecte de données est terminée sur un ordinateur cible, la console SPA importe les journaux à la base de données SQL Server.

4.  La console appelle la logique de traitement des données du module advisor spécifique.

5.  Le pack de l’Assistant traite les données et appelle les API de SPA pour insérer des enregistrements dans les tables de rapport système qui sont définies par l’infrastructure d’application à page unique.

6.  Vous pouvez utiliser la visionneuse de rapports à l’intérieur de la console pour afficher les rapports générés. Pour vous aider à résoudre les problèmes de performances, SPA fournit trois types de rapports : unique des rapports, rapports côte à côte et de tendance ou graphiques historiques. Pour plus d’informations sur les rapports, consultez [affichage des rapports](#bkmk-viewingreports).

![architecture de SPA](../media/server-performance-advisor/spa-user-manual-architecture.png)

## <a name="getting-started-with-spa"></a>Prise en main SPA


### <a name="requirements"></a>Configuration requise

La console de l’application à page unique peut être installée sur Windows 8.1, Windows 8, Windows 7, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008. SPA en cours d’exécution sur des versions antérieures du système d’exploitation Windows Server n’est pas pris en charge. SPA s’exécute sur x86 ou x64, mais il ne prend pas en charge les architectures IA64 ou ARM.

SPA repose sur Windows Presentation Foundation (WPF) 2.0, qui fait partie du Microsoft .NET Framework 4, .NET Framework 4 est donc requis.

Vous devez également installer SQL Server 2008 R2 Express sur le même ordinateur où SPA est installé. SQL Server 2008 R2 Express est un moteur de base de données gratuite est publié par Microsoft. Vous pouvez télécharger Microsoft SQL Server 2008 R2 Express à partir du Microsoft Download Center. Nous vous suggérons de SQL Server 2008 R2 Express, les versions plus récentes de SQL Server peuvent également être compatibles avec SPA.

**Remarque** SPA n’inclut pas SQL Server ou le .NET Framework en tant que partie du package d’installation SPA. Après avoir installé Microsoft SQL Server 2008 R2 et .NET Framework 4.0, nous vous recommandons d’exécuter Windows Update avant d’installer l’application à page unique.

 

Étant donné que les utilisateurs peuvent créer et gérer des bases de données avec l’application à page unique, le compte d’utilisateur qui est utilisé pour exécuter l’application à page unique doit avoir les mêmes privilèges d’administrateur que SQL Server.

### <a href="" id="bkmk-setupspa"></a>Configuration d’application à page unique

SPA est empaqueté comme un fichier .cab qui inclut tous les fichiers binaires pour le framework SPA, les applets de commande Windows PowerShell qui sont utilisés dans les scénarios avancés, et compresse le conseiller suivant : Core du système d’exploitation, Hyper-V, active directory et IIS. Après avoir extrait le fichier .cab dans un dossier, aucune installation supplémentaire n’est nécessaire. Toutefois, pour exécuter l’application à page unique, vous devez activer la collecte des données à partir des serveurs cibles comme suit :

* Pour exécuter la collecte de données PLA, le compte d’utilisateur que vous utilisez pour exécuter la console SPA doive faire partie du groupe de sécurité Administrateurs sur le serveur cible. Si le serveur cible et la console se trouvent dans le même domaine, le compte d’utilisateur de domaine doit faire partie du groupe de sécurité Administrateurs sur le serveur cible. Si le serveur cible et la console ne sont pas dans le même domaine, créez un compte d’utilisateur administratif sur le serveur cible avec le même nom d’utilisateur et le mot de passe en tant que le compte d’utilisateur que vous utilisez pour exécuter la console SPA.

* Créer un dossier partagé pour les résultats sur le serveur.

* Assurez-vous que le compte d’utilisateur que vous utilisez pour exécuter la console SPA a lu et autorisations en écriture au dossier partagé. PLA utilise ce compte pour écrire les journaux dans le dossier.
La console SPA utilise le même compte pour lire les journaux et les importer dans la base de données.

    **Remarque** ETW implémente un tampon circulaire pour stocker la trace et les déplace vers le dossier partagé quand cela est possible. Si le serveur est occupé ou l’opération d’écriture est lente, ETW supprime les traces lorsque la mémoire tampon est pleine. Il est IMPORTANT que le dossier partagé se trouve sur un serveur avec un accès d’e/s rapide. Nous recommandons que chaque serveur cible dispose d’un dossier partagé pour minimiser la perte de données due au fait d’e/s de fichier lente.

     

* Pour PLA accéder aux serveurs cibles, définir le pare-feu Windows pour autoriser l’à distance des journaux de performances et alertes sur les serveurs cibles. PLA utilise le port TCP 139.

* Assurez-vous que le **les journaux de performances et alertes** Service est en cours d’exécution.

* Si le serveur de console et cible est situé dans des sous-réseaux différents, vous devez également définir le champ d’adresse IP à distance dans les règles de pare-feu entrantes dans le **étendue** paramètres sur le **des journaux et alertes** page, comme illustré ici.

    ![propriétés de pla](../media/server-performance-advisor/spa-user-manual-pla-firewall.png)

* Activer la découverte du réseau sur la console et sur chacun des serveurs cibles.

* Si le serveur cible n’est pas joint à un domaine, activez le paramètre de Registre suivant : **HKLM\\SOFTWARE\\Microsoft\\Windows\\Currentversion\\Policies\\system\\LocalAccountTokenFilterPolicy**.

**Remarque** par défaut, SPA écrit des journaux de diagnostic dans le dossier où se trouve SpaConsole.exe. Si SPA est installée dans le dossier Program Files, SPA n’est en mesure d’écrire le journal lorsque SpaConsole.exe est exécuté en tant qu’administrateur.

Si vous souhaitez exécuter l’analyse de données par rapport à l’ordinateur de la console, vous devez exécuter l’application à page unique en tant qu’administrateur. PLA traverse un chemin d’accès du code différent lors de l’exécution sur un ordinateur local, ce qui nécessite des privilèges d’administrateur.

Si vous souhaitez exécuter le Pack d’Advisor SPA IIS sur vos serveurs, vous devez activer la requête WMI et suivi ETW pour le serveur IIS. Ce faire, vous pouvez activer **suivi** sous le **intégrité et Diagnostics** service de rôle et **gestion des Scripts et outils IIS** sous **outils de gestion**  de la **serveur Web (IIS)** rôle de serveur.

 

### <a name="creating-your-first-project"></a>Créer votre premier projet

Une fois que tout est configuré, vous pouvez créer votre premier projet SPA. Comme décrit dans la section précédente, SPA stocke tout ce qui est lié à l’analyse de données de performances à l’intérieur d’une base de données. Chaque projet SPA correspond à une base de données. Le **utilisez l’Assistant pour la première fois** vous guide à travers les étapes suivantes.

**Pour créer un projet SPA**

1.  Lancez SpaConsole.exe. La console entre un mode déconnecté, où SPA n’est pas connecté à une base de données, et la fenêtre principale est vide.

2.  Pour créer un nouveau projet, cliquez sur **fichier**, puis cliquez sur **nouveau projet**. Cette action lance la première fois, utilisez l’Assistant. La première page affiche les étapes à suivre lors de l’utilisation de l’Assistant :

    * création d'une base de données ;

    * Les packs d’approvisionner advisor

    * Ajouter des serveurs à la liste des serveurs cibles

3.  Cliquez sur **Suivant**. Le **créer la base de données de projet** page vous invite à fournir le nom de l’instance de Microsoft SQL Server où vous souhaitez créer votre base de données. Par exemple, si elle se trouve sur le même ordinateur que la console, vous pouvez utiliser **localhost\\&lt;le nom de votre serveur SQL&gt;**.

    **Remarque** le nom de l’instance par défaut pour une installation de SQL Server 2008 R2 Express est SQLExpress. Pour une instance de SQL Server 2008 R2 Express qui est installé sur l’ordinateur local, la base de données est en général par défaut **localhost\\SQLExpress**. Toutefois, il peut avoir été modifié pendant l’installation de SQL Server, vous devez vous assurer que vous utilisez le nom d’instance SQL Server approprié.

     

4.  Indiquez le nom de la base de données. Uniquement des lettres, des chiffres et des traits de soulignement (\_) sont autorisés comme caractères valides pour un nom de base de données. Une suggestion raisonnable pour le nom de la base de données SPA serait **SPA**. Si vous entrez un nom non valide, une icône d’erreur rouge s’affiche. L’info-bulle associée indique la raison de l’échec de validation.

    **Remarque** il est IMPORTANT de se rappeler le nom de la base de données et le nom d’instance de serveur, car il s’agit uniquement des identificateurs pour votre projet. Vous devez fournir ces informations si vous souhaitez basculer vers cette base de données.

     

5.  Une fois que vous fournissez le nom d’instance de serveur et le nom de la base de données, la première fois. Utilisez l’Assistant génère l’emplacement du fichier de base de données.

6.  Sur le **créer la base de données de projet** , cliquez sur **suivant**. La première fois, utilisez l’Assistant crée une base de données et génère tout schéma de base de données liés SPA, les fonctions et les procédures stockées dans la base de données. Cette étape peut prendre plusieurs secondes selon la vitesse du matériel et du réseau.

    **Remarque** si cette étape échoue, un message d’erreur s’affiche. Certains problèmes courants sont : Console ne peut pas se connecter à l’instance de SQL Server, des privilèges suffisants pour créer la base de données, ou le nom de la base de données existe déjà.

     

7.  Lorsque l’étape précédente réussit, vous voyez la **approvisionner module advisor** page. Il répertorie tous les packs d’advisor sont disponibles sur votre ordinateur. SPA analyse automatiquement le dossier nommé **APs** sous le répertoire racine SPA. Il répertorie le nom complet, version et auteur pour chaque pack d’advisor.

    **Remarque** pour plus d’informations sur la façon dont le nom complet et la version sont utilisés dans SPA, consultez [la gestion des packs d’advisor](#bkmk-manageadvisorpacks)

     

8.  Choisissez les packs d’advisor vous souhaitez approvisionner dans la base de données de projet, puis cliquez sur **suivant**. Vous pouvez également cliquer sur **Skip** passer à l’étape suivante sans mise en service de tous les packs d’advisor.

    **Remarque** vous pouvez approvisionner les packs d’advisor à tout moment à l’aide de l’outil. Pour plus d’informations, consultez [la gestion des packs d’advisor](#bkmk-manageadvisorpacks).

     

9.  Sur le **ajouter des serveurs** page, pour chaque serveur à ajouter à la liste des serveurs cibles, il existe deux champs obligatoires pour remplir : **Nom du serveur** et **emplacement du partage de fichier**.

    **Remarque** il existe également un **Remarque** champ, qui est principalement utilisé pour classifier ou de trouver le serveur. Dans les cas où vous avez de nombreux serveurs, vous pouvez importer un fichier de valeurs (.csv) séparés par des virgules qui contient le nom du serveur, le dossier des résultats et le champ de commentaire facultatif. Le **Remarque** champ est utilisé pour décrire le serveur et le terme peut être utilisé pour filtrer les serveurs de collecte de données. Si vous initialisez les serveurs via le fichier .csv, une erreur d’analyse dans le fichier ne charge pas les serveurs.

     

10. Plusieurs configurations doivent être définies pour activer la collecte de données PLA, comme décrit dans [Configurer SPA](#bkmk-setupspa). Le **ajouter serveur** page fournit une fonctionnalité de configuration de test pour vous aider à résoudre les problèmes de configuration. Activez la case à cocher associée à l’ordinateur, puis cliquez sur **tester la connectivité**. SPA tente de générer un ensemble sur les serveurs cibles de collecteurs de données, et il essaie d’importer les résultats dans la base de données. Si tout est correct, le **état** montre **passer**. En cas d’échec, une info-bulle s’affiche qui décrit la raison de l’échec.

11. Chacun des serveurs est automatiquement ajouté à la base de données, même si elle ne passe pas le test de configuration. Pour supprimer des serveurs de la liste, sélectionnez le nom du serveur, puis cliquez sur **supprimer**.

12. Lorsque tout est terminée, cliquez sur **Terminer** pour fermer la première fois, utilisez l’Assistant. Si vous fermez la première utilisation de l’Assistant avant la fin il, toutes les étapes précédentes sont conservées et aucun d'entre eux restaurer automatiquement. Vous devez apporter des modifications ultérieures manuellement.

## <a name="running-analysis"></a>Exécution de l’analyse


Après avoir configuré la base de données, vous pouvez exécuter l’analyse des performances sur les serveurs.

Chaque lancement de la console de l’application à page unique, le dernier projet qui a été utilisé par l’utilisateur actuel s’ouvre automatiquement. La fenêtre principale contient une liste de serveurs. Chaque serveur possède quatre propriétés : Nom du serveur, résultat de l’analyse, état actuel et Remarque.

* **Nom du serveur** le nom du serveur, qui est l’identificateur de serveur. Aucun nom en double n’est autorisés.

* **Résultat de l’analyse** par défaut, il affiche le résultat de l’analyse des performances dernière exécution sur le serveur. S’il n’a pas été des analyses de performances exécuté sur le serveur, elle s’affiche **aucun rapport**. Si un avertissement est déclenché par le rapport, il affiche **avertissement** et l’horodatage de la dernière génération du rapport. Si aucun problème n’a été trouvé lors de l’analyse la plus récente sur le serveur, il affiche **OK** et l’horodatage.

    **Remarque** si vous avez modifié récemment un paramètre système, nous vous recommandons d’exécuter l’analyse pour évaluer l’impact global de la modification et obtenir un rapport mis à jour sur l’état du système. SPA ne suit pas les modifications de configuration pour le système testé.

     

* **État actuel** affiche l’état des performances des tâches d’analyse en cours d’exécution sur le serveur. Vous pouvez annuler une tâche en cours d’exécution en cliquant sur le **Annuler** icône, qui est désigné par un X rouge.

* **Remarque** décrit le serveur cible en cours. Par exemple, vous pouvez décrire votre serveur à l’aide du rôle de serveur (par exemple, SQL Server) ou un emplacement (par exemple, Kent). SPA utilise le **nom du serveur** et **Remarque** pour aider à rechercher et de trouver le serveur approprié. Vous pouvez taper dans la zone de texte de recherche. Si le **nom du serveur** ou **Remarque** colonnes contient la chaîne exacte que vous avez entré dans la zone de recherche, le serveur s’affiche dans la liste des serveurs.

Les contrôles suivants sont également disponibles sur la console :

* **Répétez** une case à cocher qui décrit la possibilité de répéter régulièrement une collection, selon un intervalle de temps. Pour la plupart des installations de serveur, vous pouvez disposer d’une collection de SPA répéter toutes les heures pour ont un historique suffisant pour l’analyse. Si vous souhaitez exécuter une seule fois la collection, vous ne devez pas sélectionner le **répétez** case à cocher.

* **supprimer la périodicité** un bouton qui vous permet d’annuler une difficulté répéter la tâche de la collection. Il annule la collection de répétition, mais pas dans la collection actuelle (le cas échéant) est en cours d’exécution. Cette option permet de réinitialiser un nouvel intervalle de répétition de collection ou d’exécuter manuellement de la collection.

Avant de commencer l’analyse des performances, sélectionnez la durée de collecte de données. Bien que la collecte des données permet de fournir une image plus précise de la situation de performances du serveur, il génère également un plus grand nombre de journaux, et il peut avoir plus d’impact potentiel sur le serveur. Choisissez la durée de la collecte de données selon vos besoins spécifiques. Chaque pack d’advisor définit une durée minimale valide. La durée de collecte de données que vous choisissez doit être plus longue que la durée minimale des packs d’advisor sélectionné.

Pour exécuter l’analyse des performances sur les serveurs cibles, sélectionnez les serveurs que vous souhaitez exécuter l’analyse des performances sur, puis cliquez sur ** Exécuter l’analyse ** une boîte de dialogue s’affiche et vous demande de sélectionner les packs d’advisor à exécuter sur les serveurs. Les packs d’advisor sélectionné s’exécuter simultanément. Les rapports sont générés sont basés sur les données de performances collectées au cours de la même période.

**Remarque** si vous sélectionnez un serveur doté d’une abonnement en cours d’exécution, l’analyse des performances la **supprimer la périodicité** bouton vous permet d’annuler la collecte de données périodique. SPA n’autorise pas plusieurs sessions de collection de données en même temps sur le même ordinateur.

 

## <a href="" id="bkmk-viewingreports"></a>Affichage des rapports


Dans une seule page, il existe trois types de rapport d’analyse des performances : Rapport unique, rapport côte à côte et tendance et graphiques historiques.

Après l’analyse de performances en cours d’exécution, un rapport est généré pour chacun des packs d’advisor exécuter sur l’ordinateur cible. Dans la liste de serveur dans la fenêtre principale, vous pouvez développer **résultat de l’analyse** pour voir tous les packs de conseiller qui ont été exécutées sur le serveur spécifié. Vous pouvez cliquer sur un nom de rapport pour afficher un rapport unique.

Il existe trois icônes en regard du nom de pack de conseiller qui indiquent l’état de la dernière analyse exécutée sur le serveur :

* Le **dernière** icône indique que le rapport a été généré par l’analyse des performances plus récente sur ce serveur pour le pack de l’Assistant.

* Le **trouver** icône affiche la liste des performances des rapports d’analyse, ce qui vous permet de choisir le rapport approprié. Le **module Advisor** et **serveur cible** champs sont préremplis avec les conseiller pack et cible server informations récentes. L’intervalle de temps par défaut est définie sur une semaine, et la date de fin est définie aujourd'hui. Si vous cliquez sur le **recherche** bouton dans le coin supérieur droit, vous pouvez obtenir une liste de tous les rapports d’analyse de performances pour le pack de serveur et le conseiller sélectionné dans l’intervalle de temps.

* Le **afficher les graphiques** icône ouvre la tendance et la vue graphique d’historique.

L’illustration suivante montre le **dernière**, **trouver**, et **afficher les graphiques** icônes après chaque module advisor :

![icônes de rapport](../media/server-performance-advisor/spa-user-manual-report-icons.png)

### <a name="searching-for-and-within-reports"></a>Recherche d’et dans les rapports

Recherche de rapports s’effectue à l’aide de **Explorateur de rapports**. Cela vous permet de rechercher des rapports par plage de dates, nom du serveur et l’Assistant pack. Il s’agit de la méthode recommandée pour rechercher un rapport dignes d’intérêt, autre que la dernière exécution de rapports. La dernière exécution de rapports est disponible via **afficher le rapport** pour ce serveur.

Lorsque vous affichez un rapport spécifique, vous pouvez facilement accéder au rapport suivant et précédent par heure ou consulter un rapport connexe, comme un autre point d’accès en cours d’exécution en même temps. Ces options sont disponibles sous **Actions**.

Il est également possible de recherche au sein d’un rapport. Un nombre de rapports ont une **trouver** zone de recherche de chaîne disponible pour la recherche de chaîne de texte rapide dans le rapport. Pour supprimer la zone de texte, vous pouvez l’ignorer. Pour activer une zone de recherche (dans windows dont le texte de recherche), vous pouvez utiliser le contrôle + F contextuel. Le **trouver** zone permet à l’utilisateur spécifier une recherche respectant la casse selon les besoins du **respecter la casse** option.

L’illustration suivante montre le **trouver** zone de recherche avec la chaîne **Power** sur le **rapport** onglet.

![la zone de recherche de recherche](../media/server-performance-advisor/spa-user-manual-find-search-box.png)

### <a name="single-report"></a>Rapport unique

Un seul rapport montre les résultats d’analyse de performances à partir d’une seule exécution d’un pack d’advisor sur un seul ordinateur. Le rapport affiche le nom du pack d’advisor, le nom du serveur cible, l’heure le rapport généré et la durée de collecte de données.

Un seul rapport contient une section de notification et les sections de données.

### <a name="notification-section"></a>Section de notification

La section notification se compose d’un ensemble de règles d’analyse de performances. Chaque notification contient une source de données, certaines valeurs de seuil et une logique métier. Lorsque vous exécutez l’analyse des performances, la logique métier évalue les sources de données aux seuils pour déterminer si la règle est ou non. Si ce n’est pas le cas, un avertissement s’affiche pour vous informer sur un problème de performances potentiels. Il fournit également des recommandations pour vous aider à résoudre le problème. La section notification est toujours le premier onglet dans la vue de rapport unique.

La section de la notification est divisée en deux parties : **Avertissement** et **autres notifications**.

Si la source de données pour une règle répond à certaines conditions selon les paramètres de logique et seuil, un avertissement s’affiche dans le **avertissement** zone. Un avertissement comprend les parties suivantes :

* Une icône d’avertissement indique l’existence d’un problème potentiel.

* Nom de la règle. Par exemple, **réseau recevoir des paquets gouttes** est un lien qui pointe vers la page Détails de la règle, comme décrit dans [la gestion des packs d’advisor](#bkmk-manageadvisorpacks).

* Une description simple sur le problème potentiel.

* Une recommandation pour une solution possible au problème de performances potentiels.

Des serveurs différents peuvent avoir des modèles d’utilisation et de configuration très différente, et il est impossible de définir les seuils et les règles qui s’appliquent à tous les serveurs dans toutes les conditions. SPA vous permet de modifier les seuils. Vous pouvez également choisir de désactiver une règle si la règle ne s’applique pas à votre scénario. Par défaut, toutes les règles sont activées. Une règle désactivée n’affiche pas dans la zone de notification. Pour plus d’informations, consultez [la gestion des packs d’advisor](#bkmk-manageadvisorpacks).

Le **autres notifications** zone contient toutes les autres règles, où aucun avertissement n’est déclenché ou que la règle n’est pas applicable. Il contient des parties similaires tel que figurant dans le **avertissement** zone. La plus grande différence est que si aucun avertissement n’est pas déclenché la règle n’est pas applicable, généralement aucune recommandation n’est fournie.

### <a name="data-sections"></a>Sections de données

Les sections de données contiennent les données de performances qui génère le pack de l’Assistant basées sur les données brutes collectées sur les serveurs cibles. Les sections de données incluent un ensemble de sections de niveau supérieur et plusieurs niveaux de sous-sections. Les sections de niveau supérieur sont présentées sous forme d’onglets. Les sous-sections suivantes sous les sections de niveau supérieur sont présentés dans les zones pouvant être développés. Vous pouvez réduire ou développer chacune des sections pour aider à se concentrer sur le domaine d’intérêt, comme indiqué dans l’illustration suivante.

![sections de données](../media/server-performance-advisor/spa-user-manual-data-sections.png)

Le Pack d’Advisor SPA principale du système d’exploitation et le Pack de l’Assistant IIS SPA contiennent un **vue d’ensemble du système** section. Cette section inclut les informations de niveau supérieur sur la configuration et l’utilisation des ressources. Autres sections de niveau supérieur représentent les zones de données de performances. SPA présente les données de rapport comme suit :

* **Valeur unique** une paire clé/valeur. La clé est une chaîne qui représente la signification de la valeur. La valeur peut être une chaîne, une valeur numérique ou une valeur booléenne. Cela est souvent utilisé pour afficher des informations statiques, telles que l’architecture de la configuration, par exemple, le processeur, la taille totale de la mémoire et la version du BIOS, qui ne changent pas au fil du temps.

* **valeur de liste** c’est parfois une paire clé/valeur, mais la valeur de la liste peut contenir plusieurs champs. Par exemple, l’attribut de l’UC peut être affiché dans une table avec plusieurs colonnes et plusieurs lignes. Chaque ligne représente une UC, et chaque colonne représente un attribut de l’UC.

* **Statistiques** peut être considéré comme un type spécial de valeur unique. Il peut uniquement contenir des données numériques. Pendant le délai de collecte de données, de nombreux points de données numériques fluctuent au lieu de rester constante. Par exemple, l’utilisation du processeur change chaque fois que le PLA collecte le compteur de performances. Avec une seule valeur ne peut pas refléter précisément le problème de performances. Au lieu d’afficher qu’une seule valeur, valeur moyenne, maximum, minimum et 90 % sont utilisés pour ces points de données numériques dynamique. La valeur 90 % représente l’activité supérieure ou égale à 90e centile entre tous les événements de ce compteur dans cet intervalle de collection donnée.

* **Liste du haut** contient généralement les plus gros consommateurs d’une ressource spécifique ou les entités principales qui a subi certains événements. Par exemple, **traite les 10 principales en termes d’utilisation moyenne du processeur** comprend les processus de dix principaux avec l’utilisation du processeur moyenne la plus élevée pendant la durée de collecte de données. Étant donné que l’utilisation du processeur est également un point de données numérique dynamique, d’autres statistiques telles que le maximum, minimum et 90 % value sont également inclus dans la liste pour donner à l’utilisateur une image plus complète de la consommation du processeur.

Comme indiqué dans les sections précédentes, SPA s’appuie sur PLA collecter ETW trace WMI requêtes, les compteurs de performances, clés de Registre et les fichiers de configuration pour générer le rapport. Il est IMPORTANT de comprendre la source de données derrière chaque point de données dans le rapport. SPA fournit ces informations via les info-bulles. Vous pouvez pointer sur les colonnes clés ou les lignes pour afficher l’info-bulle de la source de données. Par exemple, **WMI:Win32\_DisDrive : légende** signifie que la source de données provient d’une requête WMI, le nom de classe WMI Win32\_lecteur de disque et la propriété est **légende**.

### <a href="" id="side-by-side-report-"></a>Rapport de côte à côte

Seul les rapports fournissent des notifications et une section de données pour aider l’utilisateur recherche éventuels problèmes de performances, mais il est souvent difficile d’identifier un problème potentiel de performances directement dans un rapport unique. Un seul rapport peut contenir trop de points de données, ce qui rend difficile de trouver les problèmes potentiels.

Pour résoudre ce problème, SPA permet de comparer deux rapports. Vous pouvez comparer un rapport avec un problème potentiel à un rapport de référence pour aider à trouver les différences.

Rapports de côte à côte peuvent être lancement à partir d’une visionneuse de rapports-unique. Les utilisateurs peuvent cliquer **Actions**, puis cliquez sur **comparer les rapports** pour sélectionner les rapports. Il est uniquement pertinent de comparer les rapports à partir du même pack de l’Assistant. Vous pouvez choisir de comparer le rapport avec un rapport précédent dans le temps, le rapport suivant dans le temps ou un rapport arbitraire qui est sélectionné par le biais des fonctionnalités de recherche. Par exemple, pour isoler un comportement anormal, vous pouvez comparer un rapport de serveur de base à un rapport qui a été généré à un autre moment sur le même ordinateur ou à un rapport qui a été généré sur un autre ordinateur ayant un rôle de serveur similaire et charger.

Un rapport côte à côte ressemble beaucoup à l’état unique. Il contient une section de notification et de sections de données. Il contient le même nombre de notifications et des sections de données que la visionneuse de rapports unique. La seule différence est que les rapports sont affichés de manière côte à côte. Chaque section contient les données à partir de la source (état 1) et la destination (état 2). Le rapport côte à côte affiche le nom du pack d’advisor, le nom du serveur cible (rapport 1 sur la gauche) et rapport 2 sur la droite, l’heure à laquelle le rapport a été généré et la durée de la collecte de données pour chaque rapport.

Si vous ignorez la **trouver** boîte de dialogue, vous pouvez le réactiver en tapant CTRL + F. Cette boîte de dialogue recherche et met en évidence les chaînes de texte dans la section en cours.

Dans la section notification, si un des résultats des deux rapports qui sont comparées est un avertissement, il est répertorié dans le **avertissement** zone. Sinon, les résultats sont répertoriées dans le **autres Notifications** zone. Étant donné que la clé pour un rapport côte à côte consiste à identifier les différences entre les rapports, aucune des informations détaillées sur une règle ne s’affiche. Les utilisateurs peuvent cliquer sur le nom de la règle pour afficher le formulaire de détail de règle pour plus d’informations sur la règle.

Dans les sections de données, les données sont présentées de manière côte à côte avec les données de rapport 1 sur la gauche et les données de rapport 2 sur la droite. SPA montre des valeurs uniques dans la même table, mais au lieu de l’étiquetage des colonnes **valeur**, ils sont nommés **Report 1** et **Report 2** respectivement. Le rapport de côte à côte affiche toutes les autres formes de données dans les tables côte à côte.

La visionneuse de rapports de côte-à-côte fournit également des info-bulles sur la source de données.

### <a name="historical-and-trend-charts"></a>Graphiques d’historique et de tendance

Il convient uniquement pour afficher la tendance et graphiques historiques pour un serveur spécifique et d’un pack de conseiller spécifique. Vous devez choisir l’intervalle de temps (ce qui est défini par défaut à la dernière semaine), puis cliquez sur **OK** pour afficher la tendance et la visionneuse de graphique historique.

Le graphique d’historique et de tendance visionneuse comporte trois onglets : graphique historique, graphique de tendance de 24 heures et graphique de tendance de 7 jours.

### <a name="historical-chart"></a>Graphique historique

Le graphique historique affiche une série de valeurs pour un point de données numériques via le laps de temps donné. Par exemple, le **latence moyenne des demandes** pour IIS sur un serveur unique au cours des 15 derniers jours. Chaque point de données dans un graphique d’historique représente la valeur d’une source de données spécifiques effectuée dans une session d’analyse de performances.

Il existe deux façons d’utiliser un graphique historique :

1.  Pour aider à trouver des anomalies à une certaine heure pour un point de données par exemple, à 2 h 00 chaque jour, le **latence moyenne des demandes** d’IIS accède à partir de 200 ms à 500 ms.

2.  Pour aider à mettre en corrélation plusieurs points de données. Par exemple, en affichant **latence moyenne des demandes** et **nombre de demandes moyenne** ensemble au cours des 15 derniers jours. Le rapport peut indiquer que la latence des requêtes et l’augmentation du nombre demande au moment même endroit, ce qui peut indiquer que l’augmentation de latence de requête est due à une augmentation de nombre de demandes.

Dans un graphique d’historique, les utilisateurs peuvent les opérations suivantes :

* Afficher plusieurs séries de données dans la zone de graphique. Chaque série de données est affiché comme un graphique en courbes dans la visionneuse de rapports. Chaque graphique en courbes est automatiquement mis à l’échelle pour s’ajuster à la visionneuse de rapports.

* Ajouter ou supprimer une série de données à partir de la liste de séries de données en bas de la visionneuse graphique historique.

* Afficher ou masquer une série de données dans la liste de séries de données. Les utilisateurs peuvent cliquer sur une série de données spécifique dans la liste pour mettre en évidence le graphique de ligne correspondante dans la zone de graphique.

* Permet d’agrandir une certaine période en sélectionnant la période de temps à l’intérieur de la zone de graphique. Pour effectuer un zoom arrière, cliquez sur le bouton qui se trouve dans le coin inférieur gauche du graphique.

* Examiner un seul rapport en double-cliquant sur un point de données particulier.

* Copier les données et le rendre disponible pour d’autres programmes, tels que Microsoft Excel. Cela vous permet d’utiliser les fonctionnalités, lorsque cela est approprié de création de graphiques de Microsoft Excel.

### <a name="trend-charts"></a>Graphiques de tendances

Nombreux problèmes de performances répétitives sont dus à des tâches périodiques en cours d’exécution sur, ou sur les serveurs cibles. Par exemple, un site Web orienté sur le divertissement peut obtenir des correspondances plus pendant le week-end, ou une tâche de sauvegarde de disque planifiée peut-être interrompre les performances d’un serveur de tous les jours à 2 h 00.

Un graphique de tendance est conçu pour vous aider à trouver de tels problèmes de performances. Problèmes de performances peuvent se produire de façon répétée dans divers modèles. Les modèles les plus courants sont les modèles quotidiennes et hebdomadaires où les problèmes de performances se produisent pendant la même heure d’un jour ou le jour même de la semaine. Par conséquent, le SPA fournit un graphique de tendances de 24 heures et un graphique de tendances de 7 jours.

Le graphique d’analyse de tendance fournit un niveau plus profond d’enquête sur un jeu de données, et il recherche des tendances reposant sur l’heure du jour. L’axe des abscisses sont définie sur une période de 24 heures, en commençant à 0:00 (minuit) et se termine à 23:59. SPA n’affiche pas les tendances de plusieurs séries de données en même temps. Vous pouvez cliquer sur **choisir des séries de données** pour sélectionner une série de données à afficher.

Pour traiter les données, SPA recherche tous les instantanés pris entre 0:00 et 0:59 pour chaque heure. SPA détermine le minimum, maximum, moyenne et les valeurs sigma pour l’ensemble des captures instantanées effectuées pendant cette heure et les graphiques en tant que les graphiques en bougies. SPA répète le processus pour des captures instantanées créées entre 1:00 à 1 h 59, puis 2:00 à 2 h 59 et ainsi de suite. Si aucun instantané n’existe pour l’heure donnée, SPA laisse cette heure vide sur le graphique et passe à l’heure suivante.

Un graphique de tendances de 7 jours est très similaire au graphique de tendance de 24 heures. La seule différence est qu’il regroupe une série de données en fonction du jour de la semaine au lieu de l’heure d’un jour.

La série de données que vous sélectionnez dans la tendance et de graphiques historiques est stockée sous la forme d’une préférence utilisateur. La prochaine fois que le graphique d’historique et de tendance visionneuse est ouverte pour le même pack d’advisor, le même ensemble de la série de données sont répertoriés en tant que la valeur par défaut.

## <a name="managing-reports"></a>La gestion des rapports


### <a name="deleting-reports"></a>suppression de rapports

Les rapports peuvent être supprimés pour réduire le nombre de rapports qui doivent être gérés par SPA. Selon la fréquence des rapports et le nombre de serveurs, nous vous recommandons de supprimer les rapports inutiles. Bien que SPA n’a pas une limite sur les rapports qu’il peut gérer, la base de données sous-jacente peut avoir une limite de taille.

**Remarque** rapports supprimés ne peut pas être annulées.

 

### <a name="exporting-and-importing-reports"></a>Exportation et importation de rapports

Rapports peuvent être exportés dans un fichier XML au transport pour une autre console SPA ou à la messagerie électronique à un autre utilisateur. Exportation du rapport ne supprime pas le rapport. Pour exporter le rapport actuellement affiché, à partir de **visionneuse de rapports**, cliquez sur **Actions**, puis cliquez sur **exporter**. Pour exporter plusieurs rapports, à partir de **Explorateur de rapports**, cliquez sur **sélection Activer plusieurs**, sélectionnez plusieurs rapports à partir de la zone de sélection, puis cliquez sur **exporter**. Cette opération exporte les rapports au format XML dans le répertoire de destination sélectionnée.

Un rapport exporté sont consultables dans l’application à page unique. les rapports importés ne sont pas ajoutés à la base de données d’application à page unique. Ils sont principalement destinés à servir d’une application de visionneuse XML pour le rapport exporté. Le serveur pour le rapport importé n’a pas besoin d’avoir les mêmes packs advisor installés en tant que la console SPA le rapport d’origine.

## <a href="" id="bkmk-manageadvisorpacks"></a>Gestion des packs d’advisor


SPA inclut les packs de conseiller pour le système d’exploitation principal, Hyper-V, active directory et IIS. SPA fournit une architecture ouverte pour le développement de packs d’advisor à l’aide de SQL, il est donc également possible pour les développeurs non Microsoft générer des versions des packs d’advisor. Il existe quatre options pour la gestion d’un pack d’advisor : configurer, personnaliser, réinitialiser ou supprimer.

### <a name="provision-new-advisor-packs"></a>Approvisionner de nouveaux packs de conseiller

Nouveaux packs advisor peuvent être libérés par Microsoft ou par les développeurs non Microsoft. Un conseiller contient un provisionMetaData.xml et un ensemble de scripts SQL qui décrivent la logique.

**Pour configurer un nouveau pack d’advisor**

1.  copier tout le contenu du pack d’advisor sous le *SpaRoot %*\\répertoire de points d’accès.

2.  Dans la fenêtre principale, cliquez sur **Configuration**, puis cliquez sur **configurer les packs d’Advisor**. Le **configurer les packs d’Advisor** boîte de dialogue s’ouvre.

    **Remarque** cette boîte de dialogue est similaire à la **approvisionner module advisor** page dans la première fois, utilisez l’Assistant. Il affiche une liste des packs d’advisor qui sont disponibles pour gérer. Chaque pack d’advisor dans la liste a des propriétés telles que nom, installé la version, la version et auteur. Nom est le nom complet du pack d’advisor, et la version installée est la version de ce pack d’advisor a déjà été configurée dans le projet. Si le pack de l’Assistant n’est pas configuré dans la base de données actuelle, la zone de texte version installée affiche **pas installé**. Le champ version indique la version de ce pack d’advisor est classé sous le dossier de packs de l’Assistant.

     

3.  Sélectionnez le pack d’advisor dans la liste. Si le pack de l’Assistant n’a pas été configuré ou s’il existe une version plus récente dans le dossier de packs de conseiller celui qui figure dans la base de données, le **approvisionner** bouton est activé. Cliquez sur le **approvisionner** bouton.

4.  Lors de l’approvisionnement est terminé, le **version installée** champ pour le pack d’advisor sélectionné contient les nouvelles informations de version.

### <a name="customize-advisor-packs"></a>Personnaliser les packs d’advisor

SPA définit une architecture ouverte qui permet aux utilisateurs de modifier les packs d’advisor. Les utilisateurs peuvent modifier les fichiers de pack de conseiller en modifiant les seuils, partage les seuils et l’activation ou la désactivation des règles.

Pour plus d’informations sur comment modifier et générer des packs d’advisor, consultez [Server Performance Advisor Pack Development Guide](server-performance-advisor-pack-development-guide.md).

### <a name="changing-thresholds"></a>Modification de seuils

Seuils dans SPA servent à déterminer si la condition de déclenchement d’une règle est remplie. Les seuils réels pour des scénarios clients peut varier considérablement en raison de la charge de travail, l’environnement de matériel et entreprise a besoin. Les seuils par défaut ne peuvent pas être appropriés pour le cas d’utilisateur actuel, afin de SPA offre la possibilité de modifier le seuil existant.

Vous pouvez modifier les valeurs de seuil en cliquant sur le nom de la règle dans un rapport unique ou côte à côte. Ou pour gérer toutes les règles d’un pack d’advisor particulier, ils peuvent modifier les valeurs de seuil à partir de la **Configuration** menu.

**Pour modifier une valeur de seuil**

1.  Dans le **Configuration** menu, cliquez sur **configurer les packs d’Advisor**, cliquez sur le nom du pack d’advisor à modifier, puis cliquez sur **configurer**.

    **Remarque** vous sont présentées avec une liste de toutes les règles qui sont inclus dans le pack de l’Assistant. La case à cocher à gauche du nom du pack de l’Assistant indique si la règle est activée. Si une règle est désactivée, il est masqué à partir de tous les rapports.

     

2.  Cliquez sur la règle spécifique que vous souhaitez modifier. Le **détails de la règle** forment la règle sélectionnée s’ouvre.

L’écran de détails de règle contient des informations détaillées sur une règle spécifique. Il inclut le nom, description, état, résultats possibles et les seuils. SPA prend en charge deux types de résultats de la règle, **avertissement** et **OK**. Pour chaque type, il est le texte de recommandations et une recommandation.

Certaines règles n’ont pas les seuils définis. Par exemple, le **HTTP Keep Alive** règle recherche d’un paramètre booléen pour IIS. Par conséquent, la liste de seuils peut être vide. Sinon, tous les seuils qui sont utilisés par la règle actuelle est répertoriée. Une description détaillée sur l’utilisation d’un seuil dans la règle est incluse dans le cadre de la description.

Si le **détails de la règle** formulaire est lancé à partir de la **Configuration** menu, la liste de seuil comporte trois colonnes : nom de paramètre d’origine et modifier les paramètres. Si elle est lancée à partir d’un rapport unique ou côte à côte, les valeurs de seuil qui sont utilisées par le rapport sont également être inclus. Les utilisateurs peuvent modifier les valeurs de seuil actuel en modifiant la valeur dans **modifier paramètre** colonne, puis en cliquant sur **enregistrer** pour enregistrer les modifications apportées à la base de données.

Toutes les modifications apportées aux seuils est uniquement être appliquée aux rapports qui sont générés après les modifications. Les rapports existants ne sont pas affectés par ces modifications.

### <a name="sharing-thresholds"></a>Partage des seuils

Si vous gérez vos serveurs dans des situations similaires, vous pouvez choisir d’utiliser le même ensemble de seuils. Vous pouvez exporter et importer des seuils d’un pack d’advisor spécifique à l’aide de la **Configuration** menu. Vous pouvez sélectionner le pack de conseiller spécifique, puis cliquez sur **configurer**. Le fichier exporté de seuil est au format XML.

Lorsque vous importez un seuil, SPA valide le format de fichier XML et vérifie que le fichier correspond à du pack de conseiller sélectionné. Si l’opération réussit, SPA importe toutes les valeurs à partir du fichier de seuil dans la base de données de projet actuel. Comme pour le scénario de seuils de changement précédent, toutes les modifications de valeur de seuil prendront effet que sur des rapports qui sont générés à l’avenir. Les rapports existants ne sont pas affectés.

### <a name="enable-or-disable-rules"></a>Activer ou désactiver des règles

Une règle peut être activée ou désactivée à partir de la **détails de la règle** formulaire. Vous devez cliquer sur **enregistrer** pour conserver les modifications apportées. Si une règle est désactivée, il doit ne pas être affiché dans des rapports. Mais la logique métier sous-jacente est déclenchée lors de la génération du rapport, donc lorsque vous choisissez de réactiver la règle, il s’affiche dans les rapports à nouveau.

### <a name="reset-advisor-packs-to-original-state"></a>Réinitialiser les packs d’advisor à l’état d’origine

Vous pouvez décider de modifier un pack d’advisor configuré dans la base de données. Autre que de modifier les seuils, vous pouvez également modifier les scripts SQL. SPA ne prend pas en charge la modification métadonnées du rapport, ajouter ou supprimer des règles d’un pack d’advisor approvisionné. Modification manuelle de ces domaines peut entraîner un comportement inattendu.

Pour plus d’informations sur la modification d’un pack de conseiller approvisionné, consultez le [Server Performance Advisor Pack Development Guide](server-performance-advisor-pack-development-guide.md).

Si vous souhaitez restaurer les modifications qui ont été effectuées sur un pack de conseiller approvisionné, vous pouvez choisir de réinitialiser le pack de l’Assistant. Cela remplace tous les scripts SQL qui sont associés au pack de conseiller et réinitialiser toutes les valeurs de seuils par défaut. Cela permet de conserver tous les rapports existants.

Réinitialiser le pack d’advisor est possible à l’aide de la **configurer les packs d’Advisor** formulaire. Vous devez sélectionner le pack de conseiller pour être réinitialisé, puis cliquez sur **réinitialiser**.

### <a name="remove-advisor-packs"></a>supprimer des packs d’advisor

Quand un pack d’advisor n’est plus nécessaire, les utilisateurs peuvent le supprimer à partir de la base de données. suppression du pack de l’Assistant supprime toutes les informations sur le pack de l’Assistant à partir de la base de données, y compris les règles et les seuils, tous les scripts SQL et tous les rapports. Aucune des actions peuvent être annulées.

suppression du pack de l’Assistant peut être effectuée à l’aide de **configurer les packs d’Advisor** formulaire. Vous devez sélectionner le pack de conseiller à supprimer, puis cliquez sur **Deprovision**.

### <a name="update-existing-advisor-packs"></a>Mettre à jour les packs d’advisor existant

La mise à jour les packs d’advisor existant est très similaire à la réinitialisation du pack de conseiller à son état d’origine. Pour mettre à jour le pack d’advisor vers une version plus récente, copiez le nouveau pack d’advisor dans le dossier de pack de l’Assistant. Un pack d’advisor est considéré comme une mise à jour un pack d’advisor existant uniquement s’il n’existe aucune modification de métadonnées du rapport. Le rapport et les ID de règles doivent être identiques. Sinon, ils doivent être traités sous forme de deux packs de conseiller différents.

s’il existe uniquement les modifications de logique métier et aucune modification de métadonnées de rapport pour un pack d’advisor, vous devez lui affecter un nouveau numéro de version, par exemple à partir de 1.0 vers la version 2.0. S’il existe des modifications de métadonnées de rapport, le pack de l’Assistant doit disposer un autre nom complet. Par exemple, Microsoft.ServerPerformanceAdvisor.IIS.V1 peut être changé à Microsoft.ServerPerformanceAdvisor.IIS.V2.

Si une version plus récente du pack d’advisor existe, la liste dans le **configurer les packs d’Advisor** formulaire remplit automatiquement la **version** colonne avec la dernière version du pack d’advisor. Vous pouvez sélectionner le pack de l’Assistant, puis cliquez sur le **réinitialiser**. Les mises à jour du pack advisor avec la nouvelle logique métier et les seuils. Tous les rapports de ce pack d’advisor sont conservés.

## <a name="managing-servers"></a>La gestion des serveurs


SPA fournit des fonctionnalités de base pour la gestion des serveurs cibles. Vous pouvez choisir Ajouter de nouveaux serveurs à la liste des serveurs cibles, de supprimer des serveurs dans la liste ou de modifier des notes pour les serveurs.

**Pour ajouter ou supprimer des serveurs ou modifier des informations sur le serveur**

1.  Cliquez sur le **Configuration** menu, cliquez sur **configurer les serveurs**.

2.  Dans la liste des serveurs qui existent actuellement dans la base de données de projet, la dernière ligne est vide. Cliquez sur la ligne et renseignez les champs. Le **nom du serveur** et **emplacement du partage de fichier** dossier champs sont obligatoires, et le nom du serveur doit être unique.

3.  Entrez d’autres informations de serveur dans le **Remarque** colonne pour chaque serveur.

    **Remarque** ce champ utilise un format de texte libre, afin de pouvoir l’utiliser comme un champ de description. Ou utilisez ce champ pour baliser les serveurs afin qu’ils puissent être localisés facilement dans la fenêtre principale, ou sur des serveurs de groupe, par exemple, par emplacement ou le rôle serveur.

     

4.  Si vous souhaitez utiliser SPA avec un grand nombre de serveurs, SPA prend en charge un format de valeurs séparées par des virgules (.csv) pour l’importation. Le fichier doit contenir au moins deux champs : **Serveur** et **emplacement du partage de fichiers**. Le troisième champ **Remarque** est facultative, mais il est recommandé d’organiser vos serveurs. Vous pouvez également exporter la liste des serveurs dans un fichier .csv pour déterminer le format approprié ou de sauvegarder votre configuration de serveur.

### <a name="searching-and-filtering"></a>Recherche et filtrage

Si vous gérez plusieurs serveurs, SPA fournit la prise en charge de base pour trouver rapidement les serveurs dans la fenêtre principale. Vous pouvez cliquer sur la colonne en-tête pour trier selon le nom du serveur, résultats de l’analyse, état actuel de la tâche ou notes. Vous pouvez également choisir d’utiliser la fonctionnalité de recherche. Dans le coin supérieur droit de la fenêtre principale, vous pouvez taper une chaîne à rechercher. Le **serveur cible** liste dans la fenêtre principale utilise la chaîne pour filtrer les serveurs et pour afficher uniquement les serveurs avec des champs de nom ou de la Remarque qui contient la chaîne de recherche.

L’illustration suivante montre comment la chaîne **delL** correspond à des serveurs avec la chaîne **delL** ou des serveurs avec **Remarque** champ contenant **delL**.

![exemple de filtrage et de recherche](../media/server-performance-advisor/spa-user-manual-find-search-example.png)

## <a name="advanced-functionality"></a>Fonctionnalités avancées


### <a name="working-with-spa-windows-powershell-cmdlets"></a>Utilisation des applets de commande PowerShell de Windows SPA

La console SPA prend en charge via l’interface utilisateur pour la collecte des données d’abonnement. Si cette fonctionnalité n’est pas suffisante pour votre environnement, il existe des applets de commande Windows PowerShell qu’avancés de l’administrateur peuvent utiliser pour personnaliser la collecte des données. Ces applets de commande Windows PowerShell permettent aux administrateurs de système pour automatiquement exécuter l’analyse des performances sur les serveurs cibles des SPA pour la prise en charge des clients distants. Par exemple, les administrateurs système peuvent écrire des scripts pour appeler les applets de commande SPA dans certains intervalles de temps à échantillonner régulièrement la condition de performances des serveurs cibles.

Nous vous recommandons de fermer l’application SPAConsole.exe avant d’utiliser ces applets de commande.

Avant d’exécuter les applets de commande Windows PowerShell, vous devez inscrire les applets de commande sur l’ordinateur de la console.

**Pour inscrire les applets de commande Windows PowerShell**

1.  À partir d’une invite de commandes Windows PowerShell avec élévation de privilèges, tapez **registerSpaCmdlets.cmd**. Le **inscrire correctement les applets de commande SPA** message s’affiche.

2.  Exécutez **SPA-PowerShell.cmd**. Si vous passez le chemin d’accès à un fichier de script Windows PowerShell, il s’exécute les scripts automatiquement. Sinon, il ouvre une invite de commandes Windows PowerShell, qui est prête à exécuter les applets de commande PowerShell de Windows SPA.

Le tableau suivant décrit les applets de commande SPA Windows PowerShell :

| Nom de l’applet de commande | Paramètres | Description |
| ------ | ------- | ------ |
| Start-SpaAnalysis | **-ServerName** nom du serveur cible.<br>**-AdvisorPackName** nom complet du pack d’advisor en file d’attente sur le serveur. Lorsque plusieurs packs est planifiée pour s’exécuter en même temps, la valeur du paramètre doit être mis en forme en tant que AP1name, AP2name.<br>**-Durée** durée pour la collecte de données.<br>**-Credential** informations d’identification de l’utilisateur pour le compte qui exécute la collecte de données sur le serveur cible.<br>**-SqlInstanceName** nom de l’instance de SQL Server.<br>**-SqlDatabaseName** nom de la base de données de projet SPA. | Démarre une session de collecte de données SPA sur le serveur spécifié. |
| Stop-SpaAnalysis | **-SqlInstanceName** nom de l’instance de SQL Server.<br>**-SqlDatabaseName** nom de la base de données de projet SPA.<br>**-ServerName** nom du serveur cible. | Tente d’arrêter une session d’application à page unique en cours d’exécution. Si une session est déjà terminée, elle est retournée sans rien faire. |
| Get-SpaServer | **-SqlInstanceName** nom de l’instance de SQL Server.<br>**-SqlDatabaseName** nom de la base de données de projet SPA. | Obtient la liste des serveurs dans la base de données. Elle retourne une liste d’objets, y compris de ces propriétés : Nom, état, le partage de fichiers et Remarque. |
| Get-SpaAdvisorPacks | **-SqlInstanceName** nom de l’instance de SQL Server<br>**-SqlDatabaseName** nom de la base de données de projet SPA | Obtient la liste des packs d’advisor dans la base de données. Elle retourne une liste d’objets, y compris de ces propriétés : Nom, DisplayName, auteur et la Version. |

Windows PowerShell vous permet de transmettre des informations d’identification via les fichiers chiffrés peuvent activer des scénarios d’automatisation. Pour plus d’informations sur l’utilisation de fichiers chiffrés pour transmettre des informations d’identification pour une applet de commande, consultez [créer des Scripts Windows PowerShell qu’informations d’identification de l’accepter](https://technet.microsoft.com/magazine/ff714574.aspx).

### <a name="automating-spa-report-collection-by-using-windows-powershell"></a>Automatisation de collecte des rapports SPA à l’aide de Windows PowerShell

Les procédures suivantes représentent un exemple pour savoir comment automatiser une collection de rapports SPA à l’aide des applets de commande PowerShell de Windows SPA.

Informations d’identification de l’utilisateur peuvent être chiffrées et mis en cache via Windows PowerShell. Ces informations d’identification sont utilisées pour se connecter aux serveurs distants. Bien que non spécifique à l’application à page unique, l’exemple suivant illustre cette technique :

``` syntax
$fileName = 'D:\temp\operator.txt'
$userName = 'domainname\operator'
 
# save credential to file
$(Get-Credential).Password | convertFrom-SecureString | Set-Content $fileName
 
# load credential from file
$credential = New-Object System.Management.Automation.PsCredential $userName, $(Get-Content $fileName | convertTo-SecureString)
 
# run command
.\start-SpaAnalysis  ServerName: Server1  Credential: $credential  AdvisorPackName:Microsoft.ServerPerformanceAdvisor.CoreOS.V1 10  Duration:10  SqlInstanceName: .\SQLExpress  SqlDatabaseName:SPA8294
```

Avant de pouvoir exécuter ce fichier, vous devez exécuter **set-executionpolicy remoteSigned**

Vous pouvez l’exécuter à partir d’un fichier de commandes à l’aide de la commande suivante :

``` syntax
PowerShell -command "& '.\RunSpa.ps1' "
```

### <a name="use-t-sql-to-generate-reports"></a>Utiliser T-SQL pour générer des rapports

Les données de rapport SPA peuvent être extraites à l’aide de SQL pour générer des rapports personnalisés pas qui sont fournis dans le SPAConsole.exe.

par exemple, la commande T-SQL suivante fournit une liste des 10 principales de serveurs par processeur pour l’intervalle de temps qui couvrent les trois derniers jours :

``` syntax
select TOP 10
   MachineName,
   AverageCpu
FROM (
   select
      __MachineName MachineName,
      AVG(AverageValue) AverageCpu
   FROM [SPA].[Microsoft.ServerPerformanceAdvisor.CoreOS.V1].vwCpuPerformance
   WHERE __Creationtime > dateadd(DAY, -3, GETUTcdate())
      AND Name = N'% Processor time' AND CpuId = N'_Total'
   GROUP BY __MachineName
) t
OrdER BY t.AverageCpu DESC 
```

### <a name="working-with-multiple-projects"></a>Travailler avec plusieurs projets

Dans certains cas, il pourrez que vous souhaitez partitionner les bases de données SQL Server qui sont utilisés par SPA. Cela peut aider à réduire la taille des données de la base de données SQL Server ou de vous aider à partitionner logiquement les serveurs. Dans ce cas, la console SPA prend en charge plusieurs projets. Vous pouvez créer une nouvelle base de données de projet en cliquant sur **fichier**, puis en cliquant sur **nouveau projet**. La première fois, utilisez l’Assistant s’affiche pour aider à créer la nouvelle base de données de projet.

Une fois que les projets sont créés, vous pouvez cliquer sur **fichier**, puis cliquez sur **ouvrir un projet** pour basculer entre les projets. La console SPA ne permet pas de basculer entre des bases de données lors de l’analyse des performances en attente est en cours d’exécution dans le projet actuellement ouvert. Il s’agit de protéger l’intégrité des données de performances.

Lorsque vous choisissez de créer une nouvelle base de données de projet ou ouvrez une base de données de projet différent, le projet actuel est fermé. Tous les rapports ouverts sont fermés lorsque le projet actuel est fermé.

**Remarque** SQL Server 2008 R2 Express a une limite de base de données de 10 Go. À l’aide de plusieurs projets, vous pouvez utiliser une ou plusieurs bases de données SQL Server et dépasser la limite de 10 Go SQL Server 2008 R2 Express.

 

### <a name="logging-and-debugging"></a>Journalisation et de débogage

SPA fournit des fonctionnalités de journalisation de base. Il autorise uniquement les journaux à écrire dans un fichier journal, qui se trouve dans le même dossier que SPAConsole.exe. Il faut que le compte d’utilisateur qui exécute la console SPA est autorisé à écrire dans le dossier où il a été installé SPA s’assurer que les journaux peuvent être écrits dans le fichier journal.

SPA contient les niveaux du journal valide suivant :

* **D’information** vide les journaux pour chaque action de la console SPA prend, et il est conçu principalement pour le débogage.

* **Avertissement** consigne tous les échecs et les exceptions qui se produisent à l’intérieur de la console SPA. Certains des échecs sont simplement des échecs de validation qui peuvent être gérés par la console SPA.

* **Critique** enregistre uniquement les échecs et les exceptions qui ne peut pas être gérées par la console SPA. Ces dysfonctionnements la console SPA blocage. Les journaux fournissent les informations de contexte pour de tels échecs.

Par défaut, le niveau de journalisation est avertissement, ce qui signifie que SPA consigne uniquement les échecs et exceptions qui se produisent dans l’application à page unique. Le niveau de journalisation peut être modifié en modifiant le **SpaConsole.exe.config** de fichiers dans le même dossier que SpaConsole.exe se trouve. Tous les journaux sont écrits dans fichier log.txt dans le même dossier.

SPA fournit également une fonctionnalité de base pour le débogage. Pour activer le débogage pour SPA, les utilisateurs doivent modifier manuellement la base de données de projet SPA. Le paramètre est stocké dans une table de Configurations. Utilisateur avez besoin exécuter le script SQL suivant pour modifier le projet d’application à page unique pour le mode de débogage :

``` syntax
UPdate [Configurations] SET Value = N'true' WHERE Name = N'Debugmode'.
```

En mode débogage, le framework SPA conserve les données temporaires sont générées pendant l’analyse des performances afin que vous pouvez résoudre les problèmes dans les packs d’advisor. Ce script est principalement conçu pour être utilisé par les développeurs de packs de l’Assistant.

Pour plus d’informations sur les fonctionnalités de débogage de SPA, consultez le [Server Performance Advisor Pack Development Guide](server-performance-advisor-pack-development-guide.md).

### <a name="managing-the-database"></a>La gestion de la base de données

### <a name="backup-and-restore-the-database"></a>Sauvegarder et restaurer la base de données

La console SPA ne fournit pas de fonctionnalité pour sauvegarder et restaurer la base de données d’application à page unique. Vous devez utiliser des scripts et outils de Microsoft SQL Server standard pour sauvegarder et restaurer des bases de données.

### <a name="clean-up-the-database"></a>nettoyer la base de données

Les bases de données de projet SPA peuvent augmenter la taille en plus l’analyse des performances est exécutée. Par défaut, SPA conserve tous les rapports. Voulez-vous supprimer les anciens rapports permettant d’améliorer les performances. Vous pouvez supprimer des rapports par le biais du **Explorateur de rapports** interface.

## <a name="privacy-and-security"></a>Sécurité et confidentialité


SPA prend uniquement en charge la collecte des données à partir de serveurs cibles à la console SPA. Il n’est pas conçu pour transmettre toutes les informations à Microsoft ou aux développeurs non Microsoft. Pour plus d’informations sur la confidentialité de l’application à page unique, consultez les termes du contrat de licence de logiciel Microsoft pour Server Performance Advisor.

Toutes les données collectées par SPA est stocké dans les bases de données de projet. En raison de la nature de certains des traces ETW, SPA peut collecter des informations sensibles qui peut avoir une importance métier élevée. Soyez conscient des risques potentiels associés partagent l’accès aux bases de données du projet SPA. Fichiers journaux temporaires sont enregistrés sous les dossiers partagés qui sont spécifiés sur chaque ordinateur cible. Même si les tentatives de SPA pour supprimer ces journaux temporaire des fichiers à l’issue de l’importation de données, il n’existe aucune garantie que ces fichiers journaux sont toujours être supprimés. Il se peut que vous devez vous assurer que les dossiers partagés sont trouvent dans des emplacements sécurisés.

Les packs d’advisor SPA contient des scripts SQL pour analyser et d’analyser les journaux de performances pour générer des rapports de performances. SPA tente de limiter le privilège ces scripts s’exécutent sous. Toutefois, il est possible que les scripts peuvent collecter des informations sensibles via SPA à partir de serveurs cibles, ou obtenir ou modifier des informations sensibles qui sont stockées dans la même base de données de projet SPA. Vous devez vous assurer que tous les packs d’advisor sont configurées sur la base de données de projet SPA proviennent de sources fiables.

## <a name="errors-and-troubleshooting"></a>Erreurs et dépannage


### <a name="spaconsoleexe-does-not-start-or-write-log-file"></a>SPAConsole.exe ne démarre pas ou ne souhaite pas écrire le fichier journal

Lorsque vous essayez d’exécuter SPAConsole.exe pour la première fois, si le .NET Framework n’est pas installé, l’application ne démarrez pas ou écrire un fichier journal. Vérifiez qu’un compatible.NET que Framework est installé et fonctionner correctement avant de commencer SPA.

### <a name="locating-log-information"></a>Localisation des informations de journal

SPA stocke les informations d’échec dans le fichier log.txt sous le dossier d’application à page unique. messages d’erreur détaillés et informations pile des appels sont écrits dans ce dossier. Si vous rencontrez des échecs nécessitant plus d’informations pour interpréter, vous pouvez ouvrir le fichier log.txt pour afficher les détails de l’erreur.

### <a name="database-size-limitations-for-sql-server-express"></a>Limitations de taille de base de données pour SQL Server Express

SQL Server Express a une limite de taille de 10 Go pour une base de données utilisateur. Cette base de données de taille a la capacité de stocker des rapports de 20 000 à 30,000. Pour réduire le risque d’atteindre cette limite de base de données, nous vous recommandons de supprimer régulièrement des rapports et/ou à l’aide d’une autre version de SQL Server qui n’a pas la limite de base de données utilisateur 10 Go.

### <a name="sql-server-express-log-size-and-disk-capacity"></a>SQL Server Express taille et le disque capacité du journal

Si vous utilisez SQL Server Express, la base de données utilisateur est limité à 10 Go, mais le fichier journal correspondant peut dépasser 70 Go. Pour ces raisons, nous vous recommandons de 100 Go ou plus d’espace disque libre pour SQL Server Express. Cet espace disque doit être suffisant pour stocker environ 20 000 à 30 000 rapports. Ce fichier journal est nommé SPADB\_log.ldf et il se trouve à **%Program Files%\\Microsoft SQL Server\\MSSQL10. SQLEXPRESS\\MSSQL\\données**.

### <a name="failure-to-connect-to-target-server"></a>Échec de connexion au serveur cible

Lorsque vous exécutez l’analyse des performances sur les serveurs cibles, le compte d’utilisateur qui exécute la console SPA doit disposer de certains privilèges en file d’attente du collecteur de données défini sur PLA sur les serveurs cibles. Paramètres de pare-feu de Windows doivent également être modifiée pour autoriser les communications PLA à traverser. Pour obtenir des instructions de configuration détaillées, consultez [Configurer SPA](#bkmk-setupspa).

### <a name="failure-to-create-pla-data-collection-set-on-the-target-server"></a>Échec de création d’ensemble de la Collection de données PLA sur le serveur cible

Si vous obtenez un ne peut pas créer l’ensemble de la Collection de données PLA sur le message du serveur cible, veillez à effectuer les opérations suivantes :

* Assurez-vous que le **les journaux de performances et alertes** service est en cours d’exécution

* Le paramètre de sécurité **accès réseau : Ne pas autoriser le stockage des mots de passe et les informations d’identification pour l’authentification réseau** est désactivé. Le paramètre de sécurité doit être désactivé, car SPA a besoin d’utiliser les informations d’identification utilisateur pour créer l’ensemble de la Collection de données sur le serveur cible.

### <a href="" id="running-spa-against-the-console-"></a>SPA en cours d’exécution par rapport à la console

PLA passe par un autre canal si le serveur cible est identique à la console SPA. Même si le compte d’utilisateur s’exécute la console de l’application à page unique avec des privilèges d’administrateur, PLA échoue. Si la console SPA est installée sur un serveur cible, les utilisateurs doivent lancer SPA en tant qu’administrateur pour vous assurer de que la tâche d’analyse de performances peut exécuter sur la console.

### <a name="running-multiple-consoles-at-the-same-time"></a>Exécution de plusieurs consoles en même temps

SPA ne prend pas en charge plusieurs consoles en cours d’exécution par rapport à la même base de données de projet SPA en même temps. SPA ne fournit pas le mécanisme de verrouillage et de synchronisation pour l’empêcher de se produire. Si les deux consoles SPA sont en cours d’exécution en même temps, la console se comportent de façon incohérente en fonction de la séquence de temps ces consoles SPA sont en cours d’exécution. Pour éviter cela, toutes les sessions d’analyse des performances en file d’attente doivent être supprimées à partir de la liste avant son traitement par la console qui démarre l’analyse.

SPA protège l’intégrité de chaque rapport est généré avec succès par SPA. en même temps, les SPA ne garantit pas que toutes les tâches d’analyse en file d’attente sont terminées. Si vous rencontrez des changements d’état incohérent pour les sessions d’analyse de performances ou les erreurs que le système de revendication ne peut pas trouver les journaux de performances qui sont générés par l’ensemble de collecteurs de données, il est probable soit dû à plusieurs instances de console SPA en cours d’exécution sur le même Base de données du projet SPA.

Exécuter les applets de commande PowerShell de Windows SPA également pourrait être affectée par une console SPA qui s’exécute sur la même base de données d’application à page unique. Nous vous recommandons de fermer la console SPA avant d’exécuter les applets de commande PowerShell de Windows SPA.

### <a name="spaconsoleexe-recurring-collection-is-disrupted"></a>Collection périodique SPAConsole.exe est interrompue.

Lorsque vous exécutez le SPAConsole.exe et vous utilisez une collection de données périodique (par exemple, une collection de toutes les heures), le serveur qui exécute le SPAConsole.exe ne doit pas être en mode d’économie d’énergie tels que suspendre. SPA ne vérifie pas pour une stratégie de veille. Cette activité interrompue peut interrompre la collecte des données périodiques régulières.

### <a name="lost-etw-events"></a>Événements ETW perdus

Pour créer l’impact sur les performances minimale sur les serveurs cibles, PLA est conçu pour s’exécuter avec une faible priorité pendant qu’il collecte des informations sur les performances. Si le serveur cible est occupé, PLA peut supprimer certaines des tâches de collecte de données pour la production des tâches de priorité élevée qui sont exécutent sur les serveurs cibles. Vous devez envisager de définir le dossier partagé où les événements sont écrits sur un disque avec le s de la charge de travail d’e/s ou un lecteur plus rapide, par exemple un disque SSD en conflit ne t. Lorsque les événements sont supprimés, ils sont signalés dans la vue rapport dans la mesure où les événements perdus peuvent avoir un impact sur la fiabilité, les métriques de performances généré.

Certains problèmes d’autorisation peut également entraîner du Registre ou les requêtes WMI doit être ignorée. Toutefois, il s’agit bien moins susceptible de se produire à la perte d’événement ETW. Par conséquent, les résultats de jeu de collecteur de données parfois ne contiennent pas toutes les valeurs qui sont demandées. Vous devez vous assurer que la situation est gérée par les scripts T-SQL pour tous les packs de l’Assistant. Si les données n’existent pas dans le résultat de collection de données, elle est marquée en tant qu’aucune donnée dans les rapports.

Étant donné que la perte d’événement ETW est courante de PLA, les points de données qui sont générés selon un suivi ETW ne peut pas être cohérente avec les points de données qui sont générées en fonction, par exemple, les compteurs de performance. Par exemple, il est possible de voir que l’utilisation totale du processeur par IIS est 80 % (qui proviennent des compteurs de performances), et que la partie supérieure URL utilisent uniquement 10 % de temps processeur total (qui est un point de données provenant de la trace ETW). En règle générale, la source de données pour un point de données peut être affichée via l’info-bulle du point de données. Vous devez connaître l’impact de ce type de perte de données.

Pour éviter cette perte d’événement, le dossier de résultat doit être fermé pour le serveur cible.

Si les résultats de collecteur de données contiennent des données incomplètes autre que de la perte de trace ETW, et le développeur du pack advisor a ajouté la prise en charge de la notification de perte d’événement ETW, une barre d’informations est affichée en haut de l’état unique pour informer l’utilisateur le potentiellement état incohérent provoquée par une perte de données. Vous trouverez des informations de perte de données détaillées dans le fichier log.txt.

## <a name="glossary"></a>Glossaire


Voici certains des termes utilisés avec l’authentification par :

* **Pack de conseiller** une collection de métadonnées et des scripts T-SQL qui traitent les journaux de performances sont collectées à partir du serveur cible. Le pack de l’Assistant génère ensuite des rapports à partir des données de journal de performances. Les métadonnées dans le pack de l’Assistant définissent les données à collecter à partir du serveur cible pour les mesures de performances. Les métadonnées définissent également l’ensemble de règles, les seuils et le format de rapport. En règle générale, un pack d’advisor est écrit spécifiquement pour un rôle de serveur unique, par exemple, Internet Information Services (IIS).

* **Console SPA** SpaConsole.exe, qui est la partie centrale de SPA. SPA n’a pas besoin d’exécuter sur le serveur cible que vous testez. La console SPA contient toutes les interfaces utilisateur pour une seule page, à partir de la configuration du projet à l’exécution de l’analyse et l’affichage de rapports. Par conception, SPA est une application à deux niveaux. La console SPA contient la couche d’interface utilisateur et une partie de la couche de logique métier. La console SPA planifie et traite les demandes d’analyse de performances.

* **Framework SPA** fournit toutes les interfaces utilisateur, traitement des journaux de performances, configuration, gestion des erreurs et procédures de gestion et les API de la base de données.

* **Projet SPA** une base de données qui contient toutes les informations sur les serveurs cibles, les packs d’advisor et l’analyse des performances des rapports qui sont générés sur les serveurs cibles pour les packs d’advisor. Vous pouvez comparer et afficher des graphiques de l’historique et de tendance au sein du même projet SPA. Vous pouvez créer plusieurs projets. Les projets SPA sont indépendants des uns des autres et aucune donnée partagés entre plusieurs projets.

* **Serveur cible** l’ordinateur physique ou virtuel qui exécute Windows Server avec certains rôles de serveur, telles que IIS.

* **Session d’analyse de données** une analyse des performances sur un serveur cible spécifique. Une session d’analyse de données peut inclure plusieurs packs d’advisor. Les ensembles de collecteurs de données à partir de ces packs d’advisor sont fusionnées dans un ensemble de collecteurs de données unique. Tous les journaux de performances pour une session d’analyse de données unique sont collectées au cours de la même période. Analyse des rapports qui sont générés par les packs d’advisor en cours d’exécution dans la même session d’analyse de données peut aider les utilisateurs à comprendre la situation de performances globales et identifier les causes principales des problèmes de performances.

* **Le suivi d’événements pour Windows** un système de suivi hautes performances, faible surcharge et évolutive qui est fourni dans Windows. Il fournit le profilage et de fonctionnalités, qui peuvent être utilisées pour dépanner un certain nombre de scénarios de débogage. SPA utilise des événements ETW comme une source de données pour générer les rapports de performances. Pour obtenir des informations générales sur le suivi ETW, consultez [Améliorez le débogage et l’optimisation des performances avec ETW](https://msdn.microsoft.com/magazine/cc163437.aspx).

* **Windows Management Instrumentation (WMI)** l’infrastructure pour les opérations dans Windows et les données de gestion. Vous pouvez écrire des scripts WMI ou des applications afin d’automatiser les tâches d’administration sur des ordinateurs distants. WMI fournit également des données de gestion à d’autres parties du système d’exploitation et aux produits. SPA utilise les informations de classe WMI et les points de données en tant que sources pour générer des rapports de performances.

* **Compteurs de performances** permet de fournir des informations sur le système d’exploitation ou une application, un service ou un pilote de performances. Les données de compteur de performances peuvent aider à déterminer les goulots d’étranglement système et affiner les performances des applications et du système. Le système d’exploitation, de réseau et les appareils fournissent des données de compteur qu’une application peut consommer pour fournir aux utilisateurs une vue graphique du degré de performances du système. SPA utilise les informations sur les compteurs de performances et de points de données en tant que sources pour générer des rapports de performances.

* **Les journaux de performances et alertes (PLA)** collecte performances se connecte et effectue le suivi et déclenche des alertes de performances lorsque certains déclencheurs sont remplies. PLA peut être utilisé pour collecter les compteurs de performance, event Tracing for Windows (ETW), les requêtes WMI, les clés de Registre et la configuration pour les fichiers. PLA prend également en charge la collecte des données à distance via des appels de procédure distante (RPC). L’utilisateur définit un ensemble de collecteurs de données, qui inclut des informations sur les données à collecter, la fréquence de collecte de données, la durée de collection de données, filtres et un emplacement pour enregistrer les fichiers de résultats. SPA utilise PLA pour collecter toutes les données de performances à partir des serveurs cibles.

* **Un seul rapport** rapport un SPA est généré en fonction de la session d’analyse de données pour le pack d’un conseiller sur un serveur cible unique. Il peut contenir des notifications et des différentes sections de données.

* **Rapport de côte à côte** rapport un SPA qui compare deux rapports uniques pour le même pack d’advisor. Les deux rapports peuvent être générés à partir de serveurs cibles différentes ou des séries d’analyse de performance distinct sur le même serveur cible. Le rapport côte à côte crée la possibilité de comparer deux rapports pour aider les utilisateurs à identifier les comportements anormaux ou paramètres dans un des rapports. Un rapport côte à côte contient des notifications et des différentes sections de données. Dans chaque section, les données à partir de ces deux rapports sont répertorié côte à côte.

* **Graphique de tendance** rapport d’une application à page unique qui sert à enquêter sur les modèles répétitifs des problèmes de performances. Nombreux problèmes de performances répétitives sont causés par des modifications de charge de server planifiées à partir du serveur ou à partir des ordinateurs clients, ce qui peuvent se produire quotidienne ou hebdomadaire. SPA fournit un graphique de tendances de 24 heures et un graphique de tendance de 7 jours pour identifier ces problèmes.

    L’utilisateur peut choisir une ou plusieurs séries de données à la fois, qui est une valeur numérique à l’intérieur du rapport unique, tel que **total utilisation moyenne du processeur**. plus précisément, une valeur numérique est une valeur scalaire à partir d’un serveur unique qui est généré par un point d’accès unique à une instance de temps donné. SPA regroupe ces valeurs dans des groupes de 24, une pour chaque heure de la journée (sept pour un rapport de 7 jours, un pour chaque jour de la semaine). SPA calcule la moyenne, minimum, maximum et les écarts types pour chaque groupe.

* **Graphique historique** rapport un SPA qui est utilisé pour afficher les modifications apportées dans certains numérique des valeurs à l’intérieur de rapports uniques pour un serveur donné et un conseiller pack paire au fil du temps. L’utilisateur peut choisir plusieurs séries de données et les afficher ensemble dans le graphique d’historique pour comprendre la corrélation entre les différentes séries de données.

* **Série de données** des données numériques qui sont collectées à partir de la même source de données sur une période de temps. La même source signifie que les données doit provenir du même serveur cible, tel que la longueur de file d’attente moyenne de demande pour IIS sur un seul serveur.

* **Règles** combinaisons de logique, des seuils et des descriptions. Ils représentent un problème potentiel de performances. Chaque pack d’advisor contient plusieurs règles. Chaque règle est déclenchée par un processus de génération de rapports. Une règle s’applique la logique et les seuils pour les données de rapport unique. Si les critères sont satisfaits, une notification d’avertissement est déclenchée. Si non, la notification est définie sur le **OK** état. Si la règle ne s’applique pas, la notification a la valeur non Applicable (**NA**) état.

* **Notifications** les informations affichées par une règle pour les utilisateurs. Il inclut l’état de la règle (**OK**, **NA**, ou un **avertissement**), le nom de la règle et les recommandations possibles pour résoudre les problèmes de performances.
