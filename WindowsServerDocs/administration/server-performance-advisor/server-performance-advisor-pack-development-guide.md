---
title: Guide de développement de packs Server Performance Advisor
description: Guide de développement de packs Server Performance Advisor
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c647e8a335aac924067d92dcb41ab4d17e0cceef
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884860"
---
# <a name="server-performance-advisor-pack-development-guide"></a>Guide de développement de packs Server Performance Advisor

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 8, Windows 10

Ce guide de développement pour Microsoft Server Performance Advisor (SPA) fournit des instructions pour aider les développeurs et administrateurs système de développement de packs d’advisor pour analyser les performances du serveur.

Il part du principe que vous êtes familiarisé avec les journaux de performances et alertes (PLA), les compteurs de performances, paramètres du Registre, Windows Management Instrumentation (WMI), suivi d’événements pour Windows (ETW) et Transact SQL (T-SQL).

Pour plus d’informations sur l’utilisation de SPA, consultez [Guide d’utilisation de Server Performance Advisor](server-performance-advisor-users-guide.md).

## <a name="spa-advisor-pack-overview"></a>Vue d’ensemble du Pack SPA Advisor


Un conseiller pack est généralement conçu pour un rôle de serveur en question, et il définit les éléments suivants :

* Données collectées via PLA, notamment Windows Management Instrumentation (WMI), les compteurs de performances, paramètres du Registre, fichiers et le suivi d’événements pour Windows (ETW)

* Règles, qui affiche les alertes et les recommandations

* Données à afficher (données brutes collectées, les données agrégées ou listes des 10 premiers)

* Statistiques d’afficher une valeur qui change au fil du temps

* Valeurs des statistiques qui peuvent être orientées

Un pack de l’Assistant inclut les éléments suivants :

* **Métadonnées XML** (ProvisionMetadata.xml)

    * [Les journaux de performances et alertes (PLA)](https://msdn.microsoft.com/library/windows/desktop/aa372635.aspx) ensemble de collecteurs de données

    * disposition du rapport

* **Scripts SQL**

    * Une procédure stockée principale

    * Objets SQL, tels que des procédures stockées et fonctions définies par l’utilisateur

* **Fichier de schéma ETW** ( : Schema.man) cette étape est facultative

### <a name="advisor-pack-workflow"></a>Flux de travail de pack Advisor

![flux de travail de pack Advisor](../media/server-performance-advisor/spa-dev-guide-workflow.png)

Dans ce diagramme de flux, les cercles verts représentent les packs d’advisor. Tous les autres cercles représentent les phases qui sont en cours d’exécution en cours de l’infrastructure d’application à page unique. SPA utilise un pack de l’Assistant pour collecter des données, importer les données dans la base de données, initialiser l’environnement d’exécution et exécuter des scripts SQL.

### <a name="collect-data"></a>Collecte des données

Quand un pack de l’Assistant est en file d’attente d’un serveur particulier à l’aide de SPA, le module de collecte de données interroge l’ensemble de collecteurs de données XML à partir du pack de l’Assistant et collecte des données à partir du serveur cible. Les données brutes sont stockées dans un partage de fichiers spécifié par l’utilisateur. La collecte de données s’arrête pas jusqu'à ce que l’application SPA désignée par l’utilisateur de durée d’exécution est dépassé.

### <a name="import-data-into-the-database"></a>importer des données dans la base de données

Une fois la collecte de données est terminée, chaque type de données est importé dans une table correspondante dans la base de données SQL Server. Par exemple, les paramètres du Registre sont importées dans une table appelée \#registryKeys.

l’importation ETW fichier nécessite un fichier de schéma ETW pour le décodage du fichier .etl. Le fichier de schéma ETW est un fichier XML. Il peut être généré à l’aide de tracerpt.exe, qui est fourni avec Windows. Le fichier de schéma ETW est uniquement requis lorsque le pack de l’Assistant a besoin importer les données ETW.

### <a name="switch-to-low-user-rights"></a>Basculer vers les droits d’utilisateur doté de faibles

Le framework SPA ajuste automatiquement les privilèges pour réduire le niveau d’accès de sécurité requis. Étant donné que les packs d’advisor peuvent être développés ou modifiées par quiconque, il est possible pour un pack de conseiller contenir les scripts SQL falsifiées. Pour atténuer les risques de sécurité, n’importe quel script SQL pour un pack de l’Assistant doit être exécuté avec des droits d’utilisateur doté de faibles. Il peut uniquement accéder à des objets de base de données limitées, telles que des tables temporaires et les API de SPA exposées en tant que procédures stockées. Les scripts SQL dans un pack d’advisor peuvent appeler ces procédures pour interagir avec l’infrastructure SPA stockées.

### <a name="initialize-execution-environment"></a>Initialiser l’environnement d’exécution

Les packs d’Advisor peuvent générer différents types de sortie, tels que des notifications, des recommandations, des tables de faits, des statistiques et des graphiques pour les statistiques. Les scripts SQL effectuer certains calculs basés sur les données collectées. Les résultats rapportés sont stockés dans des tables temporaires via les API publiques SPA. à l’étape d’initialisation, ces tables temporaires et autres ressources système doivent être approvisionnés.

### <a name="run-sql-scripts"></a>Exécuter des scripts SQL

Il existe une procédure stockée principale, qui est nommée par le développeur de pack de l’Assistant. L’infrastructure SPA appelle cette procédure stockée pour lancer le calcul. La procédure stockée consomme les données collectées et communique le résultat final à l’infrastructure d’application à page unique.

### <a name="switch-to-administrative-rights"></a>Basculer vers les droits d’administration

Droits d’administrateur sont requis pour générer un rapport. Génération de rapports est entièrement contrôlée par SPA. Il est moins susceptible d’être falsifiées.

### <a name="generate-a-report"></a>Générer un rapport

Avant la fin de la procédure stockée principale pour un pack d’advisor, tous les résultats calculés, tels que les notifications et les statistiques, ne sont pas conservées. Pendant cette phase, le framework SPA transfère le résultat final à partir de tables temporaires à des tables dans un format particulier. Une fois cette phase terminée, vous pouvez afficher les rapports à l’aide de la console SPA.

## <a name="authoring-an-advisor-pack"></a>Création d’un pack d’advisor


### <a name="quick-guidelines"></a>Instructions rapides

L’organigramme suivant décrit les étapes pour vous permet de développer un pack d’advisor entièrement fonctionnelle. Cette section contient également des exemples pas à pas pour mieux expliquer chaque étape.

![processus de développement de pack Advisor](../media/server-performance-advisor/spa-dev-guide-dev-flowchart.png)

Un pack d’advisor est généralement structuré de la façon suivante :

Pack d’Advisor

ProvisionMetadata.xml

scripts ;

main.SQL

func.sql

Schema.man

Chaque module advisor doit avoir un fichier appelé ProvisionMetadata.xml. Il définit les informations de pack de conseiller base, les données à collecter, notifications et des règles, et comment le rapport doit être stocké et affiché. Le framework SPA utilise ces informations pour générer une table temporaire, puis transférer les résultats dans la table temporaire dans une table accessibles aux utilisateurs.

Tous les scripts SQL doivent être enregistrées dans un sous-dossier appelé du rapport **Scripts**. À des fins de maintenance, nous vous recommandons d’enregistrer les objets de base de données différente dans différents fichiers de SQL Server. Il doit exister au moins une procédure stockée comme un point d’entrée principal.

> [!NOTE]
> Le fichier : Schema.man n’est pas obligatoire, sauf si votre module advisor collecte les traces ETW. Ce fichier de schéma est utilisé pour décrire le schéma des événements ETW et décoder des événements ETW.

### <a name="defining-basic-information"></a>Définition des informations de base

Cette section décrit certains des éléments qui composent un pack d’advisor, notamment ProvisionMetadata.xml et les attributs de base.

Voici un exemple d’en-tête pour le fichier ProvisionMetadata.xml :

``` syntax
<advisorPack
xmlns="http://microsoft.com/schemas/ServerPerformanceAdvisor/ap/2010"
name="Microsoft.ServerPerformanceAdvisor.CoreOS.V2"
displayName="Microsoft CoreOS Advisor Pack V2"
description="Microsoft CoreOS Advisor Pack"
author="Microsoft"
version="1.0"
frameworkversion="3.0"
minOSversion="6.0"
reportScript="ReportScript">
</advisorPack>
```

### <a name="advisor-pack-version"></a>Version du pack d’Advisor

nom de l’attribut : **version**

Les développeurs de packs de conseiller peuvent définir des versions majeure et mineure pour le pack d’advisor :

* Une version majeure implique généralement des améliorations significatives. Les résultats qui sont générés par une ancienne version n’est peut-être pas compatibles avec une nouvelle. Nous vous recommandons vivement, y compris la version majeure dans le nom du pack d’advisor.

* SPA permet des mises à niveau de version secondaire lorsqu’il y a que des changements mineurs sans problèmes d’incompatibilité de données.

Pour plus d’informations sur le contrôle de version, consultez [rubriques avancées](#bkmk-advancedtopics).

### <a name="script-entry-point"></a>Point d’entrée de script

nom de l’attribut : **reportScript**

Le framework SPA recherche le nom de la procédure stockée principale à partir du point d’entrée de script et s’exécute de manière sécurisée.

### <a name="other-attributes"></a>Autres attributs

Voici quelques autres attributs qui peuvent être utilisées pour identifier un pack d’advisor :

* Nom d’affichage : **displayName**

* Description : **description**

* Auteur : **auteur**

* Version du Framework : **frameworkversion** (3.0 par défaut)

* Version minimale du système d’exploitation : **minOSversion** (réservé pour une extensibilité future)

* Perte de notification d’événement : **showEventLostWarning**

### <a href="" id="bkmk-definedatacollector"></a>Définition de l’ensemble de collecteurs de données

Un ensemble de collecteurs de données définit les données de performances que le framework SPA doit collecter à partir du serveur cible. Il prend en charge les compteurs de performance de paramètres, WMI, Registre, les fichiers à partir du serveur cible et ETW.

``` syntax
<advisorPack>
<dataSourceDefinition xmlns="http://microsoft.com/schemas/ServerPerformanceAdvisor/dc/2010">
 <dataCollectorSet duration="10">
<registryKeys>
 ?<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\\</registryKey>
</registryKeys>
<managementpaths>
 ?<path>Root\Cimv2:select * FROM Win32_DiskDrive</path>
</managementpaths>
<performanceCounters interval="2">
 ?<performanceCounter>\PhysicalDisk(*)\Avg. Disk sec/Transfer</performanceCounter>
</performanceCounters>
<files>
 ?<path>%windir%\System32\inetsrv\config\applicationHost.config</path>
</files>
<providers>
 ?<provider session="NT Kernel Logger" guid="{9E814AAD-3204-11D2-9A82-006008A86939}" keywordsany="06010201" keywordsAll="00000000" level="00000000" />
</providers>
 </dataCollectorSet>
</dataSourceDefinition>
</advisorPack>
```

Le **durée** attribut de **&lt;dataCollectorSet /&gt;** dans l’exemple précédent définit la durée de collecte de données (l’unité de temps est secondes). **durée** est un attribut requis. Ce paramètre contrôle la durée de la collection qui est utilisée par les compteurs de performance et ETW.

### <a name="collect-registry-data"></a>Collecter des données de Registre

Vous pouvez collecter des données de Registre à partir des ruches de Registre suivantes :

* CLÉ HKEY\_CLASSES\_RACINE

* Clé HKEY\_actuel\_CONFIG

* Clé HKEY\_actuel\_utilisateur

* CLÉ HKEY\_LOCAL\_MACHINE

* CLÉ HKEY\_UTILISATEURS

Pour collecter un paramètre de Registre, spécifiez le chemin complet vers le nom de valeur : HKEY\_LOCAL\_MACHINE\\MyKey\\MyValue

Pour collecter tous les paramètres sous une clé de Registre, spécifiez le chemin d’accès complet à la clé de Registre : Clé HKEY\_LOCAL\_MACHINE\\MyKey\\

Pour collecter toutes les valeurs sous une clé de Registre et ses sous-clés (PLA récursivement collecte les données de Registre), utilisez deux barres obliques inverses, le dernier séparateur de chemin : Clé HKEY\_LOCAL\_MACHINE\\MyKey\\\\

Pour collecter des informations du Registre à partir d’un ordinateur distant, incluent le nom d’ordinateur au début du chemin du Registre : HKEY\_LOCAL\_MACHINE\\MyKey\\MyValue

Par exemple, vous pouvez avoir une clé de Registre qui s’affiche comme suit :

``` syntax
Windows registry editor version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes]
"activePowerScheme"="db310065-829b-4671-9647-2261c00e86ef"

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\db310065-829b-4671-9647-2261c00e86ef]
"Description"=
 FriendlyName = Power Source Optimized 

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\db310065-829b-4671-9647-2261c00e86ef \0012ee47-9041-4b5d-9b77-535fba8b1442\6738e2c4-e8a5-4a42-b16a-e040e769756e
"ACSettingIndex"=dword:000000b4
"DCSettingIndex"=dword:0000001e
```

Exemple 1 : Retourner uniquement le PowerSchemes active et leurs valeurs :

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes</registryKey>
```

Exemple 2 : Retourne toutes les paires clé / valeur sous ce chemin d’accès :

> [!NOTE]
> PLA s’exécute sous les informations d’identification de l’utilisateur. Certaines clés de Registre nécessitent des informations d’identification administratives. L’énumération s’arrête en cas d’échec d’accès à toutes les sous-clés.

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\\</registryKey>
```

Toutes les données collectées seront importées dans une table temporaire appelée  **\#registryKeys** avant un rapport SQL script est exécuté. Le tableau suivant montre les résultats par exemple 2 :

KeyName | KeytypeId | Value
------ | ----- | -------
HKEY_LOCAL_MACHINE...\PowerSchemes | 1 | db310065-829b-4671-9647-2261c00e86ef
\db310065-829b-4671-9647-2261c00e86ef\Description | 2 | |
\db310065-829b-4671-9647-2261c00e86ef\FriendlyName | 2 | Source d’alimentation optimisé
...\6738e2c4-e8a5-4a42-b16a-e040e769756e\ACSettingIndex | 4 | 180
...\6738e2c4-e8a5-4a42-b16a-e040e769756e\DCSettingIndex | 4 | 30

Le schéma pour le **#registryKeys** table est comme suit :

Nom de la colonne | Type de données SQL | Description
-------- | -------- | --------
KeyName | Nvarchar(300) NOT NULL | Nom de chemin complet de la clé de Registre
KeytypeId | smallint non NULL | Id de type interne
Value | Nvarchar(4000) NOT NULL | Toutes les valeurs

Le **KeytypeID** colonne peut avoir un des types suivants :
ID | Type
--- | ---
1 | Chaîne
2 | expandString
3 | Binary
4 | DWord
5 | DWordBigEndian
6 | Lien
7 | MultipleString
8 | Resourcelist
9 | FullResourceDescriptor
10 | ResourceRequirementslist
11 | Qword

### <a name="collect-wmi"></a>Collecter WMI

Vous pouvez ajouter n’importe quelle requête WMI. Pour plus d’informations sur l’écriture de requêtes WMI, consultez [WQL (SQL for WMI)](https://msdn.microsoft.com/library/windows/desktop/aa394606.aspx). L’exemple suivant interroge un emplacement de fichier de page :

``` syntax
<path>Root\Cimv2:select * FROM Win32_PageFileUsage</path>
```

La requête dans l’exemple ci-dessus retourne un seul enregistrement :

Caption | Nom | PeakUsage
----- | ----- | -----
C:\pagefile.sys | C:\pagefile.sys | 215

Étant donné que WMI retourne une table avec des colonnes différentes, lorsque les données collectées sont importées dans une base de données, SPA effectue la normalisation des données et est ajouté aux tables suivantes :

**\#Table de WMIObjects**

ID de séquence | Espace de noms | ClassName | RelativePath | WmiqueryID
----- | ----- | ----- | ----- | -----
10 | Root\Cimv2 | Win32_PageFileUsage | Win32_PageFileUsage.Name=<br>C:\\pagefile.sys | 1

**\#Table de WmiObjectsProperties**

ID | requête
--- | ---
1 | Root\Cimv2:SELECT * Win32_PageFileUsage à partir de

**\#Table de WmiQueries**

ID | requête
--- | ---
1 | Root\Cimv2:SELECT * Win32_PageFileUsage à partir de

**\#Schéma de la table WmiObjects**

Nom de la colonne | Type de données SQL | Description
--- | --- | ---
ID de séquence | int non NULL | Mettre en corrélation la ligne et ses propriétés
Espace de noms | Nvarchar(200) NOT NULL | Espace de noms WMI
ClassName | Nvarchar(200) NOT NULL | Nom de la classe WMI
RelativePath | Nvarchar(500) NOT NULL | Chemin d’accès relatif de WMI
WmiqueryId | int non NULL | Mettre en corrélation de la clé de #WmiQueries

**\#Schéma de la table WmiObjectProperties**

Nom de la colonne | Type de données SQL | Description
--- | --- | ---
ID de séquence | int non NULL | Mettre en corrélation la ligne et ses propriétés
Nom | Nvarchar(1000) NOT NULL | Nom de la propriété
Value | Nvarchar(4000) NULL | La valeur de la propriété actuelle

**\#Schéma de la table WmiQueries**

Nom de la colonne | Type de données SQL | Description
--- | --- | ---
Id | int non NULL | > ID de requête unique
requête | Nvarchar(4000) NOT NULL | Chaîne de requête d’origine dans les métadonnées de disposition

### <a name="collect-performance-counters"></a>Collecter les compteurs de performances

Voici un exemple montrant comment collecter un compteur de performance de s :

``` syntax
<performanceCounters interval="1">
  <performanceCounter>\PhysicalDisk(*)\Avg. Disk sec/Transfer</performanceCounter>
</performanceCounters>
```

Le **intervalle** attribut est un paramètre global requis pour tous les compteurs de performances. Il définit un intervalle de collecte des données de performances (l’unité de temps est secondes).

Dans l’exemple précédent, compteur \\PhysicalDisk (\*)\\moy. Disque s/transfert doivent être interrogée à chaque seconde.

Il peut y avoir deux instances : **\_Total** et **0 C: D:**, et la sortie peut se présenter comme suit :

Horodatage | CategoryName | CounterName | Valeur de l’instance de _Total | Valeur de l’instance du lecteur C: 0 D:
---- | ---- | ---- | ---- | ----
13:45:52.630 | PhysicalDisk | Moy. Disque s/transfert | 0.00100008362473995 |0.00100008362473995
13:45:53.629 | PhysicalDisk | Moy. Disque s/transfert | 0.00280023414927187 | 0.00280023414927187
13:45:54.627 | PhysicalDisk | Moy. Disque s/transfert | 0.00385999853230048 | 0.00385999853230048
13:45:55.626 | PhysicalDisk | Moy. Disque s/transfert | 0.000933297607934224 | 0.000933297607934224

Pour importer les données dans la base de données, les données sont normalisées dans une table appelée  **\#PerformanceCounters**.

CategoryDisplayName | InstanceName | CounterdisplayName | Value
---- | ---- | ---- | ----
PhysicalDisk | _Total | Moy. Disque s/transfert | 0.00100008362473995
PhysicalDisk | 0 C: D: | Moy. Disque s/transfert | 0.00100008362473995
PhysicalDisk | _Total | Moy. Disque s/transfert | 0.00280023414927187
PhysicalDisk | 0 C: D: | Moy. Disque s/transfert | 0.00280023414927187
PhysicalDisk | _Total | Moy. Disque s/transfert | 0.00385999853230048
PhysicalDisk | 0 C: D: | Moy. Disque s/transfert | 0.00385999853230048
PhysicalDisk | _Total | Moy. Disque s/transfert | 0.000933297607934224
PhysicalDisk | 0 C: D: | Moy. Disque s/transfert | 0.000933297607934224

**Remarque** le localisées telles que des noms, **CategoryDisplayName** et **CounterdisplayName**, varient selon la langue d’affichage utilisée sur le serveur cible. Évitez d’utiliser ces champs si vous souhaitez créer un pack de langue neutre advisor.

**\#PerformanceCounters** schéma de table

Nom de la colonne | Type de données SQL | Description
---- | ---- | ---- | ----
Horodatage | datetime2(3) pas NULL | L’heure collectées dans UNC
CategoryName | Nvarchar(200) NOT NULL | Nom de catégorie
CategoryDisplayName | Nvarchar(200) NOT NULL | Nom de catégorie localisée
InstanceName | Nvarchar(200) NULL | Nom de l'instance
CounterName | Nvarchar(200) NOT NULL | Nom du compteur
CounterdisplayName | Nvarchar(200) NOT NULL | Nom du compteur localisée
Value | Float non NULL | La valeur collectée

### <a name="collect-files"></a>Collecter des fichiers

Les chemins d’accès peuvent être absolu ou relatif. Le nom de fichier peut inclure le caractère générique (\*) et le point d’interrogation ( ?). Par exemple, pour collecter tous les fichiers dans le dossier temporaire, vous pouvez spécifier c:\\temp\\\*. Le caractère générique s’applique aux fichiers dans le dossier spécifié.

Si vous souhaitez également collecter des fichiers à partir des sous-dossiers du dossier spécifié, utilisez deux barres obliques inverses pour le dernier délimiteur de dossier, par exemple, c:\\temp\\\\\*.

Ici, s un exemple qui interroge le **applicationHost.config** fichier :

``` syntax
<path>%windir%\System32\inetsrv\config\applicationHost.config</path>
```

Vous trouverez les résultats dans une table appelée  **\#fichiers**, par exemple :

querypath | FullPath | ParentPath | FileName | Contenu
----- | ----- | ----- | ----- | -----
%windir%\...\applicationHost.config |C:\Windows<br>\...\applicationHost.config | C:\Windows<br>\...\config | applicationHost.confi | 0x3C3F78

**\#Schéma de table de fichiers**

Nom de la colonne | Type de données SQL | Description
---- | ---- | ----
querypath | Nvarchar(300) NOT NULL | Instruction de requête d’origine
FullPath | Nvarchar(300) NOT NULL | Chemin d’accès absolu et nom de fichier
ParentPath | Nvarchar(300) NOT NULL | Chemin d’accès au fichier
FileName | Nvarchar(300) NOT NULL | Nom du fichier
Contenu | Varbinary (max) NULL | Contenu du fichier au format binaire

### <a name="defining-rules"></a>Définition de règles

Une fois que suffisamment de données est collectée à partir d’un serveur cible à l’aide de PLA, le pack de l’Assistant peut utiliser ces données pour la validation et afficher un rapide résumé pour les administrateurs système.

Les règles offrent une vue d’ensemble rapide sur le serveur de performances de s. Ils mettre en évidence les problèmes et fournissent des recommandations. Vous pouvez répertorier toutes les règles que vous souhaitez valider pour un pack de l’Assistant. Par exemple, si vous souhaitez développer un pack de conseiller de système d’exploitation principal, les règles possibles peut inclure :

* Si l’UC power mode est d’économie d’énergie

* Si le serveur est dans un environnement virtualisé

* S’il existe une pression d’e/s disque

Règles contiennent les éléments suivants :

* Seuil dépendant (une partie configurable d’une règle)

* Définition de règle (alertes et les recommandations)

Voici un exemple d’une règle simple de s :

``` syntax
<advisorPack>
   
  <reportDefinition>
    <thresholds>
      <threshold  />
    </thresholds>
    <rules>
      <rule  />
      </rule>
    </rules>
  </reportDefinition>
</advisorPack>
```

### <a name="threshold"></a>Seuil

Seuil est un facteur configurable qui permet aux administrateurs système décider quand une règle doit afficher un bon ou mauvais état. L’exemple suivant montre une règle pour détecter l’espace libre sur un lecteur du système et un avertissement lorsque l’espace libre est inférieur à 10 Go.

``` syntax
<threshold name="freediskSize" caption="Free Disk Size (GB)" description="Free Disk Size  value="10" />
```

Toutefois, dans ce cas, l’administrateur système dispose d’un disque dur plus petits. Il pense que 5 Go d’espace libre peuvent encore être un bon état, et il ne souhaite pas voir un avertissement. Il peut mettre à jour la valeur par défaut de 10 à 5 via la console SPA sans avoir à comprendre comment développer un pack d’advisor.

L’introduction un seuil permet aux administrateurs système rapidement modifier la valeur sans avoir à modifier le pack de l’Assistant.

Dans l’exemple, tous les attributs à l’exception **description** sont requis. Vous pouvez utiliser n’importe quel nombre de **valeur**.

Un seuil peut être partagé entre les règles.

### <a name="alerts-and-recommendations"></a>Alertes et des recommandations

La définition de règle n’implique pas de tous les calculs logique. Il définit la façon dont l’interface utilisateur peut se présenter et comment le serveur SQL Server signale les script communique les résultats à l’interface utilisateur.

Une règle comporte trois parties :

* Alerte (légende de règle)

* Recommandation (Conseil)

* Seuil associé (informations facultatives sur les dépendances)

Voici un exemple d’une règle :

``` syntax
<rule name="freediskSize" caption="Free Disk Size on System Drive" description="This rule checks free disk size on system drive ">
<advice name="SuccessAdvice" level="Success" message="No issue found.">No Recommendation.</advice>
<advice name="WarningAdvice" level="Warning" message="Not enough free space on system drive.">
Install OS on larger disk.</advice>
<dependencies>
 <threshold ref="freediskSize"/>
</dependencies>
</rule>
```

Vous pouvez définir autant de conseils que vous souhaitez, et vous devez généralement définir des recommandations. Le **niveau** Conseil peut être **réussite** ou **avertissement**.

Vous pouvez lier à des seuils autant que vous le souhaitez. Vous pouvez même lier à un seuil qui n’est pas pertinent pour la règle actuelle. La liaison permet à la console SPA gérer facilement les seuils.

Le nom de la règle et les recommandations sont des clés, et ils sont uniques dans leur étendue. Aucune deux règle ne peut avoir le même nom, et aucune deux recommandation au sein d’une seule règle ne peut avoir le même nom. Ces noms seront très IMPORTANT lorsque vous créez un rapport de script SQL. Vous pouvez appeler la \[dbo\].\[ SetNotification\] API pour définir l’état de la règle.

### <a name="defining-ui-display-elements"></a>Définition des éléments d’affichage de l’interface utilisateur

Une fois que les règles sont définies, les administrateurs système puissent voir le rapport résumé. Toutefois, souvent les administrateurs système sont intéressés par les données agrégées, et ils souhaitent vérifier les sources de données qui ont été utilisés dans les règles de performances.

Poursuivre avec l’exemple précédent, l’utilisateur sait s’il existe suffisamment d’espace disque libre sur le lecteur système. Les utilisateurs peuvent également être intéressées par la taille réelle de l’espace libre. Un groupe de valeurs unique est utilisé pour stocker et afficher ces résultats. Plusieurs valeurs uniques peuvent être regroupées et affichées dans une table dans la console SPA. La table comporte deux colonnes, nom et la valeur, comme illustré ici.

Nom | Value
---- | ----
Taille de disque disponible sur le lecteur système (en Go) | 100
Taille totale du disque installé (en Go) | 500 

Si un utilisateur souhaite afficher la liste de tous les disques durs qui sont installés sur le serveur et leur taille de disque, nous pouvons appeler une valeur de liste, ce qui a trois colonnes et plusieurs lignes, comme illustré ici.

Disque | Taille de disque (Go) | Taille totale (en Go)
---- | ---- | ----
0 | 100 | 500
1 | 20 | 320

Dans un pack d’advisor, il peut exister plusieurs tables (groupes de valeur unique et les tableaux de valeurs de liste). Nous pouvons utiliser une section pour organiser et classer ces tables.

En résumé, il existe trois types d’éléments d’interface utilisateur :

* [Sections](#bkmk-ui-section)

* [Groupes de valeur unique](#bkmk-ui-svg)

* [tableaux de valeurs de liste](#bkmk-ui-lvt)

Ici s un exemple qui montre les éléments d’interface utilisateur :

``` syntax
<advisorPack>
<dataSourceDefinition/>
<reportDefinition>
 <datatypes>
<datatype .../>
 </datatypes>
 <thresholds/>
 <rule/>
 <sections>
<section .../>
 </sections>
 <singleValues>
<singleValue .../>
 </singleValues>
 <listValues>
<listValue .../>
 </listValues>
</reportDefinition>
</advisorPack>
```

### <a href="" id="bkmk-ui-section"></a>Sections

Une section est destinée à la disposition de l’interface utilisateur. Il ne participe pas à tous les calculs logiques. Chaque rapport unique contient un ensemble de sections de niveau supérieur qui n’ont pas d’une section parente. Les sections de niveau supérieur sont présentées sous forme d’onglets dans le rapport. Les sections peuvent contenir des sous-sections, avec un maximum de 10 niveaux. Les sous-sections suivantes sous les sections de niveau supérieur sont présentés dans les zones pouvant être développés. Une section peut contenir plusieurs sous-sections, groupes de valeur unique et les tableaux de valeurs de liste. Groupes de valeur unique et des tableaux de valeurs de liste est présentés en tant que tables.

Voici un exemple de section de niveau supérieur.

``` syntax
<section name="CPU" caption="CPU"/>
```

Un nom de section doit être unique. Il est utilisé en tant que clé qui peut être liée à par d’autres sections, groupes de valeur unique et les tableaux de valeurs de liste.

L’exemple suivant a un attribut, **parent**, et il ne pointe pas vers la section du processeur. CPUFacts est un enfant de la section nommée du processeur. **parent** doit faire référence à un nom de la section précédente ; sinon, elle peut entraîner une boucle.

``` syntax
<section name="CPUFacts" caption="Facts" parent="CPU"/>
```

Le groupe suivant de la valeur unique possède un attribut, **section**, et il peut pointer vers n’importe quelle section en fonction de votre conception d’interface utilisateur.

``` syntax
<singleValue name="CPUInformation" section="CPUFacts" caption="Physical CPU Information"> </singleValue>
```

### <a name="data-types"></a>Types de données

Un groupe de valeurs unique et une table de valeurs de liste contiennent différents types de données, tels que chaîne, int et float. Étant donné que ces valeurs sont stockées dans la base de données SQL Server, vous pouvez définir un type de données SQL pour chaque propriété de données. Toutefois, il est beaucoup plus compliquée de définition d’un type de données SQL. Vous devez spécifier la longueur ou la précision, qui peut-être être sujette à modification.

Pour définir les types de données logiques, vous pouvez utiliser le premier enfant de  **&lt;ReportDefinition est /&gt;**, c'est-à-dire dans laquelle vous pouvez définir un mappage de type de données SQL et votre type de logique.

L’exemple suivant définit deux types de données. Un est **chaîne** et l’autre est **companyCode**.

``` syntax
<datatype name="string" = sqltype="nvarchar(4000)" />
<datatype name="companyCode" sqltype="nvarchar(100)" />
```

Un nom de type de données peut être n’importe quelle chaîne valide. Voici une liste des types de données SQL autorisés :

* bigint

* binary

* bit

* char

* date

* DateHeure

* datetime2

* datetimeoffset

* décimal

* flottant

* entier

* Money

* nchar

* numeric

* nvarchar

* réel

* smalldatetime

* smallint

* smallmoney

* time

* tinyint

* uniqueidentifier

* varbinary

* varchar

Pour plus d’informations sur ces types de données SQL, consultez [des types de données (Transact-SQL)](https://msdn.microsoft.com/library/ms187752.aspx).

### <a href="" id="bkmk-ui-svg"></a>Groupes de valeur unique

Un groupe de valeurs unique regroupe plusieurs valeurs uniques pour présenter dans une table, comme illustré ici.

``` syntax
<singleValue name="Systemoverview" section="SystemoverviewSection" caption="Facts">
<value name="OsName" type="string" caption="Operating system" description="WMI: Win32_OperatingSystem/Caption"/>
<value name="Osversion" type="string" caption="OS version" description="WMI: Win32_OperatingSystem/version"/>
<value name="OsLocation" type="string" caption="OS location" description="WMI: Win32_OperatingSystem/SystemDrive"/>
</singleValue>
```

Dans l’exemple précédent, nous avons défini un groupe de valeurs unique. Il est un nœud enfant de la section **SystemoverviewSection**. Ce groupe dispose des valeurs uniques, qui sont **OsName**, **Osversion**, et **OsLocation**.

Une valeur unique doit avoir un attribut de nom unique global. Dans cet exemple, l’attribut de nom unique global est **Systemoverview**. Le nom unique sera utilisé pour générer une vue correspondante pour le rapport personnalisé. Chaque vue contient le préfixe **vw**, tels que vwSystemoverview.

Vous pouvez ne définir plusieurs groupes de valeur unique, aucun deux noms de valeur unique peuvent être le même, même si elles se trouvent dans différents groupes. Le nom de valeur unique est utilisé par le rapport de script SQL pour définir la valeur en conséquence.

Vous pouvez définir un type de données pour chaque valeur unique. L’entrée autorisée pour **type** est défini dans  **&lt;datatype /&gt;**. Le rapport final pourrait ressembler à ceci :

**Facts**

Nom | Value
--- | ---
Système d’exploitation | &lt;_une valeur est définie par le script de rapport_&gt;
Version du système d'exploitation | &lt;_une valeur est définie par le script de rapport_&gt;
Emplacement du système d’exploitation | &lt;_une valeur est définie par le script de rapport_&gt;

Le **légende** attribut de **&lt;valeur /&gt;** est présentée dans la première colonne. Valeurs de la colonne de valeur sont définies à l’avenir par le rapport de script via \[dbo\].\[ SetSingleValue\]. Le **description** attribut de **&lt;valeur /&gt;** est indiqué dans une info-bulle. L’info-bulle affiche généralement aux utilisateurs la source de données. Pour plus d’informations sur les info-bulles, consultez [info-bulles](#bkmk-tooltips).

### <a href="" id="bkmk-ui-lvt"></a>tableaux de valeurs de liste

Définition d’une valeur de la liste est identique à la définition d’une table.

``` syntax
<listValue name="NetworkAdapterInformation" section="NetworkIOFacts" caption="Physical network adapter information">
<column name="NetworkAdapterId" type="string" caption="ID" description="WMI: Win32_NetworkAdapter/DeviceID"/>
<column name="NetworkAdapterName" type="string" caption="Name" description="WMI: Win32_NetworkAdapter/Name"/>
<column name="type" type="string" caption="type" description="WMI: Win32_NetworkAdapter/Adaptertype"/>
<column name="Speed" type="decimal" caption="Speed (Mbps)" description="WMI: Win32_NetworkAdapter/Speed"/>
<column name="MACaddress" type="string" caption="MAC address" description="WMI: Win32_NetworkAdapter/MACaddress"/>
</listValue>
```

Le nom de la valeur de la liste doit être globalement unique. Ce nom devient le nom d’une table temporaire. Dans l’exemple précédent, la table nommée \#NetworkAdapterInformation sera créée à l’étape d’initialisation environnement d’exécution, qui contient toutes les colonnes qui sont décrites. Comme pour un nom de valeur unique, un nom de valeur de liste est également utilisé en tant que partie du nom de la vue personnalisée, par exemple, vwNetworkAdapterInformation.

@type de &lt;colonne /&gt; est défini par &lt;datatype /&gt;

L’interface utilisateur fictif de l’état final peut se présenter comme suit :

**Informations de carte réseau physique**

ID | Nom | Type | Vitesse (Mbits/s) | Adresse MAC
--- | --- | --- | --- | ---
 | <br> | | |
 | | | |


Le **légende** attribut de &lt;colonne /&gt; est affiché comme un nom de colonne et le **description** attribut de &lt;colonne /&gt; est affichée comme une info-bulle pour l’en-tête de colonne correspondant. L’info-bulle affiche généralement l’utilisateur la source de données. Pour plus d’informations, consultez [info-bulles](#bkmk-tooltips).

Dans certains cas, une table peut comporter un grand nombre de colonnes, et uniquement quelques lignes, donc le remplacement des colonnes et des lignes rendrait la table ressemblent beaucoup mieux. Pour basculer entre les lignes et colonnes, vous pouvez ajouter l’attribut de style suivant :

``` syntax
<listValue style="Transpose"  
```

### <a name="defining-charting-elements"></a>Définition des éléments graphiques

Vous pouvez choisir n’importe quelle touche de statistiques et afficher les valeurs dans un graphique d’historique ou d’un graphique de tendances. Il existe deux types de statistiques :

* **Statistiques statiques** une valeur unique, ce qui est connue au moment du design. Par exemple, l’espace disque libre sur un lecteur du système serait une statistique statique.

* **Statistiques dynamiques** peut être inconnu au moment du design. Par exemple, l’utilisation moyenne du processeur de chaque cœur est une statistique dynamique, car vous ne connaissez pas le nombre de cœurs de processeur impossible dans le système au moment du design.

La clé de statistiques a une limitation que les données doivent être compatibles avec le type de données double. Il peut être un entier, décimal ou une chaîne qui peut être convertie en valeur double.

SPA utilise un groupe de valeurs unique pour prendre en charge les statistiques statiques et une table de valeurs de liste pour prendre en charge des statistiques dynamiques. Les sections suivantes décrivent comment définir des statistiques statiques et les clés de statistiques dynamique.

### <a name="static-statistics"></a>Statistiques statiques

Comme mentionné précédemment, une statistique statique est une valeur unique. Logiquement, n’importe quelle valeur unique peut être défini comme une statistique statique. Toutefois, il est inutile d’afficher une valeur unique qui ne peut pas être castée en un type de nombre. Pour définir une statistique statique, vous pouvez simplement ajouter l’attribut **trendable** à la seule valeur clé correspondante comme indiqué ci-dessous :

``` syntax
<value name="freediskSize" type="int" trendable="true"  
```

### <a name="dynamic-statistics"></a>Statistiques dynamiques

Clés de statistiques dynamique sont inconnues au moment du design, le nombre de valeurs possibles est inconnu. Toutefois, étant donné que les valeurs de liste sont stockés dans plusieurs lignes, il est facile d’utiliser une table de valeurs de liste pour stocker les statistiques dynamiques.

par exemple, si nous devons afficher les graphiques pour l’utilisation moyenne du processeur de cœurs différents, nous pouvons définir une table avec des colonnes pour **CpuId** et **AverageCpuUsage**:

``` syntax
<listValue name="CpuPerformance">
<column name="CpuId" type="string" caption="CPU ID" columntype="Key"/>
<column name="AverageCpuUsage" type="decimal" caption="Average" columntype="Value"/>
</listValue>
```

Un autre attribut, **columntype**, peut être **clé**, **valeur**, ou **information**. Le type de données de la **clé** colonne doit être double ou convertible en double. Dans un **clé** colonne, vous ne pouvez pas insérer les mêmes clés dans une table. **Valeur** ou **information** colonnes n’ont pas cette limitation.

Les valeurs statistiques sont stockées dans **valeur** colonnes.

**D’information** colonnes sont comme des colonnes ordinaires dans les tableaux de valeurs de liste normal. **D’information** est le type de colonne par défaut si vous ne spécifiez pas une. Ces colonnes ne seront pas affecter le nombre de statistiques clés participez ou dans les calculs statistiques relatives.

Poursuivre avec l’exemple précédent, si un serveur dispose de deux cœurs de processeur, le résultat de la table peut ressembler à ceci :

CpuId | AverageCpuUsage
:---: | :---:
0 | 10
1 | 30

En même temps, les clés de deux des statistiques sont générées par l’infrastructure d’application à page unique. Une pour les UC 0, l’autre est pour le processeur 1.

Comme l’exemple suivant indique plusieurs **valeur** colonnes avec plusieurs **clé** colonnes est pris en charge.

CounterName | InstanceName | Moyenne | Sum
--- | :---: | :---: | :---:
% temps processeur | _Total | 10 | 20
% temps processeur | CPU0 | 20 | 30 

Dans cet exemple, vous disposez de deux **clé** colonnes et deux **valeur** colonnes. SPA génère deux clés de statistiques pour la colonne moyenne et une autre des deux clés pour la colonne de somme. Les clés de statistiques sont :

* CounterName (% de temps processeur) / InstanceName (\_Total) / moyenne

* CounterName (% de temps processeur) / InstanceName (CPU0) / moyenne

* CounterName (% de temps processeur) / InstanceName (\_Total) / somme

* CounterName (% de temps processeur) / InstanceName (CPU0) / somme

CounterName et InstanceName sont combinés en une seule clé. La clé combinée ne peut pas avoir de duplication d’aucune.

SPA génère plusieurs clés de statistiques. Certains d'entre eux n’est peut-être pas intéressant pour vous, et vous pouvez les masquer à partir de l’interface utilisateur. SPA permet aux développeurs de créer un filtre pour afficher uniquement les clés de statistiques utiles.

pour l’exemple précédent, les administrateurs système uniquement est peut-être intéressés par les clés dans lequel le nom de l’instance est \_Total ou CPU1. Le filtre peut être défini comme suit :

``` syntax
<listValue name="CpuPerformance">
<column name="CounterName" type="string" columntype="Key"/>
<column name="InstanceName" type="string" columntype="Key">
 <trendableKeyValues>
<value>_Total</value>
<value>CPU1</value>
 </trendableKeyValues>
</column>
<column name="Average" type="decimal" columntype="Value"/>
<column name="Sum" type="decimal" columntype="Value"/>
</listValue>
```

**&lt;trendableKeyValues /&gt;**  peuvent être définis sous n’importe quelle colonne de clé. Si plus d’une colonne de clé a un tel filtre configuré et logique sera appliquée.

### <a name="developing-report-scripts"></a>Développement de scripts de rapport

Une fois que les métadonnées de l’approvisionnement sont définies, nous pouvons commencer à écrire le script de rapport, qui est une procédure stockée T-SQL.

Il existe **nom** et **reportScript** attributs dans l’en-tête de métadonnées de disposition, comme illustré ici :

``` syntax
<advisorPack name="Microsoft.ServerPerformanceAdvisor.CoreOS.V1" reportScript="ReportScript"  
```

Le script de rapport principal est nommé en combinant le **nom** et **reportScript** attributs. Dans l’exemple suivant, il sera \[Microsoft.ServerPerformanceAdvisor.CoreOS.V2\].\[ ReportScript\].

``` syntax
create PROCEDURE [Microsoft.ServerPerformanceAdvisor.CoreOS.V2].[ReportScript] AS SET NOCOUNT ON

- Set alert and notification

- Prepare data for report view
```

Le **nom** attribut sera utilisé comme un nom de schéma de base de données, comme un espace de noms. Cette règle s’applique à tous les autres objets de base de données qui appartiennent au pack de conseiller en cours, telles que la valeur de liste et les procédures stockées.

Avantages pour portant ce nom de schéma devant les objets de base de données :

* Éviter les conflits d’affectation de noms pour les packs d’advisor différents

* En fournissant une sécurité accrue

Dans la base de données SQL Server, le nom de schéma par défaut est **dbo**. Informations d’identification du propriétaire de base de données sont généralement nécessaires au fonctionnement des objets de base de données sous **dbo**. Si nous ne créons pas un schéma pour chaque pack d’advisor, il est probable que les packs d’advisor deux définiront une valeur de liste portant le même nom. Cela doit être sans importance, car vous pouvez introduire un nom de schéma pour résoudre ce problème. En outre, il est beaucoup plus facile d’un pack de conseiller de mise hors service. Étant donné que l’objet de pack advisor appartient à un schéma autre que **dbo**, cela permet de SPA à utiliser un privilège utilisateur inférieur à y accéder.

Un script de rapport normal effectue les opérations suivantes :

* Accès à des données brutes collectées

* Effectue des calculs basés sur les données brutes

* Alertes de modifications et des recommandations

* Prépare les données pour la vue rapport

### <a name="access-raw-collected-data"></a>Données brutes collectées d’accès

Toutes les données collectées est importé dans les tables correspondantes suivantes. Pour plus d’informations sur le schéma de table, consultez [définissant l’ensemble de collecteurs de données](#bkmk-definedatacollector).

* Registre

    * \#registryKeys

* WMI

    * \#WMIObjects

    * \#WmiObjectProperties

    * \#WmiQueries

* Compteur de performances

    * \#Compteurs de performance

* Fichier

    * \#Fichiers

* ETW

    * \#Événements

    * \#EventProperties

### <a name="set-rule-status"></a>Définir l’état de règle

Le \[dbo\].\[ SetNotification\] API définit l’état de la règle, afin que vous puissiez voir une **réussite** ou **avertissement** icône dans l’interface utilisateur.

* @ruleName nvarchar(50)

* @adviceName nvarchar(50)

Les messages d’alerte et des recommandations sont stockés dans le fichier XML de métadonnées de disposition. Cela fait, le script de rapport plus facile à gérer.

Initialement, l’état de chaque règle est n/a. Vous pouvez utiliser cette API pour définir un état de la règle en spécifiant un nom de conseils. Le niveau du nom de conseils sera utilisé en tant que l’état de la règle.

Rappelez-vous que nous avons défini précédemment la règle suivante :

``` syntax
<rule name="freediskSize" caption="Free Disk Size on System Drive" description="This rule checks free disk size on the system drive ">
<advice name="SuccessAdvice" level="Success" message="No issue found.">No recommendation.</advice>
<advice name="WarningAdvice" level="Warning" message="Not enough free space on system drive.">Install the operating system on a larger disk.</advice>
</rule>
```

En supposant que l’espace libre est inférieur à 2 Go, nous devons définir la règle sur le **avertissement** niveau. Le script SQL se présente comme suit :

``` syntax
if (@freediskSizeInGB < 2)
BEGIN
    exec dbo.SetNotification N'freediskSize', N'WarningAdvice'
END
ELSE
BEGIN
    exec dbo.SetNotification N'freediskSize', N'SuccessAdvice'
END 
```

### <a name="get-threshold-value"></a>Obtenir la valeur de seuil

Le \[dbo\].\[ GetThreshold\] API Obtient les seuils :

* @key nvarchar(50)

* @value sortie de type float

> [!NOTE]
> Les seuils sont des paires nom-valeur, et elles peuvent être référencées dans toutes les règles. Les administrateurs de système peuvent utiliser la console SPA pour ajuster les seuils.

 Continuer avec l’exemple précédent, pour un seuil, la définition se présente comme suit :

``` syntax
<thresholds>
  <threshold name="freediskSize" caption="Free Disk Size (GB)" description="Free Disk Size  value="10" />
</thresholds>
<rule name="freediskSize" caption="Free Disk Size on System Drive" description="This rule checks free disk size on system drive ">
<advice name="SuccessAdvice" level="Success" message="No issue found.">No recommendation.</advice>
<advice name="WarningAdvice" level="Warning" message="Not enough free space on the system drive.">
Install the operating system on a larger disk.</advice>
<dependencies>
 <threshold ref="freediskSize"/>
</dependencies>
</rule>
```

Le script de rapport peut être modifié comme indiqué ici :

``` syntax
DECLARE @freediskSize FLOat
exec dbo.GetThreshold N freediskSize , @freediskSize output

if (@freediskSizeInGB < @freediskSize)
 
```

### <a name="set-or-remove-the-single-value"></a>Définir ou supprimer la valeur unique

Le \[dbo\].\[ SetSingleValue\] API définit la valeur unique :

* @key nvarchar(50)

* @value sql\_variant

Cette valeur peut exécuter plusieurs fois pour la même clé de valeur unique. La dernière valeur est enregistrée.

L’exemple suivant montre que certaines définies les valeurs uniques :

``` syntax
<singleValue section="Systemoverview" caption="Facts">
<value name="OsName" type="string" caption="Operating System" description="WMI: Win32_OperatingSystem/Caption"/>
<value name="Osversion" type="string" caption="OS version" description="WMI: Win32_OperatingSystem/version"/>
<value name="OsLocation" type="string" caption="OS Location" description="WMI: Win32_OperatingSystem/SystemDrive"/>
</singleValue>
```

Vous pouvez ensuite définir la valeur unique comme illustré ici :

``` syntax
exec dbo.SetSingleValue N OsName ,  Windows 7 
exec dbo.SetSingleValue N Osversion ,  6.1.7601 
exec dbo.SetSingleValue N OsLocation ,  c:\ 
```

Dans de rares cas, vous souhaiterez supprimer le résultat que vous aviez défini à l’aide de la \[dbo\].\[ removeSingleValue\] API.

* @key nvarchar(50)

Vous pouvez utiliser le script suivant pour supprimer précédemment défini valeur.

``` syntax
exec dbo.removeSingleValue N Osversion 
```

### <a name="get-data-collection-information"></a>Obtenir des informations de collecte de données

Le \[dbo\].\[ GetDuration\] API Obtient l’utilisateur désigné durée en secondes pour la collecte de données :

* @duration sortie de type int

S un exemple de rapport ici script :

``` syntax
DECLARE @duration int
exec dbo.GetDuration @duration output
```

Le \[dbo\].\[ GetInternal\] API Obtient l’intervalle d’un compteur de performances. Elle peut retourner la valeur NULL si le rapport actuel ne dispose pas d’informations sur les compteurs de performances.

* @interval sortie de type int

S un exemple de rapport ici script :

``` syntax
DECLARE @interval int
exec dbo.GetInterval @interval output
```

### <a name="set-a-list-value-table"></a>Définir une table de valeurs de liste

Il n’existe aucune API pour la mise à jour des tableaux de valeurs de liste. Toutefois, vous pouvez accéder directement les tables de valeur de liste. à l’étape d’initialisation, une table temporaire correspondante sera créée pour chaque valeur de la liste.

L’exemple suivant montre une table de valeurs de liste :

``` syntax
<listValue name="NetworkAdapterInformation" section="NetworkIOFacts" caption="Physical Network Adapter Information">
<column name="NetworkAdapterId" type="string" caption="ID" description="WMI: Win32_NetworkAdapter/DeviceID"/>
<column name="NetworkAdapterName" type="string" caption="Name" description="WMI: Win32_NetworkAdapter/Name"/>
<column name="type" type="string" caption="type" description="WMI: Win32_NetworkAdapter/Adaptertype"/>
<column name="Speed" type="decimal" caption="Speed (Mbps)" description="WMI: Win32_NetworkAdapter/Speed"/>
<column name="MACaddress" type="string" caption="MAC address" description="WMI: Win32_NetworkAdapter/MACaddress"/>
</listValue>
```

Vous pouvez ensuite écrire un script SQL pour insérer, mettre à jour ou supprimer les résultats :

``` syntax
INSERT INTO #NetworkAdapterInformation (
  NetworkAdapterId,
  NetworkAdapterName,
  type,
  Speed,
  MACaddress
)
VALUES (
   
)
```

## <a name="development-and-debugging"></a>Développement et débogage


### <a name="writing-logs"></a>Écriture des journaux

s’il existe toute information supplémentaire que vous souhaitez communiquer aux administrateurs système, vous pouvez écrire des journaux. S’il existe un journal pour un rapport particulier, une bannière jaune s’affichera dans l’en-tête du rapport. L’exemple suivant montre comment vous pouvez écrire un journal :

``` syntax
exec dbo.WriteSystemLog N'Any information you want to show to the system administrators , N Warning 
```

Le premier paramètre est le message que vous souhaitez afficher dans le journal. Le deuxième paramètre est le niveau de journalisation. L’entrée valide pour le deuxième paramètre peut être **information**, **avertissement**, ou **erreur**.

### <a name="debug"></a>Déboguer

La console SPA permettre s’exécuter dans deux modes, Debug ou Release. Mode de mise en production est la valeur par défaut, et il nettoie toutes les données brutes collectées une fois que le rapport est généré. Le mode de débogage conserve toutes les données brutes dans le partage de fichiers et de la base de données, afin que vous pouvez déboguer le script de rapport à l’avenir.

**Pour déboguer un script de rapport**

1.  Installez Microsoft SQL Server Management Studio (SSMS).

2.  Une fois que SSMS est lancée, vous connecter à localhost\\SQLExpress. N’oubliez pas que vous devez utiliser localhost, au lieu de. . Sinon, vous ne pouvez pas être en mesure de démarrer le débogueur dans SQL Server.

3.  Exécutez le script suivant pour activer le mode de débogage :

    ``` syntax
    USE SPADB
    UPdate dbo.Configurations
    SET Value = N'true'
    WHERE Name = N'Debugmode'
    ```

4.  Lancez la console SPA et exécuter le pack de l’Assistant que vous souhaitez déboguer.

5.  Attendez que la tâche se termine. Si le rapport est généré avec succès, revenez à SSMS et recherchez la dernière tâche.

    ``` syntax
    select TOP 1 * FROM dbo.Tasks OrdER BY Id DESC
    ```

    Par exemple, la sortie peut être :

    Id | SessionId | AdvisoryPackageId | ReportStatusId | LastUpdatetime | ThresholdversionId
    :---: | :---: | :---: | :---: | :---: | :---:
    12 | 17 | 1 | 2 | 2011-05-11 05:35:24.387 | 1

6.  Vous pouvez exécuter le script suivant en tant qu’autant de fois que vous souhaitez exécuter le script de rapport pour les Id 12 :

    ``` syntax
    exec dbo.DebugReportScript 12
    ```

    **Remarque** vous pouvez également appuyer sur F11 pour avancer dans l’instruction précédente et le débogage.

     

En cours d’exécution \[dbo\].\[ DebugReportScript\] retourne plusieurs jeux de résultats, y compris :

1.  Messages de Microsoft SQL Server et les journaux du pack de l’Assistant

2.  Résultats de règles

3.  Valeurs et clés de statistiques

4.  Valeurs uniques

5.  Tous les tableaux de valeurs de liste

## <a name="best-practices"></a>Meilleures pratiques

### <a name="naming-convention-and-styles"></a>Styles et la convention d’affectation de noms

Pascal casse | Casse mixte | majuscules
--- | ---- | ---
<ul><li>Noms dans ProvisionMetadata.xml</li><li>Procédures stockées</li><li>Fonctions</li><li>Noms de vues</li><li>Noms de tables temporaires</li></ul> | <ul><li>Noms de paramètres</li><li>Variables locales</li></ul> | Utilisation pour tous les mots-clés réservés SQL

### <a name="other-recommendations"></a>Autres recommandations

* Déplacer les éléments plus logiques dans d’autres procédures stockées et les fonctions définies par l’utilisateur.

* Vérifiez votre script principal aussi courte que possible à des fins de maintenance.

* Utilisez le nom complet de l’objet SQL.

* Traiter votre code SQL comme respectant la casse.

* Ajouter **SET NOCOUNT ON** au début de chaque procédure stockée.

* Envisagez d’utiliser des tables temporaires pour transférer l’énorme quantité de données.

* Envisagez d’utiliser **XACT définir\_abandon ON** pour terminer le processus si une erreur se produit.

* Toujours inclure le numéro de version principale dans le nom complet du pack advisor.

## <a href="" id="bkmk-advancedtopics"></a>Rubriques avancées

### <a name="run-multiple-advisor-packs-simultaneously"></a>Exécution simultanée de plusieurs packs d’advisor

SPA prend en charge l’exécution de plusieurs packs de conseiller en même temps. Cela est particulièrement utile lorsque vous souhaitez examiner les Internet Information Services (IIS) et les performances du système d’exploitation core en même temps. Plusieurs collecteurs de données qui sont utilisées par le pack de l’Assistant IIS peuvent également être utilisés par le pack de conseiller Core du système d’exploitation. Lorsque deux ou plusieurs packs advisor sont exécutent sur le même ordinateur cible, SPA ne collecte pas les mêmes données à deux reprises.

L’exemple suivant montre le flux de travail pour l’exécution de deux packs d’advisor.

![exécution de plusieurs packs d’advisor](../media/server-performance-advisor/spa-dev-guide-multi-advisor-packs.png)

L’ensemble de collecteurs de données de fusion est uniquement pour collecter les compteurs de performances et de sources de données ETW. Les règles de fusion suivantes s’appliquent :

1.  SPA prend la durée la plus importante en tant que la nouvelle durée.

2.  S’il existe des conflits de fusion, les règles suivantes sont appliquent :

    1.  Prendre le plus petit intervalle en tant que le nouvel intervalle.

    2.  Prendre le jeu super des compteurs de performance. Par exemple, avec **processus (\*)\\% temps processeur** et **processus (\*)\\\*,\\processus (\*)\\ \***  retourne plus de données, de sorte que **processus (\*)\\% temps processeur** et **processus (\*)\\ \***  est supprimé de l’ensemble de collecteurs de données fusionnées.

### <a name="collect-dynamic-data"></a>Collecter des données dynamiques

Les besoins SPA un collecteur de données défini est défini au moment du design. Il n’est pas toujours possible de savoir quelles données sont nécessaire pour la génération de rapports, car les données dynamiques et le chemin d’accès de requête ne sont pas connus jusqu'à ce que ses données dépendantes ne sont disponibles.

par exemple, si vous souhaitez répertorier tous les noms conviviaux des cartes réseau, vous devez commencer par interroger WMI pour énumérer toutes les cartes réseau. Chaque retournée objet WMI a un chemin de clé de Registre, où il stocke le nom convivial. Le chemin de clé de Registre est inconnu au moment du design. Dans ce cas, nous avons besoin des données dynamiques prennent en charge.

Pour énumérer toutes les cartes réseau, vous pouvez utiliser la requête WMI suivante à l’aide de Windows PowerShell :

``` syntax
Get-WmiObject -Namespace Root\Cimv2 -query "select PNPDeviceID FROM Win32_NetworkAdapter" | forEach-Object { Write-Output $_.PNPDeviceID }
```

Elle retourne une liste d’objets de carte réseau. Chaque objet possède une propriété appelée **PNPDeviceID**, qui tient à jour un chemin de clé de Registre relative. Voici un exemple de sortie de la requête précédente de s :

``` syntax
ROOT\*ISatAP\0001
PCI\VEN_8086&DEV_4238&SUBSYS_11118086&REV_35\4&372A6B86&0&00E4
ROOT\*IPHTTPS\0000
 
```

Pour rechercher le **FriendlyName** valeur, ouvrez l’Éditeur du Registre et accédez au paramètre de Registre en combinant **HKEY\_LOCAL\_MACHINE\\système\\ CurrentControlSet\\Enum\\**  avec chaque ligne dans l’exemple précédent. , par exemple : **Clé HKEY\_LOCAL\_MACHINE\\système\\CurrentControlSet\\Enum\\ racine\\\*IPHTTPS\\0000**.

Pour traduire les étapes précédentes dans les métadonnées de fourniture de SPA, ajoutez le script dans l’exemple de code suivant :

``` syntax
<advisorPack>
<dataSourceDefinition xmlns="http://microsoft.com/schemas/ServerPerformanceAdvisor/dc/2010">
 <dataCollectorSet >
<registryKeys>
 ?<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\$(NetworkAdapter.PNPDeviceID)\FriendlyName</registryKey>
</registryKeys>
<managementpaths>
 ?<path name="NetworkAdapter">Root\Cimv2:select PNPDeviceID FROM Win32_NetworkAdapter</path>
</managementpaths>
```

Dans cet exemple, vous allez tout d’abord ajouter une requête WMI sous managementpaths et définir le nom de clé **NetworkAdapter**. Ensuite, vous ajoutez une clé de Registre et faire référence à **NetworkAdapter** en utilisant la syntaxe, **$(NetworkAdapter.PNPDeviceID)**.

Le tableau suivant définit si un collecteur de données de SPA prend en charge les données dynamiques et s’il peut être référencé par d’autres collecteurs de données :

Type de données | Prise en charge des données dynamiques | Peut être référencé
--- | :---: | :---:
Clé de Registre | Oui | Oui
WMI | Oui | Oui
Fichier | Oui | Non
Compteur de performances | Non | Non
ETW | Non | Non

Pour un collecteur de données WMI, chaque objet WMI a beaucoup d’attributs jointe. N’importe quel type d’objet WMI a toujours trois attributs : \_\_Espace de noms, \_ \_(classe), et \_ \_RELpath.

Pour définir un collecteur de données qui est référencé par d’autres collecteurs de données, affecter le **nom** attribut avec une clé unique dans le ProvisionMetadata.xml. Cette clé est utilisée par les collecteurs de données dépendantes pour générer des données dynamiques.

Voici un exemple de s pour la clé de Registre :

``` syntax
<registryKey  name="registry">HKEY_LOCAL_MACHINE </registryKey>
```

Et un exemple de WMI :

``` syntax
<path name="wmi">Root\Cimv2:select PNPDeviceID FROM Win32_NetworkAdapter</path>
```

Pour définir un collecteur de données dépendants, la syntaxe suivante est utilisée : $(*{name}*.*attribut {}*).

*{name}*  et *{attribut}* sont des espaces réservés.

Lors de l’application à page unique collecte les données à partir d’un serveur cible, il remplace dynamiquement le modèle $(\*.\*) avec le texte réel des données collectées à partir de son collecteur de données de référence (clé de Registre / WMI), par exemple :

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\$(registry.key)\ </registryKey>
<registryKey  name="registry">HKEY_LOCAL_MACHINE\$(wmi.Relativeregistrypath)\ </registryKey>
<path name="wmi"> </path>
<file>$(wmi.FileName)</file>
```

**Remarque** SPA prend en charge une profondeur illimitée de référence, mais n’oubliez pas de surcharge de performances si vous avez trop de niveaux. Assurez-vous qu’il n’existe aucune référence circulaire ou auto-référence qui n'est pas pris en charge.

### <a name="versioning-limitations"></a>limitations de contrôle de version

SPA prend en charge les mises à jour de la version mineure et de réinitialisation. Ces processus utilisent le même algorithme. Le processus consiste à actualiser tous les objets de base de données et les paramètres de seuil, mais conserver les données existantes. Cela peut être mise à niveau vers une version ultérieure ou mise à niveau vers une version antérieure. Sélectionnez le pack de l’Assistant, puis cliquez sur **réinitialiser** dans le **configurer les packs d’Advisor** boîte de dialogue de SPA pour réinitialiser ou appliquer ou les mises à jour.

Cette fonctionnalité est principalement pour les mises à jour mineures. Vous ne pouvez pas modifier considérablement les éléments d’affichage de l’interface utilisateur. Si vous souhaitez apporter des modifications significatives, vous devez créer un pack d’advisor différents. Vous devez inclure la version majeure dans le nom du pack d’advisor.

Les limitations de modifications de version secondaire sont qui vous **ne peut pas** effectuez l’une des opérations suivantes :

* Modifier le nom de schéma

* Modifiez le type de données de n’importe quel groupe de valeurs unique ou les colonnes d’une table de valeur de liste

* Ajouter ou supprimer des seuils

* Ajouter ou supprimer des règles

* Ajouter ou supprimer des conseils

* Ajouter ou supprimer des valeurs uniques

* Ajouter ou supprimer des valeurs de liste

* Ajouter ou supprimer une colonne de valeurs de liste

### <a href="" id="bkmk-tooltips"></a>Info-bulles

Presque tous les **description** attributs apparaissent sous la forme d’une info-bulle dans la console SPA.

pour une table de valeur de liste, une info-bulle basée sur une ligne est possible en ajoutant l’attribut suivant :

``` syntax
<listValue descriptionColumn="Description">
<column name="Name"/>
<column name="Description"/>
</listValue>
```

Le **descriptionColumn** attribut fait référence au nom de la colonne. Dans cet exemple, la colonne de description n’affiche pas sous forme physique de la colonne. Toutefois, il affiche une info-bulle lorsque vous déplacez la souris sur chaque ligne de la première colonne.

Nous recommandons que l’info-bulle affiche la source de données à l’utilisateur. Voici les formats pour afficher les sources de données :

Source de données | Format | Exemple
--- | --- | ---
WMI | WMI : &lt;wmiclass&gt;/&lt;champ&gt; | WMI : Win32_OperatingSystem/Caption
Compteur de performances | Compteur : &lt;CategoryName&gt;/&lt;InstanceName&gt; | Compteur : Temps de processeur Process/%
Registre | registry: &lt;registerKey&gt; | Registre : HKLM\SOFTWARE\Microsoft<br>\\ASP.NET\\Rootver
Fichier de configuration | ConfigFile : &lt;FilePath&gt;\[; XPath : &lt;XPath&gt;\]<br>**Remarque**<br>XPath est facultatif et il est valide uniquement lorsque le fichier est un fichier xml. | ConfigFile : windir%\\System32\\inetsrv\config\\applicationHost.config<br>Xpath: configuration&frasl;system.webServer<br>&frasl;httpProtocol&frasl;@allowKeepAlive
ETW | ETW : &lt;Fournisseur /&gt;(mots clés) | ETW : Suivi du noyau de Windows (processus, net)

### <a name="table-collation"></a>Classement de la table

Quand un pack d’advisor devient plus complexe, vous pouvez créer vos propres tables de variable ou des tables temporaires pour stocker les résultats intermédiaires dans le script de rapport.

Classement des colonnes de type chaîne peut être problématique, car le classement de la table que vous créez peut être différent de celui qui est créé par l’infrastructure SPA. Si vous mettre en corrélation les deux colonnes de chaîne dans des tables différentes, vous pouvez voir une erreur de classement. Pour éviter ce problème, vous devez toujours définir la chaîne d’un classement de colonne en tant que **SQL\_Latin1\_général\_CP1\_CI\_AS** lorsque vous définissez une table.

Ici, s comment définir une variable table :

``` syntax
DECLARE @filesIO TABLE (
 Name nvarchar(500) COLLatE SQL_Latin1_General_CP1_CI_AS,
 AverageFileAccessvolume float,
 AverageFileAccessCount float,
 Filepath nvarchar(500) COLLatE SQL_Latin1_General_CP1_CI_AS
)
```

### <a name="collect-etw"></a>Collecter ETW

Ici, s comment définir ETW dans un fichier ProvisionMetadata.xml :

``` syntax
<dataSourceDefinition>
  <providers>
    <provider session="NT Kernel Logger" guid="{9E814AAD-3204-11D2-9A82-006008A86939}"/>
  </providers>
</dataSourceDefinition>
```

Les attributs de fournisseur suivants sont disponibles à utiliser pour la collecte ETW :

Attribut | Type | Description
--- | --- | ---
GUID | GUID | GUID du fournisseur
session | chaîne | Nom de la session ETW (facultatif, requis uniquement pour les événements de noyau)
keywordsany | Hexadécimal | Tous les mots clés (facultatif, aucun préfixe 0 x)
keywordsAll | Hexadécimal | Tous les mots clés (facultatifs)
propriétés | Hexadécimal | Propriétés (facultatives)
niveau | Hexadécimal | Niveau (facultatif)
bufferSize | Int | Taille de mémoire tampon (facultatif)
flushtime | Int | Temps de vidage (facultatif)
maxBuffer | Int | Mémoire tampon maximale (facultatif)
minBuffer | Int | Mémoire tampon minimale (facultatif)

Il existe deux tables de sortie comme indiqué ici.

**\#Schéma de table d’événements**

Nom de la colonne | Type de données SQL | Description
--- | --- | ---
ID de séquence | int non NULL | ID de séquence de corrélation
EventtypeId | int non NULL | ID de type d’événement (reportez-vous à [dbo]. [ EventTypes])
ProcessId | BigInt non NULL | ID de processus
ThreadId | BigInt non NULL | ID de thread
Horodatage | datetime2 pas NULL | Horodatage
Temps | BigInt non NULL | Temps noyau
Temps | BigInt non NULL | Temps utilisateur

**\#Schéma de la table EventProperties**

Nom de la colonne | Type de données SQL | Description
--- | --- | ---
ID de séquence | int non NULL | ID de séquence de corrélation
Nom | Nvarchar(100) | Nom de la propriété
Value | Nvarchar(4000) | Value

### <a name="etw-schema"></a>Schéma d’ETW

Un schéma ETW peut être généré en exécutant tracerpt.exe sur le fichier .etl. Un fichier : Schema.man est généré. Étant donné que le format du fichier .etl est dépendant d’ordinateur, le script suivant fonctionne uniquement dans les situations suivantes :

1.  Exécutez le script sur l’ordinateur où le fichier .etl correspondant est collecté.

2.  Ou exécutez le script sur un ordinateur avec le même système d’exploitation et les composants installés.

``` syntax
tracerpt *.etl -export
```

## <a name="glossary"></a>Glossaire


Les termes suivants sont utilisés dans ce document :

**Pack d’Advisor**

Un pack d’advisor est une collection de métadonnées et les scripts SQL qui traitent les journaux de performances sont collectées à partir du serveur cible. Le pack de l’Assistant génère ensuite des rapports à partir des données de journal de performances. Les métadonnées dans le pack de l’Assistant définissent les données à collecter à partir du serveur cible pour les mesures de performances. Les métadonnées définissent également l’ensemble de règles, les seuils et le format de rapport. En règle générale, un pack d’advisor est écrit spécifiquement pour un rôle de serveur unique, par exemple, Internet Information Services (IIS).

**Console SPA**

La console SPA fait référence à SpaConsole.exe, qui est la partie centrale de Server Performance Advisor. SPA n’a pas besoin d’exécuter sur le serveur cible que vous testez. La console SPA contient toutes les interfaces utilisateur pour une seule page, à partir de la configuration du projet à l’exécution de l’analyse et l’affichage de rapports. Par conception, SPA est une application à deux niveaux. La console SPA contient la couche d’interface utilisateur et une partie de la couche de logique métier. La console SPA planifie et traite les demandes d’analyse de performances.

**Framework SPA**

SPA contient deux parties principales, l’infrastructure et les packs d’advisor. Le framework SPA fournit toutes les interfaces utilisateur, traitement des journaux de performances, configuration, gestion des erreurs et procédures de gestion et les API de la base de données.

**Projet SPA**

Un projet SPA est une base de données qui contient toutes les informations sur les serveurs cibles, les packs d’advisor et les rapports d’analyse performances qui sont générés sur les serveurs cibles pour les packs d’advisor. Vous pouvez comparer et afficher des graphiques de l’historique et de tendance au sein du même projet SPA. L’utilisateur peut créer plusieurs projets. Les projets SPA sont indépendants des uns des autres et aucune donnée partagés entre plusieurs projets.

**Serveur cible**

Le serveur cible est l’ordinateur physique ou une machine virtuelle qui exécute le serveur Windows à certains rôles de serveur, telles que IIS.

**Session d’analyse de données**

Une session d’analyse de données est une analyse des performances sur un serveur cible spécifique. Une session d’analyse de données peut inclure plusieurs packs d’advisor. Les ensembles de collecteurs de données à partir de ces packs d’advisor sont fusionnées dans un ensemble de collecteurs de données unique. Tous les journaux de performances pour une session d’analyse de données unique sont collectées au cours de la même période. Analyse des rapports qui sont générés par les packs d’advisor en cours d’exécution dans la même session d’analyse de données peut aider les utilisateurs à comprendre la situation de performances globales et identifier les causes principales des problèmes de performances.

**Suivi d’événements pour Windows**

[Le suivi d’événements](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) pour Windows (ETW) est un système de suivi hautes performances, faible surcharge et évolutive qui est fourni dans les systèmes d’exploitation Windows. Il fournit le profilage et de fonctionnalités, qui peuvent être utilisées pour dépanner un certain nombre de scénarios de débogage. SPA utilise des événements ETW comme une source de données pour générer les rapports de performances. Pour obtenir des informations générales sur le suivi ETW, consultez [Améliorez le débogage et l’optimisation des performances avec ETW](https://msdn.microsoft.com/magazine/cc163437.aspx).

**Requête WMI**

Windows Management Instrumentation (WMI) est l’infrastructure pour les opérations dans les systèmes d’exploitation Windows et les données de gestion. Vous pouvez écrire des scripts WMI ou des applications afin d’automatiser les tâches d’administration sur des ordinateurs distants. WMI fournit également des données de gestion à d’autres parties du système d’exploitation et aux produits. SPA utilise les informations de classe WMI et les points de données en tant que sources pour générer des rapports de performances.

**Compteurs de performance**

Compteurs de performances sont utilisés pour fournir des informations sur le système d’exploitation ou une application, un service ou un pilote de performances. Les données de compteur de performances peuvent aider à déterminer les goulots d’étranglement système et affiner les performances des applications et du système. Le système d’exploitation, de réseau et les appareils fournissent des données de compteur qu’une application peut consommer pour fournir aux utilisateurs une vue graphique du degré de performances du système. SPA utilise les informations sur les compteurs de performances et de points de données en tant que sources pour générer des rapports de performances.

**Alertes et journaux de performances**

Les journaux de performances et alertes (PLA) est un service intégré dans le système d’exploitation Windows. Il est conçu pour collecter les journaux de performances et des traces, et il déclenche également des alertes de performances lorsque certains déclencheurs sont remplies. PLA peut être utilisé pour collecter les compteurs de performance, event Tracing for Windows (ETW), les requêtes WMI, les clés de Registre et la configuration pour les fichiers. PLA prend également en charge la collecte des données à distance via des appels de procédure distante (RPC). L’utilisateur définit un ensemble de collecteurs de données, qui inclut des informations sur les données à collecter, la fréquence de collecte de données, la durée de collection de données, filtres et un emplacement pour enregistrer les fichiers de résultats. SPA utilise PLA pour collecter toutes les données de performances à partir des serveurs cibles.

**Rapport unique**

Rapport unique est le rapport SPA qui est généré est basé sur une session d’analyse de données pour un module advisor sur un serveur cible unique. Il peut contenir des notifications et des différentes sections de données.

**Rapport de côte à côte**

Un rapport de côte à côte est un rapport SPA qui compare deux rapports uniques pour le même pack d’advisor. Les deux rapports peuvent être générés à partir de serveurs cibles différentes ou des séries d’analyse de performance distinct sur le même serveur cible. Le rapport côte à côte crée la possibilité de comparer deux rapports pour aider les utilisateurs à identifier les comportements anormaux ou paramètres dans un des rapports. Un rapport côte à côte contient des notifications et des différentes sections de données. Dans chaque section, les données à partir de ces deux rapports sont répertorié côte à côte.

**Graphique de tendances**

Un graphique de tendance est le rapport SPA qui sert à enquêter sur les modèles répétitifs des problèmes de performances. Nombreux problèmes de performances répétitives sont causés par des modifications de charge de server planifiées à partir du serveur ou à partir des ordinateurs clients, ce qui peuvent se produire quotidienne ou hebdomadaire. SPA fournit un graphique de tendances de 24 heures et un graphique de tendance de 7 jours pour identifier ces problèmes.

L’utilisateur peut choisir une ou plusieurs séries de données à la fois, qui est une valeur numérique à l’intérieur du rapport unique, tel que **total utilisation moyenne du processeur**. plus précisément, une valeur numérique est une valeur scalaire à partir d’un serveur unique qui est généré par un point d’accès unique à une instance de temps donné. SPA regroupe ces valeurs dans des groupes de 24, une pour chaque heure de la journée (sept pour un rapport de 7 jours, un pour chaque jour de la semaine). SPA calcule la moyenne, minimum, maximum et les écarts types pour chaque groupe.

**Graphique historique**

Un graphique d’historique est le rapport SPA qui est utilisé pour afficher les modifications apportées dans certaines des valeurs numériques à l’intérieur de rapports uniques pour un serveur donné et une paire de pack de conseiller au fil du temps. L’utilisateur peut choisir plusieurs séries de données et les afficher ensemble dans le graphique d’historique pour comprendre la corrélation entre les différentes séries de données.

**Série de données**

Une série de données est des données numériques qui sont collectées à partir de la même source de données sur une période de temps. La même source signifie que les données doit provenir du même serveur cible, tel que la longueur de file d’attente moyenne de demande pour IIS sur un seul serveur.

**règles**

Les règles sont des combinaisons de logique, des seuils et des descriptions. Ils représentent un problème potentiel de performances. Chaque pack d’advisor contient plusieurs règles. Chaque règle est déclenchée par un processus de génération de rapports. Une règle s’applique la logique et les seuils pour les données de rapport unique. Si les critères sont satisfaits, une notification d’avertissement est déclenchée. Si non, la notification est définie sur le **OK** état. Si la règle ne s’applique pas, la notification a la valeur non Applicable (**NA**) état.

**Notifications**

Une notification est une règle affiche aux utilisateurs les informations. Il inclut l’état de la règle (**OK**, **NA**, ou **avertissement**), le nom de la règle et les recommandations possibles pour résoudre les problèmes de performances.
