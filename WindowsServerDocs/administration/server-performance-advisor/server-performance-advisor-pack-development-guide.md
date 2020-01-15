---
title: Guide de développement de packs Server Performance Advisor
description: Guide de développement de packs Server Performance Advisor
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cdf812f862534ba8cd07d4558e424faf3c56c699
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75947145"
---
# <a name="server-performance-advisor-pack-development-guide"></a>Guide de développement de packs Server Performance Advisor

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 8, Windows 10

Ce guide de développement pour Microsoft Server Performance Advisor (SPA) fournit des instructions destinées à aider les développeurs et les administrateurs système à développer des packs d’Advisor pour analyser les performances du serveur.

Il suppose que vous êtes familiarisé avec Journaux et alertes de performance (PLA), les compteurs de performance, les paramètres du Registre, Windows Management Instrumentation (WMI), Suivi d’v nements pour Windows (ETW) et Transact SQL (T-SQL).

Pour plus d’informations sur l’utilisation de SPA, consultez le [Guide de l’utilisateur de Server Performance Advisor](server-performance-advisor-users-guide.md).

## <a name="spa-advisor-pack-overview"></a>Présentation du Pack SPA Advisor


Un Advisor Pack est généralement conçu pour un rôle serveur particulier et définit les éléments suivants :

* Données à collecter via PLA, y compris les Windows Management Instrumentation (WMI), les compteurs de performances, les paramètres de Registre, les fichiers et les Suivi d’v nements pour Windows (ETW)

* Règles, qui affichent les alertes et les recommandations

* Données à afficher (données brutes collectées, données agrégées ou listes des 10 premières)

* Statistiques pour afficher une valeur qui change au fil du temps

* Valeurs de statistiques pouvant être tendances

Un Advisor Pack comprend les éléments suivants :

* **Métadonnées XML** (ProvisionMetadata. Xml)

    * Ensemble de collecteurs [de données journaux et alertes de performance (PLA)](https://msdn.microsoft.com/library/windows/desktop/aa372635.aspx)

    * Mise en page du rapport

* **Scripts SQL**

    * Une procédure stockée principale

    * Objets SQL, tels que les procédures stockées et les fonctions définies par l’utilisateur

* **Fichier de schéma ETW** (Schema. Man) cette option est facultative

### <a name="advisor-pack-workflow"></a>Flux de travail Advisor Pack

![flux de travail Advisor Pack](../media/server-performance-advisor/spa-dev-guide-workflow.png)

Dans cet organigramme, les cercles verts représentent des packs d’Advisor. Tous les autres cercles représentent les phases qui s’exécutent dans le processus de l’infrastructure SPA. SPA utilise un Advisor Pack pour collecter des données, importer les données dans la base de données, initialiser l’environnement d’exécution et exécuter des scripts SQL.

### <a name="collect-data"></a>Collecte des données

Quand un pack Advisor est mis en file d’attente pour un serveur particulier à l’aide de SPA, le module de collecte de données interroge le XML de l’ensemble de collecteurs de données à partir du Pack Advisor et collecte des données à partir du serveur cible. Les données brutes sont stockées dans un partage de fichiers spécifié par l’utilisateur. La collecte de données ne s’arrêtera pas tant que la durée d’exécution du SPA désignée par l’utilisateur n’est pas dépassée.

### <a name="import-data-into-the-database"></a>importer des données dans la base de données

Une fois la collecte de données terminée, chaque type de données est importé dans une table correspondante de la base de données SQL Server. Par exemple, les paramètres du Registre sont importés dans une table nommée \#registryKeys.

l’importation d’un fichier ETW requiert un fichier de schéma ETW pour le décodage du fichier. etl. Le fichier de schéma ETW est un fichier XML. Il peut être généré à l’aide de tracerpt. exe, qui est inclus avec Windows. Le fichier de schéma ETW est requis uniquement lorsque Advisor Pack doit importer des données ETW.

### <a name="switch-to-low-user-rights"></a>Basculer vers les droits d’utilisateur faibles

L’infrastructure SPA ajuste automatiquement les privilèges pour réduire le niveau d’accès de sécurité requis. Étant donné que les packs Advisor peuvent être développés ou modifiés par n’importe qui, il est possible qu’un conseiller Advisor contienne des scripts SQL falsifiés. Pour atténuer le risque de sécurité, n’importe quel script SQL pour un Advisor Pack doit être exécuté avec des droits d’utilisateur faibles. Il peut accéder uniquement aux objets de base de données limités, tels que les tables temporaires et les API SPA exposées en tant que procédures stockées. Les scripts SQL dans un conseiller Pack peuvent appeler ces procédures stockées pour interagir avec l’infrastructure SPA.

### <a name="initialize-execution-environment"></a>Initialiser l’environnement d’exécution

Les packs d’Advisor peuvent générer différents types de sortie, tels que les notifications, les recommandations, les tables de faits, les statistiques et les graphiques pour les statistiques. Les scripts SQL effectuent certains calculs sur les données collectées. Les résultats générés sont stockés dans des tables temporaires via des API publiques SPA. à l’étape d’initialisation, ces tables temporaires et d’autres ressources système doivent être approvisionnées.

### <a name="run-sql-scripts"></a>Exécuter des scripts SQL

Il existe une procédure stockée principale, nommée par le développeur Advisor Pack. L’infrastructure SPA appelle cette procédure stockée pour initier le calcul. La procédure stockée consomme les données collectées et communique le résultat final à l’infrastructure SPA.

### <a name="switch-to-administrative-rights"></a>Basculer vers les droits d’administration

Des droits d’administrateur sont nécessaires pour générer un rapport. La génération de rapports est entièrement contrôlée par SPA. Elle est moins susceptible d’être falsifiée.

### <a name="generate-a-report"></a>Générer un rapport

Avant la fin de la procédure stockée principale pour un Advisor Pack, tous les résultats calculés, tels que les notifications et les statistiques, ne sont pas conservés. Au cours de cette phase, l’infrastructure SPA transfère les résultats finaux des tables temporaires à des tables dans un format particulier. Une fois cette phase terminée, vous pouvez afficher les rapports à l’aide de la console du SPA.

## <a name="authoring-an-advisor-pack"></a>Création d’un Advisor Pack


### <a name="quick-guidelines"></a>Instructions rapides

L’organigramme suivant décrit les étapes à suivre pour développer un Advisor Pack entièrement fonctionnel. Cette section comprend également des exemples pas à pas permettant de mieux expliquer chaque étape.

![processus de développement Advisor Pack](../media/server-performance-advisor/spa-dev-guide-dev-flowchart.png)

Un Advisor Pack est généralement structuré de la façon suivante :

Pack Advisor

ProvisionMetadata. Xml

scripts ;

main. SQL

Func. SQL

Schema. Man

Chaque pack Advisor doit avoir un fichier appelé ProvisionMetadata. Xml. Il définit les informations de base sur le Pack, les données à collecter, les notifications et les règles, ainsi que la façon dont le rapport doit être stocké et affiché. L’infrastructure SPA utilise ces informations pour générer une table temporaire, puis transférer les résultats de la table temporaire dans une table à laquelle les utilisateurs peuvent accéder.

Tous les scripts SQL de rapport doivent être enregistrés dans un sous-dossier appelé **scripts**. À des fins de maintenance, nous vous recommandons d’enregistrer différents objets de base de données dans différents fichiers de SQL Server. Il doit y avoir au moins une procédure stockée comme point d’entrée principal.

> [!NOTE]
> Le fichier Schema. Man n’est pas nécessaire, sauf si votre pack d’analyse recueille des traces ETW. Ce fichier de schéma est utilisé pour décrire le schéma des événements ETW et décoder les événements ETW.

### <a name="defining-basic-information"></a>Définition des informations de base

Cette section décrit certains des éléments de base qui composent un Advisor Pack, y compris ProvisionMetadata. xml et les attributs.

Voici un exemple d’en-tête pour le fichier ProvisionMetadata. xml :

``` syntax
<advisorPack
xmlns="https://microsoft.com/schemas/ServerPerformanceAdvisor/ap/2010"
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

### <a name="advisor-pack-version"></a>Version Advisor Pack

nom de l’attribut : **version**

Les développeurs Advisor Pack peuvent définir des versions majeures et mineures pour le Pack Advisor :

* Une version majeure implique généralement des améliorations significatives. Les résultats générés par une ancienne version peuvent ne pas être compatibles avec le nouveau. Nous vous recommandons vivement d’inclure la version majeure dans le nom du Pack Advisor.

* SPA autorise des mises à niveau de versions mineures quand il n’y a que des modifications mineures sans problèmes d’incompatibilité des données.

Pour plus d’informations sur le contrôle de version, consultez [Rubriques avancées](#bkmk-advancedtopics).

### <a name="script-entry-point"></a>Point d’entrée de script

nom de l’attribut : **reportScript**

L’infrastructure SPA recherche le nom de la procédure stockée principale à partir du point d’entrée de script et l’exécute de façon sécurisée.

### <a name="other-attributes"></a>Autres attributs

Voici d’autres attributs qui peuvent être utilisés pour identifier un pack Advisor :

* Nom complet : **DisplayName**

* Description : **Description**

* Auteur : **auteur**

* Version du Framework : **frameworkVersion** (3,0 par défaut)

* Version minimale du système d’exploitation : **minOSversion** (réservé pour une future extensibilité)

* Notification d’événement perdue : **showEventLostWarning**

### <a href="" id="bkmk-definedatacollector"></a>Définition de l’ensemble de collecteurs de données

Un ensemble de collecteurs de données définit les données de performances que l’infrastructure SPA doit collecter à partir du serveur cible. Il prend en charge les paramètres du Registre, WMI, les compteurs de performances, les fichiers du serveur cible et ETW.

``` syntax
<advisorPack>
<dataSourceDefinition xmlns="https://microsoft.com/schemas/ServerPerformanceAdvisor/dc/2010">
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

L’attribut **Duration** de **&lt;dataCollectorSet/&gt;** dans l’exemple précédent définit la durée de la collecte de données (l’unité de temps est de secondes). **Duration** est un attribut obligatoire. Ce paramètre contrôle la durée de la collection qui est utilisée par les compteurs de performance et le suivi ETW.

### <a name="collect-registry-data"></a>Collecter les données du Registre

Vous pouvez collecter des données de Registre à partir des ruches de Registre suivantes :

* CLASSES HKEY\_\_racine

* CONFIGURATION de la\_actuelle HKEY\_

* HKEY\_l’utilisateur actuel\_

* MACHINE\_locale HKEY\_

* UTILISATEURS HKEY\_

Pour collecter un paramètre du Registre, spécifiez le chemin d’accès complet du nom de la valeur : HKEY\_LOCAL\_MACHINE\\MyKey\\MyValue

Pour collecter tous les paramètres sous une clé de Registre, spécifiez le chemin d’accès complet à la clé de Registre : HKEY\_LOCAL\_MACHINE\\MyKey\\

Pour collecter toutes les valeurs sous une clé de Registre et ses sous-clés (PLA collecte de manière récursive les données du registre), utilisez deux barres obliques inverses pour le dernier délimiteur de chemin d’accès : HKEY\_LOCAL\_ordinateur\\MyKey\\\\

Pour collecter des informations de Registre à partir d’un ordinateur distant, incluez le nom de l’ordinateur au début du chemin d’accès au registre : HKEY\_LOCAL\_MACHINE\\MyKey\\MyValue

Par exemple, vous pouvez avoir une clé de Registre qui se présente comme suit :

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

Exemple 1 : retourner uniquement les PowerSchemes actifs et leurs valeurs :

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes</registryKey>
```

Exemple 2 : retourne toutes les paires clé/valeur sous ce chemin d’accès :

> [!NOTE]
> PLA s’exécute sous les informations d’identification de l’utilisateur. Certaines clés de Registre requièrent des informations d’identification d’administration. L’énumération s’arrête lorsqu’elle ne parvient pas à accéder aux sous-clés.

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Power\User\PowerSchemes\\</registryKey>
```

Toutes les données collectées seront importées dans une table temporaire appelée **\#registryKeys** avant l’exécution d’un script de rapport SQL. Le tableau suivant présente les résultats de l’exemple 2 :

KeyName | KeytypeId | Value
------ | ----- | -------
HKEY_LOCAL_MACHINE. ..\PowerSchemes | 1 | db310065-829b-4671-9647-2261c00e86ef
\db310065-829b-4671-9647-2261c00e86ef\Description | 2 | |
\db310065-829b-4671-9647-2261c00e86ef\FriendlyName | 2 | Source d’alimentation optimisée
...\6738e2c4-e8a5-4a42-b16a-e040e769756e\ACSettingIndex | 4 | 180
...\6738e2c4-e8a5-4a42-b16a-e040e769756e\DCSettingIndex | 4 | 30

Le schéma de la table **#registryKeys** est le suivant :

Nom de la colonne | Type de données SQL | Description
-------- | -------- | --------
KeyName | Nvarchar (300) non NULL | Nom du chemin d’accès complet de la clé de Registre
KeytypeId | Smallint n’est pas NULL | ID de type interne
Value | Nvarchar (4000) non NULL | Toutes les valeurs

La colonne **KeytypeID** peut avoir l’un des types suivants :

ID | Tapez
--- | ---
1 | Chaîne
2 | expandString
3 | Binaire
4 | DWord
5 | DWordBigEndian
6 | Lier
7 | MultipleString
8 | Resourcelist
9 | FullResourceDescriptor
10 | ResourceRequirementslist
11 | Q

### <a name="collect-wmi"></a>Collecter WMI

Vous pouvez ajouter n’importe quelle requête WMI. Pour plus d’informations sur l’écriture de requêtes WMI, consultez [WQL (SQL pour WMI)](https://msdn.microsoft.com/library/windows/desktop/aa394606.aspx). L’exemple suivant interroge l’emplacement d’un fichier d’échange :

``` syntax
<path>Root\Cimv2:select * FROM Win32_PageFileUsage</path>
```

La requête de l’exemple ci-dessus retourne un enregistrement :

Caption | Nom | PeakUsage
----- | ----- | -----
C:\pagefile.sys | C:\pagefile.sys | 215

Étant donné que WMI retourne une table avec des colonnes différentes, lorsque les données collectées sont importées dans une base de données, SPA effectue la normalisation des données et est ajouté aux tables suivantes :

**\#table WMIObjects**

SequenceID | Espace de noms | ClassName | RelativePath | WmiqueryID
----- | ----- | ----- | ----- | -----
10 | Root\Cimv2 | Win32_PageFileUsage | Win32_PageFileUsage. Name =<br>C :\\pagefile. sys | 1

**\#table WmiObjectsProperties**

ID | requête
--- | ---
1 | Root\Cimv2 : SELECT * FROM Win32_PageFileUsage

**\#table WmiQueries**

ID | requête
--- | ---
1 | Root\Cimv2 : SELECT * FROM Win32_PageFileUsage

**\#le schéma de la table WmiObjects**

Nom de la colonne | Type de données SQL | Description
--- | --- | ---
SequenceId | Int non NULL | Mettre en corrélation la ligne et ses propriétés
Espace de noms | Nvarchar (200) non NULL | Espace de noms WMI
ClassName | Nvarchar (200) non NULL | Nom de classe WMI
RelativePath | Nvarchar (500) non NULL | Chemin d’accès relatif WMI
WmiqueryId | Int non NULL | Mettre en corrélation la clé de #WmiQueries

**\#le schéma de la table WmiObjectProperties**

Nom de la colonne | Type de données SQL | Description
--- | --- | ---
SequenceId | Int non NULL | Mettre en corrélation la ligne et ses propriétés
Nom | Nvarchar (1000) non NULL | Nom de la propriété
Value | Nvarchar (4000) NULL | Valeur de la propriété actuelle

**\#le schéma de la table WmiQueries**

Nom de la colonne | Type de données SQL | Description
--- | --- | ---
Id | Int non NULL | ID de requête unique >
requête | Nvarchar (4000) non NULL | Chaîne de requête d’origine dans les métadonnées d’approvisionnement

### <a name="collect-performance-counters"></a>Collecter les compteurs de performances

Voici un exemple de collecte d’un compteur de performances :

``` syntax
<performanceCounters interval="1">
  <performanceCounter>\PhysicalDisk(*)\Avg. Disk sec/Transfer</performanceCounter>
</performanceCounters>
```

L’attribut **Interval** est un paramètre global requis pour tous les compteurs de performance. Il définit l’intervalle (l’unité de temps est de secondes) de la collecte des données de performances.

Dans l’exemple précédent, le compteur \\PhysicalDisk (\*)\\moyenne disque s/transfert est interrogé chaque seconde.

Il peut y avoir deux instances : **\_total** et **0 C : D :** , et la sortie peut être la suivante :

timestamp | NomCatégorie | CounterName | Valeur d’instance de _Total | Valeur de l’instance de 0 C : D :
---- | ---- | ---- | ---- | ----
13:45:52.630 | PhysicalDisk | Moyenne disque s/transfert | 0.00100008362473995 |0.00100008362473995
13:45:53.629 | PhysicalDisk | Moyenne disque s/transfert | 0.00280023414927187 | 0.00280023414927187
13:45:54.627 | PhysicalDisk | Moyenne disque s/transfert | 0.00385999853230048 | 0.00385999853230048
13:45:55.626 | PhysicalDisk | Moyenne disque s/transfert | 0.000933297607934224 | 0.000933297607934224

Pour importer les données dans la base de données, les données sont normalisées dans une table appelée **\#PerformanceCounters**.

CategoryDisplayName | InstanceName | CounterdisplayName | Value
---- | ---- | ---- | ----
PhysicalDisk | _Total | Moyenne disque s/transfert | 0.00100008362473995
PhysicalDisk | 0 C : D : | Moyenne disque s/transfert | 0.00100008362473995
PhysicalDisk | _Total | Moyenne disque s/transfert | 0.00280023414927187
PhysicalDisk | 0 C : D : | Moyenne disque s/transfert | 0.00280023414927187
PhysicalDisk | _Total | Moyenne disque s/transfert | 0.00385999853230048
PhysicalDisk | 0 C : D : | Moyenne disque s/transfert | 0.00385999853230048
PhysicalDisk | _Total | Moyenne disque s/transfert | 0.000933297607934224
PhysicalDisk | 0 C : D : | Moyenne disque s/transfert | 0.000933297607934224

**Remarque** Les noms localisés, tels que **CategoryDisplayName** et **CounterdisplayName**, varient en fonction de la langue d’affichage utilisée sur le serveur cible. Évitez d’utiliser ces champs si vous souhaitez créer un Advisor Pack indépendant de la langue.

**\#** le schéma de la table PerformanceCounters

Nom de la colonne | Type de données SQL | Description
---- | ---- | ---- | ----
timestamp | datetime2 (3) NOT NULL | Date et heure collectées dans l’UNC
NomCatégorie | Nvarchar (200) non NULL | Nom de catégorie
CategoryDisplayName | Nvarchar (200) non NULL | Nom de catégorie localisé
InstanceName | Nvarchar (200) NULL | Nom de l'instance
CounterName | Nvarchar (200) non NULL | Nom du compteur
CounterdisplayName | Nvarchar (200) non NULL | Nom du compteur localisé
Value | Float NOT NULL | Valeur collectée

### <a name="collect-files"></a>Collecter des fichiers

Les chemins d’accès peuvent être absolus ou relatifs. Le nom de fichier peut inclure le caractère générique (\*) et le point d’interrogation ( ?). Par exemple, pour collecter tous les fichiers dans le dossier temporaire, vous pouvez spécifier c :\\Temp\\\*. Le caractère générique s’applique aux fichiers dans le dossier spécifié.

Si vous souhaitez également collecter des fichiers à partir des sous-dossiers du dossier spécifié, utilisez deux barres obliques inverses pour le dernier délimiteur de dossier, par exemple c :\\Temp\\\\\*.

Voici un exemple qui interroge le fichier **ApplicationHost. config** :

``` syntax
<path>%windir%\System32\inetsrv\config\applicationHost.config</path>
```

Les résultats se trouvent dans une table appelée **fichiers\#** , par exemple :

querypath | FullPath | ParentPath | FileName | Content
----- | ----- | ----- | ----- | -----
% windir%\... \applicationHost.config |C:\Windows<br>\... \applicationHost.config | C:\Windows<br>\... \Config | applicationHost. confi | 0x3C3F78

**schéma de la table de fichiers \#**

Nom de la colonne | Type de données SQL | Description
---- | ---- | ----
querypath | Nvarchar (300) non NULL | Instruction de la requête d’origine
FullPath | Nvarchar (300) non NULL | Chemin d’accès et nom de fichier absolus
ParentPath | Nvarchar (300) non NULL | Chemin d’accès au fichier
FileName | Nvarchar (300) non NULL | Nom du fichier
Content | Varbinary (MAX) NULL | Contenu de fichier en binaire

### <a name="defining-rules"></a>Définition des règles

Une fois que suffisamment de données ont été collectées à l’aide de PLA à partir d’un serveur cible, Advisor Pack peut utiliser ces données pour la validation et afficher un résumé rapide pour les administrateurs système.

Les règles offrent une vue d’ensemble rapide des performances du serveur. Ils mettent en évidence les problèmes et fournissent des recommandations. Vous pouvez répertorier toutes les règles que vous souhaitez valider pour un Advisor Pack. Par exemple, si vous souhaitez développer un système d’exploitation principal, les règles possibles sont les suivantes :

* Si le mode d’alimentation de l’UC est l’économie d’énergie

* Si le serveur se trouve dans un environnement virtualisé

* Indique s’il y a une pression d’e/s disque

Les règles contiennent les éléments suivants :

* Seuil dépendant (partie configurable d’une règle)

* Définition de règle (alertes et recommandations)

Voici un exemple de règle simple :

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

Le seuil est un facteur configurable qui permet aux administrateurs système de décider quand une règle doit indiquer un état correct ou mauvais. L’exemple suivant montre une règle pour détecter l’espace libre sur un lecteur système et un avertissement lorsque l’espace libre est inférieur à 10 Go.

``` syntax
<threshold name="freediskSize" caption="Free Disk Size (GB)" description="Free Disk Size  value="10" />
```

Toutefois, dans ce cas, l’administrateur système dispose d’un disque dur plus petit. Il pense que 5 Go d’espace libre peuvent toujours être une bonne condition et il ne souhaite pas voir un avertissement. Il peut mettre à jour la valeur par défaut de 10 à 5 via la console du SPA sans avoir à comprendre comment développer un Advisor Pack.

L’introduction d’un seuil permet aux administrateurs système de modifier rapidement la valeur sans avoir à modifier le Pack Advisor.

Dans l’exemple, tous les attributs, à l’exception de **Description** , sont requis. Vous pouvez utiliser n’importe quel nombre pour **value**.

Un seuil peut être partagé entre les règles.

### <a name="alerts-and-recommendations"></a>Alertes et recommandations

La définition de règle n’implique aucun calcul logique. Il définit la manière dont l’interface utilisateur peut s’afficher et la façon dont le script de rapport SQL Server communique les résultats à l’interface utilisateur.

Une règle se compose de trois parties :

* Alerte (légende de règle)

* Recommandation (Conseil)

* Seuil associé (informations facultatives sur les dépendances)

Voici un exemple de règle :

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

Vous pouvez définir autant de conseils que vous le souhaitez, et vous définirez généralement des recommandations. Le **niveau** de Conseil peut être **réussite** ou **Avertissement**.

Vous pouvez créer un lien vers autant de seuils que vous le souhaitez. Vous pouvez même établir un lien vers un seuil qui n’est pas pertinent pour la règle actuelle. La liaison permet à la console SPA de gérer facilement les seuils.

Le nom de la règle et les recommandations sont des clés et sont uniques dans leur portée. Deux règles ne peuvent pas porter le même nom, et deux recommandations au sein d’une règle peuvent avoir le même nom. Ces noms sont très importants lorsque vous écrivez un rapport de script SQL. Vous pouvez appeler le \[dbo\].\[l’API SetNotification\] pour définir l’état de la règle.

### <a name="defining-ui-display-elements"></a>Définition des éléments d’affichage de l’interface utilisateur

Une fois les règles définies, les administrateurs système peuvent voir le résumé du rapport. Toutefois, les administrateurs système sont souvent intéressés par les données agrégées et veulent vérifier les sources de données qui ont été utilisées dans les règles de performances.

Si vous continuez avec l’exemple précédent, l’utilisateur sait si l’espace disque disponible est suffisant sur le lecteur système. Les utilisateurs peuvent également être intéressés par la taille réelle de l’espace libre. Un groupe de valeurs unique est utilisé pour stocker et afficher ces résultats. Plusieurs valeurs uniques peuvent être regroupées et affichées dans une table de la console du SPA. La table ne contient que deux colonnes, Name et value, comme indiqué ici.

Nom | Value
---- | ----
Taille du disque libre sur le lecteur système (en Go) | 100
Taille totale du disque installée (Go) | 500 

Si un utilisateur souhaite voir la liste de tous les disques durs installés sur le serveur et la taille de leur disque, nous pourrions appeler une valeur de liste, qui comporte trois colonnes et plusieurs lignes, comme illustré ici.

Disque | Taille de disque libre (Go) | Taille totale (Go)
---- | ---- | ----
0 | 100 | 500
1 | 20 | 320

Dans un pack Advisor, il peut y avoir de nombreuses tables (groupes de valeurs uniques et tables de valeurs de liste). Nous pouvons utiliser une section pour organiser et classer ces tables.

En résumé, il existe trois types d’éléments d’interface utilisateur :

* [Sections](#bkmk-ui-section)

* [Groupes à valeur unique](#bkmk-ui-svg)

* [tables de valeurs de liste](#bkmk-ui-lvt)

Voici un exemple qui montre les éléments d’interface utilisateur :

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

Une section est purement pour la disposition de l’interface utilisateur. Elle ne participe à aucun calcul logique. Chaque rapport contient un ensemble de sections de niveau supérieur qui n’ont pas de section parente. Les sections de niveau supérieur sont présentées sous la forme d’onglets dans le rapport. Les sections peuvent comporter des sous-sections, avec un maximum de 10 niveaux. Toutes les sous-sections sous les sections de niveau supérieur sont présentées dans des zones développables. Une section peut contenir plusieurs sous-sections, groupes de valeurs uniques et tables de valeurs de liste. Les groupes de valeurs uniques et les tables de valeurs de liste sont présentés sous forme de tables.

Voici un exemple de section de niveau supérieur.

``` syntax
<section name="CPU" caption="CPU"/>
```

Un nom de section doit être unique. Elle est utilisée en tant que clé qui peut être liée à d’autres sections, à des groupes à valeur unique et à des tables de valeurs de liste.

L’exemple suivant a un attribut, **parent**, et pointe vers la section CPU. CPUFacts est un enfant de la section nommée UC. le **parent** doit faire référence à un nom de section précédent ; dans le cas contraire, elle peut entraîner une boucle.

``` syntax
<section name="CPUFacts" caption="Facts" parent="CPU"/>
```

Le groupe à valeur unique suivant a un attribut, **section**et peut pointer vers n’importe quelle section, en fonction de la conception de votre interface utilisateur.

``` syntax
<singleValue name="CPUInformation" section="CPUFacts" caption="Physical CPU Information"> </singleValue>
```

### <a name="data-types"></a>Type de données

Un groupe de valeurs unique et une table de valeurs de liste contiennent des types de données différents, tels que String, int et float. Étant donné que ces valeurs sont stockées dans la base de données SQL Server, vous pouvez définir un type de données SQL pour chaque propriété de données. Toutefois, la définition d’un type de données SQL est très complexe. Vous devez spécifier la longueur ou la précision, qui peut être sujette à modification.

Pour définir des types de données logiques, vous pouvez utiliser le premier enfant de **&lt;fichier ReportDefinition/&gt;** , où vous pouvez définir un mappage du type de données SQL et de votre type logique.

L’exemple suivant définit deux types de données. L’une est **String** et l’autre est **companyCode**.

``` syntax
<datatype name="string" = sqltype="nvarchar(4000)" />
<datatype name="companyCode" sqltype="nvarchar(100)" />
```

Un nom de type de données peut être n’importe quelle chaîne valide. Voici une liste de types de données SQL autorisés :

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

* money

* nchar

* numeric

* nvarchar

* réels

* smalldatetime

* smallint

* smallmoney

* heure

* tinyint

* uniqueidentifier

* varbinary

* varchar

Pour plus d’informations sur ces types de données SQL, consultez [types de données (Transact-SQL)](https://msdn.microsoft.com/library/ms187752.aspx).

### <a href="" id="bkmk-ui-svg"></a>Groupes à valeur unique

Un groupe de valeurs unique regroupe plusieurs valeurs uniques pour qu’elles soient présentes dans une table, comme illustré ici.

``` syntax
<singleValue name="Systemoverview" section="SystemoverviewSection" caption="Facts">
<value name="OsName" type="string" caption="Operating system" description="WMI: Win32_OperatingSystem/Caption"/>
<value name="Osversion" type="string" caption="OS version" description="WMI: Win32_OperatingSystem/version"/>
<value name="OsLocation" type="string" caption="OS location" description="WMI: Win32_OperatingSystem/SystemDrive"/>
</singleValue>
```

Dans l’exemple précédent, nous avons défini un groupe de valeurs unique. Il s’agit d’un nœud enfant de la section **SystemoverviewSection**. Ce groupe a des valeurs uniques, qui sont **OsName**, **OSVersion**et **OsLocation**.

Une valeur unique doit avoir un attribut de nom unique global. Dans cet exemple, l’attribut de nom unique global est **Systemoverview**. Le nom unique sera utilisé pour générer une vue correspondante pour le rapport personnalisé. Chaque vue contient le préfixe **VW**, tel que vwSystemoverview.

Bien que vous puissiez définir plusieurs groupes à valeur unique, deux noms à valeur unique ne peuvent pas être identiques, même s’ils se trouvent dans des groupes différents. Le nom de la valeur unique est utilisé par le rapport de script SQL pour définir la valeur en conséquence.

Vous pouvez définir un type de données pour chaque valeur unique. L’entrée autorisée pour le **type** est définie dans **&lt;type de données/&gt;** . Le rapport final peut se présenter comme suit :

**Faits**

Nom | Value
--- | ---
Système d’exploitation | &lt;_une valeur sera définie par le script de rapport_&gt;
Version du système d'exploitation | &lt;_une valeur sera définie par le script de rapport_&gt;
Emplacement du système d’exploitation | &lt;_une valeur sera définie par le script de rapport_&gt;

L’attribut **Caption** de **&lt;valeur/&gt;** est présenté dans la première colonne. Les valeurs de la colonne valeur sont définies à l’avenir par le rapport de script via \[\]dbo.\[\]SetSingleValue. L’attribut **Description** de **&lt;valeur/&gt;** est affiché dans une info-bulle. En général, l’info-bulle montre aux utilisateurs la source des données. Pour plus d’informations sur les info-bulles, consultez [info-bulles](#bkmk-tooltips).

### <a href="" id="bkmk-ui-lvt"></a>tables de valeurs de liste

La définition d’une valeur de liste est identique à la définition d’une table.

``` syntax
<listValue name="NetworkAdapterInformation" section="NetworkIOFacts" caption="Physical network adapter information">
<column name="NetworkAdapterId" type="string" caption="ID" description="WMI: Win32_NetworkAdapter/DeviceID"/>
<column name="NetworkAdapterName" type="string" caption="Name" description="WMI: Win32_NetworkAdapter/Name"/>
<column name="type" type="string" caption="type" description="WMI: Win32_NetworkAdapter/Adaptertype"/>
<column name="Speed" type="decimal" caption="Speed (Mbps)" description="WMI: Win32_NetworkAdapter/Speed"/>
<column name="MACaddress" type="string" caption="MAC address" description="WMI: Win32_NetworkAdapter/MACaddress"/>
</listValue>
```

Le nom de la valeur de liste doit être globalement unique. Ce nom devient le nom d’une table temporaire. Dans l’exemple précédent, la table nommée \#NetworkAdapterInformation est créée lors de l’étape d’initialisation de l’environnement d’exécution, qui contient toutes les colonnes décrites. Semblable à un nom de valeur unique, un nom de valeur de liste est également utilisé dans le nom de la vue personnalisée, par exemple vwNetworkAdapterInformation.

@type de &lt;colonne/&gt; est définie par &lt;type de données/&gt;

L’interface utilisateur factice du rapport final peut se présenter comme suit :

**Informations sur la carte réseau physique**

ID | Nom | Tapez | Vitesse (Mbits/s) | Adresse MAC
--- | --- | --- | --- | ---
 | <br> | | |
 | | | |


L’attribut **Caption** de &lt;column/&gt; est affiché sous la forme d’un nom de colonne, et l’attribut **Description** de &lt;Column/&gt; est affiché sous la forme d’une info-bulle pour l’en-tête de colonne correspondant. En général, l’info-bulle montre à l’utilisateur la source des données. Pour plus d’informations, consultez [info-bulles](#bkmk-tooltips).

Dans certains cas, une table peut comporter un grand nombre de colonnes et seulement quelques lignes. par conséquent, le remplacement des colonnes et des lignes rendra la table plus performante. Pour permuter les colonnes et les lignes, vous pouvez ajouter l’attribut de style suivant :

``` syntax
<listValue style="Transpose"  
```

### <a name="defining-charting-elements"></a>Définition des éléments de graphique

Vous pouvez choisir n’importe quelle clé de statistiques et afficher les valeurs dans un graphique d’historique ou un graphique de tendances. Il existe deux types de statistiques :

* **Statistiques statiques** Valeur unique, connue au moment de la conception. Par exemple, l’espace disque libre sur un lecteur système serait une statistique statique.

* **Statistiques dynamiques** Peut être inconnu au moment de la conception. Par exemple, l’utilisation moyenne de l’UC de chaque cœur est une statistique dynamique, car vous ne connaissez pas le nombre de cœurs de processeur pouvant être dans le système au moment de la conception.

La clé de statistiques a une limite selon laquelle les données doivent être compatibles avec le type de données double. Il peut s’agir d’un entier, d’une valeur décimale ou d’une chaîne pouvant être convertie en valeur double.

SPA utilise un groupe de valeurs unique pour prendre en charge les statistiques statiques et une table de valeurs de liste pour prendre en charge les statistiques dynamiques. Les sections suivantes décrivent comment définir des statistiques statiques et des clés de statistiques dynamiques.

### <a name="static-statistics"></a>Statistiques statiques

Comme mentionné précédemment, une statistique statique est une valeur unique. Logiquement, toute valeur unique peut être définie en tant que Statistique statique. Toutefois, il est inutile d’afficher une valeur unique qui ne peut pas être convertie en un type nombre. Pour définir une statistique statique, vous pouvez simplement ajouter l’attribut **trendable** à la clé de valeur unique correspondante, comme indiqué ci-dessous :

``` syntax
<value name="freediskSize" type="int" trendable="true"  
```

### <a name="dynamic-statistics"></a>Statistiques dynamiques

Les clés de statistiques dynamiques ne sont pas connues au moment de la conception, le nombre de valeurs possibles est donc inconnu. Toutefois, étant donné que les valeurs de liste sont stockées sur plusieurs lignes, il est facile d’utiliser une table de valeurs de liste pour stocker les statistiques dynamiques.

par exemple, si nous avons besoin d’afficher des graphiques pour l’utilisation moyenne du processeur de différents cœurs, nous pourrions définir une table avec des colonnes pour **CpuId** et **AverageCpuUsage**:

``` syntax
<listValue name="CpuPerformance">
<column name="CpuId" type="string" caption="CPU ID" columntype="Key"/>
<column name="AverageCpuUsage" type="decimal" caption="Average" columntype="Value"/>
</listValue>
```

Un autre attribut, **ColumnType**, peut être une **clé**, une **valeur**ou une **information**. Le type de données de la colonne **clé** doit être double ou convertible en double. Dans une colonne **clé** , vous ne pouvez pas insérer les mêmes clés dans une table. La **valeur** ou les colonnes d' **information** n’ont pas cette limitation.

Les valeurs des statistiques sont stockées dans des colonnes de **valeur** .

Les colonnes d' **information** sont semblables aux colonnes ordinaires dans les tables de valeurs de liste normale. **Informatif** est le type de colonne par défaut si vous n’en spécifiez pas un. Ces colonnes n’affectent pas le nombre de clés de statistiques ou ne participent pas aux calculs relatifs aux statistiques.

Dans l’exemple précédent, si un serveur a deux cœurs d’UC, le résultat dans la table peut se présenter comme suit :

CpuId | AverageCpuUsage
:---: | :---:
0 | 10
1 | 30

En même temps, deux clés de statistiques sont générées par l’infrastructure SPA. L’une est pour l’UC 0 et l’autre pour l’UC 1.

L’exemple suivant indique que plusieurs colonnes de **valeurs** avec plusieurs colonnes **clés** sont prises en charge.

CounterName | InstanceName | Moyenne | Sum
--- | :---: | :---: | :---:
% temps processeur | _Total | 10 | 20
% temps processeur | PROCESSEUR | 20 | 30 

Dans cet exemple, vous avez deux colonnes **clés** et deux colonnes de **valeur** . SPA génère deux clés de statistiques pour la colonne moyenne et deux autres clés pour la colonne Sum. Les clés des statistiques sont les suivantes :

* CounterName (% temps processeur)/nom_instance (\_total)/moyenne

* CounterName (% temps processeur)/nom_instance (CPU0)/moyenne

* CounterName (% temps processeur)/nom_instance (\_total)/Sum

* CounterName (% temps processeur)/nom_instance (CPU0)/somme

CounterName et InstanceName sont combinés comme une clé. La clé combinée ne peut pas avoir de duplication.

SPA génère de nombreuses clés de statistiques. Certaines d’entre elles peuvent ne pas être intéressantes pour vous, et vous pouvez les masquer dans l’interface utilisateur. SPA permet aux développeurs de créer un filtre pour afficher uniquement les clés de statistiques utiles.

pour l’exemple précédent, les administrateurs système peuvent uniquement être intéressés par les clés dans lesquelles InstanceName est \_total ou CPU1. Le filtre peut être défini comme suit :

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

**&lt;trendableKeyValues/&gt;** peut être défini sous n’importe quelle colonne clé. Si plusieurs colonnes clés ont un tel filtre configuré et que la logique est appliquée.

### <a name="developing-report-scripts"></a>Développement de scripts de rapport

Une fois les métadonnées de configuration définies, nous pouvons commencer à écrire le script de rapport, qui est une procédure stockée T-SQL.

Il existe des attributs **Name** et **reportScript** dans l’en-tête de métadonnées provision, comme illustré ici :

``` syntax
<advisorPack name="Microsoft.ServerPerformanceAdvisor.CoreOS.V1" reportScript="ReportScript"  
```

Le principal script de rapport est nommé en combinant les attributs **Name** et **reportScript** . Dans l’exemple suivant, il sera \[Microsoft. ServerPerformanceAdvisor. Coreos. v2\].\[\]ReportScript.

``` syntax
create PROCEDURE [Microsoft.ServerPerformanceAdvisor.CoreOS.V2].[ReportScript] AS SET NOCOUNT ON

- Set alert and notification

- Prepare data for report view
```

L’attribut **Name** sera utilisé comme nom de schéma de base de données, tel qu’un espace de noms. Cette règle s’applique à tous les autres objets de base de données qui appartiennent à l’Advisor Pack actuel, tels que la valeur de liste et les procédures stockées.

Les avantages de l’utilisation de ce nom de schéma devant les objets de base de données sont les suivants :

* Éviter les conflits de noms pour différents packs d’Advisor

* Amélioration de la sécurité

Dans la base de données SQL Server, le nom de schéma par défaut est **dbo**. Les informations d’identification du propriétaire de la base de données sont généralement requises pour utiliser les objets de base de données sous **dbo**. Si nous ne créons pas de schéma pour chaque Advisor Pack, il est probable que deux packs d’Advisor définissent une valeur de liste portant le même nom. Cela ne devrait pas être sans importance, car vous pouvez introduire un nom de schéma pour résoudre ce problème. En outre, l’annulation de l’approvisionnement d’un Advisor Pack est beaucoup plus facile. Étant donné que l’objet Advisor Pack appartient à un schéma autre que **dbo**, cela permet à Spa d’utiliser un privilège utilisateur inférieur pour y accéder.

Un script de rapport normal effectue les opérations suivantes :

* Accède aux données brutes collectées

* Effectue des calculs basés sur les données brutes.

* Modifie les alertes et les recommandations

* Prépare les données pour la vue rapport

### <a name="access-raw-collected-data"></a>Accéder aux données brutes collectées

Toutes les données collectées sont importées dans les tables correspondantes suivantes. Pour plus d’informations sur le schéma de table, consultez [définition de l’ensemble de collecteurs de données](#bkmk-definedatacollector).

* registry

    * \#registryKeys

* WMI

    * \#WMIObjects

    * \#WmiObjectProperties

    * \#WmiQueries

* Compteur de performance

    * \#PerformanceCounters

* File

    * Fichiers \#

* ETW

    * Événements de \#

    * \#EventProperties

### <a name="set-rule-status"></a>Définir l’état de la règle

\]dbo \[.\[API SetNotification\] définit l’état de la règle, de sorte que vous pouvez voir une icône de **réussite** ou d' **Avertissement** dans l’interface utilisateur.

* @ruleName nvarchar (50)

* @adviceName nvarchar (50)

Les messages d’alerte et de recommandation sont stockés dans le fichier XML des métadonnées de mise en service. Cela rend le script de rapport plus facile à gérer.

Initialement, chaque État de règle est N/A. Vous pouvez utiliser cette API pour définir l’état d’une règle en spécifiant un nom de Conseil. Le niveau du nom du Conseil sera utilisé comme état de la règle.

Rappelez-vous que nous avons défini précédemment la règle suivante :

``` syntax
<rule name="freediskSize" caption="Free Disk Size on System Drive" description="This rule checks free disk size on the system drive ">
<advice name="SuccessAdvice" level="Success" message="No issue found.">No recommendation.</advice>
<advice name="WarningAdvice" level="Warning" message="Not enough free space on system drive.">Install the operating system on a larger disk.</advice>
</rule>
```

En supposant que l’espace libre est inférieur à 2 Go, nous avons besoin de définir la règle sur le niveau d' **Avertissement** . Le script SQL se présente comme suit :

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

### <a name="get-threshold-value"></a>Obtient la valeur de seuil

\]dbo \[.\[API GetThreshold\] obtient les seuils :

* @key nvarchar (50)

* sortie @value float

> [!NOTE]
> Les seuils sont des paires nom-valeur et peuvent être référencés dans toutes les règles. Les administrateurs système peuvent utiliser la console du SPA pour ajuster les seuils.

 Dans l’exemple précédent, pour un seuil, la définition se présente comme suit :

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

\]dbo \[.\[API SetSingleValue\] définit la valeur unique :

* @key nvarchar (50)

* @value SQL\_variant

Cette valeur peut être exécutée plusieurs fois pour la même clé de valeur unique. La dernière valeur est enregistrée.

L’exemple suivant montre des valeurs uniques définies :

``` syntax
<singleValue section="Systemoverview" caption="Facts">
<value name="OsName" type="string" caption="Operating System" description="WMI: Win32_OperatingSystem/Caption"/>
<value name="Osversion" type="string" caption="OS version" description="WMI: Win32_OperatingSystem/version"/>
<value name="OsLocation" type="string" caption="OS Location" description="WMI: Win32_OperatingSystem/SystemDrive"/>
</singleValue>
```

Vous pouvez ensuite définir la valeur unique comme indiqué ici :

``` syntax
exec dbo.SetSingleValue N OsName ,  Windows 7 
exec dbo.SetSingleValue N Osversion ,  6.1.7601 
exec dbo.SetSingleValue N OsLocation ,  c:\ 
```

Dans de rares cas, vous souhaiterez peut-être supprimer le résultat que vous avez défini précédemment à l’aide de la \[dbo\]. API\[removeSingleValue\].

* @key nvarchar (50)

Vous pouvez utiliser le script suivant pour supprimer la valeur précédemment définie.

``` syntax
exec dbo.removeSingleValue N Osversion 
```

### <a name="get-data-collection-information"></a>Recevoir des informations sur la collecte de données

\]dbo \[.\[API GetDuration\] obtient la durée spécifiée par l’utilisateur, en secondes, pour la collecte de données :

* sortie de @duration int

Voici un exemple de script de rapport :

``` syntax
DECLARE @duration int
exec dbo.GetDuration @duration output
```

\]dbo \[.\[API GetInternal\] obtient l’intervalle d’un compteur de performance. Elle peut retourner la valeur NULL si le rapport actuel ne contient pas d’informations sur les compteurs de performances.

* sortie de @interval int

Voici un exemple de script de rapport :

``` syntax
DECLARE @interval int
exec dbo.GetInterval @interval output
```

### <a name="set-a-list-value-table"></a>Définir une table de valeurs de liste

Il n’existe aucune API pour la mise à jour des tables de valeurs de liste. Toutefois, vous pouvez accéder directement aux tables de valeurs de liste. à l’étape d’initialisation, une table temporaire correspondante est créée pour chaque valeur de liste.

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

s’il existe d’autres informations que vous souhaitez communiquer aux administrateurs système, vous pouvez écrire des journaux. S’il existe un journal pour un rapport particulier, une bannière jaune s’affiche dans l’en-tête du rapport. L’exemple suivant montre comment vous pouvez écrire un journal :

``` syntax
exec dbo.WriteSystemLog N'Any information you want to show to the system administrators , N Warning 
```

Le premier paramètre est le message que vous souhaitez afficher dans le journal. Le deuxième paramètre est le niveau de journalisation. L’entrée valide pour le deuxième paramètre peut être à **titre d’information**, d' **Avertissement**ou d' **erreur**.

### <a name="debug"></a>Journal de débogage

La console SPA peut s’exécuter en deux modes : Debug ou Release. Le mode de mise en sortie est la valeur par défaut et nettoie toutes les données brutes collectées une fois le rapport généré. Le mode débogage conserve toutes les données brutes dans le partage de fichiers et la base de données, afin que vous puissiez déboguer le script de rapport à l’avenir.

**Pour déboguer un script de rapport**

1.  Installez Microsoft SQL Server Management Studio (SSMS).

2.  Une fois SSMS lancé, connectez-vous à localhost\\SQLExpress. N’oubliez pas que vous devez utiliser localhost au lieu de. . Dans le cas contraire, vous ne pourrez peut-être pas démarrer le débogueur dans SQL Server.

3.  Exécutez le script suivant pour activer le mode débogage :

    ``` syntax
    USE SPADB
    UPdate dbo.Configurations
    SET Value = N'true'
    WHERE Name = N'Debugmode'
    ```

4.  Lancez la console SPA et exécutez l’Advisor Pack que vous souhaitez déboguer.

5.  Attendez que la tâche se termine. Si le rapport est généré avec succès, revenez à SSMS et recherchez la tâche la plus récente.

    ``` syntax
    select TOP 1 * FROM dbo.Tasks OrdER BY Id DESC
    ```

    Par exemple, la sortie peut être :

    Id | SessionId | AdvisoryPackageId | ReportStatusId | LastUpdatetime | ThresholdversionId
    :---: | :---: | :---: | :---: | :---: | :---:
    12 | 17 | 1 | 2 | 2011-05-11 05:35:24.387 | 1

6.  Vous pouvez exécuter le script suivant autant de fois que vous le souhaitez pour exécuter le script de rapport pour l’ID 12 :

    ``` syntax
    exec dbo.DebugReportScript 12
    ```

    **Remarque** Vous pouvez également appuyer sur F11 pour effectuer un pas à pas détaillé dans l’instruction précédente et déboguer.



Exécution de \[dbo\].\[DebugReportScript\] retourne plusieurs jeux de résultats, notamment :

1.  Messages Microsoft SQL Server et journaux Advisor Pack

2.  Résultats des règles

3.  Clés et valeurs de statistiques

4.  Valeurs uniques

5.  Toutes les tables de valeurs de liste

## <a name="best-practices"></a>Bonnes pratiques

### <a name="naming-convention-and-styles"></a>Convention d’affectation de noms et styles

|                                                                 Casse Pascal                                                                 |                       Casse mixte                        |             Majuscules             |
|-----------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|-----------------------------------|
| <ul><li>Noms dans ProvisionMetadata. Xml</li><li>les procédures stockées ;</li><li>Fonctions</li><li>Afficher les noms</li><li>Noms de tables temporaires</li></ul> | <ul><li>Noms de paramètre</li><li>Variables locales</li></ul> | À utiliser pour tous les mots clés réservés SQL |

### <a name="other-recommendations"></a>Autres recommandations

* Déplacez les éléments les plus logiques dans d’autres procédures stockées et fonctions définies par l’utilisateur.

* Rendez votre script principal aussi concis que possible à des fins de maintenance.

* Utilisez le nom complet de l’objet SQL.

* Traitez votre code SQL comme sensible à la casse.

* Ajoutez **Set NOcount** au début de chaque procédure stockée.

* Envisagez d’utiliser des tables temporaires pour transférer une grande quantité de données.

* Envisagez d’utiliser **Set XACT\_Abort on** pour mettre fin au processus si une erreur se produit.

* Toujours inclure le numéro de version principale dans le nom d’affichage du Pack Advisor.

## <a href="" id="bkmk-advancedtopics"></a>Rubriques avancées

### <a name="run-multiple-advisor-packs-simultaneously"></a>Exécuter plusieurs Advisor packs simultanément

SPA prend en charge l’exécution de plusieurs packs d’Advisor en même temps. Cela s’avère particulièrement utile lorsque vous souhaitez examiner en même temps les performances de Internet Information Services (IIS) et du système d’exploitation principal. De nombreux collecteurs de données qui sont utilisés par le pack d’aide IIS peuvent également être utilisés par le pack d’aide du système d’exploitation de base. Lorsque plusieurs packs d’Advisor s’exécutent sur le même ordinateur cible, SPA ne collecte pas les mêmes données à deux reprises.

L’exemple suivant montre le flux de travail pour l’exécution de deux packs Advisor.

![exécution de plusieurs Advisor packs](../media/server-performance-advisor/spa-dev-guide-multi-advisor-packs.png)

L’ensemble de collecteurs de données de fusion concerne uniquement la collecte du compteur de performance et des sources de données ETW. Les règles de fusion suivantes s’appliquent :

1. SPA prend la plus grande durée en tant que nouvelle durée.

2. En cas de conflits de fusion, les règles suivantes sont respectées :

   1. Prenez l’intervalle le plus petit comme nouvel intervalle.

   2. Prenez le super ensemble des compteurs de performances. Par exemple, avec le **processus (\*)\\% temps processeur** et **processus (\*)\\\*,\\processus (\*)\\** \\* retourne plus de données, le **processus (\*)\\% temps processeur** et **processus (\*)\\** \\* est supprimé de l’ensemble de collecteurs de données fusionnés.

### <a name="collect-dynamic-data"></a>Collecter des données dynamiques

SPA a besoin d’un ensemble de collecteurs de données définis au moment de la conception. Il n’est pas toujours possible de savoir quelles données sont nécessaires à la génération du rapport, car les données dynamiques et le chemin d’accès de la requête ne sont pas connus tant que leurs données dépendantes ne sont pas disponibles.

par exemple, si vous souhaitez afficher la liste de tous les noms conviviaux des cartes réseau, vous devez d’abord interroger WMI pour énumérer toutes les cartes réseau. Chaque objet WMI retourné possède un chemin d’accès de clé de Registre, où il stocke le nom convivial. Le chemin d’accès de la clé de Registre est inconnu au moment de la conception. Dans ce cas, nous avons besoin de la prise en charge des données dynamiques.

Pour énumérer toutes les cartes réseau, vous pouvez utiliser la requête WMI suivante à l’aide de Windows PowerShell :

``` syntax
Get-WmiObject -Namespace Root\Cimv2 -query "select PNPDeviceID FROM Win32_NetworkAdapter" | forEach-Object { Write-Output $_.PNPDeviceID }
```

Elle retourne une liste d’objets carte réseau. Chaque objet possède une propriété appelée **PNPDeviceID**, qui conserve un chemin d’accès de clé de registre relatif. Voici un exemple de sortie de la requête précédente :

``` syntax
ROOT\*ISatAP\0001
PCI\VEN_8086&DEV_4238&SUBSYS_11118086&REV_35\4&372A6B86&0&00E4
ROOT\*IPHTTPS\0000

```

Pour trouver la valeur **FriendlyName** , ouvrez l’éditeur du Registre et accédez au paramètre du registre en combinant **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\enum\\** avec chaque ligne de l’exemple précédent. , par exemple : **HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\enum\\ racine\\\*IPHTTPS\\0000**.

Pour traduire les étapes précédentes en métadonnées de configuration de SPA, ajoutez le script dans l’exemple de code suivant :

``` syntax
<advisorPack>
<dataSourceDefinition xmlns="https://microsoft.com/schemas/ServerPerformanceAdvisor/dc/2010">
 <dataCollectorSet >
<registryKeys>
 ?<registryKey>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\$(NetworkAdapter.PNPDeviceID)\FriendlyName</registryKey>
</registryKeys>
<managementpaths>
 ?<path name="NetworkAdapter">Root\Cimv2:select PNPDeviceID FROM Win32_NetworkAdapter</path>
</managementpaths>
```

Dans cet exemple, vous ajoutez d’abord une requête WMI sous managementpaths et définissez le nom de clé **NetworkAdapter**. Ensuite, vous ajoutez une clé de Registre et vous reportez-vous à **NetworkAdapter** à l’aide de la syntaxe **$ (NetworkAdapter. PNPDeviceID)** .

Le tableau suivant définit si un collecteur de données dans SPA prend en charge les données dynamiques et s’il peut être référencé par d’autres collecteurs de données :

Type de données | Prise en charge des données dynamiques | Peut être référencé
--- | :---: | :---:
clé de Registre | Oui | Oui
WMI | Oui | Oui
File | Oui | non
Compteur de performance | non | non
ETW | non | non

Pour un collecteur de données WMI, chaque objet WMI a de nombreux attributs attachés. Tout type d’objet WMI a toujours trois attributs : \_\_espace de noms, \_\_classe et \_\_RELpath.

Pour définir un collecteur de données qui est référencé par d’autres collecteurs de données, affectez l’attribut **Name** avec une clé unique dans le ProvisionMetadata. Xml. Cette clé est utilisée par les collecteurs de données dépendants pour générer des données dynamiques.

Voici un exemple de clé de Registre :

``` syntax
<registryKey  name="registry">HKEY_LOCAL_MACHINE </registryKey>
```

Et un exemple pour WMI :

``` syntax
<path name="wmi">Root\Cimv2:select PNPDeviceID FROM Win32_NetworkAdapter</path>
```

Pour définir un collecteur de données dépendantes, utilisez la syntaxe suivante : $ ( *{Name}* . *{attribut}* ).

*{Name}* et *{attribute}* sont des espaces réservés.

Lorsque SPA collecte des données à partir d’un serveur cible, il remplace de manière dynamique le modèle $ (\*.\*) par les données collectées réelles de son collecteur de données de référence (clé de Registre/WMI), par exemple :

``` syntax
<registryKey>HKEY_LOCAL_MACHINE\$(registry.key)\ </registryKey>
<registryKey  name="registry">HKEY_LOCAL_MACHINE\$(wmi.Relativeregistrypath)\ </registryKey>
<path name="wmi"> </path>
<file>$(wmi.FileName)</file>
```

**Remarque** SPA prend en charge une profondeur de référence illimitée, mais soyez conscient de la surcharge de performances si vous avez trop de niveaux. Assurez-vous qu’il n’existe aucune référence circulaire ou auto-référence qui n’est pas prise en charge.

### <a name="versioning-limitations"></a>limitations relatives au contrôle de version

SPA prend en charge la réinitialisation et les mises à jour de version mineures. Ces processus utilisent le même algorithme. Le processus consiste à actualiser tous les objets de base de données et les paramètres de seuil, tout en conservant les données existantes. Il peut s’agir d’une mise à niveau vers une version supérieure ou vers une version antérieure. Sélectionnez le Pack Advisor, puis cliquez sur **Réinitialiser** dans la boîte de dialogue **configurer les packs d’Advisor** dans Spa pour réinitialiser ou appliquer les mises à jour.

Cette fonctionnalité est principalement destinée à des mises à jour mineures. Vous ne pouvez pas modifier radicalement les éléments d’affichage de l’interface utilisateur. Si vous souhaitez apporter des modifications significatives, vous devez créer un autre conseiller. Vous devez inclure la version principale dans le nom du Pack Advisor.

Les limitations des modifications de version mineure sont que vous **ne pouvez pas** effectuer les opérations suivantes :

* Modifier le nom du schéma

* Modifier le type de données d’un groupe de valeurs unique ou les colonnes d’une table de valeurs de liste

* Ajouter ou supprimer des seuils

* Ajouter ou supprimer des règles

* Ajouter ou supprimer des conseils

* Ajouter ou supprimer des valeurs uniques

* Ajouter ou supprimer des valeurs de liste

* Ajouter ou supprimer une colonne de valeurs de liste

### <a href="" id="bkmk-tooltips"></a>Info-bulles

Presque tous les attributs de **Description** s’affichent sous la forme d’une info-bulle dans la console du Spa.

pour une table de valeurs de liste, une info-bulle basée sur les lignes peut être obtenue en ajoutant l’attribut suivant :

``` syntax
<listValue descriptionColumn="Description">
<column name="Name"/>
<column name="Description"/>
</listValue>
```

L’attribut **descriptionColumn** fait référence au nom de la colonne. Dans cet exemple, la colonne description ne s’affiche pas comme colonne physique. Toutefois, il s’affiche sous la forme d’une info-bulle lorsque vous placez le pointeur de la souris sur chaque ligne de la première colonne.

Nous recommandons que l’info-bulle affiche la source de données à l’utilisateur. Voici les formats permettant d’illustrer les sources de données :

Source de données | Format | Exemple
--- | --- | ---
WMI | WMI : &lt;WMIClass&gt;/champ &lt;&gt; | WMI : Win32_OperatingSystem/Caption
Compteur de performance | PerfCounter : &lt;CategoryName&gt;/&lt;nom_instance&gt; | PerfCounter : processus/% de temps processeur
registry | Registre : &lt;registerKey&gt; | Registre : Hklm\software\microsoft.<br>\\ASP.NET\\Rootver
Fichier de configuration | ConfigFile : &lt;FilePath&gt;\[; XPath : &lt;&gt;XPath \]<br>**Remarque**<br>XPath est facultatif et n’est valide que si le fichier est un fichier XML. | ConfigFile : windir%\\system32\\inetsrv\config\\applicationHost. config<br>XPath : configuration&frasl;System. webServer<br>&frasl;httpProtocol&frasl;@allowKeepAlive
ETW | ETW : &lt;fournisseur/&gt;(Mots clés) | ETW : trace du noyau Windows (processus, net)

### <a name="table-collation"></a>Classement de table

Lorsqu’un Advisor Pack devient plus complexe, vous pouvez créer vos propres tables variables ou tables temporaires pour stocker les résultats intermédiaires dans le script de rapport.

Le classement des colonnes de chaîne peut être problématique, car le classement de table que vous créez peut être différent de celui créé par l’infrastructure SPA. Si vous mettez en corrélation deux colonnes de chaîne dans des tables différentes, vous pouvez voir une erreur de classement. Pour éviter ce problème, vous devez toujours définir la chaîne pour un classement de colonne en tant que **SQL\_Latin1\_général\_CP1\_CI\_comme** lorsque vous définissez une table.

Voici comment définir une table de variables :

``` syntax
DECLARE @filesIO TABLE (
 Name nvarchar(500) COLLatE SQL_Latin1_General_CP1_CI_AS,
 AverageFileAccessvolume float,
 AverageFileAccessCount float,
 Filepath nvarchar(500) COLLatE SQL_Latin1_General_CP1_CI_AS
)
```

### <a name="collect-etw"></a>Collecter ETW

Voici comment définir ETW dans un fichier ProvisionMetadata. xml :

``` syntax
<dataSourceDefinition>
  <providers>
    <provider session="NT Kernel Logger" guid="{9E814AAD-3204-11D2-9A82-006008A86939}"/>
  </providers>
</dataSourceDefinition>
```

Les attributs de fournisseur suivants peuvent être utilisés pour collecter ETW :

Attribut | Tapez | Description
--- | --- | ---
GUID | GUID | GUID du fournisseur
session | chaîne | Nom de session ETW (facultatif, requis uniquement pour les événements de noyau)
keywordsany | Hexadécimal | N’importe quel mot clé (facultatif, pas de préfixe 0x)
keywordsAll | Hexadécimal | Tous les mots clés (facultatif)
propriétés | Hexadécimal | Propriétés (facultatif)
level | Hexadécimal | Niveau (facultatif)
Tampon | Entier | Taille de la mémoire tampon (facultatif)
flushtime | Entier | Temps de vidage (facultatif)
maxBuffer | Entier | Mémoire tampon maximale (facultatif)
minBuffer | Entier | Mémoire tampon minimale (facultatif)

Il existe deux tables de sortie comme indiqué ici.

**schéma de la table d’événements \#**

Nom de la colonne | Type de données SQL | Description
--- | --- | ---
SequenceID | Int non NULL | ID de séquence de corrélation
EventtypeId | Int non NULL | ID de type d’événement (reportez-vous à [dbo]. [ EventTypes])
ProcessId | BigInt non NULL | ID de processus
ThreadId | BigInt non NULL | ID de thread
timestamp | datetime2 non NULL | timestamp
Kerneltime | BigInt non NULL | Temps noyau
Usertime | BigInt non NULL | Heure de l’utilisateur

**\#le schéma de la table EventProperties**

Nom de la colonne | Type de données SQL | Description
--- | --- | ---
SequenceID | Int non NULL | ID de séquence de corrélation
Nom | Nvarchar(100) | Nom de la propriété
Value | Nvarchar(4000) | Value

### <a name="etw-schema"></a>Schéma ETW

Un schéma ETW peut être généré en exécutant tracerpt. exe sur le fichier. etl. Un fichier Schema. Man est généré. Étant donné que le format du fichier. etl dépend de l’ordinateur, le script suivant ne fonctionne que dans les cas suivants :

1.  Exécutez le script sur l’ordinateur sur lequel le fichier. etl correspondant est collecté.

2.  Ou exécutez le script sur un ordinateur sur lequel le système d’exploitation et les composants sont installés.

``` syntax
tracerpt *.etl -export
```

## <a name="glossary"></a>Glossaire


Les termes suivants sont utilisés dans ce document :

**Pack Advisor**

Un Advisor Pack est un ensemble de métadonnées et de scripts SQL qui traitent les journaux de performances collectés à partir du serveur cible. Le Pack Advisor génère ensuite des rapports à partir des données du journal de performances. Les métadonnées dans Advisor Pack définissent les données à collecter sur le serveur cible pour les mesures de performances. Les métadonnées définissent également l’ensemble de règles, les seuils et le format de rapport. Le plus souvent, un Advisor Pack est écrit spécifiquement pour un rôle serveur unique, par exemple, Internet Information Services (IIS).

**Console SPA**

La console SPA fait référence à SpaConsole. exe, qui est la partie centrale de Server Performance Advisor. SPA n’a pas besoin de s’exécuter sur le serveur cible que vous testez. La console du SPA contient toutes les interfaces utilisateur pour SPA, de la configuration du projet à l’exécution de l’analyse et de l’affichage des rapports. Par défaut, SPA est une application à deux niveaux. La console du SPA contient la couche d’interface utilisateur et une partie de la couche logique métier. La console SPA planifie et traite les demandes d’analyse des performances.

**Infrastructure SPA**

SPA contient deux parties principales : l’infrastructure et les packs Advisor. L’infrastructure SPA fournit toutes les interfaces utilisateur, le traitement du journal des performances, la configuration, la gestion des erreurs et les API de base de données, ainsi que les procédures de gestion.

**Projet SPA**

Un projet SPA est une base de données qui contient toutes les informations sur les serveurs cibles, les packs d’conseils et les rapports d’analyse des performances qui sont générés sur les serveurs cibles pour les packs d’Advisor. Vous pouvez comparer et afficher les graphiques d’historique et de tendance au sein du même projet SPA. L’utilisateur peut créer plusieurs projets. Les projets SPA sont indépendants les uns des autres et il n’y a pas de données partagées entre les projets.

**Serveur cible**

Le serveur cible est l’ordinateur physique ou la machine virtuelle qui exécute Windows Server avec certains rôles de serveur, tels que les services Internet (IIS).

**Session d’analyse des données**

Une session d’analyse des données est une analyse des performances sur un serveur cible spécifique. Une session d’analyse de données peut inclure plusieurs packs d’Advisor. Les ensembles de collecteurs de données de ces packs Advisor sont fusionnés dans un seul ensemble de collecteurs de données. Tous les journaux de performances d’une même session d’analyse de données sont collectés au cours de la même période. L’analyse des rapports générés par les packs d’aide en cours d’exécution dans la même session d’analyse des données peut aider les utilisateurs à comprendre la situation globale des performances et à identifier les causes des problèmes de performances.

**Suivi d' v nements pour Windows**

Le [suivi d’événements](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) pour Windows (ETW) est un système de suivi hautes performances, à faible charge et évolutif, fourni dans les systèmes d’exploitation Windows. Il fournit des fonctionnalités de profilage et de débogage, qui peuvent être utilisées pour dépanner un large éventail de scénarios. SPA utilise des événements ETW comme source de données pour générer les rapports de performances. Pour obtenir des informations générales sur ETW, consultez [améliorer le débogage et le réglage des performances avec ETW](https://msdn.microsoft.com/magazine/cc163437.aspx).

**Requête WMI**

Windows Management Instrumentation (WMI) est l’infrastructure de données et d’opérations de gestion dans les systèmes d’exploitation Windows. Vous pouvez écrire des scripts ou des applications WMI pour automatiser des tâches administratives sur des ordinateurs distants. WMI fournit également des données de gestion à d’autres parties du système d’exploitation et aux produits. SPA utilise les informations de classe WMI et les points de données comme sources pour générer des rapports de performances.

**Compteurs de performance**

Les compteurs de performances sont utilisés pour fournir des informations sur la façon dont le système d’exploitation, une application, un service ou un pilote fonctionne. Les données du compteur de performances peuvent aider à déterminer les goulots d’étranglement du système et à ajuster les performances du système et des applications. Le système d’exploitation, le réseau et les périphériques fournissent des données de compteur qu’une application peut utiliser pour fournir aux utilisateurs une vue graphique de la performance du système. SPA utilise les informations de compteur de performance et les points de données comme sources pour générer des rapports de performances.

**Journaux et alertes de performances**

Journaux et alertes de performance (PLA) est un service intégré dans le système d’exploitation Windows. Il est conçu pour collecter les journaux et les traces de performance, et il génère également des alertes de performances lorsque certains déclencheurs sont satisfaits. PLA peut être utilisé pour collecter les compteurs de performances, le suivi d’événements pour Windows (ETW), les requêtes WMI, les clés de Registre et les fichiers de configuration. PLA prend également en charge la collecte de données distantes via des appels de procédure distante (RPC). L’utilisateur définit un ensemble de collecteurs de données, qui comprend des informations sur les données à collecter, la fréquence de collecte des données, la durée de collecte des données, les filtres et un emplacement pour l’enregistrement des fichiers de résultats. SPA utilise PLA pour collecter toutes les données de performances des serveurs cibles.

**Rapport unique**

Un rapport unique est le rapport SPA généré à partir d’une session d’analyse de données pour un pack Advisor sur un serveur cible unique. Il peut contenir des notifications et diverses sections de données.

**Rapport côte à côte**

Un rapport côte à côte est un rapport SPA qui compare deux rapports uniques pour le même Advisor Pack. Les deux rapports peuvent être générés à partir de différents serveurs cibles ou à partir d’une analyse de performances distincte exécutée sur le même serveur cible. Le rapport côte à côte crée la possibilité de comparer deux rapports pour aider les utilisateurs à identifier des comportements ou des paramètres anormaux dans l’un des rapports. Un rapport côte à côte contient des notifications et différentes sections de données. Dans chaque section, les données des deux rapports sont répertoriées côte à côte.

**Graphique de tendances**

Un graphique de tendances est le rapport SPA utilisé pour étudier les tendances répétitives des problèmes de performances. De nombreux problèmes de performances répétitifs sont causés par les modifications planifiées de la charge du serveur sur le serveur ou à partir d’ordinateurs clients, ce qui peut se produire quotidiennement ou toutes les semaines. SPA fournit un graphique de tendances de 24 heures et un graphique de tendances de 7 jours pour identifier ces problèmes.

L’utilisateur peut choisir une ou plusieurs séries de données à la fois, qui est une valeur numérique dans le rapport unique, par exemple l' **utilisation totale de l’UC moyenne**. plus précisément, une valeur numérique est une valeur scalaire d’un serveur unique qui est générée par un point d’accès unique à une instance d’heure donnée. SPA regroupe ces valeurs en 24 groupes, un pour chaque heure de la journée (sept pour un rapport de 7 jours, un pour chaque jour de la semaine). SPA calcule la moyenne, la valeur minimale, la valeur maximale et les écarts types pour chaque groupe.

**Graphique historique**

Un graphique d’historique est le rapport SPA utilisé pour afficher les modifications de certaines valeurs numériques dans des rapports uniques pour un serveur donné et une paire Advisor Pack dans le temps. L’utilisateur peut choisir plusieurs séries de données et les afficher ensemble dans le graphique historique pour comprendre la corrélation entre différentes séries de données.

**Série de données**

Une série de données est une donnée numérique qui est collectée à partir de la même source de données sur une période donnée. La même source signifie que les données doivent provenir du même serveur cible, par exemple la longueur moyenne de la file d’attente des demandes pour IIS sur un serveur.

**Règles**

Les règles sont des combinaisons de logique, des seuils et des descriptions. Ils représentent un problème de performances potentiel. Chaque pack Advisor contient plusieurs règles. Chaque règle est déclenchée par un processus de génération de rapports. Une règle applique la logique et les seuils aux données dans un rapport unique. Si les critères sont satisfaits, une notification d’avertissement est générée. Si ce n’est pas le cas, la notification est définie sur l’état **OK** . Si la règle ne s’applique pas, la notification est définie sur l’état non applicable (**na**).

**Notifications**

Une notification correspond aux informations qu’une règle affiche aux utilisateurs. Il comprend l’état de la règle (**OK**, **na**ou **Warning**), le nom de la règle et les recommandations possibles pour résoudre les problèmes de performances.
