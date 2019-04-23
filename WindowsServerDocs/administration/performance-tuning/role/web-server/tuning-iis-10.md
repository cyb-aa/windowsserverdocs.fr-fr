---
title: Réglage de IIS 10.0
description: Recommmendations pour les serveurs web IIS 10.0 sur Windows Server 16 de réglage des performances
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: DavSo; Ericam; YaShi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 16ea5c857d99e8a69f528e81178911236341dbb8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869850"
---
# <a name="tuning-iis-100"></a>Réglage de IIS 10.0

Internet Information Services (IIS) 10.0 est inclus avec Windows Server 2016. Il utilise un modèle de processus similaire à celui de IIS 8.5 et IIS 7.0. Un pilote de web en mode noyau (http.sys) reçoit et achemine les requêtes HTTP et répond à ces requêtes à partir de son cache de réponse. Processus de travail inscrire sous-espaces de l’URL et http.sys achemine la demande vers le processus approprié (ou un ensemble de processus pour les pools d’applications).

HTTP.sys est responsable de la gestion des connexions et de traitement des requêtes. La demande peut être pris en charge à partir du cache de HTTP.sys ou passée à un processus de travail destinée à être gérée. Plusieurs processus de travail peuvent être configurés, ce qui fournit une isolation à moindre coût. Pour plus d’informations sur la façon de demander la gestion des travaux, voir la figure suivante :

![demande de traitement dans iis 10.0](../../media/perftune-guide-iis-request-handling.png)

HTTP.sys inclut un cache de réponse. Lorsqu’une demande correspond à une entrée dans le cache de réponse, HTTP.sys renvoie la réponse de cache directement à partir de mode noyau. Certaines plateformes d’application web, tels que ASP.NET, fournissent des mécanismes permettant de n’importe quel contenu dynamique doit être mis en cache dans le cache en mode noyau. Le Gestionnaire de fichiers statiques dans IIS 10.0 met automatiquement en cache les fichiers fréquemment demandés dans http.sys.

Un serveur web ayant des composants en mode noyau et mode utilisateur, les deux composants doivent être paramétrés pour des performances optimales. Par conséquent, le réglage IIS 10.0 pour une charge de travail spécifique inclut configuration des éléments suivants :

-   HTTP.sys et le cache de noyau associé

-   Processus de travail et en mode utilisateur IIS, y compris la configuration de pool d’applications

-   Certains paramètres de réglage qui affectent les performances

Les sections suivantes expliquent comment configurer les aspects de IIS 10.0 en mode noyau et en mode utilisateur.

## <a name="kernel-mode-settings"></a>Paramètres de noyau

Paramètres liés aux performances de HTTP.sys se répartissent en deux grandes catégories : gestion et la gestion de demande de connexion et de mettre en cache. Tous les paramètres du Registre sont stockés sous l’entrée de Registre suivante :

``` syntax
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters
```

**Remarque**   si le service HTTP est déjà en cours d’exécution, vous devez le redémarrer pour que les modifications entrent en vigueur.

Â 

## <a name="cache-management-settings"></a>Paramètres de gestion du cache

L’un des avantages qui fournit de HTTP.sys est un cache en mode noyau. Si la réponse est dans le cache en mode noyau, vous pouvez satisfaire une demande HTTP entièrement à partir du mode noyau, ce qui réduit considérablement le coût de l’UC de la demande. Toutefois, le cache de noyau d’IIS 10.0 est basé sur la mémoire physique, et le coût d’une entrée est la quantité de mémoire qu’il occupe.

Une entrée dans le cache est utile uniquement lorsqu’elle est utilisée. Cependant, l’entrée utilise toujours la mémoire physique, si l’entrée est utilisée ou non. Vous devez évaluer l’utilité d’un élément dans le cache (les économies de pouvoir l’utiliser à partir du cache) et son coût (la mémoire physique occupée) pendant la durée de vie de l’entrée en tenant compte des ressources disponibles (UC et mémoire physique) et la charge de travail configuration requise. HTTP.sys essaie de conserver uniquement les éléments utiles, activement sollicitées dans le cache, mais vous peuvent augmenter les performances du serveur web en ajustant le cache HTTP.sys pour les charges de travail particulières.

Des paramètres très utiles pour le cache de noyau HTTP.sys sont les suivantes :

-   **UriEnableCache** valeur par défaut : 1

    Une valeur différente de zéro permet en mode noyau et la mise en cache de fragment. Pour la plupart des charges de travail, le cache doit rester activé. Envisagez de désactiver le cache si vous vous attendez une réponse très faible et la mise en cache de fragment.

-   **UriMaxCacheMegabyteCount** valeur par défaut : 0

    Une valeur différente de zéro qui spécifie la mémoire maximale disponible pour le cache en mode noyau. La valeur par défaut, 0, permet au système ajuster automatiquement la quantité de mémoire est disponible pour le cache.

    **Remarque** en spécifiant la taille définit uniquement la valeur maximale, et le système permettent ne peut-être pas le cache atteindre la taille maximale de jeu.

    Â 

-   **UriMaxUriBytes** valeur par défaut : 262 144 octets (256 Ko)

    La taille maximale d’une entrée dans le cache en mode noyau. Réponses ou des fragments plus long que cela ne sont pas mises en cache. Si vous avez suffisamment de mémoire, envisagez d’augmenter la limite. Si la mémoire est limitée et entrées de grande taille sont éviction petits, il serait utile de réduire la limite.

-   **UriScavengerPeriod** valeur par défaut : 120 secondes

    Le cache HTTP.sys est régulièrement analysé par un nettoyage, et qui ne sont pas accessibles entre les analyses de nettoyage sont supprimées. Définition de la période de nettoyage sur une valeur élevée réduit le nombre d’analyses de nettoyage. Toutefois, l’utilisation de la mémoire cache peut augmenter parce que les entrées plus anciennes et moins fréquemment sollicitées peuvent rester dans le cache. Définition de la période trop faible entraîne des analyses de nettoyage de la plus fréquentes, et peut entraîner de trop de vidages et mettre en cache l’activité.

## <a name="request-and-connection-management-settings"></a>Paramètres de gestion de demande et de connexion

Dans Windows Server 2016, HTTP.sys gère automatiquement les connexions. Les paramètres de Registre suivants ne sont plus utilisés :

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


## <a name="user-mode-settings"></a>Paramètres de mode utilisateur

Les paramètres dans cette section affectent le comportement de processus de travail IISÂ 10.0. Vous trouverez la plupart de ces paramètres dans le fichier de configuration XML suivant :

%SystemRoot%\\system32\\inetsrv\\config\\applicationHost.config

Utiliser Appcmd.exe, la Console de gestion IIS 10.0, le WebAdministration ou les applets de commande PowerShell IISAdministration pour les modifier. La plupart des paramètres sont automatiquement détectées, et ils ne nécessitent pas un redémarrage du processus de travail IIS 10.0 ou serveur d’applications web. Pour plus d’informations sur le fichier applicationHost.config, consultez [Introduction à ApplicationHost.config](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig).


## <a name="ideal-cpu-setting-for-numa-hardware"></a>Paramètre de processeur idéal du matériel NUMA

À partir de Windows 2016, IIS 10.0 prend en charge l’attribution automatique de processeur idéal pour ses threads de pool de thread améliorer les performances et l’évolutivité sur du matériel NUMA. Cette fonctionnalité est activée par défaut et peut être configurée via la clé de Registre suivante :

``` syntax
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\InetInfo\Parameters\ThreadPoolUseIdealCpu
```

Avec cette fonctionnalité activée, Gestionnaire de thread IIS permet de son mieux pour répartir uniformément les threads du pool IIS sur toutes les unités centrales dans tous les nœuds NUMA selon leurs charges actuelles. En règle générale, il est recommandé de conserver cette valeur par défaut inchangée du matériel NUMA.

**Remarque**   le paramètre de processeur idéal diffère les travail processus NUMA nœud Paramètres d’affectation (numaNodeAssignment et numaNodeAffinityMode) introduites dans [paramètres de l’UC pour un Pool d’applications](https://www.iis.net/configreference/system.applicationhost/applicationpools/add/cpu). Le paramètre de processeur idéal affecte comment IIS distribue ses threads de pool, tandis que les paramètres de l’attribution de nœud worker processus NUMA déterminent sur quel nœud NUMA démarre un processus de travail.

## <a name="user-mode-cache-behavior-settings"></a>Paramètres de comportement de cache en mode utilisateur

Cette section décrit les paramètres qui affectent le comportement de mise en cache dans IISÂ 10.0. Le cache en mode utilisateur est implémenté en tant que module qui écoute les événements de mise en cache globales qui sont déclenchés par le pipeline intégré. Pour désactiver complètement le cache en mode utilisateur, supprimez le module de FileCacheModule (cachfile.dll) dans la liste des modules installés dans la section de configuration system.webServer/globalModules dans applicationHost.config.

**system.webServer/caching**

|Attribut|Description|Par défaut|
|--- |--- |--- |
|Enabled|Désactive le cache d’IIS en mode utilisateur lorsque la valeur **False**. Lorsque le cache atteint le taux est très court, que vous pouvez désactiver le cache complètement pour éviter la surcharge qui est associée avec le chemin d’accès du code de cache. La désactivation du cache en mode utilisateur ne désactive pas le cache en mode noyau.|True|
|enableKernelCache|Désactive le cache en mode noyau lorsque la valeur **False**.|True|
|maxCacheSize|Limite la taille de cache en mode utilisateur IIS à la taille spécifiée en mégaoctets. IIS ajuste la valeur par défaut en fonction de la mémoire disponible. Choisissez la valeur soigneusement selon la taille de l’ensemble de fréquemment accédé aux fichiers par rapport à la quantité de RAM ou l’espace d’adressage de processus IIS.|0|
|maxResponseSize|Met en cache les fichiers jusqu'à la taille spécifiée. La valeur réelle varie selon le nombre et la taille des fichiers plus volumineux dans le jeu de données par rapport à la mémoire RAM disponible. Mise en cache volumineux, les fichiers fréquemment demandés peuvent réduire l’utilisation du processeur, les accès au disque et les latences associées.|262144|

## <a name="compression-behavior-settings"></a>Paramètres de comportement de compression

IIS à partir de 7.0 compresse le contenu statique par défaut. En outre, la compression du contenu dynamique est activée par défaut lorsque le DynamicCompressionModule est installé. La compression réduit l’utilisation de la bande passante, mais augmente l’utilisation du processeur. Contenu compressé est mis en cache dans le cache de noyau si possible. À partir de 8.5, IIS vous permet de compression contrôlés de manière indépendante pour le contenu statique et dynamique. Contenu statique fait généralement référence au contenu qui ne change pas, tels que les fichiers GIF ou HTM. Contenu dynamique est généralement généré par les scripts ou du code sur le serveur, autrement dit, les pages ASP.NET. Vous pouvez personnaliser la classification de n’importe quelle extension particulier comme statique ou dynamique.

Pour complètement désactiver la compression, supprimez StaticCompressionModule et DynamicCompressionModule à partir de la liste des modules dans la section system.webServer/globalModules dans applicationHost.config.

**system.webServer/httpCompression**

|Attribut|Description|Par défaut|
|--- |--- |--- |
|staticCompression-EnableCpuUsage<br><br>staticCompression-DisableCpuUsage<br><br>dynamicCompression-EnableCpuUsage<br><br>dynamicCompression-DisableCpuUsage|Active ou désactive la compression si le pourcentage actuel l’utilisation du processeur est au-dessus ou en dessous des limites spécifiées.<br><br>À partir de IIS 7.0, la compression est automatiquement désactivée si l’état stable UC augmente au-dessus du seuil de désactivation. La compression est activée si le processeur passe sous le seuil d’activation.|50, 100, 50 et 90 respectivement|
|Répertoire|Spécifie le répertoire dans lequel les versions compressées des fichiers statiques sont temporairement stockées et mis en cache. Envisagez de déplacer ce répertoire désactiver le lecteur du système si fréquemment sollicitée.|%SystemDrive%\inetpub\temp\IIS temporary Compressed Files|
|doDiskSpaceLimiting|Spécifie si une limite existe pour la quantité d’espace disque, tous les fichiers compressés peuvent occuper. Fichiers compressés sont stockés dans le répertoire de compression spécifié par le **directory** attribut.|True|
|maxDiskSpaceUsage|Spécifie le nombre d’octets d’espace disque que les fichiers compressés peuvent occuper dans le répertoire de compression.<br><br>Ce paramètre peut devoir être augmentée si la taille totale de tout le contenu compressé est trop volumineuse.|100 Mo|

**system.webServer/urlCompression**

|Attribut|Description|Par défaut|
|--- |--- |--- |
|doStaticCompression|Spécifie si le contenu statique est compressé.|True|
|doDynamicCompression|Spécifie si le contenu dynamique est compressé.|True|

**Remarque** pour les serveurs exécutant IIS 10.0 qui ont une faible utilisation moyenne du processeur, envisagez d’activer la compression de contenu dynamique, en particulier si les réponses sont volumineuses. Tout d’abord, cela doit être fait dans un environnement de test pour évaluer l’effet sur l’utilisation du processeur à partir de la ligne de base.


### <a name="tuning-the-default-document-list"></a>Réglage de la liste des documents par défaut

Le module de document par défaut gère les requêtes HTTP pour la racine d’un répertoire et les convertit en demandes pour un fichier spécifique, tel que Default.htm ou Index.htm. En moyenne, aroundÂ 25 pour cent de toutes les demandes sur Internet passent par le chemin d’accès de document par défaut. Cette valeur varie considérablement pour les sites individuels. Lorsqu’une requête HTTP ne spécifie pas un nom de fichier, le module de document par défaut recherche dans la liste des documents autorisé par défaut pour chaque nom dans le système de fichiers. Cela peut nuire aux performances, surtout si atteindre le contenu nécessite d’effectuer un réseau round voyage ou se touchent représentant un disque.

Vous pouvez éviter la surcharge en désactivant sélectivement des documents par défaut et en réduisant ou le classement de la liste des documents. Pour les sites Web qui utilisent un document par défaut, vous devez réduire la liste uniquement les types de document par défaut sont utilisés. En outre, ordre de la liste pour qu’elle démarre avec les plus fréquemment que nom de fichier de document par défaut accessible.

Vous pouvez sélectivement définir le comportement de document par défaut sur une URL particulière en personnalisant la configuration à l’intérieur d’une balise d’emplacement dans la section applicationHost.config ou en insérant un fichier web.config directement dans le répertoire de contenu. Cela permet une approche hybride, ce qui permet des documents par défaut uniquement où ils sont nécessaires et les jeux de nom de la liste vers le fichier approprié pour chaque URL.

Pour désactiver complètement les documents par défaut, supprimez la liste des modules dans la section system.webServer/globalModules dans applicationHost.config DefaultDocumentModule.

**system.webServer/defaultDocument**

|Attribut|Description|Par défaut|
|--- |--- |--- |
|enabled|Spécifie que les documents par défaut sont activées.|True|
|&lt;fichiers&gt; élément|Spécifie les noms de fichiers qui sont configurés en tant que documents par défaut.|La liste par défaut est Default.htm, Default.asp, Index.htm, Index.html, Iisstart.htm et Default.aspx.|

## <a name="central-binary-logging"></a>Journalisation binaire centralisé

Lorsque la session de serveur a plusieurs groupes d’URL dans cette section, le processus de création des centaines de mise en forme fichiers journaux pour des groupes d’URL individuels et écrire les données de journal sur un disque peut rapidement consommer de précieuses ressources de processeur et mémoire, ce qui entraîne des performances et problèmes d’extensibilité. La journalisation centralisée binaire réduit la quantité de ressources système qui sont utilisés pour la journalisation, tout en fournissant des données de journal détaillées pour les organisations qui en ont besoin. L’analyse de journaux d’au format binaire nécessite un outil de post-traitement.

Vous pouvez activer la journalisation binaire centralisé en définissant l’attribut centralLogFileMode à CentralBinary et en affectant la **activé** attribut **True**. Envisagez de déplacer l’emplacement du fichier journal central dehors de la partition système et sur un lecteur de journalisation dédié pour éviter la contention entre les activités système et les activités de journalisation.

**system.applicationHost/log**

|Attribut|Description|Par défaut|
|--- |--- |--- |
|centralLogFileMode|Spécifie le mode de journalisation pour un serveur. Remplacez cette valeur CentralBinary pour activer la journalisation binaire centrale.|Site|

**system.applicationHost/log/centralBinaryLogFile**

|Attribut|Description|Par défaut|
|--- |--- |--- |
|enabled|Spécifie si la journalisation binaire centralisé est activée.|False|
|Répertoire|Spécifie le répertoire dans lequel les entrées de journal sont écrits.|%SystemDrive%\inetpub\logs\LogFiles|


## <a name="application-and-site-tunings"></a>Réglages d’application et de site

Les paramètres suivants se rapportent à des réglages de site et le pool d’application.

**system.applicationHost/applicationPools/applicationPoolDefaults**

|Attribut|Description|Par défaut|
|--- |--- |--- |
|queueLength|Indique à HTTP.sys combien de demandes est en file d’attente pour un pool d’applications avant de futures demandes sont rejetées. Lorsque la valeur de cette propriété est dépassée, IIS rejette les demandes suivantes avec une erreur 503.<br><br>Envisagez d’augmenter ce pour les applications qui communiquent avec les magasins de données back-end de latence élevée, si des 503 erreurs sont observés.|1000|
|enable32BitAppOnWin64|Lorsque la valeur est True, permet à une application 32 bits pour s’exécuter sur un ordinateur qui dispose d’un processeur 64 bits.<br><br>Envisagez d’activer le mode 32 bits si la consommation de mémoire est un problème. Étant donné que les tailles de pointeur et tailles d’instructions sont plus petites, les applications 32 bits utilisent moins de mémoire des applications 64 bits. L’inconvénient à l’exécution des applications 32 bits sur un ordinateur 64 bits est que l’espace d’adressage en mode utilisateur est limitée à 4 Go.|False|

**system.applicationHost/sites/VirtualDirectoryDefault**

|Attribut|Description|Par défaut|
|--- |--- |--- |
|allowSubDirConfig|Spécifie si IIS recherche les fichiers web.config dans les répertoires de contenu inférieures à la valeur actuelle au niveau (True) ou qu’il ne recherche pas les fichiers web.config dans les répertoires de contenu inférieures au niveau actuel (False). En imposant une limitation simple, ce qui permet la configuration uniquement dans les répertoires virtuels, IISÂ 10.0 peut savoir que, à moins que  **/ &lt;nom&gt;.htm** est un répertoire virtuel, il ne doit pas se présenter un fichier de configuration. Ignorer les opérations de fichier supplémentaire peut améliorer considérablement les performances des sites Web qui ont un très grand ensemble de contenu statique accessible aléatoirement.|True|

## <a name="managing-iis-100-modules"></a>La gestion des modules IIS 10.0

IIS 10.0 a été factorisé en plusieurs modules extensible à l’utilisateur pour prendre en charge une structure modulaire. Cette factorisation a un moindre coût. Pour chaque module, le pipeline intégré doit appeler le module pour chaque événement qui se rapportent au module. Cela se produit même si le module doit effectuer un travail. Vous pouvez économiser les cycles de processeur et mémoire en supprimant tous les modules qui ne sont pas pertinentes pour un site Web particulier.

Un serveur web qui met l’accent sur les fichiers statiques simples peut-être inclure uniquement les cinq modules suivants : UriCacheModule HttpCacheModule, StaticFileModule, AnonymousAuthenticationModule et HttpLoggingModule.

Pour supprimer des modules de applicationHost.config, supprimez toutes les références au module des sections system.webServer/handlers et System.webServer/modules outre la suppression de la déclaration de module dans system.webServer/globalModules.

## <a name="classic-asp-settings"></a>Paramètres de l’ASP classique

Le coût de principaux du traitement d’une demande ASP classique inclut l’initialisation d’un moteur de script, la compilation du script ASP demandé dans un modèle ASP et l’exécution du modèle sur le moteur de script. Tandis que le coût d’exécution de modèle dépend de la complexité du script ASP demandée, module ASP classique de IIS peut mettre en cache les moteurs de script dans les modèles de mémoire et du cache en mémoire et disque (uniquement si le cache de modèles en mémoire dépasse) pour optimiser les performances dans Scénarios de processeur de manière intensive.

Les paramètres suivants sont utilisés pour configurer le cache de modèles ASP classique et le cache du moteur de script, et elles n’affectent pas les paramètres ASP.NET.

**system.webServer/asp/cache**

|Attribut|Description|Par défaut|
|--- |--- |--- |
|diskTemplateCacheDirectory|Le nom du répertoire utilisé par ASP pour stocker les modèles compilés lorsque le cache en mémoire déborde.<br><br>Recommandation : Ensemble dans un répertoire qui n’est pas fortement utilisé, par exemple, un lecteur qui n’est pas partagé avec le système d’exploitation, journaux IIS, ou autres fréquemment accédé à du contenu.|%SystemDrive%\inetpub\temp\ASP les modèles compilés|
|maxDiskTemplateCacheFiles|Spécifie le nombre maximal de modèles ASP compilés qui peuvent être mis en cache sur le disque.<br><br>Recommandation : La valeur est la valeur maximale de 0x7FFFFFFF.|2000|
|scriptFileCacheSize|Cet attribut spécifie le nombre maximal de modèles ASP compilés qui peuvent être mis en cache en mémoire.<br><br>Recommandation : La valeur est au moins autant que le nombre de prises en charge par un pool d’applications des scripts ASP fréquemment demandées. Si possible, il est défini sur autant de modèles ASP Autoriser les limites de mémoire.|500|
|scriptEngineCacheMax|Spécifie le nombre maximal de moteurs de script qui améliorera la sécurité en mémoire cache.<br><br>Recommandation : La valeur est au moins autant que le nombre de prises en charge par un pool d’applications des scripts ASP fréquemment demandées. Si possible, affectez à autant de moteurs de script comme la limite de mémoire autorisée.|250|

**system.webServer/asp/limits**

|Attribut|Description|Par défaut|
|--- |--- |--- |
|processorThreadMax|Spécifie le nombre maximal de threads de travail par processeur qu’ASP peut créer. Augmenter si le paramètre actuel est insuffisant pour gérer la charge, ce qui peut provoquer des erreurs lorsqu’il sert des requêtes ou entraîner incomplète l’utilisation des ressources du processeur.|25|

**system.webServer/asp/comPlus**

|Attribut|Description|Par défaut|
|--- |--- |--- |
|executeInMta|La valeur **True** si des erreurs ou des problèmes sont détectées pendant que IIS sert ASP contenu. Cela peut se produire, par exemple, lorsque vous hébergez plusieurs sites isolés dans lequel chaque site s’exécute sous son propre processus de travail. Les erreurs sont généralement signalés à partir de COM + dans l’Observateur d’événements. Ce paramètre active le modèle de cloisonnement multithread dans ASP.|False|


## <a name="aspnet-concurrency-setting"></a>Paramètre de simultanéité d’ASP.NET

### <a name="aspnet-35"></a>ASP.NET 3.5
Par défaut, ASP.NET limite la concurrence de requête pour réduire la consommation de mémoire de l’état stable sur le serveur. Applications de concurrence élevée devrez peut-être ajuster certains paramètres pour améliorer les performances globales. Vous pouvez modifier ce paramètre dans le fichier aspnet.config :

``` syntax
<system.web>
  <applicationPool maxConcurrentRequestsPerCPU="5000"/>
</system.web>
```

Le paramètre suivant est utile d’utiliser pleinement les ressources sur un système :

-   **maxConcurrentRequestPerCpu** valeur par défaut : 5000

    Ce paramètre limite le nombre maximal d’exécuter simultanément des requêtes ASP.NET sur un système. La valeur par défaut est conservateur pour réduire la consommation de mémoire des applications ASP.NET. Envisagez d’augmenter cette limite sur les systèmes qui exécutent des applications qui effectuent des opérations d’e/s synchrones, long. Sinon, les utilisateurs peuvent voir une latence élevée en raison d’échecs queuing ou la demande en raison du dépassement des limites de la file d’attente sous une charge élevée quand le paramètre par défaut est utilisé.

### <a name="aspnet-46"></a>ASP.NET 4.6
Outre le paramètre maxConcurrentRequestPerCpu, ASP.NET 4.7 fournit également des paramètres pour améliorer les performances dans les applications qui reposent fortement sur l’opération asynchrone. Le paramètre peut être modifié dans le fichier aspnet.config.

``` syntax
<system.web>
  <applicationPool percentCpuLimit="90" percentCpuLimitMinActiveRequestPerCpu="100"/>
</system.web>
```

-   **percentCpuLimit** valeur par défaut : Requête asynchrone 90 a certains problèmes d’extensibilité lorsqu’une charge considérable (au-delà des capacités matérielles) est placée sur un tel scénario. Le problème est dû à la nature de l’allocation sur des scénarios asynchrones. Dans ces conditions, l’allocation se produit lorsque l’opération asynchrone commence, et il ne sera utilisée lorsqu’elle est terminée. À ce stade, tout à fait possible s itâ les objets ont été déplacés à la génération 1 ou 2 par le garbage collector. Dans ce cas, l’augmentation de la charge affichera augmentation sur demande par seconde (rps) jusqu'à ce qu’un point. Une fois que nous transmettons ce point, le temps passé dans le GC commencent à devenir un problème et le rps commenceront à dip, avoir un effet négatif de mise à l’échelle. Pour résoudre le problème, lorsque l’utilisation du processeur dépasse percentCpuLimit paramètre, les demandes seront envoyées à la file d’attente natif d’ASP.NET.
-   **percentCpuLimitMinActiveRequestPerCpu** valeur par défaut : 100 (paramètre percentCpuLimit) sur la limitation de l’UC n’est pas basé sur le nombre de demandes, mais sur comment onéreux qu’ils le sont. Par conséquent, il peut être seulement quelques requêtes sollicitant beaucoup le processeur à l’origine d’une sauvegarde dans la file d’attente natif sans aucun moyen d’une vide à part les demandes entrantes. Pour résoudre ce problme, percentCpuLimitMinActiveRequestPerCpu peut être utilisé pour garantir qu'un nombre minimal de demandes est traitée avant que la limitation intervient.

## <a name="worker-process-and-recycling-options"></a>Processus de travail et les options de recyclage

Vous pouvez configurer les options pour le recyclage de processus de travail IIS et fournir des solutions pratiques aux événements ou situations AIGUES sans nécessiter d’intervention ou réinitialisation d’un service ou ordinateur. Ces situations et les événements incluent les fuites de mémoire, charge croissante de mémoire ou les processus de travail inactives ou ne répond pas. Dans des conditions normales, options de recyclage peut ne pas être nécessaire et recyclage peut être mis hors tension ou le système peut être configuré pour recycler très rarement.

Vous pouvez activer le recyclage de processus pour une application particulière en ajoutant des attributs pour le **periodicRestart/recyclage** élément. L’événement de recyclage peut être déclenchée par plusieurs événements, y compris l’utilisation de la mémoire, un nombre fixe de demandes et une période de temps fixe. Lorsqu’un processus de travail est recyclé, les demandes en file d’attente et en cours d’exécution sont purgées et un nouveau processus est démarré simultanément de nouvelles demandes. Le **periodicRestart/recyclage** élément est par application, ce qui signifie que chaque attribut dans le tableau suivant est partitionnée sur une base par application.

**system.applicationHost/applicationPools/ApplicationPoolDefaults/recycling/periodicRestart**

|Attribut|Description|Par défaut|
|--- |--- |--- |
|memory|Activer le recyclage de processus si la consommation de mémoire virtuelle dépasse la limite spécifiée en kilo-octets. Il s’agit d’un paramètre utile pour les ordinateurs 32 bits qui ont un petit, un espace d’adressage de 2 Go. Il peut aider à éviter des demandes ayant échoué en raison d’erreurs de mémoire insuffisante.|0|
|privateMemory|Activer le recyclage de processus si les allocations de mémoire privée dépassent une limite spécifiée en kilo-octets.|0|
|requests|Activer le recyclage de processus après un certain nombre de demandes.|0|
|time|Activer le recyclage de processus après un laps de temps spécifié.|29:00:00|


## <a name="dynamic-worker-process-page-out-tuning"></a>Paramétrage dynamique de la page à la sortie du processus de travail

À compter de Windows Server 2012 R2, IIS offre la possibilité de configurer les processus de travail pour interrompre une fois qu’ils sont restées inactives pendant un certain temps (en plus de l’option de la routine terminate, qui existe depuis IIS 7).

L’objectif principal du processus de travail inactif page-out et fonctionnalités d’arrêt de processus de travail inactif consiste à conserver de l’utilisation de la mémoire sur le serveur, dans la mesure où un site peut consommer une grande quantité de mémoire même si elle est simplement assis, à l’écoute. En fonction de la technologie utilisée sur le site (vs contenu statique ASP.NET vs autres infrastructures), la mémoire utilisée peut être comprise entre environ 10 Mo à des centaines de Mo, et cela signifie que si votre serveur est configuré avec de nombreux sites, déterminer les meilleurs paramètres pour vos sites peuvent améliorer considérablement les performances de sites actifs et suspendus.

Avant de rentrer dans les détails, nous devons n’oubliez pas que s’il en existe aucune contrainte de mémoire, puis il s’agit probablement de bonnes de simplement définir les sites jamais suspendre ou arrêter. Après tout, thereâ s peu d’intérêt dans un processus de travail de fin d’exécution traiter s’il s’agit de la seule sur l’ordinateur.

**Remarque**   au cas où le site exécute du code instable, tel que le code avec une fuite de mémoire, ou sinon instable, définition du site pour terminer sur inactif peut être une alternative simple et rapide pour résoudre le bogue de code. Cela n’est pas quelque chose que nous encourageons, mais dans un analysez en détail, il peut être préférable d’utiliser cette fonctionnalité comme un mécanisme de nettoyage, tandis que la solution définitive est en cours de préparation.\]

Â 

Un autre facteur à prendre en considération est que si le site n’utilise pas beaucoup de mémoire, le processus d’interruption lui-même a un impact, car l’ordinateur doit écrire les données utilisées par le processus de travail sur le disque. Si le processus de travail utilise une grande partie de la mémoire, puis en le suspendant peut être plus coûteux que de devoir attendre qu’il démarre sauvegarder.

Pour tirer profit de la fonctionnalité de suspension de processus de travail, vous devez examiner vos sites dans chaque pool d’applications et décider ce qui doit être suspendu, qui doivent être terminé, et qui doit être actif indéfiniment. Pour chaque action et chaque site, vous devez déterminer le délai d’attente idéale.

Dans l’idéal, les sites que vous configurez pour l’interruption ou l’arrêt sont celles qui ont des visiteurs tous les jours, mais pas suffisamment pour justifier la conserver active tout le temps. Il s’agit généralement de sites avec environ 20 de visiteurs par jour ou moins. Vous pouvez analyser les modèles de trafic à l’aide de fichiers de journaux du site et le calcul du trafic quotidien moyen.

N’oubliez pas qu’une fois qu’un utilisateur se connecte au site, ils généralement resteront dessus pendant au moins un certain temps, effectuer d’autres demandes et donc, en comptant de demandes quotidiennes peuvent ne pas reflètent correctement les modèles de trafic réel. Pour obtenir une lecture plus précise, vous pouvez également utiliser un outil, comme Microsoft Excel, pour calculer le temps moyen entre les demandes. Exemple :

||URL de demande|Heure de la demande|Delta|
|--- |--- |--- |--- |
|1|/SourceSilverLight/Geosource.web/grosource.html|10:01||
|2|/SourceSilverLight/Geosource.web/sliverlight.js|10:10|0:09|
|3|/SourceSilverLight/Geosource.web/clientbin/geo/1.aspx|10:11|0:01|
|4|/lClientAccessPolicy.xml|10:12|0:01|
|5|/ SourceSilverLight/GeosourcewebService/Service.asmx|10:23|0:11|
|6|/ SourceSilverLight/Geosource.web/GeoSearchServer...¦.|11:50|1:27|
|7|/REST/services/CachedServices/Silverlight_load_la...¦|12:50|1:00|
|8|/REST/services/CachedServices/Silverlight_basemap...¦.|12:51|0:01|
|9|/ rest/Services/DynamicService/Silverlight_basemap... l’objectif.|12:59|0:08|
|10|/rest/Services/CachedServices/Ortho_2004_cache.as...|13:40|0:41|
|11|/rest/Services/CachedServices/Ortho_2005_cache.js|13:40|0:00|
|12|/rest/Services/CachedServices/OrthoBaseEngine.aspx|13:41|0:01|

La partie difficile, cependant, consiste à savoir le paramètre à appliquer pour faciliter la compréhension. Dans notre cas, le site Obtient un ensemble de demandes d’utilisateurs, et le tableau ci-dessus montre qu’un total de 4 sessions uniques s’est produite dans un délai de 4 heures. Les paramètres par défaut pour la suspension de processus de travail du pool d’applications, le site est terminé après que le délai par défaut de 20 minutes, ce qui signifie que chacun de ces utilisateurs connaîtriez cycle en place un site. Cela rend un candidat idéal pour la suspension du processus de travail, étant donné que pour la plupart du temps, le site est inactif, et donc suspendant serait préserver les ressources et autorisez les utilisateurs à atteindre le site presque instantanément.

Une remarque finale et très importante à ce sujet est que les performances du disque sont crucial pour cette fonctionnalité. Étant donné que la suspension et le processus de mise en éveil impliquent l’écriture et la lecture de grandes quantités de données sur le disque dur, nous vous recommandons vivement d’à l’aide d’un disque rapide pour cela. Solid State Drive (SSD) est idéale et fortement recommandée pour cela, et vous devez vous assurer que le fichier d’échange Windows est stocké sur celui-ci (si le système d’exploitation lui-même n’est pas installé sur le disque SSD, configurer le système d’exploitation pour atteindre le fichier d’échange).

Si vous utilisez un disque SSD ou non, nous recommandons également de correction de la taille du fichier de page pour prendre en charge l’écriture des page de données à ce dernier sans redimensionnement de fichier. Redimensionnement du fichier d’échange peut se produire quand le système d’exploitation doit stocker des données dans le fichier d’échange, car par défaut, Windows est configuré pour ajuster automatiquement sa taille selon besoin. En définissant la taille à une correction, vous pouvez empêcher le redimensionnement et améliorer considérablement les performances.

Pour configurer une taille de fichier de page fixe préalable, vous devez calculer sa taille idéale, qui varie selon le nombre de sites que vous être suspension et la quantité de mémoire qu’ils utilisent. Si la moyenne est de 200 Mo pour un processus de travail actifs et vous disposez de 500 sites sur les serveurs qui seront, le fichier d’échange doit être au moins (200 \* 500) Mo sur la taille de base du fichier de page (donc base + 100 Go dans notre exemple).

**Remarque** lorsque les sites sont suspendus, elles consomment environ 6 Mo chacun, dans notre cas, l’utilisation de mémoire si tous les sites sont suspendus serait environ 3 Go. En réalité, cependant, youâ allons probablement jamais les suspendu en même temps.

 
## <a name="transport-layer-security-tuning-parameters"></a>Réglage des paramètres de sécurité de la couche de transport

L’utilisation de sécurité TLS (Transport Layer) impose coût UC supplémentaire. Le composant le plus onéreux de TLS est le coût de l’établissement d’un établissement de session, car elle implique une négociation complète. La reconnexion, le chiffrement et déchiffrement également ajoutent le coût. Pour de meilleures performances de TLS, procédez comme suit :

-   Activer les connexions HTTP persistantes pour les sessions TLS. Cela élimine les coûts d’établissement de session.

-   Sessions de réutiliser le cas échéant, en particulier avec un trafic non-keep-alive.

-   Appliquer de manière sélective le chiffrement uniquement aux pages ou des parties du site qui en ont besoin, au lieu de cela pour l’ensemble du site.

**Remarque**
-   Clés plus longues offrent une meilleure sécurité, mais ils utilisent également plus de temps processeur.

-   Tous les composants n’ayez pas à chiffrer. Toutefois, le mélange brut HTTP et HTTPS peut entraîner dans une fenêtre contextuelle avertissement que pas tout le contenu sur la page n’est pas sécurisé.

 
## <a name="internet-server-application-programming-interface-isapi"></a>Internet Server Application Programming Interface (ISAPI)

Aucun paramètre de réglage spéciales n’est nécessaires pour les applications ISAPI. Si vous écrivez une extension ISAPI privée, assurez-vous qu’il est écrit pour les performances et l’utilisation des ressources.

## <a name="managed-code-tuning-guidelines"></a>Instructions de réglage de code managé

Le modèle de pipeline intégré dans IIS 10.0 permet un degré élevé de flexibilité et d’extensibilité. Les modules personnalisés qui sont implémentées dans du code natif ou managé peuvent être insérés dans le pipeline, ou elles peuvent remplacer des modules existants. Bien que ce modèle d’extensibilité offre plus de commodité et de simplicité, vous devez être prudent avant d’insérer de nouveaux modules gérés qui se raccordent dans événements globaux. Ajout de qu'un module managé global signifie que toutes les demandes, y compris les demandes de fichier statique, doivent toucher le code managé. Modules personnalisés sont vulnérables aux événements tels que le garbage collection. En outre, les modules personnalisés ajouter significative coût UC en raison de marshaling de données entre code natif et managé. Si possible, vous devez définir la précondition managedHandler pour le module managé.

Pour obtenir de meilleures performances de démarrage à froid, assurez-vous que vous précompilez la fonctionnalité ASP.NET web site ou d’exploiter l’initialisation d’Application IIS pour le préchauffage à l’application.

Si l’état de session n’est pas nécessaire, veillez à désactiver pour chaque page.

S’il existe nombreuses e/s liée aux opérations, essayez d’utiliser une version asynchrone d’API pertinentes afin de vous donner bien une meilleure extensibilité.

Également à l’aide du Cache de sortie correctement accroît également les performances de votre site web.

Lorsque vous exécutez plusieurs ordinateurs hôtes qui contiennent des scripts d’ASP.NET en mode isolé (un pool d’applications par site), surveiller l’utilisation de la mémoire. Assurez-vous que le serveur dispose de suffisamment de RAM pour le nombre attendu de pools d’applications en cours d’exécution simultanément. Envisagez d’utiliser plusieurs domaines d’application au lieu de plusieurs processus isolés.


## <a name="other-issues-that-affect-iis-performance"></a>Autres problèmes affectant les performances IIS

Les problèmes suivants peuvent affecter les performances d’IIS :

-   Installation de filtres qui ne sont pas compatibles avec cache

    L’installation d’un filtre qui n’est pas HTTP-cache-prenant en charge provoque IIS désactiver complètement la mise en cache, ce qui entraîne une dégradation des performances. Les filtres ISAPI qui ont été écrit avant IISÂ 6.0 peuvent entraîner ce comportement.

-   Demande de l’Interface CGI (Common Gateway)

    Pour des raisons de performances, l’utilisation des applications CGI pour traiter les requêtes n’est pas recommandée avec IIS. Souvent la création et la suppression des processus CGI impliquent une surcharge significative. Meilleures alternatives incluent l’utilisation de FastCGI, scripts d’application ISAPI et scripts ASP ou ASP.NET. Isolation est disponible pour chacune de ces options.

# <a name="see-also"></a>Voir aussi
- [Web le réglage des performances de serveur](index.md) 
- [HTTP 1.1/2 de paramétrage](http-performance.md)
