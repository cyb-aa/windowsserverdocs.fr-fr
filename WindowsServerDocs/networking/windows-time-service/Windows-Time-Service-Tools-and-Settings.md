---
ms.assetid: 6086947f-f9ef-4e18-9f07-6c7c81d7002c
title: Paramètres et outils de Service de temps Windows
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 70b7ee4a9955e023d1664a3c29295a22cd5dc75b
ms.sourcegitcommit: fd6a46b702b235f38d90814dd769106ab37cd61b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
---
# <a name="windows-time-service-tools-and-settings"></a>Paramètres et outils de Service de temps Windows

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012


**Dans cette section**  
  
-   [Outils de Service de temps Windows](#w2k3tr_times_tools_dyax)  
  
-   [Entrées de Registre du Service de temps Windows](#w2k3tr_times_tools_uhlp)  
  
-   [Paramètres de stratégie de groupe de Service de temps Windows](#w2k3tr_times_tools_vwtt)  
  
-   [Ports réseau utilisés par le Service de temps Windows](#w2k3tr_times_tools_suxb)  
  
-   [Obtenir des informations connexes](#w2k3tr_times_tools_qoep)  
  
> [!NOTE]  
> Cette rubrique contient des informations uniquement sur les outils et paramètres de service de temps Windows (W32Time). Si vous souhaitez uniquement synchroniser l’heure pour un ordinateur client joint au domaine, consultez [configurer un ordinateur client pour la synchronisation automatique de domaine de l’heure](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29). Pour des rubriques supplémentaires sur la configuration du service de temps Windows, consultez la liste des rubriques dans la section [où trouver Windows Time Service Configuration Information](https://technet.microsoft.com/library/cc773061.aspx).  
  
> [!CAUTION]  
> Vous ne devez pas utiliser la commande Net time pour configurer ou définir le temps lorsque le service de temps Windows est en cours d’exécution.  

En outre, sur les anciens ordinateurs qui exécutent Windows XP ou version antérieure, le /querysntp Net time commande affiche le nom d’un serveur de protocole NTP (Network Time Protocol) à laquelle un ordinateur est configuré pour synchroniser, mais ce serveur NTP est utilisé uniquement lorsque le client de temps de l’ordinateur est configuré comme NTP ou AllSync. Cette commande depuis a été abandonnée.  
  
La plupart des ordinateurs membres du domaine ont un type de client de temps de NT5DS, ce qui signifie qu’ils synchronisent l’heure à partir de la hiérarchie de domaine. L’exception uniquement par défaut est le contrôleur de domaine qui fonctionne en tant que le maître d’opérations émulateur contrôleur principal de domaine du domaine racine de forêt, qui est généralement configuré pour synchroniser l’heure avec une source de temps externe. Pour afficher la configuration du client de temps d’un ordinateur, exécutez la commande de configuration W32tm /query à partir d’une invite de commandes avec élévation de privilèges dans à compter de Windows Server 2008 et Windows Vista et lire le **Type** ligne dans la sortie de commande. Pour plus d’informations, voir [temps Service fonctionnement Windows ](https://go.microsoft.com/fwlink/?LinkId=117753). Vous pouvez exécuter la commande **reg query HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters** et lire la valeur de **NtpServer** dans la sortie de commande.  
  
> [!IMPORTANT]  
> Avant Windows Server 2016, le service W32Time n’est pas conçu pour répondre aux besoins des applications sensibles à la fois.  Toutefois, les mises à jour vers Windows Server 2016 permettent désormais d’implémenter une solution pour 1 MS précision dans votre domaine.  Voir [Windows 2016 précis temps](accurate-time.md) et [limite de prise en charge pour configurer le service de temps Windows pour les environnements de grande précision](https://go.microsoft.com/fwlink/?LinkID=179459) pour plus d’informations.  
  
## <a name="w2k3tr_times_tools_dyax"></a>Outils de Service de temps Windows  
Les outils suivants sont associés avec le service de temps Windows.  
  
#### <a name="w32tmexe-windows-time"></a>W32tm.exe: Temps de Windows  
**Catégorie**  

Cet outil est installé dans le cadre de Windows XP, Windows Vista, Windows 7, Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 et les installations par défaut Windows Server 2008 R2.  
  
**Compatibilité des versions**  
  
Cet outil fonctionne sur Windows XP, Windows Vista, Windows 7, Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 et les installations par défaut Windows Server 2008 R2.  
  
W32tm.exe est utilisé pour configurer les paramètres de service de temps Windows. Il peut également être utilisé pour diagnostiquer les problèmes avec le service de temps. W32tm.exe est l’outil de ligne de commande par défaut pour la configuration, la surveillance ou la résolution des problèmes du service de temps Windows.  
  
Les tableaux suivants décrivent les paramètres qui sont utilisés avec W32tm.exe.  
  
**Paramètres principal W32tm.exe**  
  
|Paramètre|Description|  
|-------------|---------------|  
|W32tm /?|Aide de la ligne de commande w32tm|  
|W32tm /register|Enregistre le service de temps pour s’exécuter en tant que service et ajoute la configuration par défaut dans le Registre.|  
|W32tm / annuler l’inscription|Annule l’inscription du service de temps et supprime toutes les informations de configuration à partir du Registre.|  
|w32tm /monitor<br /><br />[/ domaine:<domain name>] [/ ordinateurs:<name>[,<name>[,<name>...]]] [/ threads:<num>]|domaine: Spécifie le domaine à analyser. Si aucun nom de domaine est attribué, ou l’option ni le domaine ou ordinateurs n’est spécifiée, le domaine par défaut est utilisé. Cette option peut être utilisée plusieurs fois.<br /><br />ordinateurs - analyse de la liste des ordinateurs donnée. Noms d’ordinateur sont séparés par des virgules, sans espaces. Si un nom est précédé un «*», il est traité comme un contrôleur de domaine principal. Cette option peut être utilisée plusieurs fois.<br /><br />threads - Spécifie le nombre d’ordinateurs à analyser simultanément. La valeur par défaut est 3. Plage autorisée est de 1 à 50.|  
|w32tm /ntte <NT time epoch>|Convertir un temps système NT, en (10 ^ -7) intervalles s comprise entre 0 h 1 - janvier 1601, dans un format lisible.|  
|w32tm /ntpte <NTP time epoch>|Convertir un temps NTP, en (2 ^ -32) intervalles s comprise entre 0 h 1 - Jan 1900, dans un format lisible.|  
|w32tm /resync<br /><br />[/ ordinateur:<computer>]<br /><br />[/nowait]<br /><br />[/rediscover]<br /><br />[/ logicielle]|Indique un ordinateur qu’il devrait resynchroniser son horloge dès que possible, envoi de toutes les statistiques d’erreur accumulées.<br /><br />ordinateur:<computer> -Spécifie l’ordinateur qui doit effectuer la synchronisation. Si non spécifié, l’ordinateur local est resynchronisé.<br /><br />NOWAIT - n’attendent pas que le resynchroniser se produire; effectue un retour immédiat. Sinon, attendez la resynchroniser à effectuer avant de retourner.<br /><br />ré - détecter la configuration réseau et redécouvrir sources réseau, puis resynchroniser.<br /><br />Soft - resynchroniser à l’aide des statistiques d’erreur existantes. Pas utiles, fourni pour la compatibilité.|  
|w32tm /stripchart<br /><br />/ Computer:<target><br /><br />[/ période:<refresh>]<br /><br />[/dataonly]<br /><br />[/ exemples:<count>]<br/><br/>[/rdtsc]<br/>|Afficher un graphique de la bande du décalage entre cet ordinateur et un autre ordinateur.<br /><br />ordinateur:<target> -de mesurer le décalage par rapport à l’ordinateur.<br /><br />période:<refresh> - le temps entre les échantillons, en secondes. La valeur par défaut est 2 s.<br /><br />dataonly - afficher uniquement les données sans graphiques.<br /><br />exemples:<count> - collecter <count> échantillons, puis arrêter. Si non spécifié, les échantillons seront collectés jusqu'à **Ctrl + C** est enfoncée.<br/><br/>écart RDTSC: pour chaque échantillon, cette option affiche des valeurs séparées par des virgules, ainsi que les en-têtes RdtscStart, RdtscEnd, FileTime, RoundtripDelay, NtpOffset au lieu du graphique de texte.<br/><ul><li>[RdtscStart – écart RDTSC (compteur d’horodatage en lecture)](https://en.wikipedia.org/wiki/Time_Stamp_Counter) valeur collectées juste avant que la demande NTP a été générée.</li><li>RdtscEnd – valeur écart RDTSC (compteur d’horodatage en lecture) collecté juste après que la réponse NTP a été reçue et traitée.</li><li>FileTime – valeur FILETIME Local utilisé dans la demande NTP.</li><li>RoundtripDelay – temps écoulé en secondes entre la création de la demande NTP et traitement de la réponse NTP reçue, est calculé en fonction des calculs de traduction NTP.</li><li>NTPOffset – délai en secondes entre l’ordinateur local et le serveur NTP, est calculé en fonction des calculs de décalage NTP.</li></ul>|
|w32tm /config<br /><br />[/ ordinateur:<target>]<br /><br />[/update]<br /><br />[/ manualpeerlist:<peers>]<br /><br />[/ syncfromflags:<source>]<br /><br />[/ LocalClockDispersion:<seconds>]<br /><br />[/ reliable: (Oui & #124; non)]<br /><br />[/ largephaseoffset:<milliseconds>]|ordinateur:<target> -ajuste la configuration de <target>. Si non spécifié, la valeur par défaut est l’ordinateur local.<br /><br />Update - notifie le service de temps que la configuration a changé, à l’origine les modifications prennent effet.<br /><br />manualpeerlist:<peers> -définit la liste d’homologues manuelle de <peers>, qui est une liste délimitée d’adresses DNS et/ou IP. Lorsque vous spécifiez plusieurs homologues, cette option doit être entourée de guillemets.<br /><br />syncfromflags:<source> -définit les sources du client NTP doit se synchroniser à partir de. <source> doit être une liste séparée par des virgules de ces mots-clés (pas respecter la casse):<br /><br />MANUEL - inclure les homologues à partir de la liste d’homologues manuelle.<br /><br />DOMHIER - synchroniser à partir d’un contrôleur de domaine (DC) dans la hiérarchie de domaine.<br /><br />LocalClockDispersion:<seconds> -configure la précision de l’horloge interne W32Time suppose que lorsqu’il ne peut pas acquérir le temps à partir de ses sources configurés.<br /><br />fiable: (Oui & #124; non) - définir si cet ordinateur est une source de temps fiable.<br /><br />Ce paramètre n’est significatif sur les contrôleurs de domaine.<br /><br />Oui - cet ordinateur est un service de temps fiable.<br /><br />NON - cet ordinateur n’est pas un service de temps fiable.<br /><br />LargePhaseOffset:<milliseconds> -définit la différence d’heure entre local et l’heure qui W32Time considère un pic du réseau.|  
|w32tm /tz|Afficher les paramètres de fuseau horaire actuel.|  
|w32tm /dumpreg<br /><br />[/ sous-clé:<key>]<br /><br />[/ ordinateur:<target>]|Afficher les valeurs associées à une clé de Registre donné.<br /><br />La clé par défaut est HKLM\System\CurrentControlSet\Services\W32Time<br /><br />(la clé racine pour le service de temps).<br /><br />sous-clé:<key> -affiche les valeurs associées de sous-clé <key> de la clé par défaut.<br /><br />ordinateur:<target> -interroge les paramètres du Registre de l’ordinateur <target>|  
|w32tm /query [/ ordinateur:<target>] {/ source & #124; configuration & #124; /peers & #124; /status} [/verbose]|Ce paramètre est disponible dans les versions client de temps Windows de Windows Vista et Windows Server 2008.<br /><br />Afficher les informations de service de temps Windows d’un ordinateur.<br /><br />**ordinateur:<target> ** -interroger les informations de ** <target> **. Si non spécifié, la valeur par défaut est l’ordinateur local.<br /><br />**Source** -afficher la source de temps.<br /><br />**Configuration** -afficher la configuration d’exécution et d'où provenance le paramètre. En mode détaillé, afficher le non défini ou inutilisés aussi un paramètre.<br /><br />**homologues** -afficher la liste des homologues et leur état.<br /><br />**état** -état du service Windows affichage de l’heure.<br /><br />**verbose** -définir le mode commenté pour afficher plus d’informations.|  
|w32tm /debug {/ désactiver & #124; {/ Activer file:<name> /size:<bytes> /entries:<value> [/ tronquer]}}|Ce paramètre est disponible dans les versions client de temps Windows de Windows Vista et Windows Server 2008.<br /><br />Activer ou désactiver l’ordinateur local journal privé de service de temps Windows.<br /><br />**désactiver** -désactiver le journal privé.<br /><br />**activer** -activer le journal privé.<br /><br />-   **fichier:<name> ** -spécifiez le nom de fichier absolu.<br />-   **taille:<bytes> ** -spécifier la taille maximale de l’enregistrement circulaire.<br />-   **entrées:<value> ** -contient une liste d’indicateurs, spécifié par le nombre et séparées par des virgules, qui spécifient les types d’informations qui doivent être consignées. Les nombres valides sont de 0 à 300. Une plage de numéros d’est valide, en plus des chiffres unique, par exemple, 0-100,103, 106. La valeur 0-300 est pour toutes les informations de connexion.<br /><br />**tronquer** -tronquer le fichier s’il existe.|  
  
Pour plus d’informations sur **W32tm.exe**, consultez le centre d’aide et Support de Windows XP, Windows Vista, Windows 7, Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 et Windows Server 2008 R2.  
  
## <a name="w2k3tr_times_tools_uhlp"></a>Entrées de Registre du Service de temps Windows  
Les entrées de Registre suivantes sont associées au service de temps Windows.  
  
Ces informations sont fournies en tant que référence pour une utilisation dans la résolution des problèmes ou de la vérification que les paramètres requis sont appliquées. Il est recommandé que vous ne modifiez pas directement le Registre à moins qu’aucune autre solution. Modifications du Registre ne sont pas validées par l’Éditeur du Registre ou par Windows avant qu’ils sont appliqués, et par conséquent, des valeurs incorrectes peuvent être stockées. Cela peut entraîner des erreurs irrécupérables dans le système.  
  
Lorsque cela est possible, utilisez la stratégie de groupe ou d’autres outils Windows, tels que Microsoft Management Console (MMC), pour accomplir des tâches, plutôt que de modifier le Registre directement. Si vous devez modifier le Registre, soyez très vigilant.  
  
> [!WARNING]  
> Certaines des valeurs prédéfinies qui sont configurés dans le fichier de modèle d’administration système (System.adm) pour les paramètres d’objet (GPO) de stratégie de groupe sont différentes des entrées de Registre par défaut correspondantes. Si vous envisagez d’utiliser un objet de stratégie de groupe pour configurer les paramètres de temps Windows, veillez à consulter [valeurs prédéfinis pour les paramètres de stratégie de groupe de service de temps Windows les entrées de Registre du service de temps Windows correspondantes dans Windows Server 2003 diffèrent](https://go.microsoft.com/fwlink/?LinkId=186066). Ce problème s’applique à Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2 et Windows Server 2003.  
  
Plusieurs entrées de Registre pour le service de temps Windows sont les mêmes que le paramètre de stratégie de groupe du même nom. Les paramètres de stratégie de groupe correspondent aux entrées de Registre du même nom situé dans:  
  
>**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\\**

  
Il existe plusieurs clés de Registre à l’emplacement du Registre. Les paramètres de temps Windows sont stockés dans les valeurs sur l’ensemble de ces touches:
* [Paramètres](#Parameters)
* [Configuration](#Configuration)
* [NtpClient](#NtpClient)
* [NtpServer](#NtpServer)
  
> [!NOTE]  
> Bon nombre des valeurs dans la section W32Time du Registre sont utilisés en interne par W32Time pour stocker des informations. Ces valeurs ne doivent pas être modifiés manuellement à tout moment. Ne modifiez pas les paramètres de cette section, sauf si vous êtes familiarisé avec le paramètre et que vous êtes certain que la nouvelle valeur fonctionnera comme prévu. Les entrées de Registre suivantes sont trouvent sous:

>>**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\\**  
  
Certains paramètres sont stockés dans les graduations dans le Registre et d’autres en secondes. Pour convertir le temps de graduations horloge en secondes:  
  
-   1 minute = 60 secondes  
  
-   1 s = 1 000 ms  
  
-   1 ms = 10 000 graduations horloge sur un système Windows, comme décrit à [DateTime.Ticks, propriété](https://msdn.microsoft.com/en-us/library/system.datetime.ticks.aspx).  
  
Par exemple, 5 minutes devient 5 * 60\ * 1000\ * 10000 = 3000000000 graduations d’horloge. 

Toutes les versions incluent Windows 7, Windows 8, Windows 10, Windows Server 2008 et Windows Server 2008 R2, Windows Server 2012, 2012 R2 de Windows Server, Windows Server 2016.  Certaines entrées sont uniquement disponibles sur les versions plus récentes de Windows.


#### <a name="Parameters"></a>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\Parameters

|Entrée de Registre|Version|Description|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|Tous|Cette entrée indique que les combinaisons de mode non standard sont autorisées dans la synchronisation entre homologues. La valeur par défaut pour les membres du domaine est 1. La valeur par défaut pour les clients et serveurs autonomes est 1.|
|NtpServer|Tous|Cette entrée spécifie une liste délimitée des homologues à partir de laquelle un ordinateur obtient la date et heure indiquées, consistant en un ou plusieurs noms DNS ou adresses IP par ligne. Chaque nom DNS ou l’adresse IP doit être unique. Ordinateurs connectés à un domaine doivent synchroniser avec une source de temps plus fiable, telles que l’horloge États-Unis officielle.  <ul><li>0 x 01 SpecialInterval </li><li>0 x 02 UseAsFallbackOnly</li><li>SymmetricActive 0 x 04 - pour plus d’informations sur ce mode, consultez [serveur de temps Windows: 3.3 Modes de fonctionnement](https://go.microsoft.com/fwlink/?LinkId=208012).</li><li>0 x 08 client</li></ul><br />Il n’existe aucune valeur par défaut pour cette entrée de Registre sur les membres du domaine. La valeur par défaut sur les clients et serveurs autonomes est time.windows.com,0x1.<br /><br />Remarque: Pour plus d’informations sur les serveurs NTP disponibles, voir [l’article de la Base de connaissances Microsoft 262680 - une liste des serveurs de temps temps protocole SNTP (Simple Network) qui sont disponibles sur Internet](https://go.microsoft.com/fwlink/?LinkId=186067)|
|ServiceDll|Tous|Cette entrée est gérée par W32Time. Il contient des données réservé qui sont utilisées par le système d’exploitation Windows, et toutes les modifications à ce paramètre peuvent entraîner des résultats imprévisibles. L’emplacement par défaut de cette DLL sur les membres du domaine et les clients et serveurs autonomes est % windir%\System32\W32Time.dll.  |
|ServiceMain|Tous|Cette entrée est gérée par W32Time. Il contient des données réservé qui sont utilisées par le système d’exploitation Windows, et toutes les modifications à ce paramètre peuvent entraîner des résultats imprévisibles. La valeur par défaut sur les membres du domaine est SvchostEntry_W32Time. La valeur par défaut sur les clients et serveurs autonomes est SvchostEntry_W32Time.  "|
|Type|Tous|Cette entrée indique laquelle homologues pour accepter la synchronisation à partir de:  <ul><li>**NoSync**. Le service de temps ne synchronise pas avec d’autres sources.</li><li>**NTP.** Le service de temps synchronise à partir des serveurs spécifiés dans le **NtpServer**. entrée de Registre.</li><li>**Nt5DS**. Le service de temps synchronise à partir de la hiérarchie de domaine.  </li><li>**AllSync**. Le service de temps utilise tous les mécanismes de synchronisation disponibles.  </li></ul>La valeur par défaut sur les membres du domaine est **NT5DS**. La valeur par défaut sur les clients et serveurs autonomes est **NTP**.   |

#### <a name="Configuration"></a>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\Config

|Entrée de Registre|Version|Description|
|------------------------------------|---------------|----------------------------|
|AnnounceFlags|Tous|Cette entrée contrôle si cet ordinateur est marqué comme un serveur de temps fiable. Un ordinateur n’est pas marqué comme fiable, sauf si elle est également marquée comme un serveur de temps.<br /> -0 x 00 pas un serveur de temps  <br /> -serveur de temps toujours 0 x 01  <br /> -serveur de temps automatique de 0 x 02  <br /> -serveur de temps fiable toujours 0 x 04  <br /> -serveur de temps fiable automatique 0 x 08  <br />La valeur par défaut pour les membres du domaine est 10. La valeur par défaut pour les clients et serveurs autonomes est 10.|
|EventLogFlags|Tous|Cette entrée contrôle les événements qui se connecte le service de temps.  <br />-Temps raccourcis: 0 x 1  <br />-Source de modification: 0 x 2  <br />La valeur par défaut sur les membres du domaine est 2. La valeur par défaut sur les clients et serveurs autonomes est 2.  |
|FrequencyCorrectRate|Tous|Cette entrée contrôle la vitesse à laquelle l’horloge a été corrigée. Si cette valeur est trop petite, l’horloge est instable et overcorrects. Si la valeur est trop importante, l’horloge prend beaucoup de temps à synchroniser. La valeur par défaut sur les membres du domaine est 4. La valeur par défaut sur les clients et serveurs autonomes est 4.  <br /><br />Notez que 0 est une valeur non valide pour l’entrée de Registre FrequencyCorrectRate. Sur Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 et les ordinateurs Windows Server 2008 R2, si la valeur est définie sur 0 le service de temps Windows sera automatiquement modifier sur 1.  |
|HoldPeriod|Tous|Cette entrée contrôle la période pour laquelle pic détection est désactivée afin de mettre l’horloge locale dans la synchronisation rapidement. Un pic est un exemple de temps indiquant cette heure est un certain nombre de secondes et est généralement reçu après que la bonne exemples ont été retournés régulièrement. La valeur par défaut sur les membres du domaine est 5. La valeur par défaut sur les clients et serveurs autonomes est 5.  |
|LargePhaseOffset|Tous|Cette entrée indique que l’un temps offset supérieur ou égale à cette valeur dans 10<sup>-7</sup> secondes est considéré comme un pic. Par exemple, une grande quantité de trafic une interruption du réseau peut entraîner un pic. Un pic est ignoré sauf si elle persiste pendant une longue période de temps. La valeur par défaut sur les membres du domaine est 50000000. La valeur par défaut sur les clients et serveurs autonomes est 50000000.  |
|LastClockRate|Tous|Cette entrée est gérée par W32Time. Il contient des données réservé qui sont utilisées par le système d’exploitation Windows, et toutes les modifications à ce paramètre peuvent entraîner des résultats imprévisibles. La valeur par défaut sur les membres du domaine est 156250. La valeur par défaut sur les clients et serveurs autonomes est 156250.  |
|LocalClockDispersion|Tous|Cette entrée contrôle la propagation (en secondes) que vous devez considérer lorsque la seule source de temps est l’horloge CMOS intégré. La valeur par défaut sur les membres du domaine est 10. La valeur par défaut sur les clients et serveurs autonomes est 10.|
|MaxAllowedPhaseOffset|Tous|Cette entrée indique le décalage maximal (en secondes) pendant laquelle W32Time essaie d’ajuster l’horloge de l’ordinateur à l’aide de la fréquence d’horloge. Lorsque le décalage dépasse ce taux, W32Time définit directement l’horloge de l’ordinateur. La valeur par défaut pour les membres du domaine est 300. La valeur par défaut pour les clients et serveurs autonomes est 1.  [Pour plus d’informations, consultez ci-dessous](#MaxAllowedPhaseOffset).|
|MaxClockRate|Tous|Cette entrée est gérée par W32Time. Il contient des données réservé qui sont utilisées par le système d’exploitation Windows, et toutes les modifications à ce paramètre peuvent entraîner des résultats imprévisibles. La valeur par défaut pour les membres du domaine est 155860. La valeur par défaut pour les clients et serveurs autonomes est 155860.  |
|MaxNegPhaseCorrection|Tous|Cette entrée indique la plus grande correction de temps négative en secondes pendant laquelle le service. Si le service détermine qu’une modification supérieure est nécessaire, il consigne un événement à la place. Cas particulier: 0xFFFFFFFF signifie toujours faire une correction de temps. La valeur par défaut pour les membres du domaine est 0xFFFFFFFF. La valeur par défaut pour les clients et serveurs autonomes est 54 000 (15 heures).  |
|MaxPollInterval|Tous|Cette entrée indique l’intervalle le plus grand, en secondes log2, autorisée pour l’intervalle d’interrogation du système. Notez que pendant un système doit interroger en fonction de l’intervalle planifié, un fournisseur peut refuser de fournir les échantillons lorsqu’il est demandé à le faire. La valeur par défaut pour les contrôleurs de domaine est 10. La valeur par défaut pour les membres du domaine est 15. La valeur par défaut pour les clients et serveurs autonomes est 15.  |
|MaxPosPhaseCorrection|Tous|Cette entrée indique la plus grande correction de temps positive en secondes pendant laquelle le service. Si le service détermine qu’une modification supérieure est nécessaire, il consigne un événement à la place. Cas particulier: 0xFFFFFFFF signifie toujours faire une correction de temps. La valeur par défaut pour les membres du domaine est 0xFFFFFFFF. La valeur par défaut pour les clients et serveurs autonomes est 54 000 (15 heures).  |
|MinClockRate|Tous|Cette entrée est gérée par W32Time. Il contient des données réservé qui sont utilisées par le système d’exploitation Windows, et toutes les modifications à ce paramètre peuvent entraîner des résultats imprévisibles. La valeur par défaut pour les membres du domaine est 155860. La valeur par défaut pour les clients et serveurs autonomes est 155860.  |
|MinPollInterval|Tous|Cette entrée indique l’intervalle le plus petit, en secondes log2, autorisée pour l’intervalle d’interrogation du système. Notez que pendant un système ne demande pas des exemples plus fréquemment, un fournisseur peut produire des échantillons parfois autre que l’intervalle planifié. La valeur par défaut pour les contrôleurs de domaine est 6. La valeur par défaut pour les membres du domaine est 10. La valeur par défaut pour les clients et serveurs autonomes est 10.  |
|PhaseCorrectRate|Tous|Cette entrée contrôle la vitesse à laquelle l’erreur phase soit corrigée. En spécifiant une petite valeur corrige l’erreur phase rapidement, mais peut entraîner l’horloge de devenir instables. Si la valeur est trop importante, elle prend plus de temps pour corriger l’erreur phase. <br /><br />La valeur par défaut sur les membres du domaine est 1. La valeur par défaut sur les clients et serveurs autonomes est 7.<br /><br />Remarque: 0 est une valeur non valide pour l’entrée de Registre PhaseCorrectRate. Sur Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 et les ordinateurs Windows Server 2008 R2, si la valeur est définie sur 0, le service de temps Windows change automatiquement il 1.  |
|PollAdjustFactor|Tous|Cette entrée contrôle la décision d’augmenter ou diminuer l’intervalle d’interrogation pour le système. Plus la valeur, plus la quantité d’erreur qui entraîne le diminuer l’intervalle d’interrogation. La valeur par défaut sur les membres du domaine est 5. La valeur par défaut sur les clients et serveurs autonomes est 5. |
|SpikeWatchPeriod|Tous|Cette entrée indique la quantité de temps pendant lequel un décalage suspect doit être conservées avant d’être accepté comme correct (en secondes). La valeur par défaut sur les membres du domaine est 900. La valeur par défaut sur les clients autonomes et des stations de travail est 900.  |
|TimeJumpAuditOffset|Tous|Un entier non signé qui indique le seuil d’audit de raccourcis de temps, en secondes. Si le service de temps règle l’horloge local en définissant l’horloge directement, et la correction de temps est supérieur à cette valeur, le service de temps enregistre un événement d’audit.|
|UpdateInterval|Tous|Cette entrée indique le nombre de graduations d’horloge entre les ajustements de correction de phase. La valeur par défaut pour les contrôleurs de domaine est 100. La valeur par défaut pour les membres du domaine est de 30 000. La valeur par défaut pour les clients et serveurs autonomes est 360 000.  <br /><br />**Remarque**: zéro est une valeur non valide pour l’entrée de Registre UpdateInterval. Sur les ordinateurs exécutant Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 et Windows Server 2003 R2, si la valeur est définie sur 0 le service de temps Windows change automatiquement il 1.<br /><br />Les entrées de trois Registre suivantes ne font pas partie de la configuration par défaut W32Time mais peuvent être ajoutées au Registre pour obtenir des fonctionnalités de journalisation accrue. Les informations enregistrées dans le journal des événements système peuvent être modifiées en changeant la valeur du paramètre EventLogFlags dans l’éditeur d’objets de stratégie de groupe. Par défaut, le service de temps crée un journal dans l’Observateur d’événements chaque fois qu’il bascule vers une nouvelle source de temps.<br /><br />**AVERTISSEMENT**: certains des valeurs prédéfinies qui sont configurés dans le fichier de modèle d’administration système (System.adm) pour les paramètres d’objet (GPO) de stratégie de groupe sont différents correspondant par défaut des entrées de Registre. Si vous envisagez d’utiliser un objet de stratégie de groupe pour configurer les paramètres de temps Windows, veillez à consulter [valeurs prédéfinis pour les paramètres de stratégie de groupe de service de temps Windows les entrées de Registre du service de temps Windows correspondantes dans Windows Server 2003 diffèrent](https://go.microsoft.com/fwlink/?LinkId=186066). Ce problème s’applique à Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2 et Windows Server 2003. |
|UtilizeSslTimeData|Validation de Windows 10 version 1511|Entrée de 1 indique que le W32Time utilise plusieurs horodatages SSL pour amorcer une horloge manifestement incorrectes.|

Les entrées de Registre suivante doivent être ajoutées afin d’activer la journalisation W32Time:  

|Entrée de Registre|Version|Description|
|------------------------------------|---------------|----------------------------|
|FileLogEntries|Tous|Cette entrée contrôle la quantité des écritures créées dans le fichier journal de temps Windows. La valeur par défaut est none, ce qui n’enregistre pas de toute activité de temps Windows. Les valeurs valides sont de 0 à 300. Cette valeur n’affecte pas les entrées de journal des événements normalement créées par temps Windows|
|FileLogName|Tous|Cette entrée contrôle l’emplacement et le nom de fichier du journal de temps Windows. La valeur par défaut est vide et ne doivent pas être modifiée, sauf si **FileLogEntries** est modifiée. Une valeur valide est un chemin d’accès complet et le nom de fichier par temps Windows pour créer le fichier journal. Cette valeur n’affecte pas les entrées de journal des événements normalement créées par temps Windows.  |
|FileLogSize|Tous|Cette entrée contrôle le comportement de l’enregistrement circulaire des fichiers journaux de temps Windows. Lorsque **FileLogEntries** et **FileLogName** sont définis, cette entrée définit la taille, en octets, d’autoriser le fichier journal à atteindre avant de remplacer les entrées du journal plus anciennes par de nouvelles entrées. Un nombre positif est valide, et 3000000 est recommandée. Cette valeur n’affecte pas les entrées de journal des événements normalement créées par temps Windows.  |



#### <a name="NtpClient"></a>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient

|Entrée de Registre|Version|Description|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|Tous|Cette entrée indique que les combinaisons de mode non standard sont autorisées dans la synchronisation entre homologues. La valeur par défaut pour les membres du domaine est 1. La valeur par défaut pour les clients et serveurs autonomes est 1.|
|CompatibilityFlags liste tout|Tous|Cette entrée indique les valeurs et les indicateurs de compatibilité suivants: <br /><br />-DispersionInvalid: 0 x 00000001  <br />-IgnoreFutureRefTimeStamp: 0 x 00000002  <br /> -AutodetectWin2K: 0 x 80000000  <br />-AutodetectWin2KStage2: 0 x 40000000  <br /><br />La valeur par défaut pour les membres du domaine est 0 x 80000000. La valeur par défaut pour les clients et serveurs autonomes est 0 x 80000000.  |
|CrossSiteSyncFlags|Tous|Cette entrée détermine si le service choisit de partenaires de synchronisation en dehors du domaine de l’ordinateur. Les options et les valeurs sont:  <br /><br />-Aucun: 0  <br />-PdcOnly: 1  <br />-Tout: 2  <br /><br />Cette valeur est ignorée si la valeur NT5DS n’est pas définie. La valeur par défaut pour les membres du domaine est 2. La valeur par défaut pour les clients et serveurs autonomes est 2.  |
|DllName|Tous|Cette entrée indique l’emplacement de la DLL pour le fournisseur de temps.  <br /><br />L’emplacement par défaut de cette DLL sur les membres du domaine et les clients et serveurs autonomes est % windir%\System32\W32Time.dll.|
|Activé|Tous|Cette entrée indique si le fournisseur NtpClient est activé dans le Service de temps en cours.  <br /><ul><li>Oui 1</li><li>0 non</li></ul>La valeur par défaut sur les membres du domaine est 1. La valeur par défaut sur les clients et serveurs autonomes est 1.|
|EventLogFlags|Tous|Cette entrée indique les événements consignés par le service de temps Windows.<ul><li>modifications de l’accessibilité de 0 x 1</li><li>0 x 2 grand exemple décalage (cela s’applique à Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 et Windows Server 2003 R2 uniquement)</li></ul>La valeur par défaut sur les membres du domaine est 0 x 1. La valeur par défaut sur les clients et serveurs autonomes est 0 x 1.|
|InputProvider|Tous|Cette entrée indique si le fournisseur NtpClient est activé.  <ul><li>Oui 1  </li><li>0 non </li></ul>La valeur par défaut sur les membres du domaine est 1. La valeur par défaut sur les clients et serveurs autonomes est 1.  |
|LargeSampleSkew|Tous|Cette entrée indique le décalage d’échantillons de grande taille pour la connexion en secondes. Pour être conformes aux spécifications de sécurité et Exchange Commission (s), cela doit être défini sur trois secondes. Les événements seront enregistrés pour ce paramètre uniquement lorsque EventLogFlags est explicitement configuré pour échantillons de grande taille 0 x 2 du décalage. La valeur par défaut sur les membres du domaine est 3. La valeur par défaut sur les clients et serveurs autonomes est 3.  |
|ResolvePeerBackOffMaxTimes|Tous|Cette entrée indique le nombre maximal de tentatives de doubler le délai d’attente lorsque répété tente de localiser un homologue pour la synchronisation avec échec. Une valeur de zéro signifie que l’intervalle d’attente est toujours au minimum. La valeur par défaut sur les membres du domaine est 7. La valeur par défaut sur les clients et serveurs autonomes est 7. |
|ResolvePeerBackoffMinutes|Tous|Cette entrée indique l’intervalle d’attente, en minutes, avant de tenter de localiser un homologue pour la synchronisation avec initiale. La valeur par défaut sur les membres du domaine est 15. La valeur par défaut sur les clients et serveurs autonomes est 15.  |
|SpecialPollInterval|Tous|Cette entrée indique l’intervalle d’interrogation spécial en secondes pour les homologues manuels. Lorsque l’indicateur SpecialInterval 0 x 1 est activé, W32Time utilise cet intervalle au lieu d’un intervalle d’interrogation déterminer par le système d’exploitation. La valeur par défaut sur les membres du domaine est 3 600. La valeur par défaut sur les clients et serveurs autonomes est 604 800.<br/><br/>Nouveau build 1702, SpecialPollInterval se trouve par les valeurs de Registre MinPollInterval et MaxPollInterval Config.|
|SpecialPollTimeRemaining|Tous|Cette entrée est gérée par W32Time. Il contient des données réservées qui sont utilisées par le système d’exploitation Windows. Il spécifie la durée en secondes avant que la resynchronisation W32Time après avoir redémarré l’ordinateur. Toutes les modifications à ce paramètre peuvent entraîner des résultats imprévisibles. La valeur par défaut sur les deux membres du domaine et sur les serveurs et clients autonomes est vide.  |

#### <a name="NtpServer"></a>HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpServer

|Entrée de Registre|Version|Description|
|------------------------------------|---------------|----------------------------|
|AllowNonstandardModeCombinations|Tous|Cette entrée indique que les combinaisons de mode non standard sont autorisées dans la synchronisation entre les clients et serveurs. La valeur par défaut pour les membres du domaine est 1. La valeur par défaut pour les clients et serveurs autonomes est 1.|
|DllName|Tous|Cette entrée indique l’emplacement de la DLL pour le fournisseur de temps.<br /><br />L’emplacement par défaut de cette DLL sur les membres du domaine et les clients et serveurs autonomes est % windir%\System32\W32Time.dll.  |
|Activé|Tous|Cette entrée indique si le fournisseur NtpServer est activé dans le Service de temps en cours. <ul><li>Oui 1</li><li>0 non</li></ul>La valeur par défaut sur les membres du domaine est 1. La valeur par défaut sur les clients et serveurs autonomes est 1.  |
|InputProvider|Tous|Cette entrée indique si le fournisseur NtpServer est activé.  <ul><li>Oui 1  </li><li>0 non </li></ul>La valeur par défaut sur les membres du domaine est 1. La valeur par défaut sur les clients et serveurs autonomes est 1.  |

#### <a name="MaxAllowedPhaseOffset"></a>Informations MaxAllowedPhaseOffset
Dans l’ordre pour W32Time définir l’horloge de l’ordinateur au fil du temps, le décalage doit être inférieure à la valeur MaxAllowedPhaseOffset et répondent à l’équation suivante en même temps:  
```  
|CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) < SystemClockRate / 2  
``` 
Le CurrentTimeOffset est mesurée en graduations, où 1 MS = 10 000 d’horloge graduations sur un système Windows.  
  
SystemClockRate et PhaseCorrectRate sont également mesurée en graduations. Pour obtenir le SystemClockRate, vous pouvez utiliser la commande suivante et le convertir en secondes de graduations à l’aide de la formule de secondes d’horloge * 1000\ * 10000:  
  
```  
W32tm /query /status /verbose  
ClockRate: 0.0156000s  
```  
  
SystemclockRate est le taux de l’horloge sur le système. À l’aide de 156000 secondes par exemple, le SystemclockRate serait être = 0.0156000 * 1000 \ * 10000 = 156000 graduations d’horloge.  
  
MaxAllowedPhaseOffset est également en secondes. Pour convertir pour graduations d’horloge, multipliez MaxAllowedPhaseOffset * 1000\ * 10000.  
  
Les deux exemples suivants montrent comment appliquer  
  
**Exemple 1**: heure diffère de 4 minutes (par exemple, votre temps est 11 h 05 et l’échantillon de temps reçu à partir d’un homologue et susceptibles d’être correct 11:09:00).  
```
phasecorrectRate = 1  
  
UpdateInterval = 30000 (clock ticks)  
  
systemclockRate = 156000 (clock ticks)  
  
MaxAllowedPhaseOffset = 10min = 600 seconds = 600*1000\*10000=6000000000 clock ticks  
  
|currentTimeOffset| = 4mins = 4*60\*1000\*10000 = 2400000000 ticks  
  
Is CurrentTimeOffset < MaxAllowedPhaseOffset?  
  
2400000000 < 6000000000 = TRUE  
```
ET ne répond pas à l’équation ci-dessus? 
```
(|CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) < SystemClockRate / 2)  
  
Is 2,400,000,000 / (30000*1) < 156000/2  
  
Is 100,000 < 78,000  
  
NO/FALSE  
```  
Par conséquent W32tm est redéfini l’horloge immédiatement.  
  
> [!NOTE]  
> Dans ce cas, si vous souhaitez redéfini lentement l’horloge, vous devez ajuster les valeurs de PhaseCorrectRate ou updateInterval dans le Registre ainsi pour garantir les résultats de l’équation dans la valeur TRUE.  
  
**Exemple 2**: heure diffère de 3 minutes.  
```  
phasecorrectRate = 1  
  
UpdateInterval = 30000 (clock ticks)  
  
systemclockRate = 156000 (clock ticks)  
  
MaxAllowedPhaseOffset = 10min = 600 seconds = 600*1000\*10000=6000000000 clock ticks  
  
currentTimeOffset = 3mins = 3*60\*1000\*10000 = 1800000000 clock ticks  
  
Is CurrentTimeOffset < MaxAllowedPhaseOffset?  
  
1800000000 < 6000000000 = TRUE  
```  
ET ne répond pas à l’équation ci-dessus?
```
(|CurrentTimeOffset| / (PhaseCorrectRate*UpdateInterval) < SystemClockRate / 2)  
  
Is 3 mins (1,800,000,000) / (30000*1) < 156000/2  
  
Is 60,000 < 78,000  
  
YES/TRUE  
```  
Dans ce cas l’horloge reprendront lentement.  
  
## <a name="w2k3tr_times_tools_vwtt"></a>Paramètres de stratégie de groupe de Service de temps Windows  
Vous pouvez configurer la plupart des paramètres W32Time à l’aide de l’éditeur d’objets de stratégie de groupe. Cela inclut la configuration d’un ordinateur pour être NTPServer ou NTPClient, la configuration du mécanisme de synchronisation et la configuration d’un ordinateur à une source de temps fiable.  
  
> [!NOTE]  
> Paramètres de stratégie de groupe pour le service de temps Windows peuvent être configurées sur les contrôleurs de domaine Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 et Windows Server 2003 R2 et peuvent être appliqués uniquement aux ordinateurs qui exécutent Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 et Windows Server 2003 R2.  
  
Vous pouvez trouver la stratégie de groupe de paramètres utilisés pour configurer W32Time dans le composant logiciel enfichable Éditeur d’objets de stratégie de groupe dans les emplacements suivants:  
  
-   Service de temps ordinateur Configuration ordinateur\Modèles Templates\System\Windows  
  
    Configurer **paramètres de Configuration globaux** ici.  
  
-   Ordinateur Configuration ordinateur\Modèles Templates\System\Windows temps Service\Time fournisseurs  
  
    Configurer **Client NTP Windows** paramètres ici.  
  
    Activer **Client NTP Windows** ici.  
  
    Activer **serveur NTP Windows** ici.  
  
> [!WARNING]  
> Certaines des valeurs prédéfinies qui sont configurés dans le fichier de modèle d’administration système (System.adm) pour les paramètres d’objet (GPO) de stratégie de groupe sont différentes des entrées de Registre par défaut correspondantes. Si vous envisagez d’utiliser un objet de stratégie de groupe pour configurer les paramètres de temps Windows, veillez à consulter [valeurs prédéfinis pour les paramètres de stratégie de groupe de service de temps Windows les entrées de Registre du service de temps Windows correspondantes dans Windows Server 2003 diffèrent](https://go.microsoft.com/fwlink/?LinkId=186066). Ce problème s’applique à Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2 et Windows Server 2003.  
  
Le tableau suivant répertorie les paramètres de stratégie de groupe globales qui sont associés avec le service de temps Windows et la valeur prédéfinie associée à chaque paramètre. Pour plus d’informations sur chaque paramètre, consultez les entrées de Registre correspondantes dans «[entrées de Registre du Service de temps Windows](#w2k3tr_times_tools_uhlp)» plus haut dans ce sujet. Les paramètres suivants sont contenus dans un seul objet de stratégie de groupe appelé **paramètres de Configuration globaux**.  
  
**Paramètres de stratégie de groupe global associés de temps Windows**  
  
|Paramètre de stratégie de groupe|Valeur prédéfinie|  
|------------------------|------------------|  
|AnnounceFlags|10|  
|EventLogFlags|2|  
|FrequencyCorrectRate|4|  
|HoldPeriod|5|  
|LargePhaseOffset|1280000|  
|LocalClockDispersion|10|  
|MaxAllowedPhaseOffset|300|  
|MaxNegPhaseCorrection|54 000 (15 heures)|  
|MaxPollInterval|15|  
|MaxPosPhaseCorrection|54 000 (15 heures)|  
|MinPollInterval|10|  
|PhaseCorrectRate|7|  
|PollAdjustFactor|5|  
|SpikeWatchPeriod|90|  
|UpdateInterval|100|  
  
Le tableau suivant répertorie les paramètres disponibles pour le **configurer Windows NTP Client** objet stratégie de groupe et les valeurs prédéfinies qui sont associés au service de temps Windows. Pour plus d’informations sur chaque paramètre, consultez les entrées de Registre correspondantes dans «[entrées de Registre du Service de temps Windows](#w2k3tr_times_tools_uhlp)» plus haut dans ce sujet.  
  
**Paramètres de stratégie de groupe de Client de NTP associés de temps Windows**  
  
|Paramètre de stratégie de groupe|Valeur par défaut|  
|------------------------|-----------------|  
|NtpServer|Time.Windows.com,0x1|  
|Type|Options par défaut:<br /><br />-   **NTP.** Utilisez cette option sur les ordinateurs qui ne sont pas joints à un domaine.<br />-   **NT5DS.** Utilisez cette option sur les ordinateurs qui sont joints à un domaine.|  
|CrossSiteSyncFlags|2|  
|ResolvePeerBackoffMinutes|15|  
|ResolvePeerBackoffMaxTimes|7|  
|SpecialPollInterval|3600|  
|EventLogFlags|0|  
  
## <a name="w2k3tr_times_tools_suxb"></a>Ports réseau utilisés par le Service de temps Windows  
Temps Windows suit la spécification du protocole NTP, qui requiert l’utilisation du port UDP 123 pour toutes les communications de synchronisation de temps. Ce port est réservé à la fois Windows et reste réservée à tout moment. Chaque fois que l’ordinateur synchronise son horloge ou offre de temps à un autre ordinateur, cette communication est effectuée sur le port UDP 123.  
  
> [!NOTE]  
> Si vous disposez d’un ordinateur avec plusieurs cartes réseau (également appelés un ordinateur multirésident), vous ne pouvez pas activer le service de temps Windows basé sur la carte réseau.  
  
## <a name="w2k3tr_times_tools_qoep"></a>Obtenir des informations connexes  
Les ressources suivantes contiennent des informations supplémentaires relatives à cette section.  
  
-   RFC *1305* dans la base de données de l’IETF RFC  

