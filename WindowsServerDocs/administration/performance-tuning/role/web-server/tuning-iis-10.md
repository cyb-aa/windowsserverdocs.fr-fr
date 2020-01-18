---
title: Paramétrage d’IIS 10,0
description: Réglage des performances recommmendations pour les serveurs Web IIS 10,0 sur Windows Server 16
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: DavSo; Ericam; YaShi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 5658a101371cf3b865dec04ac76716b536792602
ms.sourcegitcommit: c5709021aa98abd075d7a8f912d4fd2263db8803
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/18/2020
ms.locfileid: "76265701"
---
# <a name="tuning-iis-100"></a>Paramétrage d’IIS 10,0

Internet Information Services (IIS) 10,0 est inclus avec Windows Server 2016. Il utilise un modèle de processus similaire à celui d’IIS 8,5 et IIS 7,0. Un pilote Web en mode noyau (http. sys) reçoit et achemine les requêtes HTTP et satisfait les demandes provenant de son cache de réponse. Les processus de travail s’inscrivent pour les sous-espaces d’URL et http. sys achemine la requête vers le processus approprié (ou l’ensemble de processus pour les pools d’applications).

HTTP. sys est responsable de la gestion des connexions et de la gestion des demandes. La demande peut être traitée à partir du cache HTTP. sys ou passée à un processus de travail pour une gestion ultérieure. Plusieurs processus de travail peuvent être configurés, ce qui permet une isolation à moindre coût. Pour plus d’informations sur le fonctionnement de la gestion des demandes, consultez la figure suivante :

![traitement des demandes dans IIS 10,0](../../media/perftune-guide-iis-request-handling.png)

HTTP. sys comprend un cache de réponse. Lorsqu’une demande correspond à une entrée du cache de réponse, HTTP. sys envoie la réponse du cache directement à partir du mode noyau. Certaines plateformes d’application Web, telles que ASP.NET, fournissent des mécanismes pour permettre la mise en cache de tout contenu dynamique dans le cache en mode noyau. Le gestionnaire de fichiers statiques dans IIS 10,0 met automatiquement en cache les fichiers fréquemment demandés dans http. sys.

Étant donné qu’un serveur Web dispose de composants en mode noyau et en mode utilisateur, les deux composants doivent être réglés pour des performances optimales. Par conséquent, le paramétrage d’IIS 10,0 pour une charge de travail spécifique comprend la configuration des éléments suivants :

-   HTTP. sys et cache en mode noyau associé

-   Processus de travail et IIS en mode utilisateur, y compris la configuration du pool d’applications

-   Certains paramètres de paramétrage qui affectent les performances

Les sections suivantes expliquent comment configurer les aspects du mode noyau et du mode utilisateur d’IIS 10,0.

## <a name="kernel-mode-settings"></a>Paramètres du mode noyau

Les paramètres HTTP. sys liés aux performances se répartissent en deux grandes catégories : la gestion du cache et la gestion des connexions et des demandes. Tous les paramètres de Registre sont stockés sous l’entrée de Registre suivante :

``` syntax
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters
```

**Notez**  si le service http est déjà en cours d’exécution, vous devez le redémarrer pour que les modifications prennent effet.

Â 

## <a name="cache-management-settings"></a>Paramètres de gestion du cache

L’un des avantages offerts par HTTP. sys est un cache en mode noyau. Si la réponse est dans le cache en mode noyau, vous pouvez satisfaire une requête HTTP entièrement à partir du mode noyau, ce qui réduit considérablement le coût de traitement de la demande par le processeur. Toutefois, le cache en mode noyau d’IIS 10,0 est basé sur la mémoire physique et le coût d’une entrée est la mémoire qu’elle occupe.

Une entrée dans le cache est utile uniquement lorsqu’elle est utilisée. Toutefois, l’entrée consomme toujours la mémoire physique, que l’entrée soit ou non utilisée. Vous devez évaluer l’utilité d’un élément dans le cache (le gain de capacité à être utilisé à partir du cache) et son coût (la mémoire physique occupée) sur la durée de vie de l’entrée en considérant les ressources disponibles (UC et mémoire physique) et la charge de travail. exigences. HTTP. sys tente de conserver uniquement les éléments utiles et activement accessibles dans le cache, mais vous pouvez augmenter les performances du serveur Web en réglant le cache HTTP. sys pour des charges de travail spécifiques.

Voici quelques paramètres utiles pour le cache en mode noyau HTTP. sys :

-   **UriEnableCache** Valeur par défaut : 1

    Une valeur différente de zéro active la réponse en mode noyau et la mise en cache de fragments. Pour la plupart des charges de travail, le cache doit rester activé. Envisagez de désactiver le cache si vous prévoyez une réponse très faible et une mise en cache de fragments.

-   **UriMaxCacheMegabyteCount** Valeur par défaut : 0

    Valeur différente de zéro qui spécifie la mémoire maximale disponible pour le cache en mode noyau. La valeur par défaut, 0, permet au système d’ajuster automatiquement la quantité de mémoire disponible pour le cache.

    **Remarque** La spécification de la taille définit uniquement la valeur maximale et le système risque de ne pas laisser le cache atteindre la taille maximale définie.

    Â 

-   **UriMaxUriBytes** Valeur par défaut : 262144 octets (256 Ko)

    Taille maximale d’une entrée dans le cache en mode noyau. Les réponses ou les fragments supérieurs à ce nombre ne sont pas mis en cache. Si vous disposez de suffisamment de mémoire, envisagez d’accentuer la limite. Si la mémoire est limitée et que les entrées de grande taille sont volumineuses, il peut être utile de réduire la limite.

-   **UriScavengerPeriod** Valeur par défaut : 120 secondes

    Le cache HTTP. sys est analysé régulièrement par un nettoyage, et les entrées qui ne sont pas accessibles entre les analyses du nettoyage sont supprimées. La définition de la période de nettoyage sur une valeur élevée réduit le nombre d’analyses du nettoyage. Toutefois, l’utilisation de la mémoire cache peut augmenter, car les entrées les plus anciennes et les moins fréquemment sollicitées peuvent rester dans le cache. Le fait de définir une période trop faible entraîne des analyses de nettoyage plus fréquentes et peut entraîner un trop grand nombre de vidages et d’évolutions du cache.

## <a name="request-and-connection-management-settings"></a>Paramètres de gestion des demandes et des connexions

Dans Windows Server 2016, HTTP. sys gère automatiquement les connexions. Les paramètres de registre suivants ne sont plus utilisés :

-   **MaxConnections**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\MaxConnections
    ```

-   **IdleConnectionsHighMark**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\IdleConnectionsHighMark
    ```

-   **IdleConnectionsLowMark**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\IdleConnectionsLowMark
    ```

-   **IdleListTrimmerPeriod**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\IdleListTrimmerPeriod
    ```

-   **RequestBufferLookasideDepth**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\RequestBufferLookasideDepth
    ```

-   **InternalRequestLookasideDepth**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\InternalRequestLookasideDepth
    ```


## <a name="user-mode-settings"></a>Paramètres du mode utilisateur

Les paramètres de cette section affectent le comportement du processus de travail IISÂ 10,0. La plupart de ces paramètres se trouvent dans le fichier de configuration XML suivant :

% SystemRoot%\\system32\\inetsrv\\config\\applicationHost. config

Utilisez Appcmd. exe, la console de gestion IIS 10,0, les applets de commande PowerShell WebAdministration ou IISAdministration pour les modifier. La plupart des paramètres sont détectés automatiquement et ne nécessitent pas de redémarrage des processus de travail IIS 10,0 ou du serveur d’applications Web. Pour plus d’informations sur le fichier applicationHost. config, consultez [Présentation de applicationHost. config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig).


## <a name="ideal-cpu-setting-for-numa-hardware"></a>Paramètre d’UC idéal pour le matériel NUMA

À partir de Windows 2016, IIS 10,0 prend en charge l’affectation automatique de l’UC idéale pour ses threads de pool de threads afin d’améliorer les performances et l’évolutivité sur le matériel NUMA. Cette fonctionnalité est activée par défaut et peut être configurée à l’aide de la clé de Registre suivante :

``` syntax
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\InetInfo\Parameters\ThreadPoolUseIdealCpu
```

Lorsque cette fonctionnalité est activée, le gestionnaire de threads IIS fait de son mieux pour distribuer uniformément les threads de pool de threads IIS sur tous les processeurs de tous les nœuds NUMA en fonction de leurs charges actuelles. En général, il est recommandé de conserver ce paramètre par défaut inchangé pour le matériel NUMA.

**Notez**  le paramètre d’UC idéal est différent des paramètres d’attribution de NŒUDs NUMA du processus de travail (NumaNodeAssignment et numaNodeAffinityMode) introduits dans les [paramètres de processeur d’un pool d’applications](https://www.iis.net/configreference/system.applicationhost/applicationpools/add/cpu). Le paramètre d’UC idéal affecte la manière dont les services IIS distribuent ses threads de pool de threads, tandis que les paramètres d’attribution de nœud NUMA du processus de travail déterminent le nœud NUMA Démarré par un processus de travail.

## <a name="user-mode-cache-behavior-settings"></a>Paramètres de comportement du cache du mode utilisateur

Cette section décrit les paramètres qui affectent le comportement de mise en cache dans IISÂ 10,0. Le cache du mode utilisateur est implémenté en tant que module qui écoute les événements de mise en cache globaux déclenchés par le pipeline intégré. Pour désactiver complètement le cache du mode utilisateur, supprimez le module FileCacheModule (cachfile. dll) de la liste des modules installés dans la section de configuration System. webServer/globalModules de applicationHost. config.

**System. webServer/mise en cache**

|Attribut|Description|Par défaut|
|--- |--- |--- |
|Enabled|Désactive le cache IIS en mode utilisateur lorsqu’il est défini sur **false**. Lorsque le taux d’accès au cache est très faible, vous pouvez désactiver complètement le cache pour éviter la surcharge associée au chemin d’accès au code du cache. La désactivation du cache du mode utilisateur ne désactive pas le cache en mode noyau.|Vrai|
|enableKernelCache|Désactive le cache en mode noyau lorsqu’il est défini sur **false**.|Vrai|
|maxCacheSize|Limite la taille de cache du mode utilisateur IIS à la taille spécifiée en mégaoctets. IIS ajuste la valeur par défaut en fonction de la mémoire disponible. Choisissez soigneusement la valeur en fonction de la taille de l’ensemble de fichiers fréquemment utilisés par rapport à la quantité de RAM ou de l’espace d’adressage du processus IIS.|0|
|maxResponseSize|Met en cache les fichiers jusqu’à la taille spécifiée. La valeur réelle dépend du nombre et de la taille des fichiers les plus volumineux dans le jeu de données par rapport à la quantité de mémoire disponible. La mise en cache de fichiers volumineux et fréquemment demandés peut réduire l’utilisation du processeur, l’accès au disque et les latences associées.|262144|

## <a name="compression-behavior-settings"></a>Paramètres de comportement de compression

IIS à partir de 7,0 compresse le contenu statique par défaut. En outre, la compression du contenu dynamique est activée par défaut lors de l’installation de DynamicCompressionModule. La compression réduit l’utilisation de la bande passante, mais augmente l’utilisation du processeur. Le contenu compressé est mis en cache dans le cache en mode noyau, si possible. À partir de 8,5, IIS permet de contrôler indépendamment la compression pour du contenu statique et dynamique. Le contenu statique fait généralement référence à du contenu qui ne change pas, tel que des fichiers GIF ou HTM. Le contenu dynamique est généralement généré par des scripts ou du code sur le serveur, c’est-à-dire des pages ASP.NET. Vous pouvez personnaliser la classification d’une extension particulière comme statique ou dynamique.

Pour désactiver complètement la compression, supprimez StaticCompressionModule et DynamicCompressionModule de la liste des modules dans la section System. webServer/globalModules de applicationHost. config.

**System. webServer/httpCompression**

|Attribut|Description|Par défaut|
|--- |--- |--- |
|staticCompression-EnableCpuUsage<br><br>staticCompression-DisableCpuUsage<br><br>dynamicCompression-EnableCpuUsage<br><br>dynamicCompression-DisableCpuUsage|Active ou désactive la compression si l’utilisation du processeur en pourcentage passe au-dessus ou au-dessous des limites spécifiées.<br><br>Depuis IIS 7,0, la compression est automatiquement désactivée si l’UC à état stable augmente au-dessus du seuil de désactivation. La compression est activée si le processeur descend sous le seuil d’activation.|50, 100, 50 et 90 respectivement|
|Répertoire|Spécifie le répertoire dans lequel les versions compressées des fichiers statiques sont temporairement stockées et mises en cache. Envisagez de déplacer ce répertoire sur le lecteur système si vous y accédez fréquemment.|Fichiers compressés temporaires%SystemDrive%\inetpub\temp\IIS|
|doDiskSpaceLimiting|Spécifie s’il existe une limite pour la quantité d’espace disque que tous les fichiers compressés peuvent occuper. Les fichiers compressés sont stockés dans le répertoire de compression qui est spécifié par l’attribut **Directory** .|Vrai|
|maxDiskSpaceUsage|Spécifie le nombre d’octets d’espace disque que les fichiers compressés peuvent occuper dans le répertoire de compression.<br><br>Ce paramètre devra peut-être être augmenté si la taille totale de tout le contenu compressé est trop importante.|100 Mo|

**System. webServer/élément urlCompression**

|Attribut|Description|Par défaut|
|--- |--- |--- |
|doStaticCompression|Spécifie si le contenu statique est compressé.|Vrai|
|doDynamicCompression|Spécifie si le contenu dynamique est compressé.|Vrai|

**Remarque** Pour les serveurs qui exécutent IIS 10,0 et dont l’utilisation moyenne du processeur est faible, envisagez d’activer la compression pour le contenu dynamique, en particulier si les réponses sont volumineuses. Cela doit d’abord être effectué dans un environnement de test pour évaluer l’impact de l’utilisation du processeur à partir de la ligne de base.


### <a name="tuning-the-default-document-list"></a>Paramétrage de la liste de documents par défaut

Le module de document par défaut gère les requêtes HTTP pour la racine d’un répertoire et les convertit en demandes pour un fichier spécifique, par exemple default. htm ou index. htm. En moyenne, aroundÂ 25% de toutes les demandes sur Internet passent par le chemin d’accès au document par défaut. Cela varie considérablement pour les sites individuels. Quand une requête HTTP ne spécifie pas de nom de fichier, le module de document par défaut recherche dans la liste des documents par défaut autorisés pour chaque nom dans le système de fichiers. Cela peut nuire aux performances, en particulier si vous atteignez le contenu, vous devez effectuer un aller-retour réseau ou toucher un disque.

Vous pouvez éviter la surcharge en désactivant de manière sélective les documents par défaut et en réduisant ou en ordonnant la liste des documents. Pour les sites Web qui utilisent un document par défaut, vous devez réduire la liste aux types de documents par défaut qui sont utilisés. En outre, classez la liste pour qu’elle commence par le nom de fichier de document par défaut le plus fréquemment utilisé.

Vous pouvez définir de manière sélective le comportement par défaut du document sur des URL particulières en personnalisant la configuration à l’intérieur d’une balise d’emplacement dans applicationHost. config ou en insérant un fichier Web. config directement dans le répertoire de contenu. Cela permet une approche hybride, qui permet aux documents par défaut uniquement lorsqu’ils sont nécessaires et définit la liste sur le nom de fichier correct pour chaque URL.

Pour désactiver complètement les documents par défaut, supprimez DefaultDocumentModule de la liste des modules dans la section System. webServer/globalModules de applicationHost. config.

**System. webServer/defaultDocument**

|Attribut|Description|Par défaut|
|--- |--- |--- |
|enabled|Spécifie que les documents par défaut sont activés.|Vrai|
|élément&gt; &lt;fichiers|Spécifie les noms de fichiers qui sont configurés en tant que documents par défaut.|La liste par défaut est default. htm, default. asp, index. htm, index. html, iisstart. htm et default. aspx.|

## <a name="central-binary-logging"></a>Journalisation centrale binaire

Lorsque la session serveur comporte de nombreux groupes d’URL, le processus de création de centaines de fichiers journaux mis en forme pour des groupes d’URL individuels et l’écriture des données du journal sur un disque peut rapidement consommer des ressources processeur et mémoire précieuses, créant ainsi des performances et problèmes d’extensibilité. La journalisation binaire centralisée réduit la quantité de ressources système utilisées pour la journalisation, tout en fournissant en même temps des données de journal détaillées pour les organisations qui en ont besoin. L’analyse des journaux au format binaire nécessite un outil de retraitement.

Vous pouvez activer la journalisation binaire centrale en affectant à l’attribut centralLogFileMode la valeur CentralBinary et en affectant à l’attribut **Enabled** la **valeur true**. Envisagez de déplacer l’emplacement du fichier journal central en dehors de la partition système et sur un lecteur de journalisation dédié afin d’éviter les conflits entre les activités du système et les activités de journalisation.

**System. applicationHost/log**

|Attribut|Description|Par défaut|
|--- |--- |--- |
|centralLogFileMode|Spécifie le mode de journalisation d’un serveur. Remplacez cette valeur par CentralBinary pour activer la journalisation binaire centrale.|Site|

**System. applicationHost/log/centralBinaryLogFile**

|Attribut|Description|Par défaut|
|--- |--- |--- |
|enabled|Spécifie si la journalisation binaire centrale est activée.|Faux|
|Répertoire|Spécifie le répertoire dans lequel les entrées de journal sont écrites.|%SystemDrive%\inetpub\logs\LogFiles|


## <a name="application-and-site-tunings"></a>Paramétrages des applications et des sites

Les paramètres suivants sont liés aux réglages du pool d’applications et du site.

**System. applicationHost/applicationPools/applicationPoolDefaults**

|Attribut|Description|Par défaut|
|--- |--- |--- |
|queueLength|Indique à HTTP. sys combien de demandes sont mises en file d’attente pour un pool d’applications avant le rejet des demandes ultérieures. Lorsque la valeur de cette propriété est dépassée, IIS rejette les demandes suivantes avec une erreur 503.<br><br>Envisagez d’augmenter cela pour les applications qui communiquent avec les magasins de données principaux à latence élevée si des erreurs 503 sont observées.|1000|
|enable32BitAppOnWin64|Si la valeur est true, permet à une application 32 bits de s’exécuter sur un ordinateur doté d’un processeur 64 bits.<br><br>Envisagez d’activer le mode 32 bits si la consommation de mémoire est un problème. Étant donné que les tailles des pointeurs et des instructions sont plus petites, les applications 32 bits utilisent moins de mémoire que les applications 64 bits. L’inconvénient de l’exécution d’applications 32 bits sur un ordinateur 64 bits est que l’espace d’adressage en mode utilisateur est limité à 4 Go.|Faux|

**System. applicationHost/sites/VirtualDirectoryDefault**

|Attribut|Description|Par défaut|
|--- |--- |--- |
|allowSubDirConfig|Spécifie si IIS recherche les fichiers Web. config dans les répertoires de contenu inférieurs au niveau actuel (true) ou ne recherche pas les fichiers Web. config dans les répertoires de contenu inférieurs au niveau actuel (false). En imposant une limitation simple, qui autorise uniquement la configuration dans les répertoires virtuels, IISÂ 10,0 peut savoir que, sauf si **/&lt;nom&gt;. htm** est un répertoire virtuel, il ne doit pas Rechercher un fichier de configuration. Ignorer les opérations de fichiers supplémentaires peut améliorer considérablement les performances des sites Web qui ont un très grand nombre de contenus statiques ayant un accès aléatoire.|Vrai|

## <a name="managing-iis-100-modules"></a>Gestion des modules IIS 10,0

IIS 10,0 a été pris en compte dans plusieurs modules extensibles par l’utilisateur pour prendre en charge une structure modulaire. Cette factorisation a un coût réduit. Pour chaque module, le pipeline intégré doit appeler le module pour chaque événement qui est pertinent pour le module. Cela se produit même si le module doit effectuer des tâches. Vous pouvez économiser de la mémoire et des cycles de l’UC en supprimant tous les modules qui ne sont pas pertinents pour un site Web particulier.

Un serveur Web réglé pour des fichiers statiques simples peut inclure uniquement les cinq modules suivants : UriCacheModule, HttpCacheModule, StaticFileModule, AnonymousAuthenticationModule et HttpLoggingModule.

Pour supprimer des modules de applicationHost. config, supprimez toutes les références au module dans les sections System. webServer/Handlers et System. webServer/modules, en plus de la suppression de la déclaration de module dans System. webServer/globalModules.

## <a name="classic-asp-settings"></a>Paramètres ASP classiques

Le coût majeur du traitement d’une demande ASP classique comprend l’initialisation d’un moteur de script, la compilation du script ASP demandé dans un modèle ASP et l’exécution du modèle sur le moteur de script. Alors que le coût d’exécution du modèle dépend de la complexité du script ASP demandé, le module ASP classique d’IIS peut mettre en cache les moteurs de script en mémoire et les modèles de cache dans la mémoire et le disque (uniquement en cas de dépassement du cache des modèles en mémoire) pour améliorer les performances dans Scénarios liés au processeur.

Les paramètres suivants sont utilisés pour configurer le cache de modèles ASP Classic et le cache du moteur de script, et ils n’affectent pas les paramètres ASP.NET.

**System. webServer/ASP/cache**

|Attribut|Description|Par défaut|
|--- |--- |--- |
|diskTemplateCacheDirectory|Nom du répertoire utilisé par ASP pour stocker les modèles compilés en cas de dépassement du cache en mémoire.<br><br>Recommandation : définissez sur un répertoire qui n’est pas très utilisé, par exemple, un lecteur qui n’est pas partagé avec le système d’exploitation, le journal IIS ou tout autre contenu fréquemment utilisé.|Modèles compilés%SystemDrive%\inetpub\temp\ASP|
|maxDiskTemplateCacheFiles|Spécifie le nombre maximal de modèles ASP compilés qui peuvent être mis en cache sur le disque.<br><br>Recommandation : définissez sur la valeur maximale de 0x7FFFFFFF.|2000|
|scriptFileCacheSize|Cet attribut spécifie le nombre maximal de modèles ASP compilés qui peuvent être mis en cache en mémoire.<br><br>Recommandation : définissez sur au moins autant que le nombre de scripts ASP fréquemment demandés servis par un pool d’applications. Si possible, affectez la valeur autant de modèles ASP que la limite de mémoire l’autorise.|500|
|scriptEngineCacheMax|Spécifie le nombre maximal de moteurs de script qui doivent rester en mémoire cache.<br><br>Recommandation : définissez sur au moins autant que le nombre de scripts ASP fréquemment demandés servis par un pool d’applications. Si possible, définissez sur autant de moteurs de script que la limite de mémoire le permet.|250|

**System. webServer/ASP/Limits**

|Attribut|Description|Par défaut|
|--- |--- |--- |
|processorThreadMax|Spécifie le nombre maximal de threads de travail par processeur qu’ASP peut créer. Augmentez la valeur si le paramètre actuel est insuffisant pour gérer la charge, ce qui peut provoquer des erreurs lors de la fourniture des demandes ou la mise sous-utilisation des ressources du processeur.|25|

**System. webServer/ASP/ComPlus**

|Attribut|Description|Par défaut|
|--- |--- |--- |
|executeInMta|Affectez la valeur **true** si des erreurs ou des échecs sont détectés pendant que les services IIS servent du contenu ASP. Cela peut se produire, par exemple, lors de l’hébergement de plusieurs sites isolés dans lesquels chaque site s’exécute sous son propre processus de travail. Les erreurs sont généralement signalées à partir de COM+ dans le observateur d’événements. Ce paramètre active le modèle multithread cloisonné dans ASP.|Faux|


## <a name="aspnet-concurrency-setting"></a>Paramètre de concurrence ASP.NET

### <a name="aspnet-35"></a>ASP.NET 3.5
Par défaut, ASP.NET limite la concurrence des demandes pour réduire la consommation de mémoire stable sur le serveur. Des applications à concurrence élevée peuvent avoir à ajuster certains paramètres pour améliorer les performances globales. Vous pouvez modifier ce paramètre dans le fichier Aspnet. config :

``` syntax
<system.web>
  <applicationPool maxConcurrentRequestsPerCPU="5000"/>
</system.web>
```

Le paramètre suivant est utile pour utiliser pleinement des ressources sur un système :

-   **maxConcurrentRequestPerCpu** Valeur par défaut : 5000

    Ce paramètre limite le nombre maximal de demandes ASP.NET exécutées simultanément sur un système. La valeur par défaut est conservatrice pour réduire la consommation de mémoire des applications ASP.NET. Envisagez d’accentuer cette limite sur les systèmes qui exécutent des applications qui effectuent des opérations d’e/s longues et synchrones. Dans le cas contraire, les utilisateurs peuvent rencontrer une latence élevée en raison de la mise en file d’attente ou des échecs de demande en raison du dépassement des limites de file d’attente sous une charge élevée lorsque le paramètre par défaut est utilisé.

### <a name="aspnet-46"></a>ASP.NET 4.6
Outre le paramètre maxConcurrentRequestPerCpu, ASP.NET 4,7 fournit également des paramètres pour améliorer les performances dans les applications qui s’appuient fortement sur une opération asynchrone. Le paramètre peut être modifié dans le fichier Aspnet. config.

``` syntax
<system.web>
  <applicationPool percentCpuLimit="90" percentCpuLimitMinActiveRequestPerCpu="100"/>
</system.web>
```

-   **percentCpuLimit** Valeur par défaut : 90 la demande asynchrone rencontre des problèmes d’extensibilité lorsqu’une charge énorme (au-delà des capacités matérielles) est placée sur un tel scénario. Le problème est dû à la nature de l’allocation sur les scénarios asynchrones. Dans ces conditions, l’allocation se produit au démarrage de l’opération asynchrone et est consommée à la fin de celle-ci. À ce moment-là, itâs très possible, les objets ont été déplacés vers la génération 1 ou 2 par GC. Dans ce cas, l’augmentation de la charge affiche l’augmentation de la demande par seconde (RPS) jusqu’à un point. Une fois que nous passons ce point, le temps passé dans GC commencera à devenir un problème et le RPS démarrera, avec un effet de mise à l’échelle négatif. Pour résoudre le problème, lorsque l’utilisation du processeur dépasse le paramètre percentCpuLimit, les demandes sont envoyées à la file d’attente Native ASP.NET.
-   **percentCpuLimitMinActiveRequestPerCpu** Valeur par défaut : la limitation de l’UC 100 (paramètre percentCpuLimit) n’est pas basée sur le nombre de requêtes, mais sur leur coût. Par conséquent, il peut y avoir seulement quelques requêtes gourmandes en ressources processeur qui provoquent une sauvegarde dans la file d’attente native, sans pour autant vider les demandes entrantes. Pour résoudre ce problme, percentCpuLimitMinActiveRequestPerCpu peut être utilisé pour s’assurer qu’un nombre minimal de demandes est traité avant que la limitation ne démarre.

## <a name="worker-process-and-recycling-options"></a>Options de processus de travail et de recyclage

Vous pouvez configurer des options pour le recyclage des processus de travail IIS et fournir des solutions pratiques à des situations ou événements aigus sans nécessiter d’intervention ni réinitialiser un service ou un ordinateur. Ces situations et ces événements incluent des fuites de mémoire, une charge de mémoire accrue ou des processus de travail inactifs ou inefficaces. Dans des conditions ordinaires, les options de recyclage ne sont peut-être pas nécessaires et le recyclage peut être désactivé ou le système peut être configuré pour se recycler très rarement.

Vous pouvez activer le recyclage de processus pour une application particulière en ajoutant des attributs à l’élément **recyclant/PeriodicRestart** . L’événement de recyclage peut être déclenché par plusieurs événements, notamment l’utilisation de la mémoire, un nombre fixe de demandes et une durée fixe. Lorsqu’un processus de travail est recyclé, les demandes mises en file d’attente et en cours d’exécution sont vidées, et un nouveau processus est démarré simultanément pour traiter de nouvelles demandes. L’élément **recyclant/PeriodicRestart** est par application, ce qui signifie que chaque attribut du tableau suivant est partitionné par application.

**System. applicationHost/applicationPools/ApplicationPoolDefaults/Recycling/periodicRestart**

|Attribut|Description|Par défaut|
|--- |--- |--- |
|memory|Activez le recyclage de processus si la consommation de mémoire virtuelle dépasse la limite spécifiée en kilo-octets. Il s’agit d’un paramètre utile pour les ordinateurs 32 bits qui ont un espace d’adressage de petite taille de 2 Go. Cela peut aider à éviter les demandes ayant échoué en raison d’erreurs de mémoire insuffisante.|0|
|privateMemory|Activez le recyclage de processus si les allocations de mémoire privée dépassent une limite spécifiée en kilo-octets.|0|
|requests|Activez le recyclage de processus après un certain nombre de demandes.|0|
|heure|Activez le recyclage de processus après une période spécifiée.|29:00:00|


## <a name="dynamic-worker-process-page-out-tuning"></a>Paramétrage de la sortie de page du processus Worker dynamique

À compter de Windows Server 2012 R2, IIS offre la possibilité de configurer le processus de travail de façon à ce qu’il s’interrompe une fois qu’il a été inactif pendant un certain temps (en plus de l’option Terminate, qui existait depuis IIS 7).

L’objectif principal des fonctionnalités d’arrêt du processus de travail inactif et de fin du processus de travail inactif consiste à préserver l’utilisation de la mémoire sur le serveur, car un site peut consommer beaucoup de mémoire même s’il est assis, à l’écoute. En fonction de la technologie utilisée sur le site (contenu statique et ASP.NET par rapport à d’autres frameworks), la mémoire utilisée peut être comprise entre 10 Mo et centaines de Mo, ce qui signifie que si votre serveur est configuré avec de nombreux sites, en déterminant les paramètres les plus efficaces pour vos sites peut améliorer considérablement les performances des sites actifs et suspendus.

Avant de passer à des détails spécifiques, nous devons garder à l’esprit que s’il n’y a pas de contraintes de mémoire, il est probablement préférable de simplement définir les sites de sorte qu’ils ne soient jamais suspendus ou arrêtés. Après tout, thereâs peu de valeur dans l’arrêt d’un processus de travail s’il s’agit de l’unique sur l’ordinateur.

**Notez**  dans le cas où le site exécute du code instable, tel que du code avec une fuite de mémoire, ou instable, la définition du site pour qu’il se termine en cas d’inactivité peut être une alternative rapide et incorrecte à la résolution du bogue de code. Ce n’est pas un élément que nous encourageons, mais, en fait, il peut être préférable d’utiliser cette fonctionnalité comme mécanisme de nettoyage, tandis qu’une solution plus permanente est dans les travaux.\]

Â 

Un autre facteur à prendre en compte est que si le site utilise beaucoup de mémoire, le processus de suspension lui-même prend un péage, car l’ordinateur doit écrire les données utilisées par le processus de travail sur le disque. Si le processus de travail utilise un grand segment de mémoire, son interruption peut être plus coûteuse que le coût de l’attente de la sauvegarde.

Pour tirer le meilleur parti de la fonctionnalité de suspension des processus de travail, vous devez passer en revue vos sites dans chaque pool d’applications et décider qui doit être suspendu, qui doit être arrêté, et qui doit être actif indéfiniment. Pour chaque action et chaque site, vous devez déterminer le délai d’attente idéal.

Dans l’idéal, les sites que vous configurez pour la suspension ou la résiliation sont ceux qui ont des visiteurs tous les jours, mais pas suffisamment pour s’assurer qu’ils sont actifs en permanence. Il s’agit généralement de sites disposant d’environ 20 visiteurs uniques un jour ou moins. Vous pouvez analyser les modèles de trafic à l’aide des fichiers journaux du site et calculer le trafic quotidien moyen.

N’oubliez pas qu’une fois qu’un utilisateur spécifique se connecte au site, il le reste généralement pendant au moins un moment, en effectuant des demandes supplémentaires et, par conséquent, le simple comptage des demandes quotidiennes peut ne pas refléter avec précision les modèles de trafic réels. Pour obtenir une lecture plus précise, vous pouvez également utiliser un outil, tel que Microsoft Excel, pour calculer le temps moyen entre les demandes. Exemple :

||URL de requête|Heure de la demande|Delta|
|--- |--- |--- |--- |
|1|/SourceSilverLight/Geosource.web/grosource.html|10:01||
|2|/SourceSilverLight/Geosource.web/sliverlight.js|10:10|0:09|
|3|/SourceSilverLight/Geosource.web/clientbin/geo/1.aspx|10:11|0:01|
|4|/lClientAccessPolicy.xml|10:12|0:01|
|5|/SourceSilverLight/GeosourcewebService/service. asmx|10:23|0:11|
|6|/SourceSilverLight/géosource. Web/GeoSearchServer... ¦.|11 h 50|1:27|
|7|/rest/Services/CachedServices/Silverlight_load_la... ¦|12:50|1:00|
|8|/rest/Services/CachedServices/Silverlight_basemap... ¦.|12:51|0:01|
|9|/rest/Services/DynamicService/Silverlight_basemap... ¦.|12:59|0:08|
|10|/rest/Services/CachedServices/Ortho_2004_cache...|13:40|0:41|
|11|/rest/Services/CachedServices/Ortho_2005_cache. js|13:40|0:00|
|12|/rest/Services/CachedServices/OrthoBaseEngine.aspx|13:41|0:01|

La partie difficile, cependant, consiste à déterminer le paramètre à appliquer pour avoir un sens. Dans notre cas, le site reçoit une série de demandes des utilisateurs et le tableau ci-dessus montre qu’un total de 4 sessions uniques se sont produites au cours d’une période de 4 heures. Avec les paramètres par défaut pour la suspension du processus de travail du pool d’applications, le site se termine après le délai d’expiration par défaut de 20 minutes, ce qui signifie que chacun de ces utilisateurs rencontrerait le cycle de rotation du site. Cela en fait un candidat idéal pour la suspension des processus de travail, car pour la plupart du temps, le site est inactif et, par conséquent, le fait de le suspendre permet d’économiser des ressources et de permettre aux utilisateurs d’accéder presque instantanément au site.

Une remarque finale, et très importante à ce sujet, est que les performances du disque sont cruciales pour cette fonctionnalité. Étant donné que le processus de suspension et de mise en éveil implique l’écriture et la lecture d’une grande quantité de données sur le disque dur, nous vous recommandons vivement d’utiliser un disque rapide. Les disques SSD (Solid State Drives) sont idéaux et fortement recommandés pour cela, et vous devez vous assurer que le fichier d’échange Windows y est stocké (si le système d’exploitation lui-même n’est pas installé sur le disque SSD, configurez le système d’exploitation pour y déplacer le fichier d’échange).

Que vous utilisiez ou non un disque SSD, nous vous recommandons également de fixer la taille du fichier d’échange pour qu’il s’adapte à l’écriture des données de la page de sortie sans redimensionnement des fichiers. Le redimensionnement des fichiers d’échange peut se produire lorsque le système d’exploitation doit stocker des données dans le fichier d’échange, car par défaut, Windows est configuré pour ajuster automatiquement sa taille en fonction des besoins. En définissant une taille fixe, vous pouvez empêcher le redimensionnement et améliorer les performances.

Pour configurer une taille de fichier de page préfixée, vous devez calculer sa taille idéale, qui dépend du nombre de sites à suspendre et de la quantité de mémoire qu’ils consomment. Si la moyenne est de 200 Mo pour un processus de travail actif et que vous avez 500 sites sur les serveurs qui seront suspendus, le fichier d’échange doit être au moins (200 \* 500) Mo par rapport à la taille de base du fichier d’échange (par conséquent, base + 100 Go dans notre exemple).

**Remarque** Lorsque les sites sont suspendus, ils consomment environ 6 Mo chacun. ainsi, dans notre cas, l’utilisation de la mémoire si tous les sites sont suspendus serait d’environ 3 Go. En réalité, cependant, les youâre n’auront probablement jamais été interrompus en même temps.

 
## <a name="transport-layer-security-tuning-parameters"></a>Paramètres de réglage de la sécurité de la couche transport

L’utilisation du protocole TLS (Transport Layer Security) impose des coûts d’UC supplémentaires. Le composant le plus coûteux de TLS est le coût de l’établissement d’un établissement de session, car il implique une négociation complète. La reconnexion, le chiffrement et le déchiffrement sont également ajoutés au coût. Pour de meilleures performances TLS, procédez comme suit :

-   Activez les connexions HTTP persistantes pour les sessions TLS. Cela élimine les coûts d’établissement de session.

-   Réutiliser les sessions le cas échéant, en particulier avec le trafic non persistant.

-   Appliquez de manière sélective le chiffrement uniquement aux pages ou parties du site qui en ont besoin, et non à l’ensemble du site.

**Remarque**
-   Les clés plus grandes fournissent davantage de sécurité, mais elles utilisent également plus de temps processeur.

-   Il se peut que tous les composants n’aient pas besoin d’être chiffrés. Toutefois, le mélange de HTTPs et HTTPS bruts peut entraîner l’affichage d’un avertissement indiquant que tout le contenu de la page n’est pas sécurisé.

 
## <a name="internet-server-application-programming-interface-isapi"></a>ISAPI (Internet Server Application Programming Interface)

Aucun paramètre de réglage spécial n’est nécessaire pour les applications ISAPI. Si vous écrivez une extension ISAPI privée, assurez-vous qu’elle est écrite à des fins de performances et d’utilisation des ressources.

## <a name="managed-code-tuning-guidelines"></a>Instructions de paramétrage du code managé

Le modèle de pipeline intégré d’IIS 10,0 offre un niveau élevé de flexibilité et d’extensibilité. Les modules personnalisés qui sont implémentés en code natif ou managé peuvent être insérés dans le pipeline, ou ils peuvent remplacer des modules existants. Bien que ce modèle d’extensibilité offre plus de commodité et de simplicité, vous devez être prudent avant d’insérer de nouveaux modules gérés qui se raccordent à des événements globaux. L’ajout d’un module managé global signifie que toutes les demandes, y compris les demandes de fichiers statiques, doivent toucher du code géré. Les modules personnalisés sont sujets à des événements tels que garbage collection. En outre, les modules personnalisés ajoutent un coût d’UC significatif en raison du marshaling des données entre le code natif et le code managé. Si possible, vous devez définir la condition préalable sur managedHandler pour le module managé.

Pour obtenir de meilleures performances de démarrage à froid, assurez-vous de précompiler le site Web ASP.NET ou de tirer parti de la fonctionnalité d’initialisation d’application IIS pour précharger l’application.

Si l’état de session n’est pas nécessaire, veillez à le désactiver pour chaque page.

S’il existe de nombreuses opérations liées aux e/s, essayez d’utiliser une version asynchrone des API pertinentes qui offrira une plus grande évolutivité.

En outre, l’utilisation correcte du cache de sortie améliore également les performances de votre site Web.

Lorsque vous exécutez plusieurs ordinateurs hôtes qui contiennent des scripts ASP.NET en mode isolé (un pool d’applications par site), surveillez l’utilisation de la mémoire. Assurez-vous que le serveur dispose de suffisamment de RAM pour le nombre attendu de pools d’applications en cours d’exécution. Envisagez d’utiliser plusieurs domaines d’application au lieu de plusieurs processus isolés.


## <a name="other-issues-that-affect-iis-performance"></a>Autres problèmes affectant les performances d’IIS

Les problèmes suivants peuvent affecter les performances d’IIS :

-   Installation de filtres qui ne prennent pas en charge le cache

    L’installation d’un filtre qui ne prend pas en charge le cache HTTP provoque la désactivation de la mise en cache complète d’IIS, ce qui entraîne une dégradation des performances. Les filtres ISAPI qui ont été écrits avant IISÂ 6,0 peuvent provoquer ce comportement.

-   Requêtes CGI (Common Gateway Interface)

    Pour des raisons de performances, l’utilisation d’applications CGI pour traiter les demandes n’est pas recommandée avec IIS. La création et la suppression fréquentes de processus CGI impliquent une surcharge importante. Il est préférable d’utiliser FastCGI, les scripts d’application ISAPI et les scripts ASP ou ASP.NET. L’isolation est disponible pour chacune de ces options.

## <a name="see-also"></a>Articles associés
- [Réglage des performances du serveur Web](index.md) 
- [Optimisation de HTTP 1.1/2](http-performance.md)
