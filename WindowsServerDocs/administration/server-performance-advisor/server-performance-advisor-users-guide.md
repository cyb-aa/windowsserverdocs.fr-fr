---
title: Guide de l’utilisateur de Server Performance Advisor
description: Guide de l’utilisateur de Server Performance Advisor
ms.assetid: be99a5a9-b9c0-436b-912c-e5c5839a533d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.topic: article
ms.prod: windows-server
ms.technology: manage
ms.openlocfilehash: 6a6ccedeeb007b9d3ab32c308fae991deb526442
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383089"
---
# <a name="server-performance-advisor-users-guide"></a>Guide de l’utilisateur de Server Performance Advisor

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Windows 8

Ce guide de l’utilisateur pour Microsoft Server Performance Advisor (SPA) fournit des instructions sur l’utilisation de SPA pour identifier les goulots d’étranglement de performances dans les systèmes déployés dans différents rôles de serveur.

SPA peut vous aider pour les opérations suivantes :

* Gérez les performances de votre serveur et résolvez les problèmes de performances du serveur.

* Fournir des rapports de données et des recommandations sur les problèmes de configuration et de performances courants.

* Fournissez les meilleures recommandations Pratice en fonction des données collectées.

> [!NOTE]
> La console du SPA n’apporte aucune modification aux serveurs.

Pour plus d’informations sur le développement de packs d’Advisor SPA, consultez le [Guide de développement de Server Performance Advisor Pack](server-performance-advisor-pack-development-guide.md).

## <a name="server-performance-advisor-overview"></a>Présentation de Server Performance Advisor


Cette section explique l’historique de SPA, décrit son public et ses scénarios cibles, et présente une vue d’ensemble architecturale de l’outil.

### <a name="history-of-spa"></a>Historique de SPA

La version d’origine de Microsoft Server Performance Advisor (SPA) a été publiée dans 2005-2006. Il ciblait principalement Windows Server 2003, y compris le système d’exploitation principal et les rôles de serveur tels que Internet Information Services (IIS) et Active Directory. L’objectif de l’outil est de vous aider à évaluer les performances de votre serveur et à résoudre les problèmes de performances du serveur.

SPA fournit des rapports de données et des recommandations aux administrateurs système sur les problèmes de configuration et de performances courants. SPA recueille les données relatives aux performances à partir de différentes sources sur les serveurs, par exemple, les compteurs de performances, les clés de Registre, les requêtes WMI, les fichiers de configuration et les Suivi d’v nements pour Windows (ETW). En fonction des données de performances du serveur collectées, l’authentification SPA peut fournir une analyse approfondie de la situation actuelle en matière de performances du serveur et des recommandations sur ce qui peut être amélioré.

Il y a eu plus de 1 million téléchargements de l’outil SPA d’origine et a aidé de nombreux administrateurs système à gérer les performances de leurs serveurs. Il s’agit d’un outil de dépannage des performances très performant.

Depuis la publication du SPA d’origine, plusieurs modifications majeures ont été apportées dans le monde des performances du serveur. Plus particulièrement, la version de Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008, avec les nouvelles versions des rôles serveur correspondants tels que Internet Information Services (IIS) 8,5. Les versions plus récentes de Server Performance Advisor étendent la capacité du SPA d’origine aux systèmes d’exploitation et aux rôles de serveur récents.

Bien que SPA 3,1 partage les mêmes objectifs de conception et le même paradigme d’analyse des performances que l’outil d’origine, il a été réécrit avec la technologie la plus récente. Il présente également les principales améliorations suivantes :

* Peut effectuer une analyse sur des serveurs exécutant Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008.

* Prend en charge la fonctionnalité d’analyse à distance, qui permet à SPA 3,1 de collecter et d’analyser des données à partir d’une console centrale, sans qu’il soit nécessaire d’installer du code sur les serveurs à analyser.

* Utilise une Microsoft SQL Server pour l’analyse et le stockage des données, ce qui permet le traitement et le stockage d’une grande quantité de données.

* Sépare l’outil des packs d’Advisor. La logique d’analyse des performances pour chaque rôle de serveur est publiée sous la forme d’un pack Advisor, qui peut être publié ou mis à jour séparément à partir de l’outil.

* Fournit des packs Advisor développés dans des scripts SQL avec une architecture ouverte. Les tiers non-Microsoft peuvent développer des packs d’avis ou étendre les packs d’Advisor existants de Microsoft pour répondre à des besoins particuliers.

* Prend en charge de nouvelles fonctionnalités telles que les rapports de comparaison côte à côte, ainsi que des graphiques de tendances et d’historique pour vous aider à trouver des anomalies.

* Fournit des fonctionnalités telles que les seuils de modification, d’importation et d’exportation pour vous aider à affiner les rapports et les notifications et à partager ces réglages avec d’autres utilisateurs de SPA.

* Prend en charge plusieurs projets, qui peuvent être utilisés pour regrouper des serveurs ciblés.

* Fournit la collecte de données périodiques à partir de la console du SPA.

* Active les requêtes personnalisées et la génération de rapports à l’aide de Microsoft SQL Server (pour les utilisateurs expérimentés).

* Des applets de commande Windows PowerShell personnalisées peuvent être utilisées avec SPA

* À compatibilité descendante pour l’importation et l’affichage de rapports à partir d’une base de données SPA 3,0

### <a name="target-audience"></a>Public visé

L’outil SPA est principalement conçu pour les administrateurs système qui gèrent moins de 100 serveurs dans différents rôles de serveur. Il peut également être utilisé par les ingénieurs du support technique pour collecter des données de performances et résoudre les problèmes de performances pour les clients.

SPA propose des recommandations pour vous aider à résoudre les problèmes de performances. Toutefois, il est basé sur des hypothèses qui peuvent ne pas être applicables à votre environnement de serveur spécifique. Les recommandations fournies via SPA doivent être considérées comme des suggestions. Nous pensons que les utilisateurs SPA ont une bonne compréhension de la configuration de leur système, des cas d’utilisation et de l’impact du réglage du système sur le comportement global du système. Les administrateurs système doivent décider si les recommandations de SPA doivent être appliquées à leur environnement.

### <a href="" id="what-s-new-in-spa-3-1-"></a>Quelles sont les nouveautés de SPA 3,1 ?

Les fonctionnalités et améliorations suivantes ont été ajoutées dans SPA 3,1 :

* Active Directory Advisor Pack permet d’analyser les performances générales du rôle serveur des services de domaine Active Directory du serveur.

* Prise en charge des développeurs pour ajouter une alerte de notifications d’événements ETW perdue dans les packs d’Advisor et rendre l’alerte visible quand un rapport a été généré et que des événements ont été perdus en raison d’une journalisation haute fréquence sur un disque lent ou fortement sollicité

### <a name="target-scenarios"></a>Scénarios cibles

Voici les scénarios cibles pour SPA :

* **Environnement de serveur**

    SPA est conçu pour faciliter la configuration et la maintenance. Elle s’applique aux environnements serveur qui utilisent de 1 à 100 serveurs. SPA n’est pas adapté si vous essayez de gérer les performances de plus de 100 serveurs. Pour les environnements plus grands ou plus sophistiqués, vous devez envisager d’utiliser System Center Operations Manager.

* **Résolution des problèmes de performances**

    Vous pouvez utiliser SPA comme outil de résolution des problèmes de performances. Il permet de collecter des données de performances de haut niveau, il effectue un traitement de publication approfondi sur les données pour vous permettre de mieux comprendre le comportement général de leur système, et il signale toutes les anomalies. Lorsqu’un client soupçonne un problème de performances, vous pouvez utiliser SPA pour collecter et analyser les données de performances du serveur.

    SPA génère un rapport que vous pouvez afficher. Les rapports SPA fournissent une liste de notification qui met en évidence les problèmes potentiels et une section de données qui comprend différents index de performances, configurations et paramètres pour le serveur. Vous pouvez utiliser ce rapport pour identifier le problème de performances spécifique et utiliser les recommandations pour trouver des solutions pour le problème. Les rapports peuvent être comparés à d’autres rapports générés à une heure différente ou à un serveur différent. À l’aide de cette comparaison côte à côte, vous pouvez déterminer les différences entre la ligne de base et le comportement anormal.

    **Remarque** SPA n’est pas conçu pour être un outil de débogage ou de contrôle. En outre, du point de vue des serveurs, il peut être considéré comme un outil en lecture seule et ne modifie pas les configurations des serveurs.

     

* **Analyse d’index de performances**

    Vous pouvez utiliser SPA pour surveiller l’index des performances des serveurs. Vous pouvez choisir d’exécuter de manière régulière SPA sur les serveurs pour collecter des données de performances, puis exécuter un graphique de tendance ou un graphique d’historique pour identifier les anomalies. Vous pouvez afficher le rapport pour une analyse particulière, obtenir plus d’informations sur le problème de performances, puis utiliser les recommandations ou d’autres données de rapport pour résoudre le problème.

### <a name="spa-architecture"></a>Architecture SPA

La logique de la collecte de données SPA repose sur un protocole dans Windows appelé Journaux et alertes de performance (PLA). PLA permet aux programmes de collecter les données de performances des serveurs locaux ou distants, tels que les compteurs de performances, les requêtes WMI, les traces ETW, les clés de Registre et les fichiers de configuration. Lorsque SPA exécute l’analyse des performances sur un serveur ciblé, il crée un ensemble de collecteurs de données PLA basé sur le pack d’conseiller SPA spécifique que vous avez sélectionné. Le Pack Advisor contient la source de données à collecter et le partage de fichiers dans lequel les journaux doivent être stockés. La collecte de données SPA ne stocke qu’un seul compte d’utilisateur ; le même compte d’utilisateur utilisé par PLA est également utilisé pour écrire les journaux.

SPA utilise une base de données SQL Server pour stocker les journaux de performances qui sont collectés à partir des serveurs ciblés. SPA importe tous les journaux de performances du partage de fichiers dans la base de données, puis utilise la logique d’analyse de données à l’intérieur de chaque pack Advisor pour traiter les données et générer les rapports. Un Advisor Pack analyse les données de performances collectées à partir des serveurs cibles et génère les rapports du SPA. Pour plus d’informations sur la création d’un Advisor Pack, consultez le [Guide de développement de Server Performance Advisor Pack](server-performance-advisor-pack-development-guide.md).

Les interfaces utilisateur et les interactions de la console SPA sont créées dans le cadre de SPAConsole. exe. Vous pouvez utiliser la console pour créer des rapports dans une base de données, ajouter ou supprimer des packs d’avis, gérer des serveurs cibles, exécuter une analyse des performances et afficher des rapports de performances.

La console du SPA peut s’exécuter sur les systèmes d’exploitation suivants :

* Windows 8.1

* Windows 8

* Windows 7

* Windows Server 2012 R2

* Windows Server 2012

* Windows Server 2008 R2

* Windows Server 2008

Dans une application métier classique, il existe trois niveaux : la couche de présentation, la couche de logique métier et la couche de stockage. SPA est conçu comme un produit à deux niveaux, la console et la base de données. La console sert de couche de présentation avec une certaine logique liée au processus, et la base de données sert de couche de stockage et de couche de logique métier. La console capture les entrées d’utilisateur et contrôle les étapes de la collecte de données, du traitement des données et de la génération de rapports. SPA ne dépend pas des services système Windows.

Le diagramme suivant illustre l’architecture de haut niveau du système SPA. Le processus est le suivant :

1.  À partir de la console du SPA, vous exécutez l’analyse des performances sur les serveurs spécifiés.

2.  Lorsque les données de performances sont collectées, PLA sur les serveurs ciblés réécrit les journaux dans le partage de fichiers spécifié par l’ensemble de collecteurs de données.

3.  Une fois la collecte de données terminée sur un ordinateur cible, la console du SPA importe les journaux dans la base de données SQL Server.

4.  La console appelle la logique de traitement des données d’Advisor Pack spécifique.

5.  Le Pack Advisor traite les données et appelle les API SPA pour insérer des enregistrements dans les tables de rapports système définies par l’infrastructure SPA.

6.  Vous pouvez utiliser la visionneuse de rapports à l’intérieur de la console pour afficher les rapports qui ont été générés. Pour vous aider à résoudre les problèmes de performances, SPA fournit trois types de rapports : les rapports uniques, les rapports côte à côte et les graphiques de tendances ou historiques. Pour plus d’informations sur les rapports, consultez [affichage des rapports](#bkmk-viewingreports).

![architecture Spa](../media/server-performance-advisor/spa-user-manual-architecture.png)

## <a name="getting-started-with-spa"></a>Prise en main de SPA


### <a name="requirements"></a>Configuration requise

La console du SPA peut être installée sur Windows 8.1, Windows 8, Windows 7, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008. L’exécution de SPA sur des versions antérieures du système d’exploitation Windows Server n’est pas prise en charge. SPA s’exécute sur x86 ou x64, mais ne prend pas en charge les architectures IA64 ou ARM.

L’authentification SPA repose sur Windows Presentation Foundation (WPF) 2,0, qui fait partie du Microsoft .NET Framework 4, .NET Framework 4 est donc requis.

Vous devez également installer SQL Server 2008 R2 Express sur le même ordinateur que celui où est installé SPA. SQL Server 2008 R2 Express est un moteur de base de données gratuit publié par Microsoft. Vous pouvez télécharger Microsoft SQL Server 2008 R2 Express à partir du centre de téléchargement Microsoft. Bien que nous suggérions SQL Server 2008 R2 Express, les versions plus récentes de SQL Server peuvent également être compatibles avec SPA.

**Remarque** SPA n’inclut pas SQL Server ou le .NET Framework dans le cadre du package d’installation du SPA. Après l’installation de Microsoft SQL Server 2008 R2 et .NET Framework 4,0, nous vous recommandons d’exécuter Windows Update avant d’installer SPA.

 

Étant donné que les utilisateurs peuvent créer et gérer des bases de données avec SPA, le compte d’utilisateur utilisé pour exécuter SPA doit avoir les mêmes privilèges d’administrateur que SQL Server.

### <a href="" id="bkmk-setupspa"></a>Configuration de SPA

SPA est empaqueté sous la forme d’un fichier. cab qui comprend tous les fichiers binaires de l’infrastructure SPA, les applets de commande Windows PowerShell utilisées dans les scénarios avancés et les packs Advisor suivants : Le système d’exploitation principal, Hyper-V, Active Directory et IIS. Une fois que vous avez extrait le fichier. cab dans un dossier, aucune installation supplémentaire n’est nécessaire. Toutefois, pour exécuter SPA, vous devez activer la collecte des données à partir des serveurs cibles comme suit :

* Pour exécuter la collecte de données PLA, le compte d’utilisateur que vous utilisez pour exécuter la console du SPA doit faire partie du groupe de sécurité Administrateurs sur le serveur cible. Si le serveur cible et la console se trouvent dans le même domaine, le compte d’utilisateur de domaine doit faire partie du groupe de sécurité Administrateurs sur le serveur cible. Si le serveur cible et la console ne se trouvent pas dans le même domaine, créez un compte d’utilisateur administratif sur le serveur cible avec les mêmes nom d’utilisateur et mot de passe que le compte d’utilisateur que vous utilisez pour exécuter la console du SPA.

* Créez un dossier partagé pour les résultats sur le serveur.

* Assurez-vous que le compte d’utilisateur que vous utilisez pour exécuter la console du SPA dispose des autorisations en lecture et en écriture sur le dossier partagé. PLA utilise ce compte pour écrire des journaux dans le dossier.
La console SPA utilise le même compte pour lire les journaux et les importer dans la base de données.

    **Remarque** ETW implémente une mémoire tampon circulaire pour stocker la trace et les déplace dans le dossier partagé lorsque cela est possible. Si le serveur est occupé ou si l’opération d’écriture est lente, ETW supprime les traces lorsque la mémoire tampon est saturée. Il est IMPORTANT que le dossier partagé se trouve sur un serveur avec un accès d’e/s rapide. Nous recommandons que chaque serveur cible dispose d’un dossier partagé pour réduire la perte de données due à des e/s de fichier lentes.

     

* Pour PLA pour accéder aux serveurs cibles, définissez le pare-feu Windows de façon à autoriser l’accès Journaux et alertes de performance à distance sur les serveurs cibles. PLA utilise le port TCP 139.

* Assurez-vous que le service **journaux de performances & alertes** est en cours d’exécution.

* Si la console et le serveur cible se trouvent dans des sous-réseaux différents, vous devez également définir le champ adresse IP distante dans les règles de pare-feu entrantes dans les paramètres d' **étendue** sur la page **journaux et alertes de performance** , comme indiqué ici.

    ![PLA, propriétés](../media/server-performance-advisor/spa-user-manual-pla-firewall.png)

* Activez la découverte du réseau sur la console et sur chacun des serveurs cibles.

* Si le serveur cible n’est pas joint à un domaine, activez le paramètre de Registre suivant : **HKLM @ no__t-1SOFTWARE @ no__t-2Microsoft @ no__t-3Windows @ no__t-4Currentversion @ no__t-5Policies @ no__t-6System @ no__t-7LocalAccountTokenFilterPolicy**.

**Remarque** Par défaut, SPA écrit les journaux de diagnostic dans le dossier où se trouve SpaConsole. exe. Si SPA est installé dans le dossier Program Files, SPA ne peut écrire le journal que lorsque SpaConsole. exe est exécuté en tant qu’administrateur.

Si vous souhaitez exécuter l’analyse des données sur l’ordinateur de la console, vous devez exécuter SPA en tant qu’administrateur. PLA passe par un chemin de code différent en cas d’exécution sur un ordinateur local, ce qui nécessite des privilèges d’administrateur.

Si vous souhaitez exécuter le pack d’Advisor de SPA IIS sur vos serveurs, vous devez activer la requête WMI et le suivi ETW pour le serveur IIS. Pour ce faire, vous pouvez activer le **suivi** sous le service de rôle **intégrité et diagnostics** , ainsi que les **scripts et les outils de gestion IIS** sous **outils de gestion** du rôle serveur **serveur Web (IIS)** .

 

### <a name="creating-your-first-project"></a>Création de votre premier projet

Une fois que tout est configuré, vous pouvez créer votre premier projet SPA. Comme décrit dans la section précédente, SPA stocke tout ce qui est lié à l’analyse des données de performances à l’intérieur d’une base de données. Chaque projet SPA correspond à une base de données unique. La **première fois que vous utilisez l’Assistant utilisation** , vous guide dans les étapes suivantes.

**Pour créer un projet SPA**

1.  Lancez SpaConsole. exe. La console passe en mode déconnecté, où SPA n’est connecté à aucune base de données et la fenêtre principale est vide.

2.  Pour créer un nouveau projet, cliquez sur **fichier**, puis sur **nouveau projet**. Cette opération lance l’Assistant première utilisation. La première page affiche les étapes à suivre lors de l’utilisation de l’Assistant :

    * création d'une base de données ;

    * Approvisionner Advisor packs

    * Ajouter des serveurs à la liste des serveurs cibles

3.  Cliquez sur **Suivant**. La page **créer une base de données de projet** vous demande de fournir le nom de l’instance de Microsoft SQL Server dans laquelle vous souhaitez créer votre base de données. Par exemple, s’il se trouve sur le même ordinateur que la console, vous pouvez utiliser **localhost @ no__t-1 @ no__t-2YOUR SQL Server Name @ no__t-3**.

    **Remarque** Le nom d’instance par défaut pour une installation SQL Server 2008 R2 Express est SQLExpress. Pour une instance de SQL Server 2008 R2 Express installée sur l’ordinateur local, la base de données est généralement définie par défaut sur **localhost @ no__t-1SQLExpress**. Toutefois, il a peut-être été modifié pendant l’installation de SQL Server. vous devez donc vous assurer que vous utilisez le nom de l’instance de la SQL Server appropriée.

     

4.  Indiquez le nom de la base de données. Seuls les lettres, les chiffres et les traits de soulignement (\_) sont autorisés comme caractères valides pour un nom de base de données. Une suggestion raisonnable pour le nom de la base de données SPA serait **Spa**. Si vous entrez un nom non valide, une icône d’erreur rouge s’affiche. L’info-bulle associée donne la raison de l’échec de la validation.

    **Remarque** Il est IMPORTANT de se souvenir du nom de la base de données et du nom de l’instance de serveur, car il s’agit des seuls identificateurs de votre projet. Vous devez fournir ces informations si vous souhaitez basculer vers cette base de données.

     

5.  Une fois que vous avez fourni le nom de l’instance de serveur et le nom de la base de données, l’Assistant première utilisation génère l’emplacement du fichier de base de données.

6.  Sur la page **créer une base de données de projet** , cliquez sur **suivant**. L’Assistant première utilisation crée une base de données et génère le schéma de base de données, les fonctions et les procédures stockées relatives à SPA dans la base de données. Cette étape peut prendre plusieurs secondes en fonction du matériel et de la vitesse du réseau.

    **Remarque** si cette étape échoue, un message d’erreur s’affiche. Voici quelques-uns des problèmes les plus courants : La console ne peut pas se connecter à l’instance SQL Server, privilèges insuffisants pour créer une base de données ou le nom de la base de données existe déjà.

     

7.  À la suite de l’étape précédente, la page **approvisionner Advisor Pack** s’affiche. Elle répertorie tous les packs d’Advisor disponibles sur votre ordinateur. SPA analyse automatiquement le dossier nommé **APS** dans le répertoire racine Spa. Il répertorie le nom complet, la version et l’auteur pour chaque pack Advisor.

    **Remarque** pour plus d’informations sur l’utilisation du nom complet et de la version dans Spa, consultez [Managing Advisor packs](#bkmk-manageadvisorpacks) (en anglais).

     

8.  Choisissez les packs d’Advisor que vous souhaitez approvisionner dans la base de données de projet, puis cliquez sur **suivant**. Ou vous pouvez cliquer sur **Ignorer** pour passer à l’étape suivante sans configurer d’Advisor packs.

    **Remarque** Vous pouvez approvisionner Advisor packs à chaque fois que vous utilisez l’outil. Pour plus d’informations, consultez [Managing Advisor packs](#bkmk-manageadvisorpacks).

     

9.  Dans la page **Ajouter des serveurs** , pour chaque serveur à ajouter à la liste de serveurs cibles, vous pouvez remplir deux champs obligatoires : **Nom du serveur** et **emplacement du partage de fichiers**.

    **Remarque** Il y a également un champ **Remark** , qui est principalement utilisé pour classifier ou Rechercher le serveur. Dans les cas où vous disposez de nombreux serveurs, vous pouvez importer un fichier de valeurs séparées par des virgules (. csv) qui contient le nom du serveur, le dossier des résultats et le champ de Remarque facultatif. Le champ **Remark** est utilisé pour décrire le serveur et le terme peut être utilisé pour filtrer les serveurs pour la collecte de données. Si vous initialisez les serveurs par le biais du fichier. csv, une erreur d’analyse dans le fichier ne charge pas les serveurs.

     

10. Vous devez configurer plusieurs configurations pour activer la collecte de données PLA, comme décrit dans [configuration de Spa](#bkmk-setupspa). La page **Ajouter un serveur** fournit une fonctionnalité de configuration de test pour vous aider à résoudre les problèmes de configuration. activez la case à cocher associée à l’ordinateur, puis cliquez sur **tester la connectivité**. SPA tente de générer un ensemble de collecteurs de données sur les serveurs cibles et tente d’importer les résultats dans la base de données. Si tout est correct, l' **État** indique **Pass**. En cas d’échec, une info-bulle s’affiche pour décrire la raison de l’échec.

11. Chacun des serveurs est automatiquement ajouté à la base de données, même s’il ne réussit pas le test de configuration. Pour supprimer des serveurs de la liste, sélectionnez le nom du serveur, puis cliquez sur **supprimer**.

12. Lorsque tout est terminé, cliquez sur **Terminer** pour fermer la première fois que l’Assistant utilisera. Si vous fermez la première fois que vous utilisez l’Assistant utiliser pour la première fois, toutes les étapes précédentes persistent et aucune n’est restaurée automatiquement. Vous devez apporter des modifications ultérieures manuellement.

## <a name="running-analysis"></a>Analyse en cours d’exécution


Après avoir configuré la base de données, vous pouvez exécuter l’analyse des performances sur les serveurs.

Chaque fois que la console SPA démarre, le dernier projet qui a été utilisé par l’utilisateur actuel s’ouvre automatiquement. La fenêtre principale contient une liste de serveurs. Chaque serveur possède quatre propriétés : Nom du serveur, résultat de l’analyse, état actuel et remarque.

* **Nom du serveur** Nom du serveur, qui est l’identificateur pour le serveur. Aucun nom en double n’est autorisé.

* **Résultat** de l’analyse Par défaut, il affiche le résultat de la dernière exécution de l’analyse des performances sur le serveur. Si aucune analyse des performances n’a été exécutée sur le serveur, **aucun rapport**n’est affiché. Si un avertissement est émis par le rapport, il affiche un **Avertissement** et l’horodatage de la génération du rapport le plus récent. Si aucun problème n’a été détecté au cours de la dernière analyse sur le serveur, il affiche **OK** et l’horodatage.

    **Remarque** si vous avez récemment modifié un paramètre système, nous vous recommandons d’exécuter à nouveau l’analyse pour évaluer l’impact global de la modification et obtenir un rapport mis à jour de l’état du système. SPA n’effectue pas le suivi des modifications de configuration apportées au système testé.

     

* **État actuel** Affiche l’état des tâches d’analyse des performances en cours d’exécution sur le serveur. Vous pouvez annuler une tâche en cours d’exécution en cliquant sur l’icône d' **annulation** , qui est désignée par un X rouge.

* **Remarque** Décrit le serveur cible actuel. Par exemple, vous pouvez décrire votre serveur à l’aide du rôle serveur (par exemple, SQL Server) ou d’un emplacement (par exemple, Kent). SPA utilise le **nom du serveur** et **Remarque** pour faciliter la recherche et trouver le serveur approprié. Vous pouvez taper dans la zone de texte Rechercher. Si le **nom du serveur** ou les colonnes du **repère** contiennent la chaîne exacte que vous avez entrée dans la zone de recherche, le serveur s’affiche dans la liste serveur.

Les contrôles suivants sont également disponibles sur la console :

* **Répéter** Case à cocher qui décrit la capacité à répéter régulièrement une collection, en fonction d’un intervalle de temps. Pour la plupart des installations de serveur, vous souhaiterez qu’une collection SPA soit répétée toutes les heures pour avoir un historique suffisant pour l’analyse. Si vous souhaitez exécuter la collection une seule fois, vous ne devez pas activer la case à cocher **répéter** .

* **Supprimer la périodicité** Bouton qui vous permet d’annuler une tâche de collecte de répétition en cours. Elle annule la collecte de répétition, mais pas la collection en cours (le cas échéant) est en cours. Cette option vous permet de réinitialiser un nouvel intervalle de collecte de répétition ou d’exécuter le regroupement manuellement.

Avant de démarrer l’analyse des performances, sélectionnez la durée de la collecte de données. Bien que la collecte de données supplémentaires permette d’obtenir une image plus précise de la situation des performances du serveur, elle génère également un plus grand nombre de journaux et peut avoir un impact plus important sur le serveur. Choisissez la durée de collecte de données appropriée en fonction de vos besoins spécifiques. Chaque pack Advisor définit une durée minimale valide. La durée de collecte des données que vous choisissez doit être supérieure à la durée minimale des packs d’Advisor sélectionnés.

Pour exécuter l’analyse des performances sur les serveurs cibles, sélectionnez les serveurs sur lesquels vous souhaitez exécuter l’analyse des performances, puis cliquez sur **exécuter l’analyse** . une boîte de dialogue s’affiche et vous demande de sélectionner les packs d’Advisor à exécuter sur les serveurs. Les Advisor packs sélectionnés s’exécutent simultanément. Les rapports générés sont basés sur les données de performances collectées au cours de la même période.

**Remarque** si vous sélectionnez un serveur sur lequel l’analyse des performances périodiques est en cours d’exécution, le bouton **Supprimer la périodicité** vous permet d’annuler la collecte des données récurrentes. SPA n’autorise pas plusieurs sessions de collecte de données en même temps sur le même ordinateur.

 

## <a href="" id="bkmk-viewingreports"></a>Affichage des rapports


Dans SPA, il existe trois types de rapports d’analyse des performances : Rapport unique, rapport côte à côte et graphiques de tendances et d’historique.

Après l’exécution de l’analyse des performances, un rapport est généré pour chacun des packs d’Advisor exécutés sur l’ordinateur cible. Dans la liste des serveurs de la fenêtre principale, vous pouvez développer résultats de l' **analyse** pour afficher tous les packs d’Advisor qui ont été exécutés sur le serveur concerné. Vous pouvez cliquer sur un nom de rapport pour afficher un rapport unique.

Trois icônes s’affichent en regard du nom du Pack Advisor qui indiquent l’état de la dernière analyse exécutée sur le serveur :

* L’icône **dernier** affiche le rapport qui a été généré par l’analyse des performances la plus récente sur ce serveur pour le Pack Advisor.

* L’icône de **recherche** affiche la liste des rapports d’analyse des performances, qui vous permet de sélectionner le rapport approprié. Les champs **Advisor Pack** et **serveur cible** sont préremplis avec les informations actuelles relatives au pack d’aide et au serveur cible. L’intervalle de temps par défaut est défini sur une semaine, et la date de fin est définie sur aujourd’hui. Si vous cliquez sur le bouton de **recherche** dans l’angle supérieur droit, vous pouvez obtenir la liste de tous les rapports d’analyse des performances pour le serveur sélectionné et le Pack Advisor dans l’intervalle de temps.

* L’icône **afficher les graphiques** ouvre la vue graphique de tendance et historique.

L’illustration suivante montre les icônes des graphiques les **plus récents**, **Rechercher**et **Afficher** après chaque pack Advisor :

![icônes de rapport](../media/server-performance-advisor/spa-user-manual-report-icons.png)

### <a name="searching-for-and-within-reports"></a>Recherche et dans les rapports

La recherche de rapports s’effectue à l’aide de l' **Explorateur de rapports**. Cela vous permet de rechercher des rapports par plage de dates, nom de serveur et conseiller Pack. Il s’agit de la méthode recommandée pour rechercher un rapport d’intérêt autre que le dernier rapport d’exécution. Le rapport de la dernière exécution est disponible via **afficher le rapport** pour ce serveur.

Lorsque vous affichez un rapport spécifique, vous pouvez facilement accéder au rapport suivant et au rapport précédent en fonction de l’heure ou d’un rapport associé, tel qu’un autre point d’accès en cours d’exécution en même temps. Ces options sont disponibles sous **actions**.

Il est également possible d’effectuer des recherches dans un rapport. Un certain nombre de rapports ont une zone de recherche de chaîne disponible **pour une recherche** de chaîne de texte rapide dans le rapport. Pour supprimer la zone de texte, vous pouvez la faire disparaître. Pour activer une zone de recherche (dans les fenêtres qui comportent une recherche de texte), vous pouvez utiliser le raccourci Ctrl + F. La zone de **recherche** permet à l’utilisateur de spécifier une recherche qui respecte la casse, en fonction de l’option **respecter la casse** .

L’illustration suivante montre la zone de recherche **Rechercher** avec la **chaîne sous** l’onglet **rapport** .

![zone Rechercher](../media/server-performance-advisor/spa-user-manual-find-search-box.png)

### <a name="single-report"></a>Rapport unique

Un rapport unique affiche les résultats de l’analyse des performances à partir d’une seule exécution d’un Advisor Pack sur un seul ordinateur. Le rapport affiche le nom de l’Advisor Pack, le nom du serveur cible, l’heure à laquelle le rapport a été généré et la durée de la collecte des données.

Un rapport unique contient une section de notification et les sections de données.

### <a name="notification-section"></a>Section notification

La section notification se compose d’un ensemble de règles d’analyse des performances. Chaque notification contient une source de données, certaines valeurs de seuil et une certaine logique métier. Lorsque vous exécutez l’analyse des performances, la logique métier évalue les sources de données par rapport aux seuils pour déterminer si la règle réussit ou non. Si ce n’est pas le cas, un avertissement s’affiche pour vous informer de la présence d’un problème de performances potentiel. Il fournit également des recommandations pour vous aider à résoudre le problème. La section notification est toujours le premier onglet de la vue rapport unique.

La section notification est divisée en deux parties : **Avertissements** et **autres notifications**.

Si la source de données d’une règle répond à certaines conditions en fonction de la logique et des paramètres de seuil, un avertissement s’affiche dans la zone d' **Avertissement** . Un avertissement comprend les éléments suivants :

* Une icône d’avertissement indique l’existence d’un problème potentiel.

* Nom de la règle. Par exemple, l' **abandon du paquet de réception réseau** est un lien qui pointe vers la page Détails de la règle, comme décrit dans [Managing Advisor packs](#bkmk-manageadvisorpacks).

* Description simple du problème potentiel.

* Recommandation pour une solution possible au problème de performances potentiel.

Les différents serveurs peuvent avoir des modèles de configuration et d’utilisation radicalement différents et il est impossible de définir les seuils et les règles applicables à tous les serveurs sous toutes les conditions. SPA offre la possibilité de modifier les seuils. Vous pouvez également choisir de désactiver une règle si la règle ne s’applique pas à votre scénario. Par défaut, toutes les règles sont activées. Une règle désactivée n’apparaît pas dans la zone de notification. Pour plus d’informations, consultez [Managing Advisor packs](#bkmk-manageadvisorpacks).

La zone **autres notifications** contient toutes les autres règles, où aucun avertissement n’est déclenché ou la règle n’est pas applicable. Il contient des parties similaires à celles figurant dans la zone d' **Avertissement** . La plus grande différence est que si aucun avertissement n’est déclenché ou si la règle n’est pas applicable, généralement aucune recommandation n’est fournie.

### <a name="data-sections"></a>Sections de données

Les sections de données contiennent les données de performances que le Pack Advisor génère en fonction des données brutes collectées sur les serveurs cibles. Les sections de données incluent un ensemble de sections de niveau supérieur et plusieurs niveaux de sous-sections. Les sections de niveau supérieur sont présentées sous forme d’onglets. Toutes les sous-sections sous les sections de niveau supérieur sont présentées dans des zones développables. Vous pouvez réduire ou développer chacune des sections pour vous concentrer sur les domaines d’intérêt, comme illustré dans la figure suivante.

![sections de données](../media/server-performance-advisor/spa-user-manual-data-sections.png)

Le pack d’conseiller du SPA de base du système d’exploitation et le pack d’conseiller du SPA IIS contiennent une section **vue d’ensemble du système** . Cette section comprend des informations de niveau supérieur sur l’utilisation et la configuration des ressources. D’autres sections de niveau supérieur représentent des zones de données de performances. SPA présente les données de rapport des manières suivantes :

* **Valeur unique** Paire clé/valeur. La clé est une chaîne qui représente la signification de la valeur. La valeur peut être une chaîne, une valeur numérique ou une valeur booléenne. Il est souvent utilisé pour afficher des informations statiques, telles que la configuration, par exemple, l’architecture de l’UC, la taille totale de la mémoire et la version du BIOS, qui ne changent pas au fil du temps.

* **valeur de liste** Il s’agit parfois d’une paire clé/valeur, mais la valeur de liste peut contenir plusieurs champs. Par exemple, l’attribut de l’UC peut être affiché dans un tableau comportant plusieurs colonnes et plusieurs lignes. Chaque ligne représente un processeur, et chaque colonne représente un attribut de l’UC.

* **Statistiques** Peut être considéré comme un type spécial de valeur unique. Il ne peut contenir que des données numériques. Au moment de la collecte des données, la plupart des points de données numériques fluctuent au lieu de rester constants. Par exemple, l’utilisation de l’UC change chaque fois que la commande PLA collecte le compteur de performances. L’indication d’une seule valeur ne peut pas refléter avec précision la situation de performance. Au lieu d’n’en montrer qu’une seule valeur, les valeurs moyenne, maximale, minimale et 90% sont utilisées pour ces points de données numériques dynamiques. La valeur 90% représente l’activité au et au-dessus du dixième centile sur tous les événements pour ce compteur dans cet intervalle de collecte donné.

* **Liste supérieure** Contient généralement les principaux consommateurs d’une ressource spécifique ou les entités les plus importantes ayant connu certains événements. Par exemple, les **10 premiers processus en termes d’utilisation moyenne du processeur** incluent les dix principaux processus avec une utilisation moyenne du processeur la plus élevée pendant la collecte des données. Étant donné que l’utilisation du processeur est également un point de données numérique dynamique, d’autres statistiques telles que la valeur maximum, minimum et 90% sont également incluses dans la liste pour fournir à l’utilisateur une image plus complète de la consommation du processeur.

Comme mentionné dans les sections précédentes, SPA s’appuie sur PLA pour collecter les suivis ETW, les requêtes WMI, les compteurs de performances, les clés de Registre et les fichiers de configuration pour générer le rapport. Il est IMPORTANT de comprendre la source de données derrière chaque point de données dans le rapport. SPA fournit ces informations par le biais d’info-bulles. Vous pouvez pointer sur les colonnes ou lignes clés pour afficher l’info-bulle de la source de données. Par exemple, **WMI : Win32 @ no__t-1DisDrive : Caption** signifie que la source de données provient d’une requête WMI, que le nom de classe WMI est Win32 @ No__t-2DiskDrive et que la propriété est **Caption**.

### <a href="" id="side-by-side-report-"></a>Rapport côte à côte

Les rapports uniques fournissent des notifications et une section de données pour aider l’utilisateur à trouver d’éventuels problèmes de performances, mais il est souvent difficile d’identifier un problème de performances potentiel en examinant directement un rapport unique. Un rapport peut contenir un trop grand nombre de points de données, ce qui rend difficile la détection des problèmes potentiels.

Pour résoudre ce problème, SPA offre la possibilité de comparer deux rapports. Vous pouvez comparer un rapport avec un problème potentiel à un rapport de base pour faciliter la recherche des différences.

Les rapports côte à côte peuvent être lancés à partir d’une visionneuse de rapports unique. Les utilisateurs peuvent cliquer sur **actions**, puis sur **comparer les rapports** pour sélectionner les rapports. La comparaison des rapports à partir du même Advisor Pack est uniquement significative. Vous pouvez choisir de comparer le rapport avec un rapport précédent dans le temps, le rapport suivant dans le temps ou un rapport arbitraire sélectionné par le biais des fonctionnalités de recherche. Par exemple, pour isoler un comportement anormal, vous pouvez comparer un rapport de serveur de base à un rapport qui a été généré à une heure différente sur le même ordinateur, ou à un rapport qui a été généré sur un autre ordinateur ayant un rôle de serveur et une charge similaires.

Un rapport côte à côte ressemble beaucoup à un rapport unique. Elle contient une section de notification et des sections de données. Il contient le même nombre de notifications et de sections de données que la visionneuse de rapports unique. La seule différence est que les rapports sont affichés côte à côte. Chaque section contient les données du rapport source (rapport 1) et le rapport de destination (rapport 2). Le rapport côte à côte affiche le nom de l’Advisor Pack, le nom du serveur cible (rapport 1 sur la gauche et le rapport 2 à droite), l’heure à laquelle le rapport a été généré et la durée de la collecte de données pour chaque rapport.

Si vous fermez la boîte de dialogue **Rechercher** , vous pouvez la réactiver en tapant Ctrl + F. Cette boîte de dialogue recherche et met en surbrillance les chaînes de texte dans la section actuelle.

Dans la section notification, si l’un des résultats des deux rapports comparés est un avertissement, il est répertorié dans la zone d' **Avertissement** . Dans le cas contraire, les résultats sont répertoriés dans la zone **autres notifications** . Étant donné que la clé d’un rapport côte à côte consiste à identifier les différences entre les rapports, aucune information détaillée sur une règle ne s’affiche. Pour plus d’informations sur la règle, les utilisateurs peuvent cliquer sur le nom de la règle pour afficher le formulaire détaillé de la règle.

Dans les sections de données, les données sont présentées côte à côte avec les données du rapport 1 sur la gauche et les données du rapport 2 à droite. SPA affiche des valeurs uniques dans la même table, mais au lieu d’étiqueter la **valeur**des colonnes, elles sont nommées **rapport 1** et **rapport 2** , respectivement. Le rapport côte à côte affiche toutes les autres formes de données dans des tables côte à côte.

La visionneuse de rapports côte à côte fournit également des info-bulles sur la source de données.

### <a name="historical-and-trend-charts"></a>Graphiques historiques et de tendances

Il est utile d’afficher les graphiques de tendances et historiques pour un serveur spécifique et un conseiller spécifique. Vous devez choisir l’intervalle de temps (qui est la semaine dernière par défaut), puis cliquer sur **OK** pour afficher la visionneuse de tendances et de graphiques historiques.

La visionneuse de tendances et de graphiques historiques comporte trois onglets : graphique historique, graphique de tendances sur 24 heures et graphique de tendances sur 7 jours.

### <a name="historical-chart"></a>Graphique historique

Le graphique historique affiche une série de valeurs pour un point de données numérique dans le laps de temps donné. Par exemple, la **latence moyenne des demandes** pour IIS sur un serveur unique pour les 15 derniers jours. Chaque point de données d’un graphique historique représente la valeur d’une source de données spécifique, prise dans une session d’analyse des performances.

Il existe deux façons d’utiliser un graphique d’historique :

1.  Pour vous aider à trouver des anomalies à un moment donné pour un point de données par exemple, à 2:00 chaque jour, la **latence moyenne des demandes** d’IIS passe d’environ 200 ms à 500 ms.

2.  Pour aider à mettre en corrélation plusieurs points de données. Par exemple, l’indication de la **latence moyenne des demandes** et du **nombre moyen de demandes** pour les 15 derniers jours. Le rapport peut indiquer que la latence des demandes et le nombre de demandes augmentent en même temps, ce qui peut indiquer que l’augmentation de la latence des demandes est due à une augmentation du nombre de demandes.

Dans un graphique historique, les utilisateurs peuvent effectuer les opérations suivantes :

* Afficher plusieurs séries de données dans la zone de graphique. Chaque série de données est affichée sous la forme d’un graphique en courbes dans la visionneuse de rapports. Chaque graphique en courbes est automatiquement mis à l’échelle pour s’ajuster à la visionneuse de rapports.

* Ajoutez ou supprimez une série de données de la liste série de données en bas de la visionneuse de graphiques historiques.

* Affichez ou masquez une série de données dans la liste des séries de données. Les utilisateurs peuvent cliquer sur une série de données spécifique dans la liste pour mettre en surbrillance le graphique en courbes correspondant dans la zone de graphique.

* Permet d’effectuer un zoom avant sur une période donnée en sélectionnant la période à l’intérieur de la zone de graphique. Pour effectuer un zoom arrière, cliquez sur le bouton situé dans le coin inférieur gauche du graphique.

* Examinez un rapport unique en double-cliquant sur un point de données particulier.

* Copiez les données et rendez-les disponibles pour d’autres programmes, tels que Microsoft Excel. Cela vous permet d’utiliser les fonctionnalités de création de graphiques de Microsoft Excel, le cas échéant.

### <a name="trend-charts"></a>Graphiques de tendances

De nombreux problèmes de performances répétitifs sont dus à des tâches périodiques exécutées sur ou sur des serveurs cibles. Par exemple, un site Web orienté divertissement peut obtenir plus d’accès pendant le week-end, ou une tâche de sauvegarde de disque planifiée peut entraîner des performances de serveur chaque jour à 2:00 AM.

Un graphique de tendances est conçu pour vous aider à trouver de tels problèmes de performances. Des problèmes de performances peuvent se produire de façon répétée dans différents modèles. Les modèles les plus courants sont les modèles quotidiens et les modèles hebdomadaires dans lesquels des problèmes de performances se produisent pendant la même heure d’un jour ou d’un même jour de la semaine. Par conséquent, SPA fournit un graphique de tendances de 24 heures et un graphique de tendances de 7 jours.

Le graphique d’analyse de tendances fournit un niveau d’investigation plus approfondi sur un ensemble de données et recherche des tendances en fonction de l’heure de la journée. L’axe des X est défini sur une période de 24 heures, à partir de 0:00 (minuit) et jusqu’à 23:59. SPA n’affiche pas les tendances de plusieurs séries de données en même temps. Vous pouvez cliquer sur **choisir une série de données** pour sélectionner une série de données à afficher.

Pour traiter les données, SPA recherche toutes les captures instantanées prises entre 0:00 et 0:59 pour chaque heure. SPA détermine les valeurs minimale, maximale, moyenne et Sigma pour l’ensemble des captures instantanées prises au cours de cette heure, et les graphiquement sous forme de graphiques en bougies. SPA répète le processus pour les captures instantanées effectuées entre 1:00 à 1:59, 2:00 à 2:59, et ainsi de suite. S’il n’existe pas d’instantanés pour l’heure donnée, SPA laisse cette heure vide sur le graphique et passe à l’heure suivante.

Un graphique de tendances de 7 jours est très similaire au graphique de tendances de 24 heures. La seule différence est qu’elle regroupe une série de données en fonction du jour de la semaine au lieu de l’heure de la journée.

Les séries de données que vous sélectionnez dans les graphiques de tendances et historiques sont stockées en tant que préférences de l’utilisateur. Lors de la prochaine ouverture de la tendance et de la visionneuse de graphique historique pour le même conseiller Advisor Pack, le même ensemble de séries de données est répertorié comme étant la valeur par défaut.

## <a name="managing-reports"></a>Gestion des rapports


### <a name="deleting-reports"></a>suppression de rapports

Les rapports peuvent être supprimés pour réduire le nombre de rapports qui doivent être gérés par SPA. Selon la fréquence des rapports et le nombre de serveurs, nous vous recommandons de supprimer les rapports inutiles. Bien que SPA n’ait pas de limite sur les rapports qu’il peut gérer, la base de données sous-jacente peut avoir une limite de taille.

**Notez** que vous ne pouvez pas annuler la suppression des rapports supprimés.

 

### <a name="exporting-and-importing-reports"></a>Exportation et importation de rapports

Les rapports peuvent être exportés vers un fichier XML pour être transportés vers une autre console SPA ou par courrier électronique à un autre utilisateur. L’exportation du rapport ne supprime pas le rapport. Pour exporter le rapport actuellement affiché, dans la **visionneuse de rapports**, cliquez sur **actions**, puis sur **Exporter**. Pour exporter plusieurs rapports, à partir de l' **Explorateur de rapports**, cliquez sur **activer la sélection multiple**, sélectionnez plusieurs rapports dans la zone de sélection, puis cliquez sur **Exporter**. Cela exporte les rapports au format XML dans le répertoire de destination sélectionné.

Un rapport exporté peut être affiché dans SPA. les rapports importés ne sont pas ajoutés à la base de données SPA. Ils sont principalement destinés à servir d’application de visionneuse XML pour le rapport exporté. Le serveur du rapport importé n’a pas besoin d’installer les mêmes packs d’Advisor que la console du rapport d’origine à l’exportation SP.

## <a href="" id="bkmk-manageadvisorpacks"></a>Gestion des packs d’Advisor


SPA comprend des packs d’conseils pour le système d’exploitation principal, Hyper-V, Active Directory et IIS. SPA offre une architecture ouverte pour le développement d’Advisor packs à l’aide de SQL. il est donc également possible pour les développeurs non-Microsoft de créer des versions d’Advisor packs. Quatre options sont disponibles pour la gestion d’un Advisor Pack : approvisionner, personnaliser, réinitialiser ou supprimer.

### <a name="provision-new-advisor-packs"></a>Approvisionner de nouveaux Advisor packs

De nouveaux Advisor packs peuvent être publiés par Microsoft ou par des développeurs non-Microsoft. Un Advisor Pack comprend un provisionMetaData. xml et un ensemble de scripts SQL qui décrivent la logique.

**Pour approvisionner un nouveau conseiller Pack**

1.  Copiez tout le contenu du Advisor Pack sous le répertoire *% SpaRoot%* \\APs.

2.  Dans la fenêtre principale, cliquez sur **configuration**, puis sur **configurer Advisor packs**. La boîte de dialogue **configurer les packs d’Advisor** s’ouvre.

    **Remarque** Cette boîte de dialogue est similaire à la page **approvisionner Pack Advisor** de la première fois que vous utilisez l’Assistant utilisation. Elle affiche une liste des packs d’Advisor qui peuvent être gérés. Chaque Advisor Pack dans la liste possède des propriétés telles que nom, version installée, version et auteur. Nom est le nom complet de l’Advisor Pack, et version installée est la version de ce pack Advisor qui a déjà été approvisionnée dans le projet. Si le Pack Advisor n’est pas approvisionné dans la base de données actuelle, la zone de texte version installée affiche **non installé**. Le champ version indique la version de cet Advisor Pack, qui est classée sous le dossier Advisor packs.

     

3.  Sélectionnez le Pack Advisor dans la liste. Si le Pack Advisor n’a pas été approvisionné ou s’il existe une version plus récente dans le dossier Advisor packs que celle de la base de données, le bouton **approvisionner** est activé. Cliquez sur le bouton **approvisionnement** .

4.  Lorsque la configuration est terminée, le champ **version installée** de l’Advisor Pack sélectionné contient les nouvelles informations sur la version.

### <a name="customize-advisor-packs"></a>Personnaliser Advisor packs

SPA définit une architecture ouverte qui permet aux utilisateurs de modifier les packs d’Advisor. Les utilisateurs peuvent modifier les fichiers Advisor Pack en modifiant les seuils, en partageant les seuils et en activant ou désactivant des règles.

Pour plus d’informations sur la façon de modifier et de créer des packs d’Advisor, consultez le [Guide de développement de Server Performance Advisor Pack](server-performance-advisor-pack-development-guide.md).

### <a name="changing-thresholds"></a>Modification des seuils

Les seuils sont utilisés dans SPA pour déterminer si la condition de déclencheur d’une règle est remplie. Les seuils réels pour les scénarios clients réels peuvent varier considérablement en raison de la charge de travail, de l’environnement matériel et des besoins de l’entreprise. Les seuils par défaut peuvent ne pas convenir au cas de l’utilisateur actuel. par conséquent, SPA offre la possibilité de modifier le seuil existant.

Vous pouvez modifier les valeurs de seuil en cliquant sur le nom de la règle dans un rapport unique ou côte à côte. Ou pour gérer toutes les règles d’un conseiller Advisor Pack, il peut modifier les valeurs de seuil à partir du menu **configuration** .

**Pour modifier une valeur de seuil**

1.  Dans le menu **configuration** , cliquez sur **configurer Advisor packs**, cliquez sur le nom du Pack Advisor à modifier, puis cliquez sur **configurer**.

    **Remarque** Une liste de toutes les règles incluses dans Advisor Pack s’affiche. La case à cocher à gauche du nom du Pack Advisor indique si la règle est activée. Si une règle est désactivée, elle est masquée dans tous les rapports.

     

2.  Cliquez sur la règle spécifique que vous souhaitez modifier. Le formulaire **Détails** de la règle de la règle sélectionnée s’ouvre.

Le formulaire Détails de la règle contient des informations détaillées sur une règle spécifique. Il comprend le nom, la description, l’État, les résultats possibles et les seuils. SPA prend en charge deux types de résultats de règle, **Warning** et **OK**. Pour chaque type, il existe un texte de recommandations et une recommandation.

Certaines règles n’ont pas de seuils définis. Par exemple, la règle **http Keep Alive** recherche un paramètre booléen pour IIS. La liste des seuils peut donc être vide. Dans le cas contraire, tous les seuils utilisés par la règle actuelle sont répertoriés. Une description détaillée de la façon dont un seuil est utilisé dans la règle est incluse dans le cadre de la description.

Si le formulaire Détails de la **règle** est lancé à partir du menu **configuration** , la liste des seuils comporte trois colonnes : nom, paramètre d’origine et modifier le paramètre. S’il est lancé à partir d’un rapport unique ou côte à côte, les valeurs de seuil utilisées par le rapport sont également incluses. Les utilisateurs peuvent modifier les valeurs de seuil actuelles en modifiant la valeur dans la colonne **modifier le paramètre** , puis en cliquant sur **Enregistrer** pour enregistrer les modifications apportées à la base de données.

Toutes les modifications apportées aux seuils sont appliquées uniquement aux rapports générés après les modifications. Les rapports existants ne sont pas affectés par ces modifications.

### <a name="sharing-thresholds"></a>Seuils de partage

Si vous gérez vos serveurs dans des situations similaires, vous pouvez choisir d’utiliser le même jeu de seuils. Vous pouvez exporter et importer des seuils pour un conseiller Advisor spécifique à l’aide du menu **configuration** . Vous pouvez sélectionner l’Advisor Pack spécifique, puis cliquer sur **configurer**. Le fichier de seuil exporté est au format XML.

Lors de l’importation d’un seuil, SPA valide le format de fichier XML et vérifie que le fichier correspond à l’Advisor Pack sélectionné. En cas de réussite, SPA importe toutes les valeurs du fichier de seuil dans la base de données de projet actuelle. Comme pour le scénario de seuils de modification précédent, toutes les modifications de valeur de seuil prennent effet uniquement sur les rapports qui sont générés à l’avenir. Les rapports existants ne sont pas affectés.

### <a name="enable-or-disable-rules"></a>Activer ou désactiver des règles

Une règle peut être activée ou désactivée dans le formulaire Détails de la **règle** . Vous devez cliquer sur **Enregistrer** pour conserver les modifications apportées. Si une règle est désactivée, elle ne s’affiche dans aucun des rapports. Mais la logique métier sous-jacente est déclenchée lors de la génération du rapport. par conséquent, lorsque vous choisissez de réactiver la règle, celle-ci s’affiche à nouveau dans les rapports.

### <a name="reset-advisor-packs-to-original-state"></a>réinitialiser les packs d’Advisor à l’état d’origine

Vous pouvez décider de modifier un Advisor Pack approvisionné dans la base de données. En dehors de la modification des seuils, vous pouvez également modifier les scripts SQL. SPA ne prend pas en charge la modification des métadonnées de rapport, ni l’ajout ou la suppression de règles pour un Advisor Pack approvisionné. La modification manuelle de ces zones peut entraîner un comportement inattendu.

Pour plus d’informations sur la modification d’un pack Advisor configuré, consultez le [Guide de développement de Server Performance Advisor Pack](server-performance-advisor-pack-development-guide.md).

Si vous souhaitez restaurer les modifications apportées à un conseiller configuré, vous pouvez choisir de réinitialiser l’Advisor Pack. Cette action remplace tous les scripts SQL qui sont associés au Pack Advisor et rétablit toutes les valeurs des seuils par défaut. Cela permet de conserver tous les rapports existants.

la réinitialisation de Advisor Pack peut être effectuée à l’aide du formulaire **configurer Advisor packs** . Vous devez sélectionner le pack d’Advisor à réinitialiser, puis cliquer sur **Réinitialiser**.

### <a name="remove-advisor-packs"></a>supprimer Advisor packs

Quand un pack Advisor n’est plus nécessaire, les utilisateurs peuvent le supprimer de la base de données. la suppression de l’Advisor Pack supprime tout le contenu de la base de données du Pack Advisor, y compris les règles et les seuils, tous les scripts SQL et tous les rapports. Aucune des actions ne peut être annulée.

la suppression de Advisor Pack peut être effectuée à l’aide du formulaire **configurer Advisor packs** . Vous devez sélectionner le Pack Advisor à supprimer, puis cliquer sur annuler l' **approvisionnement**.

### <a name="update-existing-advisor-packs"></a>Mettre à jour les packs Advisor existants

La mise à jour des packs d’Advisor existants est très similaire à la réinitialisation de l’outil Advisor Pack à son état d’origine. Pour mettre à jour Advisor Pack vers une version plus récente, copiez le nouveau conseiller Pack dans le dossier Advisor Pack. Un Advisor Pack est considéré comme une mise à jour d’un pack Advisor existant uniquement en l’absence de modification des métadonnées du rapport. Le rapport et les ID de règles doivent être identiques. Dans le cas contraire, ils doivent être traités comme deux packs d’Advisor différents.

s’il existe uniquement des modifications de logique métier et qu’aucune modification de métadonnées de rapport n’est apportée à un Advisor Pack, un nouveau numéro de version doit lui être attribué, par exemple de 1,0 à 2,0. En cas de modification des métadonnées d’un rapport, le nom complet du Pack Advisor doit être différent. Par exemple, Microsoft. ServerPerformanceAdvisor. IIS. v1 peut être remplacé par Microsoft. ServerPerformanceAdvisor. IIS. v2.

Si une version plus récente du Pack Advisor existe, la liste figurant dans le formulaire **configurer Advisor packs** remplit automatiquement la colonne **version** avec la dernière version de Advisor Pack. Vous pouvez sélectionner le Pack Advisor, puis cliquer sur la **réinitialisation**. Le Pack Advisor est mis à jour avec la nouvelle logique métier et les nouveaux seuils. Tous les rapports de ce pack Advisor sont conservés.

## <a name="managing-servers"></a>Gestion des serveurs


SPA offre des fonctionnalités de base pour la gestion des serveurs cibles. Vous pouvez choisir d’ajouter de nouveaux serveurs à la liste de serveurs cibles, de supprimer des serveurs de la liste ou de modifier des notes pour les serveurs.

**Pour ajouter ou supprimer des serveurs ou modifier les informations du serveur**

1.  Cliquez sur le menu **configuration** , puis sur **configurer les serveurs**.

2.  Dans la liste des serveurs qui existent actuellement dans la base de données de projet, la dernière ligne est vide. Cliquez sur la ligne et renseignez les champs. Les champs **nom du serveur** et emplacement du partage de **fichiers** sont obligatoires et le nom du serveur doit être unique.

3.  Entrez d’autres informations sur le serveur dans la colonne **Remark** pour chaque serveur.

    **Remarque** Ce champ utilise un format de texte libre. vous pouvez donc l’utiliser comme champ de description. Vous pouvez aussi utiliser ce champ pour baliser les serveurs afin qu’ils puissent être facilement trouvés dans la fenêtre principale, ou pour regrouper des serveurs, par exemple, par emplacement ou rôle de serveur.

     

4.  Si vous souhaitez utiliser un SPA avec un grand nombre de serveurs, SPA prend en charge un format de valeurs séparées par des virgules (. csv) pour l’importation. Le fichier doit contenir au moins deux champs : Emplacement du **partage de fichiers**et du **serveur** . Le troisième champ, **Remark** est facultatif, mais il est recommandé d’organiser vos serveurs. Vous pouvez également exporter la liste de serveurs dans un fichier. csv pour déterminer le format approprié ou sauvegarder la configuration de votre serveur.

### <a name="searching-and-filtering"></a>Recherche et filtrage

Si vous gérez plus de quelques serveurs, SPA offre une prise en charge de base pour trouver rapidement les serveurs dans la fenêtre principale. Vous pouvez cliquer sur l’en-tête de colonne pour trier en fonction du nom du serveur, des résultats de l’analyse, de l’état actuel de la tâche ou des notes. Vous pouvez également choisir d’utiliser la fonctionnalité de recherche. Dans le coin supérieur droit de la fenêtre principale, vous pouvez taper une chaîne à rechercher. La liste **serveur cible** dans la fenêtre principale utilise la chaîne pour filtrer les serveurs et pour afficher uniquement les serveurs avec les champs nom ou remarque qui contiennent la chaîne recherchée.

La figure suivante montre comment la chaîne **Dell** correspond aux serveurs dont la chaîne est **Dell** ou les serveurs avec le champ **Remark** contenant **Dell**.

![exemple de recherche et de filtrage](../media/server-performance-advisor/spa-user-manual-find-search-example.png)

## <a name="advanced-functionality"></a>Fonctionnalités avancées


### <a name="working-with-spa-windows-powershell-cmdlets"></a>Utilisation des applets de commande Windows PowerShell SPA

La console du SPA prend en charge l’interface utilisateur pour la collecte de données périodique. Si cette fonctionnalité n’est pas suffisante pour votre environnement, des applets de commande Windows PowerShell peuvent être utilisées par un administrateur avancé pour personnaliser la collecte des données. Ces applets de commande Windows PowerShell permettent aux administrateurs système d’exécuter automatiquement l’analyse des performances sur les serveurs cibles et d’utiliser SPA pour le support client distant. Par exemple, les administrateurs système peuvent écrire des scripts pour appeler des applets de commande SPA dans certains intervalles de temps pour échantillonner périodiquement la condition de performance des serveurs cibles.

Nous vous recommandons de fermer l’application SPAConsole. exe avant d’utiliser ces applets de commande.

Avant d’exécuter des applets de commande Windows PowerShell, vous devez enregistrer les applets de commande sur l’ordinateur de la console.

**Pour inscrire les applets de commande Windows PowerShell**

1.  À partir d’une invite de commandes Windows PowerShell avec élévation de privilèges, tapez **registerSpaCmdlets. cmd**. Le message **inscrire les applets de commande Spa avec succès** s’affiche.

2.  Exécutez **Spa-PowerShell. cmd**. Si vous transmettez le chemin d’accès à un fichier de script Windows PowerShell, il exécute automatiquement les scripts. Dans le cas contraire, il ouvre une invite de commandes Windows PowerShell, qui est prête à exécuter des applets de commande Windows PowerShell SPA.

Le tableau suivant décrit les applets de commande Windows PowerShell du SPA :

| Nom de l’applet de commande | Paramètres | Description |
| ------ | ------- | ------ |
| Start-SpaAnalysis | **-ServerName** Nom du serveur cible.<br>**-AdvisorPackName** Nom complet du pack d’Advisor à mettre en file d’attente sur le serveur. Lorsque plusieurs packs sont planifiés pour s’exécuter en même temps, la valeur du paramètre doit être mise en forme en tant que AP1name, AP2name.<br>**-Duration** Durée de la collecte de données.<br>**-Informations d’identification** Informations d’identification de l’utilisateur pour le compte qui exécute la collecte de données sur le serveur cible.<br>**-SQLInstanceName** Nom de l’instance de SQL Server.<br>**-SqlDatabaseName** Nom de la base de données du projet SPA. | Démarre une session de collecte de données SPA sur le serveur spécifié. |
| Stop-SpaAnalysis | **-SQLInstanceName** Nom de l’instance de SQL Server.<br>**-SqlDatabaseName** Nom de la base de données du projet SPA.<br>**-ServerName** Nom du serveur cible. | Tente d’arrêter une session SPA en cours d’exécution. Si une session est déjà terminée, elle retourne sans rien faire. |
| SpaServer | **-SQLInstanceName** Nom de l’instance de SQL Server.<br>**-SqlDatabaseName** Nom de la base de données du projet SPA. | Obtient la liste de serveurs dans la base de données. Elle retourne une liste d’objets, y compris les propriétés suivantes : Name, Status, FileShare et ReMark. |
| SpaAdvisorPacks | **-SQLInstanceName** Nom de l’instance de SQL Server<br>**-SqlDatabaseName** Nom de la base de données du projet SPA | Obtient la liste d’avis Pack dans la base de données. Elle retourne une liste d’objets, y compris les propriétés suivantes : Name, DisplayName, Author et version. |

Windows PowerShell permet de transmettre des informations d’identification par le biais de fichiers chiffrés pour permettre des scénarios d’automatisation. Pour plus d’informations sur l’utilisation de fichiers chiffrés pour transmettre des informations d’identification à une applet de commande, consultez [créer des scripts Windows PowerShell qui acceptent les informations d’identification](https://technet.microsoft.com/magazine/ff714574.aspx).

### <a name="automating-spa-report-collection-by-using-windows-powershell"></a>Automatisation de la collecte de rapports SPA à l’aide de Windows PowerShell

Les procédures suivantes représentent un exemple d’automatisation d’une collection de rapports SPA à l’aide des applets de commande Windows PowerShell SPA.

Les informations d’identification de l’utilisateur peuvent être chiffrées et mises en cache via Windows PowerShell. Ces informations d’identification sont utilisées pour se connecter aux serveurs distants. Bien que non spécifique à SPA, l’exemple suivant illustre cette technique :

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

Avant de pouvoir exécuter ce fichier, vous devez exécuter **Set-ExecutionPolicy RemoteSigned** .

Vous pouvez l’exécuter à partir d’un fichier de commandes à l’aide de la commande suivante :

``` syntax
PowerShell -command "& '.\RunSpa.ps1' "
```

### <a name="use-t-sql-to-generate-reports"></a>Utiliser T-SQL pour générer des rapports

Les données de rapport SPA peuvent être extraites à l’aide de SQL pour créer des rapports personnalisés qui ne sont pas fournis dans SPAConsole. exe.

par exemple, la commande T-SQL suivante fournit une liste des 10 premiers serveurs par UC pour la période couvrant les trois derniers jours :

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

### <a name="working-with-multiple-projects"></a>Utilisation de plusieurs projets

Dans certains cas, vous souhaiterez peut-être partitionner les bases de données de SQL Server utilisées par SPA. Cela peut aider à réduire la taille des données de la base de données SQL Server ou vous aider à partitionner logiquement les serveurs. Dans ce cas, la console du SPA prend en charge plusieurs projets. Vous pouvez créer une nouvelle base de données de projet en cliquant sur **fichier**, puis sur **nouveau projet**. La première fois que l’Assistant utilisation s’affiche pour vous aider à créer la nouvelle base de données de projet.

Une fois les projets créés, vous pouvez cliquer sur **fichier**, puis sur **ouvrir un projet** pour basculer entre les projets. La console du SPA n’autorise pas le basculement des bases de données lorsque l’analyse des performances en suspens s’exécute dans le projet actuellement ouvert. Cela permet de protéger l’intégrité des données de performances.

Lorsque vous choisissez de créer une nouvelle base de données de projet ou d’ouvrir une autre base de données de projet, le projet actif est fermé. Tous les rapports ouverts sont fermés lorsque le projet actuel est fermé.

**Remarque** SQL Server 2008 R2 Express a une limite de base de données de 10 Go. En utilisant plusieurs projets, vous pouvez utiliser une ou plusieurs bases de données SQL Server et rester sous la limite de 10 Go SQL Server 2008 R2 Express.

 

### <a name="logging-and-debugging"></a>Journalisation et débogage

SPA offre des fonctionnalités de journalisation de base. Il autorise uniquement l’écriture des journaux dans un fichier journal, qui se trouve dans le même dossier que SPAConsole. exe. Le compte d’utilisateur qui exécute la console SPA doit disposer d’une autorisation d’écriture sur le dossier où le SPA a été installé pour s’assurer que les journaux peuvent être écrits dans le fichier journal.

SPA contient les niveaux de journal valides suivants :

* **Informations** Vide les journaux pour chaque action prise par la console du SPA et est principalement conçu à des fins de débogage.

* **Avertissement** Journalise toutes les défaillances et les exceptions qui se produisent à l’intérieur de la console du SPA. Certains des échecs sont simplement des échecs de validation qui peuvent être gérés par la console du SPA.

* **Tâches critiques** Journalise uniquement les échecs et les exceptions qui ne peuvent pas être gérés par la console du SPA. Ces échecs entraînent le blocage de la console du SPA. Les journaux fournissent des informations de contexte pour de tels échecs.

Par défaut, le niveau de journal est Warning, ce qui signifie que SPA ne journalise que les échecs et les exceptions qui se produisent dans SPA. Vous pouvez modifier le niveau de journalisation en modifiant le fichier **SpaConsole. exe. config** dans le même dossier que SpaConsole. exe. Tous les journaux sont écrits dans le fichier log. txt dans le même dossier.

SPA offre également des fonctionnalités de base pour le débogage. Pour activer le débogage pour SPA, les utilisateurs doivent modifier manuellement la base de données du projet SPA. Le paramètre est stocké dans une table de configurations. L’utilisateur doit exécuter le script SQL suivant pour changer le projet SPA en mode débogage :

``` syntax
UPdate [Configurations] SET Value = N'true' WHERE Name = N'Debugmode'.
```

En mode débogage, l’infrastructure SPA conserve toutes les données temporaires générées au cours de l’analyse des performances pour vous permettre de résoudre les problèmes dans les packs d’Advisor. Ce script est principalement conçu pour être utilisé par les développeurs Advisor Pack.

Pour plus d’informations sur les fonctionnalités de débogage dans SPA, consultez le [Guide de développement de Server Performance Advisor Pack](server-performance-advisor-pack-development-guide.md).

### <a name="managing-the-database"></a>Gestion de la base de données

### <a name="backup-and-restore-the-database"></a>Sauvegarde et restauration de la base de données

La console SPA ne fournit pas de fonctionnalités permettant de sauvegarder et de restaurer la base de données SPA. Vous devez utiliser des outils et des scripts de Microsoft SQL Server standard pour sauvegarder et restaurer des bases de données.

### <a name="clean-up-the-database"></a>nettoyer la base de données

La taille des bases de données du projet SPA peut augmenter en fonction de l’exécution d’une analyse des performances. Par défaut, SPA conserve tous les rapports. Vous souhaiterez peut-être supprimer les anciens rapports pour améliorer les performances. Vous pouvez supprimer des rapports par le biais de l’interface de l' **Explorateur de rapports** .

## <a name="privacy-and-security"></a>Confidentialité et sécurité


SPA prend uniquement en charge la collecte de données à partir des serveurs cibles vers la console du SPA. Elle n’est pas conçue pour envoyer des informations à des développeurs Microsoft ou non-Microsoft. Pour plus d’informations sur la confidentialité du SPA, reportez-vous aux termes du contrat de licence logiciel Microsoft pour Server Performance Advisor.

Toutes les données collectées par SPA sont stockées dans les bases de données de projet. En raison de la nature des traces ETW, SPA peut collecter des informations sensibles qui pourraient avoir une grande importance pour l’entreprise. Vous devez être conscient du risque potentiel associé au partage de l’accès aux bases de données du projet SPA. Les fichiers journaux temporaires sont enregistrés dans les dossiers partagés qui sont spécifiés sur chaque ordinateur cible. Même si SPA tente de supprimer ces fichiers journaux temporaires lorsque l’importation des données est terminée, il n’y a aucune garantie que ces fichiers journaux soient toujours supprimés. Vous devez vous assurer que les dossiers partagés se trouvent dans des emplacements sécurisés.

SPA Advisor packs contient des scripts SQL pour analyser et analyser les journaux de performances afin de générer des rapports de performances. SPA tente de limiter le privilège sous lequel s’exécutent ces scripts. Toutefois, il est toujours possible que les scripts puissent collecter des informations sensibles via SPA à partir des serveurs cibles, ou obtenir ou modifier des informations sensibles stockées dans la même base de données de projet SPA. Vous devez vérifier que tous les packs d’Advisor approvisionnés dans la base de données de projet SPA proviennent de sources approuvées.

## <a name="errors-and-troubleshooting"></a>Erreurs et résolution des problèmes


### <a name="spaconsoleexe-does-not-start-or-write-log-file"></a>SPAConsole. exe ne démarre pas ou n’écrit pas de fichier journal

Lorsque vous essayez d’exécuter SPAConsole. exe pour la première fois, si le .NET Framework n’est pas installé, l’application ne démarre pas ou n’écrit pas de fichier journal. Assurez-vous qu’un Framework compatible.NET est installé et qu’il fonctionne correctement avant de démarrer SPA.

### <a name="locating-log-information"></a>Recherche des informations du journal

SPA stocke les informations d’échec dans le fichier log. txt sous le dossier SPA. des messages d’erreur et des informations de la pile des appels détaillés sont écrits dans ce dossier. Si vous rencontrez des erreurs qui requièrent des informations supplémentaires à interpréter, vous pouvez ouvrir le fichier log. txt pour afficher les détails de l’erreur.

### <a name="database-size-limitations-for-sql-server-express"></a>Limitations de la taille des bases de données pour SQL Server Express

SQL Server Express a une limite de taille de 10 Go pour une base de données utilisateur. Cette base de données de taille a la capacité de stocker 20 000 à 30 000 rapports. Pour réduire la possibilité d’atteindre cette limite de base de données, nous vous recommandons de supprimer régulièrement les rapports et/ou d’utiliser une autre version de SQL Server qui n’a pas la limite de base de données utilisateur de 10 Go.

### <a name="sql-server-express-log-size-and-disk-capacity"></a>Taille du journal SQL Server Express et capacité du disque

Si vous utilisez SQL Server Express, la base de données utilisateur est limitée à 10 Go, mais le fichier journal correspondant peut dépasser 70 Go. Pour ces raisons, nous vous recommandons d’utiliser au moins 100 Go d’espace disque libre pour SQL Server Express. Cet espace disque doit être suffisant pour stocker environ 20 000 à 30 000 rapports. Ce fichier journal est nommé SPADB\_log.ldf et se trouve dans **% Program Files% \\Microsoft SQL Server @ no__t-3MSSQL10. SQLExpress @ no__t-4MSSQL @ no__t-** 5DatA.

### <a name="failure-to-connect-to-target-server"></a>Échec de la connexion au serveur cible

Lorsque vous exécutez l’analyse des performances sur des serveurs cibles, le compte d’utilisateur qui exécute la console SPA doit disposer de certains privilèges pour mettre en file d’attente l’ensemble de collecteurs de données sur PLA sur les serveurs cibles. Les paramètres du pare-feu Windows doivent également être modifiés pour permettre le transfert des communications PLA. Pour obtenir des instructions de configuration détaillées, consultez [configuration de Spa](#bkmk-setupspa).

### <a name="failure-to-create-pla-data-collection-set-on-the-target-server"></a>Impossible de créer le jeu d’ensembles de collecte de données PLA sur le serveur cible

Si vous recevez un message Impossible de créer un jeu d’éléments de collecte de données PLA sur le serveur cible, assurez-vous d’effectuer les opérations suivantes :

* Assurez-vous que le service **journaux de performances & alertes** est en cours d’exécution

* Le paramètre de sécurité @no__t 0Network l’accès : N’autorisez pas le stockage des mots de passe et des informations d’identification pour l’authentification réseau @ no__t-0 est désactivé. Le paramètre de sécurité doit être désactivé, car le SPA doit utiliser les informations d’identification de l’utilisateur pour créer le jeu d’informations de collecte de données sur le serveur cible.

### <a href="" id="running-spa-against-the-console-"></a>Exécution de SPA sur la console

PLA passe par un canal différent si le serveur cible est le même que la console du SPA. Même si le compte d’utilisateur exécute la console SPA avec des privilèges d’administrateur, PLA échoue. Si la console du SPA est installée sur un serveur cible, les utilisateurs doivent lancer SPA en tant qu’administrateur pour s’assurer que la tâche d’analyse des performances peut s’exécuter sur la console.

### <a name="running-multiple-consoles-at-the-same-time"></a>Exécution de plusieurs consoles en même temps

SPA ne prend pas en charge plusieurs consoles exécutées simultanément sur la même base de données de projet SPA. Le SPA ne fournit pas non plus le mécanisme de verrouillage et de synchronisation pour éviter qu’il ne se produise. Si deux consoles SPA s’exécutent en même temps, la console se comportera de façon incohérente en fonction de la séquence d’exécution des consoles SPA. Pour éviter cela, toutes les sessions d’analyse des performances en attente doivent être supprimées de la liste avant d’être traitées par la console qui démarre l’analyse.

SPA protège l’intégrité de chaque rapport généré avec succès par SPA. en même temps, SPA ne garantit pas que toutes les tâches d’analyse en file d’attente sont terminées. Si vous constatez des modifications d’État incohérentes pour les sessions d’analyse des performances ou les erreurs qui affirment que le système ne trouve pas les journaux de performances générés par l’ensemble de collecteurs de données, il est probable que plusieurs instances de console SPA s’exécutent sur le même Base de données de projet SPA.

L’exécution d’applets de commande Windows PowerShell SPA peut également être affectée par une console SPA exécutée sur la même base de données SPA. Nous vous recommandons de fermer la console du SPA avant d’exécuter les applets de commande Windows PowerShell du SPA.

### <a name="spaconsoleexe-recurring-collection-is-disrupted"></a>La collecte périodique SPAConsole. exe est interrompue

Lorsque vous exécutez SPAConsole. exe et que vous utilisez une collecte de données récurrente (par exemple, une collection horaire), le serveur qui exécute le SPAConsole. exe ne doit pas être en mode d’économie d’énergie, de sorte qu’il peut être suspendu. SPA ne vérifie pas la stratégie d’économie d’énergie. Cette activité interrompue peut interrompre la collecte de données périodiques régulières.

### <a name="lost-etw-events"></a>Événements ETW perdus

Pour créer un impact minimal sur les performances sur les serveurs cibles, PLA est conçu pour s’exécuter avec une priorité basse, alors qu’il collecte des informations sur les performances. Si le serveur cible est occupé, PLA peut supprimer certaines tâches de collecte de données à transmettre aux tâches de haute priorité qui s’exécutent sur les serveurs cibles. Vous devez envisager de configurer le dossier partagé dans lequel les événements sont écrits sur un disque qui n’est pas en conflit avec les e/s de la charge de travail ou un lecteur plus rapide, tel qu’un SSD. Lorsque des événements sont supprimés, ils sont signalés dans la vue rapport, car les événements perdus peuvent avoir un impact sur la fiabilité des mesures de performances générées.

Certains problèmes d’autorisation peuvent également entraîner l’omission des requêtes de registre ou WMI. Toutefois, cela est bien moins susceptible de se produire que la perte d’événements ETW. Par conséquent, les résultats de l’ensemble de collecteurs de données ne contiennent pas toutes les valeurs demandées. Vous devez vous assurer que la situation est gérée par les scripts T-SQL pour tous les packs Advisor. Si les données n’existent pas dans le résultat de la collecte de données, elles sont marquées comme aucune donnée dans les rapports.

Étant donné que la perte d’événements ETW est courante pour PLA, les points de données qui sont générés selon une trace ETW peuvent ne pas être cohérents avec les points de données qui sont générés en fonction des compteurs de performances, par exemple. Par exemple, il est possible de voir que l’utilisation totale du processeur par IIS est de 80% (provenant des compteurs de performances), et que les URL principales utilisent uniquement 10% de l’ensemble du temps processeur (qui est un point de données provenant de la trace ETW). En règle générale, la source de données d’un point de données peut être affichée dans l’info-bulle du point de données. Vous devez être conscient de l’impact de cette perte de données.

Pour éviter une telle perte d’événements, le dossier de résultats doit être fermé sur le serveur cible.

Si les résultats du collecteur de données contiennent des données incomplètes autres que la perte de trace ETW et que le développeur Advisor Pack a ajouté la prise en charge de la notification de perte d’événements ETW, une barre d’informations s’affiche en haut du rapport unique pour informer l’utilisateur de la rapport incohérent provoqué par une perte de données. vous trouverez des informations détaillées sur la perte de données dans le fichier log. txt.

## <a name="glossary"></a>Glossaire


Voici quelques-uns des termes utilisés avec SPA :

* **Pack Advisor** Collection de métadonnées et de scripts T-SQL qui traitent les journaux de performances collectés à partir du serveur cible. Le Pack Advisor génère ensuite des rapports à partir des données du journal de performances. Les métadonnées dans Advisor Pack définissent les données à collecter sur le serveur cible pour les mesures de performances. Les métadonnées définissent également l’ensemble de règles, les seuils et le format de rapport. Le plus souvent, un Advisor Pack est écrit spécifiquement pour un rôle serveur unique, par exemple, Internet Information Services (IIS).

* **Console Spa** SpaConsole. exe, qui est la partie centrale de SPA. SPA n’a pas besoin de s’exécuter sur le serveur cible que vous testez. La console du SPA contient toutes les interfaces utilisateur pour SPA, de la configuration du projet à l’exécution de l’analyse et de l’affichage des rapports. Par défaut, SPA est une application à deux niveaux. La console du SPA contient la couche d’interface utilisateur et une partie de la couche logique métier. La console SPA planifie et traite les demandes d’analyse des performances.

* **Infrastructure Spa** Fournit toutes les interfaces utilisateur, le traitement des journaux de performances, la configuration, la gestion des erreurs et les API de base de données, ainsi que les procédures de gestion.

* **Projet Spa** Une base de données qui contient toutes les informations sur les serveurs cibles, les packs d’analyse et les rapports d’analyse des performances qui sont générés sur les serveurs cibles pour les packs d’Advisor. Vous pouvez comparer et afficher les graphiques d’historique et de tendance au sein du même projet SPA. Vous pouvez créer plusieurs projets. Les projets SPA sont indépendants les uns des autres et il n’y a pas de données partagées entre les projets.

* **Serveur cible** Ordinateur physique ou ordinateur virtuel qui exécute Windows Server avec certains rôles de serveur, tels que les services Internet (IIS).

* **Session d’analyse des données** Analyse des performances sur un serveur cible spécifique. Une session d’analyse de données peut inclure plusieurs packs d’Advisor. Les ensembles de collecteurs de données de ces packs Advisor sont fusionnés dans un seul ensemble de collecteurs de données. Tous les journaux de performances d’une même session d’analyse de données sont collectés au cours de la même période. L’analyse des rapports générés par les packs d’aide en cours d’exécution dans la même session d’analyse des données peut aider les utilisateurs à comprendre la situation globale des performances et à identifier les causes des problèmes de performances.

* **Suivi d’v nements pour Windows** Système de traçage hautes performances, à faible charge et évolutif, fourni dans Windows. Il fournit des fonctionnalités de profilage et de débogage, qui peuvent être utilisées pour dépanner un large éventail de scénarios. SPA utilise des événements ETW comme source de données pour générer les rapports de performances. Pour obtenir des informations générales sur ETW, consultez [améliorer le débogage et le réglage des performances avec ETW](https://msdn.microsoft.com/magazine/cc163437.aspx).

* **Windows Management Instrumentation (WMI)** Infrastructure des données et des opérations de gestion dans Windows. Vous pouvez écrire des scripts ou des applications WMI pour automatiser des tâches administratives sur des ordinateurs distants. WMI fournit également des données de gestion à d’autres parties du système d’exploitation et aux produits. SPA utilise les informations de classe WMI et les points de données comme sources pour générer des rapports de performances.

* **Compteurs de performances** Utilisé pour fournir des informations sur le fonctionnement du système d’exploitation ou d’une application, d’un service ou d’un pilote. Les données du compteur de performances peuvent aider à déterminer les goulots d’étranglement du système et à ajuster les performances du système et des applications. Le système d’exploitation, le réseau et les périphériques fournissent des données de compteur qu’une application peut utiliser pour fournir aux utilisateurs une vue graphique de la performance du système. SPA utilise les informations de compteur de performance et les points de données comme sources pour générer des rapports de performances.

* **Journaux et alertes de performance (PLA)** Collecte les journaux et les traces de performances et génère des alertes de performances lorsque certains déclencheurs sont satisfaits. PLA peut être utilisé pour collecter les compteurs de performances, le suivi d’événements pour Windows (ETW), les requêtes WMI, les clés de Registre et les fichiers de configuration. PLA prend également en charge la collecte de données distantes via des appels de procédure distante (RPC). L’utilisateur définit un ensemble de collecteurs de données, qui comprend des informations sur les données à collecter, la fréquence de collecte des données, la durée de collecte des données, les filtres et un emplacement pour l’enregistrement des fichiers de résultats. SPA utilise PLA pour collecter toutes les données de performances des serveurs cibles.

* **Rapport unique** Rapport SPA généré à partir d’une session d’analyse de données pour un pack Advisor sur un serveur cible unique. Il peut contenir des notifications et diverses sections de données.

* **Rapport côte à côte** Rapport SPA qui compare deux rapports uniques pour le même Advisor Pack. Les deux rapports peuvent être générés à partir de différents serveurs cibles ou à partir d’une analyse de performances distincte exécutée sur le même serveur cible. Le rapport côte à côte crée la possibilité de comparer deux rapports pour aider les utilisateurs à identifier des comportements ou des paramètres anormaux dans l’un des rapports. Un rapport côte à côte contient des notifications et différentes sections de données. Dans chaque section, les données des deux rapports sont répertoriées côte à côte.

* **Graphique de tendances** Rapport SPA utilisé pour examiner les tendances répétitives des problèmes de performances. De nombreux problèmes de performances répétitifs sont causés par les modifications planifiées de la charge du serveur sur le serveur ou à partir d’ordinateurs clients, ce qui peut se produire quotidiennement ou toutes les semaines. SPA fournit un graphique de tendances de 24 heures et un graphique de tendances de 7 jours pour identifier ces problèmes.

    L’utilisateur peut choisir une ou plusieurs séries de données à la fois, qui est une valeur numérique dans le rapport unique, par exemple l' **utilisation totale de l’UC moyenne**. plus précisément, une valeur numérique est une valeur scalaire d’un serveur unique qui est générée par un point d’accès unique à une instance d’heure donnée. SPA regroupe ces valeurs en 24 groupes, un pour chaque heure de la journée (sept pour un rapport de 7 jours, un pour chaque jour de la semaine). SPA calcule la moyenne, la valeur minimale, la valeur maximale et les écarts types pour chaque groupe.

* **Graphique historique** Rapport SPA utilisé pour afficher les modifications de certaines valeurs numériques à l’intérieur de rapports uniques pour un serveur donné et une paire Advisor Pack dans le temps. L’utilisateur peut choisir plusieurs séries de données et les afficher ensemble dans le graphique historique pour comprendre la corrélation entre différentes séries de données.

* **Série de données** Données numériques collectées à partir de la même source de données sur une période donnée. La même source signifie que les données doivent provenir du même serveur cible, par exemple la longueur moyenne de la file d’attente des demandes pour IIS sur un serveur.

* **Règles** de Combinaisons de logique, de seuils et de descriptions. Ils représentent un problème de performances potentiel. Chaque pack Advisor contient plusieurs règles. Chaque règle est déclenchée par un processus de génération de rapports. Une règle applique la logique et les seuils aux données dans un rapport unique. Si les critères sont satisfaits, une notification d’avertissement est générée. Si ce n’est pas le cas, la notification est définie sur l’état **OK** . Si la règle ne s’applique pas, la notification est définie sur l’état non applicable (**na**).

* **Notifications** Informations qu’une règle affiche aux utilisateurs. Il comprend l’état de la règle (**OK**, **na**ou **Avertissement**), le nom de la règle et les recommandations possibles pour résoudre les problèmes de performances.
