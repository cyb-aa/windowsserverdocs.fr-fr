---
ms.assetid: 6086947f-f9ef-4e18-9f07-6c7c81d7002c
title: Paramètres et outils du service de temps Windows
author: Teresa-Motiv
ms.author: v-tea
ms.date: 02/24/2020
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 7e7a233d17d8f2e32286a0869b283e450a34bbbc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860142"
---
# <a name="windows-time-service-tools-and-settings"></a>Paramètres et outils du service de temps Windows

> S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 ou version ultérieure

Dans cette rubrique, vous allez découvrir les outils et les paramètres pour le service de temps Windows (W32Time).  

Si vous souhaitez synchroniser l’heure seulement pour un ordinateur client joint à un domaine, consultez [Configurer un ordinateur client pour la synchronisation automatique de l’heure du domaine](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29). Pour obtenir des rubriques supplémentaires sur la configuration du service de temps Windows, consultez [Où trouver des informations sur la configuration du service de temps Windows](windows-time-service-top.md).  

> [!CAUTION]  
> Vous ne devez pas utiliser la commande **Net time** pour configurer ou définir l’heure quand le service de temps Windows est en cours d’exécution.  
>
> De même, sur les ordinateurs plus anciens qui exécutent Windows XP ou une version antérieure, la commande **Net time /querysntp** affiche le nom d’un serveur NTP (Network Time Protocol) avec lequel un ordinateur est configuré pour se synchroniser, mais ce serveur NTP n’est utilisé que lorsque le client de temps de l’ordinateur est configuré en NTP ou AllSync. Cette commande a été dépréciée.  

La plupart des ordinateurs membres du domaine ont un type de client de temps NT5DS, ce qui signifie qu’ils synchronisent l’heure à partir de la hiérarchie de domaine. La seule exception classique est le contrôleur de domaine qui fait office de maître d’opérations de l’émulateur du contrôleur de domaine principal du domaine racine de la forêt. Le maître d’opérations de l’émulateur du contrôleur de domaine principal est généralement configuré pour synchroniser l’heure avec une source de temps externe. Pour voir la configuration du client de temps sur un ordinateur (à partir de Windows Server 2008 et Windows Vista), exécutez la commande **W32tm /query /configuration** à partir d’une invite de commandes avec élévation de privilèges, puis lisez la ligne **Type** dans la sortie de la commande. Pour plus d’informations, consultez [Fonctionnement du service de temps Windows](How-the-Windows-Time-Service-Works.md). Par ailleurs, vous pouvez exécuter la commande **reg query HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters** et lire la valeur de **NtpServer** dans la sortie de la commande.  

> [!IMPORTANT]  
> Avant Windows Server 2016, le service W32Time n’était pas conçu pour répondre aux besoins des applications sensibles au facteur temps. Cependant, les mises à jour apportées à Windows Server 2016 vous permettent maintenant d’implémenter une solution avec une précision d’une milliseconde dans votre domaine. Pour plus d’informations, consultez [Précision de l’heure dans Windows Server 2016](accurate-time.md) et [Limites de prise en charge pour configurer le service de temps Windows pour les environnements de haute précision](support-boundary.md).  

## <a name="windows-time-service-tools"></a>Outils du service de temps Windows  

L’outil suivant est associé au service de temps Windows.  

### <a name="w32tmexe-windows-time"></a>W32tm.exe : Horloge Windows  

**Catégorie**  

Cet outil fait partie de l’installation par défaut de Windows (Windows XP et versions ultérieures) et de Windows Server (Windows Server 2003 et versions ultérieures).  

**Compatibilité des versions**  

Cet outil fonctionne sur l’installation par défaut de Windows (Windows XP et versions ultérieures) et de Windows Server (Windows Server 2003 et versions ultérieures).  

Vous pouvez utiliser W32tm.exe pour configurer les paramètres du service de temps Windows et en diagnostiquer les problèmes. W32tm.exe est l’outil en ligne de commande privilégié pour la configuration, la supervision ou le dépannage du service de temps Windows.  

Les tableaux suivants décrivent les paramètres que vous pouvez utiliser avec W32tm.exe.  

**Paramètres principaux de W32tm.exe**  

|Paramètre |Description |
| --- | --- |
|**w32tm /?** |Affiche l’aide de la ligne de commande W32tm. |
|**w32tm /register** |Inscrit le service de temps pour qu’il s’exécute en tant que service et ajoute ses informations de configuration par défaut au Registre. |
|**w32tm /unregister** |Annule l’inscription du service de temps et supprime toutes ses informations de configuration du Registre. |
|**w32tm /monitor [/domain:\<*nom de domaine*>] [/computers:\<*nom*>[,\<*nom*>[,\<*nom*>...]]] [/threads:\<*num*>]** |Supervise le service de temps Windows.<p>**/domain** : spécifie le domaine à superviser. Si aucun nom de domaine n’est fourni ou que ni l’option **/domain** ni l’option **/computers** n’est spécifiée, le domaine par défaut est utilisé. Cette option peut être utilisée plusieurs fois.<p>**/computers** : supervise la liste donnée d’ordinateurs. Les noms d’ordinateur sont séparés par des virgules, sans espace. Si un nom est préfixé avec **\*** , il est traité en tant que contrôleur de domaine principal. Cette option peut être utilisée plusieurs fois.<p>**/threads** : spécifie le nombre d’ordinateurs à analyser simultanément. La valeur par défaut est trois. La plage autorisée est 1 à 50. |
|**w32tm /ntte \<NT *période de temps*>** |Convertit une heure du système Windows NT (mesurée tous les 10<sup>-7</sup> de seconde à partir du 1er janvier 1601 à 0h) dans un format lisible. |
|**w32tm /ntpte \<NTP *période de temps*>** |Convertit une heure NTP (mesurée tous les 2<sup>-7</sup> de seconde à partir du 1er janvier 1900 à 0h) dans un format lisible. |
|**w32tm /resync [/computer:\<*ordinateur*>] [/nowait] [/rediscover] [/soft]** |Indique à un ordinateur qu’il doit resynchroniser son horloge dès que possible, en éliminant toutes les statistiques d’erreurs accumulées.<p>**/computer:\<*ordinateur*>**  : spécifie l’ordinateur qui doit être resynchronisé. Si ce paramètre n’est pas spécifié, l’ordinateur local est resynchronisé.<p>**/nowait** : n’attend pas que la resynchronisation se produise ; passe la main immédiatement. Dans le cas contraire, attendre la fin de la resynchronisation avant de passer la main.<p>**/rediscover** : redétecte la configuration réseau et redécouvre les sources réseau, puis resynchronise.<p>**/soft** : resynchronise en utilisant les statistiques d’erreur existantes. Non utile, fourni à des fins de compatibilité. |
|**w32tm /stripchart /computer:\<*cible*> [/period:\<*actualiser*>] [/dataonly] [/samples:\<*nombre*>] [/rdtsc]** |Affiche un graphique à bandes du décalage entre cet ordinateur et un autre.<p>**/computer:\<*cible*>**  : ordinateur sur lequel mesurer le décalage.<p>**/period:\<*actualiser*>**  : Délai entre les échantillons, en secondes. La valeur par défaut est de deux secondes.<p>**/dataonly** : affiche les données uniquement, sans graphiques.<p>**/samples:\<*nombre*>**  : collecte \<*nombre*> échantillons, puis s’arrête. Si ce paramètre n’est pas spécifié, les échantillons sont collectés jusqu’à ce que l’utilisateur appuie sur **Ctrl + C**.<br/><br/>**/rdtsc** : pour chaque échantillon, cette option affiche les valeurs séparées par des virgules avec les en-têtes **RdtscStart**, **RdtscEnd**, **FileTime**, **RoundtripDelay** et **NtpOffset** au lieu du graphique de texte.<br/><ul><li>**RdtscStart** : valeur [RDTSC (Read Time Stamp Counter)](https://en.wikipedia.org/wiki/Time_Stamp_Counter) collectée juste avant la génération de la demande NTP.</li><li>**RdtscEnd** : valeur RDTSC collectée juste après la réception et le traitement de la réponse NTP.</li><li>**FileTime** : valeur FILETIME locale utilisée dans la demande NTP.</li><li>**RoundtripDelay** : temps écoulé en secondes entre la génération de la demande NTP et le traitement de la réponse NTP reçue, déterminé en fonction des calculs d’aller-retour NTP.</li><li>**NTPOffset** : décalage de temps, en secondes, entre l’ordinateur local et le serveur NTP, déterminé en fonction des calculs de décalage NTP.</li></ul> |
|**w32tm /config [/computer:\<*cible*>] [/update] [/manualpeerlist:\<*pairs*>] [/syncfromflags:\<*source*>] [/LocalClockDispersion:\<*secondes*>] [/reliable:(YES\|NO)] [/largephaseoffset:\<*millisecondes*>]** |**/computer:\<*cible*>**  : ajuste la configuration de \<*cible*>. Si ce paramètre n’est pas spécifié, la valeur par défaut est l’ordinateur local.<p>**/update** : indique au service de temps que la configuration a changé, entraînant l’application des modifications.<p>**/manualpeerlist:\<*pairs*>**  : définit la liste de pairs manuels sur \<*pairs*>, qui est une liste d’adresses DNS et/ou IP délimitée par des espaces. Quand vous spécifiez plusieurs pairs, cette option doit être placée entre guillemets.<p>**/syncfromflags:\<*source*>**  : définit les sources à partir desquelles le client NTP doit se synchroniser. \<*source*> doit être une liste de ces mots clés séparés par des virgules (sans respect de la casse) :<ul><li>**MANUAL** : inclut les pairs de la liste de pairs manuels.</li><li>**DOMHIER** : synchronise à partir d’un contrôleur de domaine de la hiérarchie de domaines.</li></ul>**/LocalClockDispersion:\<*secondes*>**  : configure la précision de l’horloge interne qu’adopte W32Time quand il ne peut pas obtenir l’heure de ses sources configurées.<p>**/reliable:(YES\|NO)**  : définit si cet ordinateur est une source de temps fiable. Ce paramètre est uniquement significatif sur les contrôleurs de domaine.<ul><li>**YES** : cet ordinateur est un service de temps fiable.</li><li>**NO** : cet ordinateur n’est pas un service de temps fiable.</li></ul>**/largephaseoffset:\<*millisecondes*>**  : définit la différence de temps entre l’heure locale et l’heure réseau que W32Time considère comme un pic. |
|**w32tm /tz** |Afficher les paramètres du fuseau horaire actuel. |
|**w32tm /dumpreg [/subkey:\<*clé*>] [/computer:\<*cible*>]** |Afficher les valeurs associées à une clé de Registre donnée.<p>La clé par défaut est **HKLM\System\CurrentControlSet\Services\W32Time** (clé racine du service de temps).<p>**/subkey:\<*clé*>**  : affiche les valeurs associées à la sous-clé <key> de la clé par défaut.<p>**/computer:\<*cible*>**  : interroge les paramètres de Registre pour l’ordinateur \<*cible*> |
|**w32tm /query [/computer:\<*cible*>] {/source \| /configuration \| /peers \| /status} [/verbose]** |Affiche les informations du service de temps Windows d’un ordinateur. Ce paramètre a été mis à disposition pour la première fois dans le client du service de temps Windows de Windows Vista et Windows Server 2008.<p>**/computer:\<*cible*>**  : interroge \<*cible*> pour en obtenir les informations. Si ce paramètre n’est pas spécifié, la valeur par défaut est l’ordinateur local.<p>**/source** : affiche la source de temps.<p>**/configuration** : affiche la configuration de l’exécution et l’origine du paramètre. En mode détaillé, afficher également le paramètre non défini ou inutilisé.<p>**/peers** : affiche la liste de pairs et leur état.<p>**/status** : affiche l’état du service de temps Windows.<p>**/verbose** : active le mode détaillé pour afficher plus d’informations. |
|**w32tm /debug {/disable \| {/enable /file:\<*nom*> /size:/<*octets*> /entries:\<*valeur*> [/truncate]}}** |Active ou désactive le journal privé du service de temps Windows de l’ordinateur local. Ce paramètre a été mis à disposition pour la première fois dans le client du service de temps Windows de Windows Vista et Windows Server 2008.<p>**/disable** : désactive le journal privé.<p>**/enable** : active le journal privé.<ul><li>**file:\<*nom*>**  : spécifie le nom de fichier absolu.</li><li>**size:\<*octets*>**  : spécifie la taille maximale de la journalisation circulaire.</li><li>**entries:\<*valeur*>**  : contient une liste d’indicateurs, spécifiés par un nombre et séparés par des virgules, qui spécifient les types d’informations à journaliser. Les nombres valides sont compris entre 0 et 300. Une plage de nombres est valide au même titre que les nombres uniques ; vous pouvez, par exemple, indiquer 0-100,103,106. La valeur 0-300 correspond à la journalisation de toutes les informations.</li></ul>**/truncate** : tronque le fichier s’il existe. |

Pour plus d'informations sur **W32tm.exe**, consultez l’[aide Windows](https://support.microsoft.com/hub/4338813/windows-help?os=windows-10).  

**Exemples**

Si vous souhaitez définir le client du service temps Windows local pour le faire pointer vers deux serveurs de temps différents, un nommé ntpserver.contoso.com et l’autre nommé clock.adatum.com, tapez la commande suivante sur la ligne de commande, puis appuyez sur ENTRÉE :

```cmd
w32tm /config /manualpeerlist:"ntpserver.contoso.com clock.adatum.com" /syncfromflags:manual /update
```

Pour obtenir une liste de serveurs NTP valides disponibles sur Internet pour une synchronisation d’heure externe, consultez [Liste de serveurs de temps SNTP (Simple Network Time Protocol) disponibles sur Internet](https://go.microsoft.com/fwlink/?linkid=60401).

Si vous souhaitez vérifier la configuration du client du service de temps Windows à partir d’un ordinateur client Windows ayant pour nom d’hôte CONTOSOW1, exécutez la commande suivante :

```cmd
W32tm /query /computer:contosoW1 /configuration
```

La sortie de cette commande est une liste de paramètres de configuration définis pour le client du service de temps Windows.

## <a name="using-group-policy-to-configure-the-windows-time-service"></a>Configurer le service de temps Windows à l’aide de la stratégie de groupe

Le service de temps Windows stocke un certain nombre de propriétés de configuration sous forme d’entrées de Registre. Vous pouvez utiliser des objets de stratégie de groupe (GPO) pour configurer la plupart de ces informations. Par exemple, vous pouvez utiliser des GPO pour configurer un ordinateur en tant que NTPServer ou NTPClient, configurer le mécanisme de synchronisation de l’heure ou configurer un ordinateur en tant que source de temps fiable.  

> [!NOTE]  
> Les paramètres de stratégie de groupe du service de temps Windows peuvent être configurés sur des contrôleurs de domaine Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 et Windows Server 2008 R2 et peuvent être appliqués uniquement aux ordinateurs exécutant Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 et Windows Server 2008 R2.  

Windows stocke les informations de stratégie du service de temps Windows dans le fichier de modèle d’administration W32Time.admx, sous **Configuration ordinateur\Modèles d’administration\Système\Service de temps Windows**. Les informations de configuration définies par les stratégies sont stockées dans le Registre, et ces entrées de Registre sont utilisées pour configurer les entrées de Registre du service de temps Windows. Par conséquent, les valeurs définies par la stratégie de groupe remplacent celles qui préexistaient dans la section du service de temps Windows du Registre.

> [!IMPORTANT]  
> Certains paramètres GPO prédéfinis diffèrent des entrées de Registre par défaut correspondantes. Si vous envisagez d’utiliser un GPO pour configurer un paramètre du service de temps Windows, veillez à consulter [Les valeurs prédéfinies pour les paramètres de stratégie de groupe du service de temps Windows sont différentes des entrées de Registre du service de temps Windows correspondantes dans Windows Server 2003](https://go.microsoft.com/fwlink/?LinkId=186066). Ce problème concerne Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2 et Windows Server 2003.

Par exemple, supposez que vous modifiez des paramètres de la stratégie **Configurer le client NTP Windows**.

Vos modifications sont stockées à l’emplacement suivant dans le modèle d’administration :

> **Configuration ordinateur\Modèles d’administration\Système\Service de temps Windows\Fournisseurs de temps\ Configurer le client NTP Windows**

Windows charge ces paramètres dans la zone stratégie du Registre, sous la sous-clé suivante :

> **HKLM\Software\Policies\Microsoft\W32time\TimeProviders\NtpClient**

Windows utilise ensuite les paramètres de stratégie pour configurer les entrées de Registre associées du service de temps Windows sous la sous-clé suivante :

> **HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Time Providers\NTPClient\\**

Le tableau suivant liste les stratégies que vous pouvez configurer pour le service de temps Windows ainsi que les sous-clés de Registre qui sont affectées par ces stratégies.

> [!NOTE]  
> Quand vous supprimez un paramètre de stratégie de groupe, Windows supprime l’entrée correspondante de la zone stratégie du Registre.

|Stratégie<sup>1</sup> |Emplacements dans le Registre<sup>2,</sup> <sup>3</sup> |
| --- | --- |
|Paramètres de configuration globale |W32Time<br />W32Time\Config<br />W32Time\Parameters |
|Fournisseurs de temps\Configurer le client NTP Windows |W32Time\TimeProviders\NtpClient |
|Fournisseurs de temps\Activer le client NTP Windows |W32Time\TimeProviders\NtpClient |
|Fournisseurs de temps\Activer le serveur NTP Windows |W32Time\TimeProviders\NtpServer |

> <sup>1</sup> Chemin de catégorie : **Configuration ordinateur\Modèles d’administration\Système\Service de temps Windows**  
> <sup>2</sup> Sous-clé : **HKLM\SOFTWARE\Policies\Microsoft**  
> <sup>3</sup> Sous-clé : **HKLM\SYSTEM\CurrentControlSet\Services**

## <a name="enabling-w32time-logging"></a>Activation de la journalisation de W32Time

Les trois entrées de Registre suivantes ne font pas partie de la configuration par défaut de W32Time, mais peuvent être ajoutées au Registre pour obtenir des fonctionnalités de journalisation accrues. Vous pouvez modifier les informations consignées dans le journal des événements système en changeant la valeur du paramètre **EventLogFlags** dans l’Éditeur d’objets de stratégie de groupe. Par défaut, le service de temps journalise un événement chaque fois qu’il bascule vers une nouvelle source de temps.

Pour activer la journalisation de W32Time, ajoutez les entrées de Registre suivantes :  

|Entrée de Registre |Versions |Description |
| --- | --- | --- |
|**FileLogEntries** |Toutes les versions |Détermine le nombre d’entrées créées dans le fichier journal du service de temps Windows. La valeur par défaut est none : aucune activité du service de temps Windows n’est journalisée. La valeur indiquée doit être comprise entre **0** et **300**. Cette valeur n’affecte pas les entrées de journal des événements normalement créées par le service de temps Windows. |
|**FileLogName** |Toutes les versions |Détermine l’emplacement et le nom de fichier du journal du service de temps Windows. La valeur par défaut est vide et ne doit pas être modifiée, sauf si **FileLogEntries** est changé. Une valeur valide est un chemin complet et un nom de fichier que le service de temps Windows utilise pour créer le fichier journal. Cette valeur n’affecte pas les entrées de journal des événements normalement créées par le service de temps Windows. |
|**FileLogSize** |Toutes les versions |Détermine le comportement de journalisation circulaire des fichiers journaux du service de temps Windows. Quand **FileLogEntries** et **FileLogName** sont définis, l’entrée définit la taille, en octets, que peut atteindre le fichier journal avant que les entrées de journal les plus anciennes ne soient remplacées par de nouvelles entrées. Utilisez **1000000** ou une valeur supérieure pour ce paramètre. Cette valeur n’affecte pas les entrées de journal des événements normalement créées par le service de temps Windows. |

## <a name="configuring-how-windows-time-service-resets-the-computer-clock"></a>Configuration de la façon dont le service de temps Windows réinitialise l’horloge de l’ordinateur

Pour que W32Time définisse progressivement l’horloge de l’ordinateur, le décalage doit être inférieur à la valeur **MaxAllowedPhaseOffset** et répondre à l’équation suivante en même temps :  

- Windows Server 2016 et versions ultérieures :

  > |**CurrentTimeOffset**| &divide; (16 &times; **PhaseCorrectRate** &times; **pollIntervalInSeconds**) &le; **SystemClockRate** &divide; 2  

- Windows Server 2012 R2 et versions antérieures :

  > |**CurrentTimeOffset**| &divide; (**PhaseCorrectRate** &times; **UpdateInterval**) &le; **SystemClockRate** &divide; 2  

La valeur **CurrentTimeOffset** est mesurée en cycles d’horloge, où 1 ms représente 10 000 cycles d’horloge sur un système Windows.  

**SystemClockRate** et **PhaseCorrectRate** sont également mesurés en cycles d’horloge. Pour récupérer la valeur **SystemClockRate**, vous pouvez utiliser la commande suivante et convertir le résultat en cycles d’horloge en utilisant la formule : *secondes* &times; 1 000 &times; 10 000 :  

```cmd
W32tm /query /status /verbose  
ClockRate: 0.0156000s  
```  

**SystemclockRate** est la fréquence de l’horloge sur le système. En utilisant par exemple 156 000 secondes, la valeur **SystemclockRate** est : 0,0156000 &times; 1 000 &times; 10 000, soit 156 000 cycles d’horloge.  

La valeur **MaxAllowedPhaseOffset** est également mesurée en secondes. Pour la convertir en cycles d’horloge, effectuez la multiplication **MaxAllowedPhaseOffset** &times; 1 000 &times; 10 000.  

Les exemples suivants montrent comment appliquer ces calculs quand vous utilisez Windows Server 2012 R2 ou une version antérieure.

**Exemple 1 : l’heure diffère de quatre minutes**

Votre heure est 11:05 minutes et l’échantillon d’heure que vous avez reçu d’un pair et que vous pensez correct est 11:09.

> **PhaseCorrectRate** = 1  
>  
> **UpdateInterval** = 30 000 cycles d’horloge  
>  
> **SystemClockRate** = 156 000 cycles d’horloge  
>  
> **MaxAllowedPhaseOffset** = 10 min = 600 secondes = 600 &times; 1 000 &times; 10 000 = 6 000 000 000 cycles d’horloge  
>  
> |**CurrentTimeOffset**| = 4 min = 4 &times; 60 &times; 1 000 &times; 10 000 = 2 400 000 000 cycles d’horloges  
>  
> Est-ce que **CurrentTimeOffset** &le; **MaxAllowedPhaseOffset** ?  
>  
> 2 400 000 000 &le; 6 000 000 000 : TRUE  

ET l’équation ci-dessus est-elle satisfaite ?

> (|**CurrentTimeOffset**| &divide; (**PhaseCorrectRate** &times; **UpdateInterval**) &le; **SystemClockRate** &divide; 2)  

Est-ce que 2 400 000 000 /(30 000 &times; 1) &le; 156 000 &divide; 2  

80 000 &le; 78 000 : FALSE  

Par conséquent, W32tm redéfinit l’horloge immédiatement.  

> [!NOTE]  
> Dans ce cas, si vous souhaitez redéfinir l’horloge lentement, vous devez aussi ajuster les valeurs de **PhaseCorrectRate** ou **UpdateInterval** dans le Registre afin que le résultat de l’équation soit **TRUE**.  

**Exemple 2 : l’heure diffère de trois minutes**

> **PhaseCorrectRate** = 1  
> 
> **UpdateInterval** = 30 000 cycles d’horloge  
> 
> SystemClockRate = 156 000 cycles d’horloge  
> 
> **MaxAllowedPhaseOffset** = 10 min = 600 secondes = 600 &times; 1 000 &times; 10 000 = 6 000 000 000 cycles d’horloge  
> 
> |**CurrentTimeOffset**| = 3 min = 3 &times; 60 &times; 1 000 &times; 10 000 = 1 800 000 000 cycles d’horloges  
> 
> Est-ce que |**CurrentTimeOffset**| &le; **MaxAllowedPhaseOffset** ?  
> 
> 1 800 000 000 &le; 6 000 000 000 : TRUE  

ET l’équation ci-dessus est-elle satisfaite ?

> (|**CurrentTimeOffset**| &divide; (**PhaseCorrectRate** &times; **UpdateInterval**) &le; **SystemClockRate** &divide; 2)  
> 
> Est-ce que 3 min &times; (1 800 000 000) &divide; (30 000 &times; 1) &le; 156 000 &divide; 2  
> 
> Est-ce que 60 000 &le; 78 000 : TRUE  

Dans ce cas, l’horloge est redéfinie lentement.  

## <a name="reference-windows-time-service-registry-entries"></a>Référence : Entrée de Registre du service de temps Windows

> [!WARNING]  
> Les informations sur ces entrées de Registre sont fournies à titre de référence et peuvent être utilisées pour résoudre les problèmes ou vérifier que les paramètres nécessaires sont appliqués. La plupart des valeurs de la section W32Time du Registre sont utilisées en interne par W32Time pour stocker des informations. Ne modifiez pas ces valeurs manuellement. Les modifications apportées au Registre ne sont pas validées par l’Éditeur du Registre ou par Windows avant d’être appliquées. Si le Registre contient des valeurs non valides, Windows peut rencontrer des erreurs irrécupérables.  

Le service de temps Windows stocke les informations sous les sous-clés de Registre suivantes :

- [**HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Config**](#config)
- [**HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters**](#parameters)
- [**HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient**](#ntpclient)
- [**HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpServer**](#ntpserver)

Par ailleurs, vous pouvez [ajouter des entrées afin de configurer les journaux](#enabling-w32time-logging) à des fins de dépannage.

Dans les tableaux suivants, « Toutes les versions » fait référence aux versions de Windows, à savoir Windows 7, Windows 8, Windows 10, Windows Server 2008 et Windows Server 2008 R2, Windows Server 2012 et Windows Server 2012 R2, Windows Server 2016 et Windows Server 2019. Certaines entrées sont uniquement disponibles sur les versions ultérieures de Windows.

> [!NOTE]  
> Certains paramètres du Registre sont mesurés en cycles d’horloge et d’autres en secondes. Pour convertir l’heure en secondes à partir de cycles d’horloge, utilisez ces facteurs de conversion :  
> - 1 minute = 60 s  
> - 1 s = 1000 ms  
> - 1 ms = 10 000 cycles d’horloge sur un système Windows, comme décrit dans [Propriété DateTime.Ticks](https://docs.microsoft.com/dotnet/api/system.datetime.ticks).  
>  
> Par exemple, 5 minutes deviennent 5 &times; 60 &times; 1 000 &times; 10 000 = 3  000 000 000 cycles d’horloge.  

### <a name="hklmsystemcurrentcontrolsetservicesw32timeconfig-subkey-entries"></a><a id="config"></a>Entrées de la sous-clé « HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Config »

|Entrée de Registre |Versions |Description |
| --- | --- | --- |
|**AnnounceFlags** |Toutes les versions |Détermine si cet ordinateur est marqué comme étant un serveur de temps fiable. Un ordinateur n’est pas marqué comme fiable, sauf s’il est également marqué comme serveur de temps.<ul><li>**0x00**. Pas un serveur de temps</li><li>**0x01**. Toujours un serveur de temps</li><li>**0x02**. Serveur de temps automatique</li><li>**0x04**. Serveur de temps toujours fiable</li><li>**0x08**. Serveur de temps fiable automatique</li></ul><br />La valeur par défaut pour les membres du domaine est **10**. La valeur par défaut pour les serveurs et les clients autonomes est **10**. |
|**ChainDisable** | |Détermine si le mécanisme de chaînage est désactivé ou non. Si le chaînage est désactivé (défini sur 0), un contrôleur de domaine en lecture seule (RODC) peut se synchroniser avec n’importe quel contrôleur de domaine, mais les hôtes dont le mot de passe n’est pas mis en cache sur le RODC ne peuvent pas se synchroniser avec le RODC. Il s’agit d’un paramètre booléen dont la valeur par défaut est **0**.|
|**ChainEntryTimeout** | |Spécifie la durée maximale qu’une entrée peut rester dans la table de chaînage avant d’être considérée comme ayant expiré. Les entrées ayant expiré peuvent être supprimées au moment où la demande ou la réponse suivante est traitée. La valeur par défaut est **16** (secondes). |
|**ChainLoggingRate** | |Détermine la fréquence à laquelle un événement qui indique le nombre de tentatives de chaînage réussies et avortées est consigné dans le journal Système de l’Observateur d’événements. La valeur par défaut est **30** (minutes). |
|**ChainMaxEntries** | |Détermine le nombre maximal d’entrées autorisées dans la table de chaînage. Si la table de chaînage est pleine et qu’aucune entrée expirée ne peut être supprimée, toutes les demandes entrantes sont ignorées. La valeur par défaut est **128** (entrées). |
|**ChainMaxHostEntries** | |Détermine le nombre maximal d’entrées autorisées dans la table de chaînage pour un hôte déterminé. La valeur par défaut est **4** (entrées). |
|**ClockAdjustmentAuditLimit** |Windows Server 2016 version 1709 et ultérieures ; Windows 10 version 1709 et ultérieures |Spécifie les plus petits ajustements de l’horloge locale qui peuvent être consignés dans le journal des événements du service W32Time sur l’ordinateur cible. La valeur par défaut est **800** (parties par million – PPM). |
|**ClockHoldoverPeriod** |Windows Server 2016 version 1709 et ultérieures ; Windows 10 version 1709 et ultérieures |Indique le nombre maximal de secondes pendant lesquelles une horloge système peut théoriquement conserver sa précision sans se synchroniser avec une source de temps. Si, à l’issue de cette période, W32time n’obtient pas de nouveaux échantillons de ses fournisseurs d’entrée, W32time lance une nouvelle détection des sources de temps. Default : 7 800 secondes. |
|**EventLogFlags** |Toutes les versions |Détermine les événements qui sont journalisés par le service de temps.<ul><li>**0x1**. Décalage de temps</li><li>**0x2**. Changement de la source</li></ul>La valeur par défaut sur les membres du domaine est **2**. La valeur par défaut sur les serveurs et les clients autonomes est **2**. |
|**FrequencyCorrectRate** |Toutes les versions |Détermine la fréquence de correction de l’horloge. Si cette valeur est trop petite, l’horloge est instable et fait l’objet d’une correction excessive. Si la valeur est trop grande, la synchronisation de l’horloge prend beaucoup de temps. La valeur par défaut sur les membres du domaine est **4**. La valeur par défaut sur les serveurs et les clients autonomes est **4**.<p>**Remarque** <br />La valeur zéro n’est pas valide pour l’entrée de Registre **FrequencyCorrectRate**. Sur les ordinateurs Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 et Windows Server 2008 R2, si la valeur est définie sur **0**, le service de temps Windows la remplace automatiquement par **1**. |
|**HoldPeriod** |Toutes les versions |Détermine la durée pendant laquelle la détection des pics est désactivée afin de mettre rapidement l’horloge locale en mode synchronisation. Un pic est un échantillon de temps qui indique que le temps est désactivé pendant un certain nombre de secondes, et est généralement reçu après que des échantillons de temps corrects ont été retournés de façon cohérente. La valeur par défaut sur les membres du domaine est **5**. La valeur par défaut sur les serveurs et les clients autonomes est **5**. |
|**LargePhaseOffset** |Toutes les versions |Indique qu’un décalage de temps supérieur ou égal à cette valeur en 10<sup>-7</sup> de seconde est considéré comme un pic. Une interruption du réseau, telle qu’une grande quantité de trafic, peut entraîner un pic. Un pic est ignoré à moins qu’il persiste pendant une longue période de temps. La valeur par défaut sur les membres du domaine est **50000000**. La valeur par défaut sur les serveurs et les clients autonomes est **50000000**. |
|**LastClockRate** |Toutes les versions |Géré par W32Time. Elle contient des données réservées utilisées par le système d’exploitation Windows ; toute modification apportée à ce paramètre peut entraîner des résultats imprévisibles. La valeur par défaut sur les membres du domaine est **156250**. La valeur par défaut sur les serveurs et les clients autonomes est **156250**. |
|**LocalClockDispersion** |Toutes les versions |Détermine la dispersion (en secondes) que vous devez supposer quand la seule source de temps est l’horloge CMOS intégrée. La valeur par défaut sur les membres du domaine est **10**. La valeur par défaut sur les serveurs et les clients autonomes est **10**. |
|**MaxAllowedPhaseOffset** |Toutes les versions |Spécifie le décalage maximal (en secondes) pendant lequel W32Time tente d’ajuster l’horloge de l’ordinateur en utilisant la fréquence d’horloge. Quand le décalage dépasse cette fréquence, W32Time définit l’horloge de l’ordinateur directement. La valeur par défaut pour les membres du domaine est **300**. La valeur par défaut pour les serveurs et les clients autonomes est **1**. Pour plus d’informations, consultez [Configuration de la façon dont le service de temps Windows réinitialise l’horloge de l’ordinateur](#configuring-how-windows-time-service-resets-the-computer-clock). |
|**MaxClockRate** |Toutes les versions |Géré par W32Time. Elle contient des données réservées utilisées par le système d’exploitation Windows ; toute modification apportée à ce paramètre peut entraîner des résultats imprévisibles. La valeur par défaut pour les membres du domaine est **155860**. La valeur par défaut pour les serveurs et les clients autonomes est **155860**. |
|**MaxNegPhaseCorrection** |Toutes les versions |Spécifie la plus grande correction de temps négative en secondes qu’effectue le service. Si le service détermine qu’une modification supérieure à celle-ci est nécessaire, il journalise un événement à la place.<p>**Remarque**<br />La valeur **0xFFFFFFFF** est un cas spécial. Cette valeur signifie que le service corrige toujours l’heure.<p>La valeur par défaut pour les membres du domaine est **0xFFFFFFFF**. La valeur par défaut pour les serveurs et les clients autonomes est **54 000** (15 heures).|
|**MaxPollInterval** |Toutes les versions |Spécifie le plus grand intervalle, en secondes log2, autorisé pour l’intervalle d’interrogation du système. Notez que, si un système doit effectuer une interrogation en fonction de l’intervalle planifié, un fournisseur peut refuser de produire des échantillons quand il y est invité. La valeur par défaut pour les contrôleurs de domaine est **10**. La valeur par défaut pour les membres du domaine est **15**. La valeur par défaut pour les serveurs et les clients autonomes est **15**. |
|**MaxPosPhaseCorrection** |Toutes les versions |Spécifie la plus grande correction de temps positive en secondes qu’effectue le service. Si le service détermine qu’une modification supérieure à celle-ci est nécessaire, il journalise un événement à la place.<p>**Remarque**<br />La valeur **0xFFFFFFFF** est un cas spécial. Cette valeur signifie que le service corrige toujours l’heure.<p>La valeur par défaut pour les membres du domaine est **0xFFFFFFFF**. La valeur par défaut pour les serveurs et les clients autonomes est **54 000** (15 heures). |
|**MinClockRate** |Toutes les versions |Géré par W32Time. Elle contient des données réservées utilisées par le système d’exploitation Windows ; toute modification apportée à ce paramètre peut entraîner des résultats imprévisibles. La valeur par défaut pour les membres du domaine est **155860**. La valeur par défaut pour les serveurs et les clients autonomes est **155860**. |
|**MinPollInterval** |Toutes les versions |Spécifie le plus petit intervalle, en secondes log2, autorisé pour l’intervalle d’interrogation du système. Notez que bien qu’un système ne demande pas d’échantillons plus souvent, un fournisseur peut produire des échantillons à d’autres moments que l’intervalle planifié. La valeur par défaut pour les contrôleurs de domaine est **6**. La valeur par défaut pour les membres du domaine est **10**. La valeur par défaut pour les serveurs et les clients autonomes est **10**. |
|**PhaseCorrectRate** |Toutes les versions |Détermine la fréquence de correction de l’erreur de phase. La spécification d’une petite valeur corrige rapidement l’erreur de phase, mais peut entraîner l’instabilité de l’horloge. Si la valeur est trop grande, il faut plus de temps pour corriger l’erreur de phase.<p>La valeur par défaut sur les membres du domaine est **1**. La valeur par défaut sur les serveurs et les clients autonomes est **7**.<p>**Remarque**<br />La valeur zéro n’est pas valide pour l’entrée de Registre **PhaseCorrectRate**. Sur les ordinateurs Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 et Windows Server 2008 R2, si la valeur est définie sur **0**, le service de temps Windows la remplace automatiquement par **1**. |
|**PollAdjustFactor** |Toutes les versions |Détermine la décision d’augmenter ou de diminuer l’intervalle d’interrogation pour le système. Plus la valeur est élevée, plus la quantité d’erreur qui entraîne la diminution de l’intervalle d’interrogation est faible. La valeur par défaut sur les membres du domaine est **5**. La valeur par défaut sur les serveurs et les clients autonomes est **5**. |
|**RequireSecureTimeSyncRequests** |Windows 8 et versions ultérieures |Détermine si le contrôleur de domaine répond ou non aux demandes de synchronisation de l’heure qui utilisent d’anciens protocoles d’authentification. Si cette option est activée (définie sur **1**), le contrôleur de domaine ne répond pas aux demandes utilisant de tels protocoles. Il s’agit d’un paramètre booléen dont la valeur par défaut est **0**. |
|**SpikeWatchPeriod** |Toutes les versions |Spécifie la durée pendant laquelle un décalage suspect doit être conservé avant qu’il soit accepté comme correct (en secondes). La valeur par défaut sur les membres du domaine est **900**. La valeur par défaut sur les stations de travail et les clients autonomes est **900**. |
|**TimeJumpAuditOffset** |Toutes les versions |Entier non signé qui indique le seuil d’audit de décalage de temps, en secondes. Si le service de temps ajuste l’horloge locale en définissant l’horloge directement et que la correction de l’heure est supérieure à cette valeur, le service de temps journalise un événement d’audit. |
|**UpdateInterval** |Toutes les versions |Spécifie le nombre de cycles d’horloge entre les ajustements de correction de phase. La valeur par défaut pour les contrôleurs de domaine est **100**. La valeur par défaut pour les membres du domaine est **30 000**. La valeur par défaut pour les serveurs et les clients autonomes est **360 000**.<p>**Remarque**<br />La valeur zéro n’est pas valide pour l’entrée de Registre **UpdateInterval**. Sur les ordinateurs exécutant Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 et Windows Server 2008 R2, si la valeur est définie sur **0**, le service de temps Windows la remplace automatiquement par **1**.|
|**UtilizeSslTimeData** |Versions de Windows postérieures à Windows 10 build 1511 |La valeur **1** indique que W32Time utilise plusieurs horodatages SSL pour amorcer une horloge qui est grossièrement inexacte. |

### <a name="hklmsystemcurrentcontrolsetservicesw32timeparameters-subkey-entries"></a><a id="parameters"></a>Entrées de la sous-clé « HKLM\SYSTEM\CurrentControlSet\Services\W32Time\Parameters »

| Entrée de Registre | Versions | Description |
| --- | --- | --- |
|**AllowNonstandardModeCombinations** |Toutes les versions |Indique que les combinaisons de mode non standard sont autorisées dans la synchronisation entre pairs. La valeur par défaut pour les membres du domaine est **1**. La valeur par défaut pour les serveurs et les clients autonomes est **1**. |
|**NtpServer** |Toutes les versions |Spécifie une liste délimitée par des espaces comportant les pairs à partir desquels un ordinateur obtient des horodatages, constitués d’un ou plusieurs noms DNS ou adresses IP par ligne. Chaque nom DNS ou adresse IP listé doit être unique. Les ordinateurs connectés à un domaine doivent se synchroniser avec une source de temps plus fiable, telle que l’horloge des États-Unis officielle.  <ul><li>0x01 SpecialInterval </li><li>0x02 UseAsFallbackOnly</li><li>0x04 SymmetricActive : pour plus d’informations sur ce mode, consultez [Windows Time Server: 3.3 Modes of Operation](https://go.microsoft.com/fwlink/?LinkId=208012).</li><li>0x08 Client</li></ul><br />Il n’existe pas de valeur par défaut pour cette entrée de Registre sur les membres du domaine. La valeur par défaut sur les serveurs et les clients autonomes est time.windows.com,0x1.<p>**Remarque**<br />Pour plus d’informations sur les serveurs NTP disponibles, consultez l’article 262680 de la Base de connaissances, [Liste des serveurs de temps SNTP (Simple Network Time Protocol) disponibles sur Internet](https://support.microsoft.com/help/262680/a-list-of-the-simple-network-time-protocol-sntp-time-servers-that-are). |
|**ServiceDll** |Toutes les versions |Géré par W32Time. Elle contient des données réservées utilisées par le système d’exploitation Windows ; toute modification apportée à ce paramètre peut entraîner des résultats imprévisibles. L’emplacement par défaut de cette DLL à la fois sur les membres du domaine et sur les serveurs et les clients autonomes est %windir%\System32\W32Time.dll. |
|**ServiceMain** |Toutes les versions |Géré par W32Time. Elle contient des données réservées utilisées par le système d’exploitation Windows ; toute modification apportée à ce paramètre peut entraîner des résultats imprévisibles. La valeur par défaut sur les membres du domaine est **SvchostEntry_W32Time**. La valeur par défaut sur les serveurs et les clients autonomes est **SvchostEntry_W32Time**. |
|**Type** |Toutes les versions |Indique les pairs à partir desquels la synchronisation doit être acceptée :  <ul><li>**NoSync**. Le service de temps ne se synchronise pas avec d’autres sources.</li><li>**NTP**. Le service de temps se synchronise à partir des serveurs spécifiés dans l’entrée de Registre **NtpServer** .</li><li>**NT5DS**. Le service de temps se synchronise à partir de la hiérarchie du domaine.  </li><li>**AllSync**. Le service de temps utilise tous les mécanismes de synchronisation disponibles.  </li></ul>La valeur par défaut sur les membres du domaine est **NT5DS**. La valeur par défaut sur les serveurs et les clients autonomes est **NTP**. |

### <a name="hklmsystemcurrentcontrolsetservicesw32timetimeprovidersntpclient-subkey-entries"></a><a id="ntpclient"></a>Entrées de la sous-clé « HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpClient »

|Entrée de Registre |Version |Description |
| --- | --- | --- |
|**AllowNonstandardModeCombinations** |Toutes les versions |Indique que les combinaisons de mode non standard sont autorisées dans la synchronisation entre pairs. La valeur par défaut pour les membres du domaine est **1**. La valeur par défaut pour les serveurs et les clients autonomes est **1**.|
|**CompatibilityFlags** |Toutes les versions |Spécifie les valeurs et indicateurs de compatibilité suivants :<ul><li>**0x00000001** - DispersionInvalid</li><li>**0x00000002** - IgnoreFutureRefTimeStamp</li><li>**0x80000000** - AutodetectWin2K</li><li>**0x40000000** - AutodetectWin2KStage2</li></ul>La valeur par défaut pour les membres du domaine est **0x80000000**. La valeur par défaut pour les serveurs et les clients autonomes est **0x80000000**. |
|**CrossSiteSyncFlags** |Toutes les versions |Détermine si le service choisit des partenaires de synchronisation en dehors du domaine de l’ordinateur. Les options et les valeurs sont les suivantes :<ul><li>**0** - Aucun</li><li>**1** - Contrôleur de domaine principal uniquement</li><li>**2** - Tous</li></ul>Cette valeur est ignorée si la valeur NT5DS n’est pas définie. La valeur par défaut pour les membres du domaine est **2**. La valeur par défaut pour les serveurs et les clients autonomes est **2**. |
|**DllName** |Toutes les versions |Spécifie l’emplacement de la DLL pour le fournisseur de temps.<p>L’emplacement par défaut de cette DLL à la fois sur les membres du domaine et sur les serveurs et les clients autonomes est **%windir%\System32\W32Time.dll**. |
|**Activé** |Toutes les versions |Indique si le fournisseur NtpClient est activé dans le service de temps actuel.<ul><li>**1** - Oui</li><li>**0** - Non</li></ul>La valeur par défaut sur les membres du domaine est **1**. La valeur par défaut sur les serveurs et les clients autonomes est **1**.|
|**EventLogFlags** |Toutes les versions |Spécifie les événements journalisés par le service de temps Windows.<ul><li>**0x1** - Modifications de l’accessibilité</li><li>**0x2** - Grand biais d’échantillon (concerne uniquement Windows Server 2003, Windows Server 2003 R2, Windows Server 2008 et Windows Server 2008 R2)</li></ul>La valeur par défaut sur les membres du domaine est **0x1**. La valeur par défaut sur les serveurs et les clients autonomes est **0x1**.|
|**InputProvider** |Toutes les versions |Indique si NtpClient est activé en tant qu’InputProvider, qui obtient des informations de temps auprès de NtpServer. NtpServer est un serveur de temps qui répond aux demandes de temps du client sur le réseau en retournant des échantillons de temps utiles pour la synchronisation de l’horloge locale. <ul><li>**1** - Oui</li><li>**0** - Non</li></ul>Valeur par défaut pour les membres du domaine et les clients autonomes est **1**. |
|**LargeSampleSkew** |Toutes les versions |Spécifie le grand biais d’échantillon pour la journalisation, en secondes. Pour se conformer aux spécifications SEC (Security and Exchange Commission), cette valeur doit être définie sur trois secondes. Les événements sont journalisés pour ce paramètre uniquement quand **EventLogFlags** est configuré de manière explicite pour un grand biais d’échantillon 0X2. La valeur par défaut sur les membres du domaine est 3. La valeur par défaut sur les serveurs et les clients autonomes est 3. |
|**ResolvePeerBackOffMaxTimes** |Toutes les versions |Spécifie le nombre maximal de fois que l’intervalle d’attente doit être doublé quand des tentatives répétées de localisation d’un pair avec lequel effectuer une synchronisation échouent. La valeur zéro signifie que l’intervalle d’attente est toujours le minimum. La valeur par défaut sur les membres du domaine est **7**. La valeur par défaut sur les serveurs et les clients autonomes est **7**. |
|**ResolvePeerBackoffMinutes** |Toutes les versions |Spécifie l’intervalle d’attente initial, en minutes, avant une tentative de localisation d’un pair avec lequel effectuer une synchronisation. La valeur par défaut sur les membres du domaine est **15**. La valeur par défaut sur les serveurs et les clients autonomes est **15**.  |
|**SpecialPollInterval** |Toutes les versions |Spécifie l’intervalle d’interrogation spécial, en secondes, pour les pairs manuels. Quand l’indicateur **SpecialInterval** 0x1 est activé, W32Time utilise cet intervalle d’interrogation au lieu d’un intervalle d’interrogation déterminé par le système d’exploitation. La valeur par défaut sur les membres du domaine est **3 600**. La valeur par défaut sur les serveurs et les clients autonomes est **604 800**.<br/><br/>Nouveauté de la build 1702 : **SpecialPollInterval** fait partie des valeurs de Registre de configuration **MinPollInterval** et **MaxPollInterval**.|
|**SpecialPollTimeRemaining** |Toutes les versions |Géré par W32Time. Elle contient des données réservées qui sont utilisées par le système d’exploitation Windows. Elle spécifie la durée, en secondes, avant que W32Time se resynchronise après le redémarrage de l’ordinateur. Toute modification apportée à ce paramètre peut engendrer des résultats imprévisibles. La valeur par défaut, tant sur les membres du domaine que sur les serveurs et clients autonomes, est laissée vide. |

### <a name="hklmsystemcurrentcontrolsetservicesw32timetimeprovidersntpserver-subkey-entries"></a><a id="ntpserver"></a>Entrées de la sous-clé « HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\NtpServer »

|Entrée de Registre |Versions |Description |
| --- | --- | --- |
|**AllowNonstandardModeCombinations** |Toutes les versions |Indique que les combinaisons de mode non standard sont autorisées dans la synchronisation entre les clients et les serveurs. La valeur par défaut pour les membres du domaine est **1**. La valeur par défaut pour les serveurs et les clients autonomes est **1**. |
|**DllName** |Toutes les versions |Spécifie l’emplacement de la DLL pour le fournisseur de temps. L’emplacement par défaut de cette DLL à la fois sur les membres du domaine et sur les serveurs et les clients autonomes est **%windir%\System32\W32Time.dll**.  |
|**Activé** |Toutes les versions |Indique si le fournisseur NtpServer est activé dans le service de temps actuel. <ul><li>**1** - Oui</li><li>**0** - Non</li></ul>La valeur par défaut sur les membres du domaine est **1**. La valeur par défaut sur les serveurs et les clients autonomes est **1**. |
|**InputProvider** |Toutes les versions |Indique si NtpClient est activé en tant qu’InputProvider, qui obtient des informations de temps auprès de NtpServer. NtpServer est un serveur de temps qui répond aux demandes de temps du client sur le réseau en retournant des échantillons de temps utiles pour la synchronisation de l’horloge locale. <ul><li>**1** - Oui</li><li>**0** - Non = 0 </li></ul>Valeur par défaut pour les membres du domaine et les clients autonomes : 1 |

## <a name="reference-pre-set-values-for-the-windows-time-service-gpo-settings"></a>Référence : Valeurs prédéfinies pour les paramètres GPO du service de temps Windows  

Le tableau suivant liste les paramètres de stratégie de groupe globaux associés au service de temps Windows et la valeur prédéfinie associée à chaque paramètre. Pour plus d’informations sur chaque paramètre, consultez les entrées de Registre correspondantes dans [Référence : Entrées de Registre du service de temps Windows](#reference-windows-time-service-registry-entries) plus haut dans cet article. Les paramètres suivants sont contenus dans un seul objet GPO appelé **Paramètres de configuration générale**.  

### <a name="pre-set-values-for-global-group-policy-settings"></a>Valeurs prédéfinies pour les paramètres de « Stratégie de groupe globale »

|Paramètre de stratégie de groupe|Valeur prédéfinie|  
| --- | --- |
|**AnnounceFlags**|**10**|  
|**EventLogFlags**|**2**|  
|**FrequencyCorrectRate**|**4**|  
|**HoldPeriod**|**5**|  
|**LargePhaseOffset**|**1 280 000**|  
|**LocalClockDispersion**|**10**|  
|**MaxAllowedPhaseOffset**|**300**|  
|**MaxNegPhaseCorrection**|**54 000** (15 heures)|  
|**MaxPollInterval**|**15**|  
|**MaxPosPhaseCorrection**|**54 000** (15 heures)|  
|**MinPollInterval**|**10**|  
|**PhaseCorrectRate**|**7**|  
|**PollAdjustFactor**|**5**|  
|**SpikeWatchPeriod**|**90**|  
|**UpdateInterval**|**100**|  

### <a name="pre-set-values-for-configure-windows-ntp-client-settings"></a>Valeurs prédéfinies pour les paramètres de « Configurer le client NTP Windows

Le tableau suivant liste les paramètres disponibles pour l’objet de stratégie de groupe **Configurer le client NTP Windows** et les valeurs prédéfinies associées au service de temps Windows. Pour plus d’informations sur chaque paramètre, consultez les entrées de Registre correspondantes dans [Référence : Entrées de Registre du service de temps Windows](#reference-windows-time-service-registry-entries) plus haut dans cet article.  

|Paramètre de stratégie de groupe|Valeur prédéfinie|  
|------------------------|-----------------|  
|**NtpServer**|time.windows.com, **0x1**|  
|**Type**|Options par défaut :<ul><li>**NTP.** Utilisez cette option sur des ordinateurs non joints à un domaine.</li><li>**NT5DS.** Utilisez cette option sur des ordinateurs joints à un domaine.</li></ul>|  
|**CrossSiteSyncFlags**|**2**|  
|**ResolvePeerBackoffMinutes**|**15**|  
|**ResolvePeerBackoffMaxTimes**|**7**|  
|**SpecialPollInterval**|**3 600**|  
|**EventLogFlags**|**0**|  

## <a name="reference-network-ports-that-the-windows-time-service-uses"></a>Référence : Ports réseau utilisés par le service de temps Windows

Le service de temps Windows suit la spécification NTP, qui exige l’utilisation du port UDP 123 pour toute communication de la synchronisation de l’heure. Ce port est réservé par le service de temps Windows et le reste en permanence. Chaque fois que l’ordinateur synchronise son horloge ou fournit l’heure à un autre ordinateur, cette communication est effectuée sur le port UDP 123.  

> [!NOTE]  
> Si vous disposez d’un ordinateur équipé de plusieurs cartes réseau (également appelé ordinateur *multirésident*), vous ne pouvez pas activer de manière sélective le service de temps Windows en fonction de la carte réseau.  

## <a name="related-information"></a>Informations connexes  

Les ressources suivantes contiennent des informations supplémentaires qui s’appliquent à cette section.  

- Document RFC *1305* dans la base de données RFC de l’IETF  
